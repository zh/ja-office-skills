---
name: ja
description: "[EN] Translate the previous message or argument to Japanese. Auto-detects source language; preserves code/URLs/mentions/proper nouns; matches source register (default です・ます). [JA] 直前のメッセージまたは引数を日本語に翻訳。元の言語を自動検出し、コード/URL/メンション/固有名詞はそのまま。元の口調に合わせる (中立なら です・ます)。ユーザーが /ja を叩いたとき使う。"
user_invocable: true
version: "1.2"
---

# ja: 日本語に翻訳 / Translate to Japanese

**Pure translation. Real-time conversation speed. / 純粋な翻訳。リアルタイム会話の速度。**
Terminal output only, no files. Length and structure match the source.

## Input

- Argument given → translate the argument.
- No argument → translate the most recent message in the conversation (assistant or user), excluding this `/ja` invocation itself.
- No argument and no previous message → ask once: `Translate what?`

## Behavior

1. **Detect source language.** If already Japanese, output one line and stop:
   `Already in Japanese — did you mean /en?`
2. **Mixed-language input.** Translate only the non-Japanese portions; leave existing Japanese as-is. Code, URLs, @mentions, file paths, and proper nouns stay verbatim.
3. **Register.** Match the source:
   - Casual source (slang, "lol", emoji-only context) → casual JA.
   - Neutral / register-ambiguous source → です・ます.
   - Strong formal markers (Mr./Ms., Dear, signature blocks, 拝啓 etc.) → 敬語.
4. **Structure.** Bullets → bullets. Code blocks verbatim. Markdown preserved.
5. **Optional `Note:` line.** At the end, only if an idiom genuinely loses meaning in JA. Maximum one note. No chitchat.

## Rules

- Output the translation as-is. No preamble ("Translation:", "翻訳:", "Here you go").
- No `#` headings, no meta-commentary, no file saves.
- Don't katakana-ize Latin-script names — "John Smith" stays "John Smith".
- Don't over-formalize. Match what the source actually conveys.
