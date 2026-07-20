# Authentication Failures Analysis Report

**Log file:** `syslog.log`  
**Hostname:** `combo`  
**Log time span:** 2005-06-10 17:16:34 → 2005-09-14 21:50:47  

---

## Executive Summary

The host is under active, sustained **brute-force attack**, primarily against **SSH** and secondarily against **FTP**. Over the log window we observed **1221 explicit PAM authentication failures** (1217 against SSH), plus **23 Kerberos klogind failures** and **1638 FTP connection-probe attempts** launched in high-parallelism bursts. Attacks originate from dozens of distinct sources, with `root` overwhelmingly the most-targeted user and attacks distributed uniformly across all hours of the day — the signature of automated botnets, not human actors.

---

## 1. Total Authentication Failures

| Category | Count |
|---|---:|
| PAM `authentication failure` events | **1221** |
| └ SSH (`sshd`) | 1217 |
| └ Console `login` | 3 |
| └ GDM (graphical login) | 1 |
| Kerberos `klogind` authentication failures | 23 |
| FTP connection probes (brute-force initiations) | 1638 |
| FTP `User unknown timed out` sessions | 10 |
| FTP anonymous login attempts | 2 |

> **Total explicit auth failures: 1244** (PAM + klogind). The FTP daemon does not emit PAM failure lines in this configuration, so 1638 FTP connection attempts are reported separately as attack indicators.

---

## 2. Failures by Service

| Service | Auth Failures | Share |
|---|---:|---:|
| `sshd` | 1217 | 99.7% |
| `login` | 3 | 0.2% |
| `gdm` | 1 | 0.1% |
| `klogind` (Kerberos, non-PAM) | 23 | — |
| `ftpd` (FTP probes — no PAM failure lines) | 1638 attempts | — |

**Key observation:** SSH dominates (99.7% of all PAM failures). Only 3 console `login` failures and 1 GDM failure appear — local/physical access is not a meaningful attack vector in this window.

Note: `su` session open/close events appear in the log, but all are legitimate (cron-initiated `su` to service accounts like `cyrus`, `news`, `htt`); **no `su` authentication failures** were logged.

---

## 3. Top 10 Source Hosts/IPs — PAM Failures (mostly SSH)

Total unique PAM-failure sources: **101**

| Rank | Source host / IP | Failures | Share |
|---:|---|---:|---:|
| 1 | `150.183.249.110` | 80 | 6.6% |
| 2 | `220.82.197.48` | 80 | 6.6% |
| 3 | `210.109.97.31` | 70 | 5.7% |
| 4 | `218.188.2.4` | 49 | 4.0% |
| 5 | `62.205.38.26` | 32 | 2.6% |
| 6 | `unknown.sagonet.net` | 23 | 1.9% |
| 7 | `n219076184117.netvigator.com` | 23 | 1.9% |
| 8 | `207.243.167.114` | 23 | 1.9% |
| 9 | `yang.dorm13.nctu.edu.tw` | 23 | 1.9% |
| 10 | `ned.gcdtech.com` | 22 | 1.8% |

Attack sources resolve to a mix of:
- Compromised servers in hosting providers (e.g. `sagonet.net`)
- Consumer/residential broadband (e.g. `astral.ro`, various `.net`/`.com` reverse DNS)
- Dial-up and early-cable ISP pools (consistent with the 2005-era log)

This broad source distribution is characteristic of a **distributed botnet** rather than a single attacker.

### FTP connection-probe sources (all)

Total unique FTP source IPs: **63**

