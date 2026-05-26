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
| `/youten` | Summarize hard docs (PDF/image/long/URL) into 3 items | bilingual (input → matching output) |
| `/nukemore` | Ticket/PR/spec gap finder (`/grill-me` for documents) | bilingual |
| `/yougo` | Explain a Japanese business term in context (検収, 稟議, 工数...) | JP body + EN gloss on examples |
| `/maemuki` | Pre-commitment check before saying yes — hidden costs, assumptions, what to confirm | bilingual |
| `/utagai` | Self-grill on your own thinking (`/honto` for your own plan) | bilingual |
| `/kotae-awase` | Verify your interpretation of a Japanese message — match score, missed nuance, reply direction | output matches interpretation language |

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

## Versioning

All 9 skills ship under a single plugin version. Current: **1.1.0**.

## License

MIT
