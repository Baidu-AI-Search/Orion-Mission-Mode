# Cron & Scheduled-Task Activity Analysis — `syslog.log`

**Host:** `combo` (RHEL/Fedora-era system, kernel 2.6.5-1.358, year inferred **2005**)
**Log window:** `Jun 9 06:06:20` → `Sep 14 21:50:47` (≈ 97 days, 15 h, 5 000 lines)
**Keywords searched:** `crond`, `anacron`, `atd`, `logrotate`, `su(pam_unix)`, `cupsd`, `syslogd restart`, `run-parts`, `tmpwatch`, `makewhatis`, `updatedb`

---

## Executive Summary

The host runs three separate scheduler daemons (`crond`, `anacron`, `atd`) and exhibits a very regular, almost clockwork daily maintenance window at **~04:02–04:16** local time, with an additional **weekly (Sunday ~04:04–04:22)** job that restarts CUPS and HUPs `syslogd`. Three reboots (or daemon restarts) occurred during the capture period. **Every single `logrotate` invocation failed with exit status `[1]`** for the entire 97-day window — a persistent operational issue.

| Indicator | Value |
|---|---|
| `crond` startup events | **3** (Jun 9, Jun 10, Jul 27) |
| `anacron` startup events | **3** (same 3 events, 2 s after crond) |
| `atd` startup events | **3** (same 3 events) |
| `logrotate` executions | **97** — one per day, Jun 10 → Sep 14 |
| `logrotate` successful runs | **0** — every run exited abnormally `[1]` |
| Daily `su cyrus` (uid=0) sessions | **97** (~04:03:01) |
| Daily `su news` (uid=0) sessions | **97** (~04:09–04:34, ~6 min after logrotate) |
| Weekly CUPS + syslogd restarts | **14** (every Sunday 04:03–04:22) |
| Boot-time `su htt` sessions | **3** (matches the 3 boot events) |

---

## 1. Cron Service Status

`crond` was started (or restarted) exactly **three** times during the log window. In every case it was launched as part of the System V init run (`crond startup succeeded`), preceded by a short `su` session to the `htt` (HTTP/HTT service) account and immediately followed by `anacron` and `atd`.

| # | Timestamp | Trigger | Lag from syslogd restart |
|---|---|---|---|
| 1 | **Thu 2005-06-09 06:06:49** | Fresh system boot (syslogd restart at 06:06:20) | +29 s |
| 2 | **Fri 2005-06-10 11:32:10** | Reboot/service restart cycle (syslogd restart 11:31:44; rpc.statd restarted) | +26 s |
| 3 | **Wed 2005-07-27 14:42:23** | Reboot/service restart cycle (rpc.statd restart 14:41:57; syslogd restart 14:41:57) | +26 s |

Gaps between starts:
- Jun 9 → Jun 10: **1 day 5 h 25 m** (short uptime — likely a reboot shortly after first bring-up)
- Jun 10 → Jul 27: **47 days 3 h 10 m** (long stable uptime)

After the Jul 27 restart there were no further crond startups for the remaining 49 days of the log.

> Observation: the startup sequence is highly deterministic: `syslogd/klogd` → `portmap`/`rpc.statd`/`nfslock` → `su htt` → `crond` (+2 s) → `anacron` (+2 s) → `atd` (+0 s). This is consistent with Red Hat's SysV `rc.sysinit` + runlevel 3 ordering.

---

## 2. Anacron Activity

`anacron` started exactly **3 times**, always within **2 seconds** of `crond` and `atd` during the init sequence:

| # | Timestamp | Δt from crond |
|---|---|---|
| 1 | Thu 2005-06-09 06:06:51 | +2 s |
| 2 | Fri 2005-06-10 11:32:12 | +2 s |
| 3 | Wed 2005-07-27 14:42:25 | +2 s |

**No per-job anacron messages** (e.g. `Will run job`, `Normal exit`, `Job` delay, timestamp updates) appear anywhere in the log. This means either:

1. `anacron` is enabled but has **no `anacrontab` jobs configured** (or all jobs were already caught-up immediately after boot by pre-existing timestamps), **or**
2. Anacron's normal job-level messages are logged elsewhere (e.g. to `/var/log/cron` which was not included) and only the `init.d/anacron` "startup succeeded" line hits syslog.

Given that `cron.daily` (`logrotate`, `su cyrus`, `su news`) fires reliably at ~04:02 every morning — including after the two boots that happened *after* 04:00 (Jun 10 at 11:32 and Jul 27 at 14:42 did *not* trigger a catch-up run the same day; the 04:00 window had already passed and jobs waited until the next day at 04:00) — the daily jobs are almost certainly being dispatched by **`crond` from `/etc/crontab`** (`run-parts /etc/cron.daily` at 04:02), not by anacron. This is a classic RHEL 3/4 layout.

