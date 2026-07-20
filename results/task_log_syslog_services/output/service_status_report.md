# Service Status Report — `combo` (Linux 2.6.5-1.358)

**Source log:** `syslog.log` (5,000 lines)  
**Host:** `combo`  
**Kernel:** Linux 2.6.5-1.358 (Red Hat, built May 8 2004)  
**Year inferred:** 2005 (corroborated by `ftpd` session timestamps and `named`/`ntpd` log messages)  
**Log window:** 2005‑06‑09 06:06:20 → 2005‑09‑14 21:50:47 (~97 days)

---

## 1. Service Inventory

The log shows startup and/or shutdown events for **35 distinct services**. All services (except `cupsd` and `mysqld`) only appear at boot time — they are never explicitly stopped during normal operation.

| # | Service | Daemon / Role | Starts | Stops | Fails | Notes |
|---|---|---|---|---|---|---|
| 1 | **syslogd** | system logger | 3 | 0 | 0 | Also HUP‑restarted weekly by logrotate (not a full restart) |
| 2 | **klogd** | kernel log daemon | 3 | 0 | 0 | |
| 3 | **irqbalance** | IRQ load balancer | 3 | 0 | 0 | |
| 4 | **portmap** | RPC portmapper | 3 | 0 | 0 | |
| 5 | **rpc.statd** | NFS lock status (nfslock) | 3 | 0 | 0 | |
| 6 | **rpc.idmapd** | NFSv4 ID mapper | 3 | 0 | 0 | |
| 7 | **hcid** | Bluetooth host controller | 3 | 0 | 0 | bluetooth subsystem |
| 8 | **sdpd** | Bluetooth service discovery | 3 | 0 | 0 | bluetooth subsystem |
| 9 | **netfs** | Network filesystem mounter | 3 | 0 | 0 | |
| 10 | **apmd** | APM power management | 3 | 0 | 0 | |
| 11 | **automount** / autofs | AutoFS mounter | 3 | 0 | 0 | |
| 12 | **smartd** | S.M.A.R.T. disk monitor | 3 | 0 | 0 | |
| 13 | **pcmcia** | PCMCIA/CardBus | 3 | 0 | 0 | started via `rc` |
| 14 | **hpoj** | HP OfficeJet (print/scan) | 3 | 0 | 0 | started via `rc` |
| 15 | **network** | Network bring‑up | 3 | 0 | 0 | loopback + parameters |
| 16 | **cupsd** | CUPS printing | 17 | 14 | 0 | **Frequent weekly restarts** (see §4) |
| 17 | **sshd** | SSH daemon | 3 | 0 | 0 | |
| 18 | **xinetd** | Super‑server | 3 | 0 | 0 | telnet fails (port in use); 30 services active |
| 19 | **sendmail** | MTA | 3 | 0 | 0 | |
| 20 | **sm-client** | Sendmail client queue | 3 | 0 | 0 | |
| 21 | **spamd** (spamassassin) | SpamAssassin filter | 3 | 0 | 0 | |
| 22 | **privoxy** | Web proxy / ad‑filter | 3 | 0 | 0 | |
| 23 | **gpm** | Console mouse | 3 | 0 | 0 | |
| 24 | **htt** (IIim) | X Input Method server | 3 | 0 | 0 | |
| 25 | **canna** | Japanese input server | 3 | 0 | 0 | |
| 26 | **crond** | Cron daemon | 3 | 0 | 0 | |
| 27 | **xfs** | X font server | 3 | 0 | 0 | |
| 28 | **anacron** | Anacron | 3 | 0 | 0 | |
| 29 | **atd** | At scheduler | 3 | 0 | 0 | |
| 30 | **readahead** | Boot readahead | 3 | 0 | 0 | one‑shot |
| 31 | **messagebus** | D‑Bus system bus | 3 | 0 | 0 | |
| 32 | **httpd** | Apache Web server | 3 | 0 | 0 | |
| 33 | **squid** | Squid HTTP proxy/cache | 3 | 0 | 0 | child process spawned at boot |
| 34 | **named** | BIND DNS server | 3 | 0 | 0 | listens on ~23 virtual IPs (eth0:1 – eth0:22) |
| 35 | **snmpd** | Net‑SNMP agent | 3 | 0 | 0 | |
| 36 | **ntpd** | NTP time daemon | 3 | 0 | 0 | |
| 37 | **mysqld** | MySQL database | 4 | 1 | 0 | One manual restart 2005‑08‑02 07:46 |
| 38 | **rsyncd** | rsync daemon | 3 | 0 | 0 | listens on port 873 |
| 39 | **mdmonitor** | MD RAID monitor | 3 | 0 | 0 | mdadm |
| 40 | **mdmpd** | MD multipath daemon | 0 | 0 | **3** | Failed to start on every boot |

