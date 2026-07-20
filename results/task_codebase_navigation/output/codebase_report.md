# Express.js Codebase Report — Routing, Req/Res Extension, Middleware, and Authentication

This report is based on the **express 5.2.1** source published to npm (which mirrors
`expressjs/express` `master`). In Express v5, the router has been extracted into its
own package, `router@^2.2.0`, which is used as a dependency (not vendored in
`lib/`). File paths below are relative to the repository root; files inside
`node_modules/router/` correspond to the `pillarjs/router` (a.k.a. `expressjs/router`)
package that ships with Express and contains the core routing machinery.

---

## 1. Where is routing handled?

Route matching and dispatching is split across **three layers**:

| Layer | File (relative to repo root) | Responsibility |
|-------|-------------------------------|----------------|
| App-to-router delegation | `lib/application.js` | Wires HTTP server → `app.handle()` → `this.router.handle()`; defines `app.use`, `app.METHOD`, `app.all`, `app.route`, `app.param` as thin proxies to the router. |
| Router core (middleware stack + layer matching) | `node_modules/router/index.js` | Holds the ordered `stack` of `Layer`s; walks the stack on each request; performs prefix matching, URL trimming for mounted sub-routers, param processing, method filtering, and invokes each layer. |
| Per-route dispatch (per-HTTP-method handlers) | `node_modules/router/lib/route.js` | A per-path stack of layers bound to HTTP verbs; dispatches to the verb-specific handler stack for a matched path. |
| Path matching + single handler invocation | `node_modules/router/lib/layer.js` | Encapsulates one handler with its path pattern (compiled via `path-to-regexp`), parameter extraction, `handleRequest` / `handleError` invocation, and promise/error handling. |

### Key code locations

- **`lib/application.js`**
  - Lines 69–82 — Lazily constructs the top-level `Router`:
    ```js
    Object.defineProperty(this, 'router', {
      get: function getrouter() {
        if (router === null) {
          router = new Router({
            caseSensitive: this.enabled('case sensitive routing'),
            strict: this.enabled('strict routing')
          });
        }
        return router;
      }
    });
    ```
  - Lines 152–178 (`app.handle`) — Entry point from Node's HTTP server:
    ```js
    app.handle = function handle(req, res, callback) {
      var done = callback || finalhandler(req, res, { env: this.get('env'), onerror: logerror.bind(this) });
      ...
      this.router.handle(req, res, done);
    };
    ```
  - Lines 190–244 (`app.use`) — delegates to `router.use(path, fn)`, wrapping mounted sub-apps.
  - Lines 471–482 (verb proxy loop) — `app.get/post/put/...` create a `Route` via `this.route(path)` and forward handlers.
  - Lines 494–503 (`app.all`) — registers a handler on every HTTP method.
  - Lines 598–606 (`app.listen`) — `http.createServer(this)` makes the `app` function itself the HTTP callback, which calls `app.handle`.

- **`node_modules/router/index.js`**
  - Lines 52–73 (`Router` constructor) — creates the `router` callable and initializes `router.stack = []`.
  - Lines 149–345 (`Router.prototype.handle`) — the **main dispatch loop**:
    - Lines 156–186 — sets up `idx`, `parentParams`, `parentUrl`, saves `req.next = next`, seeds `req.baseUrl`/`req.originalUrl`, calls `next()`.
    - Lines 188–302 (inner `next(err)`) — the core algorithm:
      - Lines 235–272 — iterates `stack[idx++]`, calls `matchLayer(layer, path)` (→ `layer.match(path)` in `layer.js`), skips non-matching layers, checks HTTP method via `route._handlesMethod(method)`, and handles error propagation (routes do not match when there is a pending error).
      - Lines 285–287 — merges path params into `req.params`.
      - Lines 291–301 — runs `router.param` callbacks via `processParams(...)`, then either dispatches to a route (`layer.handleRequest` → `route.dispatch`) or trims the URL prefix (for `.use` middleware / mounted routers) via `trimPrefix(...)` and invokes the layer.
    - Lines 304–344 (`trimPrefix`) — strips the matched prefix from `req.url`, updates `req.baseUrl`, then calls `layer.handleRequest` or `layer.handleError`.
  - Lines 362–410 (`Router.prototype.use`) — wraps middleware in a `Layer(path, { strict:false, end:false }, fn)` with `layer.route = undefined` and pushes it onto `this.stack`.
  - Lines 425–442 (`Router.prototype.route`) — creates a `Route` and a wrapping `Layer(path, { end:true }, handle)` where `handle` calls `route.dispatch(req,res,next)`; pushed onto the router stack.
  - Lines 445–451 — adds `router.get/post/.../all` shortcuts that call `this.route(path)[method](handlers...)`.

