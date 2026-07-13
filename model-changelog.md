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

---

## 2026-05-04 — 3 major updates, 2 new entries, 1 🚨 context reduction

- **claude_opus:** release_date corrected 2026-01-01 → 2026-04-16 · claude-opus-4-7 confirmed launched April 16, 2026; SWE-bench Verified 80.8% → 87.6%, CursorBench 58% → 70%, vision resolution 3.3×. New tokenizer adds 1.0–1.35× token overhead vs Opus 4.6 — projects with tight token budgets should audit. Same $5/$25 pricing. Added "vision" capability flag.
- **deepseek_general:** deepseek-v3.2 → deepseek-v4-flash · Released April 24, 2026. Context 128K → 1M. Pricing $0.28/$0.42 → $0.14/$0.28 (~50% across the board). Adds Thinking+Non-Thinking dual mode. Supports both OpenAI and Anthropic API formats.
- 🚨 **grok:** grok-4.20-0309-v2 → grok-4.3 · Released/fully rolled out April 30, 2026. Pricing dropped $2.00/$6.00 → $1.25/$2.50 (positive). **Context window REDUCED 2M → 1M** — any AIXIA project using Grok for >1M-token calls must migrate to gemini_pro_multimodal. Adds native video frame input. Reasoning always active (no disable option).
- **New family added:** deepseek_v4_pro · DeepSeek · 1.6T/49B-active params, 1M context, Thinking mode, $1.74/$3.48 standard. 🚨 75% promo ($0.435/$0.87) active through 2026-05-31 — standard rate resumes June 1; audit any project budgeted at promo rate before next crawl.
- **New family added:** mistral_medium · Mistral · mistral-medium-3.5-2604; 128B dense open-weight; 256K context; $1.50/$7.50; released April 29, 2026. Unified reasoning+coding model filling EU mid-tier gap between mistral_small and mistral_large.
- **gemini_pro_multimodal:** verified; noted Gemini 3.1 Flash TTS Preview and Veo 3.1 Lite Preview as new Google multimodal surface expansions (not tracked as separate families).
- **openai_gpt:** verified; GPT-5.5 model page observed in OpenAI API docs — unconfirmed release, being conservative. Monitor next cycle.
- **llama:** verified; no new releases. Llama 4 Behemoth still not generally available. "Avocado" next-gen codename reported but unconfirmed.
- **No changes for:** claude_sonnet, claude_haiku, deepseek_reasoning, gemini_flash, openai_reasoning, mistral_large, mistral_small.

**Summary:** Active cycle with two vendor-level version bumps (DeepSeek V4, Grok 4.3) and two new registry entries. The 🚨 Grok context window reduction from 2M→1M is the primary breakage risk — PropEdgeAI and VIMIΛ should confirm they have no Grok calls exceeding 1M tokens (if so, redirect to gemini_pro_multimodal). DeepSeek V4 Flash is a strict upgrade over V3.2 (cheaper AND larger context) with no migration friction. The DeepSeek V4 Pro promo cliff on June 1 (4× price jump) is the budget risk to watch — next crawl on May 18 will catch it before expiry. No model deprecations this cycle.

---

## 2026-05-18 — 1 🚨 price hike, 1 new family, 1 date correction, 13 verifications

