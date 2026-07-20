# Hadoop MapReduce Job Timeline Report

**Job ID:** `job_1445062781478_0011`  
**Job name:** `pagerank` (10 mappers × 1 reducer, ~1.2 GB input)  
**User / Queue:** `msrabi` / `default`  
**Cluster nodes (observed):** MSRA-SA-41, MSRA-SA-39 (default rack), plus datanodes 10.190.173.170, 10.86.164.9  
**Start:** 2015-10-17 15:37:56.547 (MRAppMaster created)  
**End:**   2015-10-17 15:42:47.708 (AM shutdown)  
**Total wall-clock duration:** ≈ **4 min 51 s (291 s)**  
**Final status:** ✅ **SUCCEEDED** (13 total containers allocated, 2 speculative backups killed)

---

## 1. Phase Diagram

The job is broken into five distinct phases plus a short cleanup tail. Start/end times are taken directly from state-transition events in the MRAppMaster log.

```
Phase     Start         End           Duration    Description
------------------------------------------------------------------------------
Init      15:37:56.547  15:38:02.033     5.5 s    AM bootstrap, job init, RPC/HTTP servers, RM registration
Map       15:38:02.033  15:40:29.854   147.8 s    All 10 mappers running; reduce scheduled but not launched (slow-start gated)
Shuffle   15:40:29.854  15:41:25.713    55.9 s    Reduce copying map outputs while remaining mappers finish (overlap)
Reduce    15:41:25.713  15:42:46.420    80.7 s    Merge complete, user reduce() function executes (progress 0.66→1.0)
Commit    15:42:46.420  15:42:46.468     0.0 s    OutputCommitter commits job output; job RUNNING → COMMITTING → SUCCEEDED
Cleanup   15:42:46.468  15:42:47.708     1.2 s    AM unregisters from RM, moves jhist to done dir, deletes staging dir, stops servers
```

**Visual phase bar (one char ≈ 2.9 s):**

```
Init     Map (10×m)        Shuffle       Reduce       C Cleanup
  ..###################################################SSSSSSSSSSSSSSSSSSSRRRRRRRRRRRRRRRRRRRRRRRRRRRC_
  |         |         |          |         |         |         |          |         |         |       
  37:56     38:26     38:56      39:26     39:56     40:26     40:56      41:26     41:56     42:26   
           (times are HH:MM:SS wall-clock)
```

---

## 2. Detailed Event Timeline (chronological)

Timestamps are the exact millisecond-precision values from the ApplicationMaster log. ⚠ flags warnings / errors / speculative executions.