- **`node_modules/router/lib/layer.js`**
  - Lines 34–94 (constructor) — compiles the path matcher via `path-to-regexp` (or a RegExp matcher).
  - Lines 142–167 (`Layer.prototype.handleRequest`) — invokes the handler `fn(req, res, next)` (skipped if `fn.length > 3`, which marks an error handler); catches synchronous throws and awaited-promise rejections and forwards them to `next(err)`.
  - Lines 106–131 (`Layer.prototype.handleError`) — only invokes `fn(err, req, res, next)` if the handler has arity 4 (the Express error-middleware signature); otherwise re-forwards the error.
  - Lines 178–209 (`Layer.prototype.match`) — runs the matcher and stores extracted `params` on the layer.

- **`node_modules/router/lib/route.js`**
  - Lines 41–48 (constructor) — per-path container holding its own `stack` of verb layers and a `methods` map.
  - Lines 98–162 (`Route.prototype.dispatch`) — a second, inner `next` loop that walks the route's own stack, filters by HTTP method (`layer.method === method`), and calls `layer.handleRequest` / `layer.handleError`. This is where per-route middleware (e.g. `route.get(auth, handler)`) runs.
  - Lines 192–214 (`Route.prototype.all`) and 216–242 (per-verb registration) — push `Layer('/', {}, fn)` items onto the route stack tagged with `layer.method = method` (or `undefined` for `.all`).

---

## 2. Where are request and response objects extended?

Express uses **prototype injection**: at startup it creates two prototype objects
that inherit from Node's built-ins, and on every incoming request it swaps the
`req`/`res` prototype chain so that Express-specific methods become available.

### Extension points

- **`lib/express.js`** lines 36–56 (`createApplication`) — builds the two prototypes once per app:
  ```js
  app.request = Object.create(req, { app: { value: app } });
  app.response = Object.create(res, { app: { value: app } });
  ```
- **`lib/application.js`** lines 168–170 (`app.handle`) — performs the prototype swap **on every request**:
  ```js
  Object.setPrototypeOf(req, this.request)
  Object.setPrototypeOf(res, this.response)
  ```
  This is why middleware can call `res.send(...)` even though the incoming object started life as a plain `http.ServerResponse`.
- **`lib/application.js`** lines 117–121 (`mount` event) — when a sub-app is mounted, its `request`/`response` prototypes are chained to the parent's via `Object.setPrototypeOf(...)`, so middleware added on the parent app is visible in sub-apps.

### Files that add Express-specific methods

| File | Lines | Examples of added methods / getters |
|------|-------|--------------------------------------|
| `lib/request.js` | 30–514 | `req` is created as `Object.create(http.IncomingMessage.prototype)` (line 30). Methods/getters added include: |
| | 63–83 | `req.get` / `req.header` — header access with Referer/Referrer aliasing |
| | 131–199 | `req.accepts`, `req.acceptsEncodings`, `req.acceptsCharsets`, `req.acceptsLanguages` |
| | 201–205 | `req.range` |
| | 217–228 | **`req.query`** (lazy getter) — parses querystring using the app's `query parser fn` |
| | 256–268 | `req.is` — Content-Type matching |
| | 284–302 | `req.protocol` (trust-proxy aware) |
| | 313–315 | `req.secure` |
| | 328–445 | `req.ip`, `req.ips`, `req.subdomains`, `req.hostname`, `req.host`, `req.port`, `req.path` |
| | 456–498 | `req.fresh`, `req.stale`, `req.xhr` |
| | 508–514 | helper `defineGetter` used throughout to create lazy properties |
| `lib/response.js` | 42–1053 (end of file) | `res` is created as `Object.create(http.ServerResponse.prototype)` (line 42). Methods added include: |
| | 64–76 | `res.status(code)` |
| | 97–110 | `res.links` |
| | 125–225 | **`res.send(body)`** — handles strings, Buffers, objects (falls through to `res.json`), sets Content-Type/Length/ETag, freshness, HEAD handling |
| | 239–253 | **`res.json(obj)`** — JSON-serializes via `stringify(...)` and calls `this.send(body)` |
| | 267–320 | `res.jsonp` |
| | 328–355 | `res.sendStatus` |
| | 378–509 | `res.sendFile`, `res.download` |
| | 510–610 | `res.type`/`res.contentType`, `res.format`, `res.attachment` |
| | 636–702 | `res.append`, `res.set`/`res.header` |
| | 703–715 | `res.get` |
| | 716–800 | `res.clearCookie`, **`res.cookie`** (uses the `cookie` package to serialize Set-Cookie headers) |
| | 801–879 | `res.location`, **`res.redirect`** |
| | 881–898 | `res.vary` |
| | 900–… | `res.render` — delegates to `app.render(...)` and sends the result |

