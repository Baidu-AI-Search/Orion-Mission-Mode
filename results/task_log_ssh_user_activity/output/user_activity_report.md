# OpenSSH User Authentication Activity Report

**Log source:** `auth.log`  
**Time window analysed:** 2024-12-10 06:55:46 → 2024-12-10 10:59:43 (≈ 4 h 4 min)  
**Report generated:** 2026-07-16 15:44:55

---

## Executive Summary

The SSH daemon on host `LabSZ` was the target of a **large-scale brute-force / credential-stuffing campaign** during the observation window. A total of **374 authentication attempts** were recorded against **62 unique usernames** from **23 distinct source IPs**, with only **1 successful login** (user `fztu`). The remaining **373 attempts (99.7%)** failed, and the vast majority of traffic was automated: dictionary-based username guessing, reverse-DNS anomalies (`85` POSSIBLE BREAK-IN ATTEMPT warnings), and three sessions terminated for too many authentication failures.

| Metric | Value |
|---|---|
| Log lines parsed | 1500 |
| Unique usernames probed | 62 |
| – Valid system users targeted | 7 |
| – Invalid/non-existent users probed | 55 |
| Total authentication attempts | 374 |
| – Failed passwords | 373 |
| – Successful authentications | 1 |
| Unique source IP addresses | 23 |
| Reverse-DNS lookup failures (break-in warnings) | 85 |
| Sessions closed pre-auth | 33 |
| Sessions dropped (Too many auth failures) | 3 |

---

## 1. All Usernames Attempted

Every username that appeared in an SSH authentication event is listed below (**62 total**). They are grouped by validity class.

### Valid system users (7)

These usernames exist on the target system (the daemon accepted them as real accounts and proceeded to check the password/key):

`ftp`, `fztu`, `git`, `mysql`, `root`, `sshd`, `uucp`

### Invalid / non-existent users (55)

These usernames were explicitly rejected by the system as unknown (`Invalid user` / `user unknown` in PAM). They represent guesses by attackers, not real accounts:

`0`, `123`, `1234`, `123456`, `FILTER`, `Management`, `PlcmSpIp`, `abc`, `admin`, `anonymous`, `api`, `boot`, `bssh`, `butter`, `chen`, `cheng`, `cisco`, `cyrus`, `default`, `deploy`, `dff`, `eoor`, `ftpuser`, `ghost`, `guest`, `ingrid`, `inspur`, `jay`, `magnos`, `matlab`, `monitor`, `nagios`, `nagios1`, `operator`, `oracle`, `oralce`, `pgadmin`, `pi`, `postgres`, `postgres1`, `redhat`, `support`, `ted`, `test`, `test1`, `test2`, `test9`, `ubnt`, `ubuntu`, `user`, `utsims`, `vnc`, `webmaster`, `www`, `zhangyan`

---

## 2. Valid vs Invalid User Classification

| Class | Count | % of usernames | Attempts against | % of attempts |
|---|---|---|---|---|
| **Valid users** | 7 | 11.3% | 253 | 67.6% |
| **Invalid users** | 55 | 88.7% | 121 | 32.4% |

> **Interpretation:** Only 11% of the usernames probed are real accounts, but they absorbed **68%** of the traffic — almost all of it aimed at `root`. This is a classic pattern: a small handful of high-value real accounts plus a wide dictionary sweep of common/other service names.

---

## 3. Per-User Authentication Summary

For every username observed, the table below shows total authentication attempts, source IPs (with per-IP attempt counts in parentheses), success/failure breakdown, and the first/last time the username was attempted.

### 3.1 Valid system users

