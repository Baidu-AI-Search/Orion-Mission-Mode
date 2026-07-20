# Syslog Anomaly Analysis Report

**Host:** `combo`  
**Kernel:** Linux 2.6.5-1.358 (Red Hat Linux 3.3.3-7, GCC 3.3.3)  
**Log file:** `syslog.log`  
**Analysis date range:** 2026-07-16 (analysis performed on log covering Jun 9 – Sep 14 2005)  
**Total log lines analyzed:** 5,000

---

## 1. Log Overview

### 1.1 Volume and Date Range

| Metric | Value |
|---|---|
| Total entries | **5,000** |
| First timestamp | `Jun  9 06:06:20` (syslogd/klogd restart, fresh boot) |
| Last timestamp | `Sep 14 21:50:47` |
| Covered period | ~98 days (~3.3 months), June 9 → September 14 2005 |
| Number of system reboots logged | **3** (Jun 9 06:06; Jun 10 11:31; Jul 27 14:41) — kernel restart banners visible at each |

### 1.2 Top Services by Log Volume

| Rank | Service / Process | Lines | Share |
|---|---|---:|---:|
| 1 | `ftpd` | **1,655** | 33.1 % |
| 2 | `sshd(pam_unix)` | **1,610** | 32.2 % |
| 3 | `kernel` | 545 | 10.9 % |
| 4 | `su(pam_unix)` | 394 | 7.9 % |
| 5 | `udev` | 175 | 3.5 % |
| 6 | `named` (BIND 9.2.3) | 109 | 2.2 % |
| 7 | `logrotate` | 97 | 1.9 % |
| 8 | `klogind` | 46 | 0.9 % |
| 9 | `cups` | 31 | 0.6 % |
| 10 | `smartd` / `xinetd` / `ntpd` / `gpm` / `telnetd` | 24–30 each | < 0.6 % |
| 11 | `rpc.statd` | 12 | 0.2 % |

**Observation.** Two externally-facing daemons (`ftpd` + `sshd`) alone account for **~65 %** of all log lines. This is a strong signal that the host was exposed to the public Internet and actively scanned/attacked during the entire capture window.

### 1.3 Other Notable Baseline Facts

- **`logrotate` fails every night.** 97 `ALERT exited abnormally with [1]` entries — one at ~04:0x every single day from Jun 10 through Sep 14. This is a chronic operational fault (likely a postrotate script error or permission issue), not an attack, but it masks other failures.
- **Three full reboots** occurred: Jun 9 (initial), Jun 10 (11:31), Jul 27 (14:41). Each reboot re-initialises `rpc.statd`, `named`, `portmap`, `xinetd`, and the network stack.
- **BIND 9.2.3 is running chrooted** (`-t /var/named/chroot`) with a very large number of virtual interfaces on eth0 (at least 23 aliases: `63.126.79.67` through `…125`), indicating combo is a shared web/DNS hosting machine.
- **`su` activity** is dominated by automated sessions for `cyrus` (194) and `news` (194) — routine maintenance — plus 6 sessions for the unusual account `htt`.

---

## 2. Security Anomalies (Attacks / Unauthorized Access Attempts)

### 2.1 SSH Brute-Force Campaign — **1,221 authentication-failure events**

Nearly a third of all SSH-related log entries are failed logins — a clear distributed SSH brute-force / password-spraying campaign.

**Key statistics** (from `sshd(pam_unix)` authentication-failure lines):

| Metric | Value |
|---|---|
| Total failed SSH authentications | **1,221** |
| Unique source hosts / IPs | >100 distinct external IPs |
| #1 targeted account | **`root`** (840 attempts, 69 %) |
| Other targeted accounts | `nobody` (29), `test` (20), `guest` (18), `amanda` (10), `webmaster` (10), etc. |

**Top offending source hosts:**

| Source | Attempts | Notes |
|---|---:|---|
| `150.183.249.110` | 80 | Jul 10 — sustained 150+ attempts/hr against `root` |
| `220.82.197.48` | 80 | Aug 29 07:22–07:23 — 80 attempts in **~1 minute** against `root` |
| `210.109.97.31` | 70 | Multi-hour brute-force |
| `218.188.2.4` | 49 | Sprayed `test`/default accounts |
| `62.205.38.26` | 32 | Sep 4 burst against `root` |
| `unknown.sagonet.net` | 23 | Jun 11 burst against `root` |
| `yang.dorm13.nctu.edu.tw` | 23 | Sep 13 — 16 attempts against `nobody` in a single minute |
| `207.243.167.114` | 23 | Jul 26 burst |
| `n219076184117.netvigator.com` | 23 | Jun 22 burst |

