# Changelog

## [v0.2.11](https://github.com/runapi-ai/gpt/releases/tag/v0.2.11) - 2026-08-10

### Changed
- Complete marketplace distribution for the RunAPI GPT skill.


## [v0.2.10](https://github.com/runapi-ai/gpt/releases/tag/v0.2.10) - 2026-07-22

### Added
- Document text, streaming, custom function, and automatic prompt caching support for GPT 5.6 and Codex Spark.

### Changed
- Clarify advanced Responses capabilities that may return `request_conflict` before usage is reserved, plus the terminal SSE Usage and `[DONE]` contract.


## [v0.2.9](https://github.com/runapi-ai/gpt/releases/tag/v0.2.9) - 2026-07-02

### Changed
- Supported models table simplified; every chat/reasoning/Codex model is callable via Chat Completions, Responses, Anthropic Messages, and Gemini contents (embeddings via /v1/embeddings only). Removed the misleading per-model endpoint split.

## [v0.2.8](https://github.com/runapi-ai/gpt/releases/tag/v0.2.8) - 2026-07-02

### Changed
- Supported models and endpoint tables are now generated from the RunAPI catalog.
- Description no longer enumerates a fixed model subset; the tables are the authoritative roster.

## [v0.2.7](https://github.com/runapi-ai/gpt/releases/tag/v0.2.7) - 2026-06-24

### Changed
- Clarify GPT generation compatibility across OpenAI-compatible, Anthropic Messages, and Gemini contents clients.

## [v0.2.6](https://github.com/runapi-ai/gpt/releases/tag/v0.2.6) - 2026-06-18

### Changed
- Sync skill content with the latest API surface

## [v0.2.5](https://github.com/runapi-ai/gpt/releases/tag/v0.2.5) - 2026-06-01

### Changed
- Align skill with upstream Input Contract and public API vocabulary changes
- Update endpoint definitions and field constraints