---

## 2. System Boot Events

A full reboot is identifiable by a fresh `kernel: Linux version 2.6.5-1.358 ...` line followed by the characteristic 60‑second burst of service startups. Three full boots are visible, plus weekly **syslogd HUPs** (log rotation, not a reboot).

| Boot # | Kernel boot timestamp | Trigger context |
|---|---|---|
| **1** | **Jun 9 06:06:20** | Cold/regular boot (first log entry) |
| **2** | **Jun 10 11:31:44** | Reboot ~29 h after boot 1 (no explicit shutdown logged — looks like an unclean/crash restart; brief uptime) |
| **3** | **Jul 27 14:41:57** | Reboot after 47‑day uptime (possible scheduled maintenance or crash) |

Weekly **syslogd restart** entries (`syslogd 1.4.1: restart.`) appear every Sunday ≈04:04–04:20 and are always **immediately preceded by a CUPS stop/start cycle** — they are `SIGHUP`‑based log rotations, **not** reboots:

> Jun 12, Jun 19, Jun 26, Jul 3, Jul 10, Jul 17, Jul 24, Jul 31, Aug 7, Aug 14, Aug 21, Aug 28, Sep 4, Sep 11 (all ~04:0x)

---

## 3. Per‑Service Status (start/stop events with timestamps)

Events are grouped by boot window; events that fall outside of the first minute of a boot are standalone runtime events.

### Boot 1 — Jun 9 06:06:20 → Jun 10 11:31:44

| Offset | Timestamp | Service | Event |
|---|---|---|---|
| T+0s | Jun 9 06:06:20 | syslogd, klogd, irqbalance, portmap, rpc.statd | START |
| T+1s | Jun 9 06:06:21 | rpc.idmapd | START |
| T+2s | Jun 9 06:06:22 | hcid, sdpd | START |
| T+3s | Jun 9 06:06:23 | apmd, netfs | START |
| T+4s | Jun 9 06:06:24 | automount, smartd | START |
| T+17s | Jun 9 06:06:37 | cupsd | START |
| T+18s | Jun 9 06:06:38 | sshd, xinetd | START |
| T+20s | Jun 9 06:06:40 | sendmail | START |
| T+21s | Jun 9 06:06:41 | sm-client | START |
| T+24s | Jun 9 06:06:44 | spamd | START |
| T+25s | Jun 9 06:06:45 | privoxy | START |
| T+26s | Jun 9 06:06:46 | gpm, htt | START |
| T+29s | Jun 9 06:06:49 | canna, crond | START |
| T+30s | Jun 9 06:06:50 | xfs | START |
| T+31s | Jun 9 06:06:51 | anacron, atd, readahead | START |
| T+32s | Jun 9 06:06:52 | messagebus | START |
| T+46s | Jun 9 06:07:06 | httpd | START |
| T+47s | Jun 9 06:07:07 | squid | START |
| T+53s | Jun 9 06:07:13 | named | START |
| T+55s | Jun 9 06:07:15 | snmpd | START |
| T+57s | Jun 9 06:07:17 | ntpd | START |
| T+62s | Jun 9 06:07:22 | mysqld | START |
| T+63s | Jun 9 06:07:23 | rsyncd, mdmonitor | START |
| T+64s | Jun 9 06:07:24 | mdmpd | **FAIL** |

### Boot 2 — Jun 10 11:31:44 → Jul 27 14:41:57

Boot startup sequence is identical to Boot 1 (all 36 services start within ~63 s); mdmpd again fails at T+63s. Runtime events in this window:

| Timestamp | Service | Event |
|---|---|---|
| Jun 10 11:31:44–11:32:47 | (all services) | START (same order as above) |
| Jun 10 11:32:47 | mdmpd | **FAIL** |
| **Jun 12 04:04:09** | cupsd | **STOP** |
| **Jun 12 04:04:15** | cupsd | START (downtime ≈6 s) |
| Jun 19 04:08:57 | cupsd | STOP |
| Jun 19 04:09:02 | cupsd | START (≈5 s) |
| Jun 26 04:04:19 | cupsd | STOP |
| Jun 26 04:04:24 | cupsd | START (≈5 s) |
| Jul 3 04:07:49 | cupsd | STOP |
| Jul 3 04:07:55 | cupsd | START (≈6 s) |
| Jul 10 04:04:33 | cupsd | STOP |
| Jul 10 04:04:39 | cupsd | START (≈6 s) |
| Jul 17 04:08:10 | cupsd | STOP |
| Jul 17 04:08:16 | cupsd | START (≈6 s) |
| Jul 24 04:20:21 | cupsd | STOP |
| Jul 24 04:20:26 | cupsd | START (≈5 s) |

### Boot 3 — Jul 27 14:41:57 → Sep 14 21:50:47 (end of log)

Boot startup again identical (~63 s); mdmpd fails. Runtime events:

| Timestamp | Service | Event |
|---|---|---|
| Jul 27 14:41:57–14:43:00 | (all services) | START (same order) |
| Jul 27 14:43:00 | mdmpd | **FAIL** |
| Jul 31 04:08:42 | cupsd | STOP |
| Jul 31 04:08:48 | cupsd | START (≈6 s) |
| **Aug 2 07:46:28** | mysqld | **STOP** |
| **Aug 2 07:46:32** | mysqld | START (downtime ≈4 s; standalone restart, not log‑rotation) |
| Aug 7 04:03:39 | cupsd | STOP |
| Aug 7 04:03:45 | cupsd | START (≈6 s) |
| Aug 14 04:03:39 | cupsd | STOP |
| Aug 14 04:03:44 | cupsd | START (≈5 s) |
| Aug 21 04:03:37 | cupsd | STOP |
| Aug 21 04:03:42 | cupsd | START (≈5 s) |
| Aug 28 04:10:26 | cupsd | STOP |
| Aug 28 04:10:32 | cupsd | START (≈6 s) |
| Sep 4 04:03:58 | cupsd | STOP |
| Sep 4 04:04:04 | cupsd | START (≈6 s) |
| Sep 11 04:03:41 | cupsd | STOP |
| Sep 11 04:03:46 | cupsd | START (≈5 s) |

---

## 4. CUPS Special Case — Weekly Restart Pattern

`cupsd` is the **only service that is regularly stopped and restarted outside of full reboots**.

**Pattern summary**
- **Frequency:** Roughly **every 7 days** (Sunday morning, ~04:03–04:20 local time).
- **Sequence at each cycle:**
  1. `cups: cupsd shutdown succeeded`
  2. 5–6 seconds later: `cups: cupsd startup succeeded`
  3. 6–16 seconds later: `syslogd 1.4.1: restart.` (logrotate SIGHUPs syslogd)
  4. The daily `logrotate: ALERT exited abnormally with [1]` is typically logged within the same 04:0x hour.
- **Downtime per cycle:** ~5–6 seconds (service unavailable only briefly).
- **Cycles observed:** Jun 12, Jun 19, Jun 26, Jul 3, Jul 10, Jul 17, Jul 24, Jul 31, Aug 7, Aug 14, Aug 21, Aug 28, Sep 4, Sep 11 — **14 weekly restart cycles** total (plus the 3 boot‑time starts = 17 start events).
- **Root cause (inferred):** This pattern matches a weekly cron job (`/etc/cron.weekly/` or a Sunday‑04:00 `crontab` entry) that rotates the CUPS log files (`/var/log/cups/access_log`, `error_log`, `page_log`) and then HUPs or restarts `cupsd`; the almost‑identical cadence and the co‑occurring `syslogd restart` strongly suggest **`logrotate`** is driving both cycles. The persistent `logrotate: ALERT exited abnormally with [1]` message each morning indicates a post‑rotate script failure that should be investigated.

**All CUPS events (chronological):**

