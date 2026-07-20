# Nginx Access Log — Large Responses Analysis Report

**Source file:** `nginx_access.log`
**Total requests analyzed:** 1,000
**Analysis date:** 2026-07-16

---

## 1. Top 10 Largest Responses

| Rank | Timestamp (UTC) | Client IP | Request Path | Method | Status | Bytes |
|------|-----------------|-----------|--------------|--------|--------|-------|
| 1 | 17/May/2015:08:05:52 +0000 | 37.26.93.214 | `/downloads/product_2` | GET | 200 | **3,318** |
| 2 | 17/May/2015:08:05:12 +0000 | 217.168.17.5 | `/downloads/product_2` | GET | 200 | **3,316** |
| 3 | 17/May/2015:08:05:25 +0000 | 217.168.17.5 | `/downloads/product_1` | GET | 200 | **3,301** |
| 4 | 17/May/2015:11:05:43 +0000 | 54.172.198.124 | `/downloads/product_2` | GET | 200 | 2,582 |
| 5 | 17/May/2015:08:05:19 +0000 | 23.23.226.37 | `/downloads/product_2` | GET | 200 | 2,578 |
| 6 | 17/May/2015:15:05:25 +0000 | 2.84.217.212 | `/downloads/product_2` | GET | 200 | 2,576 |
| 7 | 17/May/2015:12:05:17 +0000 | 54.194.143.19 | `/downloads/product_1` | GET | 200 | 2,573 |
| 8 | 17/May/2015:08:05:07 +0000 | 54.187.216.43 | `/downloads/product_2` | GET | 200 | 951 |
| 9 | 17/May/2015:09:05:23 +0000 | 54.193.30.212 | `/downloads/product_2` | GET | 200 | 951 |
| 10 | 17/May/2015:09:05:08 +0000 | 54.191.136.177 | `/downloads/product_2` | GET | 200 | 951 |

**Key observation:** The seven largest responses all come from `GET /downloads/product_*` endpoints returning HTTP 200, with byte counts clustered around two tiers: ~2.5–3.3 KB (full download payloads) and ~951 B (partial/error-payload responses). There is a pronounced size gap between rank #7 (2,573 bytes) and rank #8 (951 bytes), indicating two distinct response-size populations for successful downloads.

---

## 2. Byte Distribution Summary (non-zero responses only)

Excluding the 290 zero-byte responses (710 responses with actual payload):

| Metric | Value |
|--------|-------|
| Count (non-zero) | 710 |
| **Min** | **1 byte** |
| **Max** | **3,318 bytes** |
| **Mean** | **367.55 bytes** |
| **Median** | **337 bytes** |
| Total bytes transferred | 260,963 bytes (~254.8 KB) |

The distribution is heavily **right-skewed**: the median (337 B) is very close to the standard 404 error-page size (~318–346 B), which dominates the non-zero set. The mean is pulled upward by a small number of successful product downloads (2–3 KB), which are roughly an order of magnitude larger than the bulk of responses.

---

## 3. Zero-Byte Responses

- **Total zero-byte responses:** **290** (29.0% of all requests)

Breakdown by HTTP status code:

| Status Code | Count | Meaning |
|-------------|-------|---------|
| **304 Not Modified** | **274** | Conditional/cached GET — client copy is fresh, no body returned |
| **200 OK** | **14** | Successful request but empty response body (e.g., completed ranged/HEAD-style or zero-length file) |
| **403 Forbidden** | **2** | Access denied, no body served |

The overwhelming majority of zero-byte responses are **cache hits (304)**, which is healthy caching behavior. The 14 zero-byte 200s and 2 zero-byte 403s warrant a closer look — they may represent HEAD requests, empty files, or misconfigured error pages.

---

## 4. Large Response Analysis

### 4.1 Paths associated with the largest transfers

All responses ≥ 1,000 bytes (the "large" tier) hit the product-download endpoints exclusively:

| Path | Responses ≥1 KB | Max bytes | Non-zero count | Total bytes | Avg bytes (non-zero) |
|------|-----------------|-----------|----------------|-------------|----------------------|
| `/downloads/product_2` | 5 | 3,318 | 368 | 140,731 | 382.42 |
| `/downloads/product_1` | 2 | 3,301 | 342 | 120,232 | 351.56 |