### Properties added elsewhere (not in `lib/request.js` itself)

These are set by the router during dispatch, not via the prototype:

- **`req.params`** — populated by `Layer.prototype.match` and assigned in `Router.prototype.handle` at `node_modules/router/index.js` lines 285–287:
  ```js
  req.params = self.mergeParams
    ? mergeParams(layer.params, parentParams)
    : layer.params
  ```
- **`req.route`** — set at `node_modules/router/index.js` lines 280–282 and again in `route.js` line 115 when a matching route is found.
- **`req.baseUrl` / `req.originalUrl` / `req.next`** — set in `Router.prototype.handle` at lines 171–184.
- **`req.res` / `res.req`** (circular references) and **`res.locals`** — set in `app.handle` at `lib/application.js` lines 165–175.
- **`req.body`** — is **not** populated by Express core; it is added by the `body-parser` middleware (which is re-exported at `lib/express.js` lines 77–81 as `express.json`, `express.raw`, `express.text`, `express.urlencoded`).

---

## 3. How does middleware execution work?

Middleware execution in Express is a **recursive, promise-aware, depth-first walk
of a stack of `Layer` objects**. Each call to `next()` advances to the next
matching layer; calling `next(err)` short-circuits into the error-handling
sub-path.

### End-to-end code path for an incoming request

1. **HTTP server callback** — `lib/application.js` lines 598–606:
   ```js
   app.listen = function listen() {
     var server = http.createServer(this)
     ...
     return server.listen.apply(server, args)
   }
   ```
   The `app` function itself is defined in `lib/express.js` lines 37–39:
   ```js
   var app = function(req, res, next) { app.handle(req, res, next); };
   ```
   So Node delivers every request to `app.handle(req, res)`.

2. **`app.handle` (lib/application.js 152–178)**
   - Wires up `finalhandler` as the terminal `done` callback (produces 404/500 responses if nothing handles the request).
   - Sets `X-Powered-By`, `req.res = res`, `res.req = req`, `res.locals = {}`.
   - Swaps prototypes so `req`/`res` pick up Express methods (`Object.setPrototypeOf(...)`).
   - Delegates to the lazy router: `this.router.handle(req, res, done)`.

3. **`Router.prototype.handle` (node_modules/router/index.js 149–345)**
   - Initializes iterator state (`idx = 0`, `parentParams`, `parentUrl`, `paramcalled`).
   - Saves the `next` function onto `req.next = next` (line 174) so handlers can invoke it.
   - Seeds `req.baseUrl` and `req.originalUrl` (lines 183–184).
   - Invokes the local `next()` to start iteration (line 186).

4. **The inner `next(err)` loop (router/index.js 188–302)**
   - Restores any URL prefixes/bases modified by previously matched layers (lines 193–204).
   - Handles the special `'route'` and `'router'` sentinel errors (lines 189–210):
     - `next('route')` → skips the rest of the current route (but continues the router).
     - `next('router')` → exits the entire router via `done(null)`.
   - If `idx >= stack.length`, calls `done(layerError)` → falls through to `finalhandler` (404 if no error, error response otherwise).
   - **Yields to the event loop every 100 synchronous layers** (lines 219–221, `setImmediate(next, err)`) to prevent stack overflow.
   - Uses `parseurl(req).pathname` to get the path (lines 223–228, `getPathname`).
   - Walks the stack (lines 235–272):
     ```js
     while (match !== true && idx < stack.length) {
       layer = stack[idx++]
       match = matchLayer(layer, path)      // layer.match(path)
       route = layer.route
       ...
       if (route) {
         const hasMethod = route._handlesMethod(method)
         if (!hasMethod && method !== 'HEAD') match = false
       }
     }
     ```
     - Layers without a `route` are **plain middleware** added via `app.use`/`router.use` — they match on path prefix and run regardless of HTTP method.
     - Layers with a `route` are **endpoint routes**; they also require an HTTP-method match, and are skipped if there is a pending error (so errors bypass normal routes and only hit error middleware).
   - Assigns `req.params` from the matched layer (lines 285–287).
   - Runs any `router.param(name, fn)` callbacks via `processParams` (lines 291–301, implemented at lines 576–665). Each param callback is invoked as `fn(req, res, paramCallback, value, key)` and supports promises.
   - If the matched layer has a route, calls `layer.handleRequest(req, res, next)` which ultimately triggers `Route.prototype.dispatch` (route.js 98–162) — an inner `next` loop over that route's verb-specific handler stack.
   - Otherwise (plain middleware) it calls `trimPrefix(...)` (lines 304–344), which strips the matched prefix from `req.url`, updates `req.baseUrl`, and then calls `layer.handleRequest` (or `layer.handleError` if there is an error).

