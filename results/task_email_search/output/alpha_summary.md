# Project Alpha — Comprehensive Summary

*Compiled from email correspondence dated Jan 15 – Feb 25, 2026*

---

## 1. Project Overview

**What it is:** Project Alpha is a new **customer-facing analytics dashboard** that will replace the company's legacy reporting system. It will deliver real-time metrics, customizable dashboards, alerting, and exportable reports (PDF/CSV) to enterprise clients.

**Technology Stack:**
- **Storage/Database:** PostgreSQL + **TimescaleDB** for time-series metrics; **Redis** for caching aggregations; **S3** for the raw data lake.
- **Data Pipeline:** **Apache Kafka** (6-broker cluster) for real-time event streaming (~48–50K events/sec peak); **Apache Airflow** DAGs for batch/historical ingestion; **Apache Flink** for stream processing (<200 ms end-to-end latency); **dbt** for batch transformations and data modeling; **Great Expectations** for data quality checks.
- **API Layer:** **FastAPI** (Python) with REST endpoints plus a **WebSocket** gateway (`/ws/v1/live-metrics`) for real-time streaming; OAuth2 bearer-token auth integrated with existing SSO; cursor-based pagination; per-tenant/per-user rate limiting.
- **Frontend:** **React** with **Recharts** for visualizations; a Storybook-based design system; drag-and-drop dashboard layout engine.
- **Infrastructure:** Cloud-hosted, with spot instances used for Flink processing nodes to control cost.

**Budget:**
- Originally approved at **$340K total** (Infrastructure $85K; Engineering $220K for 6 FTEs over 4 months; QA/Testing $35K).
- After infrastructure re-estimation (higher data volume of ~2TB/day + Kafka cluster upsized from 3 to 6 brokers), projected spend rose to ~$432K (27% overrun).
- CFO Linda Zhao approved an **expanded budget of $410K** following cost-benefit review; the team offset ~$22K by using spot instances for Flink nodes.
- Phase 1 actual infra spend came in at **$78K vs. $85K budgeted**, putting the project on track against the revised $410K envelope.

---

## 2. Timeline

**Original plan (kickoff, Jan 15):**
| Phase | Dates |
|---|---|
| Phase 1 — Data Pipeline | Jan 20 – Feb 14 |
| Phase 2 — API Layer | Feb 17 – Mar 14 |
| Phase 3 — Frontend | Mar 17 – Apr 18 |
| Beta Launch | Apr 21 |
| GA Release | May 12 |

**Changes:** On Feb 18, Project Lead Sarah Chen announced a slip driven by (a) ~1.5 weeks of additional work to address the security team's critical findings, and (b) ~2 weeks to build a dedicated WebSocket gateway for scalable real-time streaming. Some work was parallelized, netting a **~2.5-week Phase 2 extension** and a **15-day beta slip**.

**Current expected dates (as of Feb 18):**
| Phase | Dates |
|---|---|
| Phase 1 — Data Pipeline | Jan 20 – Feb 12 (completed **on time**) |
| Phase 2 — API Layer | Feb 17 – **Apr 1** (was Mar 14) |
| Phase 3 — Frontend | **Apr 2 – May 3** (was Mar 17 – Apr 18); frontend work started early, in parallel |
| Beta Launch | **May 6** (was Apr 21) |
| GA Release | **May 27** (was May 12) |

**Mitigations applied to contain the slip:**
1. Added a **7th engineer (Alex Wong)** to accelerate security fixes.
2. Moved the compliance export feature (PDF/CSV) from Phase 2 to Phase 3.
3. Kicked off frontend component development in parallel during late Phase 2.
4. Confirmed with Sales that no beta-waitlist clients have hard deadlines before May 6.

---

## 3. Key Risks and Issues

**Budget concerns**
- Initial infrastructure estimates missed the true data volume (~2 TB/day) and under-provisioned Kafka (3 brokers vs. 6 needed for peak load). This added ~$23K/month (~$92K over 4 months) and triggered a CFO-level budget review before the expanded $410K was approved.
- CFO requested a cost-benefit/ROI analysis, sales confirmation of ARR projections, and exploration of cost optimizations (spot instances, tiered data retention) before approving.
- Risk remains that ongoing cloud spend (especially Kafka + storage) could drift if data volume grows beyond current projections.

**Security findings (Feb 10 mandatory review)**
- *Critical (must fix before launch):*
  1. Cross-tenant data exposure on `/api/v1/metrics/{metric_id}/timeseries` — tenant isolation must be enforced at the query layer, not only the API layer.
  2. WebSocket connections require per-message authentication (not just connection-time) because long-lived connections are an attack vector.
- *High (should fix before launch):* per-user rate limiting (in addition to per-tenant), SSRF hardening on the report-generation endpoint, and audit logging for dashboard/alert changes.
- *Medium (within 30 days post-launch):* request signing to prevent replay attacks, strict CORS allowlists.
- A follow-up security review must be scheduled at least 2 weeks before beta.

