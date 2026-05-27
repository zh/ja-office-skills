---
name: kotae-awase
description: "[EN] Check whether your interpretation of a Japanese message is right. Pass original + interpretation; get match score, nuance, reply direction. Output language matches the interpretation language; `lang=en|ja` forces. [JA] 日本語メッセージへの自分の解釈が合ってるか確認する。原文+解釈を渡すと一致度、ニュアンス、返事の方向を返す。出力は解釈と同じ言語 (`lang=en|ja` で強制)。ユーザーが /kotae-awase に原文と解釈を渡したとき使う。"
user_invocable: true
version: "1.2"
---

# kotae-awase: 答え合わせ / Reading-check

**Output language MUST match the LANGUAGE OF THE INTERPRETATION (not the original). / 出力は「私の解釈」の言語に厳密に合わせる (英解釈→英、日解釈→日)。**
Terminal output only, no files.

## Step 0 — Decide output language (do this first)

1. Argument starts with `lang=en` or `lang=ja` → use that, strip the token.
2. Otherwise inspect the `私の解釈:` / "My interpretation:" block ONLY (ignore the `原文:` block — it is almost always Japanese).
   - Interpretation is English → produce the ENTIRE output in English, including section labels.
   - Interpretation is Japanese → produce the ENTIRE output in Japanese.
3. Interpretation block empty or unparseable → ask once: `Output language? (en/ja)`. Do not guess.

Use exactly one of the templates below — never mix Japanese and English in one response. Short quotes from the original may stay verbatim inside 「鉤括弧」 / "quotes".

## 入力 / Input

```
原文: <Japanese message>
私の解釈: <your reading, in EN or JA>
```

(English alternative form: `Original: …` + `My interpretation: …`.)

引数不足 → 「原文と解釈を両方ください」 / Missing parts → "Please provide both original and interpretation".

## 形式 / Format (4 sections, fixed)

**JA template** (use only when output language is Japanese):

1. **一致度** — ★★★☆☆ (5段階) + 一行
2. **合ってる所 / 抜けてる所** — 短く具体的に、引用付き
3. **ニュアンス** — 温度感・敬語レベル・含み (見落とし注意点)
4. **返事の方向** — 解釈が合ってる/外れてる前提でそれぞれ一文

**EN template** (use only when output language is English):

1. **Match score** — ★★★☆☆ (out of 5) + one-line summary
2. **Aligned / missed** — short concrete points with quoted snippets (originals may stay in 「Japanese」)
3. **Nuance** — temperature, politeness level, implicit meaning (watch-out)
4. **Reply direction** — one line for "interpretation correct" and one for "interpretation off"

## ルール / Rules

- 怖がらせない、コーチの温度感 / Coach tone, not scary
- 引用は短く / Keep quotes short — 「鉤括弧」 in JA, "double quotes" in EN; Japanese originals may stay in 「」 even in EN output
- 自信なしは明記 / Mark uncertainty explicitly
- 25行以内 / Under 25 lines
- 見出し `#`、煽り、ファイル保存は禁止 / No `#` headings, no provocation, no file saves
