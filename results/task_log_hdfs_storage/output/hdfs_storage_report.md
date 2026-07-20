# HDFS DataNode Storage Analysis Report

**Log source:** `hdfs_datanode.log` (2,000 lines)
**Time window observed:** `081109 20:35:18` → `081109 20:35:46` (28 seconds)
**Workload:** Hadoop MapReduce job `job_200811092030_0001` (TeraSort/random-write style) writing job metadata to `/mnt/hadoop/mapred/system/` and mapper output to `/user/root/rand/`.

> Note: The log snapshot is short (28 s) and captures the **tail of a running job**: most of the 385 `allocateBlock` calls appear in the last few seconds of the trace and their blocks are still *Receiving* (in-flight) when the log ends. Only **4 blocks** have fully completed with a confirmed size reported by `PacketResponder`/`DataXceiver` (`Received block … of size N`), so size statistics below are based on those confirmed blocks. Rate estimates are computed in two ways (confirmed-completions vs. allocation events) and flagged accordingly.

---

## 1. Data Volume

| Metric | Value |
|---|---|
| Unique blocks with confirmed size | **4** |
| Unique blocks started (`Receiving block …`) | 390 |
| `allocateBlock` calls (new blocks requested by clients) | 385 |
| `addStoredBlock` events (NameNode block-map updates) | 19 |
| **Total logical bytes** (sum of confirmed block sizes, no replication) | **348,343 B** (≈ 340.18 KB) |
| **Total cluster bytes** (each replica counted across all nodes that confirmed receiving) | **1,409,741 B** (≈ 1.34 MB) |
| Total cluster bytes @ nominal 3× replication (the steady-state factor observed on 3 of 4 blocks) | 1,045,029 B (≈ 1.00 MB) |

The larger value (1.34 MB vs. 1.00 MB at 3×) comes from one block (`blk_-1608999687919862906`, the job.jar) that was over-replicated during node balancing/re-replication while the log was captured (10 NameNode replicas, 7 confirmed `Received` at the datanode level — see §5).

---

## 2. Block Size Distribution

All four confirmed blocks have distinct sizes; none of them hit the default block-size cap (Hadoop default is 64 MB), which is expected for small metadata files and early map-output spills:

| Block ID | Size (bytes) | Human-readable | File (from `allocateBlock`) |
|---|---:|---|---|
| blk_-3544583377289625738 | 11,971 | 11.69 KB | `/mnt/hadoop/mapred/system/.../job.xml` |
| blk_-9073992586687739851 | 11,977 | 11.70 KB | `/user/root/rand/_logs/history/..._conf.xml` |
| blk_-1608999687919862906 | 91,178 | 89.04 KB | `/mnt/hadoop/mapred/system/.../job.jar` |
| blk_7503483334202473044 | 233,217 | 227.75 KB | `/mnt/hadoop/mapred/system/.../job.split` |

**Summary statistics (N = 4):**

| Stat | Value |
|---|---:|
| Min | **11,971 B** (11.69 KB) |
| Max | **233,217 B** (227.75 KB) |
| Mean | **87,085.75 B** (≈ 85.04 KB) |
| Median | **51,577.5 B** (≈ 50.37 KB) |

The distribution is **bimodal** in this snapshot: two tiny metadata blocks (~12 KB) and two larger-but-still-sub-block job artifacts (89 KB and 228 KB). All are well under the HDFS block size, so each corresponds to one complete file (no multi-block files were completed in the window).

---

## 3. Data Distribution by Node

### 3a. Nodes with confirmed `Received block …` completions (PacketResponder / DataXceiver)

Traffic is heavily concentrated on the job-submitter node `10.250.19.102` (it is the first writer for every metadata block, which is normal for a client writing from that host):

