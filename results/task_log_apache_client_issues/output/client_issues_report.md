# Apache Error Log — Client IP Issue Report

**Source log:** `apache_error.log` (1,000 lines)
**Server:** Apache/2.0.49 (Fedora)
**Period covered:** 2005-06-09 06:07 → 2005-06-16 (8 days)
**Total client-attributed error entries:** 630 (across 159 unique client IPs)
**Non-client (server/startup) lines:** 370 (LDAP/mod_jk2 initialization, mod_security load notices, etc.)

---

## Summary Table — Top 5 Client IPs by Error Count

| Rank | IP Address       | Total Errors | Primary Error Type(s)                                              | Malicious? |
|-----:|------------------|-------------:|--------------------------------------------------------------------|:----------:|
| 1    | 202.133.98.6     | 184          | File does not exist (115), Script not found / unable to stat (69)  | **Yes**    |
| 2    | 81.199.21.119    | 23           | File does not exist (23) — probing `/sumthin`                      | **Yes**    |
| 3    | 194.116.250.2    | 23           | File does not exist (23) — probing `/sumthin`                      | **Yes**    |
| 4    | 81.214.165.213   | 23           | File does not exist (23) — probing `/_vti_bin` (FrontPage)         | **Yes**    |
| 5    | 195.23.79.241    | 22           | Directory index forbidden on `/var/www/html/` (22)                 | Likely / suspicious |

---

## Detailed Analysis by IP

### 1. 202.133.98.6 — 184 errors 🚨 (Highest-volume offender)

* **Breakdown:**
  * 115 × `File does not exist` — many to `/var/www/html/cgi`, `/var/www/html/awstats.pl`, `/var/www/html/awstats/awstats.pl`, `/var/www/html/stats/awstats.pl`
  * 69 × `script not found or unable to stat` — to `/var/www/cgi-bin/awstats`, `/var/www/cgi-bin/awstats.pl`, `/var/www/cgi-bin/stats`
* **Behavior:** In a ~2-second burst starting `Sat Jun 11 03:03:03 2005`, this host fired ~184 requests at six different AWStats-related CGI paths under both `/cgi-bin/` and the document root.
* **Verdict — Malicious (high confidence):** This matches the signature of the **AWStats / awstats.pl exploit worm** (e.g., the `awstats.pl` `configdir` remote command-execution vulnerability, CVE-2005-0116 / CVE-2005-0435, widely exploited by automated worms in mid-2005). The IP enumerated every common AWStats install location it knew — classic automated vulnerability scanning / worm propagation. **Recommendation:** block this IP at the firewall.

### 2. 81.199.21.119 — 23 errors

* **Breakdown:** 23 × `File does not exist: /var/www/html/sumthin`, all in a single sub-second burst at `Thu Jun 09 19:23:31 2005`.
* **Behavior:** Repeated rapid requests for a non-existent resource named `sumthin` at the site root.
* **Verdict — Malicious / automated probing:** Requesting an unusual, non-existent path 23 times in parallel is characteristic of a scripted scanner (possibly a DoS tool or a bot testing server behavior/error handling). Legitimate browsers do not issue 23 identical requests for the same missing file in one second.

### 3. 194.116.250.2 — 23 errors

* **Breakdown:** 23 × `File does not exist: /var/www/html/sumthin`, also arriving in a tight burst at `Thu Jun 09 19:23:31 2005` (same second as 81.199.21.119).
* **Verdict — Malicious / distributed or coordinated scan:** The identical payload (`sumthin`), identical count (23), and identical timestamp strongly suggest the two IPs are part of **the same distributed scan or botnet** hitting this server simultaneously. Same recommendation — block and treat as hostile.

### 4. 81.214.165.213 — 23 errors

* **Breakdown:** 23 × `File does not exist: /var/www/html/_vti_bin`, burst at `Tue Jun 14 15:57:00 2005`.
* **Behavior:** 23 rapid requests to `/_vti_bin`, the **Microsoft FrontPage Server Extensions** binary directory.
* **Verdict — Malicious (high confidence):** `/_vti_bin` is a classic IIS/FrontPage fingerprinting target. The host is running Linux/Apache, so the path will never exist; this is an **IIS/FrontPage vulnerability scanner** blindly enumerating Windows-vector paths (e.g., looking for FrontPage RCE, `_vti_bin/shtml.dll`, etc.). Well-known noise from mid-2000s worms like Code Red / Nimda derivatives and vulnerability scanners. Block.

### 5. 195.23.79.241 — 22 errors

* **Breakdown:** 22 × `Directory index forbidden by rule: /var/www/html/`, all within 9 seconds (`Mon Jun 13 18:38:25`–`18:38:34 2005`).
* **Behavior:** The host requested the site root (`/`) 22 times in rapid succession. Directory indexing is disabled (`-Indexes`), so each request generated a 403, not a directory listing.
* **Verdict — Suspicious / likely aggressive crawler or broken client:** Unlike the others, this IP only hit the root, not known-exploit paths. The rapid-fire behavior could be: (a) a misconfigured downloader/crawler ignoring 403s and retrying, (b) a broken keep-alive or proxy loop, or (c) a lightweight DoS/reflection test. It does **not** match known exploit signatures as cleanly as the top 4, but 22 near-simultaneous identical requests is not normal user behavior. Recommend rate-limiting and monitoring.

