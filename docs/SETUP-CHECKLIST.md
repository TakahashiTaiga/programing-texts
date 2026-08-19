# 公開前セットアップ チェックリスト

このリポジトリを実際に公開するまでにやることのリスト。

---

## 1. プレースホルダを実際の値に置き換える

現在、リポジトリの URL は `TakahashiTaiga` というプレースホルダになっています。
**GitHub にリポジトリを作ったら、次のファイルをすべて置換してください。**

置換対象：`TakahashiTaiga` → 実際の GitHub ユーザー名（またはOrganization 名）

| ファイル | 出現箇所 |
|---------|---------|
| `README.md` | AI に読ませる URL |
| `ai/instructions.md` | （現在なし。将来追加したら確認） |
| `ai/prompt-templates.md` | 質問テンプレート内の URL |
| `docs/zenn-publishing.md` | URL の説明 |
| `react-text/00-introduction.md` | **0.2.3 と 0.3.2**（最重要） |
| 各テキストの `00-introduction.md` | 同上 |

**一括置換コマンド（PowerShell）**

```powershell
Get-ChildItem -Recurse -Include *.md | ForEach-Object { (Get-Content $_.FullName -Raw) -replace 'TakahashiTaiga', 'あなたのGitHubユーザー名' | Set-Content $_.FullName -Encoding utf8 }
```

**一括置換コマンド（bash）**

```bash
grep -rl 'TakahashiTaiga' --include='*.md' . | xargs sed -i 's/TakahashiTaiga/あなたのGitHubユーザー名/g'
```

置換後、残っていないか確認します。

```bash
grep -rn 'TakahashiTaiga' --include='*.md' .
```

---

## 2. Git リポジトリを作る

```bash
git init
git add .
git commit -m "初回コミット: リポジトリ構成と React テキスト第3章まで"
git branch -M main
git remote add origin https://github.com/TakahashiTaiga/programing-texts.git
git push -u origin main
```

> **必ず public リポジトリにしてください。**
> private だと、学習者の AI が `ai/instructions.md` を読み込めません。

---

## 3. AI 指示ファイルの動作確認

公開したら、**実際に自分で試してください。**

1. Claude / ChatGPT などを開く
2. `react-text/00-introduction.md` の 0.2.3 の文章をそのまま送る
3. 0.2.4 の確認質問を投げる
4. 期待どおりの答えが返ってくるか確認する

**確認すべき挙動**

- [ ] 演習問題の答えを聞いたら、ヒントを返してくる（いきなり答えを書かない）
- [ ] 環境エラーを投げたら、コピペできる手順を全部出してくる
- [ ] 日本語・ですます調で返ってくる
- [ ] 未習の内容（4章の人に `map` など）を使ってこない

**うまくいかない AI があった場合**

`ai/instructions.md` の冒頭に、その AI 向けの補足を追加してください。
特に「URL を読み込めない AI」用の案内（現在 0.2.3 に記載）が有効かどうかを確認します。

---

## 4. Zenn に公開する

[`docs/zenn-publishing.md`](./zenn-publishing.md) を参照してください。

- [ ] Zenn 連携用のリポジトリを用意した
- [ ] `config.yaml` の `chapters` が実際のファイルと一致している
- [ ] 全チャプターに frontmatter（`title`）がある
- [ ] `npx zenn preview` で全章を目視確認した
- [ ] チャプター間のリンクが Zenn 上でも機能する形式になっている
- [ ] `published: true` にした

---

## 5. 執筆を進める

現在の進捗：

| テキスト | 進捗 |
|---------|------|
| react-text | 第0〜3章 + 解答編（第3章まで）完成 |
| python-text | 目次のみ |
| fastapi-text | 目次のみ |
| docker-text | 目次のみ |
| mysql-text | 目次のみ |

**次にやること**

1. react-text 第4章（JavaScript 基礎・前半）を書く
2. 書いたら **その場で** `90-answers-part1.md` の「第4章」に解答を追記する
3. `ai/curriculum-map.md` の既習範囲が実際の内容と合っているか確認する
4. `docs/style-guide.md` の章末チェックリストで確認する

**章を書くときは** [`docs/chapter-template.md`](./chapter-template.md) をコピーして使ってください。

---

## 6. 継続的にやること

- [ ] 半年に一度、バージョン依存の記述（Node.js のバージョンなど）を見直す
- [ ] 読者から「動かない」報告があった箇所に注記を追加する
- [ ] 新しい用語を使うときは `docs/glossary.md` に登録する
- [ ] 章を追加したら `ai/curriculum-map.md` を更新する
