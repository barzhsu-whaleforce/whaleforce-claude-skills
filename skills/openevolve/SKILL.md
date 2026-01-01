---
name: openevolve
description: Evolutionary code optimization service using AI. Use when user wants to optimize algorithms, improve code performance, or run genetic/evolutionary optimization on functions.
---

# OpenEvolve Service

Owner: Garen

## Connection Info

| Item | Value |
|------|-------|
| **Frontend UI** | `http://172.23.22.100:8201` |
| **Backend API** | `http://172.23.22.100:8200` |
| **API Docs** | `http://172.23.22.100:8200/api/docs` |

## Quick Start

```python
import requests
import time

BASE_URL = "http://172.23.22.100:8200"

# 1. Create evolution task
resp = requests.post(f"{BASE_URL}/api/evolve", json={
    "initial_code": """
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
""",
    "iterations": 50
})
task_id = resp.json()["task_id"]
print(f"Task ID: {task_id}")

# 2. Wait and check status
while True:
    status = requests.get(f"{BASE_URL}/api/evolve/status/{task_id}").json()
    print(f"Progress: {status['progress']:.0f}%")
    if status['status'] in ['completed', 'failed']:
        break
    time.sleep(5)

# 3. Get result
result = requests.get(f"{BASE_URL}/api/evolve/result/{task_id}").json()
print(f"Initial Score: {result['initial_score']}")
print(f"Final Score: {result['final_score']}")
print(f"Improvement: {result['improvement']:.2f}%")
print(f"Evolved Code:\n{result['evolved_code']}")
```

## API Endpoints

| Action | Endpoint | Method |
|--------|----------|--------|
| Create task | `/api/evolve` | POST |
| Check status | `/api/evolve/status/{task_id}` | GET |
| Get result | `/api/evolve/result/{task_id}` | GET |
| Cancel task | `/api/evolve/{task_id}/cancel` | POST |
| Health check | `/api/health` | GET |

## Task Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `initial_code` | string | (required) | Code to optimize |
| `evolve_type` | string | "code" | function/algorithm/code |
| `iterations` | int | 50 | Evolution iterations (1-500) |
| `evaluator_code` | string | null | Custom evaluation function |
| `test_cases` | array | null | Test cases |
| `model` | string | null | Specify LLM model |

## Task Status

| Status | Description |
|--------|-------------|
| `pending` | Waiting to start |
| `running` | In progress |
| `completed` | Finished |
| `failed` | Failed |
| `cancelled` | Cancelled |