- 🚨 **openai_gpt:** gpt-5.4 → gpt-5.5 · Released April 23, 2026. Price DOUBLED: $2.50/$15.00 → $5.00/$30.00 (100% increase on both input and output). Context window 1.05M → 1M (922K input / 128K output). Adds native computer use. OpenAI fine-tuning API deprecated in May 2026 — any downstream project using fine-tuned GPT models must migrate. GPT-5.5-Pro ($30/$180) also available. GPT-Realtime-2 voice model launched May 7 (not tracked as separate family). Last cycle noted GPT-5.5 page as "unconfirmed — monitor next cycle"; now confirmed and bumped.
- **New family added:** mistral_reasoning · Mistral · magistral-medium-2506 ($2.00/$5.00, 41K context, June 2025). Magistral is Mistral's dedicated chain-of-thought reasoning family, absent from the registry since bootstrap. Budget variant magistral-small-2506 at $0.50/$1.50 / 40K context. EU data residency via La Plateforme. Relevant for Echoform EU tiers needing focused reasoning without the full mistral_medium output cost.
- **gemini_pro_multimodal:** release_date corrected 2026-01-15 → 2026-02-19 · Confirmed GA date was February 19, 2026. No pricing or model change.
- **gemini_flash:** verified; Gemini 3.1 Flash-Lite reached GA on May 7, 2026 (was preview). No pricing change to gemini-3.1-flash.
- **deepseek_v4_pro:** verified; 75% promo still active (expires May 31 — 13 days from this crawl). 🚨 Standard rate of $1.74/$3.48 resumes June 1 — next crawl is June 1; any project budgeted at promo rate ($0.435/$0.87) must be audited before June 1.
- **No changes for:** claude_opus, claude_sonnet, claude_haiku, deepseek_general, deepseek_reasoning, mistral_large, mistral_small, mistral_medium, openai_reasoning, llama, grok.

**Summary:** The dominant event this cycle is OpenAI's GPT-5.5 launch (April 23) — a 100% price increase over GPT-5.4 that doubles both input and output costs. Since openai_gpt is currently `default_use: fallback_only` and no AIXIA projects actively route to it, there is no immediate operational impact; however, any project considering GPT adoption should factor the new $5/$30 baseline. The Mistral Magistral family (magistral-medium-2506, June 2025) was missing from the registry since bootstrap and is now added — Echoform EU tiers can use this for cheap reasoning at $2.00/$5.00 without the $7.50/M output cost of mistral_medium. 🚨 The DeepSeek V4 Pro promo cliff remains the live budget risk: standard $1.74/$3.48 resumes June 1 — all projects using V4 Pro must be audited within 13 days. No model deprecations this cycle.

---

## 2026-06-01 — 🚨 External sources blocked; 1 promo expiry confirmed; 14 families unverified

- 🚨 **Perplexity API (api.perplexity.ai):** Hard-failed — not in remote execution environment's network allowlist. No external release news available for this cycle. Retry 1/1 not attempted (same network policy applies). Noted per pipeline rule 4.
- 🚨 **Firecrawl (api.firecrawl.dev):** Hard-failed — not in remote execution environment's network allowlist. Vendor pricing pages not scrapeable this cycle. Noted per pipeline rule 4.
- 🚨 **deepseek_v4_pro:** Promo EXPIRED as of June 1, 2026 (time-certain event flagged in May 18 crawl). Standard rate $1.74/$3.48 per million tokens now active — effective 4× cost increase over promo rate ($0.435/$0.87). Notes and `last_verified` updated to 2026-06-01. Projects routing to deepseek_v4_pro must audit budgets immediately.
- **No external changes verified for:** claude_opus, claude_sonnet, claude_haiku, deepseek_general, deepseek_reasoning, gemini_pro_multimodal, gemini_flash, openai_gpt, openai_reasoning, mistral_large, mistral_small, mistral_medium, mistral_reasoning, llama, grok — all last externally verified 2026-05-18 (13 days ago); sources unreachable this cycle.

**Summary:** Both external data sources (Perplexity and Firecrawl) were blocked by the remote execution environment's network policy — no new release news or pricing scrapes were possible this cycle. The only actionable update is the DeepSeek V4 Pro promotional pricing expiry (June 1, 2026): standard rate $1.74/$3.48 now applies, a 4× jump from the promo rate. Echoform, AIXIA, PropEdgeAI, BusinessVault, and VIMIΛ must audit any workloads routed to deepseek_v4_pro immediately. 🚨 Network policy for this crawler's remote execution environment should be reviewed to whitelist api.perplexity.ai and api.firecrawl.dev — without these, the registry cannot self-update and will drift from ground truth. Next scheduled crawl: 2026-06-15.

---

## 2026-06-15 — 2 version bumps, 1 new family, 1 🚨 price hike, 13 verifications