**Finding:** `/downloads/product_2` is the heavier endpoint — it accounts for **5 of the 7 very large responses** (≥2.5 KB) and has the highest single response size (3,318 bytes), the highest total throughput (140.7 KB), and the highest average payload. Product downloads are the only endpoints producing responses above the ~346-byte error-page baseline.

### 4.2 Client IPs associated with the largest transfers (≥1 KB responses)

| Client IP | Large (≥1 KB) responses | Notes |
|-----------|------------------------|-------|
| **217.168.17.5** | 2 | Downloaded both product_1 (3,301 B) and product_2 (3,316 B) — top consumer |
| 37.26.93.214 | 1 | Single largest response (3,318 B to product_2) |
| 23.23.226.37 | 1 | 2,578 B to product_2 |
| 54.172.198.124 | 1 | 2,582 B to product_2 (AWS EC2 range) |
| 54.194.143.19 | 1 | 2,573 B to product_1 (AWS EC2 range) |
| 2.84.217.212 | 1 | 2,576 B to product_2 |

Notably, **three of the top seven large-transfer clients** fall in the `54.x.x.x` (AWS EC2) address space, suggesting automated download agents, mirrors, or crawlers are hitting the download endpoints — as is also corroborated by the `Debian APT-HTTP/1.3` and similar bot/CLI user agents common in this log.

---

## 5. Efficiency Assessment — Cache Hits vs. Data Transfer

### 5.1 Status-code mix

| Status | Count | Share | Bytes behavior |
|--------|-------|-------|----------------|
| **404 Not Found** | 688 | 68.8% | All carry a ~337 B error body |
| **304 Not Modified** (cache hit) | 274 | 27.4% | Zero bytes — ideal cache efficiency |
| **200 OK** (full content) | 35 | 3.5% | Mix of full downloads and some zero-byte |
| **403 Forbidden** | 2 | 0.2% | Zero bytes |
| **206 Partial Content** | 1 | 0.1% | 1 byte (ranged request) |

### 5.2 Efficiency metrics

| Metric | Value |
|--------|-------|
| Requests producing actual data transfer (bytes > 0) | **710 (71.0%)** |
| Requests with zero bytes (no data transfer) | **290 (29.0%)** |
| Cache hits (HTTP 304) | **274 (27.4%)** |
| Successful full-content responses (200 + non-zero) | **21 (2.1%)** |
| Error/other responses that still carry a body (404s + 206) | **689 (68.9%)** |

### 5.3 Interpretation

- **Cache effectiveness for conditional GETs is strong:** 274 of the 290 zero-byte responses are HTTP 304s, meaning clients revalidated and the server correctly replied "not modified" without re-sending payloads. This represents **94.5% of zero-byte responses** and saves significant bandwidth on repeat downloads.
- **However, only 2.1% of all requests actually result in successful content delivery (200 with body).** The bulk of traffic (68.8%) is **404 Not Found** responses for missing resources — each returning a ~337-byte error page. These are wasted bytes and likely indicate broken links, misconfigured crawlers, or probes for non-existent paths.
- **True "efficient" traffic ratio:** If we measure success as "useful content delivered" (200 + non-zero + 206) divided by total requests, the server only delivers meaningful payloads on **2.2%** of hits. The remaining traffic is split between cache revalidations (27.4%) and errors/denials (70.4%).
- **Recommendation:** Investigate the high 404 rate — identifying and either fixing or blocking the missing paths could substantially reduce wasted bandwidth and log noise. The 14 zero-byte 200 responses also deserve review to confirm they are intentional (e.g., HEAD requests) and not a sign of an upstream or filesystem issue.

---

## Summary Takeaways

1. The **largest single response** was 3,318 bytes for `GET /downloads/product_2` from `37.26.93.214`.
2. Response sizes are **bimodal**: ~318–346 B error pages dominate the distribution, while successful product downloads cluster around 951 B and 2,573–3,318 B.
3. **27.4% of requests are answered from cache (304)** with zero bytes — healthy caching.
4. The **download endpoints (`/downloads/product_1` and `/downloads/product_2`)** are the sole source of large payloads; `/downloads/product_2` is the heavier of the two.
5. **68.8% of requests return 404**, indicating a high proportion of invalid/probe traffic that should be addressed for better overall efficiency.
