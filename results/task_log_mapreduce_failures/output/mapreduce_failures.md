# Hadoop MapReduce Failure & Anomaly Analysis Report

**Log file:** `mapreduce.log`
**Job analyzed:** `job_1445062781478_0011` (job name visible in history path: `msrabi-pagerank`)
**Application Master attempt:** `appattempt_1445062781478_0011_000001`
**Execution window:** 2015-10-17 15:37:56 → 15:42:47 (≈ 4 min 50 s)
**Cluster / Filesystem:** YARN cluster; NameNode `hdfs://msra-sa-41:9000`; RM/AM host `MSRA-SA-41.fareast.corp.microsoft.com/10.190.173.170`
**Job profile:** 10 map tasks + 1 reduce task; input ≈ 1,256,521,728 bytes (≈ 1.17 GiB); non-uberized, multi-container

---

## 0. Executive Summary

The PageRank job **completed successfully** — the JobHistory file was renamed with a `…-SUCCEEDED-default-…` suffix and the AM log ends with `job_1445062781478_0011Job Transitioned from COMMITTING to SUCCEEDED`. All 10 mappers and the single reducer finished on their **first (attempt `_0`)** attempt. The WARN/ERROR events observed during the run were all **recovered or non-fatal**:

- A single HDFS write-pipeline failure on a remote DataNode was automatically recovered by the DFS client.
- Hadoop's **speculative execution** launched backup attempts for two slow mappers (`m_000006` and `m_000007`); those backup attempts (`_1`) were killed with exit code 137 as soon as the original attempt won — this is **expected behavior, not a failure**.
- The FileOutputCommitter emitted "Could not delete" warnings for the two killed speculative attempt's temp directories (cosmetic cleanup issue, not data loss).
- After every successful task the container is reported "Container killed by the ApplicationMaster… Exit code is 137". This is the **normal post-success container-reap message** (AM asks NM to clean up the JVM), not a task failure.

Net impact on job result: **none**.

---

## 1. Error and Warning Entries (full context)

There are **no `ERROR`-level** log entries in this file. All anomalies are `WARN`-level. All stack-trace-bearing IPC "An existing connection was forcibly closed" messages are emitted at `INFO` level by the Socket Reader during normal container teardown.

### 1.1 WARN #1 — HDFS write-pipeline failure while writing job history (jhist)

**Lines 943–946** (timestamp 15:40:45):
```
2015-10-17 15:40:45,929 WARN [ResponseProcessor for block BP-1347369012-10.190.173.170-1444972147527:blk_1073742514_1708]
    org.apache.hadoop.hdfs.DFSClient: DFSOutputStream ResponseProcessor exception
    for block BP-1347369012-10.190.173.170-1444972147527:blk_1073742514_1708
java.io.IOException: Bad response ERROR for block BP-1347369012-10.190.173.170-1444972147527:blk_1073742514_1708
    from datanode 10.86.164.9:50010
    at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer$ResponseProcessor.run(DFSOutputStream.java:898)

2015-10-17 15:40:45,935 WARN [DataStreamer for file
    /tmp/hadoop-yarn/staging/msrabi/.staging/job_1445062781478_0011/job_1445062781478_0011_1.jhist
    block BP-1347369012-10.190.173.170-1444972147527:blk_1073742514_1708]
    org.apache.hadoop.hdfs.DFSClient: Error Recovery for block BP-1347369012-10.190.173.170-1444972147527:blk_1073742514_1708
    in pipeline 10.190.173.170:50010, 10.86.164.9:50010: bad datanode 10.86.164.9:50010
```

### 1.2 WARN #2 — Could not delete speculative-attempt temp output for map 7

**Line 1105** (timestamp 15:41:12), immediately after `m_000007_0` succeeded and a kill was issued to the backup `m_000007_1`:
```
2015-10-17 15:41:12,339 WARN [CommitterEvent Processor #1]
    org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter:
    Could not delete hdfs://msra-sa-41:9000/pageout/out1/_temporary/1/_temporary/attempt_1445062781478_0011_m_000007_1
```

### 1.3 WARN #3 — Could not delete speculative-attempt temp output for map 6

**Line 1176** (timestamp 15:41:25), immediately after `m_000006_0` succeeded and a kill was issued to the backup `m_000006_1`:
```
2015-10-17 15:41:25,730 WARN [CommitterEvent Processor #2]
    org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter:
    Could not delete hdfs://msra-sa-41:9000/pageout/out1/_temporary/1/_temporary/attempt_1445062781478_0011_m_000006_1
```

