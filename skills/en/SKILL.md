---
name: en
description: "[EN] Translate the previous message or argument to English. Auto-detects source language; preserves code/URLs/mentions/proper nouns; matches source register. [JA] 直前のメッセージまたは引数を英語に翻訳。元の言語を自動検出し、コード/URL/メンション/固有名詞はそのまま。元の口調に合わせる。ユーザーが /en を叩いたとき使う。"
user_invocable: true
version: "1.2"
---

# en: Translate to English

**Pure translation. Real-time conversation speed. / 純粋な翻訳。リアルタイム会話の速度。**
Terminal output only, no files. Length and structure match the source.

## Input

- Argument given → translate the argument.
- No argument → translate the most recent message in the conversation (assistant or user), excluding this `/en` invocation itself.
- No argument and no previous message → ask once: `Translate what?`

## Behavior

1. **Detect source language.** If already English, output one line and stop:
   `Already in English — did you mean /ja?`
2. **Mixed-language input.** Translate only the non-English portions; leave existing English as-is. Code, URLs, @mentions, file paths, and proper nouns stay verbatim.
3. **Register.** Match the source:
   - JA casual / slang → casual EN ("let me know", "thanks").
   - JA です・ます → neutral business EN ("please let me know").
   - JA 敬語 / strong formal markers → polite business EN. Modern business English is the ceiling — don't go Victorian.
   - Other source languages (BG, DE, etc.) — match register similarly.
4. **Names.** Romaji when standard (田中 → Tanaka). Otherwise keep the source form.
5. **Structure.** Bullets → bullets. Code blocks verbatim. Markdown preserved.
6. **Optional `Note:` line.** At the end, only if an idiom genuinely loses meaning in EN. Maximum one note. No chitchat.

## Rules

- Output the translation as-is. No preamble ("Translation:", "Here you go").
- No `#` headings, no meta-commentary, no file saves.
- Don't over-formalize. Match what the source actually conveys.
