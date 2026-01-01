---
name: sec-filings
description: SEC filings query service for 10-K, 10-Q, 13F reports. Use when user needs SEC filings, annual reports, quarterly reports, institutional holdings, risk factors, MD&A, or company CIK lookup.
---

# SEC Filings Service

Owner: Peter

## Connection Info

| Item | Value |
|------|-------|
| **Base URL** | `http://172.23.22.100:8001` |
| **Health Check** | `GET /health` |

## Quick Start

```python
import requests

BASE_URL = "http://172.23.22.100:8001"

# 1. Search company (find CIK by ticker)
resp = requests.get(f"{BASE_URL}/search", params={"query": "AAPL"})
company = resp.json()[0]
print(f"CIK: {company['cik']}, Name: {company['name']}")

# 2. Query 10-K filings
resp = requests.get(f"{BASE_URL}/filings", params={
    "cik": company['cik'],
    "form": "10-K",
    "from_date": "2024-01-01"
})
filings = resp.json()

# 3. Query 13F holder analysis
resp = requests.get(f"{BASE_URL}/holder", params={
    "cik": company['cik'],
    "year": 2024,
    "quarter": 4
})
holders = resp.json()
```

## API Endpoints

| Action | Endpoint | Required Params |
|--------|----------|-----------------|
| Search company | `GET /search` | `query` (CIK or ticker) |
| Query filings | `GET /filings` | `cik` |
| Get download keys | `GET /filing-keys` | `cik` |
| Download file | `GET /download` | `key` |
| Company summary | `GET /summary` | - |
| 13F holder analysis | `GET /holder` | `cik`, `year` |

## Supported Form Types

| Form | Description | Item Codes |
|------|-------------|------------|
| **10-K** | Annual Report | 1, 1A (Risk Factors), 7 (MD&A), etc. |
| **10-Q** | Quarterly Report | part1item1, part2item1a, etc. |
| **13F** | Institutional Holdings | N/A (use `/holder`) |

## Example: Query Risk Factors and MD&A

```bash
curl "http://172.23.22.100:8001/filings?cik=0000072331&form=10-K&items=1A&items=7&formats=text"
```

Response includes S3 keys for downloading the actual content.