```
 Offset  Wall-clock     Event      Detail
----------------------------------------------------------------------------------------------------
   0.5s  15:37:56.547   INIT       MRAppMaster created for appattempt_1445062781478_0011_000001
   2.0s  15:37:58.024   SETUP      Metrics system started; RPC servers started
   2.6s  15:37:58.569   INITED     Job transitioned NEW → INITED; 10 splits, 1 reduce; not uberized
   3.5s  15:37:59.472   SETUP      Job INITED → SETUP; JOB_SETUP processed
   3.5s  15:37:59.481   RUN        Job SETUP → RUNNING; all 10 maps + 1 reduce scheduled
   5.6s  15:38:01.585   ALLOC      ResourceManager allocated 10 containers for mappers
   5.6s  15:38:01.588   ASSIGN     Containers 000002–000011 assigned to m_000000…m_000009 (all host-local on 2 nodes)
   6.0s  15:38:02.033   MAP        First mappers (m_000006/07/08/09) transition ASSIGNED → RUNNING
   6.1s  15:38:02.052   MAP        All 10 mappers running at full parallelism
  88.2s  15:39:24.244   DONE       m_000009 completed first (fastest, 82.2 s). CompletedMaps = 1/10
  88.5s  15:39:24.474   SPEC       ⚠ Speculator launched backup attempt m_000006_1 (original stuck at ~29%)
  88.7s  15:39:24.732   SHUF       Reduce slow-start threshold reached (10% maps = 1); reduces scheduled
  89.8s  15:39:25.757   SPEC       Speculative m_000006_1 RUNNING in container_000012
 103.5s  15:39:39.476   SPEC       ⚠ Speculator launched backup attempt m_000007_1 (original at ~36%)
 152.6s  15:40:28.630   DONE       m_000005 SUCCEEDED (146.6 s)
 153.8s  15:40:29.834   ALLOC      RM allocated container_000013 for reduce
 153.9s  15:40:29.854   RED        r_000000_0 RUNNING → SHUFFLE PHASE BEGINS (copying map outputs)
 156.1s  15:40:32.079   DONE       m_000003 SUCCEEDED (150.0 s)
 157.8s  15:40:33.843   ALLOC      RM allocated container_000014 for speculative m_000007_1
 157.9s  15:40:33.865   SPEC       Speculative m_000007_1 RUNNING
 158.1s  15:40:34.116   DONE       m_000000 SUCCEEDED (152.1 s)
 169.9s  15:40:45.929   WARN       ⚠ HDFS DFSOutputStream ResponseProcessor exception – bad response from datanode 10.86.164.9; pipeline recovery initiated
 174.1s  15:40:50.092   DONE       m_000001 SUCCEEDED (168.1 s)
 174.3s  15:40:50.266   DONE       m_000002 SUCCEEDED (168.2 s)
 174.6s  15:40:50.566   DONE       m_000004 SUCCEEDED (168.5 s)
 176.2s  15:40:52.232   DONE       m_000008 SUCCEEDED (170.2 s). CompletedMaps = 8/10
 196.3s  15:41:12.316   DONE       m_000007_0 SUCCEEDED (190.3 s) → speculative twin m_000007_1 KILLED
 196.3s  15:41:12.339   WARN       ⚠ FileOutputCommitter could not delete temp output of killed attempt m_000007_1
 209.7s  15:41:25.713   DONE       m_000006_0 SUCCEEDED (203.7 s, slowest) → speculative twin m_000006_1 KILLED. CompletedMaps = 10/10 – ALL MAPS DONE
 209.7s  15:41:25.730   WARN       ⚠ FileOutputCommitter could not delete temp output of killed attempt m_000006_1
 211.5s  15:41:27.499   REDUCE     Reduce progress 0.3 → 0.667 (shuffle & merge finished; user reduce() function executing)
 290.4s  15:42:46.420   DONE       r_000000_0 SUCCEEDED (136.6 s). All 11 tasks complete
 290.4s  15:42:46.422   COMMIT     Job RUNNING → COMMITTING; JOB_COMMIT processed
 290.5s  15:42:46.468   SUCC       Job COMMITTING → SUCCEEDED. Final Stats: ContAlloc=13
 291.7s  15:42:47.708   STOP       AM services stopped; IPC servers shut down; staging directory deleted
```

---

## 3. Gantt-Style Task View

Each row is one task. Legend: `#` = map running, `S` = reduce in shuffle/copy, `R` = reduce executing user code, `x` = killed speculative backup, `[` / `]` = start/end markers. One column ≈ 2.9 s.

```
Task     |  MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMSSSSSSSSSSSSSSSSSSSRRRRRRRRRRRRRRRRRRRRRRRRRRRC|
         |..###################################################:::::::::::::::::::...........................-|
m_000000 |  [###################################################]                                             | 152.1 s SUCCEEDED
m_000001 |  [########################################################]                                        | 168.1 s SUCCEEDED
m_000002 |  [########################################################]                                        | 168.2 s SUCCEEDED
m_000003 |  [##################################################]                                              | 150.0 s SUCCEEDED
m_000004 |  [#########################################################]                                       | 168.5 s SUCCEEDED
m_000005 |  [#################################################]                                               | 146.6 s SUCCEEDED
m_000006 |  [############################[xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx]                           | 203.7 s SUCCEEDED (slowest, speculated)
m_000007 |  [###################################################[xxxxxxxxxxxx]                                | 190.3 s SUCCEEDED (speculated)
m_000008 |  [#########################################################]                                       | 170.2 s SUCCEEDED
m_000009 |  [###########################]                                                                     | 82.2 s SUCCEEDED (first)
r_000000 |                                                     [SSSSSSSSSSSSSSSSSSRRRRRRRRRRRRRRRRRRRRRRRRRRR]| 136.6 s SUCCEEDED (shuffle 55.9 s + reduce 80.7 s)
```

**Key observations from the Gantt:**

- **10-way parallelism at launch**: RM delivered all 10 map containers in a single heartbeat at 15:38:01.585; every mapper was running within 467 ms (by 15:38:02.052).

- **Heavy-tail maps**: m_000009 finished in 82 s (fastest), while m_000006 and m_000007 dragged on for ~190–204 s — long enough for the speculator to fire **two** backup attempts (m_000006_1 at +88 s, m_000007_1 at +103 s).

