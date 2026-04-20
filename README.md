# aixia-model-registry

**Shared AI model registry for JayKay's AIXIA-workspace projects.**

Single source of truth that every project reads at runtime to resolve model **family** (e.g., `claude_sonnet`, `deepseek_general`, `gemini_pro_multimodal`) → **current version** (e.g., `claude-sonnet-4-6`). Never hardcode model version strings in project code — always resolve from this registry so projects auto-stay-current as major labs ship new releases.

## How to read the registry from a project

```ts
// TypeScript example
const REGISTRY_URL = "https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json";

async function resolveModel(family: string): Promise<string> {
  const res = await fetch(REGISTRY_URL, { cache: "force-cache" });
  const reg = await res.json();
  return reg.families[family].current_version;
}

// Usage
const model = await resolveModel("claude_sonnet"); // → "claude-sonnet-4-6"
const client = new Anthropic();
client.messages.create({ model, ... });
```

```python
# Python example
import requests
from functools import lru_cache

REGISTRY_URL = "https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json"

@lru_cache(maxsize=1)
def registry():
    return requests.get(REGISTRY_URL).json()

def resolve(family: str) -> str:
    return registry()["families"][family]["current_version"]

# Usage
model = resolve("claude_sonnet")
```

Cache the registry aggressively (5-15 min TTL in production). The bi-weekly crawler is the only thing that writes to this repo, so reads are safe to cache.

## Files

- **`model-registry.json`** — the authoritative JSON registry. Schema described in its `_description` field.
- **`model-changelog.md`** — append-only change log. Every crawler run adds a dated entry.

## Maintenance

A **bi-weekly scheduled Claude remote-agent trigger** runs every other Monday at 9 AM Pacific. It:

1. Queries Perplexity for new releases across major vendors (Anthropic, OpenAI, DeepSeek, Google/Gemini, Meta, Mistral, xAI) in the past 14 days
2. Firecrawls each vendor's official pricing/model pages
3. Diffs against the current registry
4. Updates `current_version`, `pricing_per_million_usd`, `context_window`, `capabilities`, `last_verified` per family
5. Commits + pushes the updated JSON + appends a changelog entry
6. Surfaces a short summary in the next Claude Code session via the auto-continuity hook

Never commit directly to this repo unless you know what you're doing — the crawler owns it.

## Project convention (per `~/.claude/CLAUDE.md` §9.5)

- **Never hardcode** `"claude-sonnet-4-6"` or `"gemini-3.1-pro"` etc. in any project code, config, or docs.
- **Reference families** (e.g., `"latest Claude Sonnet"`) and resolve from this registry at call-time.
- **Exception:** version-pinned regression/eval tests may pin a specific version with a clear comment explaining why.

## Privacy Mode routing

The registry defines a `privacy_mode_routing` block that routes all calls through Claude only — no DeepSeek / third-party-jurisdiction path. Echoform's Studio tier defaults to Privacy Mode; other projects opt in.
