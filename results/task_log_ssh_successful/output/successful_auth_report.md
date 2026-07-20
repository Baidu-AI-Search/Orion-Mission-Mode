# OpenSSH Successful Authentication Report
**Log source:** `auth.log` (host `LabSZ`)
**Log window:** `Dec 10 06:55:46` – `Dec 10 10:59:43` (≈ 4 h 4 min)
**Total log lines:** 1,500

---

## 1. Successful Logins

Across the entire log window, there is **exactly one** successful SSH authentication.

| # | Timestamp | Username | Source IP | Source Port | Auth Method |
|---|-----------|----------|-----------|-------------|-------------|
| 1 | `Dec 10 09:32:20` | `fztu` | `119.137.62.142` | `49116` | `password` (ssh2) |

**Raw log line (line 956):**
```
Dec 10 09:32:20 LabSZ sshd[24680]: Accepted password for fztu from 119.137.62.142 port 49116 ssh2
```

Key observations:
- Authentication was via plain **password** (not public key), which is the weaker method but not inherently malicious.
- The ephemeral source port (`49116`) is in the standard dynamic/private range (49152–65535), consistent with a normal outgoing SSH client connection.
- The username `fztu` is a non-default, lowercase account name — **not** one of the names brute-forcers were targeting (e.g., `root`, `admin`, `oracle`, `test`, `support`, `ftp`, `git`, `uucp`, `matlab`, `inspur`, `cyrus`, `webmaster`, `chen`, etc.).

---

## 2. Success vs. Failure Ratio