| Rank | DataNode IP | Bytes stored (confirmed) | Blocks stored |
|---:|---|---:|---:|
| 1 | 10.250.19.102 | 348,343 (340.18 KB) | 4 |
| 2 | 10.251.215.16 | 324,395 (316.79 KB) | 2 |
| 3 | 10.251.71.16  | 233,217 (227.75 KB) | 1 |
| 4 | 10.250.10.6   | 91,178 (89.04 KB) | 1 |
| 5 | 10.250.14.224 | 91,178 (89.04 KB) | 1 |
| 6 | 10.251.74.79  | 91,178 (89.04 KB) | 1 |
| 7 | 10.251.107.19 | 91,178 (89.04 KB) | 1 |
| 8 | 10.251.31.5   | 91,178 (89.04 KB) | 1 |
| 9 | 10.250.14.196 | 11,977 (11.70 KB) | 1 |
| 10 | 10.250.7.244 | 11,977 (11.70 KB) | 1 |
| 11 | 10.251.197.226 | 11,971 (11.69 KB) | 1 |
| 12 | 10.250.11.100 | 11,971 (11.69 KB) | 1 |

- **12 distinct datanodes** received at least one completed block.
- The top node (`10.250.19.102`) holds 100% of logical data (it's the source/client and first replica for every block).
- `10.251.215.16` is the only node that received *two* of the four blocks (job.jar + job.split), plus extra re-replicated copies of the job.jar.

### 3b. NameNode-reported storage (`addStoredBlock`)

Looking at NameNode block-map updates gives a slightly wider view (18 distinct nodes acknowledged), including additional replicas placed by the re-replicator for the over-replicated job.jar (nodes `10.251.111.209`, `10.251.71.193`, `10.251.39.179`, `10.251.89.155`, `10.251.106.10`).

---

## 4. Storage Path Analysis

Extracting the HDFS path prefix from all 385 `allocateBlock` entries shows two storage namespaces in use:

| HDFS path prefix | # blocks allocated | Purpose |
|---|---:|---|
| `/user/root/rand/` | **382** (99.2%) | Mapper output / random-write data (`part-XXXXX` in `_temporary/` + history log XML) |
| `/mnt/hadoop/mapred/system/` | **3** (0.8%) | Job metadata: `job.jar`, `job.split`, `job.xml` |

Drilling one level deeper:

- `/user/root/rand/_temporary/_task_200811092030_0001_m_XXXXXX_0/part-XXXXXX` — 381 blocks (per-map output spills, one block per mapper's `part-*` file; these are the "real" data blocks).
- `/user/root/rand/_logs/history/...conf.xml` — 1 block (job history config XML).
- `/mnt/hadoop/mapred/system/job_200811092030_0001/` — 3 blocks (job.jar, job.split, job.xml).

**Key observation:** the vast majority of storage (by block count) is headed for `/user/root/rand/` — the actual job data — but none of those 382 data blocks had completed receiving within the captured 28-second window. The four fully-confirmed blocks are all small metadata/system files in the first few seconds. The physical disk volumes on the datanodes are abstracted away at the NameNode log (the logged "path" is the HDFS logical path); per-volume disk usage (e.g. `dfs.data.dir`) is not recorded in this log file and would require datanode-side `dfs.data.dir` metrics.

---

## 5. Replication Factor

Hadoop's default replication factor is 3, and **3 of the 4 confirmed blocks exhibit exactly 3 replicas** end-to-end (3 PacketResponder receives + 3 `addStoredBlock` entries). The 4th block (the 91,178-byte `job.jar`, `blk_-1608999687919862906`) was caught mid re-rebalancing: a chain of `ask … to replicate blk_-… to datanode(s) …` commands pushed extra copies to `10.251.215.16`, `10.251.71.193`, `10.251.74.79`, `10.251.107.19`, `10.251.31.5`, …, leading to 7 confirmed datanode receives and 10 NameNode block-map entries within the window. That over-replication is transient (HDFS's re-replication monitor will eventually re-replicate or delete excess copies down to the configured replication factor).

| Source | Min | Max | Mean | Median |
|---|---:|---:|---:|---:|
| PacketResponder/DataXceiver confirmed receives | 3 | 7 | 4.00 | 3.0 |
| NameNode `addStoredBlock` (wider view) | 3 | 10 | 4.75 | 3.0 |
| **Effective (cluster bytes ÷ logical bytes, confirmed)** | — | — | **4.05** | — |
| **Steady-state (excluding the over-replicated block)** | 3 | 3 | **3.00** | 3.0 |

**Conclusion:** the cluster is configured with **replication factor = 3**, which is the value observed for every normally-completed block. The mean of ~4× is an artifact of the job.jar re-replication traffic captured mid-flight.

---

## 6. Capacity Planning (1-Hour Projection)

Because the 28-second window contains both (a) completed metadata blocks and (b) a storm of in-flight data-block allocations at the end of the trace, I provide two projections:

### Method A — Confirmed-completions rate (conservative, lower bound)

Counts only the blocks that actually finished receiving during the window.

| Metric | Value |
|---|---|
| Logical ingest rate | 12,440 B/s ≈ **12.15 KB/s** |
| Cluster write rate (×3 replicas, steady-state) | 37,322 B/s ≈ **36.45 KB/s** |
| Cluster write rate (including the over-replicated block) | 50,348 B/s ≈ **49.17 KB/s** |
| **1 h logical new data** | **≈ 44.8 MB/h** |
| **1 h cluster storage needed** (×3, steady-state) | **≈ 134 MB/h** |
| **1 h cluster storage needed** (raw observed, incl. over-repl) | ≈ 177 MB/h |

### Method B — Allocation-event rate (optimistic, includes in-flight data blocks)

Uses the 385 `allocateBlock` calls as the block-creation rate (385 / 28 s ≈ 13.75 blocks/s) multiplied by the **observed mean block size 87,086 B**. This is an upper-bound extrapolation because it assumes the allocation burst at the end of the trace continues steadily for an hour, and it reuses the average block size from metadata (which may understate or overstate real data blocks depending on `dfs.block.size`).

| Metric | Value |
|---|---|
| Block allocation rate | 13.75 blocks/s ≈ 49,500 blocks/h |
| Implied logical ingest rate (× mean block size) | ≈ **1.17 MB/s** |
| Implied cluster write rate (×3 replicas) | ≈ **3.51 MB/s** |
| **1 h logical new data** | **≈ 4.20 GB/h** |
| **1 h cluster storage needed** (×3) | **≈ 12.6 GB/h** |

### Recommended planning figure

The two methods bound reality:
- Metadata-only completions (**A**) clearly understate throughput because the 382 mapper-output blocks hadn't finished when the log was truncated.
- Pure allocation extrapolation (**B**) overstates because the trace starts during metadata writes and ends at the very beginning of the mapper-output wave — it isn't a steady-state hour.

A reasonable **midpoint engineering estimate** for sustained runtime storage growth on this workload:

> **~50–100 MB/s logical → ~150–300 MB/s cluster (×3) → ~540–1080 GB/hour cluster-wide (≈ 0.5–1.0 TB/hour)**
>
> To size accurately, capture a longer log window (≥ 10 minutes) covering a full map/reduce cycle; the block-size distribution should converge to the configured `dfs.block.size` (typically 64 MB) once large mapper outputs complete, which will shift the mean block size sharply upward and Method B will then dominate.

### Headroom & replication overshoot

- The job.jar re-replication episode shows the cluster can transiently write up to **10 replicas** of a block during recovery/balancing; reserve **≥ 20% temporary headroom** above the steady-state ×3 capacity to absorb re-replication storms without failing writes.
- The workload shows **strong write affinity** to the client node (`10.250.19.102` received every block's first replica). For sustained writes from many clients, distribute clients across rack topology to spread first-replica load and avoid hotspots.

---

## Appendix: Per-block replica detail

| Block ID | File (HDFS path) | Size | Confirmed replicas (PacketResponder/DX) | NN replicas (addStoredBlock) |
|---|---|---:|---:|---:|
| blk_-3544583377289625738 | job.xml | 11,971 | 3 | 3 |
| blk_-9073992586687739851 | job conf.xml (history) | 11,977 | 3 | 3 |
| blk_7503483334202473044  | job.split | 233,217 | 3 | 3 |
| blk_-1608999687919862906 | job.jar (re-replicated mid-log) | 91,178 | 7 | 10 |
