# カリキュラムマップ（AI 参照用）

学習者が章番号を伝えてきたときに、**その時点で何を習得済みか／未習か**を判断するための表です。
未習の概念を説明やサンプルコードに使わないでください（`ai/instructions.md` 4.2 参照）。

---

## 1. react-text（Web 開発入門 + React）

| 章 | タイトル | この章を終えた時点の既習範囲（累積） | よくあるつまずき |
|----|---------|-----------------------------------|----------------|
| 0 | はじめに | — | AI の準備をせずに進んでしまう |
| 1 | Web の仕組みと開発環境 | ブラウザ／サーバー／HTTP の概念、VS Code、ターミナル基本操作、Node.js インストール | **PATH**、ターミナル恐怖症、拡張子が隠れていてファイル名を間違える |
| 2 | HTML | タグ、要素、属性、見出し・段落・リスト・リンク・画像・表・フォーム、構造タグ | 閉じタグ忘れ、パスの相対指定、日本語の文字化け |
| 3 | CSS | セレクタ、ボックスモデル、色・文字、Flexbox、レスポンシブ | CSS が効かない（読み込み・詳細度・キャッシュ）、`margin` の相殺 |
| 4 | JavaScript 基礎（前半） | 変数（`let`/`const`）、データ型、演算子、`if`、`for`/`while`、関数、アロー関数 | `=` と `==` と `===`、スコープ、`i` の意味 |
| 5 | JavaScript 基礎（後半） | 配列（`push`/`pop`/`unshift`/`shift`/`includes`/`indexOf`/`join`/`slice`/`concat`/`for...of`）、オブジェクト（読み書き・入れ子・オプショナルチェーン`?.`）、`map`/`filter`/`find`/`reduce`/`sort`とチェーン、分割代入、スプレッド構文、イミュータブルな更新、非同期処理（`setTimeout`/`Promise`/`async`/`await`）、`fetch`とエラー処理、`export`/`import`（名前付き・デフォルト）、`type="module"`、DOM 操作（`querySelector`/`textContent`/`classList`/`addEventListener`/`createElement`/`appendChild`/`remove`） | `TypeError: Cannot read properties of undefined`、`const copy = original` が複製にならない、`reduce` の初期値省略、`setTimeout` が待ってくれると誤解する、モジュールを `file://` で直接開いて動かない |
| 6 | React をはじめる | Vite、プロジェクト構成、JSX、コンポーネント、`className` | Node バージョン非互換、JSX の1要素ルール、`class` と書いてしまう |
| 7 | props と state | `props`、`useState`、イベント、リスト表示と `key`、条件付きレンダリング、フォーム（制御コンポーネント） | state 直接代入、`key` 無し警告、`onClick={fn()}` と書いて即実行 |
| 8 | 状態設計と副作用 | 状態のリフトアップ、`useEffect`、データ取得、`useRef`、`useMemo`/`useCallback`、カスタムフック | `useEffect` の無限ループ、依存配列、クリーンアップ |
| 9 | ルーティングと全体設計 | React Router、`Context`、ディレクトリ構成、エラー処理 | ルーティングのパス指定、Context の再レンダリング |
| 10 | 実践：タスク管理アプリ | 上記すべての統合、localStorage | 設計の分解ができない |
| 11 | 次のステップ | TypeScript / テスト / デプロイ の概観 | — |
| 12 | 解答編 | — | — |

> **注意**：4 章の学習者に `map` を使ったコードを見せないでください（5 章の内容）。
> 6 章の学習者に `useEffect` の話をしないでください（8 章の内容）。

---

## 2. python-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに | — |
| 1 | Python の環境構築 | Python インストール、`python` コマンド、REPL、`venv`、`pip` |
| 2 | 基本文法 | 変数、型、演算子、文字列、`input`/`print`、インデント |
| 3 | 制御構文 | `if`、`for`、`while`、`range`、`break`/`continue` |
| 4 | データ構造 | list、tuple、dict、set、内包表記 |
| 5 | 関数 | 定義、引数（デフォルト・可変長・キーワード）、戻り値、スコープ |
| 6 | モジュールとパッケージ | `import`、標準ライブラリ、自作モジュール、`__name__` |
| 7 | ファイル操作と例外 | `open`、`with`、`try`/`except`、`pathlib` |
| 8 | オブジェクト指向 | クラス、インスタンス、継承、特殊メソッド、`dataclass` |
| 9 | 型ヒントとモダン Python | 型ヒント、`typing`、`mypy`、ツール（ruff / uv） |
| 10 | 実践：データ処理スクリプト | CSV / JSON、外部 API |
| 11 | 解答編 | — |