Representative evidence (excerpts):

```
Jun 11 09:45:45 combo sshd(pam_unix)[6472]: authentication failure; ... rhost=unknown.sagonet.net  user=root
Jun 11 09:45:55 combo sshd(pam_unix)[6474]: authentication failure; ... rhost=unknown.sagonet.net  user=root
... (repeated ~20x in 2 minutes)
Jul 10 16:02:24 combo sshd(pam_unix)[...]: authentication failure; ... rhost=150.183.249.110 user=root
... (80 attempts over ~1 hour)
Aug 29 07:22–07:23 combo sshd[...]: 80 failures from 220.82.197.48 in ~60 seconds against root
Sep 13 15:00:32 combo sshd(pam_unix)[28060]: authentication failure; ... rhost=203.223.40.241 user=nobody
Sep 14 15:45:29 combo sshd(pam_unix)[31752]: authentication failure; ... rhost=lneuilly-152-22-93-12.w193-251.abo.wanadoo.fr user=root
```

**Pattern characteristics:** classic early-2000s SSH dictionary/brute-force worm behavior (similar to `ssh-scanner`, `Kaiten`, `Miori` precursors) — distributed sources, password spray against `root` first, then `test`/`guest`/`nobody`/service accounts. The rate from `220.82.197.48` (80/min) far exceeds a human typo rate — it is automated.

### 2.2 Telnet Exploitation / Binary Probes

`telnetd` logs 24 entries; most are normal, but on **Aug 7 11:33–11:35** there is a burst of ~20 simultaneous connections that all terminate with:

```
Aug  7 11:33:59 combo telnetd[16729-16755]: ttloop:  peer died: Invalid or incomplete multibyte or wide character
Aug  7 11:34:07 combo telnetd[16746]: ttloop:  read: Connection reset by peer
```

The message "Invalid or incomplete multibyte or wide character" is telnetd's way of saying it received non-ASCII/binary byte sequences on the telnet TCP session — a hallmark of **telnetd buffer-overflow / shellcode probes** (e.g., old `telnetd` ENV_OPT overflow, CVE-2001-0557-class worms sending binary exploit payloads). 16–20 parallel connections from the same /24-scale source in a few seconds is consistent with a bot/worm.

### 2.3 Suspicious Anonymous FTP Login

```
Jul 24 02:38:23 combo ftpd[16781]: ANONYMOUS FTP LOGIN FROM 84.102.20.2,  (anonymous)
Jul 24 02:38:23 combo ftpd[16782]: ANONYMOUS FTP LOGIN FROM 84.102.20.2,  (anonymous)
```
Two simultaneous anonymous logins from `84.102.20.2` at 2:38 AM. Anonymous FTP was a well-known vector for warez-drop / malware staging on misconfigured servers.

### 2.4 BIND DNS Anomalies

After a restart on Jul 25, BIND logs **16 consecutive** malformed NOTIFY packets:

```
Jul 25 12:09:06 combo named[2306]: notify question section contains no SOA
Jul 25 12:18:50 combo named[2306]: notify question section contains no SOA
... (every ~8–10 minutes until 16:29, total 16)
```
This is a classic **DNS NOTIFY spoof / cache-poisoning probe signature** (malformed NOTIFY with empty question section to elicit a REFUSED that reveals version/allow-transfer state, or as part of an early cache-poisoning attempt). Source IPs are not logged by default in BIND 9.2.3 for this message type, but the uniform ~9-minute cadence and same malformed payload is a scan, not a legitimate slave.

---

## 3. Format-String / Binary Payload Detection

A signature search for format-string specifiers (`%n`, `%x%x`, `%u%u`, `%s%s%s`), NOP sledges (`\220\220...` ≈ `\x90\x90...`), and non-printable runs returns **9 lines** — all of them from `rpc.statd` and **all identical payloads**, i.e. one root-cause exploit attempt repeated 9 times across 3 dates. These are described in detail in §5 below (they are the rpc.statd format-string / buffer-overflow attack, which combines both format strings and shellcode/NOP-sled).

Outside of the rpc.statd entries, **no other lines** contain format-string payloads or embedded binary.

**However**, a separate format-string-class exploitation attempt is observable through the telnet service:

- The Aug 7 11:33 `telnetd: ttloop: peer died: Invalid or incomplete multibyte or wide character` burst (§2.2) is the same behavior when telnetd receives bytes that fail multibyte-character validation — consistent with binary exploit payloads being shoved down the telnet TCP stream rather than valid UTF-8/ASCII.

---

## 4. FTP Anomalies

### 4.1 Volume and Sources