### 1.4 INFO-level IPC stack traces (often misread as errors)

Two identical stack traces appear on the IPC server's Socket Reader thread during container cleanup (lines ~1137 and ~1178). They are emitted at `INFO`, not `WARN`/`ERROR`:
```
java.io.IOException: An existing connection was forcibly closed by the remote host
    at sun.nio.ch.SocketDispatcher.read0(Native Method)
    ...
    at org.apache.hadoop.ipc.Server$Listener$Reader.run(Server.java:607)
```
These happen precisely when the AM kills the child JVM of a completed/speculative container and the child's IPC socket is reset — harmless and expected.

---

## 2. Task Retries / Multiple Attempts

> **Important:** The line "Container killed by the ApplicationMaster. Container killed on request. Exit code is 137. Container exited with a non-zero exit code 137" is logged for **every** container — including the ones that finished SUCCESSFULLY — because that is how Hadoop 2.x reaps the container JVM after a task commits. It is **not** a failure signal. The signal that distinguishes failure vs. cleanup is whether the `TaskAttemptImpl` transitions to `FAILED`/`KILLED` **instead of** `SUCCEEDED`.

### 2.1 Genuine retry/speculative attempts (attempt number > 0)

| Task ID                    | Attempts launched              | Outcome                                                                 |
|----------------------------|--------------------------------|-------------------------------------------------------------------------|
| `task_..._m_000000` – `m_000005`, `m_000008`, `m_000009` | `_0` only | `_0` SUCCEEDED; no speculation; container reaped with exit 137 after success |
| `task_..._m_000006`        | `_0` (original) and `_1` (speculative) | `_0` SUCCEEDED at 15:41:25 after finishing progress 1.0; `_1` launched at 15:39:24 by the speculator (log line: *"DefaultSpeculator.addSpeculativeAttempt -- we are speculating task_...m_000006"*), ran on container `container_..._000012` (MSRA-SA-39:49130), reached only ~0.62 progress, and was KILLED (exit 137) at 15:41:25 as a duplicate |
| `task_..._m_000007`        | `_0` (original) and `_1` (speculative) | `_0` SUCCEEDED at 15:41:12; `_1` launched at 15:39:39 by the speculator, ran on container `container_..._000014` (MSRA-SA-41:42313), reached only ~0.038 progress, and was KILLED (exit 137) at 15:41:13 as a duplicate |
| `task_..._r_000000`        | `_0` only                      | `_0` SUCCEEDED; progress 0.0 → 1.0 between 15:40:33 and 15:42:46; reduce committed cleanly |

The exact speculation triggers are visible in the log:

- Line 541 (15:39:24): `DefaultSpeculator.addSpeculativeAttempt -- we are speculating task_..._m_000006`
- Line 623 (15:39:39): `DefaultSpeculator.addSpeculativeAttempt -- we are speculating task_..._m_000007`

The corresponding "kill the loser" actions are explicit:

- Line 1102 (15:41:12): `TaskImpl: Issuing kill to other attempt attempt_..._m_000007_1`
- Line 1171 (15:41:25): `TaskImpl: Issuing kill to other attempt attempt_..._m_000006_1`

### 2.2 Tasks that did **NOT** actually retry

The diagnostics for `m_000000_0`, `m_000001_0`, `m_000002_0`, `m_000003_0`, `m_000004_0`, `m_000005_0`, `m_000008_0`, `m_000009_0` and the two speculative `_1` attempts all show "Container killed by the AM… exit 137", but each `_0` mapper had already transitioned to `SUCCEEDED` *before* that diagnostics line was printed (e.g., `m_000009_0` SUCCEEDED at line 535; diagnostics for the same attempt at line 557). These are **post-success container-reap messages, not failures**.

---

## 3. Root Cause Analysis

### 3.1 HDFS pipeline error on DataNode `10.86.164.9:50010` (WARN #1)

*What happened:* While the AM was streaming the job history file (`job_1445062781478_0011_1.jhist`) into the staging directory, the second DataNode in the 2-node pipeline (`10.86.164.9:50010`) returned a bad ACK (`Bad response ERROR`). The DFS `DataStreamer` immediately ran `Error Recovery for block … bad datanode 10.86.164.9:50010`, removed that DN from the pipeline, and continued writing.

*Likely root causes (most probable → least):*

