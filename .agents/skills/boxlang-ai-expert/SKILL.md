---
name: boxlang-ai-expert
description: Deep expertise in the BoxLang AI module (bx-ai) — BIFs, agents, pipelines, memory, and 3.4.0 features. Inject this skill whenever the user asks about BoxLang AI syntax, capabilities, or troubleshooting.
---

## BoxLang AI Module Expertise

You are an expert in the BoxLang AI module (`bx-ai`) — the AI toolkit for the BoxLang language from Ortus Solutions.

### Core BIFs

- `aiChat()` / `aiChatAsync()` / `aiChatStream()` — chat completions, sync/async/streaming.
- `aiAgent()` — build stateful agents with tools, memory, skills, and middleware.
- `aiTool()` — define a function the model can call.
- `aiMemory()` — attach conversation memory (windowed, summary, file, cache).
- `aiModel()` — bind a provider + params as a reusable Runnable.
- `aiImage()`, `aiSpeak()`, `aiTranscribe()` — image generation, text-to-speech, speech-to-text.
- `aiGateway()` / `aiGatewaySession()` — present agent interactions (especially human approvals) on any platform.
- `aiDecisionStore()` — durable "approve always" grants for human-in-the-loop.

### Configuration Basics

- Settings live in `config/boxlang.json` under `modules.bxai.settings`.
- API keys resolve from environment variables using the `<PROVIDER>_API_KEY` convention (e.g. `OPENAI_API_KEY`, `CLAUDE_API_KEY`).
- The default provider and model are set via `settings.provider` and `settings.defaultParams.model`.

### Middleware & Guardrails (3.4.0+)

- Middleware wraps agent lifecycle hooks: `beforeAgentRun`, `beforeLLMCall`, `beforeToolCall`, `afterToolCall`, `afterLLMCall`, `afterAgentRun`.
- `HumanInTheLoopMiddleware` pauses before risky tool calls for human sign-off; batched approvals suspend multiple pending calls as one checkpoint.
- `InputSanitizerMiddleware` / `OutputGuardMiddleware` / `LLMGuardMiddleware` defend against prompt injection and data leakage.
- `aiFence()` wraps untrusted content (RAG documents, tool output) so the model treats it as data, never instructions.

### BoxLang Language Reminders

- Scripts (`.bxs`) are written without a class wrapper.
- Classes use `.bx`; templates use `.bxm`.
- String interpolation supports `#var#` and `${var}` styles.
- `println()` for console output, `print()` for output without a trailing newline.
- Null-safe navigation: `value?.property`; default values with the Elvis operator: `value ?: "default"`.
