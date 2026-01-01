---
name: paper2notebook
description: Paper to presentation service using Google NotebookLM. Use when user wants to convert academic papers to stylized PDF presentations, generate slides from papers, or create presentations automatically.
---

# Paper2Notebook Service

## Connection Info

| Item | Value |
|------|-------|
| **Frontend URL** | `https://paper2notebook.gpu5090.whaleforce.dev` |
| **API Base** | `https://paper2notebook.gpu5090.whaleforce.dev/api` |
| **Internal Port** | 8650 |
| **Server** | 172.23.22.100 |

## Quick Start

### Web Interface

Open in browser:
```
https://paper2notebook.gpu5090.whaleforce.dev
```

### API Usage

```python
import requests
import time

BASE_URL = "https://paper2notebook.gpu5090.whaleforce.dev"

# 1. Send generate request
resp = requests.post(f"{BASE_URL}/api/generate", json={
    "paper_url": "https://arxiv.org/pdf/xxxx.xxxxx.pdf",  # optional
    "style": "蠟筆小新風格",  # presentation style
    "language": "中文（繁體）"  # language
})
task_id = resp.json()["task_id"]
print(f"Task ID: {task_id}")

# 2. Poll task status
while True:
    status = requests.get(f"{BASE_URL}/api/task/{task_id}").json()
    print(f"Status: {status['status']} - {status.get('progress', '')}")

    if status["status"] == "completed":
        print(f"Download: {BASE_URL}{status['download_url']}")
        break
    elif status["status"] == "failed":
        print(f"Error: {status['error']}")
        break

    time.sleep(5)

# 3. Download PDF
if status["status"] == "completed":
    pdf = requests.get(f"{BASE_URL}{status['download_url']}")
    with open("presentation.pdf", "wb") as f:
        f.write(pdf.content)
```

### cURL Example

```bash
# Send generate request
curl -X POST "https://paper2notebook.gpu5090.whaleforce.dev/api/generate" \
  -H "Content-Type: application/json" \
  -d '{"style": "蠟筆小新風格", "language": "中文（繁體）"}'

# Query task status
curl "https://paper2notebook.gpu5090.whaleforce.dev/api/task/{task_id}"

# Download PDF
curl -O "https://paper2notebook.gpu5090.whaleforce.dev/api/task/{task_id}/download"
```

## API Endpoints

| Action | Method | Endpoint |
|--------|--------|----------|
| Generate presentation | POST | `/api/generate` |
| Get task status | GET | `/api/task/{task_id}` |
| Download PDF | GET | `/api/task/{task_id}/download` |
| List all tasks | GET | `/api/tasks` |
| Health check | GET | `/api/health` |

## Available Styles

| Style | Description |
|-------|-------------|
| 蠟筆小新風格 | Cute, lively, hand-drawn style |
| 專業商務風格 | Clean, professional business style |
| 可愛插畫風格 | Colorful illustration style |
| 科技未來感 | Dark theme, tech-futuristic |
| 學術風格 | Clear, academic report style |

## Task Status

| Status | Description |
|--------|-------------|
| `pending` | Task created, queued |
| `uploading` | Uploading paper |
| `generating` | Generating presentation |
| `downloading` | Downloading result |
| `completed` | Done, ready to download |
| `failed` | Failed with error |
