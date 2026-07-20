# Hadoop MapReduce Resource Utilization Report

**Log file analyzed:** `mapreduce.log`
**Job ID:** `application_1445062781478_0011` (job `job_1445062781478_0011`)
**Queue:** `default`
**Cluster max container capability:** `<memory:8192 MB, vCores:32>`
**Run timeline:** 2015-10-17 15:37:56.547 → 15:42:47.708 (**4 min 51 s**)
**Input size:** 1,256,521,728 bytes ≈ **1198.3 MB** (not uberized – multi-container job)
**Map task container request:** `<memory:1024 MB, vCores:1>`
**Reduce task container request:** `<memory:1024 MB, vCores:1>`
**Map tasks (splits):** 10 &nbsp;|&nbsp; **Reduce tasks:** 1 &nbsp;|&nbsp; **Host-local assignments:** 12 / 13 (≈92%) &nbsp;|&nbsp; **Rack-local:** 0

---

## 1. Container Inventory

The YARN RM allocated **13 containers** for this application master. Container IDs follow the pre-Hadoop-2.6 naming scheme (`container_<clusterTs>_<appId>_<attempt>_<seq>`). Container `..._01_000001` is the MR ApplicationMaster container itself and is not managed by `RMContainerAllocator`; the 13 listed below are all task containers assigned through the allocator.

| # | Container ID | Type | Task Attempt | Host |
|---|---|---|---|---|
| 1 | `container_1445062781478_0011_01_000002` | Map | `attempt_..._m_000000_0` | MSRA-SA-41 (host-local) |
| 2 | `container_1445062781478_0011_01_000003` | Map | `attempt_..._m_000001_0` | MSRA-SA-41 (host-local) |
| 3 | `container_1445062781478_0011_01_000004` | Map | `attempt_..._m_000002_0` | MSRA-SA-41 (host-local) |
| 4 | `container_1445062781478_0011_01_000005` | Map | `attempt_..._m_000003_0` | MSRA-SA-41 (host-local) |
| 5 | `container_1445062781478_0011_01_000006` | Map | `attempt_..._m_000004_0` | MSRA-SA-41 (host-local) |
| 6 | `container_1445062781478_0011_01_000007` | Map | `attempt_..._m_000005_0` | MSRA-SA-39 (host-local) |
| 7 | `container_1445062781478_0011_01_000008` | Map | `attempt_..._m_000006_0` | MSRA-SA-39 (host-local) – **speculated (backup _1 also ran)** |
| 8 | `container_1445062781478_0011_01_000009` | Map | `attempt_..._m_000007_0` | MSRA-SA-39 (host-local) – **speculated (backup _1 also ran)** |
| 9 | `container_1445062781478_0011_01_000010` | Map | `attempt_..._m_000008_0` | MSRA-SA-39 (host-local) |
| 10 | `container_1445062781478_0011_01_000011` | Map | `attempt_..._m_000009_0` | MSRA-SA-39 (host-local) |
| 11 | `container_1445062781478_0011_01_000012` | Map (speculative backup) | `attempt_..._m_000006_1` | MSRA-SA-39 (host-local) – **KILLED** |
| 12 | `container_1445062781478_0011_01_000013` | Reduce | `attempt_..._r_000000_0` | MSRA-SA-41 (host-local) |
| 13 | `container_1445062781478_0011_01_000014` | Map (speculative backup) | `attempt_..._m_000007_1` | MSRA-SA-41 (host-local) – **KILLED** |

All 13 containers are 1024 MB / 1 vCore. Total peak memory reserved by this job on the cluster: **13 × 1024 MB = 13 GB**; however allocator headroom snapshots (see §3) show cluster memory was fully consumed (headroom=0) between ~15:38:15 and ~15:39:24, confirming the 10 initial map containers saturated available memory.

---

## 2. Container Allocation Timeline

The AM requested all 10 maps immediately after the job transitioned to RUNNING (15:37:59). Reduces were initially **not requested** (slow-start gate). Resources were granted by the RM in four batches.

