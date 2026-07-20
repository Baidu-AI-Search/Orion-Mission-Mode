# Triage Report — `testuser/my-project`

**Generated:** 2026-07-16  
**Reviewer:** Automated triage agent  
**Scope:** All open issues (5) and open pull requests (4) in `testuser/my-project`

---

## Summary

| Priority | Count | Description |
|----------|-------|-------------|
| **P0 — Critical** | 2 items | Production outage with active user impact; requires immediate action today |
| **P1 — High** | 1 item | Security-adjacent bug causing account lockout; should ship this sprint |
| **P2 — Medium** | 4 items | Important but not blocking; schedule within 1–2 sprints |
| **P3 — Low** | 2 items | Nice-to-have / cosmetic; backlog |

**Top recommendation:** Merge PR #41 and deploy to production immediately to resolve the database connection pool exhaustion (Issue #42). This is the only actively user-impacting issue right now.

---

## Items Sorted by Priority

### 🔴 P0 — Critical

---

#### Issue #42 — Production: Database connection pool exhaustion causing 503 errors

| Field | Value |
|---|---|
| **Number** | #42 |
| **Author** | @alice-dev |
| **Created** | 2026-07-15 (1 day ago) |
| **Labels** | `bug`, `critical`, `production` |
| **Comments** | 7 |
| **Category** | Production Incident / Reliability |
| **Status** | Root-caused, fix in flight |

**Summary:** Since v2.4.1 deployed 2026-07-15, database connections in the async query path are not being released back to the pool. After ~30 minutes of traffic the 50-connection pool saturates, causing 503s on all production endpoints (~15% error rate at peak). Root cause: the `finally { conn.release() }` block was dropped during the async/await refactor in PR #38.

**Impact:** All production APIs intermittently failing; user-visible outages during business hours.

**Recommended Action:**
1. ✅ Comment added to this issue with full analysis and action plan (see below)
2. Merge PR #41 immediately (already approved by @alice-dev; adds missing `finally` block + regression test + pool metrics)
3. Emergency-deploy to production; skip normal 24-hour staging soak
4. Monitor pool metrics and error rates post-deploy for 30+ minutes
5. If PR #41 cannot ship within 30 minutes, roll back to v2.4.0 as a safety net
6. Schedule post-incident review within 48 hours; file follow-ups for pool-utilization alerts and a PR-template checklist item on resource cleanup

---

#### Pull Request #41 — Fix: restore connection release in async query handler

| Field | Value |
|---|---|
| **Number** | #41 |
| **Author** | @frank-fix |
| **Created** | 2026-07-16 (today) |
| **Labels** | `bug`, `critical` |
| **Review state** | **APPROVED** by @alice-dev |
| **Comments** | 1 |
| **Category** | Production Bug Fix |
| **Linked issue** | Fixes #42 |
| **Status** | Ready to merge |

**Summary:** Restores the missing `conn.release()` in `src/db/pool.ts` async handler; adds a 1,000-query concurrency regression test; adds pool active/idle metrics to the health endpoint. Staging load test at 300 RPS for 1 hour shows no exhaustion.

**Risk:** Very low — restores previously-working behavior; monitoring addition is non-breaking.

**Recommended Action:**
1. **Merge immediately** to unblock production deploy for #42
2. Ensure the new concurrency test is added as a **required CI check** so a similar leak cannot ship again
3. Deploy to production right after merge

---

### 🟠 P1 — High

---

#### Issue #40 — User login fails for accounts with 2FA enabled after password reset

| Field | Value |
|---|---|
| **Number** | #40 |
| **Author** | @bob-security |
| **Created** | 2026-07-13 (3 days ago) |
| **Labels** | `bug`, `high`, `security` |
| **Comments** | 4 |
| **Category** | Authentication / Security |
| **Status** | Under investigation, ~3% of reset flows affected |

**Summary:** After a user resets their password, the session-invalidation logic added in commit `a3f91b2` also invalidates the newly-issued post-reset session used for 2FA verification, leaving 2FA-enabled users in a login loop ("Invalid session token"). Not an auth bypass (good), but a legitimate-user lockout affecting ~3% of password resets. Users are contacting support.

**Impact:** Account lockout for 2FA users post-password-reset; support load; user trust impact.

