# OpenSSH Unusual-Hours Authentication Activity Report

**Log file analyzed:** `auth.log` (1,500 sshd events)
**Log date:** December 10
**Time coverage in log:** 06:55:46 – 10:59:43 local server time
**Business-hours baseline:** 08:00–18:00 local server time
**Off-hours definition:** any time before 08:00 or at/after 18:00

> ⚠️ Note on coverage: The supplied log captures only a ~4-hour morning window (06:55–10:59). There is no afternoon, evening, or overnight data, so conclusions about evening/night attacks are limited to what is not present in this window.

---

## 1. Hourly Distribution

The table and chart below count every sshd authentication-related log line per hour.

| Hour  | All Auth Events | Failed Passwords | Window         |
|:-----:|----------------:|-----------------:|:---------------|
| 00:00 | 0               | 0                | Off-hours      |
| 01:00 | 0               | 0                | Off-hours      |
| 02:00 | 0               | 0                | Off-hours      |
| 03:00 | 0               | 0                | Off-hours      |
| 04:00 | 0               | 0                | Off-hours      |
| 05:00 | 0               | 0                | Off-hours      |
| **06:00** | **7**       | **1**            | **Off-hours**  |
| **07:00** | **169**     | **44**           | **Off-hours**  |
| 08:00 | 118             | 24               | Business hours |
| 09:00 | 676             | 133              | Business hours |
| 10:00 | 530             | 163              | Business hours |
| 11–17 | 0               | 0                | Business hours (no log coverage after 10:59) |
| 18–23 | 0               | 0                | Off-hours (no log coverage) |

**Totals**

- Business hours (08:00–17:59 in the covered range): **1,324** events (88.3%)
- Off-hours (06:55–07:59): **176** events (11.7%)
- Failed-password attempts inside business hours: **320**
- Failed-password attempts off-hours: **45**

![Hourly distribution of auth events](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784187448638233057/a0da8f9a210031fdfc4e821987ff3289_hourly_distribution.png?responseContentDisposition=attachment%3B%20filename%3D%22hourly_distribution.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T07%3A41%3A39Z%2F-1%2Fhost%2Fc2441d1fd2656ff9fa69e1856623294bd8a7f5314defd0b6ade93a049dc800ba)

The event volume rises steeply after 07:00, peaking at **09:00 (676 events)** and **10:00 (530 events)**—these hours are dominated by automated brute-force bursts (see §5).

---

## 2. Off-Hours Activity (before 08:00 / after 18:00)

All 176 off-hours events in the log fall in the **06:55–07:59** window (no events exist after 18:00 because the log ends at 10:59). These off-hours events are **100% malicious**—every single one is a connection from a non-whitelisted external IP performing an invalid-user probe, failed-password attempt, or reverse-DNS break-in indicator.

Breakdown of off-hours event types:

| Category                       | Count |
|--------------------------------|------:|
| Failed password                | 45    |
| Invalid user (pre-auth)        | 20    |
| Authentication failure (PAM)   | 46    |
| Connection closed / disconnect | 45    |
| POSSIBLE BREAK-IN ATTEMPT (rDNS) | 5   |
| PAM "service(sshd)" / other    | 15    |
| **Total off-hours events**     | **176** |

**Off-hours source IPs (≥ 4 events):**

| Source IP           | Events | Failed pw | Invalid users | Activity window     | Notable usernames tried             |
|---------------------|-------:|----------:|--------------:|---------------------|-------------------------------------|
| 112.95.230.3        | 80     | 26        | 2             | 07:27:52–07:28:51   | root, pgadmin, utsims               |
| 123.235.32.19       | 22     | 7         | 0             | 07:32:27–07:34:23   | root                                |
| 195.154.37.122      | 8      | 2         | 1             | 07:51:12–07:51:20   | support, uucp                       |
| 173.234.31.186      | 6      | 2         | 2             | 06:55:46–07:08:30   | webmaster (rDNS flagged as BREAK-IN)|
| 52.80.34.196        | 6      | 2         | 2             | 07:07:38–07:56:02   | test, test9 (AWS China EC2)         |
| 103.207.39.165      | 5      | 1         | 1             | 07:56:14–07:56:15   | support                             |
| 202.100.179.208     | 4      | 1         | 1             | 07:11:42–07:11:44   | chen                                |
| 183.136.162.51      | 4      | 1         | 1             | 07:42:49–07:42:51   | inspur                              |

