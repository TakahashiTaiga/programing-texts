# 公開前セットアップ チェックリスト

このリポジトリを実際に公開するまでにやることのリスト。

---

## 1. プレースホルダの置換 ✅ 完了

リポジトリ内の URL は `TakahashiTaiga/programing-texts` に置換済みです。
リポジトリ名やユーザー名を変えた場合は、次のファイルを更新してください。

- `README.md`
- `ai/prompt-templates.md`
- `docs/zenn-publishing.md`
- `react-text/00-introduction.md`（0.2.3 と 0.3.2）
- 各テキストの `00-introduction.md`

```bash
grep -rn 'TakahashiTaiga' --include='*.md' .
```

---

## 2. Git リポジトリ ✅ 完了

`https://github.com/TakahashiTaiga/programing-texts` にプッシュ済みです。

> **必ず public リポジトリにしてください。**
> private だと、学習者の AI が `ai/instructions.md` を読み込めません。
> GitHub の Settings → Danger Zone から変更できます。

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

**章立ては全5冊分、3階層（`2.3.4`）まで確定済み**です（各テキストの `README.md`）。
そこから **1章 = 1タスク**に分割し、キューを上から消化していきます。

| ファイル | 役割 |
|---------|------|
| [`writing-tasks/TASKS.md`](./writing-tasks/TASKS.md) | 全59タスクの一覧と進捗 |
| [`writing-tasks/RUNBOOK.md`](./writing-tasks/RUNBOOK.md) | 実行手順・完成条件・ルーティンに貼るプロンプト |
| [`writing-tasks/review-notes.md`](./writing-tasks/review-notes.md) | 人間が検証すべき箇所の申し送り |

### クラウドルーティンの設定

1. [`writing-tasks/RUNBOOK.md`](./writing-tasks/RUNBOOK.md) の「2. ルーティンに設定するプロンプト」をコピー
2. Claude のクラウドルーティンに貼り付け、リポジトリを `TakahashiTaiga/programing-texts`、ベースブランチを `main` に設定
3. 実行環境で `gh`（GitHub CLI）がこのリポジトリに認証済みであることを確認する（PR 作成に必要）
4. 頻度は **1日1〜2回**を推奨（人間が読んで確認できるペースにする）

**ルーティンは main に直接コミットしません。** タスクごとにブランチを切り、
PR を作成するところまでを行います。マージは人間が行います。

### 人間がやること

- [ ] **作成された PR をレビューしてマージする**（これが新しい必須作業）
- [ ] **各本の第1〜2章（環境構築）は必ず実機で通しで検証する**
- [ ] SVG → PNG の変換待ちがないか `writing-tasks/review-notes.md` を確認する
- [ ] スクリーンショットを撮って `<book>/images/` に配置する
- [ ] 気づいたことを `writing-tasks/review-notes.md` に追記する

---

## 6. 継続的にやること

- [ ] 半年に一度、バージョン依存の記述（Node.js のバージョンなど）を見直す
- [ ] 読者から「動かない」報告があった箇所に注記を追加する
- [ ] 新しい用語を使うときは `docs/glossary.md` に登録する
- [ ] 章を追加したら `ai/curriculum-map.md` を更新する
