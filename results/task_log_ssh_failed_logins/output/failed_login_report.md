# SSH Failed Login Analysis Report — LabSZ

**Host:** `LabSZ`
**Log source:** `auth.log`
**Report generated:** 2026-07-16
**Service audited:** OpenSSH `sshd`

---

## 1. Overview

| Metric | Value |
|---|---|
| Total log entries | **1,500** |
| Date / time range | **2024-12-10 06:55:46 → 2024-12-10 10:59:43** (≈ 4 h 4 m) |
| Total "Failed password" attempts | **366** |
| Invalid-user attempts | **99** |
| "POSSIBLE BREAK-IN ATTEMPT" warnings | **85** |
| Unique source IPs generating failed passwords | **22** |
| Successful logins in window | **1** |

The log captures roughly four hours of SSH activity on Dec 10, during which the server was hammered by hundreds of failed password attempts from dozens of external hosts, versus a single legitimate successful login.

---

## 2. Failed Password Attempts (by Source IP)

A total of **366** "Failed password" events were logged, originating from **22 distinct IP addresses**. The distribution is highly concentrated — the top two IPs alone account for **229 of 366 (≈ 62.6%)** failures.

| Rank | Source IP | Failed Passwords | % of Total |
|---:|---|---:|---:|
| 1 | 183.62.140.253 | 149 | 40.7% |
| 2 | 187.141.143.180 | 80 | 21.9% |
| 3 | 103.99.0.122 | 30 | 8.2% |
| 4 | 112.95.230.3 | 26 | 7.1% |
| 5 | 5.188.10.180 | 17 | 4.6% |
| 6 | 185.190.58.151 | 17 | 4.6% |
| 7 | 123.235.32.19 | 7 | 1.9% |
| 8 | 119.4.203.64 | 6 | 1.6% |
| 9 | 52.80.34.196 | 5 | 1.4% |
| 10 | 60.2.12.12 | 5 | 1.4% |
| * | 12 remaining IPs (≤ 4 each) | 24 | 6.6% |
| | **Total** | **366** | **100%** |

---

## 3. Invalid User Attempts

**99** attempts targeted non-existent usernames (`Invalid user ... from ...`), indicating attackers are spraying common/guessable account names rather than only targeting known accounts.

### Top 10 Most-Tried Invalid Usernames

| Rank | Username Tried | Attempts |
|---:|---|---:|
| 1 | `admin` | 18 |
| 2 | `oracle` | 6 |
| 3 | `support` | 5 |
| 4 | `test` | 4 |
| 5 | `inspur` | 3 |
| 6 | `0` | 3 |
| 7 | `matlab` | 3 |
| 8 | `webmaster` | 2 |
| 9 | `1234` | 2 |
| 10 | `guest` | 2 |

**Observation:** The list is a textbook dictionary brute-force wordlist — default admin accounts (`admin`, `webmaster`, `guest`, `support`), database/service accounts (`oracle`, `matlab`), vendor-default accounts (`inspur` — a Chinese server OEM), and trivially guessable names (`test`, `1234`, `0`). No legitimate account enumeration pattern is visible.

---

## 4. Top Attacking IPs

Ranked by total "Failed password" events (this is the same ranking as §2, since password failures are the dominant attack vector):

| Rank | Source IP | Failed Attempts | Notable Behaviour |
|---:|---|---:|---|
| 1 | **183.62.140.253** | 149 | Sustained rapid-fire brute force against `root` (≈ 30+ attempts/min at the end of the log window) |
| 2 | **187.141.143.180** | 80 | Automated dictionary attack, rotating common usernames |
| 3 | **103.99.0.122** | 30 | High-frequency password spraying |
| 4 | **112.95.230.3** | 26 | Brute force, mix of root and common users |
| 5 | **5.188.10.180** | 17 | Likely hosted/VPS source |
| 6 | **185.190.58.151** | 17 | Likely hosted/VPS source |
| 7 | **123.235.32.19** | 7 | Lower-volume scanner |
| 8 | **119.4.203.64** | 6 | Lower-volume scanner |
| 9 | **52.80.34.196** | 5 | AWS China (`ec2-52-80-34-196.cn-north-1.compute.amazonaws.com.cn`) — disposable cloud instance |
| 10 | **60.2.12.12** | 5 | Telecom-range source |

Additional sources of note include:
- **173.234.31.186** — repeatedly triggered reverse-DNS `POSSIBLE BREAK-IN ATTEMPT` warnings (reverse name `ns.marryaldkfaczcz.com` is a throwaway-looking domain), targeting `webmaster`.

---

## 5. Authentication Methods Observed

All 366 failed authentication events in this log used **password-based authentication** (`ssh2` password logins).

