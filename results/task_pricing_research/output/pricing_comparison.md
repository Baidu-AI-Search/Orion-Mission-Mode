# Managed PostgreSQL Pricing Comparison

**Pricing reference date:** July 2026 · All prices USD · All estimates based on **on-demand / pay-as-you-go** rates, **us-east-1 / US East** region, no reserved-instance/commitment discounts, 730 hours/month.

> Prices are estimates pulled from public AWS/GCP/Azure/DO/Neon pricing pages and calculators in July 2026. Always re-run your provider's official calculator before making a purchase decision—regional, tax, and discount variations apply.

---

## Summary Table — Monthly Cost (USD)

| Provider | Config A (Small: 2 vCPU / 8 GB RAM / 100 GB / single zone) | Config B (Production: 4 vCPU / 16 GB RAM / 500 GB / HA) | Pricing model | Free tier / trial |
|---|---|---|---|---|
| **AWS RDS for PostgreSQL** (db.m7g.large / db.m7g.xlarge, gp3, us-east-1) | **~$134** | **~$607** | Per-hour instances + per-GB storage + IOPS/throughput | 750 hrs/mo + 20 GB gp2 + 20 GB backup, 12 months (new accounts) |
| **Google Cloud SQL for PostgreSQL** (Enterprise, us-central1) | **~$118** | **~$575** | Per-vCPU-hour + per-GB RAM-hour + per-GB storage | $300 credit for 90 days; no permanent free tier |
| **Azure Database for PostgreSQL** (Flexible Server Ds v6, East US) | **~$141** | **~$577** | Per-vCore-hour + per-GB storage | $200 credit for 30 days; 12 months free services w/ B1ms equivalent not applicable to GP |
| **DigitalOcean Managed Databases** (General Purpose nodes) | **~$129** | **~$547** | Flat per-node monthly + per-GB add-on storage | $200 credit for 60 days with new account; no permanent free tier |
| **Neon (Serverless Postgres)** | **~$190** (Launch, 2 CU always-on); **~$15/mo** typical with autoscale/scale-to-zero | **~$823** (Scale, 4 CU always-on; HA is built-in) | Per-CU-hour (1 CU ≈ 1 vCPU + 4 GB) + per-GB-month storage; autoscale / scale-to-zero | Free plan forever: 0.5 GB storage, 100 CU-hrs/mo per project, no credit card |

### Relative ranking (by sticker price)
- **Config A (small/single-zone):** GCP (~$118) < DO (~$129) < AWS (~$134) < Azure (~$141) < Neon (~$190 always-on; cheaper if idle)
- **Config B (prod/HA):** DO (~$547) < GCP (~$575) ≈ Azure (~$577) < AWS (~$607) < Neon (~$823 always-on)

---

## 1. AWS RDS for PostgreSQL

**SKUs used for estimate (us-east-1, PostgreSQL, gp3 storage):**
- Config A: `db.m7g.large` (2 vCPU, 8 GB) Single-AZ — $0.168/hr × 730 = **$122.64**; gp3 storage $0.115/GB-mo × 100 GB = **$11.50** → **~$134/mo**
- Config B: `db.m7g.xlarge` (4 vCPU, 16 GB) Multi-AZ (one standby) — $0.674/hr × 730 = **$492.02**; Multi-AZ gp3 storage $0.23/GB-mo × 500 GB = **$115** → **~$607/mo**

**What's included**
- Automated backups (PITR) with configurable retention up to 35 days
- Software patching, automated minor-version upgrades, OS maintenance
- CloudWatch metrics (basic at 1-minute granularity); Enhanced Monitoring with agent
- Encryption at rest (KMS) and in transit (TLS)
- Built-in read replicas (charged as additional instances)
- **RDS Proxy** (managed connection pooler) is a separate paid add-on (~$0.018/vCPU-hr in us-east-1) — **not** included in base price.

