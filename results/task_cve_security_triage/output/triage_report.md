# Vulnerability Triage Report — `acme-webapp`

**Scanner:** AcmeSec Vulnerability Scanner v3.2.1
**Scan Date:** 2026-04-10
**Triage Date:** 2026-04-11
**Reviewer:** Security Engineering
**Scope:** 10 CVEs across runtime, build tooling, and transitive dependencies

## Priority Scale (per Change Management Policy)

| Tier | SLA | Trigger |
|------|-----|---------|
| **P0 — Critical** | Deploy within 24 hours (hotfix permitted) | Active exploit in the wild, unauthenticated RCE/auth bypass on runtime path, or direct impact to payment scope |
| **P1 — High** | Deploy within 7 days | High CVSS runtime issue with proven exploit path; affects authenticated users or can expose sensitive data |
| **P2 — Medium** | Deploy within 30 days (batch with sprint release) | Medium CVSS, mitigated by existing controls, build-tooling only, or requires non-default configuration |
| **P3 — Low** | Deploy within 90 days (team discretion) | Low CVSS, requires unusual preconditions, or informational/defense-in-depth |

**PCI DSS Scope Note:** CVEs are flagged "In Scope" when they affect code paths that can reach `/api/v1/payments/*` (the cardholder-data environment boundary), authentication/authorization middleware, or the database layer that stores payment records. CVEs in build tooling, transitive dependencies not reachable from payment code paths, or requiring non-default production configuration are considered Out of Scope but still remediated per policy.

---

## Triage Findings (Ordered Most Critical → Least Critical)

### 1. CVE-2026-31845 — `jsonwebtoken` (JWT signature bypass)
- **CVSS:** 9.1 (CRITICAL) — `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N`
- **Installed → Fixed:** 8.5.1 → 9.0.1 (major version bump)
- **Priority: P0**
- **Justification:** Actively exploited in the wild and enables unauthenticated forgery of JWTs used for RS256-signed authentication. Token forgery directly compromises auth on every endpoint — including `/api/v1/payments/*` — and cannot be mitigated by WAF rules; hotfix required within 24 hours.
- **PCI DSS Scope:** **In Scope** (authentication bypass on payment endpoints; violates Req. 6/8 access control).

### 2. CVE-2026-29112 — `express` (RCE via Transfer-Encoding headers)
- **CVSS:** 9.8 (CRITICAL) — `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`
- **Installed → Fixed:** 4.17.1 → 4.19.2 (minor version bump)
- **Priority: P0**
- **Justification:** Unauthenticated remote code execution on the core HTTP server with a public PoC available. While the ALB/WAF provides some edge filtering, Transfer-Encoding smuggling is processed at the Node.js layer and reaches `src/server/api.js` and `src/server/middleware.js` (which front payment routes); must ship on the same hotfix window as CVE-2026-31845.
- **PCI DSS Scope:** **In Scope** (HTTP front door for payment endpoints; RCE would compromise cardholder data environment).

### 3. CVE-2026-27519 — `pg` (SQL injection via array parameter bypass)
- **CVSS:** 8.1 (HIGH) — `AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:N`
- **Installed → Fixed:** 8.7.1 → 8.11.4 (minor version bump)
- **Priority: P1**
- **Justification:** Authenticated SQL injection against the PostgreSQL driver used by `user` and `order` models (which back payment records). Parameterized queries normally protect us, but crafted array parameters bypass escaping — public PoC exists; ships within the 7-day window.
- **PCI DSS Scope:** **In Scope** (queries `order` model containing payment records; violates Req. 6.5.1 SQL injection controls).

### 4. CVE-2026-30291 — `axios` (SSRF via redirect URL parsing)
- **CVSS:** 7.4 (HIGH) — `AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:N`
- **Installed → Fixed:** 0.21.1 → 1.6.8 (major version bump)
- **Priority: P1**
- **Justification:** SSRF in `external-api.js` and webhook handler; the deployment doc explicitly notes WAF "does not block all SSRF vectors," and an attacker who can control a redirect target can pivot to internal RDS/ECS metadata. Exploit complexity is high (needs user-supplied URL + follow-redirects), but the blast radius on internal services warrants the 7-day SLA.
- **PCI DSS Scope:** **In Scope** (webhook handler is reachable from payment callback flows; SSRF could reach cardholder-data network).

### 5. CVE-2026-28003 — `lodash` (prototype pollution DoS)
- **CVSS:** 7.5 (HIGH) — `AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H`
- **Installed → Fixed:** 4.17.20 → 4.17.25 (patch bump)
- **Priority: P2**
- **Justification:** High CVSS but impact is limited to availability (DoS) via prototype pollution, and exploitation requires passing attacker-controlled JSON through `_.merge`/`_.defaultsDeep` in config/data-transform — not in the payment request path. Patch-level upgrade is low risk and can be batched into the next sprint release.
- **PCI DSS Scope:** **Out of Scope** (availability impact only; no C/I to cardholder data).

### 6. CVE-2026-25877 — `helmet` (CSP nonce bypass)
- **CVSS:** 5.4 (MEDIUM) — `AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:L/A:N`
- **Installed → Fixed:** 4.6.0 → 7.1.0 (major version bump)
- **Priority: P2**
- **Justification:** Medium-severity CSP bypass requiring a vulnerable browser and user interaction (click/navigation). Defense-in-depth for XSS only — does not directly expose data; major version jump of helmet requires validating middleware configuration. Batch into sprint release.
- **PCI DSS Scope:** **Out of Scope** (browser-side defense; payment endpoints are JSON APIs not serving CSP-protected HTML).