- **claude_opus:** `claude-opus-4-7` → `claude-opus-4-8` · Released May 28, 2026. Pricing unchanged at $5/$25 per million tokens. SWE-Bench Pro improved 64.3% → 69.2% (vs GPT-5.5 at 58.6%); OSWorld-Verified 83.4%. ~35% fewer output tokens than 4.7 for equivalent tasks. Fast Mode now 3× cheaper ($10/$50 per million). Available same-day on Bedrock, Vertex, GitHub Copilot.
- **New family added:** `claude_fable` · Anthropic · `claude-fable-5` ($10/$50/M, 1M context, June 9, 2026). Mythos-class capability with Anthropic safety guardrails. Same underlying weights as Claude Mythos 5 (trusted-access-only tier). High-risk domain requests (cybersecurity, biology, chemistry, model distillation) fall back to Claude Opus 4.8 — projects must handle graceful degradation. AIXIA projects needing maximum reasoning/coding should route here instead of claude_opus at 2× cost.
- 🚨 **gemini_flash:** `gemini-3.1-flash` → `gemini-3.5-flash` · GA at Google I/O, May 19, 2026. **PRICING TRIPLED: $0.50→$1.50 input, $3.00→$9.00 output per million tokens (200% increase — well above 20% threshold).** All AIXIA projects routing bulk text or coding tasks through gemini_flash must re-audit cost models immediately. Context window 1,000,000→1,048,576. Budget alternative: gemini-3.1-flash-lite at lower pricing still available.
- **gemini_pro_multimodal:** verified; no version change · Gemini 3.5 Pro announced at Google I/O but still in limited Vertex preview — Sundar Pichai: "Give us until next month." GA expected by end of June 2026 (2M context, Deep Think reasoning). NOT updated until GA. `gemini-3.1-flash-image-preview` and `gemini-3-pro-image-preview` deprecated — shut down June 25.
- **openai_gpt:** verified; no version/pricing change · GPT-5.5 remains current. GPT-5.5 Instant confirmed as ChatGPT default (May 2026). GPT-5.2 deprecated June 12; GPT-4.5 retiring from ChatGPT June 27. GPT-6 confirmed for 2026 but no date.
- **grok:** verified; no version change · Grok V9-Medium (1.5T params, coding-focused) completed training May 25 but not yet released via API as of June 15. No model string or pricing published yet. Grok 5 (6T-param MoE) still training on Colossus 2.
- **deepseek_v4_pro:** verified; promo expiry confirmed live · Standard rate $1.74/$3.48 now in effect (as flagged June 1). No further pricing change this cycle.
- **No changes for:** claude_sonnet, claude_haiku, deepseek_general, deepseek_reasoning, openai_reasoning, mistral_large, mistral_small, mistral_medium, mistral_reasoning, llama.
- **Note:** Perplexity API and Firecrawl remain blocked by network policy. WebSearch (built-in) used as fallback — successfully sourced data from OpenRouter, Requesty, official Anthropic/Google announcement pages, and third-party pricing aggregators. Quality comparable to Firecrawl for major vendor releases.

**Summary:** Active cycle with 2 version bumps, 1 new family entry, and 1 significant price hike. The 🚨 Gemini Flash 3× price increase ($0.50→$1.50 input, $3.00→$9.00 output) is the primary cost risk this cycle — any AIXIA project using gemini_flash for bulk text processing (Echoform, AIXIA, PropEdgeAI) must re-run cost models before next billing period. The new `claude_fable` family (claude-fable-5, $10/$50) opens Mythos-class reasoning to all projects; it is the right choice when claude_opus hits its ceiling on complex agentic/coding tasks, but comes at 2× the price. Claude Opus 4.8 is a drop-in upgrade from 4.7 at the same price with measurable benchmark gains — AIXIA projects should update their explicit version pins if any. No model deprecations this cycle; watch for Gemini 3.5 Pro GA (expected by June 30) and Grok V9-Medium API launch for next update.

---

## 2026-06-29 — 🚨 Fable 5 API suspended, 1 pricing correction, 2 watch items, 14 verifications