**Pricing model**
- Per-second billing (10-min minimum) for compute, per-GB-month for storage, per-IOPS-month and per-MiB/s-month for provisioned gp3/io1/io2 IOPS/throughput above the free baseline (gp3 includes 3,000 IOPS and 125 MiB/s free).
- On-Demand, Reserved Instances (1/3-yr, up to ~40–55% off), and Savings Plans available.

**Hidden / add-on costs to watch**
- **Backup storage:** Free up to 100% of provisioned DB storage; extra backups $0.095/GB-mo. Long retention = extra $.
- **IOPS/throughput (gp3):** First 3,000 IOPS / 125 MiB/s free; additional IOPS $0.02/IOPS-mo (single-AZ) or $0.04/IOPS-mo (Multi-AZ); additional throughput $0.08/MiBps-mo / $0.16/MiBps-mo.
- **Multi-AZ storage costs ~2×** single-AZ ($0.23 vs $0.115/GB-mo) because data is synchronously mirrored.
- **Data transfer:** Inbound free; outbound to internet $0.09/GB (first 100 GB/mo free to internet within a region); cross-AZ traffic billed both ways at $0.02/GB; cross-region replication $0.02/GB.
- **Performance Insights** (advanced database load analysis) is free for 7 days, then paid after.
- **RDS Proxy** (connection pooling) billed by vCPU-hour.

**Free tier / trial**
- AWS Free Tier: 750 hrs/mo of Single-AZ `db.t2.micro`/`db.t3.micro`, 20 GB gp2 storage, 20 GB backup — 12 months for new accounts.

---

## 2. Google Cloud SQL for PostgreSQL

**SKUs used (us-central1, Cloud SQL Enterprise, zonal for A, regional/HA for B, SSD):**
- Config A: 2 vCPU × $0.0413/hr + 8 GB × $0.007/GB-hr + 100 GB SSD (zonal PD SSD $0.17/GB-mo) → **~$60.30 + $40.88 + $17.00 = ~$118/mo**
- Config B: HA doubles vCPU+RAM (primary + standby): 4 × 2 vCPU × $0.0413/hr + 16 × 2 GB × $0.007/GB-hr + 500 GB regional PD SSD ($0.34/GB-mo) → **~$241.19 + $163.52 + $170 = ~$575/mo**

**What's included**
- Automated backups + PITR (on-demand + automated, up to 365-day retention)
- Automated patches, maintenance windows, storage auto-resize
- Built-in **connection pooling (PgBouncer-compatible "Connector" with built-in proxy)** at no extra charge
- Cloud Monitoring / Cloud Logging integration
- Encryption at rest (CMEK supported) and in transit; IAM DB auth
- HA with automatic failover via regional persistent disk
- Read replicas charged at same rate as standalone instances

**Pricing model**
- Per-second (1-min minimum) charges for vCPU + RAM. Separate per-GB-month charge for storage (zonal vs. regional SSD vs. Hyperdisk).
- CUDs (Committed Use Discounts) 1/3-yr save up to ~52% on compute (no discount on storage/IP).

**Hidden / add-on costs**
- **HA = 2× compute + regional storage (2× SSD price)** — reflected above.
- **IP address:** Static public IP is charged at ~$0.004/hr (~$3/mo) per instance if not using Private Service Connect / Private IP only.
- **Backups:** Continuous PITR (write-ahead logs) and on-demand backups are stored at snapshot rates — typically around $0.08/GB-month in us-central1 for regional snapshots (effectively 24–50% of SSD price depending on region); retained backups beyond automated retention and snapshots add up.
- **Outbound data transfer:** Inbound free; internet egress $0.12/GB (after 100 GB free/mo premium tier, or discounted for CDN/interconnect); cross-region replica egress $0.08–$0.12/GB.
- **Cloud SQL Enterprise Plus** (for lower downtime, faster perf) costs ~2–3× Enterprise — we quoted Enterprise, which is the default for production.
- **Network:** Private Service Connect / Service Attachment endpoints have minor per-hour charges.

**Free tier / trial**
- **Google Cloud Free Trial:** $300 credit for 90 days for new customers.
- **Always Free:** No permanent free Cloud SQL instance (the old f1-micro SQL free tier was retired).

---

