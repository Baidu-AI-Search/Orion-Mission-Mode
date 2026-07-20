# Open Source Self-Hosted Alternatives to Notion
### A Research Report for Startup Teams — Internal Knowledge Base & Project Documentation

**Date:** July 16, 2026
**Audience:** Early-to-mid stage startup teams seeking data sovereignty via self-hosting
**Scope:** Open source projects that can credibly replace Notion for team knowledge bases, project docs, and collaborative workspaces

---

## Executive Summary

Notion remains the gold standard for all-in-one collaborative workspaces, but its closed-source SaaS model conflicts with a startup's need for data sovereignty, predictable cost, and deep customization. This report evaluates **six leading open source alternatives** that can be self-hosted, comparing them across licensing, tech stack, deployment, Notion-like feature parity, community health, limitations, and operational complexity.

**Key takeaways for a startup team:**

- **Best overall Notion replacement for teams:** **Outline** — closest Notion-like UX, mature real-time collaboration, strong team permissions, fastest release cadence. The BSL 1.1 license means you cannot sell it as a hosted service, which is fine for internal use.
- **Best for Notion + Miro / whiteboard-heavy teams:** **AFFiNE** — uniquely merges block docs with an infinite canvas, local-first CRDTs, MIT-licensed, very active development. Self-host story is improving rapidly but still has rough edges for large teams.
- **Best for strict OSI-approved license + cross-platform native apps:** **AppFlowy** — AGPL-3.0, Flutter+Rust, mature desktop/mobile clients, AI built-in. Self-hosted cloud is functional but the ecosystem is still catching up to Notion's depth.
- **Best for engineering-heavy / wiki-style documentation:** **BookStack** — MIT, PHP/Laravel, rock-solid, extremely easy to self-host, opinionated book/chapter/page structure. Lacks Notion's flexible block/database model.
- **Best for personal knowledge graphs + Markdown purists (secondary):** **Logseq** — AGPL-3.0, ClojureScript, outliner + bi-directional links. Excellent for individual "second brain" use, weaker as a centralized team wiki.
- **Best for hacker/engineer personal workspaces:** **SilverBullet** — MIT, Deno/TypeScript, markdown-as-database with live queries. Single-binary easy, but niche UX and smaller team-collab story.

For a startup prioritizing **team knowledge base + project documentation + self-host**, the recommended order is: **Outline → AFFiNE → AppFlowy → BookStack → Logseq → SilverBullet**.

---

## Summary Comparison Table

