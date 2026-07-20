# Hadoop MapReduce 作业时间线报告

**作业 ID：** `job_1445062781478_0011`
**作业名称：** `pagerank`（10 个 mapper × 1 个 reducer，~1.2 GB 输入）
**用户 / 队列：** `msrabi` / `default`
**集群节点（观测到的）：** MSRA-SA-41、MSRA-SA-39（default rack），以及 datanode 10.190.173.170、10.86.164.9
**开始时间：** 2015-10-17 15:37:56.547（MRAppMaster 创建）
**结束时间：** 2015-10-17 15:42:47.708（AM 关闭）
**总挂钟时长：** ≈ **4 分 51 秒（291 秒）**
**最终状态：** ✅ **SUCCEEDED**（共分配 13 个容器，2 个推测执行备份被终止）

---

## 1. 阶段图

作业分为五个明确阶段加上一个短暂的清理尾部。起止时间直接取自 MRAppMaster 日志中的状态转换事件。

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

**可视化阶段条（一个字符 ≈ 2.9 秒）：**

```
Init     Map (10×m)        Shuffle       Reduce       C Cleanup
  ..###################################################SSSSSSSSSSSSSSSSSSSRRRRRRRRRRRRRRRRRRRRRRRRRRRC_
  |         |         |          |         |         |         |          |         |         |
  37:56     38:26     38:56      39:26     39:56     40:26     40:56      41:26     41:56     42:26
           (times are HH:MM:SS wall-clock)
```

---

## 2. 详细事件时间线（按时间顺序）

时间戳为 ApplicationMaster 日志中精确到毫秒的值。⚠ 标记警告/错误/推测执行。

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

## 3. Gantt 风格任务视图

每行代表一个任务。图例：`#` = map 运行中，`S` = reduce 在 shuffle/复制阶段，`R` = reduce 执行用户代码，`x` = 被终止的推测执行备份，`[` / `]` = 起止标记。一列 ≈ 2.9 秒。

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

**Gantt 图的关键观察：**

- **启动时 10 路并行**：RM 在 15:38:01.585 的单次心跳中交付了全部 10 个 map 容器；所有 mapper 在 467 ms 内运行就绪（到 15:38:02.052）。

- **长尾 map 任务**：m_000009 在 82 秒内完成（最快），而 m_000006 和 m_000007 拖延了 ~190–204 秒——足以触发推测执行器启动**两个**备份尝试（m_000006_1 在 +88 秒，m_000007_1 在 +103 秒）。

- **推测执行本质上是浪费的**：两种情况下原始尝试都先于备份完成；备份被终止且其临时输出清理失败并产生 WARN。

- **Reduce 提前启动**：slow-start 阈值（mapreduce.job.reduce.slowstart.completedmaps = 0.05 → 5% 取整 → 1 个 map）在 m_000009 完成的瞬间被跨过；r_000000 在 15:40:29 开始 shuffle，此时仍有 8 个 mapper 在运行。

- **Shuffle ≈ 56 秒**，**reduce 用户代码 ≈ 81 秒**（在 15:41:27 的进度跳跃 0.3 → 0.667 中可见，恰在最后一个 map 完成之后）。

---

## 4. 关键事件（影响分析）