| # | Timestamp | Event | Cycle gap |
|---|---|---|---|
| 1 | Jun 9 06:06:37 | START (boot 1) | — |
| 2 | Jun 10 11:32:00 | START (boot 2) | — |
| 3 | Jun 12 04:04:09 | STOP | weekly |
| 4 | Jun 12 04:04:15 | START | |
| 5 | Jun 19 04:08:57 | STOP | +7 d 00:04 |
| 6 | Jun 19 04:09:02 | START | |
| 7 | Jun 26 04:04:19 | STOP | +6 d 23:55 |
| 8 | Jun 26 04:04:24 | START | |
| 9 | Jul 3 04:07:49 | STOP | +7 d 00:03 |
| 10 | Jul 3 04:07:55 | START | |
| 11 | Jul 10 04:04:33 | STOP | +6 d 23:57 |
| 12 | Jul 10 04:04:39 | START | |
| 13 | Jul 17 04:08:10 | STOP | +7 d 00:04 |
| 14 | Jul 17 04:08:16 | START | |
| 15 | Jul 24 04:20:21 | STOP | +7 d 00:12 |
| 16 | Jul 24 04:20:26 | START | |
| 17 | Jul 27 14:42:13 | START (boot 3) | — |
| 18 | Jul 31 04:08:42 | STOP | weekly |
| 19 | Jul 31 04:08:48 | START | |
| 20 | Aug 7 04:03:39 | STOP | +6 d 23:55 |
| 21 | Aug 7 04:03:45 | START | |
| 22 | Aug 14 04:03:39 | STOP | +7 d 00:00 |
| 23 | Aug 14 04:03:44 | START | |
| 24 | Aug 21 04:03:37 | STOP | +6 d 23:59 |
| 25 | Aug 21 04:03:42 | START | |
| 26 | Aug 28 04:10:26 | STOP | +7 d 00:07 |
| 27 | Aug 28 04:10:32 | START | |
| 28 | Sep 4 04:03:58 | STOP | +6 d 23:54 |
| 29 | Sep 4 04:04:04 | START | |
| 30 | Sep 11 04:03:41 | STOP | +6 d 23:59 |
| 31 | Sep 11 04:03:46 | START | |

---

## 5. Service Dependencies & Startup Ordering

The ordering of services is **identical across all three boots** (to within 1–2 s jitter), which reflects the SysV `rc` runlevel ordering (numeric `S##service` links in `/etc/rc.d/rc3.d/` or rc5.d).

### Tier 0 — Core OS / Kernel support (T+0 s to T+1 s)
Started the instant init takes over, before filesystems are fully usable for complex daemons:
1. **syslogd / klogd** — logging must be up first.
2. **irqbalance** — IRQ distribution.
3. **portmap** — required by any RPC service.
4. **rpc.statd (nfslock)** — needs portmap.
5. **rpc.idmapd** — NFSv4 support.

### Tier 1 — Hardware / System daemons (T+2 s to T+4 s)
Depend on core kernel but not on the network being fully up:
6. **hcid / sdpd** (Bluetooth stack)
7. **apmd** (power)
8. **netfs** (mounts NFS/SMB filesystems — depends on portmap being ready)
9. **automount** (autofs, depends on netfs/portmap)
10. **smartd** (disk health — direct block‑device access, no network needed)
11. *Also brought up during this phase by rc.sysinit (visible in kernel/rc messages):* **pcmcia**, **hpoj** (parport/USB printer), **network** (loopback + parameters).

### Tier 2 — Mid‑level system services (T+16 s to T+32 s)
After filesystems are mounted and network interfaces are up:
12. **cupsd** — printing (requires parport/USB initialized by hpoj).
13. **sshd** — remote admin access.
14. **xinetd** — super server (covers ftpd, telnet, comsat, etc.; telnet fails because port already in use).
15. **sendmail / sm-client** — Mail Transport Agent.
16. **spamd (SpamAssassin)** — mail filter, feeds sendmail.
17. **privoxy** — HTTP proxy.
18. **gpm** — console mouse.
19. **htt (IIim) / canna** — input method servers (i18n desktop stack).
20. **crond / anacron / atd** — job schedulers.
21. **xfs** — X Window font server.
22. **readahead** — one‑shot boot preload.
23. **messagebus (D‑Bus)** — system IPC bus.

### Tier 3 — Network server applications (T+42 s to T+63 s)
Wait until the network stack has fully stabilized (IPs added, routing up):
24. **httpd** (Apache)
25. **squid** (HTTP cache proxy — child spawned)
26. **named** (BIND DNS) — binds to ~23 IP aliases (eth0:1–eth0:22 on 63.126.79.x)
27. **snmpd**
28. **ntpd** (time sync)
29. **mysqld** (database)
30. **rsyncd** (port 873)
31. **mdmonitor** (RAID monitor)
32. **mdmpd** — **fails at every boot** (multipathd requires MD multipath arrays; host has only single‑disk partitions hda1/hda2/hda3, so the daemon correctly exits).

### Inferred dependency graph (simplified)