- **Speculation was essentially wasted**: in both cases the original attempt finished before the backup could; the backups were killed and their temp-output cleanup failed with a WARN.

- **Reduce starts early**: the slow-start threshold (mapreduce.job.reduce.slowstart.completedmaps = 0.05 → 5% rounded → 1 map) was crossed the moment m_000009 finished; r_000000 began shuffling at 15:40:29, while 8 mappers were still running.

- **Shuffle ≈ 56 s**, **reduce user-code ≈ 81 s** (visible in the progress jump 0.3 → 0.667 at 15:41:27, right after the last map completed).

---

## 4. Critical Events (impact analysis)

| # | Time | Severity | Event | Impact on job |
|---|------|----------|-------|----------------|
| 1 | 15:37:58.550 | Info | Job **not uberized** (too many maps / too much input) | Forced multi-container execution → full 10-container parallelism; correct decision for a 1.2 GB, 10-split job. |
| 2 | 15:38:01.585 | Info | RM allocated **10 containers in one shot** (all host-local) | Mappers started simultaneously — no slow-start delay; ideal data locality. |
| 3 | 15:39:24.244 | Info | **First map (m_000009) completed** in 82 s | Crossed the 5% slow-start threshold; kicked off reduce scheduling. |
| 4 | 15:39:24.474 | ⚠ | **Speculative execution of m_000006** (progress ~29% vs. peer avg ~45%) | Allocated an 11th container; added cluster load but ultimately did NOT finish before the original. |
| 5 | 15:39:39.476 | ⚠ | **Speculative execution of m_000007** (progress ~36%) | Allocated a 13th container; also killed without winning. |
| 6 | 15:40:29.854 | Info | **Reduce r_000000 launched**; shuffle phase begins | Reduce started copying map outputs while 8 maps were still running — typical Hadoop overlap to hide shuffle latency. |
| 7 | 15:40:45.929 | ⚠⚠ | **HDFS write failure**: `Bad response ERROR for block … from datanode 10.86.164.9`; pipeline recovery | Transient datanode error while writing the job history file. Client recovered by removing the bad datanode from the pipeline; **no task failure, no data loss** (the jhist was successfully moved to the done-intermediate dir at job end). |
| 8 | 15:41:12.317 | ⚠ | Speculative attempt **m_000007_1 KILLED** after original succeeded | Wasted ~38.5 s of container_000014; temp output could not be deleted (benign WARN). |
| 9 | 15:41:25.714 | ⚠ | Speculative attempt **m_000006_1 KILLED** after original succeeded | Wasted ~120 s of container_000012; again a benign delete WARN. |
| 10 | 15:41:27.499 | Info | Reduce progress jumped **0.3 → 0.667** (shuffle+merge complete, reduce() function started) | Marks the boundary between shuffle and actual reduce computation. |
| 11 | 15:42:46.420 | Info | **Reducer r_000000 SUCCEEDED** → all 11 tasks complete | Job enters COMMIT; final output materialized. |
| 12 | 15:42:46.468 | ✅ | **Job COMMITTING → SUCCEEDED** | Job officially successful; counters finalized. |
| 13 | 15:42:47.708 | Info | **AM shutdown** complete (servers stopped, staging dir deleted) | End of application. |

**No FAILED tasks, no FAILED job state.** The only warnings are (a) one transient HDFS pipeline error that auto-recovered and (b) two post-speculation temp-file delete warnings that are harmless.

---

## 5. Concurrency Analysis

Below is the number of task attempts in `RUNNING` state sampled every 5 seconds across the job. Each `■` ≈ one running container.

```
Time       M  R  Total  Running containers
------------------------------------------------------------
15:37:56    0  0   0    
15:38:06   10  0  10    ■■■■■■■■■■
15:38:26   10  0  10    ■■■■■■■■■■
15:38:56   10  0  10    ■■■■■■■■■■
15:39:26   10  0  10    ■■■■■■■■■■
15:39:56   10  0  10    ■■■■■■■■■■
15:40:26   10  0  10    ■■■■■■■■■■
15:40:31    9  1  10    ■■■■■■■■■■
15:40:36    8  1   9    ■■■■■■■■■
15:40:41    8  1   9    ■■■■■■■■■
15:40:46    8  1   9    ■■■■■■■■■
15:40:51    5  1   6    ■■■■■■
15:40:56    4  1   5    ■■■■■
15:41:01    4  1   5    ■■■■■
15:41:06    4  1   5    ■■■■■
15:41:11    4  1   5    ■■■■■
15:41:16    2  1   3    ■■■
15:41:21    2  1   3    ■■■
15:41:26    0  1   1    ■
15:41:31    0  1   1    ■
15:41:36    0  1   1    ■
15:41:41    0  1   1    ■
15:41:46    0  1   1    ■
15:41:51    0  1   1    ■
15:41:56    0  1   1    ■
15:42:01    0  1   1    ■
15:42:06    0  1   1    ■
15:42:11    0  1   1    ■
15:42:16    0  1   1    ■
15:42:21    0  1   1    ■
15:42:26    0  1   1    ■
15:42:31    0  1   1    ■
15:42:36    0  1   1    ■
15:42:41    0  1   1    ■
15:42:46    0  1   1    ■
```