1. **Transient DataNode or network fault on 10.86.164.9** — note that this IP is on a different subnet from the cluster nodes (which are `10.190.173.x` / `MSRA-SA-*`); the block was likely placed on a "remote-rack" DN that momentarily dropped or NAK'd the write (disk error, GC pause, DN restart, network hiccup across subnets).
2. **Datante-side disk I/O error** while writing the block replica, causing the DN to send an ERROR ack.
3. Less likely: a checksum mismatch on the wire (would have produced a different message).

*Why it wasn't fatal:* HDFS's built-in pipeline recovery rebuilt the write pipeline excluding the bad DN, and the job history file was later successfully copied/moved to `history/done_intermediate/…-SUCCEEDED-…jhist` (lines ~1254–1279), proving the file landed safely.

### 3.2 Speculative execution duplicate-kills (WARN #2, #3; exit-137 for `_1` attempts)

*What happened:* Two of the ten mappers (`m_000006` and `m_000007`) progressed noticeably slower than the median. The `DefaultSpeculator` (checking every 15 s — *"We launched 1 speculations. Sleeping 15000 milliseconds."*) launched backup attempts on different nodes. The originals eventually won the race; the AM then issued `TASK_ABORT` to the backups and asked the `FileOutputCommitter` to delete the backup's temporary output directory under `_temporary/1/_temporary/attempt_…_1/`. The delete call failed, producing WARN #2 and #3.

*Likely root causes:*