5. **`Layer.prototype.handleRequest` (node_modules/router/lib/layer.js 142–167)**
   - Skips handlers whose arity `> 3` (they are error handlers — `fn(err, req, res, next)`).
   - Invokes `fn(req, res, next)` inside try/catch; if the return value is a promise, attaches a rejection handler that calls `next(err)`. Synchronous throws are also caught and forwarded to `next(err)`.
   - When the middleware is done, the user calls `next()` / `next(err)`, which returns control to step 4's `next` closure, incrementing `idx` and moving on.

6. **Error middleware — `Layer.prototype.handleError` (layer.js 106–131)**
   - Only invokes handlers whose `fn.length === 4`.
   - Same promise/throw handling as `handleRequest`, but passes the error as the first argument.
   - This is why `function (err, req, res, next) { ... }` signatures are treated as error handlers — discovered purely by arity.

7. **Per-route dispatch — `Route.prototype.dispatch` (route.js 98–162)**
   - A second `next` loop that filters the route's inner stack by HTTP method (`layer.method === method`, or `undefined` for `.all` handlers).
   - Calls `layer.handleError` or `layer.handleRequest` per matched layer, so per-route middleware (e.g. `router.get('/x', auth, handler)`) runs in order before the final verb handler.

8. **Terminal `done` — `finalhandler`**
   - If no layer matches and there's no error → 404.
   - If `next(err)` propagates all the way out → default error response (respecting `err.status`/`err.statusCode`), with stack traces only in development.

### Key properties that enable the middleware model

- **Registration (`app.use` / `router.use`, application.js 190–244 and router/index.js 362–410)** — wraps each function in a `Layer` and appends it to `stack` in registration order. Order is therefore the only thing that determines execution order.
- **Path prefix matching with URL rewriting** — when a middleware is mounted at `/api`, `trimPrefix` strips `/api` from `req.url` and appends it to `req.baseUrl` before the middleware runs, so the mounted handler sees `/users` instead of `/api/users`. The prefix is restored when control returns via `next()`.
- **Sentinel "errors"** — `'route'` (skip rest of current route) and `'router'` (exit current router) are implemented as special error values in `next()` rather than separate APIs, which is why `next('route')` looks like an error call.
- **Promise support** — any middleware can return a Promise (router v2, i.e. Express v5); rejections are routed to `next(err)` automatically.

---

## 4. Where would authentication middleware (e.g. Passport) hook in?

Because Express's dispatch is purely a stack of ordered layers, authentication
middleware integrates **exactly like any other middleware** — by being added to
the router's `stack` with `app.use(...)` (globally), `router.use(...)` (per
sub-router), or as a per-route argument. Passport's own strategies ultimately
read/modify `req`, call `next()` on success, or either respond directly (e.g.
401) or call `next(err)` on failure.

### Integration points visible in the codebase

1. **Global auth via `app.use(passport.authenticate(...))`**
   - This hits `lib/application.js` lines 190–244 (`app.use`), which forwards to `router.use(path, fn)` at `node_modules/router/index.js` lines 362–410.
   - There a `Layer` is created with `end: false` (prefix match), `layer.route = undefined`, and pushed onto `router.stack`.
   - On every request whose URL starts with the prefix (default `/`), the layer matches in the main loop (router/index.js 235–252) and is invoked by `layer.handleRequest` (layer.js 142–167).
   - If the auth middleware calls `next()`, iteration continues to later routes/middleware; if it calls `next(err)` (or throws, or returns a rejected promise), Express jumps to error middleware (layers with 4-argument handlers) via `Layer.prototype.handleError`.
   - Typical pattern:
     ```js
     app.use(passport.initialize());
     app.use(passport.session());           // reads req.session, sets req.user
     app.use(ensureAuthenticated);           // custom 3-arg middleware, calls next() or res.status(401).end()
     ```