The 07:00 hour accounts for **169/176 (96%)** of all off-hours traffic. None of the off-hours connections produced a successful login.

---

## 3. Early Morning Analysis (06:00–08:00)

This is the earliest portion of the log (06:55:46 is the very first event) and is split cleanly between two hours:

- **06:00 hour (7 events, 06:55:46–06:55:48):** A very short, isolated probe from **173.234.31.186** (reverse-DNS resolves to `ns.marryaldkfaczcz.com`, flagged by sshd as a **POSSIBLE BREAK-IN ATTEMPT**). The attacker tried username `webmaster` with one failed password and disconnected 2 seconds later.

- **07:00 hour (169 events):** Continuous, automated brute-force activity beginning at 07:02:47 and running right up to the 08:00 boundary. Key bursts:
  - **07:07–07:08** — `173.234.31.186` retries `webmaster`; `52.80.34.196` (AWS China EC2) tries `test9`.
  - **07:11–07:13** — `202.100.179.208` tries `chen`; `5.36.59.76` (Omantel DSL, Oman) hits `root` six times in ~25 seconds and is disconnected for **"Too many authentication failures for root"**.
  - **07:27:52–07:28:51 (~60 s)** — **Heaviest early-morning burst** from `112.95.230.3`: rapid-fire `root` attempts across multiple source ports (45378, 47068, 49188, 50999, 52660, …), ~1 attempt every 2 seconds, plus side-probes for `pgadmin` and `utsims`. This single IP produced **80 events (≈47% of all early-morning traffic)** in less than 2 minutes.
  - **07:32–07:34** — `123.235.32.19` batters `root` with 7 failed passwords.
  - **07:42–07:56** — Lower-volume probes from `183.136.162.51` (inspur), `195.154.37.122` (support/uucp), `103.207.39.165` (support), and re-appearances of `52.80.34.196`.

**Characteristics of the 06–08 window:**

- Traffic is entirely hostile; no legitimate interactive session occurs.
- Targeted usernames are typical brute-force dictionary entries: `root`, `admin`, `webmaster`, `test`/`test9`, `support`, `uucp`, `pgadmin`, `chen`, `inspur`, `utsims`, `matlab`.
- Attackers use short, high-rate bursts (≤2 minutes) with spoofed/rotated source ports, a classic SSH scanning signature.
- The 06:55 probe from `173.234.31.186` is the "canary in the coal mine" — a single early probe followed 12 minutes later by broader attack waves.

---

## 4. Successful Logins Timing

There is **exactly one successful authentication** in the entire log:

| Timestamp       | User  | Source IP       | Auth Method | During business hours? |
|-----------------|-------|-----------------|-------------|------------------------|
| Dec 10 09:32:20 | fztu  | 119.137.62.142  | password    | ✅ Yes (09:32, well inside 08–18) |

```
Dec 10 09:32:20 LabSZ sshd[24680]: Accepted password for fztu from 119.137.62.142 port 49116 ssh2
```

- The successful login **occurred during business hours** (09:32).
- The source IP `119.137.62.142` does **not** appear in any failed-password or invalid-user records earlier in the log, so it does not look like a credential-stuffing win — it appears to be a legitimate user logon.
- All 45 off-hours failed password attempts and all off-hours invalid-user attempts were **unsuccessful**.

---

## 5. Attack Timing Patterns

Focusing on the `Failed password` events (the clearest signal of active credential guessing):

| Hour  | Failed-password count | Dominant attacker IPs (attempts in that hour)                  |
|:-----:|----------------------:|----------------------------------------------------------------|
| 06:00 | 1   | 173.234.31.186 (1)                                             |
| 07:00 | 44  | 112.95.230.3 (26), 123.235.32.19 (7), 5.36.59.76 (2)           |
| 08:00 | 24  | 5.188.10.180 (17), 52.80.34.196 (1)                            |
| 09:00 | 133 | 187.141.143.180 (80), 103.99.0.122 (30), 185.190.58.151 (17)   |
| 10:00 | 163 | **183.62.140.253 (149)**, 119.4.203.64 (6), 60.2.12.12 (5)     |

**Pattern observations:**

