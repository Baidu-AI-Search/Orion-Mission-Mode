# Hadoop MapReduce Straggler Analysis Report

**Job:** `job_1445062781478_0011`
**Input size:** 1,256,521,728 bytes (~1.17 GiB) across 10 splits
**Configuration:** 10 map tasks × 1 reduce task, non-uberized, speculative execution enabled
**Log file analyzed:** `mapreduce.log` (MRAppMaster log from Hadoop 2.6.0)
**Job timeline (MRAppMaster view):**

| Milestone | Timestamp |
|---|---|
| Job → RUNNING | 2015-10-17 15:37:59.481 |
| All 10 map containers assigned | 15:38:01.588 – 15:38:01.596 (batch) |
| All maps → RUNNING | 15:38:02.033 – 15:38:02.053 (batch) |
| First map SUCCEEDED (m_000009) | 15:39:24.265 |
| Reduce slow-start threshold met | 15:39:24.732 (1/10 maps complete) |
| Reduce container assigned / RUNNING | 15:40:29.834 / 15:40:29.854 |
| Last map SUCCEEDED (m_000006) | 15:41:25.713 |
| Reduce SUCCEEDED | 15:42:46.420 |
| Job → SUCCEEDED | 15:42:46.468 |

**Total job duration (SETUP→RUNNING to SUCCEEDED):** **287.0 s (4 min 47 s)**

![Job timeline](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784186677922025497/6edd59db51998fa2f8c07ef22ed92f26_timeline.png?responseContentDisposition=attachment%3B%20filename%3D%22timeline.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T07%3A33%3A28Z%2F-1%2Fhost%2F5d86bc9a6ee0faf39449e6df3f41e03a82b39c1b0c9376c9135845a9dfc48327)

---

## 1. Task Completion Times

Duration is measured from **container assignment** (RM "Assigned container … to attempt_…" event) to **task-level SUCCEEDED** event. A secondary "runtime" duration is also provided, measured from TaskAttempt `ASSIGNED → RUNNING` to SUCCEEDED (i.e., excluding ~0.45 s of container-launch overhead). All ten map attempts were launched in a single batch at 15:38:01.6 ± 0.01 s, so elapsed time from assignment is a fair direct comparison.

### Map tasks

| # | Task ID | Container Assigned | Task Succeeded | Duration (assigned → succeeded) | Runtime (running → succeeded) |
|---|---|---|---|---:|---:|
| 1 | task_…_m_000000 | 15:38:01.588 | 15:40:34.116 | 152.5 s | 152.1 s |
| 2 | task_…_m_000001 | 15:38:01.591 | 15:40:50.092 | 168.5 s | 168.1 s |
| 3 | task_…_m_000002 | 15:38:01.592 | 15:40:50.266 | 168.7 s | 168.2 s |
| 4 | task_…_m_000003 | 15:38:01.592 | 15:40:32.079 | 150.5 s | 150.0 s |
| 5 | task_…_m_000004 | 15:38:01.593 | 15:40:50.566 | 169.0 s | 168.5 s |
| 6 | task_…_m_000005 | 15:38:01.593 | 15:40:28.630 | 147.0 s | 146.6 s |
| 7 | task_…_m_000006 | 15:38:01.594 | 15:41:25.713 | **204.1 s** 🔶 | 203.7 s |
| 8 | task_…_m_000007 | 15:38:01.594 | 15:41:12.316 | 190.7 s | 190.3 s |
| 9 | task_…_m_000008 | 15:38:01.595 | 15:40:52.232 | 170.6 s | 170.2 s |
| 10 | task_…_m_000009 | 15:38:01.596 | 15:39:24.265 | **82.7 s** ⭐ | 82.2 s |

### Reduce task

| Task ID | Container Assigned | RUNNING | SUCCEEDED | Duration (assigned → succeeded) | Runtime (running → succeeded) |
|---|---|---|---|---:|---:|
| task_…_r_000000 | 15:40:29.834 | 15:40:29.854 | 15:42:46.420 | 136.6 s | 136.6 s |

