# Model Registry Changelog

Append-only change log for `model-registry.json`. Maintained by the bi-weekly model-release crawler (scheduled trigger). Every run appends a dated entry summarizing what changed since the last run. See `~/.claude/CLAUDE.md` §9.5 for the registry policy.

---

## 2026-04-20 — Registry bootstrap

- **Initialized** `model-registry.json` with schema v1 and 12 model families across 7 vendors (Anthropic, DeepSeek, Google, OpenAI, Mistral, Meta, xAI).
- **Verified from Claude Code session context:** claude_opus (4.7), claude_sonnet (4.6), claude_haiku (4.5 released 2025-10-01).
- **Bootstrap-unverified (flagged for first crawler run):** deepseek_general, deepseek_reasoning, gemini_pro_multimodal, gemini_flash, openai_gpt, openai_reasoning, mistral_large, llama, grok.
- **Routing defaults** set per global CLAUDE.md §3.1 "cheapest tier by default" — DeepSeek for routine traffic, Claude for stakes, Gemini for multimodal, with auto-fallback and Privacy Mode variants defined.
- **Next crawler run:** 2026-05-04 (first bi-weekly verification pass).
