# Apache Error Log Summary Report

**Server:** Apache/2.0.49 (Fedora)
**Source file:** `apache_error.log`
**Report generated:** 2026-07-16

---

## 1. Overview

| Metric | Value |
|---|---|
| **Total log entries** | 1,000 |
| **Date range covered** | **Thu 2005-06-09 06:07:04 → Thu 2005-06-16 01:06:31** (~7 days) |
| **Server start events** | 3 (Jun 09 06:07, Jun 10 11:32, Jun 12 04:04) |
| **Total `[error]` entries** | 753 (75.3%) |
| **Total `[notice]` entries** | 247 (24.7%) |

The log is dominated by error-level messages. `[notice]` entries are almost exclusively start-up / module initialization messages (LDAP, suEXEC, Digest auth, mod_python, mod_security, mod_jk/`jk2_init()`, `workerEnv.init()`), logged each time the server is started and as worker child processes are spawned.

### Log level breakdown

- **Error — 753 entries:** split between server/module problems on one hand and client-originated failures (404s, forbidden indexes, malformed requests, script not found) on the other.
- **Notice — 247 entries:** all routine — LDAP/SSL banners, suEXEC, Digest secret, mod_python mutex creation, mod_security load banner, `Apache/2.0.49 (Fedora) configured -- resuming normal operations`, and the high-volume `jk2_init() Found child … in scoreboard slot …` / `workerEnv.init() ok …` pair emitted each time a new JK child is initialized.

---

## 2. Server Configuration Issues

These are errors **not** tied to a specific `[client x.x.x.x]` — i.e. they originate from the server itself during startup or runtime worker management. They total **~129 error lines** and all point to **mod_jk / JK2 connector problems**.

### 2.1 JNI worker bean factory failures (during startup)
Emitted in two identical bursts (Jun 09 06:07:05 and Jun 10 11:32:27) right after LDAP/suEXEC/Digest init — i.e. at the very start of two of the three observed server runs:

| Error | Count |
|---|---|
| `env.createBean2(): Factory error creating channel.jni:jni …` / `config.update(): Can't create channel.jni:jni` | 6 |
| `env.createBean2(): Factory error creating vm: ( vm, )` / `config.update(): Can't create vm:` | 6 |
| `env.createBean2(): Factory error creating worker.jni:onStartup …` / `config.update(): Can't create worker.jni:onStartup` | 6 |
| `env.createBean2(): Factory error creating worker.jni:onShutdown …` / `config.update(): Can't create worker.jni:onShutdown` | 6 |

**Root cause:** The JK2 JNI channel (`channel.jni:jni`) and the JNI-based worker (`worker.jni`) cannot be instantiated — typically because a JVM can't be found/started (the `vm:` bean failure is the direct symptom) or the required native libraries/`JAVA_HOME`/`LD_LIBRARY_PATH` are not set in Apache's environment. The server then falls back to the **socket channel** worker (evidenced by the flood of successful `workerEnv.init() ok /etc/httpd/conf/workers2.properties` notices immediately afterward).

### 2.2 mod_jk child scoreboard errors (runtime)
These appear in bursts when child processes fork faster than the JK2 scoreboard can register them — heavily amplified during the AWStats scan on Jun 11 03:03 (see Section 4):

| Error | Count |
|---|---|
| `jk2_init() Can't find child <pid> in scoreboard` | 48 |
| `mod_jk child init 1 -2` (paired with each missing-child event) | 48 |
| `mod_jk child init 1 0` (one per server start, paired with first child) | 3 |

**Root cause:** A race / tuning issue in `mod_jk2` where the parent's scoreboard is not updated yet when the child initializes its JK2 channel. The `-2` return value is `JK2_ERR` (child not registered); service to Tomcat may be intermittently impaired for those children. It is **not** fatal to Apache but is a configuration/bug in the JK2 connector setup.