## 3. Azure Database for PostgreSQL — Flexible Server

**SKUs used (East US, General Purpose Dsv6, Premium SSD):**
- Config A: `D2s v6` (2 vCPU / 8 GB) — $129.94/mo; Premium SSD storage $0.115/GB-mo × 100 = $11.50 → **~$141/mo**
- Config B: `D4s v6` (4 vCPU / 16 GB) with Zone-Redundant HA (standby billed at same rate = 2×) — $259.88 × 2 = $519.76; Premium SSD $0.115 × 500 = $57.50 → **~$577/mo**

**What's included**
- Built-in **zone-redundant HA** with synchronous streaming replication (turnkey, no manual replica setup)
- Automated backups with configurable 7–35 day retention + geo-redundant backup option
- **Stop/Start** capability (pay only for storage when stopped) — great for dev/staging
- Auto-scaling storage, automated patching, maintenance windows
- **Microsoft Entra ID (Azure AD)** authentication, TLS 1.2/1.3, TDE with customer-managed keys
- Azure Monitor / Log Analytics basic metrics included; PgBouncer connection-pooler **built in at no extra charge**
- Virtual network injection (private access) free; Private Link available

**Pricing model**
- Per-second billing, per-vCore + per-GB-month storage. Burstable (B-series) is cheaper but not eligible for HA. General Purpose / Memory Optimized are required for HA.
- Reservations (1/3 yr) and Azure Savings Plans for databases offer ~20–60% compute discounts.
- Separate charges for provisioned IOPS on Premium SSD v2 if you need more than the included baseline (3,000 IOPS up to 399 GiB, 12,000 IOPS for ≥400 GiB disks — 500 GiB comfortably hits the 12,000-IOPS baseline for free).

