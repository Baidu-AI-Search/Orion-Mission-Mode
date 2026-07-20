# NOTES.md — DataFetcher Process Documentation

## 1. Task Summary

Read `config.json`, extract the API endpoint configuration, create a Python script to invoke the endpoint, and document the whole process.

## 2. Reading config.json

The provided `config.json` contains two top-level sections:

- `api` — network / request parameters
- `project` — metadata (name, version, description)

### 2.1 Extracted API Endpoint Information

| Field       | Value                             |
|-------------|-----------------------------------|
| endpoint    | `https://api.example.com/v2/data` |
| method      | `GET`                             |
| Content-Type| `application/json`                |
| Accept      | `application/json`                |
| timeout     | `30` seconds                      |

### 2.2 Project Metadata

| Field       | Value                            |
|-------------|----------------------------------|
| name        | DataFetcher                      |
| version     | 1.0.0                            |
| description | Automated data fetching utility  |

## 3. Created Python Script: `call_api.py`

A self-contained script (uses only the Python standard library — no third-party dependencies) that:

1. **Loads config** — `load_config(path)` reads and parses `config.json`. A custom path can be passed as the first CLI argument.
2. **Reads API settings** — pulls `endpoint`, `method`, `headers`, and `timeout` from the `api` block (with safe defaults).
3. **Issues the HTTP request** — uses `urllib.request` to make the call, honoring the configured method, headers, and timeout.
4. **Handles errors gracefully** — catches `HTTPError`, `URLError`, and unexpected exceptions, returning a structured result with status code / error message instead of crashing.
5. **Parses the response** — attempts to decode JSON; falls back to raw text if parsing fails.
6. **Prints a readable report** — project info, request parameters, status, and response body (pretty-printed JSON when possible).
7. **Exit code** — returns `0` on HTTP 200, otherwise `1`, so it is safe to use in CI / shell pipelines.

### 3.1 File Layout

```
.
├── config.json      # source configuration (provided)
├── call_api.py      # Python script created for this task
└── NOTES.md         # this documentation file
```

### 3.2 How to Run

From the directory containing all three files:

```bash
# Use default config.json in the current directory
python3 call_api.py

# Or point to a different config file
python3 call_api.py /path/to/another_config.json
```

### 3.3 Expected Output Shape

```
[DataFetcher] Loading config from: config.json
[DataFetcher] Project : DataFetcher v1.0.0
[DataFetcher] Descr   : Automated data fetching utility
[DataFetcher] Endpoint: https://api.example.com/v2/data
[DataFetcher] Method  : GET
[DataFetcher] Timeout : 30s
[DataFetcher] Calling API ...
[DataFetcher] Status  : <status code>
[DataFetcher] Body    :
<formatted JSON or raw response body>
```

> **Note:** `https://api.example.com/v2/data` is a placeholder/example endpoint defined in the config. It is not expected to return a real 200 response in the open internet; in live use, replace it (and headers/method) with a real API. The script will report the network/HTTP error cleanly rather than crashing.

## 4. Verification Steps Performed

1. Opened and read `config.json` to confirm structure and field names.
2. Mapped every `api.*` field to a concrete usage in `call_api.py` (endpoint → URL, method → HTTP verb, headers → request headers, timeout → socket timeout).
3. Wrote the script using only stdlib modules (`json`, `urllib.request`, `pathlib`, `sys`) so it runs on any standard Python 3 installation without `pip install`.
4. Added structured error handling so misconfigured / unreachable endpoints do not produce tracebacks.
5. Documented the endpoint, script design, usage, and expected output in this file.

## 5. Deliverables

- `config.json` (unchanged, original)
- `call_api.py` (newly created)
- `NOTES.md` (newly created, this document)