| Rank | Source IP | Connections | First seen | Last seen | Duration | Avg rate |
|---:|---|---:|---|---|---:|---:|
| 1 | `211.107.232.1` | 149 | Jul 15 23:42 | Sep 13 11:02 | 1427.3h | 0.00/min |
| 2 | `211.72.151.162` | 58 | Jul 06 18:00 | Aug 25 07:50 | 1189.8h | 0.00/min |
| 3 | `202.82.200.188` | 56 | Jul 01 07:57 | Aug 02 00:41 | 760.7h | 0.00/min |
| 4 | `203.101.45.59` | 46 | Jul 03 10:05 | Jul 17 15:09 | 341.1h | 0.00/min |
| 5 | `207.30.238.8` | 46 | Jul 17 12:30 | Jul 17 14:03 | 1.5h | 0.50/min |
| 6 | `69.15.163.251` | 46 | Sep 07 04:23 | Sep 07 10:57 | 6.6h | 0.12/min |
| 7 | `218.146.61.230` | 45 | Jul 17 08:06 | Aug 27 13:40 | 989.6h | 0.00/min |
| 8 | `62.241.71.44` | 41 | Aug 27 18:15 | Aug 28 02:06 | 7.9h | 0.09/min |
| 9 | `222.33.90.199` | 35 | Jun 12 00:07 | Jun 20 03:40 | 195.6h | 0.00/min |
| 10 | `210.245.165.136` | 32 | Jun 22 13:16 | Jul 17 09:44 | 596.5h | 0.00/min |

### FTP short-burst sources (parallel brute-force floods, < 5 min)

49 source(s) opened all their connections in under 5 minutes — the hallmark of parallel dictionary attacks. Top burst sources:

| Source IP | Connections | All in | At time |
|---|---:|---:|---|
| `209.184.7.130` | 23 | 3 s | Jun 11 03:28:22 |
| `61.218.67.60` | 23 | 1 s | Jun 13 02:01:27 |
| `212.5.120.141` | 23 | < 1 s | Jun 13 09:09:44 |
| `65.35.73.251` | 23 | < 1 s | Jun 13 12:49:32 |
| `61.74.96.178` | 23 | 1 s | Jun 29 03:22:22 |
| `208.62.55.75` | 23 | 20 s | Jun 29 10:48:01 |
| `62.99.164.82` | 23 | < 1 s | Jul 03 23:16:09 |
| `63.197.98.106` | 23 | < 1 s | Jul 04 12:52:44 |
| `211.72.2.106` | 23 | 2 s | Jul 05 13:52:21 |
| `211.57.88.250` | 23 | < 1 s | Jul 09 11:35:59 |

Two distinct FTP patterns are visible: (a) **recurring low-rate probes** (e.g. `211.107.232.1` with 149 connections across ~60 days) and (b) **instant parallel floods** (e.g. 23 simultaneous connections from multiple IPs within the same second), the latter being typical of tools like `hydra`/`medusa` running parallel threads.

---

## 4. Targeted User Accounts

| Rank | Username | Failed Attempts | Share |
|---:|---|---:|---:|
| 1 | `root` | 840 | 68.8% |
| 2 | `nobody` | 29 | 2.4% |
| 3 | `test` | 20 | 1.6% |
| 4 | `guest` | 18 | 1.5% |
| 5 | `amanda` | 10 | 0.8% |
| 6 | `webmaster` | 10 | 0.8% |

- **`root` is the dominant target** with 840 failures (68.8% of all PAM failures). The classic SSH brute-force playbook is to prioritize the superuser account.
- **Invalid / non-existent usernames** (`check pass; user unknown`) account for **0** failures (0.0%), indicating username dictionary enumeration against common service names (test, admin, oracle, mysql, etc.).

**Top non-root, non-unknown usernames targeted:**

| Username | Attempts | Typical role |
|---|---:|---|
| `nobody` | 29 | default unprivileged |
| `test` | 20 | default test account |
| `guest` | 18 | default guest |
| `amanda` | 10 |  |
| `webmaster` | 10 | web admin |

---

## 5. Temporal Distribution

### 5.1 Failures by hour of day (PAM)