### 7. CVE-2026-34201 — `semver` (ReDoS in version parsing)
- **CVSS:** 5.3 (MEDIUM) — `AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L`
- **Installed → Fixed:** 5.7.1 → 7.5.4 (major version bump, build-tooling)
- **Priority: P2**
- **Justification:** Affects build/CI tooling only (not runtime), and exploitation requires an attacker-controlled version string reaching semver parsing. Major version bump is tested in CI without customer impact; ships with sprint release.
- **PCI DSS Scope:** **Out of Scope** (build-only dependency; no runtime exposure to cardholder data).

### 8. CVE-2026-33010 — `minimatch` (ReDoS in glob parsing)
- **CVSS:** 5.3 (MEDIUM) — `AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L`
- **Installed → Fixed:** 3.0.4 → 3.1.2 (minor version bump, build-tooling)
- **Priority: P2**
- **Justification:** Build-only ReDoS with minimal availability impact (CI jobs, not production). Minor version, low risk; batch with other build-tooling fixes in the sprint release.
- **PCI DSS Scope:** **Out of Scope** (build-only dependency).

### 9. CVE-2026-37188 — `qs` (query string nested-object DoS)
- **CVSS:** 3.7 (LOW) — `AV:N/AC:H/PR:N/UI:N/S:U/C:N/I:N/A:L`
- **Installed → Fixed:** 6.5.2 → 6.12.1 (minor, transitive via express)
- **Priority: P3**
- **Justification:** Only exploitable if the `depth` option is explicitly raised above default; our codebase uses express defaults and express 4.19.2 (pulled in by the P0 fix) already bumps qs to a safe version. Defense-in-depth explicit bump can be scheduled at team discretion.
- **PCI DSS Scope:** **Out of Scope** (default configuration blocks exploitation).

### 10. CVE-2026-36500 — `debug` (verbose error disclosure)
- **CVSS:** 3.1 (LOW) — `AV:N/AC:H/PR:N/UI:R/S:U/C:L/I:N/A:N`
- **Installed → Fixed:** 2.6.9 → 4.3.5 (major, transitive)
- **Priority: P3**
- **Justification:** Requires the `DEBUG` environment variable to be enabled in production (it is not per our Fargate task definitions, confirmed via IaC) and requires user interaction. Transitive dependency with negligible real-world risk; schedule per team discretion in 90-day window.
- **PCI DSS Scope:** **Out of Scope** (misconfiguration-gated; DEBUG is disabled in prod).

---

## Summary Table

| # | CVE | Package | CVSS | Priority | PCI In-Scope? | Version Change |
|---|-----|---------|------|----------|---------------|----------------|
| 1 | CVE-2026-31845 | jsonwebtoken | 9.1 | **P0** | ✅ Yes | 8.5.1 → 9.0.1 (major) |
| 2 | CVE-2026-29112 | express | 9.8 | **P0** | ✅ Yes | 4.17.1 → 4.19.2 (minor) |
| 3 | CVE-2026-27519 | pg | 8.1 | **P1** | ✅ Yes | 8.7.1 → 8.11.4 (minor) |
| 4 | CVE-2026-30291 | axios | 7.4 | **P1** | ✅ Yes | 0.21.1 → 1.6.8 (major) |
| 5 | CVE-2026-28003 | lodash | 7.5 | **P2** | ❌ No | 4.17.20 → 4.17.25 (patch) |
| 6 | CVE-2026-25877 | helmet | 5.4 | **P2** | ❌ No | 4.6.0 → 7.1.0 (major) |
| 7 | CVE-2026-34201 | semver | 5.3 | **P2** | ❌ No | 5.7.1 → 7.5.4 (major, build) |
| 8 | CVE-2026-33010 | minimatch | 5.3 | **P2** | ❌ No | 3.0.4 → 3.1.2 (minor, build) |
| 9 | CVE-2026-37188 | qs | 3.7 | **P3** | ❌ No | 6.5.2 → 6.12.1 (minor, transitive) |
| 10 | CVE-2026-36500 | debug | 3.1 | **P3** | ❌ No | 2.6.9 → 4.3.5 (major, transitive) |

## Cross-Cutting Observations

- **4 of 10 CVEs are in PCI DSS scope**, all of which are P0 or P1 — these are the audit-relevant items for the SOC 2 assessment in 6 weeks.
- The P0 hotfix bundle (express + jsonwebtoken) must be expedited **today/tomorrow**, independent of the scheduled April 14/17 deploy windows.
- 3 CVEs involve **major version bumps** (jsonwebtoken, axios, helmet, semver, debug) — these require extra attention in code review and regression testing (detailed in the remediation plan).
- `qs` will likely be remediated implicitly by the express upgrade because express 4.19.2 depends on qs ≥6.11; we will verify via `npm ls qs` after the P0 patch.
- `debug` is gated by production environment configuration; an immediate config audit (confirm `DEBUG` is not set in Fargate task defs) is recommended as a zero-cost mitigation while the upgrade is scheduled.
