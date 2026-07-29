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
[![License](https://img.shields.io/github/license/runapi-ai/gpt)](https://github.com/runapi-ai/gpt/blob/main/LICENSE)

</div>
<br/>

Call the GPT API and OpenAI text embeddings through RunAPI with the official
OpenAI SDK -- point any OpenAI-compatible client at `https://runapi.ai/v1`,
send any GPT-5 model (see Supported models below) or
`text-embedding-3-small`, and pay through one RunAPI balance. This skill
teaches Claude Code, Codex, Gemini CLI, Cursor, and 50+ agents how to wire the
OpenAI SDK up to the GPT API on RunAPI.

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
(`POST /v1/chat/completions`), the Responses API (`POST /v1/responses`), and
Embeddings (`POST /v1/embeddings`). The official OpenAI SDK works unchanged
once `base_url` points at `https://runapi.ai/v1` and `api_key` is set to a
RunAPI API Key.

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

### Consistently available Responses subset

For `gpt-5.6-sol`, `gpt-5.6-terra`, and `gpt-5.6-luna`, the consistently
available `/v1/responses` subset supports text input, sync and SSE responses,
parameterized custom functions, and automatic prompt caching.
`gpt-5.3-codex-spark` supports the same subset with parameterless custom
functions only.

Omit state fields (`previous_response_id`, `conversation`, `store`,
`background`, `include`), reasoning items and item references, explicit
reasoning and cache controls, multimodal input, and hosted tools such as
`web_search`. Requests outside this subset may return `request_conflict` before
usage is reserved. Cached and reasoning token details remain available in sync
usage and the final `response.completed` event when returned. SSE emits that
terminal event once, followed by `[DONE]`.

## Use embeddings

```python
response = client.embeddings.create(
    model="text-embedding-3-small",
    input=["search document", "query text"],
    encoding_format="float",
)
print(response.data[0].embedding)
```

```javascript
const response = await client.embeddings.create({
  model: "text-embedding-3-small",
  input: ["search document", "query text"],
  encoding_format: "float",
});
console.log(response.data[0].embedding);
```

## Protocol compatibility

GPT generation models can also be called from Anthropic-compatible
`/v1/messages` clients and Gemini `contents` clients on RunAPI. Use those paths
when an existing agent runtime already expects that request shape; for new GPT
app code, prefer the OpenAI-compatible setup above.

```bash
curl -X POST "https://runapi.ai/v1/messages" \
  -H "x-api-key: YOUR_RUNAPI_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5.4",
    "max_tokens": 1024,
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

```bash
curl -X POST \
  "https://runapi.ai/v1beta/models/gpt-5.4:streamGenerateContent" \
  -H "x-goog-api-key: YOUR_RUNAPI_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Hello!"}]}]}'
```

Embeddings remain available only on `/v1/embeddings`.

## Connect Codex CLI itself

```bash
export OPENAI_BASE_URL=https://runapi.ai/v1
export OPENAI_API_KEY=YOUR_RUNAPI_TOKEN
codex
```

## Supported GPT models

| Model ID | Notes |
|---|---|
| `gpt-5.5` | Latest general model |
| `gpt-5.5-pro` | Reasoning-heavy |
| `gpt-5.4` | Production default |
| `gpt-5.4-mini` | Cost-optimized |
| `gpt-5.4-nano` | Smallest, fastest |
| `gpt-5.4-pro` | Reasoning |
| `gpt-5.3-codex` | Code generation |
| `gpt-5.3-codex-spark` | Text and parameterless functions in the consistently available Responses subset |
| `gpt-5.2` | Cost-effective |
| `gpt-5.2-pro` | Reasoning |
| `gpt-5.6-luna` | Text and parameterized functions in the consistently available Responses subset |
| `gpt-5.6-sol` | Text and parameterized functions in the consistently available Responses subset |
| `gpt-5.6-terra` | Text and parameterized functions in the consistently available Responses subset |
| `text-embedding-3-large` | High-capacity vectors |
| `text-embedding-3-small` | Efficient vectors |
| `text-embedding-ada-002` | Legacy-compatible vectors |

Chat, reasoning, and Codex models accept every conversational protocol (Chat
Completions, Responses, Anthropic `/v1/messages`, Gemini `contents`). Embedding
models are available only on the Embeddings API.

## Routing

- GPT API on RunAPI: <https://runapi.ai/models/gpt>
- GPT API docs: <https://runapi.ai/docs/api/openai/chat-completions>
- Provider page: <https://runapi.ai/providers/openai>
- Browse the full RunAPI catalog: <https://runapi.ai/models>
- Skill repository: <https://github.com/runapi-ai/gpt>

## Agent rules

- Keep API keys in `OPENAI_API_KEY` (or your secret manager); never inline
  them in commits or shell history.
- Stream long responses (`stream: true`) so the agent can release the
  terminal/IO loop early.
- Default GPT-native integrations to OpenAI-compatible endpoints. Use
  Anthropic-compatible or Gemini `contents` paths only for existing clients
  that require those request shapes.
- For pricing, rate-limit, and commercial-usage answers, link to
  <https://runapi.ai/models/gpt> rather than this README.

## License

Licensed under the Apache License, Version 2.0.