## 3. fastapi-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに（前提：python-text 完了） | — |
| 1 | Web API とは | HTTP メソッド、ステータスコード、JSON、REST |
| 2 | FastAPI をはじめる | インストール、最初のエンドポイント、自動ドキュメント |
| 3 | パラメータ | パスパラメータ、クエリパラメータ、リクエストボディ |
| 4 | Pydantic | モデル定義、バリデーション、レスポンスモデル |
| 5 | ルーター分割とプロジェクト構成 | `APIRouter`、依存性注入 |
| 6 | データベース連携 | SQLAlchemy、マイグレーション |
| 7 | 認証 | パスワードハッシュ、JWT、依存性による認可 |
| 8 | テスト | `TestClient`、pytest |
| 9 | 実践：React と繋ぐ | CORS、フロントとの結合 |
| 10 | 解答編 | — |

## 4. docker-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに | — |
| 1 | Docker が解決する問題 | 仮想化とコンテナ、イメージとコンテナ |
| 2 | Docker のインストールと基本操作 | `run`/`ps`/`stop`/`rm`/`images` |
| 3 | Dockerfile | `FROM`/`RUN`/`COPY`/`CMD`、レイヤ、キャッシュ |
| 4 | ボリュームとネットワーク | bind mount、named volume、ポート公開、コンテナ間通信 |
| 5 | Docker Compose | `compose.yaml`、複数サービス、依存関係 |
| 6 | 実践：React + FastAPI + MySQL を1コマンドで起動 | 統合 |
| 7 | イメージの最適化と本番運用 | マルチステージビルド、`.dockerignore`、セキュリティ |
| 8 | 解答編 | — |

## 5. mysql-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに | — |
| 1 | データベースとは | RDB の考え方、テーブル・行・列 |
| 2 | 環境構築（Docker で MySQL） | 接続、クライアント |
| 3 | SELECT | `WHERE`、`ORDER BY`、`LIMIT`、関数 |
| 4 | データの追加・更新・削除 | `INSERT`/`UPDATE`/`DELETE`、トランザクション |
| 5 | テーブル設計 | データ型、主キー、外部キー、制約、正規化の入口 |
| 6 | 結合と集計 | `JOIN`、`GROUP BY`、サブクエリ |
| 7 | インデックスと実行計画 | `EXPLAIN`、遅いクエリの直し方 |
| 8 | アプリからの利用 | FastAPI からの接続、N+1 問題 |
| 9 | 解答編 | — |

---

## 6. 学習順序の依存関係

```
react-text（1冊目）
   └─ 前提なし。ここから始める。

python-text（2冊目）
   └─ 前提なし（react-text の 1 章「ターミナル」を読んでいると楽）

fastapi-text（3冊目）
   └─ 前提: python-text 完了

docker-text（4冊目）
   └─ 前提: react-text と fastapi-text で「動くアプリ」を作った経験

mysql-text（5冊目）
   └─ 前提: docker-text 2章まで（Docker で MySQL を立てるため）
```

学習者が前提を満たさず質問してきた場合、
「先に〜を読むと楽ですが、いまの範囲でも説明できます」と伝えたうえで説明してください。
**門前払いはしないでください。**

---

## 7. 全テキスト共通の「レベル C 頻出トラブル」

学習者がこれらに遭遇したら、迷わず全部解決してあげてください。

- Node.js / Python のバージョンが古い or 新しすぎる
- PATH が通っていない（特に Windows）
- PowerShell の実行ポリシーで `.ps1` が動かない
  → `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`
- 仮想環境（venv）の有効化を忘れている
- プロキシ環境で `npm install` / `pip install` が失敗する
- ポート 3000 / 5173 / 8000 / 3306 が既に使われている
- Windows の改行コード（CRLF）が Docker コンテナ内で問題を起こす
- ファイル名の拡張子が Windows で隠れていて `index.html.txt` になっている
- 日本語パス・スペース入りパスに起因する不具合
- Docker Desktop が起動していない
- Apple Silicon（M1〜）でのイメージ非対応（`platform: linux/amd64` が必要）
- VS Code の拡張機能が原因のフォーマット崩れ