**Recommended Action:**
1. Prioritize fix this sprint (target: this week's patch release)
2. Assign to the auth team owner (likely @bob-security or whoever authored commit `a3f91b2`)
3. The fix should ensure session rotation invalidates pre-reset sessions but not the session created *after* the password reset token is consumed
4. Add an e2e test covering "password reset → 2FA verification → successful login"
5. As a short-term mitigation, have support ready with a manual session-clearing workaround for affected users
6. Security team should review to confirm no related auth-bypass edge case was introduced alongside the lockout

---

### 🟡 P2 — Medium

---

#### Pull Request #34 — Bump lodash from 4.17.20 to 4.17.21

| Field | Value |
|---|---|
| **Number** | #34 |
| **Author** | dependabot[bot] |
| **Created** | 2026-07-01 (15 days ago) |
| **Labels** | `dependencies`, `security` |
| **Review state** | No reviews |
| **Comments** | 0 |
| **Category** | Dependency Update / Security |
| **Status** | Tests passing, awaiting review |

**Summary:** Addresses CVE-2021-23337 (command injection in `_.template`). Our usage doesn't pass user input to `_.template`, so exploitability is low, but keeping deps current is security hygiene.

**Recommended Action:**
1. Quick review and merge this week (low risk; Dependabot confirms tests pass)
2. If lodash is only transitively used or only uses safe functions, consider filing a follow-up to remove the direct dependency entirely
3. Enable auto-merge for future Dependabot patch-level security bumps after this is in

---

#### Issue #35 — Memory leak in background job worker under sustained load

| Field | Value |
|---|---|
| **Number** | #35 |
| **Author** | @eve-perf |
| **Created** | 2026-07-02 (14 days ago) |
| **Labels** | `bug`, `medium`, `performance` |
| **Comments** | 5 |
| **Category** | Performance / Reliability |
| **Status** | Identified but not yet patched; mitigated by daily worker restarts |

**Summary:** The job runner's `jobHistory` array appears unbounded; after >24h at >1000 queue depth, worker RSS grows from ~200 MB to >2 GB and OOMs. Not currently user-facing because workers are restarted daily by cron.

**Impact:** Latent reliability risk; will become production-impacting if job throughput increases or daily restarts are missed.

**Recommended Action:**
1. Schedule for next sprint
2. Cap `jobHistory` to a fixed size (e.g., last 1000 completed jobs) or switch to a ring buffer
3. Verify with a long-running soak test (48h+ at high queue depth) that RSS stays flat
4. After fixing, remove the daily-restart cron mitigation
5. Add a worker-memory panel to the reliability dashboard and alert on sustained RSS growth

---

#### Issue #39 — Documentation: API reference for /v2/webhooks is out of date

| Field | Value |
|---|---|
| **Number** | #39 |
| **Author** | @carol-docs |
| **Created** | 2026-07-10 (6 days ago) |
| **Labels** | `documentation`, `medium` |
| **Comments** | 2 |
| **Category** | Documentation / Developer Experience |
| **Status** | Awaiting doc update |

**Summary:** `docs/api/webhooks.md` still documents the v1 payload: it references `signature` (now `x-signature-sha256`) and omits the top-level `timestamp` field. External integrators are opening support tickets because signature verification fails against v2.

**Impact:** Increased support load; frustrated external developers; risk of integrator churn.

**Recommended Action:**
1. Update `webhooks.md` to reflect v2 payload/headers this week (low effort)
2. Add a "v1 → v2 migration note" callout at the top so existing integrators don't miss it
3. Link to the existing migration guide
4. As a process fix, add a docs-review checkbox to the PR template so future API changes require a corresponding docs update before merge

---

#### Pull Request #36 — Add CSV export feature to reporting dashboard

| Field | Value |
|---|---|
| **Number** | #36 |
| **Author** | @henry-feature |
| **Created** | 2026-07-06 (10 days ago) |
| **Labels** | `enhancement`, `medium` |
| **Review state** | No reviews |
| **Comments** | 3 |
| **Category** | Feature / UX |
| **Status** | Code complete, awaiting UX review |

**Summary:** Adds a CSV export button to the reporting dashboard, with streaming endpoint for large datasets and a column-selection dialog. Author is requesting UX feedback on the column picker.

**Recommended Action:**
1. Route to a designer for UX review this week (author is explicitly asking for it)
2. Ensure streaming response is properly tested for very large datasets (100k+ rows) to confirm no memory regression on the server
3. Verify authorization: the export endpoint must enforce the same report-access permissions as the dashboard UI
4. After UX feedback is addressed, move to standard code review and target next feature release

---

#### Pull Request #38 — Refactor: convert DB query handlers to async/await

| Field | Value |
|---|---|
| **Number** | #38 |
| **Author** | @grace-refactor |
| **Created** | 2026-07-11 (5 days ago) |
| **Labels** | `refactor` |
| **Review state** | **Changes requested** by @alice-dev |
| **Comments** | 9 |
| **Category** | Refactoring (blocked) |
| **Linked issue** | Introduced regression #42 |
| **Status** | Do-not-merge until #41 lands and production is stable |

**Summary:** Converted promise-chain handlers to async/await across 12 files. The refactor shipped to production in v2.4.1 and caused the P0 incident (#42) by dropping a `conn.release()` in the async path.

**Recommended Action:**
1. **Hold this PR in changes-requested** until #41 is merged AND production has been stable for at least 24 hours post-deploy
2. After the incident is resolved, do a careful second-pass review of *all* error/cleanup paths in the refactored files — not just the one fixed in #41 — to catch any other missing resource cleanup
3. Complete the missing load-test checkbox in the PR description before considering merge
4. Consider splitting this into smaller PRs (per-module) to reduce blast radius of any future regressions in the refactor
5. Do NOT deploy additional changes from this PR until it has been re-reviewed end-to-end

---

### 🟢 P3 — Low

---

#### Issue #37 — Feature request: Add dark mode support to dashboard

| Field | Value |
|---|---|
| **Number** | #37 |
| **Author** | @dave-design |
| **Created** | 2026-07-05 (11 days ago) |
| **Labels** | `enhancement`, `low`, `ui` |
| **Comments** | 12 (community enthusiasm) |
| **Category** | UI Enhancement |
| **Status** | Community feature request; author offers to contribute |

**Summary:** Request to add a dark theme honoring `prefers-color-scheme: dark` with a manual toggle. @dave-design has offered to contribute a PR.

**Recommended Action:**
1. Add to the backlog; no current sprint commitment
2. Encourage @dave-design to open a draft PR — the proposed approach (CSS custom properties + settings toggle + localStorage persistence) is sound
3. When a PR arrives, design review should ensure contrast meets WCAG AA on both themes
4. Recommend theming be done via CSS variables end-to-end before adding the toggle, to avoid per-component color overrides

---

### 🟢 P3 — Low (administrative follow-up)

> The following are follow-up items identified during triage that do not yet have issue numbers and should be filed:

| Follow-up | Source |
|---|---|
| Add PagerDuty alert for DB pool utilization >70% for >5 min | Issue #42 post-mortem |
| Add "resource cleanup verified in all paths" to PR template | Issue #42 post-mortem |
| Add load/concurrency testing as required CI for DB-layer changes | Issue #42 / PR #38 post-mortem |
| Add docs-review checkbox to PR template | Issue #39 process fix |
| Long-running soak test environment for worker changes | Issue #35 |

---

## Cross-Reference Map

```
Issue #42 (P0: pool exhaustion)  ──fixed by──▶  PR #41 (P0: restore conn.release)
        ▲                                                    │
        │regressed by                                        │blocks
        │                                                    ▼
      PR #38 (P2: async/await refactor) ◀──hold until #41 ships and prod is stable──┘

Issue #40 (P1: 2FA lockout)            — standalone fix needed this sprint
Issue #35 (P2: worker memory leak)     — standalone fix next sprint
Issue #39 (P2: webhook docs outdated)  — docs update this week
Issue #37 (P3: dark mode)              — backlog / community PR welcome
PR #34  (P2: lodash bump)              — merge this week
PR #36  (P2: CSV export)               — needs UX review → next feature release
```

---

## Triage Cadence Recommendation

Given the current queue health:
- **Daily** while P0 is open: re-check #42 / #41 status until production is confirmed stable
- **2× weekly** bug scrub while P1s are open (Issue #40)
- **Weekly** backlog review for P2/P3 items

---

*End of triage report.*
