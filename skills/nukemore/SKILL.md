---
name: nukemore
description: "[EN] Surface gaps, implicit assumptions, and clarifying questions in a ticket / PR / spec before starting work. Document-form of /grill-me. Output matches input language; `lang=en|ja` forces. [JA] チケット/PR/仕様の抜け漏れ、暗黙の前提、聞くべき質問を洗い出す。/grill-me の文書版。出力は入力言語に合わせる (`lang=en|ja` で強制)。ユーザーが /nukemore にパスや文章を渡したとき使う。"
user_invocable: true
version: "1.2"
---

# nukemore: 抜け漏れ / Gaps

**Output language MUST match the input language. / 出力は入力言語に厳密に合わせる (英→英、日→日)。**
Terminal output only, no files.

## Step 0 — Decide output language (do this first)

1. Argument starts with `lang=en` or `lang=ja` → use that, strip the token.
2. Otherwise inspect the source document's natural-language words (ignore code, URLs, file paths, proper nouns). For a path/URL, fetch first then detect on the fetched text.
   - English majority → produce the ENTIRE output in English, including section labels.
   - Japanese majority → produce the ENTIRE output in Japanese.
3. Mixed / very short → ask once: `Output language? (en/ja)`. Do not guess.

Use exactly one of the templates below — never mix Japanese and English in one response.

## 入力 / Input

- パス → Read
- URL (Redmine/GitHub) → WebFetch
- 文章 → そのまま / inline text → pass through
- 引数なし → 「何を点検しますか?」 / no argument → ask "What should I inspect?"

## 形式 / Format (4 sections, fixed)

**JA template** (use only when output language is Japanese):

1. **暗黙の前提** — 書かれてないが当然視されていること 1〜3個
2. **足りない情報** — 着手に必要だが欠けているもの 1〜3個
3. **聞くべき質問** — 着手前に確認する具体的な問い 3〜5個 (Yes/No か選択肢付き)
4. **危ない箇所** — 仕様の曖昧さ・矛盾・無理筋 0〜3個

**EN template** (use only when output language is English):

1. **Implicit assumptions** — 1–3 things treated as obvious but not written
2. **Missing information** — 1–3 things needed to start but not present
3. **Questions to ask** — 3–5 concrete pre-start questions (Yes/No or with options)
4. **Risky spots** — 0–3 ambiguities, contradictions, or stretches in the spec

## ルール / Rules

- 具体的に / Be concrete — 「曖昧」ではなく「『早く』が日数か時間か不明」 / not "vague" but "'soon' — days or hours?"
- 重要度順 / Order by importance
- 専門用語は日常語に翻訳 / Translate jargon to plain words
- 25行以内 / Under 25 lines
- 見出し `#`、メタ発言、丸ごとコピー、ファイル保存は禁止 / No `#` headings, meta talk, whole-copy, or file saves