Authentication attempts were counted by expanding `message repeated N times` entries (syslog's rate-limiting) and including both `Failed password` and `Failed none` events, which represent actual credential-submission attempts.

| Metric | Count |
|---|---|
| Successful authentications (`Accepted …`) | **1** |
| Failed password attempts (expanded for repeats) | 366 |
| Failed `none` method attempts (expanded for repeats) | 4 |
| **Total authentication attempts** | **371** |
| **Success rate** | **≈ 0.27 %** |
| **Failure rate** | **≈ 99.73 %** |
| **Success : Failure ratio** | **1 : 370** |

The host is clearly under **active, widespread brute-force attack** — for every one legitimate login, the SSH daemon rejected ~370 password guesses from automated sources.

---

## 3. Legitimate User Profile (`fztu`)

There is only one successfully authenticated account. Its access pattern, derived from the log, is:

| Attribute | Value |
|---|---|
| Account | `fztu` (non-privileged-looking username, not a known default/service account) |
| Login time | `Dec 10 09:32:20` |
| Logout time | `Dec 10 09:45:06` |
| Session duration | **≈ 12 minutes 46 seconds** |
| Source IP | `119.137.62.142` (single source, no roaming) |
| Source port | `49116` (single TCP connection for the entire session) |
| Auth method | Password over SSH2 |
| sshd PID | `24680` (one session per successful auth; no multiplexed/re-used PID) |
| Re-authentications within the window | None (one login, one logout) |
| Failed attempts immediately preceding success | None from this IP — the line before is an unrelated failed attempt from `104.192.3.34` against `root`. |
| Failed attempts targeting the `fztu` username anywhere in the log | **0** — no brute-forcer even tried this username. |

**Access-pattern summary:** A single, short interactive session from a single IP, opening cleanly and closing cleanly after ~13 minutes. There is no indication of scripted/automated access (no rapid successive logins, no multiple sessions, no key-based automation), which is consistent with a human administrator or user logging in to perform a brief task.

---

## 4. Session Activity

Session lifecycle events for PID `24680` (the `fztu` session):

| Timestamp | Event |
|-----------|-------|
| `Dec 10 09:32:20` | `Accepted password for fztu from 119.137.62.142 port 49116 ssh2` |
| `Dec 10 09:32:20` | `pam_unix(sshd:session): session opened for user fztu by (uid=0)` |
| `Dec 10 09:45:06` | `Received disconnect from 119.137.62.142: 11: disconnected by user` *(logged by child/rekey PID 24761)* |
| `Dec 10 09:45:06` | `pam_unix(sshd:session): session closed for user fztu` |

Interpretation:
- The session was **opened immediately** after successful authentication (same second), as expected for a normal PAM-driven login.
- The session was **closed by the user** (`disconnected by user`, reason code 11 = SSH2_DISCONNECT_BY_APPLICATION), not by the server (not idle-timeout, not admin-killed, not crash).
- The `Received disconnect` was logged under a related child PID (`24761`) while the `session closed` was logged under the original authenticated PID (`24680`); this is normal OpenSSH privilege-separation / rekey behavior and is **not** a sign of a second session.
- There are **no other session-open or session-close events** anywhere else in the log, confirming exactly one interactive login during the entire window.
- `auth.log` does not record shell commands or post-login activity — those would live in `bash_history`, `auditd`, or `secure`/tty logs. Within this file we can only confirm a clean open and clean close.

---

## 5. Source IP Validation

| Check for `119.137.62.142` | Result |
|---|---|
| Appears in a `Failed password` or `Failed none` event | **No** |
| Appears in an `Invalid user` event | **No** |
| Appears in a `POSSIBLE BREAK-IN ATTEMPT!` reverse-DNS warning | **No** |
| Appears in an `authentication failure` PAM record | **No** |
| Appears in a `Connection closed … [preauth]` (dropped pre-auth) event | **No** |
| Appears at all outside the successful session + its disconnect | **No** — exactly 2 hits: line 956 (accepted) and line 964 (user-initiated disconnect) |

**Conclusion:** `119.137.62.142` is **not** associated with any failed authentication attempts anywhere in the log. It is a clean IP that connected once, authenticated correctly on the first try, and disconnected 13 minutes later.

For contrast, the top brute-force sources in the same log window were:

| IP | Failed attempts |
|---|---|
| `183.62.140.253` | 149 |
| `187.141.143.180` | 80 |
| `103.99.0.122` | 30 |
| `112.95.230.3` | 26 |
| `5.188.10.180` | 17 |
| `185.190.58.151` | 17 |
| (16 other IPs) | … |

`119.137.62.142` does **not** appear on this list.

---

## 6. Anomaly Check

I evaluated the successful login against typical indicators of SSH compromise/brute-force success:

| Signal | Finding | Verdict |
|---|---|---|
| **IP previously brute-forcing this host?** | 0 failed attempts from `119.137.62.142` in the entire log | ✅ Normal |
| **IP brute-forcing other accounts before succeeding?** | No — the IP does not appear before `09:32:20` at all | ✅ Normal |
| **Password spray / credential stuffing signature?** | First connection from this IP, first password try succeeded (no retry series) | ✅ Normal |
| **Username a common brute-force target?** | Username is `fztu` — not `root`, `admin`, `oracle`, `test`, `support`, etc., and was **never** probed by any attacker | ✅ Normal |
| **Reverse-DNS / "POSSIBLE BREAK-IN ATTEMPT" flag?** | No reverse-mapping warning was emitted for `119.137.62.142` | ✅ Normal |
| **Off-hours / suspicious timing?** | Login at 09:32 local, logout at 09:45 — typical business hours | ✅ Normal |
| **Multiple rapid logins from the same IP?** | Single login, single logout | ✅ Normal |
| **Session ended abnormally?** | Closed by user (SSH disconnect code 11), not kicked by server | ✅ Normal |
| **Auth method** | Password (not key-based) | ⚠️ Hardening opportunity, not an indicator of compromise |
| **Session duration** | ~13 minutes | ✅ Plausible for a short admin task |
| **Other suspicious activity during the session?** | Brute-force noise from unrelated IPs continues in parallel (expected Internet background radiation); no other sessions opened, no privilege-escalation markers visible in auth.log | ✅ Normal |

### Overall verdict
The single successful login by user `fztu` from `119.137.62.142:49116` at `Dec 10 09:32:20` appears to be a **legitimate, clean interactive login**, not a brute-force success. The IP is "quiet" (no failed attempts, no pre-auth drops, no DNS anomalies), the username is non-obvious, and the session opened and closed cleanly in a short window consistent with human use.

### Recommended hardening (non-blocking)
Although the successful login itself is not suspicious, the **overall threat picture** (99.73% failure rate, 22 distinct attacking IPs in ~4 hours, 85 reverse-DNS break-in warnings) demonstrates that the host is continuously targeted. Recommended actions include:
1. **Disable password authentication** and enforce SSH public-key authentication (the `fztu` login used `password`).
2. **Disable or severely restrict `root` login** (`PermitRootLogin no` or `prohibit-password`) — `root` accounted for 231 of 366 failed password attempts.
3. Deploy **fail2ban** / `sshd` throttling (e.g., `MaxAuthTries 3`, connection rate-limiting via iptables/nftables).
4. Move SSH to a non-default port and/or require a VPN/jump-host exposure.
5. Enable key-based login combined with 2FA (e.g., `libpam-google-authenticator`) for privileged accounts.

---

## Appendix A: Attack Landscape Snapshot (context)

- Distinct source IPs producing failed attempts: **22**
- Distinct usernames targeted in failed attempts: **61**
- Most-targeted usernames: `root` (231), `admin` (41), `oracle` (6), `support` (5), `uucp`/`test` (4 each).
- `message repeated` (syslog rate-limit) compressions triggered: **2** — each collapsing 5 repeated `Failed password for root …` entries, giving 10 additional failures that would otherwise be undercounted by a naive `grep -c "Failed password"`.
- "POSSIBLE BREAK-IN ATTEMPT!" (failed reverse DNS) warnings: **85**.

## Appendix B: Methodology
- Parsed `auth.log` line-by-line with regexes for `Accepted`, `Failed password`, `Failed none`, `Invalid user`, `session opened`, `session closed`, `Connection closed`, `Received disconnect`, and syslog `message repeated N times` compression markers.
- Expanded `message repeated N times` entries so that ratios reflect real attempt counts, not syslog-compressed lines.
- Correlated events by `sshd[PID]` to reconstruct each session's lifecycle; noted that the disconnect message for `fztu` was logged by a child/rekey PID (`24761`) but corresponds to the same TCP session as PID `24680`.