- **1,638** `ftpd[..]: connection from …` log lines — the single noisiest service.
- **155 distinct external source IPs** connected to FTP over the period.
- Top sources (FTP only):

| Source IP | FTP connections | Notes |
|---|---:|---|
| `211.107.232.1` | **149** | Sep 13 burst — single-day, last day of capture |
| `211.72.151.162` | 58 |  |
| `202.82.200.188` | 56 | Jul 1 burst |
| `203.101.45.59` | 46 | Jul 3 burst |
| `207.30.238.8` | 46 |  |
| `69.15.163.251` | 46 | Jun 13 burst |
| `218.146.61.230` | 45 |  |
| `62.241.71.44` | 41 |  |
| `222.33.90.199` | 35 |  |
| `210.245.165.136` | 32 |  |
| `61.218.67.60` / `61.74.96.178` / `62.99.164.82` / `208.62.55.75` / `212.5.120.141` | 23 each | All appear in a single "23-in-one-second" burst (see §4.2) |

### 4.2 Connection Bursts (Parallel FTP Flood / Scanning)

A highly distinctive pattern: **dozens of source IPs each open exactly 23 FTP connections in the same second**. That is not a normal FTP client; it is either a parallel-port-scan tool, a worm (e.g., Ramen/Cheese/Adore family scanning for wu-ftpd vulnerabilities), or a DDoS/syn-flood reflector.

Sample of the burst clusters (all are exactly 23 connections/second from one IP):

| Timestamp | Source IP | # conns / sec |
|---|---|---:|
| Jun 13 09:09:44 | `212.5.120.141` | 23 |
| Jun 13 12:49:32 | `65.35.73.251` | 23 |
| Jul  1 07:57:30 | `202.82.200.188` | 23 |
| Jul  3 10:05:25 | `203.101.45.59` | 23 |
| Jul  3 23:16:09 | `62.99.164.82` | 23 |
| Jul  4 12:52:44 | `63.197.98.106` | 23 |
| Jul  9 11:35:59 | `211.57.88.250` | 23 |
| Jul 10 03:55:15 | `217.187.83.139` | 23 |
| Jul 10 13:17:22 | `220.94.205.45` | 23 |
| Jul 22 09:27:27 | `211.42.188.206` | 23 |

Each burst spans exactly 23 PIDs (e.g. PIDs 27722–27741 on Sep 13), confirming **23 concurrent `ftpd` child processes spawned in a single second** — strong evidence of an automated scanner hitting `combo`'s FTP port (TCP/21) in a parallel sweep, most likely probing for **wu-ftpd / vsftpd vulnerabilities** (e.g., CVE-2004-1048 SITE EXEC, CVE-2001-0550 glob heap corruption, or the `SITE EXEC` format-string bugs) that were epidemic in 2005.

The Sep 13 burst from `211.107.232.1` is the largest single source (149 conns, all within a few minutes on the last captured day) and opens connections in the same pattern.

### 4.3 FTP getpeername Errors — Possible Port-Scan / RST Flood

```
Jul 25 23:23:13 combo ftpd[26463/26466/26482/26484]: getpeername (ftpd): Transport endpoint is not connected
Jul 25 23:23:13 combo xinetd[26482/26484]: warning: can't get client address: Connection reset by peer
Sep 12 16:39:42 combo ftpd[25297]: getpeername (ftpd): Transport endpoint is not connected
Sep 12 16:39:42 combo xinetd[25297]: warning: can't get client address: Connection reset by peer
```
These occur when the client sends a SYN, the server accepts and forks an ftpd child, but the client immediately sends a RST before the child can call `getpeername()` — classic half-open / SYN-scan or aggressive worm that RSTs after seeing the banner.

### 4.4 Unfinished FTP Handshakes ("User unknown timed out")

```
Jun 18 02:23:10 ftpd[31277]: User unknown timed out after 900 seconds
Aug  7 07:07:07 ftpd[16261]: User unknown timed out after 900 seconds
Aug 27 18:30:17–18:30:18 ftpd[24342/24345/24346/24347/24349/24350/24352/24355]: User unknown timed out after 900 seconds
```
Clients that opened TCP/21, never completed a USER command, and sat idle for 15 minutes. On Aug 27 **8 such connections timed out together** — another automated scan signature (scanner opens many concurrent TCP connections to grab banners but never finishes USER/PASS).

---

## 5. `rpc.statd` Exploitation — Format-String + Buffer Overflow (rpc.statd / statd)

### 5.1 The Smoking Gun