| Username | Attempts | Failed | Succeeded | Source IPs | First seen | Last seen |
|---|---:|---:|---:|---|---|---|
| `root` | 239 | 239 | 0 | `183.62.140.253` (139), `187.141.143.180` (46), `112.95.230.3` (24), `123.235.32.19` (7), `106.5.5.195` (6), `5.36.59.76` (6), `60.2.12.12` (5), `103.99.0.122` (4), +2 more | 2024-12-10 07:13:43 | 2024-12-10 10:59:43 |
| `uucp` | 4 | 4 | 0 | `103.207.39.16` (1), `103.207.39.212` (1), `103.99.0.122` (1), `195.154.37.122` (1) | 2024-12-10 07:51:20 | 2024-12-10 09:18:33 |
| `ftp` | 3 | 3 | 0 | `103.99.0.122` (1), `187.141.143.180` (1), `5.188.10.180` (1) | 2024-12-10 08:26:12 | 2024-12-10 09:18:18 |
| `git` | 3 | 3 | 0 | `187.141.143.180` (2), `183.62.140.253` (1) | 2024-12-10 09:18:00 | 2024-12-10 10:55:49 |
| `mysql` | 2 | 2 | 0 | `187.141.143.180` (2) | 2024-12-10 09:19:22 | 2024-12-10 09:19:28 |
| `fztu` | 1 | 0 | 1 | `119.137.62.142` (1) | 2024-12-10 09:32:20 | 2024-12-10 09:32:20 |
| `sshd` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:11:52 | 2024-12-10 09:11:52 |

### 3.2 Invalid / non-existent users

