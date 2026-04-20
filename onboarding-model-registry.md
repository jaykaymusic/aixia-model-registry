# AI Model Registry — Universal Onboarding Snippet

**Drop this paragraph into any new project** (README, system prompt for Cowork / Cursor / Windsurf / bolt.new / Lovable / Antigravity / ChatGPT / etc.) so the project auto-stays-current on model versions without manual upkeep.

---

## The snippet (paste-ready)

> **Model version policy:** this project uses the AIXIA model registry at `https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json` (public, no auth). **Never hardcode model version strings.** Read the registry at runtime and resolve `families.<family>.current_version`. Cache the registry 10 minutes. Fall back to env var `<FAMILY>_FALLBACK` (e.g. `CLAUDE_SONNET_FALLBACK`) if the registry is unreachable. Canonical family keys: `claude_opus`, `claude_sonnet`, `claude_haiku`, `deepseek_general`, `deepseek_reasoning`, `gemini_pro_multimodal`, `gemini_flash`, `openai_gpt`, `openai_reasoning`, `mistral_large`, `llama`, `grok`. The registry auto-updates every 2 weeks via a scheduled crawler — just read it.

---

## Drop-in helpers by language

### Node.js / TypeScript (ESM)

```ts
// lib/models.ts
const REGISTRY_URL = "https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json";
const CACHE_TTL_MS = 10 * 60 * 1000;
const FALLBACKS: Record<string, string> = {
  claude_opus: "claude-opus-4-7",
  claude_sonnet: "claude-sonnet-4-6",
  claude_haiku: "claude-haiku-4-5-20251001",
  gemini_pro_multimodal: "gemini-3.1-pro",
  gemini_flash: "gemini-3.1-flash",
  deepseek_general: "deepseek-v3",
  deepseek_reasoning: "deepseek-r1",
};
let _cache: any = null;
let _cacheExpiry = 0;

export async function resolveModel(family: string): Promise<string> {
  if (_cache && Date.now() < _cacheExpiry) {
    const v = _cache.families?.[family]?.current_version;
    if (v) return v;
  }
  try {
    const controller = new AbortController();
    const t = setTimeout(() => controller.abort(), 5000);
    const res = await fetch(REGISTRY_URL, { signal: controller.signal });
    clearTimeout(t);
    if (!res.ok) throw new Error(`Registry HTTP ${res.status}`);
    _cache = await res.json();
    _cacheExpiry = Date.now() + CACHE_TTL_MS;
    const v = _cache.families?.[family]?.current_version;
    if (!v) throw new Error(`Family "${family}" not in registry`);
    return v;
  } catch (err) {
    const fb = FALLBACKS[family];
    if (!fb) throw new Error(`No fallback for "${family}" and registry unreachable`);
    console.warn(`[models] registry unreachable, using fallback for ${family}: ${fb}`);
    return fb;
  }
}
```

### Python

```python
# models.py
import time, urllib.request, json
from typing import Optional

REGISTRY_URL = "https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json"
CACHE_TTL_S = 10 * 60
FALLBACKS = {
    "claude_opus": "claude-opus-4-7",
    "claude_sonnet": "claude-sonnet-4-6",
    "claude_haiku": "claude-haiku-4-5-20251001",
    "gemini_pro_multimodal": "gemini-3.1-pro",
    "gemini_flash": "gemini-3.1-flash",
    "deepseek_general": "deepseek-v3",
    "deepseek_reasoning": "deepseek-r1",
}
_cache: Optional[dict] = None
_cache_expiry: float = 0

def resolve_model(family: str) -> str:
    global _cache, _cache_expiry
    now = time.time()
    if _cache and now < _cache_expiry:
        fam = _cache.get("families", {}).get(family)
        if fam and fam.get("current_version"):
            return fam["current_version"]
    try:
        with urllib.request.urlopen(REGISTRY_URL, timeout=5) as r:
            _cache = json.loads(r.read().decode())
            _cache_expiry = now + CACHE_TTL_S
        fam = _cache.get("families", {}).get(family)
        if not fam:
            raise RuntimeError(f'Family "{family}" missing from registry')
        return fam["current_version"]
    except Exception as e:
        fb = FALLBACKS.get(family)
        if not fb:
            raise RuntimeError(f'No fallback for "{family}" and registry unreachable: {e}')
        print(f"[models] registry unreachable, using fallback for {family}: {fb}")
        return fb
```

### Bash / Shell

```bash
REGISTRY_URL="https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json"
resolve_model() {
  local family="$1"
  local fallback="$2"
  local v=$(curl -sS --max-time 3 "$REGISTRY_URL" 2>/dev/null | jq -r ".families.\"$family\".current_version // empty" 2>/dev/null)
  if [ -n "$v" ] && [ "$v" != "null" ]; then echo "$v"; else echo "$fallback"; fi
}
# Usage:
MODEL=$(resolve_model "claude_sonnet" "claude-sonnet-4-6")
```

### Dart / Flutter

```dart
// services/model_resolver.dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class ModelResolver {
  static const _registryUrl = 'https://raw.githubusercontent.com/jaykaymusic/aixia-model-registry/main/model-registry.json';
  static const _cacheTtl = Duration(minutes: 10);
  static const _fallbacks = {
    'claude_opus': 'claude-opus-4-7',
    'claude_sonnet': 'claude-sonnet-4-6',
    'claude_haiku': 'claude-haiku-4-5-20251001',
    'gemini_pro_multimodal': 'gemini-3.1-pro',
    'gemini_flash': 'gemini-3.1-flash',
    'deepseek_general': 'deepseek-v3',
  };
  static Map<String, dynamic>? _cache;
  static DateTime? _cacheExpiry;

  static Future<String> resolve(String family) async {
    if (_cache != null && _cacheExpiry != null && DateTime.now().isBefore(_cacheExpiry!)) {
      final v = _cache!['families']?[family]?['current_version'];
      if (v is String) return v;
    }
    try {
      final res = await http.get(Uri.parse(_registryUrl)).timeout(const Duration(seconds: 5));
      if (res.statusCode != 200) throw Exception('HTTP ${res.statusCode}');
      _cache = jsonDecode(res.body);
      _cacheExpiry = DateTime.now().add(_cacheTtl);
      final v = _cache!['families']?[family]?['current_version'];
      if (v is String) return v;
      throw Exception('Family "$family" missing from registry');
    } catch (e) {
      final fb = _fallbacks[family];
      if (fb == null) throw Exception('No fallback for "$family" and registry unreachable: $e');
      return fb;
    }
  }
}
```

---

## What NOT to do

- ❌ `const model = "claude-sonnet-4-6"` — hardcoded; will go stale
- ❌ `ANTHROPIC_MODEL=claude-sonnet-4-6` in .env — env-var pins defeat auto-update
- ❌ Trusting LLM-generated code that picks a model string from training data — always wrap with a registry resolve call
- ❌ Reading the registry on every LLM call — cache 10 min minimum

## What TO do

- ✅ Resolve family name to version via `resolveModel(family)` once at startup or on first call, cache in memory
- ✅ Let env var `<FAMILY>_FALLBACK` override registry only when registry is unreachable (safety net)
- ✅ Honor user-explicit model pins (e.g., `ANTHROPIC_MODEL`) at top priority — users pinning a specific version is intentional

## Maintenance

A **bi-weekly remote Claude agent** runs every other Monday at 9 AM Pacific, crawls Perplexity + each vendor's pricing page, and commits registry updates automatically. Projects get the new values on their next cache-refresh (≤10 minutes after the crawler finishes).
