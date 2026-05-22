<p align="center">
  <a href="https://github.com/runapi-ai/gpt">
    <h3 align="center">GPT API Skill for RunAPI</h3>
  </a>
</p>

<p align="center">
  Configure existing OpenAI SDK clients to use GPT models on RunAPI.
</p>

<p align="center">
  <a href="https://runapi.ai/models/gpt"><strong>Model Reference</strong></a> · <a href="https://github.com/runapi-ai/gpt"><strong>Skill Repo</strong></a> · <a href="https://runapi.ai/models"><strong>All Models</strong></a>
</p>

<div align="center">

[![skills.sh](https://www.skills.sh/b/runapi-ai/gpt)](https://www.skills.sh/runapi-ai/gpt/gpt)
[![ClawHub](https://img.shields.io/badge/ClawHub-runapi--gpt-111827)](https://clawhub.ai/runapi-ai/runapi-gpt)
[![Model Reference](https://img.shields.io/badge/Model%20Reference-runapi.ai-0f766e)](https://runapi.ai/models/gpt)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-skills.sh-2563eb)](https://www.skills.sh/runapi-ai)
[![License](https://img.shields.io/github/license/runapi-ai/gpt)](https://github.com/runapi-ai/gpt/blob/main/LICENSE)

</div>
<br/>

Call the GPT API through RunAPI with the official OpenAI SDK — point any
OpenAI-compatible client at `https://runapi.ai/v1`, send `gpt-5.2`,
`gpt-5.4`, `gpt-5.4-mini`, `gpt-5.5`, or `gpt-5.3-codex`, and pay through one
RunAPI balance. This skill teaches Claude Code, Codex, Gemini CLI, Cursor,
and 50+ agents how to wire the OpenAI SDK up to the GPT API on RunAPI.

The canonical agent file is `skills/gpt/SKILL.md`.

## Install the skill

```bash
npx skills add runapi-ai/gpt -g
```

Or paste this prompt to your AI agent:

```text
Install the gpt skill for me:

1. Clone https://github.com/runapi-ai/gpt
2. Copy the skills/gpt/ directory into your
   user-level skills directory (e.g. ~/.claude/skills/
   for Claude Code, ~/.codex/skills/ for Codex).
3. Verify that SKILL.md is present.
4. Confirm the install path when done.
```

## Use the GPT API on RunAPI

The GPT API on RunAPI speaks the standard OpenAI protocol: Chat Completions
(`POST /v1/chat/completions`) and the Responses API (`POST /v1/responses`).
The official OpenAI SDK works unchanged once `base_url` points at
`https://runapi.ai/v1` and `api_key` is set to a RunAPI API Key.

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_RUNAPI_TOKEN",
    base_url="https://runapi.ai/v1",
)

response = client.chat.completions.create(
    model="gpt-5.4",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(response.choices[0].message.content)
```

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "YOUR_RUNAPI_TOKEN",
  baseURL: "https://runapi.ai/v1",
});

const response = await client.chat.completions.create({
  model: "gpt-5.4",
  messages: [{ role: "user", content: "Hello!" }],
});
console.log(response.choices[0].message.content);
```

```bash
curl -X POST "https://runapi.ai/v1/chat/completions" \
  -H "x-api-key: YOUR_RUNAPI_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5.4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

Get a RunAPI API Key at <https://runapi.ai/api_keys>.

## Connect Codex CLI itself

```bash
export OPENAI_BASE_URL=https://runapi.ai/v1
export OPENAI_API_KEY=YOUR_RUNAPI_TOKEN
codex
```

## Supported GPT models

| Model ID | API | Notes |
|---|---|---|
| `gpt-5.5` | Chat Completions, Responses | Latest general model |
| `gpt-5.5-pro` | Responses only | Reasoning-heavy |
| `gpt-5.4` | Chat Completions, Responses | Production default |
| `gpt-5.4-mini` | Chat Completions, Responses | Cost-optimized |
| `gpt-5.4-nano` | Chat Completions, Responses | Smallest, fastest |
| `gpt-5.4-pro` | Responses only | Reasoning |
| `gpt-5.3-codex` | Chat Completions, Responses | Code generation |
| `gpt-5.3-codex-spark` | Chat Completions, Responses | Faster Codex variant |
| `gpt-5.2` | Chat Completions, Responses | Cost-effective |
| `gpt-5.2-pro` | Responses only | Reasoning |

Pro models (`gpt-5.*-pro`) only support the Responses API per OpenAI.

## Routing

- GPT API on RunAPI: <https://runapi.ai/models/gpt>
- GPT API docs: <https://runapi.ai/docs#gpt>
- Provider page: <https://runapi.ai/providers/openai>
- Browse the full RunAPI catalog: <https://runapi.ai/models>
- Skill repository: <https://github.com/runapi-ai/gpt>

## Agent rules

- Keep API keys in `OPENAI_API_KEY` (or your secret manager); never inline
  them in commits or shell history.
- Stream long responses (`stream: true`) so the agent can release the
  terminal/IO loop early.
- For pricing, rate-limit, and commercial-usage answers, link to
  <https://runapi.ai/models/gpt> rather than this README.

## License

Licensed under the Apache License, Version 2.0.