---

## 🚨 High-Severity Threats — IPs Sending "Invalid method" Requests

All of the following entries are of the form `Invalid method in request get /scripts/.../cmd.exe?/c+dir` (note the **lowercase** `get` — these are malformed HTTP requests that do not conform to RFC 1945/2616 because the method is not uppercase and the request line is crafted to traverse to Windows `cmd.exe`).

This is the signature of the **Unicode/Directory-Traversal IIS exploit** (CVE-2000-0884 / Nimda / Code Red–family worms) trying to reach `cmd.exe /c dir` through double-encoded, UTF-8 overlong (`%c0%af`, `%e0%80%af`, `%c1%9c`), double-url-encoded (`%252e`, `%255c`), or backslash (`%5c`, `%35c`) path traversal. Because the server is Linux/Apache (not IIS), these requests will fail — but they are unambiguous **hostile exploitation attempts** sent by automated worms/scanners and are the highest-severity threats in this log.

| IP Address       | Invalid-method count | Sample attempted exploit path |
|------------------|---------------------:|-------------------------------|
| 202.118.167.71   | 5                    | `/scripts/root.exe?/c+dir` (repeated 5× — Code Red II/Nimda root.exe probe) |
| 62.221.237.83    | 2                    | `/scripts/..%35c../winnt/system32/cmd.exe?/c+dir`, `/scripts/..%5c../winnt/system32/cmd.exe?/c+dir` |
| 63.203.254.140   | 2                    | `/scripts/.%252e/.%252e/winnt/system32/cmd.exe?/c+dir` |
| 12.216.230.125   | 1                    | `/scripts/..%c0%af../..%c0%af../..%c0%af../winnt/system32/cmd.exe?/c+dir` |
| 201.252.246.11   | 1                    | `/scripts/.%252e/.%252e/winnt/system32/cmd.exe?/c+dir` |
| 207.181.126.3    | 1                    | `/scripts/..%c0%af../..%c0%af../..%c0%af../winnt/system32/cmd.exe?/c+dir` |
| 213.205.73.192   | 1                    | `/scripts/..%e0%80%af../..%e0%80%af../..%e0%80%af../winnt/system32/cmd.exe?/c+dir` |
| 213.61.135.6     | 1                    | `/scripts/.%252e/.%252e/winnt/system32/cmd.exe?/c+dir` |
| 220.228.80.199   | 1                    | `/scripts/..%e0%80%af../..%e0%80%af../..%e0%80%af../winnt/system32/cmd.exe?/c+dir` |
| 61.72.66.8       | 1                    | `/scripts/..%5c../winnt/system32/cmd.exe?/c+dir` |
| 63.197.230.242   | 1                    | `/scripts/..%c1%9c../winnt/system32/cmd.exe?/c+dir` |
| 64.147.69.59     | 1                    | `/scripts/..%c0%af../..%c0%af../..%c0%af../winnt/system32/cmd.exe?/c+dir` |
| 64.60.251.53     | 1                    | `/scripts/..%e0%80%af../..%e0%80%af../..%e0%80%af../winnt/system32/cmd.exe?/c+dir` |

**Total hostile IPs in this category: 13** (18 invalid-method requests total). All 13 IPs should be considered **compromised or hostile** and added to blocklists. Note that `202.118.167.71` is particularly aggressive (5 retries of `/scripts/root.exe`) and warrants immediate blocking.

---

## Overall Observations & Recommendations

* The log is consistent with a public-facing web server in **mid-2005**, receiving constant background noise from the automated worm and scanner ecosystem of that era (AWStats exploits, Nimda/Code Red II IIS Unicode-traversal probes, FrontPage fingerprinting, distributed `sumthin` probes).
* The **`Invalid method in request`** entries are the clearest evidence of attempted server compromise and represent the highest priority for blocking.
* The **single highest-volume source** (`202.133.98.6`, 184 errors) is an AWStats exploit worm — immediate block recommended.
* Consider deploying/verifying:
  * Firewall/`iptables` rules to block the 13 invalid-method IPs + top-4 scanners listed above.
  * `mod_security` core rules (already loaded per notices but not logging any blocks in this window — ruleset should be reviewed).
  * `mod_evasive` or `fail2ban` to throttle the rapid-repeat request pattern seen from `195.23.79.241`, `81.199.21.119`, `194.116.250.2`, `81.214.165.213`, and `202.133.98.6`.
  * `ServerTokens Prod` / `ServerSignature Off` to reduce fingerprinting, and removal of any unused CGI/script aliases.