| Project | License | Primary Language / Stack | Stars (Jul '26) | Contributors | Latest Release | Blocks Editor | Databases / Tables | Real-time Collab | API | Self-host Ease |
|---|---|---|---|---|---|---|---|---|---|---|
| **Outline** | BSL 1.1 | TypeScript / React / Node.js + Postgres + Redis | ~39.7k | ~251 | v1.9.1 (Jul 2026) | ✅ (rich block editor) | ✅ (collections + tables) | ✅ (mature, Yjs/CRDT) | ✅ REST | 🟢 Easy (Docker Compose) |
| **AFFiNE** | MIT (core, client); some server parts AGPL | TypeScript / React + Rust (OctoBase) + BlockSuite | ~70.5k | ~256 | v0.27.0 (Jul 2026) | ✅ (BlockSuite) | ✅ (databases/collections) | ✅ (CRDT) | ✅ (growing) | 🟡 Medium (Docker, PG + Redis) |
| **AppFlowy** | AGPL-3.0 | Flutter + Rust (core), Dart (UI), React (new web) | ~73.9k | ~381 | 0.12.5 (Jun 2026) | ✅ | ✅ (multi-view DB) | ✅ (AppFlowy Cloud) | ✅ | 🟡 Medium (multi-service Docker) |
| **BookStack** | MIT | PHP / Laravel / MySQL or Postgres | ~18.9k | — (moved to Codeberg) | Active (Jul 2026) | 🟡 WYSIWYG + Markdown, not "block" | ❌ (hierarchical, no DB views) | ✅ (multi-user edit, not fully CRDT) | ✅ REST | 🟢 Very easy (single container) |
| **Logseq** | AGPL-3.0 | ClojureScript / React / Electron | ~43.9k | ~339 | 2.0.1 (Jul 2026) | ✅ (outliner blocks) | 🟡 queries/advanced (Datalog) | 🟡 partial sync, not primary | ✅ plugin API | 🟡 Docker webapp available; local-first by design |
| **SilverBullet** | MIT | TypeScript / Deno / Preact / CodeMirror 6 | ~5.6k | ~164 | 2.9.0 (Jun 2026) | ✅ (Markdown + blocks) | ✅ (live queries over frontmatter) | 🟡 limited concurrent editing | ✅ plug API | 🟢 Very easy (single Docker / Deno) |

> Star, fork, contributor and release figures are pulled live from the GitHub API on July 16, 2026 and rounded.

---

## Detailed Profiles

### 1. Outline — The fastest knowledge base for growing teams

- **URL:** <https://github.com/outline/outline>
- **License:** Business Source License 1.1 (BSL 1.1). *Source is open but you may not offer Outline as a hosted SaaS to third parties. Internal self-hosting is explicitly allowed.* After the Change Date (typically 4 years) the code converts to Apache 2.0 for older versions.
- **Tech Stack:** TypeScript / React / Node.js (Koa). Uses PostgreSQL for storage, Redis for caching/websocket coordination, ProseMirror/Yjs for the collaborative editor.
- **Deployment Options:**
  - Official Docker image + docker-compose (recommended).
  - Manual install via Node.js build + Postgres + Redis + S3-compatible object storage.
  - AWS/GCP/Azure friendly; community Helm charts exist for Kubernetes.
  - Requires OIDC/SAML/Slack/Google/GitHub/etc. for auth (no built-in email/password — by design).
- **Key Features vs. Notion:**
  - Excellent block-based editor (headings, lists, todos, quotes, code, callouts, embeds, tables).
  - Nested pages, collections, backlinks, mentions, slash commands — very Notion-like UX.
  - Real-time multi-user collaboration powered by Yjs/CRDT; presence indicators, comment threads.
  - Rich permissions (collection-level, user groups, guest sharing).
  - Slack, Google Workspace, Azure AD, OIDC, SSO integrations; Zapier/webhook support.
  - REST API; public share links; import from Notion, Markdown, HTML, Confluence.
  - Search is fast and full-text; document version history.
- **Community Health:**
  - ~39.7k GitHub stars, ~3.4k forks.
  - Very active: commits pushed daily (last push within hours of this report), releases weekly to monthly (v1.9.1 on Jul 13, 2026).
  - ~251 contributors; maintained by a small core team (the Outline company) plus community PRs.
  - Issues are triaged well; only ~60 open issues at time of writing — a sign of tight maintainership.
- **Limitations vs. Notion:**
  - **Not a database/spreadsheet tool.** No Airtable-like tables with multiple views (kanban/grid/calendar/gallery). For that you'll still need something else (NocoDB, Trello, etc.).
  - No true whiteboard/canvas mode.
  - BSL 1.1 is **not OSI-approved** — companies with strict "OSI-only" open source policies may reject it.
  - No built-in email/password auth; must bring your own SSO (okay for teams on Google Workspace/OIDC, friction for very small teams).
  - Limited native mobile apps (web-first; mobile web works but no dedicated iOS/Android app as good as Notion's).
  - Template ecosystem smaller than Notion's.
- **Self-hosting Complexity:** 🟢 **Low-to-moderate.** The official `docker-compose.yml` brings up postgres/redis/minio + Outline in one command. Upgrades are usually pull-new-image + run migrations. Backups are a Postgres dump away. Plan for an S3-compatible bucket (or Minio) for file uploads.

---

### 2. AFFiNE — Write, Draw, Plan All at Once

- **URL:** <https://github.com/toeverything/AFFiNE>
- **License:** Client/core libraries under **MIT**; the self-hosted server (`affine-self-hosted` / `affine-ce`) is released under a mix that contains AGPL components in some modules. The core editor framework **BlockSuite** (powering the editor) is MIT-licensed.
- **Tech Stack:** TypeScript / React / Next.js for the frontend; **BlockSuite** (custom block editor framework); **OctoBase** (Rust-based CRDT storage layer, MIT) for local-first sync; PostgreSQL + Redis in self-host mode.
- **Deployment Options:**
  - Official Docker image `ghcr.io/toeverything/affine-ce` (community edition) with docker-compose.
  - Desktop apps for Windows/macOS/Linux (Electron/Tauri-based); iOS and Android clients.
  - Can be run purely local-first (no server required) with optional sync server.
  - Kubernetes manifests maintained by community.
- **Key Features vs. Notion:**
  - **Edgeless mode** — an infinite whiteboard/canvas that lives in the same document model as regular pages. This is AFFiNE's biggest differentiator vs. Notion (Notion has only limited canvas features). Combine docs, sticky notes, embedded media, shapes, and slides on one canvas.
  - Block-based editor with rich blocks, databases/collections (table, kanban, list, calendar views added across recent releases).
  - **Local-first by default:** data lives on disk; optional cloud sync via CRDTs means offline-first is native, not an afterthought.
  - Real-time multi-user collaboration (CRDT-based; improving each release).
  - Built-in AI assistant (can generate text, mind maps, slide decks from notes).
  - Cross-platform: Web, Desktop, Mobile.
- **Community Health:**
  - ~70.5k stars, ~5.1k forks — the fastest-growing project in this space.
  - ~256 contributors, releases roughly **every 1–2 weeks** (v0.27.0 on Jul 15, 2026).
  - Backed by ToEverything, a well-funded startup (>$10M raised), so development velocity is very high.
  - Active Discord; daily commits.
- **Limitations vs. Notion:**
  - **Pre-1.0 rapid iteration** — features are still landing fast, which means UI/UX churn and occasional breaking changes between versions.
  - Self-hosted collaboration is functional but the deployment story has historically been bumpy (it has improved significantly with `affine-ce`).
  - Permission model and enterprise admin features (SSO, SCIM, audit logs) are less mature than Outline or Notion.
  - Fewer third-party integrations than Notion.
  - Some advanced database capabilities (rollups, complex formulas, relation fields compared to Notion) are still maturing.
- **Self-hosting Complexity:** 🟡 **Moderate.** Official Docker Compose gets you running, but the stack includes Postgres, Redis, and the AFFiNE server. Upgrades during pre-1.0 occasionally require migration steps. For a startup willing to tolerate a bit of velocity, it's manageable.

---

### 3. AppFlowy — AI collaborative workspace, data in your control

- **URL:** <https://github.com/AppFlowy-IO/AppFlowy> (core) / <https://github.com/AppFlowy-IO/AppFlowy-Cloud> (self-host server)
- **License:** **AGPL-3.0** for the main codebase.
- **Tech Stack:**
  - Core editor/collab engine written in **Rust** (Flowy SDK, uses lib0 CRDT).
  - Desktop/mobile UI in **Flutter** (Dart) for native cross-platform performance (Windows, macOS, Linux, iOS, Android).
  - Web client newly rebuilt in **React/TypeScript** (`AppFlowy-Web`).
  - AppFlowy Cloud (self-host backend) in Rust; Postgres + Redis + object storage.
  - SQLite for local/offline storage.
- **Deployment Options:**
  - Native desktop/mobile apps (signed binaries from GitHub Releases; stores).
  - Self-hosted **AppFlowy Cloud** via Docker Compose (Postgres + Redis + Rust services + Nginx proxy).
  - Manual build possible (Flutter + Rust toolchain).
  - Community Kubernetes charts.
- **Key Features vs. Notion:**
  - Block-based document editor with rich blocks, slash menu, markdown shortcuts.
  - **Multi-view databases**: grid, board (kanban), calendar views of table data — closest Notion-database parity among open source options.
  - Real-time collaboration (via AppFlowy Cloud, using CRDTs) with guest sharing.
  - Built-in **AI assistant** integrated into the editor (summarize, write, translate — pluggable to OpenAI / local models).
  - Cross-platform native clients that feel fast (Flutter gives near-native performance vs. Electron).
  - Offline-first local data (SQLite) with optional sync to self-hosted cloud.
  - REST/gRPC API; plugin/extension marketplace growing; selective page sharing.
  - Theme customization / dark mode / customizable shortcuts.
- **Community Health:**
  - ~73.9k stars, ~5.6k forks — the most-starred project in this category.
  - ~381 contributors; releases roughly monthly (0.12.5, Jun 23, 2026); active commits daily.
  - Funded by AppFlowy Inc., with a commercial cloud offering that funds OSS development.
  - Active Discord; community plugins ecosystem is growing.
- **Limitations vs. Notion:**
  - Self-hosted cloud is **multi-service** and heavier to operate (several Rust services + PG + Redis + Minio).
  - Template and integration marketplace still much smaller than Notion's.
  - Comments and granular permissions are functional but less polished than Notion/Outline.
  - The React web client is newer; parity with the Flutter desktop client is still catching up.
  - AI features require bringing your own model key (OpenAI or self-hosted); not bundled for free.
  - Migrating an existing large Notion workspace is not fully seamless (importer exists but isn't 1:1).
- **Self-hosting Complexity:** 🟡 **Moderate.** You'll be running a mini-service-architecture (3+ Rust services + DB + Redis + Minio + Nginx). The community `docker-compose` works but isn't as turnkey as Outline's. Good for teams with DevOps capacity; heavier for a founder deploying solo.

---

### 4. BookStack — Simple, opinionated, wiki-style documentation

- **URL:** Development now lives on **Codeberg**: <https://codeberg.org/BookStack/BookStack> (legacy GitHub mirror: <https://github.com/BookStackApp/BookStack>)
- **License:** **MIT**
- **Tech Stack:** **PHP** (8.2+) on **Laravel** framework; MySQL/MariaDB or PostgreSQL backend; TinyMCE or Lexical WYSIWYG editor; Markdown-it for Markdown rendering; Bootstrap-based frontend.
- **Deployment Options:**
  - Official Docker image (linuxserver/bookstack is the most popular community image).
  - Traditional LAMP/LEMP stack install (Composer + PHP + web server + MySQL).
  - One-click installs on many cloud marketplaces.
- **Key Features vs. Notion:**
  - Opinionated **Shelf → Book → Chapter → Page** hierarchy — great for structured documentation (runbooks, runbooks, onboarding), less free-form than Notion.
  - WYSIWYG + Markdown dual editor; code blocks, callouts, images, attachments.
  - Multi-user editing, role-based permissions (public/private books, chapter/page-level ACL).
  - Built-in authentication: email/password + social login (GitHub, Google, Slack, Azure AD, Okta, LDAP, SAML via add-ons).
  - Full REST API for automation; webhooks; revision history per page.
  - Full-text search with relevance ranking; cross-book search.
  - 40+ language translations; customizable themes; content export to PDF/HTML/Markdown.
- **Community Health:**
  - ~18.9k GitHub stars (plus Codeberg presence).
  - Steady, mature project (founded 2015), releases roughly monthly.
  - Led by a core maintainer (Dan Brown) with consistent contributions.
  - Development moved to Codeberg in 2026 — note this when watching for updates.
- **Limitations vs. Notion:**
  - **Not a block editor / no "Notion databases"**. No inline kanban, calendar views, spreadsheet grids, or Airtable-style tables on a page. It is a wiki, not a workspace OS.
  - No real-time collaborative cursors (multi-user edit is supported but sequential; last-write-wins/merge is not CRDT-based).
  - No whiteboard / canvas.
  - Hierarchical structure is rigid — you can't freely nest pages the way you do in Notion.
  - Older-feeling UI compared to Outline/AFFiNE/AppFlowy.
  - No native mobile apps (web responsive).
- **Self-hosting Complexity:** 🟢 **Very low.** A single PHP container + MySQL. Any developer who has deployed Laravel can run it. Backups are trivial (DB dump + file storage). Lowest ops burden of any tool on this list.

---

### 5. Logseq — Privacy-first outliner / knowledge graph

- **URL:** <https://github.com/logseq/logseq>
- **License:** **AGPL-3.0**
- **Tech Stack:** **ClojureScript** frontend (React + Electron for desktop); Rust for the sync/backend components; stores data as plain **Markdown or Org-mode** files on the local filesystem.
- **Deployment Options:**
  - Native desktop apps for Windows/macOS/Linux (primary).
  - iOS and Android apps.
  - Webapp available via Docker (`ghcr.io/logseq/logseq-webapp`) for self-hosted in-browser use.
  - By default, **local-first** — you pick a folder on disk; Git is the recommended sync mechanism for solo users. The Logseq Sync service (official paid offering) is optional; self-hosted sync server options exist in the community.
- **Key Features vs. Notion:**
  - **Outliner-first block editor** — every line is a block that can be linked, referenced, embedded.
  - **Bidirectional links (`[[page]]`)** and **block references** (`((block-id)))` — pioneered the Roam-style knowledge graph.
  - Whiteboards (built-in, since v1.x), PDF annotation, flashcards/spaced repetition, queries (Datalog-based) over your graph.
  - Daily journal / Zettelkasten-flavored workflow.
  - Namespaces and tags for organization.
  - Plugin marketplace (150+ plugins), themes, custom CSS.
  - Vim/Emacs keybindings; developer-friendly.
- **Community Health:**
  - ~43.9k stars, ~2.7k forks, ~339 contributors.
  - Released **2.0.1** on Jul 13, 2026 — major v2 landed in 2026 with DB-version backend improvements; active commit activity daily.
  - Very passionate community (Reddit r/logseq, Discord, forums).
- **Limitations vs. Notion:**
  - **Outliner-first is a different paradigm** than Notion's free-form docs/blocks/databases. Non-technical teammates often find it disorienting.
  - **Weak team collaboration story.** Self-hosted multi-user editing is not the primary use case; sync is primarily for multi-device individual use.
  - No native databases/kanban/calendar views (plugins can emulate some).
  - No native whiteboard equivalent to Miro (added whiteboard is more a freeform space for blocks, not a diagramming canvas).
  - Storing everything as Markdown/Org files is great for portability but makes some "database-like" queries slow on large graphs.
  - Web/Docker version is not as polished as the desktop app.
- **Self-hosting Complexity:** 🟡 **Medium.** Docker webapp is easy, but "true" self-hosted multi-user sync requires community solutions (no officially supported open source sync server). Most teams end up using it in "local folder + Git" mode rather than a centralized server.

---

### 6. SilverBullet — The "knowledge hacker's" notebook

- **URL:** <https://github.com/silverbulletmd/silverbullet>
- **License:** **MIT**
- **Tech Stack:** **Deno** (TypeScript) server + **Preact** client; **CodeMirror 6** editor; Oak HTTP framework; ESBuild. Notes stored as **plain Markdown files** on disk (plus a SQLite index for queries).
- **Deployment Options:**
  - Single Docker image (`zefhemel/silverbullet`) — easiest path.
  - Direct Deno install (`deno task install`).
  - Desktop PWA (install from browser); mobile browser works.
  - Pluggable "spaces" (backends) allow sync via WebDAV, S3, etc.
- **Key Features vs. Notion:**
  - All content is **plain Markdown files** on your server's filesystem — zero lock-in.
  - **Frontmatter + Live Queries + Templates** turns notes into a queryable database (e.g., build a task list across all pages with a `#task` tag, rendered live).
  - Slash commands, wikilinks, embeds, code blocks, math.
  - End-user scripting via its "Space Script" (JavaScript in `PLUGS`); rich plug ecosystem.
  - Offline mode with PWA; end-to-end encryption option for client-side.
  - Command palette and vim-friendly keybindings.
- **Community Health:**
  - ~5.6k stars, ~438 forks, ~164 contributors.
  - Releases monthly (2.9.0, Jun 11, 2026); active daily commits.
  - Primarily authored/maintained by Zef Hemel, with an active plug-developer community.
- **Limitations vs. Notion:**
  - **Niche / hacker-oriented.** Target audience is "people with a hacker mindset" — your PMs and designers may bounce off.
  - Limited WYSIWYG; you write Markdown. Not as drag-and-drop as Notion.
  - No true multi-user real-time collaboration (single-user or light concurrent editing; sharing is via file sync or federation).
  - No database views like kanban/calendar/gallery (queries render as lists/tables).
  - No whiteboard.
  - Smaller ecosystem and team-facing features (permissions, SSO, audit logs).
- **Self-hosting Complexity:** 🟢 **Very low.** Literally `docker run -p 3000:3000 -v ./space:/space zefhemel/silverbullet`. Single Deno process, plain Markdown files on disk, trivial backups. The easiest to run, period.

---

## Honorable Mentions

- **Memos** (<https://github.com/usememos/memos>) — ~61.6k stars, MIT, Go/React. Excellent lightweight "memo/tweet-style" note hub with great self-host story. **Too lightweight to replace Notion** for docs/KB, but great as an addition.
- **NocoDB** (<https://github.com/nocodb/nocodb>) — ~64k stars, AGPL-3.0. The leading open source Airtable alternative. **Complements** (not replaces) the tools above — use it when you need a relational database UI, not docs.
- **TriliumNext Notes** (~2.9k stars, AGPL-3.0) — successor to Trilium Notes (which was archived). Hierarchical personal notes; development activity has slowed (last release Jun 2025). Currently risky as a primary platform.
- **Wiki.js** — MIT, Node.js + Vue wiki. Good traditional wiki, less Notion-like.
- **Anytype** (<https://github.com/anyproto/anytype-ts>) — local-first, any-sync protocol, not fully open-to-self-host for the server side; worth tracking but excluded because a self-hosted multi-user server story isn't as straightforward.

---

## Recommendation & Ranking

For a **startup team** whose primary uses are **(1) internal knowledge base, (2) project documentation, (3) collaborative project notes**, and whose constraint is **self-hosting for data sovereignty**, we rank as follows:

### 🥇 #1 Outline — Recommended default

- **Why:** It is the tool that most closely recreates the Notion team-wiki experience out of the box: great block editor, mature real-time collab, solid permissions, strong search, easy Slack/SSO integration, and a very small operational footprint (one docker-compose up).
- **When to pick:** You want to replace Notion's "team wiki + docs" function and don't need Notion's database/spreadsheet features.
- **Caveat:** BSL 1.1 is not OSI-approved; if your company policy requires an OSI license, move to #2 or #3.
- **Ops profile:** A small startup with no dedicated DevOps can run it reliably.

### 🥈 #2 AFFiNE — Best if you want docs + whiteboard + rapid evolution

- **Why:** It uniquely combines docs (Notion) with an infinite canvas (Miro) and is local-first. The pace of development is extraordinary, and the MIT-licensed core is license-friendly.
- **When to pick:** Your team brainstorms/whiteboards a lot alongside docs, or you want to be on the most forward-looking platform.
- **Caveat:** Still in heavy pre-1.0 motion; expect occasional churn and growing pains in admin/permissions.
- **Ops profile:** Manageable with Docker Compose, but expect to pay more attention to upgrades than Outline.

### 🥉 #3 AppFlowy — Best if you want native cross-platform clients + AI + OSI license

- **Why:** The Flutter+Rust clients feel fast natively on every OS; databases/kanban/calendar are closer to Notion's than Outline's; AI is built in; AGPL is OSI-approved.
- **When to pick:** Heavy mobile/desktop app usage matters; your team wants offline-native clients; strict OSS licensing required.
- **Caveat:** Self-hosting the collab cloud is more complex (multiple services).
- **Ops profile:** Best for teams with at least one person comfortable running a small Rust-based microservice stack.

### #4 BookStack — Best for engineering docs / runbooks / SOPs

- **Why:** Zero-frills, rock-solid, easiest to maintain for years. MIT-licensed, battle-tested since 2015.
- **When to pick:** You need an engineering / IT-style knowledge base (runbooks, onboarding, architecture docs) and don't need flexible blocks/databases.
- **Caveat:** It is a wiki with a fixed hierarchy, not a Notion-style flexible workspace.
- **Ops profile:** Single PHP container + MySQL; set-and-forget.

### #5 Logseq — Best as a personal "second brain" alongside a team wiki

- **Why:** Best-in-class outliner + bidirectional links + knowledge graph. Great for individual researchers, PMs, engineers building personal knowledge bases.
- **When to pick:** Individual knowledge management first; team wiki second. Pairs well with BookStack/Outline as a personal layer.
- **Caveat:** Team collaboration is not its strong suit; the outliner paradigm divides teams.

### #6 SilverBullet — Best for technical founders as a personal productivity tool

- **Why:** Markdown-as-database, infinitely hackable via plugs, trivial to self-host.
- **When to pick:** Technical founders, note-taking power users, hackers. Not recommended as the **team's** primary KB without heavy customization.
- **Caveat:** Niche UX; non-technical teammates will struggle.

---

## Suggested Starter Stacks

Depending on team composition:

1. **Small engineering-led team (<15 people), want "just works":**
   - **Outline** (team wiki + docs) + **NocoDB** (if you need light databases/kanbans)

2. **Product/design-heavy team that does whiteboarding:**
   - **AFFiNE** (docs + canvas + databases) — consider adding a NocoDB if you outgrow AFFiNE's DB features

3. **OSI-license-only policy + strong mobile/desktop need:**
   - **AppFlowy** (with AppFlowy Cloud self-hosted)

4. **Strict runbooks / SOPs / engineering wiki focus:**
   - **BookStack** (wiki) + **SilverBullet** (founder/lead engineer personal notes)

5. **Research-heavy / knowledge-graph team:**
   - **Logseq** (personal + small-team graph) + **Outline** (published team docs)

---

## Sources & Notes on Data

- GitHub star / fork / contributor / release data: GitHub REST API, queried live July 16, 2026.
- License data verified via project `LICENSE` files and official documentation:
  - Outline: BSL 1.1 (see <https://github.com/outline/outline/blob/main/LICENSE>).
  - AFFiNE: Client/core MIT; self-host server is the Community Edition with mixed licensing (MIT/AGPL components).
  - AppFlowy, Logseq: AGPL-3.0.
  - BookStack, SilverBullet, Memos: MIT.
  - NocoDB: AGPL-3.0 (community edition).
- Feature claims cross-checked against official docs and community guides published 2025–2026.
- BookStack moved primary development from GitHub to Codeberg in 2026; star counts reflect the legacy GitHub mirror at time of writing.
