---
name: performance-metrics
description: Performance metrics calculation service for Sharpe ratio, excess return, volatility. Use when user needs to calculate stock performance metrics, compare against VOO benchmark, or analyze annualized returns.
---

# Performance Metrics Service

Owner: Peter

## Connection Info

| Item | Value |
|------|-------|
| **Base URL** | `http://172.23.22.100:8100` |
| **Health Check** | `GET /api/health` |

## Quick Start

```python
import requests

BASE_URL = "http://172.23.22.100:8100"

# Calculate AAPL H1 2024 performance
resp = requests.get(f"{BASE_URL}/api/metrics", params={
    "ticker": "AAPL",
    "start_date": "2024-01-01",
    "end_date": "2024-06-30"
})
data = resp.json()

print(f"Sharpe Ratio: {data['sharpe_ratio']:.2f}")
print(f"Annualized Excess Return: {data['annualized_excess_return_pct']:.2f}%")
print(f"Annualized Volatility: {data['annualized_volatility_pct']:.2f}%")
```

## API Endpoints

| Action | Endpoint | Params |
|--------|----------|--------|
| Calculate metrics | `GET /api/metrics` | `ticker`, `start_date`, `end_date` |
| Health check | `GET /api/health` | - |

## Response Metrics

| Metric | Description |
|--------|-------------|
| `sharpe_ratio` | Annualized Sharpe ratio (can be null) |
| `ticker_total_return_pct` | Stock total return (%) |
| `benchmark_total_return_pct` | VOO total return (%) |
| `excess_return_pct` | Excess return (%) |
| `annualized_excess_return_pct` | Annualized excess return (%) |
| `annualized_volatility_pct` | Annualized volatility (%) |

## Calculation Methods

### Sharpe Ratio (Annualized)
```
Daily Sharpe = (mean(daily_returns) - 0) / std(daily_returns)
Annualized Sharpe = Daily Sharpe * sqrt(252)
```

### Excess Return
```
Excess Return = Ticker Total Return - VOO Total Return
Annualized = ((1 + Excess Return) ^ (252 / trading_days) - 1) * 100%
```

## Example Response

```json
{
  "ticker": "AAPL",
  "benchmark": "VOO",
  "start_date": "2024-01-01",
  "end_date": "2024-06-30",
  "trading_days": 123,
  "ticker_total_return_pct": 13.4562,
  "benchmark_total_return_pct": 15.2347,
  "excess_return_pct": -1.7785,
  "annualized_excess_return_pct": -3.6098,
  "sharpe_ratio": 1.1868,
  "annualized_volatility_pct": 24.2257
}
```
