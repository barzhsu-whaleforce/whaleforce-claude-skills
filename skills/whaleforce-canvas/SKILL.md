---
name: whaleforce-canvas
description: AI web page generation service using Claude AI. Use when user wants to generate HTML/CSS/JavaScript web pages, create dashboards, prototypes, or interactive visualizations from natural language descriptions.
---

# Whaleforce Canvas Service

Owner: Barz

## Connection Info

| Item | Value |
|------|-------|
| **Web UI (HTTPS)** | `https://canvas.gpu5090.whaleforce.dev` |
| **API URL (HTTPS)** | `https://canvas.gpu5090.whaleforce.dev/api/generate` |
| **Web UI (Internal)** | `http://172.23.22.100:8700` |
| **API URL (Internal)** | `http://172.23.22.100:8700/api/generate` |

## Quick Start

```bash
# Generate a complete web page
curl -X POST "https://canvas.gpu5090.whaleforce.dev/api/generate" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Create a stock price tracking page with real-time quotes and K-line chart"
  }'
```

## Python Example

```python
import requests

response = requests.post(
    "https://canvas.gpu5090.whaleforce.dev/api/generate",
    json={"prompt": "Create an interactive chart showing AAPL stock price trends"}
)
result = response.json()

# Get generated HTML
html_content = result["html"]

# Save to file
with open("stock_chart.html", "w") as f:
    f.write(html_content)
```

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/generate` | POST | Generate web page from prompt |
| `/api/preview/{id}` | GET | Preview generated page |

## Features

- **Auto-generate complete web pages**: Generate HTML/CSS/JavaScript from natural language
- **Real-time preview**: Preview immediately after generation
- **Interactive components**: Support charts, tables, forms, etc.
- **Responsive design**: Auto-adapt to different screen sizes

## Use Cases

- Quick data visualization pages
- Report visualization
- Prototype web pages
- Interactive dashboards

## Limitations

- Generated pages are single-page applications (SPA)
- No backend logic, frontend only
- Complex interactions may require prompt refinement
