# ja-office-skills

[English](README.md) | **日本語**

オフィスワークを助ける、フレンドリーな日本語の「考えるための」スキル 11 個。専門用語ゼロ、ターミナル出力、ファイル保存なし、固定フォーマット 25 行以内。

日本企業で働くエンジニアと非エンジニアの同僚のために設計しました。特に、日本語が母語でない方が「読む・確かめる・決める」を速くこなすための足場として役立ちます。

## スキル一覧

### 考えるための 3 点セット (日本語のみ)

| コマンド | 用途 |
|--|--|
| `/kantan` | 技術用語を非エンジニアの同僚向けに説明する |
| `/honto` | AI の答えをチェックする (★1〜5、怪しいところ、確かめ方、別の見方) |
| `/bunkai` | 話題を 3〜7 個の部品/手順に分解 + つながり + ひとこと要約 |

### オフィスワーク 6 点セット (ほぼバイリンガル)

| コマンド | 用途 | 言語 |
|--|--|--|
| `/youten` | 難しい資料 (PDF/画像/長文/URL) を 3 項目に要約 | バイリンガル (自動判定、`lang=en\|ja` で強制) |
| `/nukemore` | チケット/PR/仕様の抜け漏れ探し (文書版 `/grill-me`) | バイリンガル (自動判定、`lang=en\|ja` で強制) |
| `/yougo` | 日本のビジネス用語を文脈付きで説明 (検収、稟議、工数...) | 本文は日本語 + 例文に英訳付き |
| `/maemuki` | 「はい」と言う前の確認 — 隠れたコスト、前提、確認すべき点 | バイリンガル (自動判定、`lang=en\|ja` で強制) |
| `/utagai` | 自分の考えを自分で疑う (自分の計画に対する `/honto`) | バイリンガル (自動判定、`lang=en\|ja` で強制) |
| `/kotae-awase` | 日本語メッセージの解釈が合っているか確認 — 一致度、見落としたニュアンス、返信の方向性 | **解釈**の言語に合わせる (自動判定、`lang=en\|ja` で強制) |

### 翻訳ペア (どの言語からでも → 日本語/英語)

| コマンド | 用途 |
|--|--|
| `/ja` | 直前のメッセージ (または引数) を日本語に翻訳。元の言語は自動判定。元の口調に合わせる (中立なら です・ます)。 |
| `/en` | 直前のメッセージ (または引数) を英語に翻訳。元の言語は自動判定。元の口調に合わせる。 |

多言語オフィス向けの設計です。引数なしの `/ja` は会話の直前のメッセージを翻訳します — `/kantan` などで日本語出力が出た直後に、サッと英語版 (や他言語版) が欲しいとき、またその逆にも便利です。コード、URL、@メンション、固有名詞はそのまま通します。元の文章がすでに翻訳先の言語なら、丁寧にお断りします。

## 出力言語 (バイリンガルスキル)

バイリンガルの 5 スキル (`/youten`、`/nukemore`、`/maemuki`、`/utagai`、`/kotae-awase`) は、**1.2.x** から厳密な言語ルールに従います:

1. **明示的な指定が最優先。** 引数が `lang=en` または `lang=ja` で始まる場合、その言語を使い、トークンは取り除かれます。

   ```
   /utagai lang=en Redmine MCP サーバーを作りたい   → 英語で出力
   /utagai lang=ja i want to create a Redmine MCP   → 日本語で出力
   ```

2. **指定がなければ自動判定。** 引数の自然言語の単語を見て判定します (コード、URL、ファイルパス、「Redmine」「Claude Cowork」などの固有名詞は無視):
   - 英語が過半数 → 出力全体が英語 (セクションラベルも英語)
   - 日本語が過半数 → 出力全体が日本語

3. **曖昧なら質問。** 言語が混ざっていたり、とても短い (自然言語の単語が 5 個未満程度) 場合は、推測せずに `Output language? (en/ja)` と質問します。

4. **`/kotae-awase` は例外あり。** 言語は `原文:` ブロックではなく `私の解釈:` ブロックから判定します — 原文はほぼ必ず日本語だからです。

1 つの返答の中で言語が混ざることはありません。構造全体 (セクションラベル、本文、例) が単一の言語になります。

## インストール

### Claude Code (CLI)

```bash
/plugin marketplace add zh/ja-office-skills
/plugin install ja-office-skills          # 全 11 スキル
```

スキルを個別にインストールすることもできます (`<スキル名>@ja-office-skills`):

```bash
/plugin install honto@ja-office-skills
/plugin install en@ja-office-skills
/plugin install kantan@ja-office-skills
# ... bunkai, youten, nukemore, yougo, maemuki, utagai, kotae-awase, ja も同様
```

**メモ:**

- スキルは短い名前だけで呼び出せます — `/honto`、`/ja`、`/en` — プラグイン名のプレフィックスは不要です (Claude Code 2.1.216 以降)。名前空間付きの形 (`/ja-office-skills:honto`) が必要になるのは、同じ名前のコマンドが他にある場合だけです。
- 全部入りプラグイン**か**個別スキルの**どちらか一方**をインストールしてください — 両方入れると各スキルが二重になります。
- インストール済みプラグインはキャッシュされ、バージョンが固定されます。新しいリリースを反映するには:

  ```bash
  /plugin update ja-office-skills
  ```

