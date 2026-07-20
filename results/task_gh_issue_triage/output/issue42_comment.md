## Triage Analysis & Recommended Next Steps

**Severity:** P0 — Critical Production Outage  
**Triage performed by:** Agent reviewer  
**Status:** Root cause identified, fix PR #41 is approved and ready to merge

---

### Root Cause Analysis

The connection pool exhaustion is confirmed to have been introduced by PR #38 (async/await refactor of `src/db/pool.ts`, commit shipped in v2.4.1). The refactor inadvertently removed the `conn.release()` call from the `finally` block along the async code path, while the synchronous path remained correct. This means every async query leaks exactly one DB connection, causing the pool (size 50) to saturate over roughly 20–30 minutes at current traffic levels (~15% error rate during peak).

### Immediate Actions (within the next hour)

1. **Approve and merge PR #41 immediately.** The fix has already been:
   - Load-tested at 300 RPS for 1 hour in staging with no exhaustion observed
   - Approved by @alice-dev (lead)
   - Verified to add monitoring metrics that will help catch similar regressions
2. **Deploy to production** following the standard emergency rollout procedure. Skip the usual 24-hour staging soak given the severity and the narrow nature of the fix.
3. **Immediately after deploy**, verify on dashboards:
   - Active DB connections return to baseline (<10 idle, <20 active)
   - 503 error rate drops to 0
   - p50/p95 latency returns to pre-v2.4.1 levels
4. **Communicate** in the #incident channel and post a 2-sentence status page update acknowledging the intermittent errors and confirming resolution.

### Validation Steps

- [ ] PR #41 merged and CI green
- [ ] Production deploy complete
- [ ] Pool metrics on `/health` show stable active/idle counts for 30+ minutes post-deploy
- [ ] Error rate SLO back to 99.9%+
- [ ] Post-incident review scheduled within 48 hours

### Why Not Roll Back?

Rolling back v2.4.1 is not preferred because PR #41 is a one-line logic restoration (`finally { conn.release() }`) plus non-breaking monitoring additions. A rollback would revert other unrelated fixes in v2.4.1 and take roughly the same amount of time as deploying the targeted fix. However, if PR #41 cannot be deployed within 30 minutes of this comment, **proceed with v2.4.0 rollback** as a safety net.

### Follow-up Work (to file as new issues/PRs)

1. **Prevent recurrence — add a regression test in CI** that runs 1,000+ concurrent queries and asserts the pool returns to baseline. PR #41 already includes this test; ensure it runs as a required check.
2. **Connection pool alerts:** configure PagerDuty alerts when pool utilization exceeds 70% for >5 minutes, so future leaks are caught before user impact.
3. **Code review checklist:** add "resource cleanup (connections, streams, file handles) verified in all code paths, including error branches" to the PR template.
4. **PR #38 follow-up:** keep PR #38 in `changes_requested` state until #41 is merged; after deploy, review whether the remaining error-path changes in #38 have similar cleanup gaps before merging additional refactoring.
5. **Post-mortem:** document timeline (detection → merge → deploy → recovery) and identify why load testing in staging before v2.4.1 didn't catch the leak (apparently load test was skipped per PR #38 checklist).

### Linking

- Fix PR: #41
- Regressed by: #38
- Related future work: pool monitoring alerting (to file)
