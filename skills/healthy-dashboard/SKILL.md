---
name: healthy-dashboard
description: Service monitoring dashboard for Whaleforce services health status. Use when user wants to check service status, monitor uptime, view system health, or troubleshoot service availability.
---

# Healthy Dashboard Service

Owner: Barz

## Connection Info

| Item | Value |
|------|-------|
| **Dashboard UI (HTTPS)** | `https://dashboard.gpu5090.whaleforce.dev` |
| **Backend API (HTTPS)** | `https://dashboard-api.gpu5090.whaleforce.dev` |
| **Dashboard UI (Internal)** | `http://172.23.22.100:8030` |
| **Backend API (Internal)** | `http://172.23.22.100:8031` |

## Quick Start

```bash
# Browser access
open https://dashboard.gpu5090.whaleforce.dev

# Query all service status
curl https://dashboard-api.gpu5090.whaleforce.dev/services/status

# Query specific service
curl https://dashboard-api.gpu5090.whaleforce.dev/services/PostgreSQL/status
```

## Python Example

```python
import requests

resp = requests.get("https://dashboard-api.gpu5090.whaleforce.dev/services/status")
data = resp.json()

print(f"Total Services: {data['summary']['total']}")
print(f"Healthy: {data['summary']['healthy']}")
print(f"Unhealthy: {data['summary']['unhealthy']}")

for svc in data['services']:
    status = "OK" if svc['status'] == 'healthy' else "FAIL"
    print(f"{status} {svc['name']}: {svc['response_time_ms']:.0f}ms")
```

## API Endpoints

| Action | Endpoint |
|--------|----------|
| All services status | `GET /services/status` |
| Specific service status | `GET /services/{name}/status` |
| API health check | `GET /health` |
| Service list | `GET /services` |

## Monitored Services

| Service | Type | Endpoint |
|---------|------|----------|
| Backtester API | HTTP | https://backtest.api.whaleforce.dev |
| SEC Filings API | HTTP | http://172.23.22.100:8001 |
| LiteLLM Proxy | HTTP | https://litellm.whaleforce.dev |
| PostgreSQL | TCP | 172.23.22.100:5432 |
| Neo4j Graph | Bolt | 172.23.22.100:7687 |

## Response Format

```json
{
  "summary": {
    "total": 5,
    "healthy": 5,
    "unhealthy": 0,
    "unknown": 0
  },
  "checked_at": "2025-12-22T07:24:06.119131",
  "services": [
    {
      "name": "Backtester API",
      "description": "Backtest service API",
      "service_type": "http",
      "endpoint": "https://backtest.api.whaleforce.dev",
      "status": "healthy",
      "response_time_ms": 125.84,
      "error_message": null
    }
  ]
}
```

## Features

- Real-time service health monitoring
- Auto-refresh every 60 seconds
- Visual status overview
- Quick identification of unhealthy services