直接クローンする場合:

```bash
git clone https://github.com/zh/ja-office-skills ~/.claude/plugins/ja-office-skills
```

### Claude Cowork

1. [Releases](https://github.com/zh/ja-office-skills/releases) から最新のリリース ZIP をダウンロードします。
2. Claude Desktop を開き、**Cowork** → **Customize** → **Browse plugins** → ZIP をアップロードします。
3. チーム全員のスラッシュコマンドパレットにスキルが表示されます。

組織全体への展開は、管理者が **Organization → Plugin Marketplace** で GitHub リポジトリを接続するか、ZIP をアップロードしてください。

### claude.ai (Web 版)

1. [Releases](https://github.com/zh/ja-office-skills/releases) から最新のリリース ZIP をダウンロードします。
2. **Settings → Features → Skills → Upload custom skill** に進みます。
3. スキルフォルダを 1 つずつアップロードします (プランが対応していれば、まとめてのアップロードも可能です)。

コード実行が有効な Pro / Max / Team / Enterprise プランが必要です。

## 旧バージョンからのアップグレード

### 1.3.1 → 1.3.2

スキルの変更はありません。マーケットプレイスで各スキルが個別プラグインとしてもインストール可能になり (`/plugin install honto@ja-office-skills`)、全部入りの `ja-office-skills` プラグインと併存します。日本語 README (`README_ja.md`) も追加しました。

### 1.2.x → 1.3.0

新スキルを 2 つ追加: `/ja` と `/en` — 任意の言語と日本語/英語の間のクイック翻訳です。既存の 9 スキルに変更はありません。

**動作のポイント:**
- 引数なしの場合、会話の直前のメッセージ (アシスタント/ユーザーどちらでも) を翻訳します。他のスキルの直後に自然につながります (例: `/kantan` → `/en`)。
- 純粋な翻訳で、長さと構造は元の文章に合わせます。慣用句の意味がどうしても失われる場合のみ、最後に `Note:` 行を 1 つ付けます。
- 元の言語は自動判定。すでに翻訳先の言語なら 1 行のヒント付きでお断りします (`Already in Japanese — did you mean /en?`)。
- この 2 スキルに `lang=` 指定はありません — コマンド自体が翻訳先の言語を決めるためです。

### 1.1.x → 1.2.x

1.2.x 系で変わるのはバイリンガルの 5 スキル (`utagai`、`maemuki`、`nukemore`、`youten`、`kotae-awase`) のみです。残りの 4 つ (`kantan`、`honto`、`bunkai`、`yougo`) は変更ありません。

**変更点:**
- 各バイリンガルスキルに必須の `Step 0` 言語判定を追加。自動判定 + `lang=en|ja` の強制指定に対応。
- フォーマットセクションに日本語テンプレートと英語テンプレートの**両方**を用意し、セクションラベルを揃えました — 英語テンプレートがなかったために英語入力でも日本語出力になりがちだった旧動作を修正。
- 冒頭ルールを 10 行目の太字バイリンガル文に変更。
- YAML の `description` をバイリンガル化 (`[EN] … [JA] …`)。スキル発見が日本語に偏らないようにするためです。

**体感できる動作の違い:**
- 英語のみの入力で、確実に英語のみの出力になります (以前はこれが静かに失敗していました)。
- とても短い入力や言語が混ざった入力では、日本語をデフォルトにせず `Output language? (en/ja)` と質問します。
- 先頭トークンの `lang=en` / `lang=ja` で言語を強制できます (例: `/utagai lang=en my plan…`)。トークンは処理前に消費されます。
- 引数の形式、出力セクション、行数制限に変更はありません。

**アップグレード方法 (Claude Code):**

```bash
/plugin update ja-office-skills
```

`update` が使えない場合:

```bash
/plugin uninstall ja-office-skills
/plugin install ja-office-skills
```

キャッシュが新バージョンになっているか確認:

```bash
ls ~/.claude/plugins/cache/ja-office-skills/ja-office-skills/
cat ~/.claude/plugins/cache/ja-office-skills/ja-office-skills/1.2.*/.claude-plugin/plugin.json
```

(新しいディレクトリは `1.2.x` です。新バージョンの動作を確認したら、古い `1.1.x` ディレクトリは削除して構いません。)

**アップグレード方法 (Claude Cowork / claude.ai Web 版):**
既存のプラグイン/スキルを削除し、最新の 1.2.x リリース ZIP をアップロードしてください。

**1.1.x へのロールバック:** 古いバージョンを指定してインストールするか (マーケットプレイスがタグに対応していれば `/plugin install ja-office-skills@1.1.x`)、古いリリース ZIP を再アップロードしてください。データ移行はありません。変更は指示文のみです。

## バージョニング

全 11 スキルは単一のプラグインバージョンで配布されます。現行シリーズ: **1.3.x** — 正確なバージョンは [Releases](https://github.com/zh/ja-office-skills/releases) または `.claude-plugin/plugin.json` を参照してください。

## ライセンス

MIT