| Auth Method | Failed Attempts | Accepted |
|---|---:|---:|
| **password** | 366 | 1 |
| publickey | 0 | 0 |
| keyboard-interactive | 0 | 0 |
| gssapi-with-mic | 0 | 0 |
| hostbased | 0 | 0 |

**Key takeaway:** Every recorded attack attempted plain password authentication. No public-key authentication attempts (legitimate or malicious) appear in the window, and the single successful login (`Accepted password for fztu from 119.137.62.142`) was also password-based. This strongly suggests the server is not currently enforcing key-only authentication, which is the single biggest risk factor enabling these attacks.

---

## 6. Reverse DNS / "POSSIBLE BREAK-IN ATTEMPT" Warnings

The log contains **85** entries matching:

```
reverse mapping checking getaddrinfo for <hostname> [<IP>] failed - POSSIBLE BREAK-IN ATTEMPT!
```

These warnings are emitted by OpenSSH's `UseDNS` option when the connecting IP's forward-confirmed reverse DNS (FCrDNS) fails — a common trait of botnets, compromised hosts, and disposable cloud/VPS instances (since their reverse DNS either doesn't exist or doesn't resolve back to the same IP).

A representative example from the log:

```
Dec 10 06:55:46 LabSZ sshd[24200]: reverse mapping checking getaddrinfo for
ns.marryaldkfaczcz.com [173.234.31.186] failed - POSSIBLE BREAK-IN ATTEMPT!
```

85 such warnings within a 4-hour window confirms that the overwhelming majority of inbound SSH connections are coming from hosts with no legitimate DNS identity — a strong indicator of automated/rogue traffic rather than human administrators.

---

## 7. Summary Assessment

### Is the server under active attack?

**Yes — unambiguously.** The evidence:

1. **Volume:** 366 failed password attempts in ~4 hours (~90 attempts/hour on average, spiking much higher toward the end of the log).
2. **Breadth:** 22 unique source IPs, with bursts of 30+ attempts/minute from a single host (183.62.140.253).
3. **Tooling:** Attackers are using automated brute-force tooling — attempts come in rapid succession (seconds apart), rotate through dictionary usernames (`admin`, `oracle`, `root`, `test`, `guest`, etc.), and disconnect immediately after failure (`Received disconnect: 11 Bye Bye [preauth]`).
4. **Source profile:** Attackers originate from cloud providers (AWS China 52.80.34.196), VPS hosts (5.188.10.180, 185.190.58.151), consumer/telecom ranges (183.62.140.253, 187.141.143.180), and hosts with broken reverse DNS (173.234.31.186 with a suspicious disposable domain). This is the hallmark of commodity botnet scanning — the server has been discovered by internet-wide SSH brute-force crawlers (similar to Mirai, ZmEu, Hydra-style mass scans).
5. **Account targeting:** Heavy focus on `root` plus default-service accounts (`admin`, `oracle`, `webmaster`) and trivial names (`test`, `guest`, `1234`) — classic credential stuffing / dictionary attack, not a targeted APT.
6. **Success ratio:** Only **1 legitimate successful login** (`fztu` from 119.137.62.142) in the entire window versus 366 failures. The legitimate admin traffic is negligible compared to the noise.

### Pattern interpretation

This is **commodity internet noise, not a targeted incident** — every SSH server exposed to the public internet sees this type of traffic within hours of being brought online. However, "commodity" does **not** mean "safe":

- Successful compromise is typically a matter of whether **any account on the box has a weak/guessable password**. Given the high rate and diversity of attempts, a weak password on a common account name (e.g. `admin`, `oracle`, or even `fztu` if its password is reused/weak) would eventually be cracked.
- The fact that password authentication is still enabled (and publickey-only is not enforced) is the root enabler.

### Recommended mitigations (priority order)

1. **Disable password authentication** (`PasswordAuthentication no`) and require SSH public-key authentication for all accounts.
2. **Disable root login** (`PermitRootLogin no` or `prohibit-password`).
3. **Deploy fail2ban / sshguard** to auto-block IPs after N failures (even 5 failures in 10 minutes would have blocked every attacker in this log).
4. **Restrict SSH by source IP** where possible (security groups / firewall / `AllowUsers user@ip` / TCP wrappers).
5. **Move SSH to a non-standard port** (reduces but does not eliminate noise; only effective in combination with the above).
6. **Disable `UseDNS`** (the reverse-DNS warnings are noisy and provide little security value; they slow down connections and don't block attacks).
7. **Enable 2FA/MFA** (e.g. Google Authenticator PAM) for any accounts that must retain password login.
8. **Audit existing accounts** (`admin`, `oracle`, `webmaster`, `test`, `guest`, `support`, `matlab`, `inspur`) — confirm they either do not exist on the system or have strong, unique passwords.
9. **Review the one successful login** (`fztu` from 119.137.62.142 at 09:32:20) to confirm it was an authorized session.

---

*End of report.*