| Username | Attempts | Failed | Succeeded | Source IPs | First seen | Last seen |
|---|---:|---:|---:|---|---|---|
| `admin` | 41 | 41 | 0 | `185.190.58.151` (15), `5.188.10.180` (11), `103.99.0.122` (7), `119.4.203.64` (6), `103.207.39.16` (1), `103.207.39.212` (1) | 2024-12-10 08:24:58 | 2024-12-10 10:14:13 |
| `oracle` | 6 | 6 | 0 | `187.141.143.180` (4), `183.62.140.253` (2) | 2024-12-10 09:17:11 | 2024-12-10 10:55:45 |
| `support` | 5 | 5 | 0 | `103.207.39.16` (1), `103.207.39.165` (1), `103.207.39.212` (1), `103.99.0.122` (1), `195.154.37.122` (1) | 2024-12-10 07:51:12 | 2024-12-10 09:18:30 |
| `test` | 4 | 4 | 0 | `103.99.0.122` (1), `183.62.140.253` (1), `187.141.143.180` (1), `52.80.34.196` (1) | 2024-12-10 07:55:55 | 2024-12-10 10:55:43 |
| `inspur` | 3 | 3 | 0 | `183.136.162.51` (2), `175.102.13.6` (1) | 2024-12-10 07:42:49 | 2024-12-10 10:32:30 |
| `matlab` | 3 | 3 | 0 | `52.80.34.196` (3) | 2024-12-10 08:44:20 | 2024-12-10 10:21:09 |
| `123` | 2 | 2 | 0 | `183.62.140.253` (1), `185.190.58.151` (1) | 2024-12-10 09:07:56 | 2024-12-10 10:55:56 |
| `1234` | 2 | 2 | 0 | `103.99.0.122` (1), `5.188.10.180` (1) | 2024-12-10 08:24:50 | 2024-12-10 09:11:34 |
| `default` | 2 | 2 | 0 | `5.188.10.180` (2) | 2024-12-10 08:25:58 | 2024-12-10 08:26:03 |
| `deploy` | 2 | 2 | 0 | `187.141.143.180` (2) | 2024-12-10 09:18:28 | 2024-12-10 09:18:35 |
| `ftpuser` | 2 | 2 | 0 | `103.99.0.122` (2) | 2024-12-10 09:12:30 | 2024-12-10 09:12:44 |
| `guest` | 2 | 2 | 0 | `103.99.0.122` (1), `5.188.10.180` (1) | 2024-12-10 08:26:22 | 2024-12-10 09:12:03 |
| `magnos` | 2 | 2 | 0 | `187.141.143.180` (2) | 2024-12-10 09:19:37 | 2024-12-10 09:19:45 |
| `ubuntu` | 2 | 2 | 0 | `183.62.140.253` (1), `187.141.143.180` (1) | 2024-12-10 09:18:11 | 2024-12-10 10:55:47 |
| `user` | 2 | 2 | 0 | `103.99.0.122` (2) | 2024-12-10 09:11:26 | 2024-12-10 09:12:06 |
| `webmaster` | 2 | 2 | 0 | `173.234.31.186` (2) | 2024-12-10 06:55:46 | 2024-12-10 07:08:30 |
| `0` | 1 | 1 | 0 | `5.188.10.180` (1), `181.214.87.4`, `185.190.58.151` | 2024-12-10 08:24:40 | 2024-12-10 09:48:23 |
| `123456` | 1 | 1 | 0 | `183.62.140.253` (1) | 2024-12-10 10:55:52 | 2024-12-10 10:55:54 |
| `FILTER` | 1 | 1 | 0 | `104.192.3.34` (1) | 2024-12-10 09:31:22 | 2024-12-10 09:31:24 |
| `Management` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:12:38 | 2024-12-10 09:12:40 |
| `PlcmSpIp` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:12:35 | 2024-12-10 09:12:37 |
| `abc` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:41 | 2024-12-10 09:17:43 |
| `anonymous` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:11:39 | 2024-12-10 09:11:40 |
| `api` | 1 | 1 | 0 | `185.190.58.151` (1) | 2024-12-10 09:12:57 | 2024-12-10 09:12:59 |
| `boot` | 1 | 1 | 0 | `183.62.140.253` (1) | 2024-12-10 10:55:49 | 2024-12-10 10:55:51 |
| `bssh` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:19:15 | 2024-12-10 09:19:17 |
| `butter` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:16:59 | 2024-12-10 09:17:00 |
| `chen` | 1 | 1 | 0 | `202.100.179.208` (1) | 2024-12-10 07:11:42 | 2024-12-10 07:11:44 |
| `cheng` | 1 | 1 | 0 | `202.100.179.208` (1) | 2024-12-10 10:55:07 | 2024-12-10 10:55:10 |
| `cisco` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:11:56 | 2024-12-10 09:11:57 |
| `cyrus` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:20:00 | 2024-12-10 09:20:02 |
| `dff` | 1 | 1 | 0 | `183.62.140.253` (1) | 2024-12-10 10:54:29 | 2024-12-10 10:54:31 |
| `eoor` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:16:48 | 2024-12-10 09:16:50 |
| `ghost` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:18:05 | 2024-12-10 09:18:06 |
| `ingrid` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:19:49 | 2024-12-10 09:19:51 |
| `jay` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:19:54 | 2024-12-10 09:19:57 |
| `monitor` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:12:28 | 2024-12-10 09:12:30 |
| `nagios` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:31 | 2024-12-10 09:17:33 |
| `nagios1` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:18:52 | 2024-12-10 09:18:54 |
| `operator` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:12:06 | 2024-12-10 09:12:08 |
| `oralce` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:18:40 | 2024-12-10 09:18:42 |
| `pgadmin` | 1 | 1 | 0 | `112.95.230.3` (1) | 2024-12-10 07:28:03 | 2024-12-10 07:28:05 |
| `pi` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:12:33 | 2024-12-10 09:12:35 |
| `postgres` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:26 | 2024-12-10 09:17:28 |
| `postgres1` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:18:58 | 2024-12-10 09:19:00 |
| `redhat` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:05 | 2024-12-10 09:17:07 |
| `ted` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:46 | 2024-12-10 09:17:48 |
| `test1` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:19:04 | 2024-12-10 09:19:06 |
| `test2` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:19:09 | 2024-12-10 09:19:11 |
| `test9` | 1 | 1 | 0 | `52.80.34.196` (1) | 2024-12-10 07:07:38 | 2024-12-10 07:07:45 |
| `ubnt` | 1 | 1 | 0 | `103.99.0.122` (1) | 2024-12-10 09:11:45 | 2024-12-10 09:11:47 |
| `utsims` | 1 | 1 | 0 | `112.95.230.3` (1) | 2024-12-10 07:28:25 | 2024-12-10 07:28:28 |
| `vnc` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:52 | 2024-12-10 09:17:54 |
| `www` | 1 | 1 | 0 | `187.141.143.180` (1) | 2024-12-10 09:17:36 | 2024-12-10 09:17:38 |
| `zhangyan` | 1 | 1 | 0 | `183.62.140.253` (1) | 2024-12-10 10:54:27 | 2024-12-10 10:54:29 |

---

