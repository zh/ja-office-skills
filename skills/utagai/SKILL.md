---
name: utagai
description: "[EN] Question your own thinking — find holes in a plan/diagnosis. Self-version of /honto. Output matches input language; `lang=en|ja` forces. [JA] 自分の考え/計画/診断を疑って穴を探す。/honto の自己版。出力は入力言語に合わせる (`lang=en|ja` で強制)。ユーザーが /utagai に自分の考えを渡したとき使う。"
user_invocable: true
version: "1.2.0"
---

# utagai: 疑い / Doubt

**Output language MUST match the input language. / 出力は入力言語に厳密に合わせる (英→英、日→日)。**
Terminal output only, no files.

## Step 0 — Decide output language (do this first)

1. Argument starts with `lang=en` or `lang=ja` → use that, strip the token.
2. Otherwise inspect the argument's natural-language words (ignore code, URLs, file paths, proper nouns like "Redmine", "Claude Cowork").
   - English majority → produce the ENTIRE output in English, including section labels.
   - Japanese majority → produce the ENTIRE output in Japanese.
3. Mixed / under ~5 natural-language words → ask once: `Output language? (en/ja)`. Do not guess.

Use exactly one of the templates below — never mix Japanese and English in one response.

## 入力 / Input

- 自分の考え/計画/原因分析 → そのまま / own thought, plan, or diagnosis → pass through
- 引数なし → 「何を疑いますか?」 / no argument → ask "What should I question?"

## 形式 / Format (4 sections, fixed)

**JA template** (use only when output language is Japanese):

1. **多分間違ってる所** — 1〜3個、なぜ怪しいか付き
2. **見落としてる視点** — 別の解釈や考慮されてない要因 1〜3個
3. **自分に聞くべき質問** — 答えると判断が変わる問い 2〜3個
4. **それでも進めるなら** — 最初に確かめるべきこと 一つ

**EN template** (use only when output language is English):

1. **Likely wrong** — 1–3 items, each with why it looks shaky
2. **Missed angles** — 1–3 alternative readings or unconsidered factors
3. **Questions to ask yourself** — 2–3 decision-flipping questions
4. **If proceeding anyway** — one thing to verify first

## ルール / Rules

- 攻撃しない、点検の温度感 / Inspection tone, not attack
- 具体的に / Be concrete — 「楽観的すぎる」ではなく「テスト時間を見てない」 / not "too optimistic" but "didn't budget test time"
- 自信ない指摘は「かもしれません」/ "might" or "may" for low confidence
- 専門用語は日常語に翻訳 / Translate jargon to plain words
- 20行以内 / Under 20 lines
- 見出し `#`、強い断定、ファイル保存は禁止 / No `#` headings, no strong claims, no file saves
