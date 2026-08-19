# プログラミング未経験から Web エンジニアになるためのテキスト

プログラミングを**一度もやったことがない人**が、順番に読み進めるだけで
「一人前の Web エンジニア」の入り口まで到達することを目指した学習テキスト集です。

各テキストは [Zenn](https://zenn.dev/) で無料公開します。

---

## このリポジトリの使い方（学習者の方へ）

### 1. AI サポートの準備をする（最初にやってください）

このテキストは**生成 AI にサポートしてもらいながら学ぶこと**を前提に作られています。

普段使っている AI（ChatGPT / Claude / Gemini / GitHub Copilot / Codex など）に、
まず次の URL を読み込ませてください。

```
https://github.com/TakahashiTaiga/programing-texts/blob/main/ai/instructions.md
```

そのうえで、こう伝えてください。

> このドキュメントの指示に従って、私の学習をサポートしてください。

これで AI が「答えを全部教えてしまう先生」ではなく、
**あなたが伸びるちょうどいい距離感のサポーター**として振る舞ってくれます。

> 普段使っている AI が特にない場合は **Claude** または **Codex** をおすすめします。
> 詳しくは各テキストの第0章を読んでください。

### 2. つまずいたら、章番号とエラーを AI に投げる

```
React本の 2.2.1 で詰まりました。
やったこと: （書いたコード）
出たエラー: （エラーメッセージをそのまま貼る）
```

この形で投げれば、AI が「今のあなたに必要な粒度」で助けてくれます。

---

## テキスト一覧

| # | テキスト | ディレクトリ | 状態 |
|---|---------|------------|------|
| 1 | React（Web 開発入門 + React） | [`react-text/`](./react-text/) | 執筆中 |
| 2 | Python | [`python-text/`](./python-text/) | 準備中 |
| 3 | FastAPI | [`fastapi-text/`](./fastapi-text/) | 準備中 |
| 4 | Docker | [`docker-text/`](./docker-text/) | 準備中 |
| 5 | MySQL | [`mysql-text/`](./mysql-text/) | 準備中 |

将来的に扱う予定のテーマ（ハードウェア基礎 / ネットワーク / データベース設計 / DDD / AWS / IaC）は
[`docs/roadmap.md`](./docs/roadmap.md) を参照してください。

---

## ディレクトリ構成

```
.
├── ai/                  # 生成AIへの指示ファイル群（★学習者はここを AI に読ませる）
│   ├── instructions.md      # サポートAIの振る舞いを定義したメインファイル
│   ├── hint-policy.md       # 「どこまで答えるか」の判定基準
│   ├── prompt-templates.md  # 学習者がコピペで使える質問テンプレート
│   └── curriculum-map.md    # 章番号 → 既習範囲・頻出のつまずき
├── docs/                # 執筆者向けの共通メモ
│   ├── roadmap.md
│   ├── writing-guidelines.md
│   ├── chapter-template.md
│   ├── style-guide.md
│   ├── glossary.md
│   ├── zenn-publishing.md
│   ├── SETUP-CHECKLIST.md
│   └── writing-tasks/       # ★執筆タスクのキュー（1タスクずつ消化する）
│       ├── RUNBOOK.md          実行手順と完成条件
│       ├── TASKS.md            全59タスクの一覧と進捗
│       └── review-notes.md     人間向けの申し送り
├── react-text/          # 1冊目
├── python-text/         # 2冊目
├── fastapi-text/        # 3冊目
├── docker-text/         # 4冊目
└── mysql-text/          # 5冊目
```

---

## 執筆の進め方（執筆者向け）

章立ては各テキストの `README.md` に3階層（`2.3.4`）まで確定済みです。
そこから **1章 = 1タスク**に分割し、[`docs/writing-tasks/TASKS.md`](./docs/writing-tasks/TASKS.md)
のキューを上から1件ずつ消化していきます。

| ファイル | 役割 |
|---------|------|
| [`docs/writing-tasks/TASKS.md`](./docs/writing-tasks/TASKS.md) | 全59タスクの一覧と進捗 |
| [`docs/writing-tasks/RUNBOOK.md`](./docs/writing-tasks/RUNBOOK.md) | 実行手順・完成条件・ルーティンに貼るプロンプト |
| [`docs/writing-tasks/review-notes.md`](./docs/writing-tasks/review-notes.md) | 人間が検証すべき箇所の申し送り |

> **各本の第1〜2章（環境構築）だけは、必ず人間が実機で検証してください。**

---

## ライセンス

本文・コードともに自由に利用いただけます（[LICENSE](./LICENSE) 参照）。