## 4. Most Targeted Users (by failed attempts)

Ranking of usernames by the number of failed password attempts (valid and invalid combined). This identifies which accounts the attackers considered highest-value.

| Rank | Username | Valid? | Failed attempts | % of all failures | Distinct IPs |
|---:|---|---|---:|---:|---:|
| 1 | `root` | ✅ Yes | 239 | 64.1% | 10 |
| 2 | `admin` | ❌ No | 41 | 11.0% | 6 |
| 3 | `oracle` | ❌ No | 6 | 1.6% | 2 |
| 4 | `support` | ❌ No | 5 | 1.3% | 5 |
| 5 | `test` | ❌ No | 4 | 1.1% | 4 |
| 6 | `uucp` | ✅ Yes | 4 | 1.1% | 4 |
| 7 | `ftp` | ✅ Yes | 3 | 0.8% | 3 |
| 8 | `git` | ✅ Yes | 3 | 0.8% | 2 |
| 9 | `inspur` | ❌ No | 3 | 0.8% | 2 |
| 10 | `matlab` | ❌ No | 3 | 0.8% | 1 |
| 11 | `123` | ❌ No | 2 | 0.5% | 2 |
| 12 | `1234` | ❌ No | 2 | 0.5% | 2 |
| 13 | `default` | ❌ No | 2 | 0.5% | 1 |
| 14 | `deploy` | ❌ No | 2 | 0.5% | 1 |
| 15 | `ftpuser` | ❌ No | 2 | 0.5% | 1 |
| 16 | `guest` | ❌ No | 2 | 0.5% | 2 |
| 17 | `magnos` | ❌ No | 2 | 0.5% | 1 |
| 18 | `mysql` | ✅ Yes | 2 | 0.5% | 1 |
| 19 | `ubuntu` | ❌ No | 2 | 0.5% | 2 |
| 20 | `user` | ❌ No | 2 | 0.5% | 1 |

**Key observations:**

- **`root` dominates** with **239 failed attempts (64.1% of all failures)** from 10 different IPs. It is by far the primary target.
- **`admin`** is the most-attacked invalid username (**41 failures**, 6 IPs), reflecting a generic "default admin" dictionary entry.
- Service/daemon names — `oracle`, `mysql`, `ftp`, `git`, `uucp`, `postgres`, `nagios`, `cisco`, `vnc`, `matlab`, `pgadmin`, `ubnt`, `PlcmSpIp`, `inspur` — collectively account for dozens of attempts, indicating scanners looking for weakly-secured service accounts.
- The **only successful login** was by user `fztu` from `119.137.62.142` at 09:32:20 using password authentication — this is the sole legitimate (or at least non-failed) session in the window.

---

## 5. Attacker Username Patterns & Dictionary Analysis

The invalid-username set leaves a clear signature of **automated SSH dictionary / botnet wordlists**. The 55 non-existent usernames can be grouped into the following categories:

### 5.1 Common default / superuser accounts
- `root`, `admin`, `user`, `default`, `operator`, `guest`, `anonymous`, `www`

These are the first guesses in virtually every SSH brute-force wordlist and account for the bulk of invalid-account traffic (`admin` alone has 41 failures).

### 5.2 Test / demo accounts
- `test`, `test1`, `test2`, `test9`, `pi`, `ubuntu`, `redhat`, `123`, `1234`, `123456`, `0`, `abc`, `dff`, `eoor`

A classic small-integer / short-password / default-distro-user cluster. `pi` targets Raspberry Pi default logins; `ubuntu`/`redhat` target cloud images; `123…` strings are lazy password-as-username combos.