| # | 时间 | 严重级别 | 事件 | 对作业的影响 |
|---|------|----------|-------|----------------|
| 1 | 15:37:58.550 | 信息 | 作业**未 uber 化**（map 数过多/输入量过大） | 强制多容器执行 → 完整 10 容器并行；对于 1.2 GB、10 split 的作业是正确决策。 |
| 2 | 15:38:01.585 | 信息 | RM **一次性分配 10 个容器**（全部 host-local） | Mapper 同时启动——无 slow-start 延迟；理想的数据本地性。 |
| 3 | 15:39:24.244 | 信息 | **第一个 map（m_000009）完成**，耗时 82 秒 | 跨过 5% slow-start 阈值；触发 reduce 调度。 |
| 4 | 15:39:24.474 | ⚠ | **m_000006 的推测执行**（进度 ~29% vs. 同伴平均 ~45%） | 分配了第 11 个容器；增加了集群负载但最终**未**先于原始尝试完成。 |
| 5 | 15:39:39.476 | ⚠ | **m_000007 的推测执行**（进度 ~36%） | 分配了第 13 个容器；同样被终止且未赢得竞争。 |
| 6 | 15:40:29.854 | 信息 | **Reduce r_000000 启动**；shuffle 阶段开始 | Reduce 在 8 个 map 仍在运行时开始复制 map 输出——典型的 Hadoop 重叠以隐藏 shuffle 延迟。 |
| 7 | 15:40:45.929 | ⚠⚠ | **HDFS 写入失败**：`Bad response ERROR for block … from datanode 10.86.164.9`；pipeline 恢复 | 写入作业历史文件时的瞬态 datanode 错误。客户端通过将坏 datanode 从 pipeline 中移除来恢复；**无任务失败，无数据丢失**（jhist 在作业结束时成功移至 done-intermediate 目录）。 |
| 8 | 15:41:12.317 | ⚠ | 推测尝试 **m_000007_1 被终止**（原始成功后） | 浪费了 container_000014 约 38.5 秒；临时输出无法删除（良性 WARN）。 |
| 9 | 15:41:25.714 | ⚠ | 推测尝试 **m_000006_1 被终止**（原始成功后） | 浪费了 container_000012 约 120 秒；同样是良性的删除 WARN。 |
| 10 | 15:41:27.499 | 信息 | Reduce 进度跳跃 **0.3 → 0.667**（shuffle+merge 完成，reduce() 函数开始） | 标记了 shuffle 和实际 reduce 计算之间的边界。 |
| 11 | 15:42:46.420 | 信息 | **Reducer r_000000 成功** → 所有 11 个任务完成 | 作业进入 COMMIT；最终输出物化。 |
| 12 | 15:42:46.468 | ✅ | **作业 COMMITTING → SUCCEEDED** | 作业正式成功；计数器已最终确定。 |
| 13 | 15:42:47.708 | 信息 | **AM 关闭**完成（服务器停止，staging 目录删除） | 应用结束。 |

**无失败任务，无失败作业状态。** 唯一的警告是 (a) 一个自动恢复的瞬态 HDFS pipeline 错误和 (b) 两个无害的推测执行后临时文件删除警告。

---

## 5. 并发分析

以下是作业执行期间每 5 秒采样一次的处于 `RUNNING` 状态的任务尝试数。每个 `■` ≈ 一个运行中的容器。

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

**并发摘要**

- **初始化阶段（0–6 秒）：** 0 个任务容器；AM 正在引导启动。

- **Map 爬升阶段（6–88 秒）：** 稳定的 **10 个并发 mapper**（给定 10 容器分配的最大容量）。

- **第一波 + 推测执行（88–153 秒）：** m_000009 完成后短暂降至 **9**，然后推测执行 m_000006_1 启动时回到 **10**；RM 在此窗口期不调度 reducer，因为它持续发出 map 推测容器。

- **重叠 / shuffle（153–158 秒）：** **10** 个并发容器——8 个 map + 推测备份 + reducer。整个作业运行尝试数的峰值。

- **Map 收尾阶段（158–210 秒）：** 随着 map 完成，计数下降：10 → 9 → 6 → 5 → 3 → 2 → 1。m_000005 在 15:40:28 完成后，m_000007_1 推测启动，回弹至 10。

- **单独 reduce（210–290 秒）：** 仅 **1** 个容器运行（reducer）——纯 reduce/merge 阶段。

- **Commit / 清理（290–292 秒）：** 0 个任务容器；AM 执行簿记工作。

**峰值并行度：** 10 个容器同时运行，在作业大部分时间内保持——集群对可用的 mapper 槽位被充分利用。

---

## 6. 每任务耗时

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

## 7. 结论与观察

- **健康、成功的作业**，无失败任务，总挂钟时间 ~4 分 51 秒，处理 1.2 GB / 10 split → 单 reducer PageRank 迭代。

- **掉队者缓解机制有效但过于激进。** 为慢 mapper 启动了两个推测备份，但两个原始尝试都击败了备份。总浪费：一个容器 ~120 秒，另一个 ~38 秒。如果此模式常见，考虑提高 `mapreduce.job.speculative.speculativecap` 或 `slowtaskthreshold`。

- **Reduce 的 slow-start** 设置了非常低的阈值（≈5%，1 个 map）。这导致 reducer 提前开始复制，有利于重叠，但在这个小型 1-reducer 作业中，reducer 在 shuffle 阶段等待尾部 mapper 约 56 秒。

- **一次 HDFS 闪断**（坏 datanode 10.86.164.9）在作业历史写入期间导致 pipeline 恢复，但对作业完全透明——HDFS HA / 客户端重试按设计运行。

- **两个良性的 `Could not delete` 警告**来自 `FileOutputCommitter` 对已终止推测尝试的处理。这些不是致命的，但表明 AM 终止尝试和 committer 清理其 `_temporary` 输出之间存在小的竞争条件；可以忽略。

- **数据本地性优秀**：所有 10 个初始 map 容器均被分配为 **host-local**（最终调度器统计中 HostLocal:10，RackLocal:0）。
