# Hadoop MapReduce v2 (YARN) — Job Completion Summary

> Generated from `mapreduce.log` (MRAppMaster log for the job). All timestamps are in the local clock of the host running the AM (`MSRA-SA-41.fareast.corp.microsoft.com`).

---

## 1. Job Identification

| Field | Value |
|---|---|
| **Job ID** | `job_1445062781478_0011` |
| **Application ID** | `application_1445062781478_0011` |
| **Application Attempt ID** | `appattempt_1445062781478_0011_000001` (the first/only AM attempt) |
| **Job Name** | `pagerank` (extracted from the job-history filename: `…-msrabi-pagerank-1445067766465-10-1-SUCCEEDED-default-…jhist`) |
| **Job Type** | Standard Java MapReduce (new API / `mapred` new API committer), **non-uberized**, multi-container job |
| **Submitter** | User `msrabi` |
| **Cluster Timestamp** | 1445062781478 (used to construct job/application IDs) |

---

## 2. Job Configuration

| Configuration Item | Value |
|---|---|
| **OutputCommitter class** | `org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter` (using the **new API committer** — *"Using mapred newApiCommitter"*) |
| **File System (default FS)** | `hdfs://msra-sa-41:9000` (HDFS NameNode at `MSRA-SA-41:9000`) |
| **ResourceManager** | `MSRA-SA-41/10.190.173.170:8030` |
| **MRAppMaster RPC port** | 47468 (client service on `MSRA-SA-41:47455`) |
| **MRAppMaster Web UI port** | 47462 (Jetty `/mapreduce` webapp) |
| **YARN queue** | `default` |
| **Container memory per task** | Map: `<memory:1024, vCores:1>` · Reduce: `<memory:1024, vCores:1>` |
| **Max cluster container capability** | `<memory:8192, vCores:32>` |
| **Node blacklisting** | enabled (`nodeBlacklistingEnabled:true`), `maxTaskFailuresPerNode=3`, `blacklistDisablePercent=33` |
| **Uber mode** | Disabled — *"Not uberizing … because: not enabled; too many maps; too much input;"* → ran as a normal multi-container job |
| **Input size** | `1,256,521,728 bytes` (~1.17 GiB) |
| **Number of input splits** | 10 (→ 10 map tasks) |
| **Number of reduces** | 1 |
| **Shuffle port on NodeManagers** | 13562 (per-container shuffle handler) |
| **Known NodeManagers** | 5 (incl. `MSRA-SA-39` and `MSRA-SA-41`; both on `/default-rack`) |
| **Container launcher thread-pool upper limit** | 500 |
| **Job JAR (staging)** | `hdfs://msra-sa-41:9000/tmp/hadoop-yarn/staging/msrabi/.staging/job_1445062781478_0011/job.jar` |
| **Job conf file (staging)** | `…/.staging/job_1445062781478_0011/job.xml` |
| **Job history file (final)** | `hdfs://msra-sa-41:9000/tmp/hadoop-yarn/staging/history/done_intermediate/msrabi/job_1445062781478_0011-1445067474313-msrabi-pagerank-1445067766465-10-1-SUCCEEDED-default-1445067479468.jhist` |
| **History UI URL** | `http://MSRA-SA-41.fareast.corp.microsoft.com:19888/jobhistory/job/job_1445062781478_0011` |
| **Output path (inferred)** | `hdfs://msra-sa-41:9000/pageout/out1/` (from temp-path warnings emitted by `FileOutputCommitter`) |
| **Hadoop version** | Hadoop 2.6.0-SNAPSHOT (`hadoop-yarn-common-2.6.0-SNAPSHOT.jar`, `hadoop-mapreduce-client-*`) |
| **Authentication** | SIMPLE (no Kerberos — logged `auth:SIMPLE`) |
| **Timeline server publishing** | Not enabled (*"Emitting job history data to the timeline server is not enabled"*) |

---

## 3. Task Summary

### 3.1 Tasks

| Type | Total Tasks Planned | Launched Attempts | **Succeeded** | Failed | Killed (speculative backups) |
|---|---|---|---|---|---|
| Map (`m`) | **10** (splits 000000–000009) | 12 (10 original + 2 speculative backups for `m_000006` and `m_000007`) | **10 / 10** | 0 | 2 (`_m_000006_1`, `_m_000007_1` — killed by AM when the original attempt won the race) |
| Reduce (`r`) | **1** (`r_000000`) | 1 | **1 / 1** | 0 | 0 |
| **Total** | **11** | 13 | **11 / 11** ✅ | 0 | 2 (speculative duplicates only) |