### 5.3 Software / service / database accounts
- Databases: `oracle`, `postgres`, `postgres1`, `mysql`, `oralce` (typo for oracle)
- DevOps / version control: `git`, `deploy`, `api`, `ftp`, `ftpuser`, `webmaster`
- Monitoring / IT admin: `nagios`, `nagios1`, `monitor`, `cisco`, `matlab`, `pgadmin`, `vnc`, `inspur`, `PlcmSpIp` (Polycom VoIP default), `ubnt` (Ubiquiti), `Management`, `FILTER`, `bssh`
- Network / hardware: `cyrus`, `boot`, `sshd` (attacker probed the daemon's own account name)

### 5.4 Human / personal names
- `support`, `chen`, `cheng`, `zhangyan`, `jay`, `ted`, `ingrid`, `magnos`, `butter`, `ghost`

A small number of anglo/european first names plus **Chinese-language surnames** (`chen`, `cheng`, `zhangyan`) — evidence of geographically targeted scanning, likely from/against Chinese-speaking infrastructure (the host `LabSZ` and several Chinese IP ranges support this).

### 5.5 Bot behaviour signatures

- **Username `oralce`** is a well-known typo in popular Chinese SSH bot dictionaries (the correct spelling is `oracle`), indicating a commodity wordlist rather than manual attack.
- **Sequential test accounts** (`test`, `test1`, `test2`; `nagios`/`nagios1`; `postgres`/`postgres1`) are strong indicators of an auto-generated dictionary.
- **Burst pattern from single IPs**: e.g. `183.62.140.253` fired 149 attempts (mostly `root` and a dictionary sweep), `187.141.143.180` fired 80 attempts — classic high-rate bot behaviour.
- **Reverse-DNS failures**: 85 sessions triggered `POSSIBLE BREAK-IN ATTEMPT` because the forward-confirmed reverse DNS (FCrDNS) check failed, a typical sign of compromised bots / cloud / VPS hosts.

> **Conclusion — yes, attackers are using a dictionary.** Multiple overlapping commodity wordlists (a generic "top defaults" list, a services/database list, and a small Chinese-targeted name list) are being sprayed from ~23 IPs, with `root` and `admin` as the lead entries.

---

## 6. User Risk Assessment

This section answers two questions: (a) which **real** accounts are currently at risk, and (b) which **probed-but-invalid** usernames would be most dangerous *if they existed*.

### 6.1 Highest-risk **existing** accounts

| Priority | Username | Risk | Why |
|---|---|---|---|
| 🔴 Critical | `root` | **Critical** | 239 failed attempts from 10 IPs. UID 0 superuser; if successfully brute-forced the host is fully compromised. Direct root SSH login should be **disabled** (`PermitRootLogin no`). |
| 🟠 High | `ftp`, `git`, `mysql`, `uucp` | High | These are real service accounts that received targeted dictionary hits. Service accounts often have weak/known passwords, are shared across hosts, and may own sensitive data or chroot environments. Ensure they have no shell (`/sbin/nologin`) and cannot SSH in. |
| 🟡 Medium | `fztu` | Medium | The only user who **successfully** logged in during the window (password auth from `119.137.62.142`). Verify this was an authorised session; prefer key-based auth and consider MFA. |
| 🟡 Medium | `sshd` | Medium | Appears to be a real account on the system (the daemon attempted PAM auth for it). A privileged account literally named `sshd` is unusual and should be audited; if it is a real account it should not be allowed to log in. |

### 6.2 Highest-risk **non-existent** usernames (would be dangerous if created)

These usernames were heavily probed. If an administrator were to create any of them (even with a seemingly "good" password), they would immediately be in the cross-hairs of ongoing bot campaigns.

| Priority | Username (invalid) | Attempts | Why dangerous if it existed |
|---|---|---:|---|
| 🔴 Critical | `admin` | 41 | The #1 targeted non-root name across every SSH botnet. Often UID 0 or sudo-capable in appliances. |
| 🟠 High | `oracle`, `postgres`, `mysql`, `ftp`, `git` | 2–6 each | Default DB/service accounts frequently have well-known default passwords and broad file-system access. |
| 🟠 High | `support`, `deploy`, `operator`, `ubuntu` | 1–5 each | Human/service users that commonly ship with distros/devops tooling and are given sudo rights. |
| 🟠 High | `test`, `guest`, `default`, `pi` | 2–4 each | Accounts designed for convenience; almost always have trivial passwords and broad access. |
| 🟡 Medium | `nagios`, `cisco`, `vnc`, `ubnt`, `PlcmSpIp`, `inspur`, `matlab`, `pgadmin` | 1–3 each | IoT/enterprise appliance defaults. If a device or container exposes any of these, it is a pre-seeded target. |

### 6.3 Overall recommended actions

1. **Disable direct root login** over SSH (`PermitRootLogin no` in `sshd_config`) — this alone removes 64% of the current attack surface.
2. **Enforce key-based authentication only** (`PasswordAuthentication no`) and, where passwords remain, require strong, unique credentials plus fail2ban/denyhosts.
3. **Rate-limit / block** the most aggressive source IPs (top talkers listed below), ideally at the firewall or via `sshguard`/`fail2ban`.
4. **Lock down service accounts** (`ftp`, `git`, `mysql`, `uucp`, `sshd`) — set shell to `/sbin/nologin`, remove SSH access, and verify they are not in `AllowUsers`/wheel groups.
5. **Audit the successful login** by `fztu` on 2024-12-10 09:32:20 from `119.137.62.142` to confirm it was legitimate.
6. **Do not create** any of the heavily-probed invalid usernames (`admin`, `test`, `guest`, `oracle`, `postgres`, `ubuntu`, `pi`, etc.) without compensating controls (key-only, no tty, source IP allowlist).
7. **Enable verbose logging & alerting** on `Failed password` bursts and on any new `Accepted` events outside business hours/known IPs.

---

## Appendix A — Top attacking source IPs

| Rank | Source IP | Auth attempts | Primary targets |
|---:|---|---:|---|
| 1 | `183.62.140.253` | 149 | `root`(139), `oracle`(2), `test`(1) |
| 2 | `187.141.143.180` | 80 | `root`(46), `oracle`(4), `git`(2) |
| 3 | `103.99.0.122` | 30 | `admin`(7), `root`(4), `user`(2) |
| 4 | `112.95.230.3` | 26 | `root`(24), `pgadmin`(1), `utsims`(1) |
| 5 | `5.188.10.180` | 17 | `admin`(11), `default`(2), `0`(1) |
| 6 | `185.190.58.151` | 17 | `admin`(15), `123`(1), `api`(1) |
| 7 | `123.235.32.19` | 7 | `root`(7) |
| 8 | `5.36.59.76` | 6 | `root`(6) |
| 9 | `106.5.5.195` | 6 | `root`(6) |
| 10 | `119.4.203.64` | 6 | `admin`(6) |
| 11 | `52.80.34.196` | 5 | `matlab`(3), `test9`(1), `test`(1) |
| 12 | `60.2.12.12` | 5 | `root`(5) |
| 13 | `103.207.39.212` | 3 | `support`(1), `uucp`(1), `admin`(1) |
| 14 | `103.207.39.16` | 3 | `support`(1), `uucp`(1), `admin`(1) |
| 15 | `173.234.31.186` | 2 | `webmaster`(2) |
| 16 | `202.100.179.208` | 2 | `chen`(1), `cheng`(1) |
| 17 | `183.136.162.51` | 2 | `inspur`(2) |
| 18 | `195.154.37.122` | 2 | `support`(1), `uucp`(1) |
| 19 | `104.192.3.34` | 2 | `root`(1), `FILTER`(1) |
| 20 | `191.210.223.172` | 1 | `root`(1) |
| 21 | `103.207.39.165` | 1 | `support`(1) |
| 22 | `175.102.13.6` | 1 | `inspur`(1) |
| 23 | `119.137.62.142` | 1 | `fztu`(1) |

---

## Appendix B — Methodology

- Parsed `auth.log` line-by-line using regex against OpenSSH syslog message formats: `Invalid user`, `Failed password for [invalid user] …`, `Accepted … for …`, and the `message repeated N times: [ … ]` compression lines emitted by syslog (the two repeated lines in the log inflated `root`'s failure count by 10 additional entries that a naive line count would miss).
- An 'attempt' is any event where a password/key was submitted (one `Failed password …` or one `Accepted …` line, counting `message repeated` multiplicities). `Invalid user` announcements alone are not counted as additional attempts (they precede the failed-password line for the same session).
- A username is classified **valid** if the daemon ever accepted it as a real account (i.e. `Failed password for <user>` without `invalid user`, or an `Accepted` event). It is **invalid** if any `Invalid user <user>` line appears.
