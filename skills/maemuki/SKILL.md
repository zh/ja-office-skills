---
name: maemuki
description: "[EN] Surface hidden costs, implicit assumptions, and clarifying questions before saying yes to a request. Output matches input language; `lang=en|ja` forces. [JA] 依頼に「はい」と返す前に隠れたコスト、暗黙の前提、確認すべき点を洗い出す。出力は入力言語に合わせる (`lang=en|ja` で強制)。ユーザーが /maemuki に依頼内容を渡したとき使う。"
user_invocable: true
version: "1.2"
---

# maemuki: 前向き / Forward-leaning

**Output language MUST match the input language. / 出力は入力言語に厳密に合わせる (英→英、日→日)。**
Terminal output only, no files.

## Step 0 — Decide output language (do this first)

1. Argument starts with `lang=en` or `lang=ja` → use that, strip the token.
2. Otherwise inspect the request text's natural-language words (ignore code, URLs, file paths, proper nouns).
   - English majority → produce the ENTIRE output in English, including section labels.
   - Japanese majority → produce the ENTIRE output in Japanese.
3. Mixed / under ~5 natural-language words → ask once: `Output language? (en/ja)`. Do not guess.

Use exactly one of the templates below — never mix Japanese and English in one response.

## 入力 / Input

- 依頼文 (メール/Slack/口頭メモ) → そのまま / request text (mail/Slack/notes) → pass through
- パス/URL → Read/WebFetch
- 引数なし → 「何の依頼ですか?」 / no argument → ask "What's the request?"

## 形式 / Format (4 sections, fixed)

**JA template** (use only when output language is Japanese):

1. **依頼の本体** — 一文で要約
2. **隠れたコスト** — 言われてないが必要な工数/影響 1〜3個 (具体的に)
3. **暗黙の前提** — 相手が当然と思ってそうな前提 1〜3個
4. **返事の前に確認すること** — 具体的な質問 2〜4個

**EN template** (use only when output language is English):

1. **The request** — one-line summary
2. **Hidden costs** — 1–3 unspoken but real work/impact items (concrete)
3. **Implicit assumptions** — 1–3 things the asker seems to take for granted
4. **Confirm before replying** — 2–4 concrete questions

## ルール / Rules

- 「断れ」と判断しない — 判断材料を出すだけ / Don't say "refuse" — only surface decision material
- コストは具体的 / Costs concrete — 「時間かかる」ではなく「テスト書き直し2日」 / not "takes time" but "2 days to rewrite tests"
- 質問は答えやすい形 / Questions easy to answer
- 専門用語は日常語に翻訳 / Translate jargon to plain words
- 20行以内 / Under 20 lines
- 見出し `#`、メタ発言、ファイル保存は禁止 / No `#` headings, no meta talk, no file saves
