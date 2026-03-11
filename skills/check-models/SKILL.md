---
name: check-models
description: Search the web for the latest AI model names and identifiers for a provider, then compare with repository.yaml
user-invocable: true
args:
  - name: provider
    description: "Provider to check: gemini, openai, gpt, claude, grok, or 'all' to check every provider"
---

# Check Latest AI Models for a Provider

You are given a `provider` argument identifying which AI provider's models to look up.

## Provider Mapping

Map the argument to a search scope:

| Argument         | Providers in repository.yaml         | Search terms                                    |
|------------------|--------------------------------------|-------------------------------------------------|
| `gemini`         | `gemini`, `gemini-image`             | Google Gemini latest models API identifiers      |
| `openai` / `gpt` | `gpt`, `gpt-mini`, `openai-image`   | OpenAI latest models API model identifiers       |
| `claude`         | `claude`                             | Anthropic Claude latest models API identifiers   |
| `grok`           | `grok`, `xai-image`                  | xAI Grok latest models API identifiers           |
| `all`            | all of the above                     | run each provider in sequence                    |

If the argument doesn't match any of the above, tell the user the valid options and stop.

## Steps

1. **Read `repository.yaml`** from the repository root. Parse and display the current model entries for the matched provider(s). Show the `identifier` and `thinking_identifier` values.

2. **Search the web** for the latest available models from that provider. Use WebSearch with targeted queries:
   - Focus on the provider's **official API documentation** or **model listing pages**
   - Use queries like: `site:ai.google.dev Gemini models list`, `site:platform.openai.com models`, `site:docs.anthropic.com models`, `site:docs.x.ai models`
   - Also try a general query: `<provider> latest AI models 2026 API identifier`

3. **Fetch the relevant documentation page(s)** using WebFetch to get the actual model identifiers (not just names). Look for:
   - Model ID strings (e.g., `gemini-2.5-pro`, `gpt-4o-2024-11-20`, `claude-sonnet-4-5-20250514`)
   - Stable vs preview/experimental distinctions
   - Which models are current/recommended vs deprecated

4. **Compare** the fetched model identifiers against what's in `repository.yaml`. For each model entry, determine:
   - Is the current identifier still valid/available?
   - Is there a newer version or successor?
   - Are there new models worth adding?

5. **Present a summary table** to the user:

   ```
   Provider: <name>

   Entry              Current Identifier                  Latest Available                   Status
   ─────              ──────────────────                  ────────────────                   ──────
   gemini             google-gla:gemini-2.5-flash         google-gla:gemini-2.5-flash        ✓ up to date
   gemini (thinking)  google-gla:gemini-2.5-pro           google-gla:gemini-3.0-pro          ⚠ update available
   ```

   Use these status indicators:
   - `✓ up to date` — current identifier is the latest
   - `⚠ update available` — a newer model exists
   - `✗ deprecated` — current identifier is deprecated or removed
   - `? unknown` — couldn't determine status

6. **If updates are available**, show the exact YAML changes needed for `repository.yaml` as a diff, but **do NOT apply them automatically**. Ask the user whether to apply the changes.

## Important Notes

- Model identifiers in `repository.yaml` use a `provider:model-id` format (e.g., `anthropic:claude-sonnet-4-5`). The prefix before `:` is the provider routing key — preserve it when suggesting updates.
- Image model entries (like `gemini-image`, `openai-image`) may not have a provider prefix — check the existing format and match it.
- When uncertain about whether a model is newer or just different, flag it as `? check manually` rather than recommending a change.
- Prefer stable/GA models over preview/experimental ones, unless the current config already uses a preview model.
