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
| 4 | JavaScript 基礎（前半） | `<script>` の読み込み、`console.log`、エラーの読み方、変数（`let`/`const`、`var` は使わない）、数値・文字列・真偽値・`undefined`/`null`、`typeof`、算術演算子と `%`、`Math.floor`/`ceil`/`round`、テンプレートリテラル、`===`/`!==`、`&&`/`\|\|`/`!`、`Number()`、`if`/`else if`/`else`、三項演算子、`switch`、`for`/`while`、`break`/`continue`、関数（`function`・アロー関数・引数・戻り値）、スコープ | `=` と `==` と `===`、スコープ、`i` の意味、小数の誤差、`return` を書かず `undefined` になる、無限ループ |
| 5 | JavaScript 基礎（後半） | 配列（`push`/`pop`/`unshift`/`shift`/`includes`/`indexOf`/`join`/`slice`/`concat`/`for...of`）、オブジェクト（読み書き・入れ子・オプショナルチェーン`?.`）、`map`/`filter`/`find`/`reduce`/`sort`とチェーン、分割代入、スプレッド構文、イミュータブルな更新、非同期処理（`setTimeout`/`Promise`/`async`/`await`）、`fetch`とエラー処理、`export`/`import`（名前付き・デフォルト）、`type="module"`、DOM 操作（`querySelector`/`textContent`/`classList`/`addEventListener`/`createElement`/`appendChild`/`remove`） | `TypeError: Cannot read properties of undefined`、`const copy = original` が複製にならない、`reduce` の初期値省略、`setTimeout` が待ってくれると誤解する、モジュールを `file://` で直接開いて動かない |
| 6 | React をはじめる | 命令的と宣言的の違い、Vite（`npm create vite@latest ... -- --template react`／`npm install`／`npm run dev`／Ctrl+C）、プロジェクト構成（`src`／`index.html`／`main.jsx`／`App.jsx`／`package.json`／`node_modules`／`public`）、`createRoot`と`StrictMode`（存在のみ）、CSS を `import` で読み込む、JSX（1要素ルール、`className`、閉じタグ必須、キャメルケース属性、`{/* */}`）、`{ }` での式の埋め込み（変数・計算・三項演算子。`if`/`for` は書けない）、属性への値渡しと `style={{ }}`、フラグメント `<>`、コンポーネント（作成・`export default`/`import` でのファイル分割・分け方の基準・大文字始まりの命名） | Node バージョン非互換（Vite は Node 20.19+／22.12+ が必要）、ポート 5173 の衝突、`Missing script: "dev"`（プロジェクト外で実行）、JSX の1要素ルール、`class` と書いて CSS が効かない、オブジェクトを `{ }` に直接入れる、`style` の波かっこ1つ、コンポーネント名を小文字で始めて何も表示されない、`import` パスに `./` を付け忘れる |
| 7 | props と state | props（分割代入での受け取り・デフォルト値・型ごとの渡し方・`children`・書き換え禁止）、`useState`（初期値・`set○○`・再レンダリング・フックのルール）、イミュータブルな state 更新（配列・オブジェクト）、関数形式の更新 `set○○((prev) => ...)`、イベント（`onClick`／`onChange`／`onSubmit`、`handle○○` の命名、引数を渡すアロー関数、`event.target.value`）、`map` での一覧表示と `key`（index を避ける理由）、`Date.now()` での id 生成、条件表示（`&&`／三項演算子／早期 `return`／`null` を返す）、制御コンポーネント（`value`＋`onChange`、`name` 属性と `[event.target.name]`、`checked`、`event.preventDefault()`）、文字列の `includes` での絞り込み | state を直接書き換えて画面が変わらない（`push`）、`onClick={fn()}` と書いて `Too many re-renders`、`key` 無し警告と index による行のずれ、`{items.length && ...}` で `0` が出る、`value` だけ書いて入力できない、`preventDefault` 忘れでページが再読み込みされる |
| 8 | 状態設計と副作用 | 状態のリフトアップ（共通の親に state を上げる・更新関数を props で渡す・`on〜`／`handle〜` の命名・state の置き場所の決め方）、派生した値は state にせず計算する、`useEffect`（副作用の考え方・依存配列の3通り・無限ループ・クリーンアップ関数・`<StrictMode>` による二重実行・使うべきでない場面）、`fetch` によるデータ取得（`useEffect` 内で `async function` を定義して呼ぶ・`response.ok` の確認・data／isLoading／errorMessage の3 state・早期 `return` での出し分け・依存配列を使った再取得・CORS の考え方）、`useRef`（DOM 操作と、再レンダリングされない値の保持）、`useMemo`／`useCallback`／`memo`、`console.time` での計測、カスタムフック（`use` で始まる関数・`useFetch`・フックのルール） | `useEffect` の無限ループ、依存配列の書き忘れ／オブジェクトを入れてしまう、クリーンアップ忘れでタイマーが残る、`useEffect(async () => ...)` と書く、`fetch` が 404 を失敗にしないこと、`data` の初期値が `null` のまま `map` を呼ぶ、`ref.current` の書き忘れ、早期 `return` のあとにフックを呼ぶ |
| 9 | ルーティングと全体設計 | SPA の考え方、React Router（`BrowserRouter`／`Routes`／`Route`／`element` に渡すもの・`Link`／`NavLink` と `<a>` の違い・`useParams` と URL パラメータ（値は文字列）・`useNavigate`・`Outlet` によるネストしたルートと共通レイアウト・`index` ルート・`path="*"` の 404・`useLocation`）、Context（`createContext`／`Provider`／`useContext`・値と更新関数をまとめて流す・props のバケツリレー・使いどころの判断）、状態管理ライブラリの位置づけ（使わない）、ディレクトリ構成（種類で分ける `pages`／`components`／`hooks`／`contexts`／`data`・命名規則）、エラーバウンダリ（`react-error-boundary`・`FallbackComponent`・`resetKeys`・受け止めない範囲）、共通の `Loading`／`ErrorMessage` 部品、props の初期値、`useFetch` への `reload` の追加、画面の4状態（読み込み中／エラー／0件／表示） | 子ルートの `path` に `/` を付ける、`element={HomePage}` と書く、`<a href>` で state が消える、`useParams` の値が文字列で `===` が成立しない、`onClick={navigate('/')}` で即移動、Provider の外で `useContext` して `null`、Context に入力中の値を入れる、`import` パスの直し忘れ |
| 10 | 実践：タスク管理アプリ | 上記すべての統合に加えて、設計の手順（完成イメージ→機能一覧→MVP の切り出し→画面を描く→コンポーネント分解→データの形→state の置き場所）、派生値を state にしない判断（10.2.4）、`Date.now()` による id、配列 state の更新パターン（追加は `[...tasks, newTask]`／1件変更は `map` + スプレッド／削除は `filter`）、`[...配列].sort()`、`localeCompare(相手, 'ja')`、対応表オブジェクトと `オブジェクト[変数]` での参照、`localStorage`（`setItem`／`getItem`／`removeItem`・保存できるのは文字列だけ）、`JSON.stringify` / `JSON.parse`、読み込み前の上書きを防ぐガード、`useLocalStorage` カスタムフック、0件表示の出し分け（未登録／絞り込み結果0件）、`trim()`、`disabled` 属性、エラー時の早期 return | 設計の分解ができない、`push` で更新して画面が変わらない、`sort` が元の配列を書き換える、`localStorage` にオブジェクトを直接入れて `[object Object]` になる、保存の `useEffect` が読み込みより先に走って初期値で上書きされる、`Date.now()` の id と `useParams` の文字列を `===` で比較する |
| 11 | 次のステップ | 上記すべてに加えて、TypeScript の基礎（`.tsx`／型注釈 `: string` `: number` `: boolean`／型推論／関数の引数と戻り値の型／`type` によるオブジェクトの型／props の型 `({ ... }: Props)`／`(引数: 型) => void`／`useState<Task[]>`／`npm create vite@latest ... -- --template react-ts`／`tsc -b` による型チェック）、テスト（Vitest のインストール・`test`／`expect().toBe()`／`toEqual()`・`npm test`・値を返す関数に切り出してからテストする）、ビルド（`npm run build`／`dist` の中身／`npm run preview`（4173））、デプロイ（Netlify Drop／Vercel + GitHub の自動デプロイ）、Git（`git init`／`status`／`add`／`commit -m`／`push`／`restore`／`log --oneline`／`remote add origin`／`.gitignore`） | `dist/index.html` を `file://` で開いて真っ白になる、`npm run preview` を 5173 で開く、`node_modules` をコミットしてしまう、GitHub 側に README を作って `git push` が拒否される、`import` パスの大文字小文字の違いが公開環境でだけ失敗する、型エラーを放置したまま `npm run dev` で進めてビルドで詰まる、`localStorage` のデータが公開先で共有されると誤解する |
| 12 | 解答編 | — | — |

