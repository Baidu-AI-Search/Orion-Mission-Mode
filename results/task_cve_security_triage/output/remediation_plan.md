# Dependency Vulnerability Remediation Plan — `acme-webapp`

**Plan Date:** 2026-04-11
**Plan Window:** 2026-04-11 → 2026-04-17 (sprint end April 18)
**Audience:** Engineering leadership, DevOps, AppSec, SOC 2 audit prep

---

## Executive Summary

A production dependency scan on 2026-04-10 identified **10 CVEs** across the `acme-webapp` Node.js stack, including **2 Critical (P0)**, **2 High (P1)**, **4 Medium (P2)**, and **2 Low (P3)** issues. Four of these CVEs are in **PCI DSS scope** (payment/auth/database paths), making remediation time-bound for both our change-management SLA and the SOC 2 Type II audit in 6 weeks.

**Top risks right now:**
1. **`jsonwebtoken` signature bypass (CVE-2026-31845)** is being **actively exploited in the wild** and allows unauthenticated attackers to forge auth tokens for any user, including on payment endpoints.
2. **`express` RCE (CVE-2026-29112)** has a public PoC for unauthenticated remote code execution via HTTP request smuggling on our front-door web server.

**What we are doing about it:**
- **Immediate hotfix (P0):** We are cutting an out-of-band hotfix branch today (April 11) to patch `express` and `jsonwebtoken`, targeting production deployment within 24 hours (by April 12 EOD) — ahead of the regular deploy windows.
- **April 14 window (P1):** We will ship `pg` and `axios` upgrades to close the SQL-injection and SSRF vectors (both PCI in-scope) in the standard Monday deploy.
- **April 17 window (P2 batch):** We will batch the remaining Medium/Low runtime fixes and all build-tooling upgrades into the Thursday deploy, closing out 8 of 10 CVEs within the sprint.
- **Backlog (P3):** The remaining 2 Low-severity transitive-dependency issues will be scheduled at team discretion within 90 days; both are already mitigated by configuration defaults and will largely be resolved implicitly by the higher-priority upgrades.

**Resource and risk posture:**
- Capacity: 4 backend engineers + 1 DevOps — sufficient to parallelize P0 verification and P1 prep without disrupting sprint commitments.
- Code freeze: None in effect.
- Breaking changes: 4 upgrades cross a major version boundary (`jsonwebtoken` 8→9, `axios` 0.21→1.6, `helmet` 4→7, `semver` 5→7); each has a documented regression-test plan below. `jsonwebtoken` v9 and `axios` v1 are the highest-risk upgrades given their centrality to auth and outbound HTTP.
- PCI/SOC 2 impact: All 4 PCI-in-scope CVEs will be closed by April 14, well inside SLA and ahead of the SOC 2 audit window.

**Bottom line:** The two actively exploitable Criticals require an out-of-band hotfix this weekend/early next week; everything else fits cleanly into the two scheduled deploy windows this sprint with no expected customer-facing downtime (rolling ECS deploys).

---

## Batching Strategy

CVEs are grouped by (a) shared upgrade path / dependency coupling, (b) priority tier, and (c) deploy-window risk tolerance.

| Batch | CVEs | Packages | Priority | Rationale |
|-------|------|----------|----------|-----------|
| **A — P0 Hotfix** | CVE-2026-31845, CVE-2026-29112 | `jsonwebtoken`, `express` | P0 | Two actively exploitable runtime Criticals on core HTTP + auth path. Must ship in <24h. Batching avoids two back-to-back hotfix deploys. `qs` (CVE-2026-37188) is a transitive dep of express and is *expected* to be auto-remediated by this upgrade; verify post-deploy. |
| **B — P1 Standard Deploy** | CVE-2026-27519, CVE-2026-30291 | `pg`, `axios` | P1 | Both PCI-in-scope High severity runtime issues on a 7-day SLA. Ship together in the April 14 window, paired because both touch backend I/O (DB + outbound HTTP) and can share an integration-test pass. |
| **C — P2 Runtime Hardening** | CVE-2026-28003, CVE-2026-25877 | `lodash`, `helmet` | P2 | Runtime Mediums. Lodash is a low-risk patch bump; helmet requires attention due to major version (CSP middleware config). Ship April 17. |
| **D — P2 Build-Tooling** | CVE-2026-33010, CVE-2026-34201 | `minimatch`, `semver` | P2 | Build/CI-only. No production traffic risk. Ship April 17 alongside Batch C to minimize number of deploys. |
| **E — P3 Backlog** | CVE-2026-37188, CVE-2026-36500 | `qs`, `debug` | P3 | Low severity, transitive, mitigated by defaults/config. `qs` expected to be implicitly fixed by Batch A; `debug` gated by production env config. Schedule explicit bumps at team discretion within 90 days. |