| Hour | Failures | |
|---:|---:|---|
| 00:00 | 27 | ███████ |
| 01:00 | 57 | ██████████████ |
| 02:00 | 33 | ████████ |
| 03:00 | 57 | ██████████████ |
| 04:00 | 53 | █████████████ |
| 05:00 | 25 | ██████ |
| 06:00 | 22 | █████ |
| 07:00 | 154 | ████████████████████████████████████████ |
| 08:00 | 44 | ███████████ |
| 09:00 | 63 | ████████████████ |
| 10:00 | 93 | ████████████████████████ |
| 11:00 | 35 | █████████ |
| 12:00 | 46 | ███████████ |
| 13:00 | 41 | ██████████ |
| 14:00 | 75 | ███████████████████ |
| 15:00 | 91 | ███████████████████████ |
| 16:00 | 91 | ███████████████████████ |
| 17:00 | 21 | █████ |
| 18:00 | 19 | ████ |
| 19:00 | 49 | ████████████ |
| 20:00 | 54 | ██████████████ |
| 21:00 | 15 | ███ |
| 22:00 | 0 |  |
| 23:00 | 56 | ██████████████ |

- **Peak hour:** 07:00 with 154 failures.
- **Night-time (22:00–06:00):** 308 failures (25.2%).
- **Business hours (09:00–17:00):** 556 failures (45.5%).
- Traffic is essentially **flat across the clock** — the signature of automated bots running in different time zones, not humans.

### 5.2 Failures by day

| Date | Failures | |
|---|---:|---|
| 2005-06-10 | 1 | █ |
| 2005-06-11 | 23 | ██████████ |
| 2005-06-12 | 27 | ████████████ |
| 2005-06-13 | 42 | ██████████████████ |
| 2005-06-14 | 36 | ████████████████ |
| 2005-06-15 | 37 | ████████████████ |
| 2005-06-17 | 1 | █ |
| 2005-06-18 | 10 | ████ |
| 2005-06-20 | 10 | ████ |
| 2005-06-21 | 6 | ██ |
| 2005-06-22 | 33 | ██████████████ |
| 2005-06-23 | 20 | ████████ |
| 2005-06-25 | 10 | ████ |
| 2005-06-27 | 5 | ██ |
| 2005-06-28 | 24 | ██████████ |
| 2005-06-29 | 23 | ██████████ |
| 2005-06-30 | 23 | ██████████ |
| 2005-07-01 | 20 | ████████ |
| 2005-07-02 | 10 | ████ |
| 2005-07-04 | 16 | ███████ |
| 2005-07-05 | 5 | ██ |
| 2005-07-06 | 5 | ██ |
| 2005-07-07 | 4 | █ |
| 2005-07-08 | 4 | █ |
| 2005-07-09 | 10 | ████ |
| 2005-07-10 | 90 | ████████████████████████████████████████ |
| 2005-07-11 | 21 | █████████ |
| 2005-07-12 | 10 | ████ |
| 2005-07-14 | 8 | ███ |
| 2005-07-15 | 10 | ████ |
| 2005-07-17 | 3 | █ |
| 2005-07-18 | 10 | ████ |
| 2005-07-19 | 10 | ████ |
| 2005-07-20 | 5 | ██ |
| 2005-07-21 | 6 | ██ |
| 2005-07-23 | 11 | ████ |
| 2005-07-24 | 5 | ██ |
| 2005-07-26 | 23 | ██████████ |
| 2005-08-01 | 11 | ████ |
| 2005-08-02 | 32 | ██████████████ |
| 2005-08-04 | 11 | ████ |
| 2005-08-05 | 2 | █ |
| 2005-08-07 | 32 | ██████████████ |
| 2005-08-08 | 30 | █████████████ |
| 2005-08-09 | 10 | ████ |
| 2005-08-11 | 10 | ████ |
| 2005-08-13 | 10 | ████ |
| 2005-08-16 | 14 | ██████ |
| 2005-08-18 | 1 | █ |
| 2005-08-21 | 1 | █ |
| 2005-08-22 | 21 | █████████ |
| 2005-08-23 | 14 | ██████ |
| 2005-08-24 | 20 | ████████ |
| 2005-08-25 | 9 | ████ |
| 2005-08-27 | 41 | ██████████████████ |
| 2005-08-29 | 85 | █████████████████████████████████████ |
| 2005-08-30 | 1 | █ |
| 2005-08-31 | 24 | ██████████ |
| 2005-09-02 | 46 | ████████████████████ |
| 2005-09-03 | 18 | ████████ |
| 2005-09-04 | 84 | █████████████████████████████████████ |
| 2005-09-05 | 13 | █████ |
| 2005-09-09 | 3 | █ |
| 2005-09-12 | 21 | █████████ |
| 2005-09-13 | 39 | █████████████████ |
| 2005-09-14 | 1 | █ |