**Concurrency summary**

- **Initialization (0–6 s):** 0 task containers; AM is bootstrapping.

- **Map ramp-up (6–88 s):** steady **10 concurrent mappers** (max capacity given the 10-container allocation).

- **First wave + speculation (88–153 s):** drops briefly to **9** after m_000009 finishes, then back to **10** when speculative m_000006_1 launches; RM never schedules the reducer during this window because it keeps issuing map-speculation containers first.

- **Overlap / shuffle (153–158 s):** **10** concurrent containers — 8 maps + speculative backup + the reducer. Peak running-attempt count for the whole job.

- **Map wind-down (158–210 s):** counts drop as maps finish: 10 → 9 → 6 → 5 → 3 → 2 → 1. After m_000005 finishes at 15:40:28, m_000007_1 speculative starts, bumping back to 10.

- **Solo reduce (210–290 s):** only **1** container running (the reducer) — pure reduce/merge phase.

- **Commit / cleanup (290–292 s):** 0 task containers; AM performing bookkeeping.

**Peak parallelism:** 10 containers running simultaneously, sustained for most of the job — the cluster was fully utilized for the mapper slots available.

---

## 6. Per-Task Durations

```
Task        Attempt Start         End             Duration  Status
------------------------------------------------------------------------
m_000000    _0      15:38:02.050  15:40:34.116      152.1s  SUCCEEDED
m_000001    _0      15:38:02.042  15:40:50.092      168.1s  SUCCEEDED
m_000002    _0      15:38:02.041  15:40:50.266      168.2s  SUCCEEDED
m_000003    _0      15:38:02.045  15:40:32.079      150.0s  SUCCEEDED
m_000004    _0      15:38:02.047  15:40:50.566      168.5s  SUCCEEDED
m_000005    _0      15:38:02.052  15:40:28.630      146.6s  SUCCEEDED
m_000006    _0      15:38:02.033  15:41:25.713      203.7s  SUCCEEDED
m_000006    _1      15:39:25.757  15:41:25.731      120.0s  KILLED (speculative)
m_000007    _0      15:38:02.036  15:41:12.316      190.3s  SUCCEEDED
m_000007    _1      15:40:33.865  15:41:12.341       38.5s  KILLED (speculative)
m_000008    _0      15:38:02.034  15:40:52.232      170.2s  SUCCEEDED
m_000009    _0      15:38:02.035  15:39:24.265       82.2s  SUCCEEDED
r_000000    _0      15:40:29.854  15:42:46.420      136.6s  SUCCEEDED
```

## 7. Conclusions & Observations

- **Healthy, successful job** with no failed tasks and a total wall-clock of ~4 m 51 s for 1.2 GB / 10 splits → single reducer PageRank iteration.

- **Straggler mitigation worked but was over-eager.** Two speculative backups were launched for slow mappers, yet both originals beat their backups. Total waste: one container for ~120 s and another for ~38 s. Consider raising `mapreduce.job.speculative.speculativecap` or the `slowtaskthreshold` if this pattern is common.

- **Slow-start for reduces** was set to a very low threshold (≈5%, 1 map). This caused the reducer to start copying early, which is good for overlap, but on this small 1-reducer job the reducer then sat ~56 s in shuffle waiting for the tail mappers.

- **One HDFS blip** (bad datanode 10.86.164.9) during job-history writing caused a pipeline recovery but was fully transparent to the job — HDFS HA / client retry behaved as designed.

- **Two benign `Could not delete` warnings** from `FileOutputCommitter` for killed speculative attempts. These are not fatal but indicate a small race between the AM killing the attempt and the committer cleaning its `_temporary` output; they can be ignored.

- **Data locality was excellent**: all 10 initial map containers were assigned **host-local** (HostLocal:10, RackLocal:0 in the final scheduler stats).