```
kernel ──► syslogd ──► irqbalance ──► portmap ──► rpc.statd, rpc.idmapd
                                                  │
                                                  ▼
                                               netfs ──► automount
                                                  │
                          ┌───────────────────────┴───────────┐
                          ▼                                       ▼
                       apmd, smartd,                     network (loopback/eth0)
                       pcmcia, hpoj                             │
                          │                                     ▼
                          └──────────────────► cupsd, sshd, xinetd
                                                    │
                                    ┌───────────────┴─────────────────┐
                                    ▼                                 ▼
                              sendmail+sm-client              gpm, crond, xfs,
                              spamd, privoxy                   anacron, atd,
                              htt, canna                       messagebus
                                    │                                 │
                                    └─────────────┬───────────────────┘
                                                  ▼
                                         httpd, squid, named,
                                         snmpd, ntpd, mysqld,
                                         rsyncd, mdmonitor
                                                  │
                                                  ▼
                                               mdmpd (FAILS)
```

---

## 6. Uptime Estimate Between Restarts

Because syslog timestamps lack a year, and ftpd "at" lines and day‑of‑week strings (`Sun Jun 12`, `Mon Jun 13`, `Tue Sep 13`, `Wed Sep 14`) fix the calendar, the log window is unambiguously **June 9 – September 14, 2005**.

| Uptime period | Start | End | Duration | Notes |
|---|---|---|---|---|
| **Session 1** | Jun 9 06:06:20 | Jun 10 11:31:44 | **1 day, 5 h 25 m 24 s ≈ 29.4 h (≈1.23 days)** | Short uptime; no clean shutdown message logged — likely a crash/forced reboot. |
| **Session 2** | Jun 10 11:31:44 | Jul 27 14:41:57 | **47 days, 3 h 10 m 13 s ≈ 1,131.2 h (≈47.13 days)** | Long stable stretch; only service disruption is weekly CUPS/logrotate cycling. |
| **Session 3** | Jul 27 14:41:57 | Sep 14 21:50:47 (end of log — host presumably still up) | **49 days, 7 h 08 m 50 s ≈ 1,183.1 h (≈49.30 days)** | One standalone mysqld restart Aug 2 07:46 (downtime ≈4 s); otherwise only weekly CUPS restarts. |

### Aggregate statistics over the 97.7‑day log window
- **Total observed uptime:** ≈97.66 days (system comes back up immediately at each reboot — only seconds lost during the three BIOS/kernel initializations).
- **Number of reboots:** 2 (3 boot events, meaning the host started the period already scheduled to be up from an earlier state, but we catch it in mid‑log).
- **Mean time between boots (MTBF) over this log:** ≈(29.4 + 1131.2 + 1183.1)/2 intervals after the first truncated session ≈ **48.6 days**.
- **Mean time between full reboots of sessions 2+3:** ≈48.2 days — long, stable Linux‑server class uptime.
- **Scheduled service outages (non‑reboot):**
  - `cupsd`: 14 weekly restarts × ~5.5 s ≈ **77 s total CUPS downtime** over 97 days (99.999% availability).
  - `mysqld`: 1 restart × ~4 s.
- **Persistent startup failure:** `mdmpd` fails every boot — host has no MD multipath devices, so this is likely a harmless misconfiguration (safe to `chkconfig mdmpd off`).

---

## Appendix — Other Notable Observations

- **Security noise (not service events, but visible):** Continuous SSH brute‑force attempts from many IPs (e.g. `unknown.sagonet.net`, `218.188.2.4`, `218.38.14.205`, `61.153.202.254`, `yang.dorm13.nctu.edu.tw`, etc.) and parallel FTP connection floods from `209.184.7.130`, `222.33.90.199`, `61.218.67.60`, `212.5.120.141`, `86.193.48.194`, `211.107.232.1` — these are `xinetd`‑spawned sessions, **not** service starts/stops, but they indicate the host is directly exposed to the Internet and under constant attack.
- **`logrotate` error** appears daily near 04:0x (`logrotate: ALERT exited abnormally with [1]`) — correlates with the weekly CUPS+syslogd cycle; the daily cron entry is failing on a post‑rotate script.
- **`xinetd` telnet failure** at every boot: `bind failed (Address already in use). service = telnet` — something else (possibly a stale `in.telnetd` or another inetd) holds port 23; telnet is deactivated each boot.
- **`rpc.statd` gethostbyname error** Jun 13 11:55:04 — isolated DNS resolution failure for a malformed hostname (`^X`), non‑fatal.