Lines **868–873**, **1339**, **2615–2616** (9 lines total across 3 dates) are identical `rpc.statd` `gethostbyname error` entries whose "hostname" argument is **not a hostname** but a fully-armed remote-code-execution payload. First occurrence at **Jun 13 11:55:04** (only hours after the first 23-conn FTP burst from `212.5.120.141` — same attack wave):

```
Jun 13 11:55:04 combo rpc.statd[1636]: gethostbyname error for
^X<0x18><0x18>^X<0x18><0x18>^Z<0x1a><0x1a>^Z<0x1a><0x1a>
%8x%8x%8x%8x%8x%8x%8x%8x%8x%62716x%hn%51859x%hn
\220\220\220\220\220\220\220\220 ... (~250 bytes of \220, i.e. 0x90 NOP sled)
```

### 5.2 Payload Decoding

The "hostname" sent to rpc.statd (which it unsafely passes to `gethostbyname()` / syslog; the statd vulnerability is **CVE-2000-0666** — a format-string bug in the logging of failed reverse-lookups, and the payload also matches the `rpc.statd` x86 Linux exploits published 2000–2003) decodes as:

1. **Canary / return-address padders** (`^X` / `^Z` garbage bytes): alignment/padding for the overwritten stack frame.
2. **Format-string exploit body**:
   - `%8x%8x%8x%8x%8x%8x%8x%8x%8x` — walk 9 stack-pointers up to reach the saved return address.
   - `%62716x%hn` — write `0xF4FC` (62716 = 0xF4FC) into the first `%hn` target (low 2 bytes of return address).
   - `%51859x%hn` — write another 2 bytes (high 2 bytes) — total write = classic **`%hn`-based two-shot GOT/return overwrite**.
3. **NOP sled** — ~250 bytes of `\220` (octal 220 = decimal 144 = **0x90** on x86, i.e. NOP).
4. **Shellcode** follows the NOP sled (the binary bytes at the very start that rendered as `^X…^Z…`).

This is a textbook **rpc.statd remote root format-string exploit** for Linux/x86 that was published in 2000 and widely incorporated into worms (e.g., Ramen, Lion, Slapper variants, and the "statdx" exploit).

### 5.3 Timeline of Exploit Attempts

| Date / Time (2005) | Lines | Process | Note |
|---|---:|---|---|
| **Jun 13 11:55:04–11:55:10** | 868–873 (6 attempts in 6 seconds) | `rpc.statd[1636]` | First wave, rapid fire |
| **Jun 25 09:11:48** | 1339 | `rpc.statd[1636]` | Second wave (12 days later) |
| **Jul 20 13:59:41–13:59:42** | 2615–2616 (2 attempts) | `rpc.statd[1636]` | Third wave |

Each wave uses **byte-identical payload**. The Jul 27 reboot restarts rpc.statd as PID 1618; after that no further statd exploit attempts appear in the log (capture ends Sep 14).

### 5.4 Was It Successful?

- The log shows `rpc.statd` logging the payload as a `gethostbyname error` — which means the payload reached the **format-string-vulnerable `syslog()` call** inside rpc.statd.
- There is **no direct log evidence that the exploit succeeded** (no subsequent unexpected `root` login, no new service startup). However, a successful format-string exploit would typically give the attacker a raw shell without going through PAM/logging, so success would leave **no trace in syslog**. The fact that the host continued running (with the same PID 1636) until the Jul 27 reboot is weakly reassuring but not definitive.
- rpc.statd 1.0.1 is vulnerable; the box runs `nfs-utils / rpc.statd Version 1.0.6` (per the startup line) — 1.0.6 contained partial fixes but was still affected by several statd issues, so **risk is high**.

---

## 6. Anomaly Summary — Top 5 Most Concerning Findings

Ranked by severity (likelihood × impact):