The reduce container was launched **instantly** (20 ms between ASSIGNED and RUNNING), so assignment-based and runtime-based durations are effectively identical.

---

## 2. Fastest vs. Slowest

### Maps

- **Fastest map:** `task_…_m_000009` — **82.7 s**. It finished **almost a full minute** before the next map (m_000005 at 147.0 s) and was the only map done before 15:40. This is the task whose completion *unlocked* reduce scheduling (Hadoop logged "Reduce slow start threshold reached. Scheduling reduces." 0.47 s after this map finished).
- **Slowest map:** `task_…_m_000006` — **204.1 s** (3 min 24 s). 2.47× slower than the fastest map and **35.5 s slower than the next-slowest** (m_000007 at 190.7 s).
- **Slowest → fastest ratio:** 2.47× — a very wide spread for tasks that were all scheduled on the same two nodes (MSRA-SA-39 and MSRA-SA-41) at the same instant.

### Reduce

There is only one reduce task, so no fastest/slowest comparison within the reduce phase. Its **136.6 s** runtime is examined in §5.

---

## 3. Straggler Analysis

Across the 10 maps, the elapsed-time distribution is:

| Statistic | Value |
|---|---:|
| Min | 82.7 s |
| Max | 204.1 s |
| Mean (μ) | **160.4 s** |
| Median | 168.6 s |
| Std deviation (σ) | 32.5 s |
| Max − Min | 121.4 s |

Tasks whose duration exceeds **μ + 1σ (≈ 192.9 s)** are classified as stragglers:

| Task | Duration | Δ vs. mean | % above mean | z-score | Verdict |
|---|---:|---:|---:|---:|---|
| task_…_m_000006 | 204.1 s | **+43.7 s** | +27% | **+1.34 σ** | **Straggler** 🔶 |
| task_…_m_000007 | 190.7 s | +30.3 s | +19% | +0.93 σ | Borderline/near-straggler |

Tasks below **μ − 1σ (≈ 127.9 s)** are outliers on the fast side:

| Task | Duration | Δ vs. mean | % below mean | z-score | Verdict |
|---|---:|---:|---:|---:|---|
| task_…_m_000009 | 82.7 s | −77.7 s | −48% | **−2.39 σ** | **Fast outlier** ⭐ |

The 8 non-outlier maps (m_000000–m_000005, m_000008) are tightly clustered between **147.0 s and 170.6 s** (23.6 s spread, σ ≈ 9.6 s if m_000006/m_000007/m_000009 are excluded). In other words, two tasks — one very fast, one very slow — account for nearly all of the variance. The most operationally important one is **m_000006**, because it was the *last* map to finish and directly delayed the reduce phase.

Hadoop's speculative execution itself corroborates this: **speculative backups were launched for exactly m_000006 and m_000007** (see §4) — the framework independently identified them as stragglers.

---

## 4. Retry / Speculative Execution Impact

There were **no task failures** (no `FAILED` transitions in the log). The two "retry" attempts (attempt_no = 1) were **speculative (backup) copies** launched by `DefaultSpeculator`. In both cases the *original* attempt (attempt_0) finished first, and the backup was KILLED immediately after the original SUCCEEDED.

### m_000006 (confirmed straggler)

| Attempt | Container | Assigned | Outcome | Elapsed (assigned → end) |
|---|---|---|---|---:|
| attempt_0 (original) | container_…_000008 | 15:38:01.594 | **SUCCEEDED** @ 15:41:25.713 | 204.1 s |
| attempt_1 (speculative) | container_…_000012 | 15:39:25.740 | KILLED @ 15:41:25.731 | 120.0 s |

