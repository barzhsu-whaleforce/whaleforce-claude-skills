---
name: multi-model-aggregator
description: Multi-model LLM aggregation service that calls GPT-5, Claude Opus 4.5, Gemini 3 Pro in parallel and summarizes results. Use when user wants to compare multiple AI models, get aggregated responses, or use ensemble LLM approach.
---

# Multi-Model Aggregator Service

Owner: Barz

## Connection Info

| Item | Value |
|------|-------|
| **Frontend (HTTPS)** | `https://multi-model.gpu5090.whaleforce.dev` |
| **Backend API (HTTPS)** | `https://multi-model-api.gpu5090.whaleforce.dev` |
| **Frontend (Internal)** | `http://172.23.22.100:8040` |
| **Backend API (Internal)** | `http://172.23.22.100:8041` |

## Quick Start

```bash
# Browser access
open https://multi-model.gpu5090.whaleforce.dev

# API aggregation request
curl -X POST https://multi-model-api.gpu5090.whaleforce.dev/v1/chat/completions/aggregate \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [{"role": "user", "content": "Explain quantum computing basics"}],
    "models": ["gpt-5.2", "claude-opus-4.5", "gemini-3-pro-preview"],
    "summarizer_model": "grok-4.1-fast"
  }'
```

## Python Example

```python
import requests

BASE_URL = "https://multi-model-api.gpu5090.whaleforce.dev"

resp = requests.post(f"{BASE_URL}/v1/chat/completions/aggregate", json={
    "messages": [{"role": "user", "content": "Explain quantum computing basics"}],
    "models": ["gpt-5.2", "claude-opus-4.5", "gemini-3-pro-preview"],
    "summarizer_model": "grok-4.1-fast"
})

result = resp.json()
print(f"Summary: {result['summary']}")
print(f"Total Latency: {result['total_latency_ms']:.0f}ms")

# View individual responses
for r in result.get('individual_responses', []):
    print(f"\n{r['model']}: {r['latency_ms']:.0f}ms")
    print(r['content'][:200] + "...")
```

## API Endpoints

| Action | Endpoint |
|--------|----------|
| Multi-model aggregate | `POST /v1/chat/completions/aggregate` |
| OpenAI compatible | `POST /v1/chat/completions` |
| List available models | `GET /v1/models` |
| Health check | `GET /health` |

## Default Model Configuration

| Type | Model |
|------|-------|
| **Query Models** | `gpt-5.2`, `claude-opus-4.5`, `gemini-3-pro-preview` |
| **Summarizer** | `grok-4.1-fast` |

## OpenAI Compatible Mode

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://multi-model-api.gpu5090.whaleforce.dev/v1",
    api_key="not-needed"
)

# Use model="aggregate" to trigger multi-model aggregation
response = client.chat.completions.create(
    model="aggregate",
    messages=[{"role": "user", "content": "What is machine learning?"}]
)

print(response.choices[0].message.content)
```

## Response Format

```json
{
  "request_id": "abc12345",
  "summary": "Aggregated analysis...",
  "summarizer_model": "grok-4.1-fast",
  "summarizer_latency_ms": 2500.0,
  "individual_responses": [...],
  "total_latency_ms": 8500.0
}
```