**Hidden / add-on costs**
- **HA = 2× compute** (standby billed at same rate as primary, same SKU, storage only billed once because of compute/storage separation — so HA storage doesn't double, a real saving vs. AWS Multi-AZ storage doubling).
- **Backup storage:** Free up to 100% of provisioned storage; beyond that $0.095/GB-mo (LRS) / $0.19/GB-mo (GRS). PITR logs accumulate with longer retention and heavy write loads.
- **Provisioned IOPS (Premium SSD v2)** or Ultra Disk: $0.02/IOPS-mo over included baseline, $0.08/MB/s-mo over throughput baseline.
- **Data transfer:** Inbound free; outbound to internet $0.087/GB (first 100 GB free/mo); cross-zone data transfer $0.01/GB; cross-region replica transfer $0.02–$0.09/GB depending on zones.
- **Extended Support** (when a Postgres major version goes EOL): $0.07/vCore-hour — plan to upgrade!
- **Long-term backup retention** via Azure Backup is extra.

**Free tier / trial**
- **Azure free account:** $200 credit for 30 days; 12 months of free services but that applies primarily to VMs/storage — PostgreSQL Flexible Server itself is pay-as-you-go outside of the credit.
- "B1ms" Burstable 1 vCore / 2 GB can run almost free inside a dev subscription under the monthly free services combination for 12 months but doesn't match our Config A/B specs.

---

## 4. DigitalOcean Managed Databases (PostgreSQL)

**SKUs used (General Purpose plan — closest vCPU:RAM ratio to the spec; node pricing from do.com/pricing/managed-databases):**
- Config A: 8 GB / 2 vCPU General Purpose node — $113.45/mo (includes 30 GB SSD); additional storage $0.215/GiB-mo → 70 GB extra = $15.05 → **~$129/mo** (standby/replica not enabled, single-node)
- Config B: 16 GB / 4 vCPU General Purpose node × 2 (primary + standby for HA) — $227.05 × 2 = $454.10; cluster storage beyond the 70 GB included on primary — 430 GB extra @ $0.215 = $92.45 → **~$547/mo**

**What's included**
- Daily backups + PITR (up to 30 days on paid plans)
- End-to-end TLS, firewall/IP allowlists, VPC peering
- Automated failover for HA clusters (standby nodes + optional read replicas)
- Built-in **pgBouncer connection pooling**, read replicas at extra node price
- Maintenance windows, auto-upgrades, one-click major version upgrades
- Built-in monitoring with basic graphs + integrated alerting
- Encryption at rest (LUKS) and in transit
- Free daily snapshots / automatic backups retained up to 30 days
- Generous outbound transfer included with nodes (5 TiB/mo aggregate at these sizes)

**Pricing model**
- Flat **monthly/holly** per-node pricing + per-GB add-on disk. Simpler than hyperscalers. Droplet-style pricing (no per-IOPS or per-MB/s charges — storage is "overprovisioned" to absorb typical PostgreSQL workloads).
- No reservations/Savings Plans, but simple and predictable.

**Hidden / add-on costs**
- **Standby nodes and read replicas are billed as full additional nodes** — transparent, but HA ~doubles compute cost (visible above).
- **Disk is comparatively expensive** per GB vs AWS/Azure ($0.215/GB-mo vs ~$0.115/GB-mo) — at small sizes this is negligible; at multi-TB sizes it matters.
- **Backups:** Free automated backups (full + PITR) are included up to plan limits, but longer retention or many manual snapshots may consume additional block-storage at regular rates.
- **Data transfer:** 5 TiB/mo outbound transfer included on these plans (very generous vs hyperscalers); overage $0.01/GB. Cross-region traffic is charged at standard transfer rates.
- **VPC peering / Private networking** free; **spaces/object storage** (for dumps) separate.
- **Insights/advanced monitoring** is basic compared to CloudWatch/Azure Monitor; users often add an external APM.
- No committed-use discounts; list price is what you pay.

**Free tier / trial**
- **60-day free trial** with $200 credit for new sign-ups.
- No permanently free managed Postgres (the $4/mo $7/mo plans are for the smallest Basic nodes, but these aren't free).

---

## 5. Neon (Serverless Postgres)

Neon is architecturally different: **storage and compute are separated**, compute autoscales and can scale to zero when idle, and **storage is multi-AZ by default with no standby required for HA**.

**CU definition:** 1 CU ≈ 1 vCPU + 4 GB RAM. So Config A ≈ 2 CU; Config B ≈ 4 CU.

**SKUs used (always-on 24/7, scale-to-zero disabled for a stable production-like profile):**
- Config A (Launch plan, 2 CU always-on): 2 × $0.106/CU-hr × 730 = $154.76; 100 GB storage × $0.35/GB-mo = $35 → **~$190/mo**
  - *Neon suggests typical "intermittent load, 1 GB" Launch usage is* ~$15/mo — attainable only if you let compute scale to zero and have small storage.
- Config B (Scale plan, 4 CU always-on — required for SLA, HIPAA, private networking): 4 × $0.222/CU-hr × 730 = $648.24; 500 GB × $0.35/GB-mo = $175 → **~$823/mo**

**What's included**
- **Built-in multi-AZ HA for storage** — no extra compute needed for failover (copies are in object storage across AZs; compute can be restarted on any node).
- **Autoscaling** (configurable min/max CU) and **scale-to-zero** after 5 minutes of inactivity (can be disabled for consistent latency)
- **Instant point-in-time restore** ("time-travel" branching), database branches for dev/preview/staging
- **Built-in connection pooling (pgBouncer-compatible)**, up to 10,000 pooled connections
- **Read replicas** at no extra storage cost (storage is shared), only compute hours
- Built-in monitoring dashboards (1-day retention on Free, 3-day on Launch, 14-day on Scale)
- Extensions: pgvector, PostGIS, TimescaleDB, etc.
- Project branching (copy-on-write) — near-zero-cost preview branches

**Pricing model**
- Fully **usage-based (serverless)**:
  - **Compute (CU-hours):** $0.106/CU-hr on Launch; $0.222/CU-hr on Scale
  - **Storage:** $0.35/GB-month (data); $0.20/GB-month history (WAL/change history for time-travel, configurable retention up to 30 days)
  - **Branches (child):** $0.002/branch-hour on Launch/Scale (first 10 included)
  - **Snapshots / scheduled backups:** $0.09/GB-month
  - **Egress:** 500 GB included per month on Launch/Scale; $0.10/GB thereafter (Scale adds $0.01/GB for private network)
  - Autoscaling: set min/max CU; Neon scales within those bounds based on load.

**Hidden / add-on costs**
- **Always-on "fixed" deployments are expensive** compared to provisioned RDS/Cloud SQL — Neon's Launch plan 2-CU-always-on is ~$190/mo vs ~$118–$141 on the Big 3. The serverless economics only shine when compute can scale down (dev/staging, preview branches, spiky traffic, serverless apps).
- **Time-travel / history storage** ($0.20/GB-mo) grows with write volume and retention window (up to 30 days). Heavy-write DBs can double storage cost.
- **Scale plan is 2× the CU price of Launch** but is required for SLAs (99.95%), HIPAA, private networking, longer metrics retention, support.
- **Branch-hour costs** can accumulate if you auto-create many preview environments (e.g., one per PR).
- **No static IP / VPC peering** on Launch — only public + IP allow rules; private networking is Scale-tier-only.
- **Cold-start latency** when scaling from zero (~hundreds of ms to seconds); not ideal for latency-sensitive always-on workloads unless scale-to-zero is disabled (at which point you lose the cost advantage).
- **No maximum 24/7 "provisioned" discount** — you pay per CU-hour continuously if you pin a minimum size.

**Free tier / trial**
- **Free plan forever:** 0.5 GB storage per project, 100 CU-hours per project per month, up to 2 CU / 8 GB RAM, scale-to-zero after 5 min, no credit card required. Excellent for prototyping.
- **Startup Program** offers up to $100k in credits for VC-funded early-stage startups.

---

## Total Cost of Ownership — Hidden Costs Compared

| Cost vector | AWS RDS | GCP Cloud SQL | Azure PostgreSQL | DigitalOcean | Neon |
|---|---|---|---|---|---|
| Compute (per-hour vs. flat) | Per-second | Per-second | Per-second | Flat monthly | Per-CU-hour serverless |
| Doubling of compute for HA? | Yes (Multi-AZ ~2×) | Yes (regional ~2×) | Yes (standby ~2×) | Yes (extra node) | **No** (HA built into storage layer) |
| Storage for HA doubles? | **Yes** (~$0.23 vs $0.115) | **Yes** (regional PD $0.34 vs zonal $0.17) | **No** (compute/storage split, storage billed once) | Effectively 2× (standby has own disk) | N/A (shared object storage) |
| Extra IOPS charges? | Yes (over gp3 baseline) | Yes (if using Hyperdisk) | Yes (Premium SSD v2 over baseline) | **No** (overprovisioned) | **No** (no IOPS concept) |
| Backup > 100% storage | $0.095/GB-mo | ~$0.08/GB-mo | $0.095/GB-mo (LRS) | Included (within plan) | $0.09/GB-mo (snapshots) + $0.20/GB-mo history |
| Connection pooling (pgBouncer/RDS Proxy) | RDS Proxy extra | Built-in (free) | Built-in (free) | Built-in (free) | Built-in (free) |
| Public IP | Free | ~$3/mo per static IP | Free | Free | Free (Scale plan only for private) |
| Internet egress | First 100 GB free, then $0.09/GB | First 100 GB free premium, then ~$0.12/GB | First 100 GB free, then ~$0.087/GB | 5 TiB free, $0.01/GB after | 500 GB free (plans), $0.10/GB after |
| Cross-AZ / cross-region traffic | Billed | Billed | Billed | Small fees | Small fees |
| Commitment discounts | RI/Savings Plans up to ~55% | CUD up to ~52% | RI/Savings up to ~60% | None | None |
| Support plans | $$$ Developer/Business/Enterprise tiers | Paid support tiers | Paid support plans | Starts at ~$100+/mo for business-level; community free | Community (Free/Launch), billing (Scale), paid enterprise |

### Real-world TCO multipliers (rule of thumb)
- **AWS/GCP/Azure** — sticker price + 5–15% for extra backups/IOPS/IP + egress, plus support plan if enterprise SLA required. Budget 10–20% above sticker for steady-state production.
- **DigitalOcean** — sticker is largely all-in; only meaningful add-ons are extra nodes/standbys and extra storage. Very predictable.
- **Neon** — sticker only represents "always-on worst case"; with autoscale + scale-to-zero you can spend **5–30%** of sticker for spiky/low-utilization workloads, but 24/7 pinned workloads cost ~30–40% more than provisioned alternatives.

---

## Recommendation

There's no one-size-fits-all answer — but here's the decision framework:

1. **Best overall balance for steady-state production (Config B): DigitalOcean Managed Databases (~$547/mo)**
   - Most predictable pricing, all-in (connection pooling, generous transfer, PITR included), simplest ops.
   - Choose this if you don't need the hyperscaler ecosystem (Lambda/GKE/Azure Functions integration, global backbone, enterprise IAM), your team wants low operational overhead, and the workload fits on General Purpose nodes (up to 40 vCPU / 160 GB RAM — covers most mid-sized production Postgres).
   - Caveat: smaller global footprint, fewer advanced features (no Aurora/AlloyDB-like read scaling, no logical replication to native cloud services).

2. **Best if you're already committed to a hyperscaler:**
   - **AWS house → RDS for PostgreSQL.** Massive ecosystem (Aurora migration path, DMS, IAM, Lambda triggers, Glue/Athena federation). Use gp3 with free baseline IOPS and a 1-year Reserved Instance for ~30–40% savings. Plan ~$600–700/mo all-in for Config B.
   - **GCP house → Cloud SQL for PostgreSQL.** Lowest sticker at small sizes, built-in PgBouncer, seamless with GKE/Cloud Run/Cloud Functions. Enterprise Plus if you need <30s failover. Plan ~$575–650/mo all-in.
   - **Azure house → Azure Database for PostgreSQL Flexible Server.** Unique advantage: **HA doesn't double storage costs**, PgBouncer built in, Stop/Start, strong Entra ID / Private Link integration, and arguably the most aggressive Savings Plan discounts. Plan ~$575–650/mo; lowest TCO among the Big 3 for HA workloads with large storage because storage isn't doubled.

3. **Best for spiky/dev/staging/serverless-workloads (NOT steady 24/7 production at 4 vCPU): Neon**
   - Excellent when you want **scale-to-zero, database branching (preview DBs per PR), instant restore, built-in HA without paying for a standby**, and Postgres-compatible serverless.
   - For a **truly intermittent workload** (e.g., internal tool, side project, preview environments, dev databases) Neon's Launch plan comes in at **~$15/mo typical** — dramatically cheaper than any provisioned option.
   - For a steady 24/7 4-vCPU production database, however, Neon's Scale plan (~$823/mo) is the most expensive option here, with less mature operational tooling. **Avoid Neon for pinned, always-on production workloads** where you can't tolerate cold starts and don't need serverless elasticity.

### Quick decision matrix

| Your situation | Pick |
|---|---|
| Startup / small team, predictable traffic, want cheapest + simplest all-in | **DigitalOcean** |
| Already on AWS / need Aurora path / mature tooling | **AWS RDS** |
| Already on GCP / want built-in pooling & tight GKE/Run integration | **Google Cloud SQL** |
| Already on Azure / want best HA storage economics / Entra ID | **Azure PostgreSQL Flexible Server** |
| Spiky traffic, preview branches, scale-to-zero dev/staging, Jamstack/serverless apps | **Neon** |
| Prototype / side project / hobby with no budget | **Neon Free tier** (forever) or AWS/GCP/Azure free trials |

---

*Prepared July 2026. Prices reflect public list pricing in us-east-1 / East US / us-central1 regions for on-demand, pay-as-you-go, single-tenant managed PostgreSQL. Actual costs will vary with region, discounts (RIs/CUDs/Savings Plans), taxes, support levels, backup retention, IOPS tuning, and actual data transfer volumes.*