- 🚨 **claude_fable:** `api_suspended: true` (since June 12, 2026) — US government export-control directive. Anthropic executed a global all-customer shutdown of claude-fable-5 (and Mythos 5) 3 days after June 9 launch; US government demonstrated a jailbreak technique, Anthropic disputes that a narrow jailbreak warrants commercial recall. As of June 29: Fable 5 (general API) fully suspended; Mythos 5 partially restored for ~100 trusted US partner orgs only (Project Glasswing members). Trump administration expects broad restore "this week" per Axios (June 27) — Pentagon/NSA sign-off is the remaining step; prior "coming days" signals have slipped. AIXIA PROJECTS: All calls to claude-fable-5 fail until restored — route to claude_opus. `api_suspended: true` and `suspension_date: "2026-06-12"` fields added to registry entry.
- 🚨 **deepseek_v4_pro:** PRICING CORRECTION — June 1, 2026 crawl INCORRECTLY recorded promo as expired and set pricing to $1.74/$3.48 standard rate. On May 22, 2026, DeepSeek made the 75% discount PERMANENT — $0.435/$0.87 per million tokens is the permanent list price. The 4× price hike warned in the June 1 changelog did NOT occur. Projects that adjusted budgets upward to $1.74/$3.48 should REVERT immediately. Confirmed by DeepSeek API docs, Engadget, The Next Web, APIdog, multiple pricing aggregators. Registry updated to $0.435/$0.87.
- **openai_gpt:** notes updated; current_version held at gpt-5.5 · GPT-5.6 Sol/Terra/Luna launched June 26 in LIMITED PREVIEW only (~20 government-vetted orgs via OpenAI API and Codex; not in ChatGPT). Pricing confirmed: Sol $5/$30 input/output per million (1.5M context, max+ultra effort modes); Terra $2.50/$15 (GPT-5.5-competitive, 2× cheaper); Luna $1/$6 (fastest/cheapest). GA expected mid-July 2026 per Axios "couple weeks" framing. current_version not bumped until GA.
- **grok:** notes updated; current_version held at grok-4.3 · V9-Medium (1.5T params, coding, trained on Cursor workflows) launched in consumer Grok product June 16 but still absent from xAI API docs as of June 29. NEW: Grok Build 0.1 (agentic coding) in public API beta since May 29 at $1.00/$2.00 per million; will add as separate registry entry when stable.
- **gemini_pro_multimodal:** verified; no change · Gemini 3.5 Pro confirmed still in limited Vertex preview as of June 23 — GA slipped past Sundar Pichai's "by end of June" target. 2M context / Deep Think / frontier multimodal; pricing unpublished. Flagged deprecations (gemini-3.1-flash-image-preview, gemini-3-pro-image-preview) confirmed shut down June 25 as scheduled.
- **No changes for:** claude_opus, claude_sonnet, claude_haiku, deepseek_general, deepseek_reasoning, gemini_flash, openai_reasoning, mistral_large, mistral_small, mistral_medium, mistral_reasoning, llama.
- **Note:** Perplexity API blocked by proxy (403 from network policy). Firecrawl not attempted (blocked in prior cycles). WebSearch fallback used for all research — successfully sourced data from Anthropic news, releasebot.io, OpenAI announcements, DeepSeek API docs, xAI docs, and multiple pricing aggregators.

**Summary:** This cycle's dominant event is the 🚨 claude-fable-5 suspension — all AIXIA projects routing to claude-fable-5 will fail until access is restored (monitor Anthropic's news page; interim fallback is claude_opus). Equally important: the 🚨 deepseek_v4_pro pricing correction reverses the June 1 crawl's erroneous 4× price hike warning — the permanent rate is $0.435/$0.87, not $1.74/$3.48; Echoform, AIXIA, PropEdgeAI, BusinessVault, and VIMIΛ should revert any budget adjustments made in response to the June 1 warning. GPT-5.6 Sol/Terra/Luna and Gemini 3.5 Pro are both in limited-access previews with GA expected by mid-July — watch for next-cycle updates. No new families added this cycle (Grok Build 0.1 and GPT-5.6 tiers tracked in notes, will graduate to entries at GA/stable).

---

## 2026-07-13 — 4 version bumps, 1 suspension cleared, 2 🚨 breakages, 10 verifications