| # | Anomaly | Severity | Evidence (log lines) | Why it matters |
|---|---|:---:|---|---|
| **1** | **`rpc.statd` format-string + buffer-overflow exploit attempts** (3 waves / 9 packets, Jun 13, Jun 25, Jul 20) | **CRITICAL** | L868–L873, L1339, L2615–L2616 | Payload is a fully-weaponized **remote-root** exploit (`%8x…%hn` write-what-where + 250-byte NOP sled + x86 shellcode) targeting the rpc.statd/NFS lock service (CVE-2000-0666 class). Successful exploitation gives attacker **root** with no authentication. Attack came from an un-logged external source via rpc/portmap (UDP/111 or TCP/1024+). |
| **2** | **Distributed SSH brute-force campaign** (1,221 failures, >100 external IPs, Jun 11 → Sep 14) | **HIGH** | 1,610 sshd lines, e.g. L707–L710 (Jun 11 sagonet), L150.183.249.110 (Jul 10), 220.82.197.48 (Aug 29 80/min), 62.205.38.26 (Sep 4), 203.223.40.241 (Sep 13) | Sustained, automated password-spray against `root` (840 attempts) and default accounts (`test`, `guest`, `nobody`, `amanda`, `webmaster`). If any account had a weak password the host was compromised. Rate patterns (tens of attempts per second) prove automation / botnet. |
| **3** | **FTP parallel-scan / worm activity** (1,638 FTP conns, 23-per-second bursts from dozens of IPs) | **HIGH** | Bursts at L835–L857, L874–L896, L1629–L1651, L1719–L1741, L1742–L1764, …, Sep 13 211.107.232.1 (149 conns) | ~55 distinct source IPs each open **exactly 23** simultaneous FTP connections in a single second — classic wu-ftpd/vftpd vulnerability scanner (worm), very likely probing for SITE EXEC/glob/format-string bugs common in 2005. `getpeername: Transport endpoint is not connected` + `ANONYMOUS FTP LOGIN FROM 84.102.20.2` further indicate probing and potential warez-drop staging. |
| **4** | **Telnetd binary-payload exploit burst** (Aug 7 11:33–11:35) | **MEDIUM-HIGH** | L3286, L3592–L3614 (~20 parallel connections ending in "Invalid or incomplete multibyte or wide character" / "Connection reset by peer") | 20 simultaneous telnet sessions sending non-ASCII byte sequences is diagnostic of telnetd-exploit probes (e.g., ENV_OPT overflow, CVE-2001-0557, or later LSD telnetd exploit). telnet was also failing to bind at startup (`Address already in use` on every boot), suggesting either misconfiguration or a pre-existing rogue listener on TCP/23. |
| **5** | **Malformed DNS NOTIFY floods to BIND 9.2.3** (Jul 25 12:09–16:29) | **MEDIUM** | L2813–L2828 (16 "notify question section contains no SOA" messages at ~9-min intervals) | Possible cache-poisoning / DNS-amp reconnaissance or BIND vulnerability scanner targeting a very outdated BIND (9.2.3 is from 2003; multiple CVEs existed by 2005). BIND is running chrooted which mitigates full compromise, but cache poisoning could redirect traffic served by combo's 23 virtual DNS domains (`63.126.79.67–125`). |

### Additional lower-severity findings

- **Chronic `logrotate` ALERT every night at ~04:0x (97 events)** — not an attack per se, but means logs may be incomplete/non-rotated and should be remediated so forensic evidence is preserved.
- **3 unexpected reboots** (Jun 10, Jul 27) during the capture window — no crash messages precede them; combined with the rpc.statd and SSH activity, post-mortem review of `/var/log/wtmp`, `/var/log/secure`, and shell history is strongly recommended.
- **`su` sessions for account `htt`** (6 sessions) — unusual service username worth verifying (potential webserver user, but should be confirmed legitimate).
- **xinetd telnet bind failure at every boot** (`Address already in use`) — may indicate a competing in.telnetd or backdoor already bound to TCP/23 before xinetd starts; warrants a `netstat -anp | grep :23` review.

---

## Recommendations (immediate actions)

1. **Assume rpc.statd was compromised** until proven otherwise (exploit reached the vulnerable `syslog()` call). Rebuild the host from known-good media, change all credentials, and audit for rootkits (chkrootkit / rkhunter). Block portmap/rpc (111/udp+tcp) and statd (1024/tcp, 1034/udp) at the perimeter; upgrade to `nfs-utils >= 1.0.10`.
2. **Disable password-based root SSH login** (`PermitRootLogin no`), enforce key-based auth, install fail2ban/DenyHosts, and rate-limit TCP/22. The 1,221 failed attempts would have been 99 %+ stopped by these controls.
3. **Disable FTP if not required**, or at minimum require TLS, disallow anonymous FTP, and place ftpd behind connection-rate limits. The 23-conn/sec bursts are automation and should be blocked at the firewall by source-IP rate limiting.
4. **Disable telnet entirely** (TCP/23) — any telnet exposure in 2005 is unsafe, and the binary payload probes confirm it was actively targeted. Investigate the "Address already in use" bind failure.
5. **Patch BIND** (9.2.3 → latest supported version), restrict zone transfers (`allow-transfer`), disable IN-notify from non-slaves, and consider moving DNS off the shared webhost.
6. **Fix `logrotate`** (exit code 1 every day) so logs rotate correctly and history is retained for future forensics.
7. **Audit the `htt` account** and any unusual SUID binaries / crontab entries added between Jun 13 and Jul 27.