---

## 3. Logrotate Activity

### 3.1 Summary

- **97 executions**, one every calendar day from **Fri 2005-06-10 04:03:02** through **Wed 2005-09-14 04:08:05** (zero missing days, zero duplicate runs per day).
- **100% failure rate.** Every invocation produces a single syslog line:

  ```
  combo logrotate: ALERT exited abnormally with [1]
  ```
  No prior "rotating…" / config / debug messages are present (either because RHEL 3's `logrotate` init/cron wrapper only logs failures, or stdout went to a different file).

### 3.2 Timing characteristics

| Statistic | Value |
|---|---|
| Hour of day | **04:xx (all 97 runs)** |
| Earliest minute | 04:**02**:49 (Jun 27) |
| Latest minute | 04:**20**:42 (Jul 24, the Sunday that also ran the weekly job) |
| Mean minute of logrotate line | **04:04:50** |
| Mean start delay (from first `su cyrus` open) | **+3.6 s** (i.e. logrotate fires 3–4 s after `cron.daily` is entered) |

### 3.3 Affected services (inferred from surrounding log evidence)

`logrotate` itself does not name the rotated files in syslog, but the `su(pam_unix)` sessions bracketing every run reveal which service users are being switched to during the `cron.daily` script sequence:

| Sequence in cron.daily | User | Service inferred | Typical time | Duration |
|---|---|---|---|---|
| Step 1 (just before logrotate line) | **cyrus** | Cyrus IMAP mail server (likely `/etc/logrotate.d/cyrus-imapd` or a `cyrus`-user maintenance script invoked by `logrotate`'s `postrotate`/`su` directive, or `/etc/cron.daily/cyrus*`) | 04:02–04:20 | **~1 s** |
| Step 2 (logrotate proper) | root | **logrotate itself** (rc=1 every run) | 04:03–04:20 | — |
| Step 3 (~6 min later) | **news** | INN/CNEWS news server maintenance (likely `/etc/cron.daily/inn-cron-expire` or `news.daily` run via `su news`) | 04:09–04:34 | **~1 s** |

The steady **~6 minute gap** between logrotate and the `news` su (avg **368 s** weekdays, **397 s** Sundays) suggests additional cron.daily scripts (e.g. `tmpwatch`, `slocate.cron`, `00-logwatch`/`00webalizer`, `rpm` checks) execute between logrotate and the news job, even though they emit no syslog lines.

### 3.4 Logrotate failure — likely causes

The "ALERT exited abnormally with [1]" message is emitted by the RHEL3 `/etc/cron.daily/logrotate` wrapper script when `/usr/sbin/logrotate` returns non-zero. Common causes on that vintage of RHEL:

- A log file listed in a `/etc/logrotate.d/*` config is missing or has bad permissions.
- A `postrotate` script (e.g. `service syslog reload` or `killall -HUP`) fails.
- A lock / `state` file is stale.
- Disk full on `/var` or `/var/log` partition (not verifiable from this log).

Because the failure is continuous for 97 consecutive days and the wrapper always returns 1, a single broken config (possibly introduced just before Jun 10 — the log's first full 04:00 cycle) is the most probable root cause.

---

## 4. `su(pam_unix)` Session Patterns

There are **394 su PAM session records** in the log, split exactly into 197 `opened` / 197 `closed` pairs across three users:

| User | Sessions opened | Invoker | When | Typical duration | Role |
|---|---|---|---|---|---|
| `htt` | 3 | `uid=0` (root) | Boot-time only (06:06:46 Jun 9, 11:32:07 Jun 10, 14:42:21 Jul 27) | <1 s | HTTP/HTT toolkit / startup helper (invoked by an init script, not cron) |
| `cyrus` | 97 | `uid=0` (root) | **Every day 04:03–04:20** | ~0.8 s (max 1 s) | Cron-driven Cyrus mail task (rotate / db maintenance) — always precedes `logrotate` by ~3 s |
| `news` | 97 | `uid=0` (root) | **Every day 04:09–04:34** | ~0.9 s (max 3 s) | Cron-driven INN news maintenance (`news.daily`), last job in the 04:00 daily sequence |

### 4.1 Structural pattern

Every non-boot day produces an identical pair of su-sandwich events around logrotate:

```
04:0x:xx  su(pam_unix): session opened for user cyrus by (uid=0)
04:0x:xx  su(pam_unix): session closed for user cyrus          (≤ 1 s later)
04:0x:xx  logrotate: ALERT exited abnormally with [1]          (~3 s after cyrus open)
   … (≈ 6 min of other cron.daily scripts, silent in syslog) …
04:0x:xx  su(pam_unix): session opened for user news by (uid=0)
04:0x:xx  su(pam_unix): session closed for user news           (≤ 1–3 s later)
```

Sessions are extremely short (< 1 s for cyrus, ~1 s for news), which means the `su` is only being used to spawn the target script under that UID; the real work runs in the child process (and does not itself talk to syslog). The `by (uid=0)` origin confirms that root (i.e. the cron/run-parts process) is the one switching identities — the canonical signature of jobs in `/etc/cron.daily/*` that wrap themselves with `su -s /bin/sh <user> -c "…"`.

### 4.2 Anomalous days

- **Sun Jul 24** shows the longest daily window: cyrus 04:20:19 → logrotate 04:20:42 → news 04:33:57 (news delayed to **+13 min 39 s** after logrotate). This day is also a Sunday (weekly CUPS restart occurred at 04:20:21–26 and syslog restart at 04:20:42); the weekly job appears to have interfered with the daily sequence, delaying the news run.
- Boot days (Jun 9, Jun 10, Jul 27) add an extra `htt` session but the daily 04:00 sequence still runs at the next scheduled window.

---

## 5. Scheduled-Task Timeline

### 5.1 Boot/init timeline (same pattern for all 3 starts)

```
T+0 s    syslogd / klogd started
T+30 s   portmap, rpc.statd, nfslock, irqbalance started
T+26–29 s  su(pam_unix) open+close for user htt (by uid=0)   ← init.d/htt or similar
T+29–32 s  crond startup succeeded                           ← cron daemon online
T+31–35 s  anacron startup succeeded                         ← anacron online
T+31–35 s  atd startup succeeded                             ← atd online
```

### 5.2 Typical weekday cron.daily window (~04:02–04:12)

```
04:02:45–04:04:30   su open/close → user cyrus   (cyrus-imap daily job)
04:03:02–04:20:42   logrotate: ALERT exited abnormally with [1]
04:09:15–04:16:55   su open/close → user news    (INN news.daily expire)
                     (…silent jobs between logrotate and news — tmpwatch, slocate, rpm, logwatch…)
```
Total duration: **5.5–7 min** on a normal weekday.

### 5.3 Typical Sunday (~04:03–04:22 plus 04:09–04:34 news)

Every Sunday the daily window also contains the weekly job that bounces services:

```
04:03:30–04:20:20   su open/close → user cyrus
04:03:40–04:20:42   logrotate: ALERT exited abnormally with [1]
04:03:39–04:20:21   cups: cupsd shutdown succeeded            ← weekly job
04:03:45–04:20:26   cups: cupsd startup succeeded
04:03:57–04:20:42   syslogd 1.4.1: restart                    ← HUP after logrotate
04:09–04:34         su open/close → user news
```

### 5.4 Graphical timeline

![Daily/weekly scheduled task timeline](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784187949700740141/e6448bee354325e0179b1dc0a96cd16c_cron_timeline.png?responseContentDisposition=attachment%3B%20filename%3D%22cron_timeline.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T07%3A51%3A44Z%2F-1%2Fhost%2Fd64c62bd2e3274d377a1d629a7b0d3892773c5f9030710b8f5368e9879e8dff2)

*Vertical orange dashed lines mark the three `crond` restart events (Jun 9 is off the left edge because daily windows begin Jun 10).*

![logrotate events by weekday](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784187949700740141/5221bf5164be599fe3c51367358cd510_weekday_distribution.png?responseContentDisposition=attachment%3B%20filename%3D%22weekday_distribution.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T07%3A51%3A44Z%2F-1%2Fhost%2F96baed2eb0f4b600c12cf9b5f039a08e1b7cb7db14f95129dd72307e35a55e98)

*Sundays (highlighted purple) run the same logrotate job plus the weekly CUPS/syslog restart.*

### 5.5 Key events table (selected milestones)

| Date | Time | Event |
|---|---|---|
| Thu Jun 9 | 06:06:20 | syslogd restart (boot #1) |
| Thu Jun 9 | 06:06:49 | crond start #1 |
| Thu Jun 9 | 06:06:51 | anacron start #1, atd start #1 |
| Fri Jun 10 | 04:03:01 | First daily `su cyrus` of the log |
| Fri Jun 10 | 04:03:02 | **First `logrotate` ALERT exit 1** (of 97) |
| Fri Jun 10 | 04:09:15 | First daily `su news` |
| Fri Jun 10 | 11:32:10 | crond/anacron/atd restart #2 |
| Sun Jun 12 | 04:04:09–04:04:19 | First weekly CUPS restart + syslogd restart |
| Wed Jul 27 | 14:42:23 | crond/anacron/atd restart #3 (long uptime ended) |
| Sun Jul 24 | 04:20–04:34 | Longest daily window (13 min news delay during weekly job) |
| Wed Sep 14 | 04:08:05 | Last `logrotate` ALERT exit 1 |

---

## 6. Recurring Patterns

### 6.1 Daily (runs every day, Mon–Sun, no exceptions)

| Job | Schedule (approx.) | Evidence in syslog |
|---|---|---|
| `/etc/cron.daily` entry for **cyrus** (Cyrus IMAP) via `su cyrus` | 04:02–04:20, after `run-parts` kicks off at 04:02 | 97 `su(pam_unix) … session open/closed for user cyrus` events |
| **logrotate** (wrapper `/etc/cron.daily/logrotate`) | ~3 s after cyrus su opens | 97 `logrotate: ALERT exited abnormally with [1]` lines (rc=1) |
| Other silent `cron.daily` scripts (tmpwatch, slocate/mlocate, rpm –q, logwatch, 0anacron update stamp) | between logrotate and news su (~6 min window) | Inferred from the consistent 5–7 min gap; these jobs do not log to syslog at their default log level |
| `/etc/cron.daily` entry for **news** (INN `news.daily`) | 04:09–04:34, ~6 min after logrotate | 97 `su(pam_unix) … session open/closed for user news` events |

The uniformity across all 7 days of the week (13–14 events each weekday, no day is skipped — see chart §5.4) confirms these are classic **`04:02` daily run-parts** jobs invoked by `crond`, not anacron.

### 6.2 Weekly (runs every Sunday — 14 consecutive Sundays)

| Job | Schedule | Evidence |
|---|---|---|
| **CUPS weekly restart/log job** | Sundays 04:03–04:21 | 14 `cups: cupsd shutdown succeeded` followed within 6 s by `cupsd startup succeeded` — **only on Sundays** |
| **syslogd restart/HUP** (post-logrotate) | Sundays 04:03–04:22, 5–21 s after cups restart | 14 `syslogd 1.4.1: restart` events — all on Sundays (plus the 3 boot days) |

All 14 weekly events fall strictly on Sunday (weekday distribution: **Sun=14, every other weekday=0**), with an inter-run gap of **6 days 23 h 48 min – 7 days 0 h 12 min** — i.e. exactly 7 days within jitter of a few minutes. This is the signature of a `/etc/cron.weekly` job (run at 04:22 on Sundays by the default RHEL `/etc/crontab` `22 4 * * 0 root run-parts /etc/cron.weekly` entry), most likely `/etc/cron.weekly/00-rhsm-cups`-style (or a custom script such as `/etc/cron.weekly/cups` or `makewhatis.cron`/`prelink` bouncing cups as part of log rotation).

### 6.3 Boot-time (aperiodic, 3 occurrences)

- `su htt` session followed by `crond` / `anacron` / `atd` start — this is an **init-time** pattern, not a cron schedule, but it is the only way `htt` ever appears in the log.

### 6.4 Monthly / hourly / @reboot

- **No hourly jobs** were detected (no events recurring at any fixed minute-past-the-hour pattern in other hours).
- **No monthly jobs** were detected (no event that fires once per month on a fixed date).
- **`atd` is running** (startup seen) but **no `at` job executions** appear in the log during the capture window.
- **No `anacron` job-level messages** were observed (see §2).

---

## Findings & Recommendations

1. **🔴 Critical / Actionable: `logrotate` has failed every single day for 97 days.** Exit code `[1]` from `/etc/cron.daily/logrotate` means no logs are being rotated. This will cause `/var/log` (and any Cyrus/News spools) to grow unbounded and eventually fill the disk. Investigate with `logrotate -d /etc/logrotate.conf` to find the offending config; check for missing files, bad `postrotate` hooks (especially the `killall -HUP syslogd` / service reload lines), and stale `/var/lib/logrotate.status`.
2. **🟡 Weekly Sunday job bounces `cupsd` and HUPs `syslogd`.** Confirm this is intentional (likely `/etc/cron.weekly/cups` or a custom maintenance script). On Jul 24 it caused the daily news job to slip to 04:33:57 (+13 min), suggesting it can block subsequent daily scripts.
3. **🟢 Scheduler daemons are healthy.** `crond`, `anacron`, and `atd` have been stable since the Jul 27 restart (≈49 days uptime through end-of-log).
4. **ℹ️ Daily maintenance is dispatched by `crond` run-parts, not anacron.** The 04:02 cadence and the absence of "missed-job catch-up" runs after the Jun 10 / Jul 27 boots confirm this. Anacron appears to be installed and running but not actively used.
5. **ℹ️ User identities in `su(pam_unix)` lines map directly to service accounts:** `cyrus` (IMAP), `news` (INN news), `htt` (boot-time HTTP helper). The fact that all 194 cron-driven `su` calls originate from `uid=0` confirms they are root-owned `cron.daily` scripts dropping privileges.

---

*Report generated from `syslog.log` (5 000 lines, 509 KB) by analyzing crond/anacron/atd startups, logrotate events, su(pam_unix) PAM sessions, and service restart patterns.*