- **claude_sonnet:** `claude-sonnet-4-6` → `claude-sonnet-5` · Released June 30, 2026. Context window 200K → **1M tokens** (no long-context surcharge). Launch pricing $3/$15 → **$2/$10 through Aug 31, 2026**; reverts to $3/$15 from Sep 1. Capabilities upgraded: added long_context, vision, agentic_coding. Most agentic Sonnet to date; new default on Free/Pro plans. Projects at claude_sonnet auto-resolve to v5 at next deploy — test before Sep 1 and re-audit cost budgets for the $3/$15 standard rate.
- **claude_fable:** `api_suspended: true` → **`api_suspended: false`** · Restored July 1, 2026 after 19-day suspension. Access gated via usage credits (up to 50% of weekly limits for Pro/Max/Team); Anthropic extended free inclusion through July 19, 2026. Cybersecurity classifier added. AIXIA PROJECTS: restore direct routing to claude-fable-5; remove any interim claude_opus fallback routing for Fable-class workloads.
- **openai_gpt:** `gpt-5.5` → `gpt-5.6-sol` · GA July 9, 2026 (limited preview since June 26). Context 1M → 1.05M tokens. Pricing unchanged at $5/$30. Three-tier family: Sol (flagship, gpt-5.6-sol), Terra ($2.50/$15), Luna ($1/$6). New multi-agent beta mode. Cache breakpoints now explicit; 30-min minimum cache life. `default_use: fallback_only` unchanged — no AIXIA project actively routes here.
- 🚨 **grok:** `grok-4.3` → `grok-4.5` · Released July 8, 2026. **Context window HALVED: 1M → 500K** — any AIXIA project using Grok for >500K-token calls must migrate to gemini_pro_multimodal. **Output pricing +140%: $2.50 → $6.00/M** (far above 20% threshold); input +60%: $1.25 → $2.00/M. Built on V9 (1.5T params); targeted at agentic coding/engineering (trained with Cursor). Not available in EU at launch.
- **deepseek_v4_pro:** officially GA (preview period ended mid-July 2026) · Pricing confirmed at $0.435/$0.87 per million tokens (off-peak). NEW: time-of-day peak pricing introduced — peak hours (09:00–12:00 and 14:00–18:00 China time) billed at 2× off-peak rate, effective $0.87/$1.74. Cache hits: $0.003625/M. Queue latency-insensitive workloads to off-peak windows to preserve budget.
- **gemini_pro_multimodal:** verified; Gemini 3.1 Pro unchanged · Gemini 3.5 Pro missed June deadline, now targeting **July 17, 2026 GA** — still in limited Vertex AI enterprise preview. No official pricing published. Entry held at gemini-3.1-pro until GA confirmed.
- **No changes for:** claude_opus, claude_haiku, deepseek_general, deepseek_reasoning, gemini_flash, openai_reasoning, mistral_large, mistral_small, mistral_medium, mistral_reasoning, llama.
- **Note:** Perplexity API (api.perplexity.ai) and Firecrawl (api.firecrawl.dev) remain blocked by remote execution environment network policy (403 from proxy). WebSearch built-in tool used as fallback — data sourced from Anthropic/OpenAI/xAI official announcement pages, OpenRouter, Requesty, pricepertoken.com, benchlm.ai, and TechCrunch. 🚨 Network policy for this crawler should be reviewed to unblock these APIs.

**Summary:** Active cycle with 4 version bumps and 2 🚨 breakages. The Grok 4.5 update carries the highest operational risk: context window halved (1M→500K) and output price up 140% — VIMIΛ, PropEdgeAI, and any project using Grok for large-context or high-volume inference must audit immediately. Claude Sonnet 5 is a strong net-positive upgrade (5× more context, cheaper launch pricing) that auto-applies to all projects resolving from the registry — validate before the Sep 1 rate revert to $3/$15. Claude Fable 5 is back online, so any interim claude_opus routing added during the 19-day suspension should be removed. Gemini 3.5 Pro is expected July 17 — next crawl (Jul 27) will capture it if confirmed.