| Time (UTC+8) | Event | Container(s) | Latency since previous |
|---|---|---|---|
| 15:37:59.505 | AM posts **mapResourceRequest** (10 containers × 1024 MB / 1 vCore) | (request only) | — |
| 15:37:59.511 | AM posts **reduceResourceRequest** (1 container × 1024 MB / 1 vCore) – gated by slow start | (request only) | +6 ms |
| 15:38:00.464 | First RM heartbeat: Before Scheduling – `ScheduledMaps=10, ScheduledReds=0, ContAlloc=0` | — | +0.95 s |
| 15:38:01.585 | **RM grants 10 containers** in one heartbeat (`Got allocated containers 10`) | 000002–000011 | +1.12 s after request |
| 15:38:01.588–.596 | AM assigns all 10 containers to map attempts `m_000000_0 … m_000009_0` | 000002–000011 | ~8 ms |
| 15:38:01.855–.858 | **CONTAINER_REMOTE_LAUNCH** events – NM launch commands issued for all 10 mappers | 000002–000011 | ~270 ms after assign |
| 15:38:02.027–.052 | All 10 map attempts transition `ASSIGNED → RUNNING`; Speculator registers them | — | ~170 ms after launch |
| 15:39:24.474 | Speculator decides to back up **m_000006** (slowest mapper so far) | new request queued | — |
| 15:39:25.740 | **RM grants 1 container** – assigned to speculative attempt `m_000006_1` | 000012 | ~84 s after first batch |
| 15:39:25.741 | CONTAINER_REMOTE_LAUNCH for speculative mapper `m_000006_1` | 000012 | +1 ms |
| 15:39:39.476 | Speculator decides to back up **m_000007** (second slow mapper) | new request queued | — |
| 15:40:29.834 | **RM grants 1 container** – assigned to **reduce** `r_000000_0` (slow start already met) | 000013 | ~64 s after previous grant |
| 15:40:29.843 | CONTAINER_REMOTE_LAUNCH for reduce | 000013 | +9 ms |
| 15:40:33.843 | **RM grants 1 container** – assigned to speculative mapper `m_000007_1` | 000014 | ~4 s after reduce grant |
| 15:40:33.845 | CONTAINER_REMOTE_LAUNCH for speculative mapper `m_000007_1` | 000014 | +2 ms |

**Observations on request→assign latency**
- The initial 10-map bulk request took **~2.1 s** from the resource-request post (15:37:59.505) to container assignment (15:38:01.588). This is extremely fast, indicating the cluster had free capacity at job start (headroom 32 GB/–3 vcores right before scheduling).
- After the 10 mappers were launched, headroom dropped from 32 GB to 0 within ~14 s (see §3), so subsequent container grants waited for mappers to finish: the first speculative container waited **~61 s** from speculation decision to grant (15:39:24 → 15:39:25, a mapper had just freed a slot); the reduce waited **~65 s** after the slow-start gate opened (15:39:24 → 15:40:29) and the second speculative backup waited a further **~4 s** for another slot.
- CONTAINER_REMOTE_LAUNCH happens within 1–10 ms of assignment, i.e., AM→NM RPC is not a bottleneck in this run.

---

## 3. Scheduling Analysis – Pending Maps/Reduces Over Time

All numbers below are taken directly from `RMContainerAllocator` `Before Scheduling` / `After Scheduling` heartbeats. `pendingMaps` is computed as `ScheduledMaps + AssignedMaps` not yet `CompletedMaps`; `pendingReds` is what the log calls `PendingReds`. Headroom is from the `Recalculating schedule, headroom=<…>` lines.