> **注意**：4 章の学習者に `map` を使ったコードを見せないでください（5 章の内容）。
> 6 章の学習者に `props` / `useState` / JSX 内での `map` と `key` を使わせないでください（7 章の内容）。
> 6 章の学習者に `useEffect` の話をしないでください（8 章の内容）。
> 7 章の学習者に `useEffect` / 状態のリフトアップ / `useRef` / カスタムフック / React Router を使わせないでください（8 章以降の内容）。
> 7 章の時点では、state はコンポーネント1つの中で完結させ、データの保存（`localStorage`）にも触れません。
> 8 章の学習者に React Router / `Context` / `localStorage` を使わせないでください（9 章・10 章の内容）。
> 8 章の時点では、画面は1つだけです。URL による画面の切り替えは 9 章で扱います。
> 9 章では状態管理ライブラリ（Redux / Zustand など）を導入しません。
> 共有する値は `useState` のリフトアップ（8.1）と Context（9.2）だけで扱います。
> 9 章までの学習者に `localStorage` / `JSON.stringify` / `JSON.parse` を使わせないでください（10 章の内容）。
> 10 章で作るのは `my-first-react` ではなく、新しく作る `task-app` プロジェクトです。
> 10 章では React Router を使いません（10.6.4 の発展課題としてのみ触れています）。
> 11 章の TypeScript は**入口だけ**です。ジェネリクス・`interface`・ユニオン型・`unknown` などは
> 扱っていないので、11 章の学習者に前提として使わせないでください。
> 11 章のテストも入口だけです。React Testing Library による画面のテストは扱っていません
> （テストするのは「値を渡すと値が返る関数」に限っています）。
> 11 章で `ts-practice` という練習用プロジェクトを新しく作ります（`task-app` は JavaScript のままです）。