All 10 map tasks and the single reduce task completed successfully; zero task failures. The two "killed" attempts are normal **speculative-execution backups** that were aborted once the other attempt of the same task finished first.

### 3.2 State-machine transitions observed for tasks

- **Job state transitions**: `NEW` → `INITED` → `SETUP` → `RUNNING` → `COMMITTING` → **`SUCCEEDED`**
- **Successful task-attempt transitions**: `NEW` → `UNASSIGNED` → `ASSIGNED` → `RUNNING` → `SUCCESS_CONTAINER_CLEANUP` → **`SUCCEEDED`**
- **Killed (speculative) attempt transitions**: … → `RUNNING` → `KILL_CONTAINER_CLEANUP` → `KILL_TASK_CLEANUP` → **`KILLED`**
- **Reduce attempt transition** was special: … → `RUNNING` → **`COMMIT_PENDING`** → `SUCCESS_CONTAINER_CLEANUP` → `SUCCEEDED` (commit was authorized by the AM: *"given a go for committing the task output"*).

---

## 4. Task Completion Timeline

The table below lists **the time each task transitioned to `SUCCEEDED`** (i.e. the task-level `Task Transitioned from RUNNING to SUCCEEDED` event), in chronological order. `Δ from job start` is relative to the moment the job entered `RUNNING` (15:37:59.481).

| # | Timestamp | Elapsed since job RUNNING | Task ID | Notes |
|---|---|---|---|---|
| 1 | **15:39:24.273** | 1 m 24.8 s | `task_…_m_000009` | Fastest map (highest progress ~0.86 → done first) |
| 2 | **15:40:28.631** | 2 m 29.2 s | `task_…_m_000005` |  |
| 3 | **15:40:32.079** | 2 m 32.6 s | `task_…_m_000003` |  |
| 4 | **15:40:34.116** | 2 m 34.6 s | `task_…_m_000000` |  |
| 5 | **15:40:50.093** | 2 m 50.6 s | `task_…_m_000001` |  |
| 6 | **15:40:50.267** | 2 m 50.8 s | `task_…_m_000002` |  |
| 7 | **15:40:50.567** | 2 m 51.1 s | `task_…_m_000004` |  |
| 8 | **15:40:52.233** | 2 m 52.8 s | `task_…_m_000008` |  |
| 9 | **15:41:12.317** | 3 m 12.8 s | `task_…_m_000007` | Beat its speculative backup `_m_000007_1` → backup killed |
| 10 | **15:41:25.714** | 3 m 26.2 s | `task_…_m_000006` | Beat its speculative backup `_m_000006_1` → backup killed; **last map** |
| — | 15:41:25.714 | 3 m 26.2 s | *all 10 maps done* | Shuffle/sort phase continues on the reducer |
| 11 | **15:42:46.421** | 4 m 46.9 s | `task_…_r_000000` | **Only reduce** (commit at 15:42:46.367, success at 46.421) |

### Visual timeline (relative to job RUNNING)

```
0:00  ──── JOB RUNNING starts (15:37:59.481) ──── map containers are launched
0:02.7     10 map containers all RUNNING
1:24.8  ██▌                                                   m_000009 SUCCEEDED (1st)
2:29.2  █████████████████████████▊                             m_000005
2:32.6  ██████████████████████████▍                           m_000003
2:34.6  ██████████████████████████▊                           m_000000
2:50.6  █████████████████████████████████▎                    m_000001
2:50.8  █████████████████████████████████▎                    m_000002
2:51.1  █████████████████████████████████▍                    m_000004
2:52.8  █████████████████████████████████▌                    m_000008
3:12.8  █████████████████████████████████████████▎            m_000007 (backup _1 killed)
3:26.2  ███████████████████████████████████████████▏          m_000006 (backup _1 killed) — LAST MAP
           │                                                  
           │  ← reduce was launched at ~15:40:29 (8th map done), slow-start threshold hit
           │  ← shuffle+merge+reduce runs ~1 m 44 s after last map
4:46.9  ████████████████████████████████████████████████████  r_000000 SUCCEEDED
4:47.0  ──── JOB COMMITTING → SUCCEEDED (15:42:46.468) ────
```

### Speculation detail
Speculative (backup) attempts were launched by `DefaultSpeculator` for the two slowest maps:

| Backup attempt | Launched (RUNNING) | Killed by AM | Reason |
|---|---|---|---|
| `attempt_…_m_000006_1` | 15:39:25.757 | 15:41:25.731 | Container killed after original `_0` finished at 15:41:25.714 |
| `attempt_…_m_000007_1` | 15:40:33.865 | 15:41:12.341 | Container killed after original `_0` finished at 15:41:12.317 |

