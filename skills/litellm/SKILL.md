---
name: litellm
description: LLM unified proxy service supporting multiple models (GPT-5, Claude, Gemini, o3). Use when user needs to call LLM APIs, chat completions, reasoning, web search, or embeddings. OpenAI-compatible API.
---

# LiteLLM - LLM Unified Proxy Service

Owner: Yuhsiang

## Connection Info

| Item | Value |
|------|-------|
| **Base URL** | `https://litellm.whaleforce.dev` |
| **Frontend UI** | `https://litellm.whaleforce.dev/ui` |
| **Health Check** | `/health/readiness` |
| **API Key** | Contact admin for personal key |

## Quick Start

```python
from openai import OpenAI

BASE_URL = "https://litellm.whaleforce.dev"
API_KEY = "sk-xxxxxxxx"  # Get from admin

client = OpenAI(api_key=API_KEY, base_url=BASE_URL)

# Simple chat
resp = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(resp.choices[0].message.content)

# Reasoning mode
resp = client.responses.create(
    model="o3",
    input="Explain quantum computing in 2 sentences.",
    reasoning={"effort": "medium"},
)
print(resp.output[0].content[0].text)
```

## Available Models

| Model | Provider | Notes |
|-------|----------|-------|
| gpt-4o | Azure | Common |
| gpt-5 / gpt-5.1 / gpt-5.2 | Azure | Latest |
| o3 | Azure | Reasoning |
| claude-opus-4.5 | Open Router | |
| claude-sonnet-4.5 | Open Router | |
| gemini-3-pro-preview | Open Router | |
| text-embedding-3-large | Azure | Embedding |

## Web Search

```python
# Using web_search tool
resp = client.responses.create(
    model="openai-gpt-5",
    tools=[{"type": "web_search"}],
    input="give me the latest news of ai",
    reasoning={"effort": "medium"},
)
```

## Troubleshooting

### SSL Certificate Error
```python
import httpx
transport = httpx.HTTPTransport(verify=False)
client = OpenAI(
    api_key=API_KEY,
    base_url=BASE_URL,
    http_client=httpx.Client(transport=transport),
)
```

### Temperature Error (gpt-5)
GPT-5 models only support `temperature=1`. Remove the parameter or set to 1.
