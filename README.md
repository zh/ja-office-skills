# ja-office-skills

11 friendly-Japanese cognitive-aid skills for office life. Zero jargon, terminal output, no file saves, fixed format ≤25 lines.

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

### Translation pair (any → JA/EN)

| Command | Purpose |
|--|--|
| `/ja` | Translate the previous message (or argument) to Japanese. Auto-detects source language. Matches source register (default です・ます). |
| `/en` | Translate the previous message (or argument) to English. Auto-detects source language. Matches source register. |

Designed for multilingual offices. `/ja` with no argument translates the most recent message in the conversation — useful right after `/kantan` or any other skill that produced JA output, when you want a quick English (or other-language) version, and vice versa. Code, URLs, @mentions, and proper nouns pass through verbatim. Refuses politely if the source is already in the target language.

## Output language (bilingual skills)

The five bilingual skills (`/youten`, `/nukemore`, `/maemuki`, `/utagai`, `/kotae-awase`) enforce a strict language rule as of **1.2.x**:

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

### From 1.2.x → 1.3.0

Adds two new skills: `/ja` and `/en` — quick translators between any source language and Japanese/English. No changes to the existing 9 skills.

**Behavioral highlights:**
- With no argument, translates the most recent message in the conversation (assistant or user), so it chains naturally after another skill (e.g. `/kantan` → `/en`).
- Pure translation, length and structure match the source. One optional `Note:` line at the end if an idiom genuinely loses meaning.
- Auto-detects the source language. Refuses with a one-line hint if the source is already in the target language (`Already in Japanese — did you mean /en?`).
- No `lang=` override on these two skills — the command itself specifies the target language.

### From 1.1.x → 1.2.x

The 1.2.x series changes only the five bilingual skills (`utagai`, `maemuki`, `nukemore`, `youten`, `kotae-awase`). The other four (`kantan`, `honto`, `bunkai`, `yougo`) are unchanged.

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

(The new directory will be `1.2.x`; the old `1.1.x` directory can be removed once you've confirmed the new version loads.)

**How to upgrade (Claude Cowork / claude.ai web):**
Remove the existing plugin/skill and upload the latest 1.2.x release ZIP.

**Rollback to 1.1.x:** install at a pinned older version (`/plugin install ja-office-skills@1.1.x` if your marketplace supports tags) or re-upload an older release ZIP. No data migration; the change is purely instructional.

## Versioning

All 11 skills ship under a single plugin version. Current series: **1.3.x** — see [Releases](https://github.com/zh/ja-office-skills/releases) or `.claude-plugin/plugin.json` for the exact pinned version.

## License

MIT