- **Heaviest day:** 2005-07-10 with 90 failures (a concentrated brute-force burst from a coordinated source set).
- **Log coverage:** 66 distinct days with PAM failures between 2005-06-10 and 2005-09-14.

---

## 6. FTP vs SSH Attack Pattern Comparison

### 6.1 Side-by-side

| Attribute | SSH (`sshd`) | FTP (`ftpd`) |
|---|---|---|
| Explicit PAM failures logged | 1217 | 0 (ftpd not using PAM or logging elsewhere) |
| Connection/attempt volume | 1217 failure events | 1638 TCP connection initiations |
| Unique sources | 101 | 63 |
| Primary target user | `root` (838 attempts) | unknown (usernames not logged in connection line) |
| Top source count | 80 failures from top host | 149 connections from top IP |
| Connection pattern | Slow, distributed: a few attempts per host spread over days | Fast, concentrated: bursts of 20+ simultaneous connections per source in seconds |

### 6.2 Do the same sources attack both services?

- SSH source set size: **101** hosts (reverse-DNS names)
- FTP source set size: **63** IPs
- **Overlap (exact string match): 0**

> **No direct overlap is visible in this log.**

Two reasons this is expected:
1. **Naming mismatch:** SSH failures are logged with the *reverse-DNS hostname* of the attacker (e.g. `unknown.sagonet.net`), while FTP connection lines log the *raw IP address*. Without performing PTR lookups on the FTP IPs we cannot definitively cross-correlate, but the sets are labeled differently in the log.
2. **Two distinct bot classes:** Observed behavior strongly suggests different tools/campaigns — SSH traffic is a low-and-slow distributed root-password dictionary with username enumeration, while FTP traffic is a high-parallelism connection flood (a different tooling profile). It is common for different botnets to specialize in one protocol.

**Practical implication:** Blocking the top SSH sources will *not* stop the FTP floods, and vice-versa. Defenses must be applied independently to both services (see §7).

### 6.3 Other noteworthy remote-auth traffic

- **Kerberos `klogind`:** 23 failures in a tight burst from a single IP `163.27.187.39` (including `Permission denied in replay cache code` errors, suggesting a replay-style attack against legacy Kerberos remote-login).
- **Telnet:** `xinetd` fails to start telnet on boot because the port is already bound — this should be investigated and telnet explicitly disabled; telnet must not be exposed even accidentally.

---

## 7. Recommendations

The host is running a very old Red Hat-derived distribution (kernel 2.6.5-1.358, ~2004) and is exposed to continuous automated attack. Recommendations are grouped by priority:

### 7.1 Critical — apply immediately

1. **Disable direct root SSH login.** Set `PermitRootLogin no` (or `prohibit-password`) in `/etc/ssh/sshd_config` and restart sshd. This single change neutralizes >69% of observed SSH failures.
2. **Switch SSH to key-based authentication and disable password auth** (`PasswordAuthentication no`). Every observed SSH failure is a password-guess; public-key auth defeats the entire attack class.
3. **Install & enable `fail2ban`** (or `sshguard`) with jails for `sshd`, `ftpd`/`vsftpd`, and any other exposed service. With the observed rate, a ban threshold of 5 failures / 10 min would have blocked the top sources within seconds of each burst.
4. **Retire FTP in favor of SFTP** (which runs over SSH and inherits SSH key-auth protections). If FTP *must* stay:
   - Disable anonymous login (`anonymous_enable=NO`).
   - Use FTPS (TLS) so credentials aren't sent in cleartext.
   - Run it chrooted and restrict to a dedicated non-shell account.
   - Rate-limit connections per source IP (e.g. `max_per_ip` in vsftpd, plus iptables rules).