1. **Volume ramps up through the morning, peaking at 10:00.** Attack activity doesn't stay flat — each hour shows a higher peak than the last, suggesting multiple botnet waves rather than one continuous session.
2. **Each peak hour is dominated by a *single* IP that sprays hundreds of attempts:**
   - 09:00 → `187.141.143.180` (80 attempts, mostly `root`)
   - 10:00 → `183.62.140.253` (149 attempts in this hour alone, 139 against `root`)
   These are classic dictionary/brute-force bots that rotate one IP for a burst, likely from compromised VPS hosts.
3. **Off-hours attacks are smaller and more distributed.** 07:00's 44 failures are spread across ~9 IPs (highest single-IP count is 26), consistent with opportunistic *scanning* rather than a dedicated brute-force campaign. Large "crunching" attacks appear to prefer the 09–10 window.
4. **Username targeting shifts with hour:** early morning hits `root`/`webmaster`/`test`; 08:00 switches to `admin`/`default`; 09:00 mixes `admin`/`root`/`oracle`/`git`; 10:00 is overwhelmingly `root` again. This is consistent with automated tools cycling through username wordlists.
5. **Attackers don't stop at the business-hours boundary** — they simply *scale up* after 08:00. This is a common bot behavior: early mornings are used for light reconnaissance/probing, and heavier attacks launch when sysadmin attention might be highest (ironically producing more noise in which to hide) — or, more simply, bots run 24/7 from multiple time zones.

**Do attackers prefer certain hours?** Within the 06:55–10:59 window captured, **yes** — the **09:00–10:59 business-morning slot** is where the large-scale credential-stuffing campaigns launch (296/365 = 81% of all failed passwords). The 07:00 off-hours slot sees meaningful but smaller-scale reconnaissance traffic.

---

## 6. Temporal Risk Assessment

### Highest-risk times (monitor most closely)

| Priority | Time Window          | Rationale                                                                                                                                  | Recommendation                                                                 |
|:--------:|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| 🔴 P1    | **09:00–11:00**      | Peak brute-force volume: 296 failed passwords (81% of total), driven by single high-rate IPs spraying `root` and `admin`.                  | Real-time alerting on >5 failed auths per minute; auto-block (fail2ban) at ≤3 failures in 10 min. |
| 🟠 P2    | **07:00–08:00**      | Off-hours reconnaissance, 100% malicious, includes high-rate burst (80 events in 60 s from 112.95.230.3). Staff unlikely to be present.    | Alert on *any* successful login in this window; tighten fail2ban thresholds overnight/early morning. |
| 🟡 P3    | **06:00–07:00**      | Earliest canary probes; low volume but precedes broader attacks.                                                                           | Log and flag; first-failure tripwire per source IP.                            |
| 🟡 P3    | **18:00–06:00** (overnight) | No data in this log, but industry pattern + early-morning activity indicate ongoing bot traffic outside work hours.                 | Same strict fail2ban policy as P2; review overnight logs first thing.          |
| 🟢 P4    | **08:00–09:00**      | Transition period; moderate volume (24 fails), some admin/default probes.                                                                  | Standard monitoring.                                                           |

### Other temporal observations

- **Short bursts (<2 minutes) from a single IP are the #1 attack signature in this log.** Tools that alert on rate-per-IP per minute will catch 100% of the significant campaigns.
- **Successful logon at 09:32 is low-risk by timing alone** (business hours, no prior failures from that IP), but it uses **password authentication** for `fztu` — SSH key-based auth would neutralize the brute-force threat entirely.
- **Reverse-DNS "POSSIBLE BREAK-IN ATTEMPT" warnings appeared only at 06:55 and 07:08**, but they correctly identified one of the earliest hostile IPs (`173.234.31.186`) — consider these early-warning indicators even before failed-password counts spike.

### Recommended hardening actions

1. **Enable / accelerate fail2ban (or equivalent)** with thresholds tuned for the observed pattern: ban after **3 failures in 10 minutes**, with a longer bantime (e.g., 24 h) for the 07:00 off-hours window and overnight.
2. **Disable password authentication** and require SSH keys; this single control would have rendered all 365 failed-password attempts harmless, including the off-hours bursts.
3. **Deny `root` SSH login** (`PermitRootLogin no`) — 255/365 (≈70%) of failed passwords in the log targeted `root`.
4. **Implement an explicit allowlist of source IPs** or use a VPN/bastion host for SSH, eliminating Internet-wide scanning.
5. **Schedule log review first thing each morning** specifically for the 06–08 window; hostile probing in this window reliably precedes the 09:00+ brute-force crescendo.

---

*Report generated from `auth.log` (Dec 10, 06:55–10:59 coverage).*
