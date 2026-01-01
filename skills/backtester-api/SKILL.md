---
name: backtester-api
description: Stock backtesting service API for running backtest strategies, calculating performance metrics (Sharpe ratio, drawdown, returns), and managing portfolio weights. Use when user asks about backtesting, stock strategy, performance analysis, or portfolio rebalancing.
---

# Backtester API Service

Owner: Yuhsiang

## Connection Info

| Item | Value |
|------|-------|
| **Frontend** | `https://backtest.whaleforce.dev` |
| **Backend API** | `https://backtest.api.whaleforce.dev` |
| **Health Check** | `/api/health` |

## Quick Start

```python
import requests

BASE_URL = "https://backtest.api.whaleforce.dev"

# Run backtest
response = requests.post(f"{BASE_URL}/backtest/run", json={
    "start_date": "2024-01-01T00:00:00Z",
    "end_date": "2024-12-31T00:00:00Z",
    "interval": "1d",
    "initial_capital": 100000,
    "base_currency": "USD",
    "strategy_name": "weighted_rebalance",
    "initial_portfolio": [
        {"ticker": "AAPL", "weight": 0.5},
        {"ticker": "MSFT", "weight": 0.5}
    ]
}, verify=False)

backtest_id = response.json()["backtest_id"]

# Get results
result = requests.get(f"{BASE_URL}/backtest/result/{backtest_id}", verify=False).json()
print(f"Total Return: {result['summary_metrics']['total_return_pct']:.2f}%")
print(f"Sharpe Ratio: {result['summary_metrics']['sharpe_ratio']:.2f}")
```

## API Endpoints

| Action | Method | Endpoint |
|--------|--------|----------|
| Run backtest | POST | `/backtest/run` |
| Get result | GET | `/backtest/result/{id}` |
| Get positions | GET | `/backtest/positions/{id}` |
| Get trades | GET | `/backtest/trades/{id}` |
| Get daily snapshots | GET | `/backtest/daily-snapshots/{id}` |
| List backtests | GET | `/backtest/list` |
| Get OHLCV data | GET | `/data-management/ohlcv?ticker=AAPL` |

## Available Strategies

| Strategy | Description |
|----------|-------------|
| `weighted_rebalance` | Weight allocation with auto-normalization |
| `generalize` | Trading signals with long/short and stop-loss |

## Data Coverage

- **Stocks**: S&P 500 + major ETFs + Benchmark indices
- **Time Range**: 2015 ~ today
- **Interval**: 1d (daily)

## Performance Metrics

| Metric | Description |
|--------|-------------|
| `total_return_pct` | Total return (%) |
| `annualized_return_pct` | Annualized return (%) |
| `sharpe_ratio` | Sharpe ratio |
| `max_drawdown_pct` | Maximum drawdown (%) |
| `volatility_pct` | Volatility (%) |
