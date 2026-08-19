# Zenn への公開手順メモ

このリポジトリのテキストを Zenn の「本」として公開する手順。

---

## 1. Zenn のディレクトリ構成との対応

Zenn CLI は、リポジトリ直下の `books/` と `articles/` を見に行きます。
一方このリポジトリは、執筆しやすさを優先して `react-text/` のような
名前でディレクトリを切っています。

対応方法は2つ。**方法 A を推奨**します。

### 方法 A：Zenn 用リポジトリを別に用意し、シンボリックリンク or コピーで同期する（推奨）

執筆はこのリポジトリで行い、公開用リポジトリへコピーする。

```
zenn-contents/            ← Zenn と連携するリポジトリ
└── books/
    ├── react-for-beginners/
    │   ├── config.yaml
    │   ├── 00-introduction.md
    │   └── ...
    └── python-for-beginners/
```

**理由**：このリポジトリには `ai/` `docs/` など Zenn に不要なものが多く、
公開物と執筆資産を分けたほうが管理しやすいため。

### 方法 B：このリポジトリを直接 Zenn 連携する

`books/` ディレクトリを作り、その下に各テキストを置く構成に変える。
`ai/instructions.md` の URL が変わる点に注意。

---

## 2. Zenn CLI のセットアップ

```bash
npm init --yes
npm install zenn-cli
npx zenn init
```

プレビュー:

```bash
npx zenn preview
```

ブラウザで http://localhost:8000 が開きます。

---

## 3. `config.yaml` の書き方

`books/<本のslug>/config.yaml`

```yaml
title: "プログラミング未経験から始める React"
summary: |
  プログラミングを一度もやったことがない人向けの React 入門書。
  Web の仕組み、HTML、CSS、JavaScript から始めて、React でアプリを作れるところまで進みます。
  生成 AI にサポートしてもらいながら学ぶ構成です。
topics: ["react", "javascript", "html", "css", "初心者"]
published: false        # 公開するときに true にする
price: 0                # 無料
chapters:
  - 00-introduction
  - 01-web-and-environment
  - 02-html
  - 03-css
  - 04-javascript-basics
  - 05-javascript-advanced
  - 06-react-start
  - 07-props-and-state
  - 08-state-design-and-effects
  - 09-routing-and-architecture
  - 10-practice-task-app
  - 11-next-steps
  - 90-answers-part1
  - 91-answers-part2
```

### 注意点

- `chapters` に書いた**順番**が本の並び順になる。ファイル名の数字は関係ない
- チャプターのスラッグ（ファイル名から `.md` を除いたもの）は
  **半角英数字・ハイフン・アンダースコアのみ**
- 各チャプターの `.md` は、先頭に frontmatter が必要

```markdown
---
title: "第1章 Web の仕組みと開発環境"
---

本文...
```

- `published: false` のまま push すれば下書き状態で確認できる
- 無料公開なら `price: 0`（`price` を書かなければ無料）

---

## 4. 画像

`books/<本のslug>/images/` ではなく、Zenn では**リポジトリ直下の `/images`** に置くのが基本です。

```
images/
└── react-for-beginners/
    └── 01-vscode.png
```

参照:

```markdown
![VS Code の画面](/images/react-for-beginners/01-vscode.png)
```

- 1ファイル 3MB まで
- 対応形式：png / jpg / jpeg / gif / webp

---

## 5. GitHub 連携

1. Zenn にログイン → ダッシュボード → 「GitHubからのデプロイ」
2. 公開用リポジトリを連携する
3. `main` ブランチに push すると自動反映される（数十秒〜数分）

---

## 6. 公開前チェックリスト

- [ ] `config.yaml` の `published` を `true` にした
- [ ] `chapters` の順番が正しい
- [ ] すべてのチャプターに frontmatter（`title`）がある
- [ ] 本文中の相対リンク（`./03-css.md`）が Zenn 上で機能するか確認した
      → **Zenn のチャプター間リンクは `/books/<slug>/<chapter-slug>` 形式**が確実
- [ ] `ai/instructions.md` の URL が正しい（公開リポジトリの URL になっているか）
- [ ] 画像のパスが `/images/...` になっている
- [ ] `npx zenn preview` で全章を目視確認した

---

## 7. 章間リンクの書き分け

GitHub 上では相対パス、Zenn 上では絶対パスが必要になります。
**Zenn 形式に統一する**のが安全です。

```markdown
→ [第2章 HTML](https://zenn.dev/<username>/books/<book-slug>/viewer/02-html)
```

執筆中は GitHub 相対リンクで書き、公開直前に置換するスクリプトを用意すると楽です。

---

## 8. AI 指示ファイルの URL

学習者に読み込ませる URL は、**この執筆リポジトリ（public）**のものを使います。

```
https://github.com/TakahashiTaiga/programing-texts/blob/main/ai/instructions.md
```

Raw URL のほうが AI に読ませやすい場合もあります。

```
https://raw.githubusercontent.com/TakahashiTaiga/programing-texts/main/ai/instructions.md
```

**この URL は各テキストの第0章に登場します。**
リポジトリ名やユーザー名を変えた場合は、次のファイルをすべて更新してください。

- `README.md`
- `ai/instructions.md`
- `ai/prompt-templates.md`
- `<各テキスト>/00-introduction.md`
