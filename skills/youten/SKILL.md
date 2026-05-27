---
name: youten
description: "[EN] Compress a document (PDF / image / long text / URL) into 3 key points. Output matches input language; `lang=en|ja` forces. [JA] 資料 (PDF/画像/長文/URL) の要点を3項目に圧縮。出力は入力言語に合わせる (`lang=en|ja` で強制)。ユーザーが /youten にパスや文章を渡したとき使う。"
user_invocable: true
version: "1.2.0"
---

# youten: 要点 / Key points

**Output language MUST match the input language. / 出力は入力言語に厳密に合わせる (英→英、日→日)。**
Terminal output only, no files.

## Step 0 — Decide output language (do this first)

1. Argument starts with `lang=en` or `lang=ja` → use that, strip the token.
2. Otherwise inspect the source document's natural-language words (ignore code, URLs, file paths, proper nouns). For a path/URL, fetch first then detect on the fetched text.
   - English majority → produce the ENTIRE output in English, including section labels and quotes.
   - Japanese majority → produce the ENTIRE output in Japanese.
3. Mixed / very short → ask once: `Output language? (en/ja)`. Do not guess.

Use exactly one of the templates below — never mix Japanese and English in one response.

## 入力 / Input

- パス (.pdf/.png/.jpg/.md/.txt) → Read で読む
- URL → WebFetch
- 文章貼り付け → そのまま / pasted text → pass through
- 引数なし → 「何の要点ですか?」 / no argument → ask "Key points of what?"

## 形式 / Format (3 sections, fixed)

**JA template** (use only when output language is Japanese):

1. **これは何** — 一行
2. **中身 / 決まったこと** — 1〜3項目、短い引用付き
3. **次の動き / 未決** — 1〜3項目

**EN template** (use only when output language is English):

1. **What this is** — one line
2. **Contents / decisions** — 1–3 items with short quoted snippets
3. **Next moves / open items** — 1–3 items

## ルール / Rules

- 専門用語は日常語に翻訳 / Translate jargon to plain words ("cache" → "stash" / "ためておく")
- 引用は短く「鉤括弧」/ "short" / Keep quotes short — 「鉤括弧」 in JA output, "double quotes" in EN
- 自信なしは「だいたい」「可能性が高い」と明記 / Mark uncertainty with "roughly" / "likely"
- 15行以内 / Under 15 lines
- 見出し `#`、メタ発言、丸ごとコピー、ファイル保存は禁止 / No `#` headings, meta talk, whole-copy, or file saves
