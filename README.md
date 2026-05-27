# ja-office-skills

9 friendly-Japanese cognitive-aid skills for office life. Zero jargon, terminal output, no file saves, fixed format ≤25 lines.

Designed for engineers and non-engineer office colleagues at Japanese companies — and especially helpful for non-native Japanese speakers who need a scaffold to read, verify, and decide faster.

## Skills

### Cognitive trio (JP only)

| Command | Purpose |
|--|--|
| `/kantan` | Explain a technical term to a non-engineer colleague |
| `/honto` | Sanity-check an AI answer (★1–5, sketchy bits, how to verify, another angle) |
| `/bunkai` | Decompose a topic into 3–7 parts/steps + connection + one-liner |

### Office-life six (mostly bilingual)

| Command | Purpose | Language |
|--|--|--|
| `/youten` | Summarize hard docs (PDF/image/long/URL) into 3 items | bilingual (auto-detect, `lang=en\|ja` override) |
| `/nukemore` | Ticket/PR/spec gap finder (`/grill-me` for documents) | bilingual (auto-detect, `lang=en\|ja` override) |
| `/yougo` | Explain a Japanese business term in context (検収, 稟議, 工数...) | JP body + EN gloss on examples |
| `/maemuki` | Pre-commitment check before saying yes — hidden costs, assumptions, what to confirm | bilingual (auto-detect, `lang=en\|ja` override) |
| `/utagai` | Self-grill on your own thinking (`/honto` for your own plan) | bilingual (auto-detect, `lang=en\|ja` override) |
| `/kotae-awase` | Verify your interpretation of a Japanese message — match score, missed nuance, reply direction | matches **interpretation** language (auto-detect, `lang=en\|ja` override) |

## Output language (bilingual skills)

The five bilingual skills (`/youten`, `/nukemore`, `/maemuki`, `/utagai`, `/kotae-awase`) enforce a strict language rule as of **1.2.0**:

1. **Explicit override wins.** If your argument starts with `lang=en` or `lang=ja`, that language is used and the token is stripped.

   ```
   /utagai lang=en Redmine MCP サーバーを作りたい   → English output
   /utagai lang=ja i want to create a Redmine MCP   → Japanese output
   ```

2. **Otherwise auto-detect.** The skill inspects the natural-language words of the argument (ignoring code, URLs, file paths, and proper nouns like "Redmine" or "Claude Cowork"):
   - English majority → entire output in English (English section labels too)
   - Japanese majority → entire output in Japanese

3. **Ambiguous → asks.** Mixed-language or very short (< ~5 natural-language words) input prompts the skill to ask `Output language? (en/ja)` rather than guess.

4. **`/kotae-awase` variant.** Language is detected from the `私の解釈:` (My interpretation:) block, not the `原文:` (original) block — the original is almost always Japanese.

Languages are never mixed in one response: the entire structure (section labels, body, examples) is in a single language.

## Install

### Claude Code (CLI)

```bash
/plugin marketplace add zh/ja-office-skills
/plugin install ja-office-skills
```

Or clone directly:

```bash
git clone https://github.com/zh/ja-office-skills ~/.claude/plugins/ja-office-skills
```

### Claude Cowork

1. Download the latest release ZIP from [Releases](https://github.com/zh/ja-office-skills/releases).
2. Open Claude Desktop → **Cowork** → **Customize** → **Browse plugins** → upload the ZIP.
3. Skills appear in the slash-command palette for everyone on the team.

For org-wide deployment, admins can connect the GitHub repo or upload the ZIP at **Organization → Plugin Marketplace**.

### claude.ai (web)

1. Download the latest release ZIP from [Releases](https://github.com/zh/ja-office-skills/releases).
2. Go to **Settings → Features → Skills → Upload custom skill**.
3. Upload one skill folder at a time (or the full bundle, if supported in your plan).

Requires Pro / Max / Team / Enterprise with code execution enabled.

## Upgrading from earlier versions

### From 1.1.0 → 1.2.0

1.2.0 changes only the five bilingual skills (`utagai`, `maemuki`, `nukemore`, `youten`, `kotae-awase`). The other four (`kantan`, `honto`, `bunkai`, `yougo`) are unchanged.

**What changed:**
- A mandatory `Step 0` language decision was added to each bilingual skill, with auto-detect plus a `lang=en|ja` override.
- The format section now contains **both** a Japanese template and an English template, with matching section labels — fixing the previous behavior where English input often produced Japanese output because no English template existed.
- The opening rule is now a bold bilingual sentence on line 10.
- The YAML `description` is now bilingual (`[EN] … [JA] …`) so skill discovery doesn't bias toward Japanese.

**Behavioral differences you may notice:**
- English-only input now reliably produces English-only output (this previously failed silently).
- Very short or mixed-language input now triggers a `Output language? (en/ja)` prompt instead of defaulting to Japanese.
- You can force a language with `lang=en` / `lang=ja` as the first token (e.g. `/utagai lang=en my plan…`). The token is consumed before processing.
- No change to argument formats, output sections, or line limits.

**How to upgrade (Claude Code):**

```bash
/plugin update ja-office-skills
```

Or, if `update` is not available:

```bash
/plugin uninstall ja-office-skills
/plugin install ja-office-skills
```

Verify the cache is on the new version:

```bash
ls ~/.claude/plugins/cache/ja-office-skills/ja-office-skills/
cat ~/.claude/plugins/cache/ja-office-skills/ja-office-skills/1.2.*/.claude-plugin/plugin.json
```

(The new directory will be `1.2.x`; the old `1.1.0` directory can be removed once you've confirmed the new version loads.)

**How to upgrade (Claude Cowork / claude.ai web):**
Remove the existing plugin/skill and upload the 1.2.0 release ZIP.

**Rollback to 1.1.0:** install at the pinned older version (`/plugin install ja-office-skills@1.1.0` if your marketplace supports tags) or re-upload the older release ZIP. No data migration; the change is purely instructional.

## Versioning

All 9 skills ship under a single plugin version. Current series: **1.2.x** — see [Releases](https://github.com/zh/ja-office-skills/releases) or `.claude-plugin/plugin.json` for the exact pinned version.

## License

MIT