2. **Per-route / per-group auth via `router.use` or middleware arguments**
   - Sub-routers are just nested Routers. When you do `app.use('/api', apiRouter)`, `app.use` (application.js 225–241) wraps the sub-app in a `mounted_app` shim that calls `fn.handle(req, res, ...)` and restores prototypes. Inside the sub-router, the same stack walk happens, with `req.baseUrl` updated by `trimPrefix` (router/index.js 304–344). So:
     ```js
     const api = express.Router();
     api.use(passport.authenticate('jwt', { session: false }));
     api.get('/profile', (req, res) => res.json(req.user));
     app.use('/api', api);
     ```
     the JWT auth layer sits at the top of `api.stack` and gates every route under `/api/*`.

3. **Inline auth on individual routes**
   - `app.get('/admin', passport.authenticate('basic', { session: false }), handler)` goes through `app.METHOD` (application.js 471–482) → `router.route(path)[method](...handlers)` → `Route.prototype.get` (route.js 216–242), which pushes **every** handler (including the Passport callback) as a separate `Layer` onto the route's inner stack, in declaration order.
   - `Route.prototype.dispatch` (route.js 119–161) walks these in order, so the Passport layer runs first and can either call `next()` to reach the final handler, short-circuit with `next('router')`/`next('route')`, send a response (`res.status(401)...`), or pass an error.
   - This works because `Route.prototype.all` (route.js 192–214) pushes non-method-specific layers onto the same stack — that is the idiom used for per-route guards:
     ```js
     router.route('/posts/:id')
       .all(loadUser)      // runs for GET/POST/PUT/DELETE on this path
       .get(showHandler)
       .put(updateHandler);
     ```

4. **Writing auth results onto `req` (e.g. `req.user`, `req.isAuthenticated`)**
   - The prototype swap in `lib/application.js` lines 168–170 means any property a middleware assigns to `req` is visible to all later middleware and to route handlers, and is also readable via `req.res` from the response side.
   - This is exactly how Passport works: `passport.initialize()` adds `req._passport`, `req.login`, `req.logIn`, `req.logout`, `req.logOut`, `req.isAuthenticated`, `req.isUnauthenticated` onto `req`, and strategy middleware sets `req.user` on success and then calls `next()`. No modification to Express core is required — the extensibility point is simply **being a normal (req, res, next) middleware**.

5. **Auth failure flows use standard middleware semantics**
   - If authentication fails, Passport typically either calls `res.status(401).send(...)` and does **not** call `next`, or calls `next(err)` (e.g. when custom callbacks are used). In the latter case the error propagates out via the `catch (err) { next(err) }` in `Layer.prototype.handleRequest` (layer.js 164–165) and promise-rejection handler (lines 155–162), then gets routed to 4-arg error middleware or to `finalhandler`. This is why apps commonly add something like:
     ```js
     app.use((err, req, res, next) => {
       res.status(err.status || 500).json({ error: err.message });
     });
     ```
     — such a function is detected by `Layer.prototype.handleError` (layer.js 109) because `fn.length === 4`.

### Why Passport doesn't need any "special hook"

There is no dedicated "auth" hook in Express. The contract that Passport and
other auth libraries rely on is:

- Middleware signature: `function (req, res, next)` (or `(err, req, res, next)` for errors).
- Ordering: placed via `app.use` / `router.use` / per-route args **before** the handlers that require authentication.
- Side effects on `req`/`res` (e.g. `req.user`, `res.locals.user`) are visible downstream because the same `req`/`res` objects are threaded through every layer.
- Access to `req.params`/`req.query`/`req.body` works because:
  - `req.params` is populated just before the layer is invoked (router/index.js 285–287).
  - `req.query` is a lazy getter on the `req` prototype (lib/request.js 217–228).
  - `req.body` is populated earlier in the stack by `express.json()`/`express.urlencoded()` (lib/express.js 77–81), so auth middleware must be registered **after** body-parsing middleware if it needs to read credentials from the body (common for local/username-password strategies).

In short: authentication middleware hooks into Express by being an ordinary
entry in the ordered `Layer` stack maintained by `Router` (node_modules/router/index.js),
invoked by `Layer.prototype.handleRequest` (node_modules/router/lib/layer.js),
and empowered by the prototype-extended `req`/`res` objects created in
`lib/express.js` and injected in `lib/application.js`.