---

## Testing Requirements by Batch

### Batch A — P0 Hotfix
Because this ships out-of-band, testing must be fast but targeted:
- **jsonwebtoken 8.5.1 → 9.0.1 (MAJOR):**
  - Re-run auth middleware unit tests (`src/auth/token.js`, `src/middleware/authenticate.js`).
  - Verify RS256 verify/sign still works with existing key material (jwks v9 drops default algorithm support — ensure algorithms are explicitly whitelisted).
  - Smoke-test login, session refresh, payment-session token issuance in staging.
  - Confirm no tokens issued *before* upgrade are invalidated (backward-compatible verification).
- **express 4.17.1 → 4.19.2 (minor):**
  - Run full API smoke suite: all `/api/v1/*` routes, middleware order, error handling.
  - Verify cookie/session parsing and `body-parser` behavior (express 4.18+ changed JSON error handling).
  - Transfer-Encoding smuggling regression test (send a chunked + CL request; confirm ALB/app returns 400).
- **Gate:** Staging ECS deploy, 15 min soak, 100% API smoke pass → promote to production via rolling ECS deploy.

### Batch B — P1 Standard Deploy
- **pg 8.7.1 → 8.11.4 (minor):**
  - Full DB integration test suite (parameterized queries + array parameters — the exact attack surface).
  - Replay a sample of production read replicas' slow-query log against staging to catch plan regressions.
  - Verify `order` and `user` model CRUD + payment-record insert/select.
- **axios 0.21.1 → 1.6.8 (MAJOR):**
  - Update adapter/imports for any breaking changes (TypeScript types, default `withCredentials`, `axios` Promise-interceptor signatures).
  - Test external-api flows: payment provider webhook callbacks, address verification, shipping API.
  - Redirect-following behavior must be explicitly re-tested with a malicious redirect target in staging to confirm SSRF is mitigated.
  - Validate that timeout/retry middleware works with new API.
- **Gate:** Full CI + 30 min staging soak; deploy during the April 14 window with on-call SRE on deck.