### 2.3 Non-errors worth noting
- `LDAP: SSL support unavailable` appears on every start — LDAP SSL is not compiled/linked, so any LDAP auth over TLS will fail.
- `mod_security/1.9dev2 configured` — a development build of mod_security is loaded; dev builds are not recommended for production.
- 3 server restarts in ~7 days is somewhat frequent and likely correlates with the JNI failures being "fixed" by restarts (they weren't) or with admin intervention after the attack bursts.

---

## 3. Client Error Summary

| Metric | Value |
|---|---|
| **Client-associated error entries** | 630 |
| **Unique client IPs** | 159 |

### 3.1 Client error categories

| Category | Count | % of client errors | Description |
|---|---:|---:|---|
| **File does not exist (HTTP 404)** | 295 | 46.8% | `File does not exist: /var/www/html/...` — missing static files, largely attack probes (see §4) |
| **Directory index forbidden (HTTP 403)** | 224 | 35.6% | `Directory index forbidden by rule: /var/www/html/` — clients requesting `/` when no `DirectoryIndex` exists and auto-indexing is denied |
| **Script not found / unable to stat** | 89 | 14.1% | `script not found or unable to stat: /var/www/cgi-bin/...` — almost exclusively AWStats CGI probes |
| **Invalid method in request** | 19 | 3.0% | Malformed requests using lowercase `get` (not `GET`) — nearly all are IIS-traversal exploit attempts |
| **URI too long (> 8190 bytes)** | 3 | 0.5% | `request failed: URI too long (longer than 8190)` from 210.91.137.35, 211.211.14.224, 150.161.187.25 — classic buffer-overrun/smuggling probes |

### 3.2 Top client IPs by error count

| Rank | Client IP | Errors | Nature |
|---:|---|---:|---|
| 1 | **202.133.98.6** | 184 | Aggressive AWStats vulnerability scanner (see §4) |
| 2 | 81.199.21.119 | 23 | "sumthin" worm/scanner hitting `/sumthin` |
| 2 | 194.116.250.2 | 23 | "sumthin" worm/scanner hitting `/sumthin` |
| 2 | 81.214.165.213 | 23 | FrontPage/IIS extension probe (`/_vti_bin`, `/_mem_bin`, `/MSADC`, `/scripts/root.exe`, `default.ida`, `null.ida`, `/c/winnt/...`) |
| 5 | 195.23.79.241 | 22 | FrontPage/IIS extension probe |
| 6 | 212.238.198.203 | 20 | FrontPage/IIS extension probe |
| 7 | 60.191.134.226 | 19 | FrontPage/IIS extension probe |
| 8 | 219.133.247.159 | 18 | FrontPage/IIS extension probe |
| 9 | 219.133.246.207 | 15 | FrontPage/IIS extension probe |
| 10 | 61.142.136.20 | 13 | IIS traversal / Code Red–style probe |
| 10 | 203.112.195.156 | 13 | FrontPage/IIS probe |
| 10 | 24.163.159.161 | 13 | FrontPage/IIS probe |
| 10 | 68.251.32.120 | 13 | FrontPage/IIS probe |
| 10 | 218.82.188.130 | 13 | FrontPage/IIS probe |
| 10 | 61.109.45.7 | 13 | FrontPage/IIS probe |
| 10 | 85.226.115.57 | 13 | FrontPage/IIS probe |

The tail (IPs after the top list) is mostly single-request hits from ordinary web scanners / misbehaving crawlers receiving 403 on `/`.

---

## 4. Security Assessment

The log shows **heavy, continuous automated scanning and exploit probing** — the vast majority of the 630 client errors are malicious traffic rather than legitimate user errors. In total at least **251 attack-related entries from 35 unique source IPs** were identified (the real number is higher, since many directory-index hits against `/` are also from scanners but cannot be unambiguously classified from the error log alone).

### 4.1 Attack categories and evidence

#### (a) AWStats vulnerability scanner — single worst offender
- **Source:** `202.133.98.6` — **184 errors in ~7 seconds (03:03:03–03:03:11 on Sat Jun 11)** — a clear automated script.
- **Pattern:** Iterates common AWStats paths:
  - `/cgi-bin/awstats`, `/cgi-bin/awstats.pl`, `/cgi-bin/stats`
  - `/awstats/awstats.pl`, `/stats/awstats.pl`
  - `/cgi`
- **Exploit targeted:** The well-known AWStats `configdir` remote command-execution vulnerability (CVE-2005-0116 / CVE-2005-0435-era), which allows arbitrary Perl execution via a crafted `configdir` parameter.
- **Side effect:** The rapid-fire requests caused Apache to spawn ~65+ JK2 children in seconds, producing 40+ `jk2_init() Can't find child … in scoreboard` / `mod_jk child init 1 -2` errors (this is the source of nearly all the "server configuration" errors in §2.2).
- **Result:** All probes returned 404/script-not-found; the server is not running vulnerable AWStats.

#### (b) Code Red / Nimda style `root.exe` & IIS Unicode directory traversal
Multiple sources (18+ unique IPs, distributed globally) send classic IIS-exploit payloads that are meaningless against Apache on Linux:
- `get /scripts/.%252e/.%252e/winnt/system32/cmd.exe?/c+dir` (double-encoded dot-dot-slash)
- `get /scripts/..%35c../winnt/system32/cmd.exe?/c+dir`
- `get /scripts/..%e0%80%af../..%e0%80%af../..%e0%80%af../winnt/system32/cmd.exe?/c+dir` (long UTF-8 overlong encoding bypass)
- `get /scripts/..%c0%af../..%c0%af../..%c0%af../winnt/system32/cmd.exe?/c+dir`
- `get /scripts/root.exe?/c+dir`
- `File does not exist: /var/www/html/scripts/root.exe`
- `File does not exist: /var/www/html/scripts/..\xc1\x1c..`, `..\xc0\xaf..`, `..\xc1\x9c..` (illegal multibyte slash evasion)

Sources include `63.203.254.140`, `213.61.135.6`, `201.252.246.11`, `62.221.237.83`, `213.205.73.192`, `64.147.69.59`, `207.181.126.3`, `12.216.230.125`, `202.118.167.71`, `210.22.201.117–125`, `61.142.136.20`, `66.201.171.199`, etc.

These are automated **Code Red II / Nimda / IIS Unicode worm** remnants still scanning in 2005, plus copy-cat script-kiddie tools. They all fail because the target paths do not exist on a Fedora/Apache host.

#### (c) FrontPage / IIS extension probes
Bursts of requests from a block of distinct IPs (`81.214.165.213`, `195.23.79.241`, `212.238.198.203`, `60.191.134.226`, `219.133.247.159`, `219.133.246.207`, `203.112.195.156`, `24.163.159.161`, `68.251.32.120`, `218.82.188.130`, `61.109.45.7`, `85.226.115.57`, `218.87.132.28`, `218.18.40.53`, …) — each hits ~10–23 classic IIS-only paths per IP:
- `/_vti_bin` (FrontPage Server Extensions)
- `/_mem_bin` (IIS sample bin)
- `/MSADC`, `/msadc` (IIS MDAC RDS exploit path)
- `/scripts/..%5c..`, `/scripts/..%5c%5c..`, `/scripts/..%2f..` (backslash/encoding traversal)
- `/scripts/root.exe`, `/c`, `/d`
- `/default.ida`, `/null.ida` (.ida buffer overflow, e.g. Code Red)
- `/c/winnt/system32/cmd.exe`

This is a standardized "IIS vulnerability scan list" being blindly sprayed across IP space; it is ineffective against Linux/Apache.

#### (d) "sumthin" worm / scanner
- Sources: `81.199.21.119` (23 hits), `194.116.250.2` (23 hits), plus other IPs.
- Payload: Repeated `GET /sumthin` (23 identical 404s per IP in < 2 seconds).
- This is the signature of the **"Sumthin" (Anticrash/Delude) family** of IRC-controlled mass-scanning worms active in 2004–2005.

#### (e) Oversized-URI buffer-overflow probes
Three distinct sources (`210.91.137.35`, `211.211.14.224`, `150.161.187.25`) triggered `request failed: URI too long (longer than 8190)`. Sending URIs longer than the server's `DEFAULT_LIMIT_REQUEST_LINE` is a common fuzz/buffer-overflow/smuggling probe. Apache safely rejected all three.

#### (f) Method-violation probes
19 requests arrived with lowercase `get …` instead of the HTTP-standard uppercase `GET …`. All 19 are the traversal/Cmd.exe payloads described in (b). This is why they appear as `Invalid method in request` rather than 404 — Apache rejects them at the request-parsing stage before routing.

### 4.2 Security posture summary
- **No successful compromise is visible** in this log: every exploit probe was rejected (404/403/invalid method/URI too long). This is consistent with the server being Apache/Linux, not IIS/Windows.
- **The site has no `DirectoryIndex` document at `/var/www/html/`**: every request for `/` returns 403 "Directory index forbidden by rule". This tells every automated scanner that the web root exists but has no index page — effectively advertising the host.
- **mod_security 1.9dev2 is loaded** but clearly is not (yet) configured to block/drop these probes, since every attack is reaching Apache's default handler and generating 404/403 log entries.
- **Source IP diversity is very high** (~159 unique IPs in 7 days, with at least 35 confirmed-malicious). This is background Internet noise; blocking individual IPs will not help.

---

## 5. Recommendations

### Recommendation 1 — Fix the mod_jk2 JNI configuration (or move to mod_jk / mod_proxy_ajp)
The recurring `Factory error creating channel.jni:jni` / `Can't create vm:` errors on every startup indicate that the JNI connector is misconfigured (missing JVM / missing `libjkjni.so` / wrong `JkWorker` setting in `workers2.properties`). Two alternatives:
- **If you do not use in-process JNI/Tomcat JNI workers** (typical case — most setups use the APR/socket channel): disable the JNI worker/channel in `workers2.properties` — remove/comment the `[channel.jni:jni]`, `[vm:]`, `[worker.jni:*]` beans and the `channel.jni:jni` channel reference. The socket channel is already working (every child gets `workerEnv.init() ok`).
- **If you do need JNI:** set `JAVA_HOME`, `LD_LIBRARY_PATH`, and `LD_PRELOAD` (for `libjsig.so`) in Apache's environment (e.g. `envvars` or the `httpd` init script), and point `workers2.properties` at the correct `libjvm.so`.
- Strongly consider **replacing mod_jk2 entirely** — mod_jk2 was abandoned upstream in 2004/2005 and is unsupported; either `mod_jk` 1.2.x or `mod_proxy_ajp` (bundled with httpd 2.2+) is the supported replacement. This will also likely eliminate the race-condition `jk2_init() Can't find child … in scoreboard` / `mod_jk child init 1 -2` errors seen during traffic spikes.

### Recommendation 2 — Add a landing page and tighten directory-index handling, to reduce noise and exposure
- **Add a `DirectoryIndex`** (e.g. `index.html`) at `/var/www/html/`. The 224 `Directory index forbidden by rule: /var/www/html/` entries are primarily scanners hitting `/` to detect a web host. A static welcome/blank page turns those 403s into normal 200s *and* stops advertising the empty/autoindex-disabled webroot.
- If the site is not intended to serve `/var/www/html` at all, point `DocumentRoot` at the real application root or return a 404/400 for the default vhost instead of leaving an empty directory.

### Recommendation 3 — Configure mod_security (and OS/firewall rules) to drop the automated attack traffic
A working mod_security 1.9dev2 is already loaded but apparently not configured with rules. Tighten this to both reduce log noise and harden the host:
- Enable a core rule set (or even just a small local ruleset) to reject the obvious exploit strings seen in this log: patterns containing `cmd.exe`, `root.exe`, `..%`, `..\x`, `/scripts/`, `/MSADC`, `/_vti_bin`, `/_mem_bin`, `/default.ida`, `/null.ida`, `/awstats.pl` with traversal chars, plus any method token that is not all-uppercase. These requests are **never** legitimate against a Fedora/Apache host and should be **denied with 400/403 at the mod_security layer** rather than passing through to the default handler and generating 404 log lines.
- Turn on `SecAuditLog` to capture blocked requests (separate from the error log) so the error log stays useful for real problems.
- Upgrade mod_security from `1.9dev2` (a development build) to the latest stable 1.9.x or 2.x release.
- As defense-in-depth, consider iptables/`fail2ban` rate-limiting for IPs that trigger > N 4xx errors per minute — this would have throttled `202.133.98.6` (184 requests in 7 seconds) automatically and prevented the JK2 scoreboard/child-spawn storm it triggered.

---

## Appendix — Quick Numbers at a Glance

- 1,000 log lines, 7 days, 3 server restarts
- 753 errors / 247 notices
- 129 server-side errors (all mod_jk2 JNI + scoreboard)
- 630 client errors from 159 unique IPs
- 251 confirmed-malicious requests from 35 unique attacker IPs
- 1 single scanner (`202.133.98.6`) produced **184** of those in 7 seconds
- **0** successful attacks visible in the log