- The backup was launched **84.1 s after** the original started (≈ 41% into the original's runtime).
- The backup ran for only **120 s before being killed** — at the moment it was killed it had not yet produced output, but its completion rate (120 s projected to a full task) was clearly faster than the 204 s original.
- Net impact: **no time saved** on this task (original won by 18 ms), but the speculative backup consumed an extra container on MSRA-SA-39 for ~2 minutes.

### m_000007 (near-straggler)

| Attempt | Container | Assigned | Outcome | Elapsed (assigned → end) |
|---|---|---|---|---:|
| attempt_0 (original) | container_…_000009 | 15:38:01.594 | **SUCCEEDED** @ 15:41:12.316 | 190.7 s |
| attempt_1 (speculative) | container_…_000014 | 15:40:33.843 | KILLED @ 15:41:12.341 | 38.5 s |

- The backup was launched **very late** — only 38.5 s before the original finished. This is speculative execution essentially "waking up" just as the straggler was about to complete, and it added no value.
- Net impact: **no time saved**; wasted a container on MSRA-SA-41 for 38 s.

### Take-away

The retry/speculative attempts in this job were **not effective** — both originals beat their backups. The speculative copies for m_000007 in particular was launched too late to help. The root cause of m_000006's slowness (data skew, bad node, GC pauses, or a particular split) was not visible at the AM-log level, but its effect on the job is the dominant straggler effect.

---

## 5. Reduce Phase Timing

The job has exactly **one reduce task**, whose schedule relative to the map phase is the single most important timing relationship in the job.

### Key timing relationships

| Event | Timestamp | Δ from reduce RUNNING |
|---|---|---:|
| First map finished (m_000009) | 15:39:24.265 | **−65.6 s** (reduce started 65.6 s later) |
| Reduce slow-start threshold met (1/10 maps) | 15:39:24.732 | −65.1 s |
| Reduce container ASSIGNED | 15:40:29.834 | −0.02 s |
| **Reduce RUNNING** (shuffle/copy phase begins) | **15:40:29.854** | **0 s** |
| Maps finished at this point: m_000009 only (1/10 = 10%) | | |
| 5th map finishes (m_000000) | 15:40:34.116 | +4.3 s |
| 6th/7th/8th/9th maps finish (m_000001/02/04/08) | 15:40:50 – 15:40:52 | +20 – 22 s |
| 2nd-to-last map finishes (m_000007) | 15:41:12.316 | +42.5 s |
| **Last map finishes** (m_000006) | **15:41:25.713** | **+55.9 s** |
| Reduce SUCCEEDED | **15:42:46.420** | **+136.6 s** |

### Interpreting the overlap

- The reduce was scheduled under Hadoop's **slow-start** (`mapreduce.job.reduce.slowstart.completedmaps = 0.05` by default; the AM logged `completedMapsForReduceSlowstart = 1`, i.e., start reduces as soon as **any one** of the ten maps finishes).
- There was an **~65 s delay** between the slow-start threshold being reached and the reduce container actually being assigned. During that interval the RM headroom repeatedly dropped to 0 (log lines show `headroom=<memory:0, vCores:-35>` continuously until 15:40:29), meaning **cluster resources were fully consumed by the 10 running maps** — the reduce had to wait for a container to free up, which happened only after the second map (m_000005) completed at 15:40:28.
- The reduce **started running 55.9 s before the last map finished**, so the **shuffle (copy) phase overlapped almost entirely with the tail of the map phase**. It could copy map outputs from m_000009, m_000005, m_000003, m_000000, and the 15:40:50–15:40:52 wave while m_000007 and m_000006 were still running.
- After the **last map finished at 15:41:25**, the reduce still took **80.7 s** to finish (15:41:25 → 15:42:46). This post-map portion covers the end of copy, merge/sort, and the actual user-defined reduce function. So the reduce phase breaks down approximately as:
  - **Shuffle/copy (while maps finishing):** ~55.9 s (15:40:29 → 15:41:25, overlapped with maps)
  - **Post-shuffle (final merge + reduce + write):** ~80.7 s (15:41:25 → 15:42:46)
  - **Total reduce runtime:** 136.6 s

---

## 6. Bottleneck Identification & Critical Path

### Critical path reconstruction

```
15:38:02  all maps launch  ─────────────────────────────────────────────────┐
                                                                           │
15:39:24  m_000009 done (82 s)  ──► slow-start met, but no free container   │
                                                                           │
15:40:28  m_000005 done        ──►  container frees up                      │
15:40:29  reduce LAUNCHED  ──────────► (shuffle begins)                     │
15:40:32  m_000003 done                                                   │
15:40:34  m_000000 done                                                   │
15:40:50  m_000001/000002 done                                            │
15:40:50  m_000004 done                                                   │
15:40:52  m_000008 done                                                   │
15:41:12  m_000007 done (190.7 s)  ◄── near-straggler                      │
15:41:25  m_000006 done (204.1 s)  ◄── *** MAP STRAGGLER ***               │
15:42:46  reduce done (136.6 s)      ◄── *** REDUCE BOTTLENECK ***         │
15:42:46  JOB SUCCEEDED                                                  ──┘
```

### Two distinct bottlenecks appear

1. **Map-phase straggler: `task_…_m_000006` (204.1 s)**
   - 2.47× slower than the fastest map and 1.34σ beyond the mean.
   - Was the **last map to finish**, and Hadoop's own speculator launched a backup copy — confirming that Hadoop itself viewed it as lagging.
   - Held up the completion of the map stage by **~13.4 s** relative to the next-slowest map (m_000007).
   - Because m_000006 finished 55.9 s after the reduce had started, the reduce *was* able to copy other outputs in parallel — so m_000006 did not block the reduce 1-for-1, but it delayed the point at which the reduce could finish shuffle.

2. **Reduce task duration (136.6 s) — the true critical-path determinant**
   - Although m_000006 was the slowest map, the reduce ran for **136.6 s total and continued for 80.7 s after the last map finished**.
   - If we imagine removing the m_000006 straggler (i.e., m_000006 behaved like the median map at ~169 s and finished at ~15:40:50), the last map would have completed around **15:41:12** (m_000007's finish) and the reduce would still only finish at ~15:42:46 — shaving only ~13 s off the 287 s job.
   - Conversely, the reduce's **80.7 s of non-overlapped post-shuffle work** is the longest single sequential segment on the critical path.

### Verdict

- **Critical-path bottleneck: the single reduce task** (`task_…_r_000000`). It accounts for 136.6 s on the timeline, 80.7 s of which cannot be overlapped with maps and directly determines job completion. With only one reducer, all 10 map outputs must be processed by one container; increasing `mapreduce.job.reduces` (e.g., to multiple reducers) is the highest-leverage improvement.
- **Secondary bottleneck / map straggler: `m_000006`**. It is the clear outlier on the map side (204.1 s, z = +1.34σ) and is the primary reason the map stage lasted ~13 s longer than it otherwise would have. It is also the *reason* speculative execution was triggered — but speculative copies were launched too late (m_000007's backup ran only 38 s before being killed) to help.
- **Cluster capacity was the tertiary bottleneck**: the reduce could not be scheduled until ~65 s after slow-start triggered, because all map containers were consuming the available memory (headroom was 0 for most of the map phase). With only two NodeManagers visible (MSRA-SA-39, MSRA-SA-41) and 10 maps + 1 reduce needing 1 GB each, the reduce had to wait for a map to finish (m_000005) to free a slot.

### Recommended actions

1. **Increase the number of reducers** from 1 to a value proportional to the cluster capacity (e.g., 4–8). The 1.17 GiB input and single-reducer design creates a reduce-heavy critical path that no map-side tuning can remove.
2. **Investigate m_000006's data split** for skew (e.g., is one input split larger or more computationally expensive than the others?) and check the node-local mapper logs on MSRA-SA-39 for GC or I/O anomalies.
3. **Tune speculative execution** — the backup attempts for m_000006/m_000007 were launched too late to win. Consider lowering `mapreduce.job.speculative.speculativecap`/`slowstartthreshold`-equivalent speculation aggressiveness, or accept that with two slow nodes speculation won't help.
4. **Increase cluster capacity** (or lower per-container memory) so the reduce can start sooner after slow-start; the 65 s scheduling delay here is pure queueing waste.