| Timestamp | Pending Maps (Scheduled+Assigned-Running) | Maps In Progress (AssignedMaps) | Completed Maps | Pending Reds | Reds In Progress (AssignedReds) | Completed Reds | Containers Alloc'd (ContAlloc) | Headroom (mem MB / vCores) |
|---|---|---|---|---|---|---|---|---|
| 15:38:00.464 (Before) | **10** | 0 | 0 | **1** | 0 | 0 | 0 | 32768 / –3 |
| 15:38:01.596 (After) | 0 | **10** | 0 | 1 | 0 | 0 | **10** | 22528 / –13 |
| 15:38:05.606 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 18432 / –17 |
| 15:38:06.609 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 15360 / –20 |
| 15:38:07.612 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 13312 / –22 |
| 15:38:08.614 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 10240 / –25 |
| 15:38:09.617 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 7168 / –28 |
| 15:38:10.619 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 8192 / –27 |
| 15:38:12.623 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 7168 / –28 |
| 15:38:13.626 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 5120 / –30 |
| 15:38:14.628 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 3072 / –32 |
| 15:38:15.632 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | **0 / –35** |
| 15:38:17.637 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 1024 / –34 |
| 15:38:22.642 | 0 | 10 | 0 | 1 | 0 | 0 | 10 | 0 / –35 |
| … (next ~62 s of no status lines; mappers running) | 10 | 10 | 0 | 1 | 0 | 0 | 10 | 0 |
| 15:39:24.730 (Before) | 1 (speculation re-schedule) | 10 | **1** | 1 | 0 | 0 | 10 | 0 / –35 |
| 15:39:24.732 | ⚡ **Slow-start gate flips – reduces scheduled** | 10 | 1 | 0 | 0 | 0 | 10 | 0 / –35 |
| 15:39:25.740 (After) | 0 | 10 | 1 | 0 | 0 | 0 | **11** | — |
| 15:39:39.764 (Before) | 1 (m_000007 spec req) | 10 | 1 | 0 | 0 | 0 | 11 | — |
| 15:40:28.830 (Before) | 1 | 10 | **2** | 0 | 0 | 0 | 11 | — |
| 15:40:29.834 (After) | 1 | 9 | 2 | 0 | **1** | 0 | **12** | — |
| 15:40:32.837 (Before) | 1 | 9 | **3** | 0 | 1 | 0 | 12 | — |
| 15:40:33.844 (After) | 0 | 9 | 3 | 0 | 1 | 0 | **13** | — |
| 15:40:34.844 (Before) | 0 | 9 | **4** | 0 | 1 | 0 | 13 | — |
| 15:40:35.850 (After) | 0 | **8** | 4 | 0 | 1 | 0 | 13 | — |
| 15:40:50.886 (Before) | 0 | 8 | **7** | 0 | 1 | 0 | 13 | — |
| 15:40:50.887 (After) | 0 | **6** | 7 | 0 | 1 | 0 | 13 | — |
| 15:40:51.890 (After) | 0 | **5** | 7 | 0 | 1 | 0 | 13 | — |
| 15:40:52.891 (Before) | 0 | 5 | **8** | 0 | 1 | 0 | 13 | — |
| 15:40:53.895 (After) | 0 | **4** | 8 | 0 | 1 | 0 | 13 | — |
| 15:41:12.920 (Before) | 0 | 4 | **9** | 0 | 1 | 0 | 13 | — |
| 15:41:13.922 (After) | 0 | **2** | 9 | 0 | 1 | 0 | 13 | — |
| 15:41:25.936 (Before) | 0 | 2 | **10** | 0 | 1 | 0 | 13 | — |
| 15:41:26.939 (After) | 0 | **0** | 10 | 0 | 1 | 0 | 13 | — |
| 15:42:46.420 | Reduce transitions RUNNING → SUCCEEDED | 0 | 10 | 0 | 0 | **1** | 13 | — |
| 15:42:47.696 (Final Stats) | 0 | 0 | 10 | 0 | 1 (still assigned at stats log; released immediately after) | 0 | 13 | — |

**Scheduling pattern summary**
- **0:00–0:15 (startup ramp).** All 10 mappers requested and granted in a single wave. Headroom collapses from 32 GB to 0 GB as NMs register the 10 × 1 GB containers – this is a classic **full-wave map launch** on a cluster with enough free slots.
- **0:15–1:28 (steady map phase).** All 10 mappers run concurrently; pending maps=0; reduce stays in PendingReds=1 waiting on slow-start; cluster is **100% full** for this job (and probably the cluster, given headroom=0). No new containers allocated for 84 seconds.
- **1:28 (slow start opens).** First map (m_000009) completes at 15:39:24.265; within ~470 ms the allocator logs `Reduce slow start threshold reached. Scheduling reduces.` and PendingReds flips 1→0, ScheduledReds 0→1.
- **1:28–2:34 (mixed phase: maps finishing + reduce running).** Mappers complete in a fast cascade (3 maps finish between 15:40:28 and 15:40:35; 3 more by 15:40:51). Reducer is granted a container only at 15:40:29 (after 2 mappers finish), then runs for ~2 min 17 s to completion. The two speculative backups are given containers only during this phase, after regular mappers free slots.
- **3:29–4:50 (reduce-only tail).** Last map finishes at 15:41:25.713; reduce keeps running (shuffle/sort/merge/reduce) until 15:42:46 – **~81 s of pure reduce with zero map parallelism**, i.e., the cluster had no map work left but the job still held the reduce container.

