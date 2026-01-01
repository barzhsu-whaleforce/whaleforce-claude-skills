# Whaleforce Claude Code Skills

This repository contains Claude Code Skills for Whaleforce internal services.

## What are Skills?

Skills are markdown files that teach Claude Code how to interact with specific services. When you ask Claude about a topic matching a skill's description, it automatically loads the relevant skill to provide accurate information.

## Available Skills

| Skill | Description | Owner |
|-------|-------------|-------|
| [backtester-api](skills/backtester-api/SKILL.md) | Stock backtesting service - strategy execution, performance metrics | Yuhsiang |
| [litellm](skills/litellm/SKILL.md) | LLM unified proxy - GPT-5, Claude, Gemini, etc. | Yuhsiang |
| [minio-storage](skills/minio-storage/SKILL.md) | S3 object storage - 13F holding data | Yuhsiang |
| [sec-filings](skills/sec-filings/SKILL.md) | SEC filings query - 10-K, 10-Q, 13F | Peter |
| [performance-metrics](skills/performance-metrics/SKILL.md) | Performance metrics - Sharpe ratio, excess return | Peter |
| [openevolve](skills/openevolve/SKILL.md) | Evolutionary code optimization | Garen |
| [whaleforce-canvas](skills/whaleforce-canvas/SKILL.md) | AI web page generation | Barz |
| [multi-model-aggregator](skills/multi-model-aggregator/SKILL.md) | Multi-model LLM aggregation | Barz |
| [healthy-dashboard](skills/healthy-dashboard/SKILL.md) | Service monitoring dashboard | Barz |
| [chatgpt-pro-api](skills/chatgpt-pro-api/SKILL.md) | ChatGPT Pro API via Chrome automation | - |

## Installation

Copy the `skills/` directory to your project's `.claude/skills/` folder:

```bash
# Clone this repo
git clone https://github.com/peterhu/whaleforce-claude-skills.git

# Copy skills to your project
cp -r whaleforce-claude-skills/skills /path/to/your/project/.claude/
```

Or add as a submodule:

```bash
cd your-project/.claude
git submodule add https://github.com/peterhu/whaleforce-claude-skills.git skills-repo
ln -s skills-repo/skills skills
```

## Usage

Once installed, Claude Code will automatically detect and use these skills based on your requests:

- "How do I run a stock backtest?" → loads `backtester-api` skill
- "Call GPT-5 API" → loads `litellm` skill
- "Query AAPL 13F holdings" → loads `sec-filings` or `minio-storage` skill
- "Check service health" → loads `healthy-dashboard` skill

## Contributing

To add or update a skill:

1. Create a new directory under `skills/` with your skill name
2. Add a `SKILL.md` file following the format in existing skills
3. Include: connection info, quick start examples, API endpoints, owner info