Both exited with exit code **137** (SIGKILL), which is expected for AM-requested container termination — not a failure.

---

## 5. Job Duration

Key timing checkpoints (from the MRAppMaster log):

| Milestone | Timestamp |
|---|---|
| MRAppMaster process created | 2015-10-17 **15:37:56.547** |
| Job `NEW → INITED` | 15:37:58.569 |
| Job `INITED → SETUP` | 15:37:59.472 |
| Job `SETUP → RUNNING` | **15:37:59.481** ← start of actual work |
| First map task starts RUNNING (`m_000006_0`) | 15:38:02.033 |
| All 10 maps originally launched | by 15:38:02.053 |
| First map finished (`m_000009`) | 15:39:24.273 |
| Slow-start threshold met → reduce scheduled | shortly after the 5th/8th-map completion |
| Reduce starts RUNNING (`r_000000_0`) | 15:40:29.854 |
| Last map finished (`m_000006`) | 15:41:25.714 |
| Reduce enters COMMIT_PENDING | 15:42:46.367 |
| Reduce SUCCEEDED / `Num completed Tasks: 11` | 15:42:46.421 |
| Job `RUNNING → COMMITTING` | 15:42:46.422 |
| **Job `COMMITTING → SUCCEEDED`** | **15:42:46.468** ← job done |
| AM notifies RM "finishing cleanly, last retry" | 15:42:46.469 |
| AM unregisters from RM | 15:42:47.696 |
| Staging directory deleted / AM shuts down | 15:42:47.708 |

### Computed durations

| Phase | Duration |
|---|---|
| **Total job runtime (RUNNING → SUCCEEDED)** | **4 minutes 46.987 seconds ≈ 4 m 47 s** (15:37:59.481 → 15:42:46.468) |
| AM startup overhead (MRAppMaster created → job RUNNING) | ~2.9 s |
| Map phase (first map RUNNING → last map SUCCEEDED) | **3 m 23.7 s** (15:38:02.03 → 15:41:25.71) |
| First map completion lag (map start → first map done) | **1 m 22.2 s** |
| Shuffle/copy overlap (reduce launch → last map done) | 55.9 s |
| Reduce phase (reduce RUNNING → reduce SUCCEEDED) | **2 m 16.6 s** (15:40:29.85 → 15:42:46.42) |
| Post-map reduce-only work (after last map → reduce done) | ~1 m 20.7 s |
| Commit phase (`COMMITTING → SUCCEEDED`) | ~46 ms (negligible; the output was already in `_temporary`) |
| AM teardown (SUCCEEDED → IPC server stop) | ~1.24 s |
| End-to-end wall time (MRAppMaster created → IPC stop) | **4 m 51.2 s** |

---

## 6. Final Status

- **Job result: ✅ SUCCESS (SUCCEEDED)**
  - Final job state transition logged at 15:42:46,468:
    `job_1445062781478_0011Job Transitioned from COMMITTING to SUCCEEDED`
  - MRAppMaster: *"We are finishing cleanly so this is the last retry"*
  - RM diagnostics were empty (`Setting job diagnostics to `), i.e. no error message.
  - Final container-allocation stats: `CompletedMaps:10 CompletedReds:0 … ContAlloc:13 ContRel:0 HostLocal:12 RackLocal:0` — no container releases/failures.
  - Job history filename suffix: `…-SUCCEEDED-default-…jhist`, confirming success.

### Notable non-fatal warnings during the run

1. **`Could not delete hdfs://msra-sa-41:9000/pageout/out1/_temporary/1/_temporary/attempt_…_m_000006_1`** (and similarly for `_m_000007_1`) — `FileOutputCommitter` could not clean up the speculative-attempt temp directories of the two killed backup attempts. This is a benign HDFS-side deletion race/warning and did **not** affect the final result.
2. **`An existing connection was forcibly closed by the remote host`** (two occurrences at `Socket Reader #1 for port 47468`) — container JVMs closing their IPC connections after being killed by the AM (exit code 137). Expected during speculative-kill cleanup; not an error.
3. Speculative execution triggered for `m_000006` and `m_000007` — both slower than the median; original attempts finished first and backups were killed cleanly.

### Final job state sequence (summary)

```
NEW  →  INITED  →  SETUP  →  RUNNING  →  COMMITTING  →  SUCCEEDED   ✅
```

The pagerank job (`job_1445062781478_0011`) therefore completed successfully — all 10 map tasks and the 1 reduce task finished, the output was committed to `hdfs://msra-sa-41:9000/pageout/out1/`, the job-history and summary files were moved to the `done_intermediate` history directory, the staging directory was deleted, and the MRAppMaster unregistered cleanly from the ResourceManager.