---

## 2. python-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに | （コードなし）AI サポートの準備、JavaScript / Python 対応表（`let`/`const`→代入のみ、`===`→`==`、`{}`→インデント、配列→リスト、オブジェクト→辞書、`map`→内包表記、`null`/`undefined`→`None` の「地図」。詳細は各章で扱う） |
| 1 | Python の環境構築 | Python 3.13 のインストール、`python`（Windows）/ `python3`（macOS）の呼び分け、`--version` での確認、`py` ランチャー（Windows）、REPL（`>>>`・`exit()`）、`.py` ファイルの実行（`python ファイル名`）、`print`（存在のみ。詳細は 2.5）、トレースバックの読み方（下から上・種類・行番号・`NameError`）、仮想環境 venv（`python -m venv .venv`・有効化 `Activate.ps1` / `source .venv/bin/activate`・`deactivate`）、実行ポリシー（`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`）、pip（`pip install`・`pip list`・`pip freeze > requirements.txt`・`-r`）、`import`（存在のみ。詳細は第6章）、VS Code の Python 拡張・インタプリタ選択・▷ 実行・ブレークポイントでのステップ実行 |
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

> **注意**：第1章の学習者は、まだ変数・型・条件分岐・繰り返し・関数を学んでいません（第2章以降）。
> 第1章の範囲は「環境を用意し、REPL とファイル実行で Python を動かし、venv と pip を扱える」までです。
> 環境構築・エラー（トレースバック）・venv・pip のトラブルは、レベル C（第7節）として全部解決してあげてください。

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