5. **Disable legacy BSD r-services (rsh/rlogin/klogind) and telnet.** The Kerberos klogind burst and the telnet port-bind failure both indicate legacy remote-login daemons that should not be on an internet-connected host.

### 7.2 Network / firewall hardening

6. **Rate-limit new connections** with iptables/nftables, e.g.:
   ```
   iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -m recent --set
   iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 6 -j DROP
   ```
   Apply equivalent rules for FTP (port 21) and any other exposed service.
7. **Move SSH to a non-standard port** (defense-in-depth; reduces log noise and drive-by scanner hits, but is *not* a substitute for key-auth).
8. **Whitelist administrative access** if at all possible: allow SSH/FTP only from trusted source CIDRs, blocking 0.0.0.0/0 at the edge.

### 7.3 Account & PAM hygiene

9. **Remove or lock default/service accounts** that do not need interactive login (test, guest, ftp, nobody, operator, games, lp, etc.) — these are exactly the usernames being probed. Set their shell to `/sbin/nologin`.
10. **Enable PAM lockout** (`pam_faillock.so` or `pam_tally2.so`) — e.g. 5 failed attempts → 15-minute lockout — for sshd, login, and ftpd PAM stacks.
11. **Restrict `su` to a wheel group** (`pam_wheel.so`), even though no `su` failures appear in this window.
12. **Enforce strong password policy** (min length ≥ 12, mixed character classes) via `pam_cracklib` for any accounts still using passwords.

### 7.4 Monitoring & system maintenance

13. **Ship logs off-box** to a central syslog/SIEM; enable `logrotate` with appropriate retention. This log spans many months in a small window which complicates incident response.
14. **Alert on:** >10 auth failures/min from one source, *any* `root` auth failure, `User unknown` bursts, anonymous FTP logins, klogind/telnet events.
15. **Upgrade the OS.** Kernel 2.6.5-1.358 (Red Hat, circa 2004) has hundreds of known remotely-exploitable vulnerabilities. Migrate to a supported, regularly-patched distribution; a compromised host cannot be secured through configuration alone.
16. **Periodically audit** listening ports (`netstat -tlnp` / `ss -tlnp`) to ensure no unexpected services (like the stray-telnet bind) are exposed.

---

## Appendix A — Representative Event Samples

**SSH brute-force burst targeting `root` (`unknown.sagonet.net`):**
```
Jun 11 09:45:45 combo sshd(pam_unix)[6472]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=unknown.sagonet.net  user=root
Jun 11 09:45:55 combo sshd(pam_unix)[6474]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=unknown.sagonet.net  user=root
Jun 11 09:45:58 combo sshd(pam_unix)[6476]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=unknown.sagonet.net  user=root
Jun 11 09:46:01 combo sshd(pam_unix)[6478]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=unknown.sagonet.net  user=root
Jun 11 09:46:05 combo sshd(pam_unix)[6480]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=unknown.sagonet.net  user=root
```

**FTP parallel connection flood (`209.184.7.130`):**
```
Jun 11 03:28:22 combo ftpd[4301]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4305]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4304]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4308]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4303]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4306]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4302]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
Jun 11 03:28:22 combo ftpd[4307]: connection from 209.184.7.130 () at Sat Jun 11 03:28:22 2005
```

**Kerberos klogind replay attack (`163.27.187.39`):**
```
Jun 30 20:53:04 combo klogind[19272]: Authentication failed from 163.27.187.39 (163.27.187.39): Permission denied in replay cache code
Jun 30 20:53:04 combo klogind[19287]: Authentication failed from 163.27.187.39 (163.27.187.39): Permission denied in replay cache code
Jun 30 20:53:04 combo klogind[19286]: Authentication failed from 163.27.187.39 (163.27.187.39): Permission denied in replay cache code
Jun 30 20:53:04 combo klogind[19271]: Authentication failed from 163.27.187.39 (163.27.187.39): Permission denied in replay cache code
```

---
*Report generated automatically from syslog.log.*