### Batch C — P2 Runtime Hardening
- **lodash 4.17.20 → 4.17.25 (patch):** Low risk — run unit tests for `config.js` and `data-transform.js`.
- **helmet 4.6.0 → 7.1.0 (MAJOR):**
  - helmet v7 splits some default headers and changes CSP nonce API.
  - In staging, use browser devtools to verify CSP, HSTS, X-Frame-Options, and Referrer-Policy headers are still set correctly on HTML-serving routes.
  - Confirm API routes (which don't set HTML CSP) are unaffected.
- **Gate:** Standard CI; visual check of security headers in staging.

### Batch D — P2 Build-Tooling
- **minimatch 3.0.4 → 3.1.2 (minor):** Re-run build scripts, test glob patterns used in bundling/lint.
- **semver 5.7.1 → 7.5.4 (MAJOR):**
  - semver v7 changes some range-parsing edge cases. Validate CI version-tagging, release scripts, and any runtime code that parses semver ranges (lockfile/version gating).
- **Gate:** Green CI on a clean install; no production deploy risk (devDependencies only).

### Batch E — P3 Backlog
- **qs / debug:** After Batch A lands, run `npm ls qs` and `npm ls debug` to see whether transitive resolution already brings in a fixed version. If not, add explicit `resolutions` (or `overrides`) in `package.json` at team convenience within the 90-day window.
- Configuration audit (zero-cost mitigation, done immediately): verify Fargate task definitions do not set `DEBUG=*` in production.

---

## Deployment Timeline

> Today is Friday, April 11. Sprint ends April 18. Deploy windows are Monday April 14 and Thursday April 17; P0s deploy out-of-band within 24h.

| Date | Window | Batch | Action | Owner | Rollback Plan |
|------|--------|-------|--------|-------|---------------|
| **Fri Apr 11 (today)** | Immediate — business hours | Prep | Cut `hotfix/p0-auth-express` branch; bump `express` → 4.19.2, `jsonwebtoken` → 9.0.1; open draft PR. Run targeted tests locally/CI. | Security + Backend Lead | N/A (pre-deploy) |
| **Sat Apr 12** | 10:00–12:00 ET (low-traffic) | **A — P0 Hotfix** | Deploy Batch A to staging, 15-min soak, rolling ECS prod deploy. On-call SRE + security engineer on bridge. Run `npm ls qs` post-deploy to confirm CVE-2026-37188 is transitively fixed. | DevOps + Security | ECS rollback to previous task definition (sub-5 min). |
| **Mon Apr 14** | Standard deploy window (10:00 ET) | **B — P1 Standard** | Merge `pg` + `axios` upgrade PR. Full CI → staging 30-min soak → production rolling deploy. Smoke-test payment flows end-to-end post-deploy. | Backend engineers | ECS rollback + RDS query plan rollback script (prepared but expected unnecessary). |
| **Mon Apr 14** | Post-deploy | Config audit | Confirm Fargate task defs do not have `DEBUG` enabled (mitigates CVE-2026-36500 in advance). File Jira ticket for Batch E explicit overrides. | DevOps | N/A |
| **Tue Apr 15 – Wed Apr 16** | Sprint work | Prep | Open PRs for Batch C (`lodash`, `helmet`) and Batch D (`minimatch`, `semver`). Address helmet v7 config changes and semver v7 release-script updates in code review. | Backend engineers | N/A (pre-deploy) |
| **Thu Apr 17** | Standard deploy window (10:00 ET) | **C + D — P2 Batches** | Deploy Batches C and D together (single PR train to minimize deploy overhead). Staging soak 20 min → production rolling deploy. Verify security headers, CI green. | Backend + DevOps | ECS rollback; for build-tooling revert via git revert and re-run CI. |
| **Fri Apr 18** | Sprint close | Verification | Run a re-scan with AcmeSec against production; update SOC 2 evidence pack with remediation timestamps. Confirm 8/10 CVEs closed, 2 P3s tracked in backlog. | Security | N/A |
| **By Jul 10 (90-day SLA)** | Backlog | **E — P3 Backlog** | Explicitly bump/override `qs` and `debug` if transitive resolution has not already fixed them; close out Jira. | Backend | Standard sprint rollback |

---

## Major-Version Risk Register

The following upgrades cross a major version boundary and carry the highest regression risk. Mitigations are called out so reviewers know what to focus on:

| Package | Change | Risk Areas | Mitigation / Reviewer Checklist |
|---------|--------|------------|---------------------------------|
| **jsonwebtoken** | 8.5.1 → 9.0.1 | Algorithm confusion was *the* bug class; v9 requires explicit `algorithms` in `verify()`. Existing RS256 tokens must still verify. Callback/Promise API changes. | Audit every `jwt.verify` call for explicit `algorithms: ['RS256']`; regression-test token minting + refresh; confirm legacy sessions not invalidated. |
| **axios** | 0.21.1 → 1.6.8 | Defaults changed for `withCredentials`, CSRF/XSRF header names, adapter resolution, max redirects, and error shape. Webhook signature verification may depend on response buffering. | Grep for `axios.create` defaults; replay recorded production webhook payloads in staging; test redirect-following with a controlled 30x to an internal IP (should fail closed). |
| **helmet** | 4.6.0 → 7.1.0 | `helmet.contentSecurityPolicy()` defaults changed; some middleware was split into separate packages; nonce-based CSP directive ordering changed. | Compare response headers against pre-upgrade baseline; verify CSP nonce rotation per request; ensure HSTS `max-age` preserved. |
| **semver** | 5.7.1 → 7.5.4 | Build-tooling only; semver v7 drops loose-mode parsing by default and changes some prerelease comparisons. | Re-run release/tagging scripts on a throwaway git tag; confirm CI correctly resolves dependency ranges in lockfile. |
| **debug** | 2.6.9 → 4.3.5 | Transitive; v3+ changed namespace format and env var handling. Only matters if DEBUG is enabled (which it is not). | Not actively bumped in Batch A–D; confirm production env is clean (already a P3 control). |

Minor/patch bumps (`express`, `pg`, `lodash`, `minimatch`, `qs`) are low-risk per upstream changelogs and standard SemVer guarantees; regression coverage through existing tests is sufficient.

---

## Communications & Sign-Off

- **CISO / VP Eng:** Notify of P0 hotfix on April 11; share executive summary.
- **SOC 2 audit lead:** Provide evidence (PR links, scan reports) showing all PCI-in-scope CVEs remediated by April 14 — ahead of audit in 6 weeks.
- **Customer Support:** No customer-facing impact expected (rolling ECS deploys, zero-downtime); inform only if P0 hotfix causes a brief auth blip (unlikely).
- **On-call:** Rotate an extra on-call during the April 12 P0 deploy and the April 14 P1 deploy.

## Success Criteria

1. All P0 CVEs closed in production by **April 12 EOD** (≤24h SLA met).
2. All P1 / PCI-in-scope CVEs closed in production by **April 14** (≤7-day SLA met, 4 days early).
3. 8 of 10 CVEs closed by end of sprint (**April 18**); remaining 2 P3s tracked with July 10 due date.
4. Post-remediation AcmeSec scan shows zero Critical/High findings; SOC 2 evidence pack updated.