- **Stragglers cause speculation** (normal): PageRank per-iteration record distribution is naturally skewed; some splits may contain denser rows/longer adjacency lists, so `m_000006` and `m_000007` ran longer. The speculative backup `_1` for `m_000006` only reached ~0.62 before the original finished; `m_000007_1` barely got to ~0.038. Both launches therefore wasted cluster resources.
- **"Could not delete" warnings** are thrown by `FileOutputCommitter.abortTask()` when the HDFS `delete()` on the attempt's temp directory returns `false` or throws. Typical reasons:
  1. The backup container had only just been killed and still held a lease/handle on the part files it had created (delete-before-close race), so the NameNode returned false transiently.
  2. The attempt wrote no files at all (it was killed before producing any output) and the directory did not exist; in older Hadoop 2.6.0 this path also yields a warn-level message instead of a debug.
  3. A transient HDFS RPC failure during the delete call (no stack trace printed here, so more likely #1 or #2).

*Why it wasn't fatal:* Once one attempt commits, the other attempt's temp output is irrelevant. Orphaned temp files are cleaned by subsequent runs (and by the `StagingDir` cleaner / `OutputCommitter.cleanupJob`). The job committed successfully and produced output in `/pageout/out1/`.

### 3.3 INFO-level "connection forcibly closed by remote host"

*Root cause:* Asynchronous socket RST from a child container JVM that the AM had just asked the NodeManager to kill (via `StopContainers`). By the time the AM's IPC `Socket Reader` next tries to read from the child's RPC socket, the child process has already been SIGKILL'd and the OS sends RST. Purely a cleanup-ordering artifact; no data loss, no retry.

### 3.4 Exit-code-137 "Container killed by the ApplicationMaster"

*Root cause (for every occurrence):* Exit code **137 = 128 + 9 (SIGKILL)** on Linux. In Hadoop YARN this happens in two very different situations:

1. **Normal container reaping (the vast majority in this log):** after a task attempt reports success and the AM commits it, the AM explicitly asks the NodeManager to kill/clean up the container JVM. The NM sends SIGKILL → exit 137 → the AM logs the diagnostic message. This is **not a failure**; every successful task attempt in this job emits this.
2. **Speculative backup being killed (m_000006_1, m_000007_1):** same mechanism, but the kill is issued because a sibling attempt already won, so the backup is redundant.

Crucially, **no attempt was killed due to memory oversubscription (`Container killed by YARN for exceeding memory limits`)**, preemption, or hardware failure in this log.

---

## 4. I/O Errors and Network-Related Failures

| # | Timestamp | Type | Endpoint(s) | Description | Recovery / Outcome |
|---|-----------|------|-------------|-------------|--------------------|
| 1 | 15:40:45 | `java.io.IOException: Bad response ERROR for block … from datanode 10.86.164.9:50010` | AM (10.190.173.170) → DN 10.86.164.9:50010 (HDFS data-transfer port) | Writing the `.jhist` block `blk_1073742514_1708`; DN in pipeline returned ERROR ack. | DFS client invoked `Error Recovery for block … bad datanode 10.86.164.9:50010`, removed the bad DN from the pipeline and continued. Block persisted; jhist later moved to `done_intermediate`. |
| 2 | 15:41:12 | `java.io.IOException: An existing connection was forcibly closed by the remote host` (INFO) | IPC client 10.190.173.170 → AM port 47468 | Socket RST after `m_000007_0`'s container was killed. | Benign; Socket Reader thread continued serving other clients. |
| 3 | 15:41:25 | Same as #2 (INFO) | IPC client 172.22.149.145 → AM port 47468 | Socket RST after `m_000006` containers (both `_0` and `_1`) were killed. | Benign. |
| 4 | 15:41:12 & 15:41:25 | `FileOutputCommitter: Could not delete hdfs://msra-sa-41:9000/pageout/out1/_temporary/1/_temporary/attempt_..._{000007,000006}_1` | AM → NameNode (HDFS RPC) | Delete of speculative attempt's temp directory failed. | Warning only; job committed successfully; orphan temp files can be removed later. |

There are **no** `java.net.ConnectException`, `SocketTimeoutException`, "Connection refused", "Failed to fetch shuffle", "Too many fetch-failures", "Lost task tracker / node", or "No route to host" errors in the log. The Shuffle phase for the single reducer (`r_000000_0`) proceeded without incident (reducer started pulling map outputs once `slow start` threshold hit, and the 10 `MapCompletionEvents` were served successfully by the AM's `TaskAttemptListenerImpl`).

---

## 5. Impact Assessment

| Concern | Impact on job result |
|---|---|
| **Job final state** | ✅ **`SUCCEEDED`** — transitioned COMMITTING → SUCCEEDED at 15:42:46; job history renamed with `-SUCCEEDED-default-` suffix. |
| **All 10 map tasks** | ✅ All SUCCEEDED on their `_0` attempt. Only two mappers launched speculative backups; backups were cleanly killed before committing; no input split was processed more than once for the final output. |
| **Reduce task `r_000000`** | ✅ Single reducer `_0` SUCCEEDED; progress 1.0 reported; output committed. |
| **HDFS write pipeline failure (WARN #1)** | ✅ Transparent recovery. Job history file successfully copied to `history/done_intermediate/msrabi/…SUCCEEDED….jhist`. No data loss. |
| **Could-not-delete temp dirs (WARN #2, #3)** | ⚠️ Cosmetic. Orphaned `_temporary/1/_temporary/attempt_..._{000006,000007}_1/` directories may remain under `/pageout/out1/`. They are **not** consumed by downstream jobs (FileOutputFormat reads only `part-*` at the job output root, not `_temporary/`), so they do not affect correctness, but they waste HDFS space until cleaned. |
| **Exit-code-137 container kills** | ✅ All were normal reaps (post-success cleanup or speculative-duplicate kill). No evidence of OOM, NodeManager eviction, or preemption. |
| **Performance / resource waste** | ⚠️ Two speculative containers ran for ~93 s and ~60 s respectively without producing useful output. That added roughly (CPU * 2 containers) of wasted capacity for this job, but did not delay the job — the winning attempts (`_0`) were the ones already running, and the reducer did not start until all maps anyway. |
| **Output correctness** | ✅ No reason to suspect corruption. The successful attempt for each mapper was the one that committed; speculative backups were aborted before they could write to the final output path. |

**Bottom line:** the job's result is correct and complete. The warnings are infrastructure noise / expected behavior under speculative execution.

---

## 6. Failure-Prevention Recommendations

### 6.1 Address the HDFS pipeline instability
1. **Investigate DataNode `10.86.164.9`.** Check its DN log (`hadoop-hdfs-datanode-*.log`) for the same timestamp (2015-10-17 15:40:45) for disk errors (`IOException: Input/output error`, "No space left on device", "volume failure"), GC pauses, or network errors. If disks are healthy, verify network connectivity/SLA between the `10.190.173.x` cluster subnet and the `10.86.164.x` subnet (MTU, firewall idle-timeouts, link errors).
2. **Tune HDFS write pipeline resiliency:**
   - Keep `dfs.client.block.write.replace-datanode-on-failure.policy=DEFAULT` and `dfs.client.block.write.replace-datanode-on-failure.enable=true` (defaults in 2.6+) — already helped here.
   - For cross-subnet/cross-rack pipelines, ensure `dfs.client.use.datanode.hostname=true` when IP routability is asymmetric, to avoid "bad datanode" false positives from DNS/IP mismatches.
   - Increase `dfs.replication` (from the default 3? — confirm) so that losing one DN in the pipeline leaves at least 2 replicas and the client can find a replacement without going to a remote rack.

### 6.2 Reduce speculative-execution noise (and wasted resources)
3. **Tune the speculator for the PageRank workload.** PageRank maps are often skewed (dense rows = long tails), and speculative backups on small clusters (only 5 NodeManagers listed in the log — `knownNMs=5`) usually just steal slots from reduce or from other maps. Consider:
   - Raising `mapreduce.job.speculative.speculativecap` (default `0.1` → `0.2` or higher) to require a larger performance gap before speculating.
   - Raising `mapreduce.job.speculative.slowtaskthreshold` so only significantly slower tasks trigger backups.
   - Alternatively, **turn off** map-side speculation for PageRank-style jobs with `mapreduce.map.speculative=false`; keep `mapreduce.reduce.speculative=true` where stragglers are more costly.
4. **Fix the "Could not delete" WARN:** This is a known cosmetic issue in Hadoop 2.6.0 FileOutputCommitter. Options:
   - Upgrade to a Hadoop release where `MAPREDUCE-6212` / `MAPREDUCE-6424` (FileOutputCommitter abortTask logging noise) are fixed.
   - As a workaround, set `mapreduce.fileoutputcommitter.algorithm.version=2` (introduced in 2.7, but Hadoop 2.6 here so not available) or add a post-job step that runs `hadoop fs -rm -r -skipTrash /pageout/out1/_temporary` to clean leftover attempt dirs.

### 6.3 Operational best practices
5. **Distinguish benign exit-137 messages in monitoring.** Add an alert rule that only flags containers killed *before* their attempt reached `SUCCESS_CONTAINER_CLEANUP` (i.e., where the attempt transitioned to FAILED/KILLED without a preceding SUCCEEDED transition). Today every successful container looks like a "kill" in the diagnostic text, which desensitizes operators.
6. **Track slow-node patterns.** Mention of `maxTaskFailuresPerNode is 3` and `blacklistDisablePercent is 33` is good (node-blacklisting is enabled), but watch whether `10.86.164.9` or other DNs show up repeatedly in "bad datanode" messages; if so, proactively decommission rather than wait for three failed tasks per node.
7. **Output-path cleanup job.** Add a routine (e.g., a periodic Oozie coordinator or a post-job step) that removes `_temporary` and `_logs` directories under `/pageout/out*/` to prevent the leftover speculative-attempt directories from accumulating across runs.
8. **Container sizing.** Although there was no OOM kill in this run, note that the maps were processing ~120 MB splits on a queue with `maxContainerCapability` of 8 GiB / 32 vcores. If PageRank iterations grow, monitor `Container killed by YARN for exceeding memory limits` — the current log has none, but this is the dominant failure mode for iterative graph jobs.

---

## Appendix A — Per-task outcome matrix

| Task | Original attempt | Backup (spec) attempt | Speculated? | Final result |
|------|------------------|-----------------------|-------------|--------------|
| `m_000000` | `_0` SUCCEEDED 15:40:34 | — | No | ✅ |
| `m_000001` | `_0` SUCCEEDED 15:40:50 | — | No | ✅ |
| `m_000002` | `_0` SUCCEEDED 15:40:50 | — | No | ✅ |
| `m_000003` | `_0` SUCCEEDED 15:40:32 | — | No | ✅ |
| `m_000004` | `_0` SUCCEEDED 15:40:50 | — | No | ✅ |
| `m_000005` | `_0` SUCCEEDED 15:40:28 | — | No | ✅ |
| `m_000006` | `_0` SUCCEEDED 15:41:25 | `_1` KILLED 15:41:25 (exit 137, duplicate) | **Yes** (launched 15:39:24) | ✅ (with WARN on temp delete) |
| `m_000007` | `_0` SUCCEEDED 15:41:12 | `_1` KILLED 15:41:13 (exit 137, duplicate) | **Yes** (launched 15:39:39) | ✅ (with WARN on temp delete) |
| `m_000008` | `_0` SUCCEEDED 15:40:52 | — | No | ✅ |
| `m_000009` | `_0` SUCCEEDED 15:39:24 | — | No | ✅ (first to finish) |
| `r_000000` | `_0` SUCCEEDED 15:42:46 | — | No | ✅ |