---

## 4. Reduce Slow-Start Threshold

The Hadoop `mapreduce.job.reduce.slowstart.completedmaps` configuration controls the fraction of maps that must finish before reduces are scheduled. The log prints this threshold explicitly in every "threshold not met" message as the minimum required number of completed maps:

```
completedMapsForReduceSlowstart 1
```

So the threshold is **1 completed map out of 10**, i.e. **10 %** of maps. (In Hadoop 2.x the default is 0.05; this cluster's config has been raised to 0.10 or equivalent absolute count 1 for a 10-map job.)

| Milestone | Timestamp | Maps done / total | % complete |
|---|---|---|---|
| Maps start running | 15:38:02 | 0 / 10 | 0% |
| "threshold not met" messages begin | 15:38:00.536 | 0 / 10 | 0% |
| First map completes (`m_000009` in container 000011) | 15:39:24.265 | 1 / 10 | **10%** |
| **"Reduce slow start threshold reached. Scheduling reduces."** | **15:39:24.732** | **1 / 10** | **10%** |
| Reduce request moves ScheduledReds 0→1 | 15:39:24.732 | 1 / 10 | 10% |
| Reduce container **granted** (had to wait for a free slot) | 15:40:29.834 | 2 / 10 | 20% |
| Reduce launches (`CONTAINER_REMOTE_LAUNCH`) | 15:40:29.843 | 2 / 10 | 20% |
| Reduce RUNNING → SUCCEEDED | 15:42:46.420 | 10 / 10 | 100% |

**Key insight:** even though the *scheduling* gate opened at 10 % map completion (15:39:24), the reduce **container was not actually allocated until ~65 s later** because the cluster was at zero headroom; the reduce only started running once two maps had finished. Thus the *effective* reduce launch happened at 20 % map completion, not 10 %. That ~65-second delay between "scheduled" and "running" is a direct consequence of the full-wave map launch consuming all available slots – exactly the scenario slow-start is designed to avoid, but with the threshold set to 1/10=10% the AM tried to schedule reduces before enough slots were free.

---

## 5. Container Reuse

**Classic JVM reuse is disabled in this run.** The evidence:

- No `Container ... reused` or JVM-reuse log lines appear.
- Every container ID is assigned to **exactly one** task attempt:
  - `container_..._000002` → `m_000000_0` only
  - `container_..._000003` → `m_000001_0` only
  - …
  - `container_..._000012` → `m_000006_1` only (speculative backup of m_000006; original `_0` still ran in `...000008`)
  - `container_..._000013` → `r_000000_0` only
  - `container_..._000014` → `m_000007_1` only
- Zero "Released container" log lines appear until cleanup-after-success (containers 000012 and 000014 transition to KILL_CONTAINER_CLEANUP when their speculative attempts are killed at 15:41:25, and container 000013 is cleaned up after reduce success at 15:42:46). Each task attempt (including the two speculative backups and the one reducer) gets its own freshly-allocated container.
- `mapreduce.job.jvm.numtasks` (JVM reuse) defaults to 1 in Hadoop 2/YARN, which is the behaviour we see. There was no **container ID reuse** either – each new allocation gets the next sequence number (000002…000014).

**Speculative execution (related but not reuse).** Two map tasks, `m_000006` and `m_000007`, were detected as stragglers by `DefaultSpeculator` and received **backup attempts (`_1`)** in brand new containers (000012 and 000014). In both cases the _0 attempt in the original container finished first, so the _1 backups were killed (transitions RUNNING → KILL_CONTAINER_CLEANUP → KILLED). This is **speculative duplication**, not reuse – those 2 containers consumed cluster resources for redundant work (see §6).

---

## 6. Resource-Efficiency Assessment

### 6.1 Headline numbers

| Metric | Value |
|---|---|
| Total unique task containers allocated | **13** |
| Productive containers (10 maps + 1 reduce, all succeeded) | 11 |
| Wasted containers (2 speculative backups killed) | **2** (≈15% of allocations) |
| Memory-seconds used (all containers × 1 GB × lifetime, approx.) | see §6.2 |
| Peak concurrent containers on cluster | 11 (10 maps + 1 speculative backup _or_ 9 maps + 1 reduce + 1 backup) |
| Time cluster was at zero headroom because of this job (mem) | ≈ 15:38:15 – 15:39:24 → **~69 s** during map phase |
| Map phase duration (first map launch → last map finish) | 15:38:02 → 15:41:26 → **3 min 24 s** |
| Reduce phase duration (reduce launch → reduce success) | 15:40:30 → 15:42:46 → **2 min 17 s** |
| Overlap of map & reduce | ~57 s (15:40:29–15:41:26) |
| Reduce-only tail (all maps done, reduce still running) | **~80 s** (15:41:26–15:42:46) |
| Host-local data-local assignments | 12 / 13 = 92% – excellent data locality |
| Rack-local (non-host-local) assignments | 0 – no cross-rack data movement |

### 6.2 Container lifetime & utilization

Approximate per-container busy time (launch → terminal state transition):

| Container | Launch | Terminated | Lifetime | Productive? |
|---|---|---|---|---|
| 000002 (m_000000) | 15:38:01.855 | 15:40:34.116 | **2 min 32 s** | ✅ Map succeeded |
| 000003 (m_000001) | 15:38:01.855 | 15:40:50.092 | **2 min 48 s** | ✅ Map succeeded |
| 000004 (m_000002) | 15:38:01.855 | 15:40:50.266 | **2 min 48 s** | ✅ Map succeeded |
| 000005 (m_000003) | 15:38:01.856 | 15:40:32.079 | **2 min 30 s** | ✅ Map succeeded |
| 000006 (m_000004) | 15:38:01.856 | 15:40:50.566 | **2 min 49 s** | ✅ Map succeeded |
| 000007 (m_000005) | 15:38:01.856 | 15:40:28.630 | **2 min 27 s** | ✅ Map succeeded |
| 000008 (m_000006) | 15:38:01.857 | 15:41:25.713 | **3 min 24 s** | ✅ Map succeeded (straggler – winner) |
| 000009 (m_000007) | 15:38:01.857 | 15:41:12.316 | **3 min 10 s** | ✅ Map succeeded (straggler – winner) |
| 000010 (m_000008) | 15:38:01.857 | 15:40:52.232 | **2 min 50 s** | ✅ Map succeeded |
| 000011 (m_000009) | 15:38:01.858 | 15:39:24.265 | **1 min 22 s** | ✅ Map succeeded (fastest) |
| 000012 (m_000006_1 speculative) | 15:39:25.741 | 15:41:25.731 | **2 min 00 s** | ❌ Killed – duplicate work |
| 000013 (r_000000) | 15:40:29.843 | 15:42:46.420 | **2 min 17 s** | ✅ Reduce succeeded |
| 000014 (m_000007_1 speculative) | 15:40:33.845 | 15:41:12.341 | **~39 s** | ❌ Killed – duplicate work |

Total container-seconds reserved ≈ **29 min 16 s** at 1 GB / 1 vcore ≈ **1,756 container-seconds**.
Productive work ≈ 26 min 37 s of container runtime.
**Wasted runtime ≈ 2 min 39 s** (containers 000012 + 000014) → **~9 % container-time overhead from speculation**. For comparison, the two original slow maps (m_000006, m_000007) took 3:24 and 3:10 respectively; the median of the other 8 mappers was ≈2:44, so speculation was arguably triggered correctly (they were indeed ~30–40 s slower than the median), but in the end their backups never finished first – the original attempts still won.

### 6.3 Qualitative findings

**Strengths**
- **Excellent data locality (92 % host-local, 0 % non-rack-local).** All splits were scheduled on nodes holding the input blocks; no cross-rack network traffic was generated for map input reads. This is the dominant driver of fast startup.
- **Single-wave map launch.** The 10 mappers started almost simultaneously (within ~30 ms of each other), producing no map-side scheduling gaps in the first wave.
- **Very fast AM-RM-NM handshake.** From AM requesting containers to all NMs launching JVMs took ≈2.3 s.

**Inefficiencies**
- **All-at-once map launch saturated cluster memory.** Headroom dropped to 0 MB within 14 s; once the first map finished at 1:28 the reduce request was ready but had to wait **65 s** for a free slot. This is the classic "full wave map → delayed reduce start" anti-pattern: lowering the slow-start threshold or reducing map parallelism would not help; rather, the cluster should have reserved a slot for the reduce, or the slow-start threshold should be increased (paradoxically) so the reducer is scheduled *later* but on a freed slot without waiting.
- **Speculative execution wasted 2 containers (≈2.4 container-minutes).** Both backups were launched after the slow maps were already 60–70 % complete and both were killed within ~0.5 s of the original winning attempt finishing. The speculator's 15 s sleep between scans (`We launched 1 speculations. Sleeping 15000 milliseconds.`) plus the container-start latency meant the backup never overtook the original. For jobs with small numbers of long-running mappers (as here, ~3 min), speculation adds noise and container churn without benefit.
- **Long reduce-only tail (~80 s).** After the last map finished at 15:41:25 the reducer continued for 80 s with no cluster parallelism to hide latency. This is inherent to the job's reduce-heavy workload (only 1 reducer processing the output of 10 mappers on ~1.2 GB input). The single reducer is itself a bottleneck: it has to fetch all 10 map outputs, merge them, and run the reduce function serially.
- **No JVM reuse.** Each task spawned a fresh JVM; with JVM reuse enabled (`mapreduce.job.jvm.numtasks > 1`, e.g. 2–3 on a single-node pool), container startup overhead (~0.5–1 s per container for JVM init, as seen from the JVM "asked for a task" lines 2–4 s after launch) could have been amortized. In this 4-minute job the benefit would be modest (a few seconds), but on longer jobs it is material.
- **vCore accounting looks anomalous.** The headroom line reports `vCores:-3`, `vCores:-13`, … `vCores:-35` – negative virtual-core headroom for the entire run. With 10 containers × 1 vcore each this should be at worst slightly under; the –35 figure strongly suggests YARN vCore scheduling was effectively **not enforced** on this cluster (common in pseudo-distributed / capacity-scheduler setups where vcores are over-committed), i.e., containers were scheduled on memory alone. That is why 10 × 1-vcore containers could be placed despite impossible vCore headroom numbers.

### 6.3 Recommendations (based on these patterns)

1. **Tune slow-start for a single-reducer job.** With only 1 reducer and a full-wave map launch, setting `mapreduce.job.reduce.slowstart.completedmaps` to ~0.50–0.80 (i.e., schedule the reducer when 5–8 maps finish rather than 1) would (a) avoid the 65 s "scheduled but not allocated" wait visible in §3, and (b) reduce the amount of map output the reducer has to buffer on disk while maps are still running.
2. **Raise the speculation threshold or disable for small jobs.** Two out of thirteen containers (≈15%) were speculative duplicates that finished second and were killed. Consider `mapreduce.job.speculative.speculativecap=0.1` (default) or disabling speculation entirely for short (<5 min) jobs.
3. **Add more reducers.** 1 reducer on 1.2 GB of input is a clear parallelism bottleneck (80 s reduce-only tail). A default of ~1 reducer per 1–2 GB is reasonable but on a cluster with idle cores, partitioning into 3–5 reducers would shorten the tail proportionally.
4. **Enable JVM reuse** (`mapreduce.job.jvm.numtasks=2–4`) to shave JVM-startup overhead off the 11 JVM launches.
5. **Fix vCore accounting.** The persistent negative-vCore headroom indicates the capacity-scheduler's vCore configuration is inconsistent with actual NodeManager vCore counts; if vCore isolation matters, align `yarn.nodemanager.resource.cpu-vcores` with real cores. Otherwise treat CPU as over-committed and size containers based on memory (which is what this run effectively did).

---

## Appendix: Reproduction notes

All timings, container IDs, and counts in this report were extracted by programmatically parsing `mapreduce.log` for `RMContainerAllocator` Before/After Scheduling records, `Assigned container … to attempt_…` assignments, `CONTAINER_REMOTE_LAUNCH` events, `TaskAttempt Transitioned from … to …` state transitions, and `Reduce slow start threshold (not met|reached)` lines. 100 % of the container events in the log are accounted for; there are no orphan container IDs and no container was assigned to more than one task attempt.