**Technical challenges**
- **WebSocket scalability:** Existing infrastructure lacks sticky sessions for WebSocket; team decided to build a separate WebSocket gateway service (option b), adding ~2 weeks to Phase 2.
- **Kafka reliability:** A 4-hour outage on Feb 8 caused by a rebalancing storm during a broker restart; mitigated with graceful shutdown procedures and additional monitoring alerts.
- **Frontend blockers:** Real-time streaming UI and custom metric builder are blocked pending the WebSocket gateway and the metric-definition API (Marcus ETA Mar 10).
- **Feature scope pressure:** Client wish-list (anomaly detection, multi-region/GDPR residency, white-labeling, Snowflake integration, SOC 2 Type II) could further stress timeline and budget if pulled into v1.

---

## 4. Client / Business Impact

**Sales pipeline (from Jessica Torres, Head of Sales — Feb 14):**
Demos with 5 enterprise beta-waitlist prospects yielded very positive feedback:

| Client | Potential ARR | Key Signals / Asks |
|---|---|---|
| Acme Corp | **$500K** | Loves real-time dashboards; wants custom KPI alerting; Okta SSO integration |
| Summit Financial | **$420K** | Wants anomaly detection (not yet built); requires SOC 2 Type II before signing; wants white-labeling |
| GlobalTech | **$350K** | Impressed by pipeline latency; needs PDF/CSV compliance export; wants API access |
| DataFlow Inc | **$300K** | Snowflake integration; custom metrics via API; budget tied to Q2 board approval |
| Nexus Industries | **$280K** | Loves UI; concerned about multi-region/GDPR data residency; wants streaming SLAs |

- **Pipeline from these 5 alone: $1.85M ARR.**
- **Total tracked pipeline: ~$2.8M ARR**, which is **ahead of the original $2.1M projection**.
- Top cross-client feature requests: (1) custom alerting / anomaly detection, (2) PDF/CSV compliance export, (3) API access, (4) multi-region/data residency, (5) white-labeling.

**Client feedback highlights:**
- Strong positive reaction to real-time latency numbers and dashboard UI.
- Compliance and trust items (GDPR residency, SOC 2 Type II, audit-grade exports) are recurring gating items for larger deals.
- White-labeling appears to be low-cost (~3 days of work thanks to existing design-system theme tokens) and is recommended for early prioritization.

**Client impact of the slip:** Sales confirmed none of the beta-waitlist clients have hard deadlines before the revised May 6 beta date, so business impact of the 15-day delay is assessed as minimal.

---

## 5. Current Status (as of most recent update, Feb 25)

**Phase 1 — Data Pipeline: ✅ Complete (on schedule, Feb 12).**
- 6-broker Kafka cluster live, processing ~48K events/sec average.
- Flink streaming delivering <200 ms end-to-end latency.
- TimescaleDB populated with 3 weeks of historical data; dbt models running hourly; Great Expectations passing at 99.7%.
- Feb 8 Kafka rebalancing outage documented and mitigated.
- Pipeline ops handed off from Raj to the SRE team.
- Phase 1 actual infra spend: $78K (under the $85K budget).

**Phase 2 — API Layer: 🟡 In progress (started Feb 17; targets Apr 1).**
- Initial API design circulated (dashboards CRUD, timeseries queries, report generation, alerts, live-metrics WebSocket).
- WebSocket gateway service under development (chosen over Redis pub/sub and SSE for scalability).
- Security critical/high items (cross-tenant isolation at query layer, per-message WebSocket auth, per-user rate limits, SSRF hardening on report endpoint, audit logging) being addressed with Alex Wong added to the team.
- Metric definition API targeted for **Mar 10**.

**Phase 3 — Frontend: 🟢 Started early, in parallel.**
- **Completed:** Storybook design system & component library; drag-and-drop dashboard layout engine; Recharts chart components (line/bar/area/pie); SSO authentication flow; responsive tablet/desktop layouts.
- **In progress:** Real-time data streaming UI, report export interface (PDF preview + CSV download), alert configuration wizard.
- **Blocked:** Live metric widgets awaiting WebSocket gateway; custom metric builder awaiting metric definition API (Mar 10).
- Staging demo environment live at https://alpha-staging.mycompany.com.
- Frontend Lead Emily Nakamura estimates **white-labeling is only ~3 days of work** thanks to existing theme tokens, and has flagged it for prioritization.

**Budget posture:** Revised $410K budget approved; Phase 1 came in under budget; spot-instance savings of ~$22K realized. Ongoing monitoring of cloud spend vs. 2 TB/day data volume.

**Bottom line:** The core data foundation is solid and on budget, the API/security work is the critical path driving a 15-day beta slip to May 6, the frontend is progressing ahead of schedule in parallel, and the sales pipeline is outperforming projections ($2.8M ARR tracked vs. $2.1M target) — provided compliance/security asks are met before launch.
