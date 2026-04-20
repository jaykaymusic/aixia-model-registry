# Model Registry Changelog

Append-only change log for `model-registry.json`. Maintained by the bi-weekly model-release crawler (scheduled trigger). Every run appends a dated entry summarizing what changed since the last run. See `~/.claude/CLAUDE.md` §9.5 for the registry policy.

---

## 2026-04-20 — Registry bootstrap

- **Initialized** `model-registry.json` with schema v1 and 12 model families across 7 vendors (Anthropic, DeepSeek, Google, OpenAI, Mistral, Meta, xAI).
- **Verified from Claude Code session context:** claude_opus (4.7), claude_sonnet (4.6), claude_haiku (4.5 released 2025-10-01).
- **Bootstrap-unverified (flagged for first crawler run):** deepseek_general, deepseek_reasoning, gemini_pro_multimodal, gemini_flash, openai_gpt, openai_reasoning, mistral_large, llama, grok.
- **Routing defaults** set per global CLAUDE.md §3.1 "cheapest tier by default" — DeepSeek for routine traffic, Claude for stakes, Gemini for multimodal, with auto-fallback and Privacy Mode variants defined.
- **Next crawler run:** 2026-05-04 (first bi-weekly verification pass).

---

## 2026-04-20 — First crawler run: 11 updates, 1 new entry, 2 🚨 pricing corrections

- 🚨 **gemini_pro_multimodal:** pricing $1.25/$5.00 → $2.00/$12.00 · Bootstrap estimate was significantly low; verified from Gemini API pricing. Tiered: $4/$18 above 200K tokens.
- 🚨 **gemini_flash:** pricing $0.10/$0.40 → $0.50/$3.00 · Bootstrap estimate was significantly low; verified from Gemini API pricing. Budget alternative: gemini-3.1-flash-lite at $0.25/$1.50.
- **claude_opus:** pricing $15.00/$75.00 → $5.00/$25.00 · Correction — bootstrap value reflected older Opus 3 pricing; multiple April 2026 sources confirm Opus 4.7 is $5/$25. Price DROP, not a hike. Claude Mythos confirmed Apr 7 but withheld from public API under Project Glasswing.
- **claude_sonnet:** verified; no change ($3/$15, claude-sonnet-4-6 confirmed).
- **claude_haiku:** verified; no change ($1/$5, claude-haiku-4-5-20251001 confirmed).
- **deepseek_general:** deepseek-v3 → deepseek-v3.2 · V3.2-Exp released Sept 29 2025; output pricing corrected $1.10 → $0.42 (significant drop). Adds Thinking-in-Tool-Use. `bootstrap_unverified` cleared.
- **deepseek_reasoning:** verified; deepseek-r1 at $0.55/$2.19 confirmed. DeepSeek-R2 anticipated but not yet released. `bootstrap_unverified` cleared.
- **openai_gpt:** UNKNOWN → gpt-5.4 · Released March 5 2026; 1.05M context; $2.50/$15.00. gpt-5.4-mini also available ($0.75/$4.50, 400K context, March 17 2026).
- **openai_reasoning:** UNKNOWN → o4-mini · Newest OpenAI reasoning model; $1.10/$4.40, 200K context. Effective cost 5–20× listed due to billed reasoning tokens.
- **mistral_large:** UNKNOWN → mistral-large-3-2512 · Released Dec 2025; $0.50/$1.50, 262K context. EU data residency option.
- **llama:** UNKNOWN → llama-4-maverick · Released April 2025; $0.15/$0.60, 1M context, natively multimodal open-weight MoE. Llama 4 Scout (10M context) also available.
- **grok:** UNKNOWN → grok-4.20-0309-v2 · Released April 7 2026; $2.00/$6.00, 2M context, four-agent architecture.
- **New family added:** mistral_small · Mistral · mistral-small-2603, $0.15/$0.60, 256K context, native multimodal MoE. Cheapest EU-residency multimodal option in registry — relevant for Echoform Studio privacy-mode tiers needing image input.
- **No changes for:** claude_sonnet, claude_haiku (pricing and versions stable).

**Summary:** First bi-weekly crawler run populated 9 previously UNKNOWN families and corrected pricing on 3 entries. The two 🚨 Gemini corrections are most impactful: gemini_flash is 5× higher than bootstrapped estimate and gemini_pro_multimodal is 2.4× higher — Echoform, AIXIA, and any project routing bulk tasks or video verification through Gemini should audit budget estimates immediately. claude_opus pricing dropped 67% vs bootstrap (stale Opus 3 value); this is net-positive for swarm/QA workflows. One new entry (mistral_small) adds a cheap EU-residency multimodal path. No model deprecations this cycle.
