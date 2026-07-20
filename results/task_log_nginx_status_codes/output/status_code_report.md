# Nginx Access Log — HTTP Status Code Report

**Source file:** `nginx_access.log`  
**Generated:** 2026-07-16  

## 1. Total Requests

- **Total log entries analyzed:** **1,000**

## 2. Status Code Breakdown

| Status Code | Count | Percentage | Distribution |
|-------------|------:|-----------:|--------------|
| 200 | 35 | 3.50% | `█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░` |
| 206 | 1 | 0.10% | `░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░` |
| 304 | 274 | 27.40% | `████████░░░░░░░░░░░░░░░░░░░░░░` |
| 403 | 2 | 0.20% | `░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░` |
| 404 | 688 | 68.80% | `█████████████████████░░░░░░░░░` |

## 3. Status Code Categories

| Category | Count | Percentage |
|----------|------:|-----------:|
| 2xx Success | 36 | 3.60% |
| 3xx Redirect | 274 | 27.40% |
| 4xx Client Error | 690 | 69.00% |
| 5xx Server Error | 0 | 0.00% |

## 4. Top Offenders — Client IPs (4xx & 5xx errors)

| Rank | Client IP | Error Count | % of All Errors |
|-----:|-----------|------------:|----------------:|
| 1 | `80.91.33.133` | 154 | 22.32% |
| 2 | `5.83.131.103` | 24 | 3.48% |
| 3 | `50.57.209.92` | 20 | 2.90% |
| 4 | `144.76.160.62` | 16 | 2.32% |
| 5 | `83.161.14.106` | 16 | 2.32% |

## 5. Top 3 Requested Paths per Status Code

### HTTP 200

| # | Path | Requests | % within this status |
|--:|------|---------:|---------------------:|
| 1 | `/downloads/product_2` | 26 | 74.29% |
| 2 | `/downloads/product_1` | 9 | 25.71% |

### HTTP 206

| # | Path | Requests | % within this status |
|--:|------|---------:|---------------------:|
| 1 | `/downloads/product_1` | 1 | 100.00% |

### HTTP 304

| # | Path | Requests | % within this status |
|--:|------|---------:|---------------------:|
| 1 | `/downloads/product_2` | 139 | 50.73% |
| 2 | `/downloads/product_1` | 135 | 49.27% |

### HTTP 403

| # | Path | Requests | % within this status |
|--:|------|---------:|---------------------:|
| 1 | `/downloads/product_2` | 2 | 100.00% |

### HTTP 404

| # | Path | Requests | % within this status |
|--:|------|---------:|---------------------:|
| 1 | `/downloads/product_2` | 353 | 51.31% |
| 2 | `/downloads/product_1` | 335 | 48.69% |

## 6. Server Health Assessment

**Overall status:** 🟡 **Caution**

Client errors (4xx) are high at **69.00%**. This often indicates broken links, misconfigured crawlers, or suspicious scanning activity rather than a server fault. Examine the top offending IPs and paths.

### Key Metrics Summary

- **Success rate (2xx):** 3.60%
- **Redirect rate (3xx):** 27.40%
- **Client-error rate (4xx):** 69.00%
- **Server-error rate (5xx):** 0.00%

### Notes & Recommendations

- Investigate the top offending IPs for 4xx errors; they may be scanners, broken clients, or misbehaving crawlers.
- A high volume of **304 Not Modified** responses is generally healthy — it indicates effective client-side caching.
- Frequent **404 Not Found** responses should be triaged for broken links (internal) vs. random scans (external).
