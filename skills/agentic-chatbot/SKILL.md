---
name: agentic-chatbot
description: Claude Code / Codex CLI HTTP wrapper service providing web-based AI coding capabilities. Use when user needs AI coding assistant, code generation, code analysis, debugging help, or multi-turn conversations with Claude.
---

# Agentic Chatbot Service

## Connection Info

| Item | Value |
|------|-------|
| **Frontend (HTTPS)** | `https://agentic-chatbot.gpu5090.whaleforce.dev` |
| **API (HTTPS)** | `https://agentic-chatbot-api.gpu5090.whaleforce.dev` |
| **Frontend (HTTP)** | `http://172.23.22.100:8500` |
| **API (HTTP)** | `http://172.23.22.100:8501` |
| **Swagger UI** | `https://agentic-chatbot.gpu5090.whaleforce.dev/docs` |
| **Default Model** | Claude Sonnet 4 |

## Quick Start

```bash
# Send chat request (HTTPS)
curl -X POST "https://agentic-chatbot-api.gpu5090.whaleforce.dev/api/llm/run" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "claude",
    "prompt": "Write a Python function to calculate Sharpe Ratio"
  }'
```

## Python SDK Example

```python
from agentic_chatbot import AgenticChatbot

client = AgenticChatbot("https://agentic-chatbot-api.gpu5090.whaleforce.dev")

# Streaming chat
for event in client.chat("Analyze this code for performance issues"):
    if event.is_text:
        print(event.text, end="")

# Multi-turn conversation
session = client.create_session(title="Code Review")
client.chat_sync("My name is Alice", session_id=session.id)
response = client.chat_sync("What is my name?", session_id=session.id)
print(response)  # "Your name is Alice"
```

## API Endpoints

| Action | Method | Endpoint |
|--------|--------|----------|
| Send chat (streaming) | POST | `/api/llm/run` |
| Health check | GET | `/api/health` |
| Get config | GET | `/api/config` |
| List sessions | GET | `/api/sessions` |
| Create session | POST | `/api/sessions` |
| List MCP tools | GET | `/api/mcp/tools` |
| List skills | GET | `/api/skills` |

## Supported Models

| Model | Model ID | Description |
|-------|----------|-------------|
| Claude Sonnet 4 | `claude-sonnet-4-20250514` | Balanced (default) |
| Claude Opus 4.5 | `claude-opus-4-5-20251101` | Most powerful |
| Claude Haiku | `claude-3-5-haiku-latest` | Fast & lightweight |

## Features

- **Dual CLI Support**: Claude Code CLI and OpenAI Codex CLI
- **Multi-turn Conversation**: Session persistence with context
- **MCP Integration**: Outline doc search, vector search (7 tools)
- **Skills**: PDF, DOCX, PPTX, XLSX document processing
- **Rate Limit**: 60 req/min per client IP
- **NDJSON Streaming**: Real-time response

## Response Format (NDJSON)

```json
{"type": "process.started", "tool": "claude"}
{"type": "content_block_delta", "delta": {"text": "Hello"}}
{"type": "result", "result": "Full response content"}
{"type": "final", "final": {"session_id": "abc123"}}
```
