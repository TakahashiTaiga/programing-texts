# カリキュラムマップ（AI 参照用）

学習者が章番号を伝えてきたときに、**その時点で何を習得済みか／未習か**を判断するための表です。
未習の概念を説明やサンプルコードに使わないでください（`ai/instructions.md` 4.2 参照）。

---

## 1. react-text（Web 開発入門 + React）

> **この本は全章（第0章〜第11章＋解答編）が完成しています。**
> 学習者が章番号を伝えてきたら、その章までの累積範囲だけで答えてください。

| 章 | タイトル | この章を終えた時点の既習範囲（累積） | よくあるつまずき |
|----|---------|-----------------------------------|----------------|
| 0 | はじめに | （コードなし）AI サポートの準備（指示ファイルの読み込み・動作確認）、学習の進め方、このテキストの表記ルール | AI の準備をせずに進んでしまう |
| 1 | Web の仕組みと開発環境 | プログラムとプログラミング言語、ブラウザ／サーバー／クライアント、リクエストとレスポンス、HTTP・HTTPS、URL の読み方（スキーム・ドメイン・ポート・パス）、HTML/CSS/JavaScript の役割分担、VS Code（インストール・日本語化・拡張子の表示）、ターミナル（PowerShell と bash の違い・`pwd`／`cd`／`ls`（`dir`）／`mkdir`）、作業用ディレクトリ `react-lesson` の作成、Node.js のインストールと `node --version`、`index.html` を作ってブラウザで開く（`file://`）、開発者ツール（Elements / Console） | **PATH**、ターミナル恐怖症、拡張子が隠れていてファイル名を間違える |
| 2 | HTML | タグ・要素・入れ子・コメント、HTML の骨組み（`<!DOCTYPE html>`／`html`／`head`／`body`／`meta charset`／`title`）、見出し `h1`〜`h6`、`p`／`br`、`ul`／`ol`／`li`、`strong`／`em`（`b`／`i` との違い）、属性の書き方、`a`（`href`・相対パスと絶対パス）、`img`（`src`／`alt`）、`id` と `class`、表（`table`／`thead`／`tbody`／`tr`／`th`／`td`）、フォーム（`form`／`input` の各 `type`／`label`／`textarea`／`select`／`button`）、`div` と `span`、意味を持つ構造タグ（`header`／`main`／`footer`／`section`／`article`／`nav`） | 閉じタグ忘れ、パスの相対指定、日本語の文字化け |
| 3 | CSS | `<link>` での読み込み、ルールの構造（セレクタ・プロパティ・値）、セレクタ（要素／class／id／子孫／複数指定）、詳細度、ボックスモデル（`padding`／`border`／`margin`、`width` と `box-sizing: border-box`、`margin` の相殺）、色（キーワード・`#rrggbb`・`rgb()`）、フォント（`font-family`／`font-size`／`font-weight`、`rem`）、`line-height`／`text-align`、`border-radius`、`:hover`、`display`（`block`／`inline`／`inline-block`／`none`）、Flexbox（`display: flex`／`flex-direction`／`justify-content`／`align-items`／`flex-wrap`／`gap`／`flex-grow`／`flex-shrink`）、Grid の入口（`grid-template-columns`）、viewport の `meta`、メディアクエリ（`@media (max-width: 768px)`）、CSS が効かないときの調べ方 | CSS が効かない（読み込み・詳細度・キャッシュ）、`margin` の相殺 |
| 4 | JavaScript 基礎（前半） | `<script>` の読み込み、`console.log`、エラーの読み方、変数（`let`/`const`、`var` は使わない）、数値・文字列・真偽値・`undefined`/`null`、`typeof`、算術演算子と `%`、`Math.floor`/`ceil`/`round`、テンプレートリテラル、`===`/`!==`、`&&`/`\|\|`/`!`、`Number()`、`if`/`else if`/`else`、三項演算子、`switch`、`for`/`while`、`break`/`continue`、関数（`function`・アロー関数・引数・戻り値）、スコープ | `=` と `==` と `===`、スコープ、`i` の意味、小数の誤差、`return` を書かず `undefined` になる、無限ループ |
| 5 | JavaScript 基礎（後半） | 配列（`push`/`pop`/`unshift`/`shift`/`includes`/`indexOf`/`join`/`slice`/`concat`/`for...of`）、オブジェクト（読み書き・入れ子・オプショナルチェーン`?.`）、`map`/`filter`/`find`/`reduce`/`sort`とチェーン、分割代入、スプレッド構文、イミュータブルな更新、非同期処理（`setTimeout`/`Promise`/`async`/`await`）、`fetch`とエラー処理、`export`/`import`（名前付き・デフォルト）、`type="module"`、DOM 操作（`querySelector`/`textContent`/`classList`/`addEventListener`/`createElement`/`appendChild`/`remove`） | `TypeError: Cannot read properties of undefined`、`const copy = original` が複製にならない、`reduce` の初期値省略、`setTimeout` が待ってくれると誤解する、モジュールを `file://` で直接開いて動かない |
| 6 | React をはじめる | 命令的と宣言的の違い、Vite（`npm create vite@latest ... -- --template react`／`npm install`／`npm run dev`／Ctrl+C）、プロジェクト構成（`src`／`index.html`／`main.jsx`／`App.jsx`／`package.json`／`node_modules`／`public`）、`createRoot`と`StrictMode`（存在のみ）、CSS を `import` で読み込む、JSX（1要素ルール、`className`、閉じタグ必須、キャメルケース属性、`{/* */}`）、`{ }` での式の埋め込み（変数・計算・三項演算子。`if`/`for` は書けない）、属性への値渡しと `style={{ }}`、フラグメント `<>`、コンポーネント（作成・`export default`/`import` でのファイル分割・分け方の基準・大文字始まりの命名） | Node バージョン非互換（Vite は Node 20.19+／22.12+ が必要）、ポート 5173 の衝突、`Missing script: "dev"`（プロジェクト外で実行）、JSX の1要素ルール、`class` と書いて CSS が効かない、オブジェクトを `{ }` に直接入れる、`style` の波かっこ1つ、コンポーネント名を小文字で始めて何も表示されない、`import` パスに `./` を付け忘れる |
| 7 | props と state | props（分割代入での受け取り・デフォルト値・型ごとの渡し方・`children`・書き換え禁止）、`useState`（初期値・`set○○`・再レンダリング・フックのルール）、イミュータブルな state 更新（配列・オブジェクト）、関数形式の更新 `set○○((prev) => ...)`、イベント（`onClick`／`onChange`／`onSubmit`、`handle○○` の命名、引数を渡すアロー関数、`event.target.value`）、`map` での一覧表示と `key`（index を避ける理由）、`Date.now()` での id 生成、条件表示（`&&`／三項演算子／早期 `return`／`null` を返す）、制御コンポーネント（`value`＋`onChange`、`name` 属性と `[event.target.name]`、`checked`、`event.preventDefault()`）、文字列の `includes` での絞り込み | state を直接書き換えて画面が変わらない（`push`）、`onClick={fn()}` と書いて `Too many re-renders`、`key` 無し警告と index による行のずれ、`{items.length && ...}` で `0` が出る、`value` だけ書いて入力できない、`preventDefault` 忘れでページが再読み込みされる |
| 8 | 状態設計と副作用 | 状態のリフトアップ（共通の親に state を上げる・更新関数を props で渡す・`on〜`／`handle〜` の命名・state の置き場所の決め方）、派生した値は state にせず計算する、`useEffect`（副作用の考え方・依存配列の3通り・無限ループ・クリーンアップ関数・`<StrictMode>` による二重実行・使うべきでない場面）、`fetch` によるデータ取得（`useEffect` 内で `async function` を定義して呼ぶ・`response.ok` の確認・data／isLoading／errorMessage の3 state・早期 `return` での出し分け・依存配列を使った再取得・CORS の考え方）、`useRef`（DOM 操作と、再レンダリングされない値の保持）、`useMemo`／`useCallback`／`memo`、`console.time` での計測、カスタムフック（`use` で始まる関数・`useFetch`・フックのルール） | `useEffect` の無限ループ、依存配列の書き忘れ／オブジェクトを入れてしまう、クリーンアップ忘れでタイマーが残る、`useEffect(async () => ...)` と書く、`fetch` が 404 を失敗にしないこと、`data` の初期値が `null` のまま `map` を呼ぶ、`ref.current` の書き忘れ、早期 `return` のあとにフックを呼ぶ |
| 9 | ルーティングと全体設計 | SPA の考え方、React Router（`BrowserRouter`／`Routes`／`Route`／`element` に渡すもの・`Link`／`NavLink` と `<a>` の違い・`useParams` と URL パラメータ（値は文字列）・`useNavigate`・`Outlet` によるネストしたルートと共通レイアウト・`index` ルート・`path="*"` の 404・`useLocation`）、Context（`createContext`／`Provider`／`useContext`・値と更新関数をまとめて流す・props のバケツリレー・使いどころの判断）、状態管理ライブラリの位置づけ（使わない）、ディレクトリ構成（種類で分ける `pages`／`components`／`hooks`／`contexts`／`data`・命名規則）、エラーバウンダリ（`react-error-boundary`・`FallbackComponent`・`resetKeys`・受け止めない範囲）、共通の `Loading`／`ErrorMessage` 部品、props の初期値、`useFetch` への `reload` の追加、画面の4状態（読み込み中／エラー／0件／表示） | 子ルートの `path` に `/` を付ける、`element={HomePage}` と書く、`<a href>` で state が消える、`useParams` の値が文字列で `===` が成立しない、`onClick={navigate('/')}` で即移動、Provider の外で `useContext` して `null`、Context に入力中の値を入れる、`import` パスの直し忘れ |
| 10 | 実践：タスク管理アプリ | 上記すべての統合に加えて、設計の手順（完成イメージ→機能一覧→MVP の切り出し→画面を描く→コンポーネント分解→データの形→state の置き場所）、派生値を state にしない判断（10.2.4）、`Date.now()` による id、配列 state の更新パターン（追加は `[...tasks, newTask]`／1件変更は `map` + スプレッド／削除は `filter`）、`[...配列].sort()`、`localeCompare(相手, 'ja')`、対応表オブジェクトと `オブジェクト[変数]` での参照、`localStorage`（`setItem`／`getItem`／`removeItem`・保存できるのは文字列だけ）、`JSON.stringify` / `JSON.parse`、読み込み前の上書きを防ぐガード、`useLocalStorage` カスタムフック、0件表示の出し分け（未登録／絞り込み結果0件）、`trim()`、`disabled` 属性、エラー時の早期 return | 設計の分解ができない、`push` で更新して画面が変わらない、`sort` が元の配列を書き換える、`localStorage` にオブジェクトを直接入れて `[object Object]` になる、保存の `useEffect` が読み込みより先に走って初期値で上書きされる、`Date.now()` の id と `useParams` の文字列を `===` で比較する |
| 11 | 次のステップ | 上記すべてに加えて、TypeScript の基礎（`.tsx`／型注釈 `: string` `: number` `: boolean`／型推論／関数の引数と戻り値の型／`type` によるオブジェクトの型／props の型 `({ ... }: Props)`／`(引数: 型) => void`／`useState<Task[]>`／`npm create vite@latest ... -- --template react-ts`／`tsc -b` による型チェック）、テスト（Vitest のインストール・`test`／`expect().toBe()`／`toEqual()`・`npm test`・値を返す関数に切り出してからテストする）、ビルド（`npm run build`／`dist` の中身／`npm run preview`（4173））、デプロイ（Netlify Drop／Vercel + GitHub の自動デプロイ）、Git（`git init`／`status`／`add`／`commit -m`／`push`／`restore`／`log --oneline`／`remote add origin`／`.gitignore`） | `dist/index.html` を `file://` で開いて真っ白になる、`npm run preview` を 5173 で開く、`node_modules` をコミットしてしまう、GitHub 側に README を作って `git push` が拒否される、`import` パスの大文字小文字の違いが公開環境でだけ失敗する、型エラーを放置したまま `npm run dev` で進めてビルドで詰まる、`localStorage` のデータが公開先で共有されると誤解する |
| 12 | 解答編 | — | — |

> **注意**：1 章の学習者には、HTML のタグはまだ説明しないでください（2 章の内容）。
> 1 章で書くのは、動作確認のための最小限の `index.html` だけです。
> 2 章の学習者に CSS を書かせないでください（3 章の内容）。見た目の相談は「3 章で扱う」と伝えてください。
> 3 章の学習者に JavaScript を書かせないでください（4 章の内容）。
> 4 章の学習者に `map` を使ったコードを見せないでください（5 章の内容）。
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

> **この本は全章（第0章〜第11章＋解答編）が完成しています。**
> 学習者が章番号を伝えてきたら、その章までの累積範囲だけで答えてください。

| 章 | タイトル | 既習範囲（累積） | よくあるつまずき |
|----|---------|----------------|----------------|
| 0 | はじめに | （コードなし）AI サポートの準備、JavaScript / Python 対応表（`let`/`const`→代入のみ、`===`→`==`、`{}`→インデント、配列→リスト、オブジェクト→辞書、`map`→内包表記、`null`/`undefined`→`None` の「地図」。詳細は各章で扱う） | AI の準備をせずに進んでしまう、react-text を読んでいないまま 0.3 の対応表で立ち止まる |
| 1 | Python の環境構築 | Python 3.13 のインストール、`python`（Windows）/ `python3`（macOS）の呼び分け、`--version` での確認、`py` ランチャー（Windows）、REPL（`>>>`・`exit()`）、`.py` ファイルの実行（`python ファイル名`）、`print`（存在のみ。詳細は 2.5）、トレースバックの読み方（下から上・種類・行番号・`NameError`）、仮想環境 venv（`python -m venv .venv`・有効化 `Activate.ps1` / `source .venv/bin/activate`・`deactivate`）、実行ポリシー（`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`）、pip（`pip install`・`pip list`・`pip freeze > requirements.txt`・`-r`）、`import`（存在のみ。詳細は第6章）、VS Code の Python 拡張・インタプリタ選択・▷ 実行・ブレークポイントでのステップ実行 | **「Add Python to PATH」の入れ忘れ**、PowerShell の実行ポリシーで venv を有効化できない、`>>>` が出たままターミナルのコマンドを打つ、仮想環境を有効化せずに `ModuleNotFoundError`、コードへの全角スペースの混入 |
| 2 | 基本文法 | 変数と代入（`=`・再代入・`NameError`）、`let`/`const` がないこと、snake_case・予約語、定数の慣習（UPPER_SNAKE_CASE）、型（`int` / `float` / `str` / `bool` / `NoneType`）、`True`/`False`/`None`（先頭大文字）、`type()`、型変換（`int()` / `float()` / `str()`・`ValueError`・`int("08")` → `8`）、浮動小数点の誤差、桁区切り `1_000`、算術演算子（`+ - * / // % **`・`round()`・`ZeroDivisionError`）、代入演算子（`+=` など。`++` はない）、比較演算子（`==` / `!=` / `<=` など。`===` はない）、論理演算子（`and` / `or` / `not`）、文字列（連結 `+`・繰り返し `*`・`len()`・インデックス（0 始まり・負の添字）・スライス `[開始:終了]`・イミュータブル・`IndexError`）、文字列メソッド（`strip` / `upper` / `lower` / `replace` / `startswith` / `endswith` / `count` / `zfill`）、f-string（`{}`・書式指定 `:,` `:.2f` `:>5` `:05`）、エスケープ（`\n` `\t` `\\` `\"`）・複数行文字列 `"""`・raw string `r"..."`、`print`（複数引数・`sep`・`end`・空行）、`input`（戻り値は必ず `str`・`int(input(...))`）、インデント（`:` とブロック・半角スペース4つ・入れ子・`IndentationError` / `TabError`・VS Code の空白可視化設定）、コメント `#` とコメントアウト、PEP 8、Black による自動整形 | `=` と `==` の混同、全角の記号・スペース・`”` の混入、`True` / `False` を小文字で書く、`input` の戻り値が文字列であることの見落とし、タブとスペースの混在（`IndentationError` / `TabError`）、`int("abc")` の `ValueError` |
| 3 | 制御構文 | 条件分岐（`if` / `elif` / `else`・`:` と字下げ・`( )` が不要なこと・`SyntaxError: expected ':'`・`elif` は上から順に最初の1つだけ・範囲は狭い条件から書く・`if` を並べた場合との違い）、条件の組み合わせ（`and` / `or` の優先順位とかっこ・比較の連鎖 `0 <= x <= 100`）、`in` / `not in`（文字列・リスト。`== ... or ...` の置き換え）、真偽値として扱われる値（偽になるのは `False` / `0` / `0.0` / `""` / `[]` / `None` だけ・`bool()`・`if name:` での空入力判定・`== True` と書かない・数値の 0 の落とし穴）、条件式 `A if 条件 else B`（JS とは順番が逆）、`for`（文字列・リストを回す・ループ変数・入れ子・JS の `for...of` に相当）、`range`（`range(終了)` / `range(開始, 終了)` / `range(開始, 終了, 増分)`・終了を含まない・逆順・off-by-one）、累積パターン（`total = 0` をループの外で用意し `total += x`）、`enumerate`（`start=1`・変数を2つ書く。アンパックは第4章）、`while`（条件が真のあいだ・変数は自分で進める・`for` との使い分け）、無限ループと `Ctrl` + `C`（`KeyboardInterrupt`・VS Code のターミナル）、`break`（`while True:` + `break`・入れ子では内側だけ抜ける）、`continue`（不正入力の読み飛ばし・`continue` の位置で何が飛ぶか変わる）、`str.isdigit()`、`for ... else`（`break` されなかったとき・フラグ変数を使う書き方との比較）、`pass`（空ブロックはエラー・コメントは処理として数えない）、ネストの解消（字下げ3段が見直しの合図・ガード節＝早期 `continue`・条件の反転表・`and` と `or` が入れ替わること・`elif` で平らにする） | `:` の書き忘れ（`SyntaxError: expected ':'`）、`elif` で広い条件を先に書いて後ろに到達しない、`== True` と書く、`range(1, 10)` に 10 が含まれると思う（off-by-one）、`while` の変数を進め忘れて無限ループ、入れ子の `break` が内側しか抜けない、`for ... else` を「ループが終わったら実行」と読む |
| 4 | データ構造 | リスト（`[ ]`・`len()`・`sum()` / `max()` / `min()`・インデックスと負の添字・`IndexError`・要素の書き換え＝ミュータブル・スライス（新しいリストを返す・`[::2]` / `[::-1]`）・`append` / `insert` / `remove` / `pop`（`pop` 以外は戻り値なし・`ValueError: list.remove(x): x not in list`・`IndexError: pop from empty list`）・`sort()` と `sorted()`（破壊的か否か・`reverse=True`・`sort()` の戻り値は `None`・漢字は文字コード順・`key` は第5章）・コピー（`b = a` は同じ実体・`copy()` / `[:]` / `list()`）、タプル（`( )`・変更不可 `TypeError: 'tuple' object does not support item assignment`・要素1つは `(1,)`・定数の組・アンパック `x, y = point`・`ValueError: not enough values to unpack`・`a, b = b, a`）、辞書（`{"キー": 値}`・`[ ]` での読み書き・`KeyError`・代入で追加／上書き・`del`・`in` は**キーだけ**を調べる・`get(キー)` / `get(キー, 既定値)`・`counts[x] = counts.get(x, 0) + 1` の数える型・`for` はキーが回る・`keys()` / `values()` / `items()`・`sorted(辞書)` はキーのリスト・**辞書のリスト**と集計3型（合計／絞り込み／最大は1件目を仮の答えにする）・f-string 内は引用符を変える）、集合（`{1, 2}`・**空集合は `set()`**・順番なし・`[ ]` で取り出せない・`\|` / `&` / `-`・`set(リスト)` で重複除去・`sorted()` で表示順を決める・順序を保つ重複除去は `not in` + `append`）、内包表記（`[式 for 変数 in 元]`・絞り込みの `if` は**うしろ**・値の出し分けの `A if 条件 else B` は**前**・辞書内包表記 `{k: v for ...}`・`items()` との組み合わせ・読みにくければ `for` に戻す）、使い分けの比較表と判断フロー | 「1番目」を `[1]` と書く、`numbers = numbers.sort()` でリストが `None` になる、`append` の戻り値を代入する、`b = a` がコピーにならない、要素1つのタプルの `,` 忘れ、辞書の `in` が値も調べると思う、空集合を `{}` と書く、内包表記の `if` を前に書く |
| 5 | 関数 | 関数の定義（`def 名前():`・`:` と字下げ・空行2つの慣習・定義しただけでは動かない・呼び出しは定義より後（`NameError`）・`()` を書き忘れると何も起きない）、**`print` と `return` の違い**（`return` のない関数は `None` を返す・`return` を実行した時点で関数が終わる・戻り値をそのまま式に使える）、引数（位置引数・順番違いはエラーにならない・`TypeError: ... missing 1 required positional argument`・キーワード引数・`SyntaxError: positional argument follows keyword argument`・デフォルト引数と「省略できるものは後ろ」・**デフォルト引数にリスト／辞書を書かない**（定義時に1回だけ作られて使い回される・`items=None` + `if items is None: items = []`）・**`is None` による判定**・可変長引数 `*args`（タプル）と `**kwargs`（辞書））、戻り値（`if` で返り値を出し分ける・リストを返す関数・`return a, b` はタプル→アンパックで受け取る・返す値が増えるなら辞書1つを返す・**早期 `return`**（ガード節。`ZeroDivisionError` を先に弾く・`return` だけ書くと `None`））、スコープ（ローカル／グローバル・関数の中の変数は外から見えない（`NameError`）・外の定数は中から読める・代入しようとすると `UnboundLocalError`・**引数で受け取り `return` で返す**形にする・ただしリスト／辞書は中身を変えられる（同じ実体）・`global` は使わない）、関数を値として扱う（`()` を付けなければ関数そのもの・`f = double`・ラムダ式 `lambda 引数: 式`（`return` を書かない・式1つだけ・名前を付けるなら `def`）・**`sorted(データ, key=lambda x: x["キー"])` / `key=len` / `reverse=True`**・`max()` / `min()` にも `key` を渡せる）、良い関数（1つの関数は1つのこと・計算する関数と表示を分ける・docstring `"""..."""` と `help()`・切り出しの3つの合図） | 呼び出しの `()` を書き忘れて何も起きない、`return` の書き忘れで `None` が返る、デフォルト引数にリストを書いて使い回される、`UnboundLocalError`、関数に渡したリストが書き換わる、`key` に `()` を付けて渡す |
| 6 | モジュールとパッケージ | モジュール＝1つの `.py` ファイル（自作モジュールの作成と読み込み・`import` に `.py` は書かない・関数だけでなく変数も取り出せる）、`import モジュール`（`モジュール名.関数名()` の形。`with_tax()` だけで呼ぶと `NameError`）、`from モジュール import 名前`（モジュール名は取り込まれない）、`as` による別名（ぶつかったときと慣習の短縮名だけ）、**`from ... import *` は使わない**（同じ名前が静かに上書きされる実例）、モジュールの探索順（**実行した `.py` の場所** → 標準ライブラリ → pip。`cd` した場所ではない・`sys.path[0]` で確認・`ModuleNotFoundError` の切り分け3手順・`random.py` などの名前を自作モジュールに付けない・`__pycache__`）、標準ライブラリ（インストール不要）：`datetime`（`date` / `datetime` / `timedelta`・`.year` などの属性は `()` なし・日付の加減算・`.days`・`strftime` と `%Y %m %d %H %M`・`date.today()`・`weekday()` は月曜が 0）、`random`（`randint` は**上限を含む**・`choice` / `sample` / `shuffle`（破壊的・戻り値 `None`）・`seed`・パスワード用途には使わない）、`math`（`floor` / `ceil` / `sqrt` / `pi`・**`round(2.5)` は `2`**（偶数丸め）・`int()` との違い）、`collections`（`Counter`（キーがなくても `0`・`most_common(n)` はタプルのリスト）・`defaultdict(list)`（`()` を付けずに関数を渡す）・`", ".join(リスト)`（中身は文字列だけ））、`import` は読み込んだファイルを**上から下まで実行する**、`__name__`（直接実行なら `"__main__"`、`import` ならモジュール名）と `if __name__ == "__main__":`、`__doc__`、パッケージ（ディレクトリ＋`__init__.py`・`shop.taxes` のドット表記・`__init__.py` を窓口にする）、絶対 import と相対 import（`.` は同じディレクトリ・**このテキストは絶対 import を基本**・`ImportError: attempted relative import with no known parent package`） | `import price_utils.py` と書く、`from ... import` のあとにモジュール名を付けて呼ぶ、自作モジュールに `random.py` と名前を付ける、探す基準が「実行した `.py` の場所」だと知らない、`shuffle` の戻り値を代入して `None` になる、`round(2.5)` が `2`、`join` に数値のリストを渡す、`if __name__ == "__main__":` の書き間違いが静かに失敗する |
| 7 | ファイル操作と例外 | ファイルを読む（`open(パス, encoding="utf-8")`・**探す基準はターミナルの現在地**（`import` とは逆）・`f.read()` は1本の文字列・`f.close()`・`for line in f:` は改行が付いたまま（`rstrip()`）・`read().splitlines()`（改行なしのリスト）・`readlines()`（改行あり））、**`with open(...) as f:`**（ブロックを抜けると自動で閉じる・`f.closed`・`ValueError: I/O operation on closed file.`）、**`encoding="utf-8"` を必ず書く**（文字コード＝文字と番号の対応表・UTF-8 と cp932・`UnicodeDecodeError`・エラーにならず文字化けする場合もある）、モード（`"r"` / `"w"`（**開いた時点で中身が消える**）/ `"a"`（追記）/ `"x"`（`FileExistsError`）・`f.write` の戻り値は文字数）、**`write` は改行を付けない**（`+ "\n"` か `"\n".join(...) + "\n"`）、`repr()`、改行の OS 差（`\r\n` / `\n`。テキストモードが自動変換する）、`pathlib`（`Path`・`.name` / `.stem` / `.suffix` / `.parent`（属性なので `()` なし）・`read_text` / `write_text`・**`/` でパスをつなぐ**（両方が文字列だと `TypeError`）・`Path.cwd()`・`exists` / `is_file` / `is_dir`・`mkdir(exist_ok=True)` / `parents=True`・`iterdir` / `glob("*.txt")`（順番は不定なので `sorted()` で囲む）・`unlink` は使わない）、Windows のパス（`\` はエスケープ記号・raw string `r"..."`・`Path` で組み立てる・`/` 区切りも動く・**絶対パスをコードに書かない**）、CSV（`split(",")` では引用符内のカンマを扱えない・`csv.reader` と `next()`・**`csv.DictReader` で辞書のリストに**・**`newline=""` を付ける**（Windows で空行が入る）・**値はすべて文字列なので `int()` する**・`csv.DictWriter` と `fieldnames` / `writeheader` / `writerow`）、JSON（Python の辞書との対応表（`true`/`false`/`null`・キーはダブルクォート）・`json.load` / `loads` / `dump` / `dumps`（**`s` は文字列の `s`**）・`json.JSONDecodeError`（末尾カンマ・コメント不可）・**`ensure_ascii=False` と `indent=2`**・`encoding="utf-8"` とセット・「集計 → 辞書のリストに組み立て → `sorted` → `json.dump`」の5段階）、例外（**プログラムの間違いと外の世界の事情を区別する**・例外の一覧表・`try` / `except 種類:`・`as e` で `print(e)`・`isdigit()` より `try` のほうが確実・`except` は上から順に最初の1つ・タプルでまとめる・**広い例外を先に書かない**・`else`（例外が起きなかったときだけ）・`finally`（`return` があっても必ず）・**`except:` と裸で書かない / `pass` で終わらせない / それらしい値を返して隠さない**）、`raise 例外("メッセージ")`（ガード節の形・`return 0` との比較・対処は呼び出し側が決める・迷ったら `ValueError`）、`class 名前(Exception):`（**例外の種類を増やすための決まった書き方**として導入。`class` と継承の詳細は第8章）、**例外は捕まえられなければ呼び出し元へ戻る**（部品の関数では捕まえない・`main` など「どうするか決められる場所」で捕まえる） | `FileNotFoundError`（原因はターミナルの現在地）、`"w"` で開いて中身が消える、`with` のブロックの外で `read()`、`encoding="utf-8"` の書き忘れ（`UnicodeDecodeError` や文字化け）、`newline=""` 忘れで CSV に空行が入る、`write` が改行を付けないことの見落とし、広い例外を先に書く、`except: pass` で握りつぶす |
| 8 | オブジェクト指向 | クラスが必要になる理由（辞書＋関数の3つの問題：関係ない辞書を渡しても止まらない・キー名の打ち間違いが離れた場所で `KeyError` になる・必須項目を保証できない）、`class 名前:`（`UpperCamelCase`・中身は空にできない（`pass`）・`名前()` でインスタンスを作る・`type()` は `<class '__main__.Product'>`・**クラスを定義するとは新しい型を作ること**）、属性（`インスタンス.名前` で読み書き・辞書との対応表・`AttributeError`・インスタンスは別々の実物（`is` が `False`））、設計図と実物のたとえ、**`__init__`**（作るときに自動で呼ばれる・引数の数が違うと `TypeError: ... missing 1 required positional argument`・**代入に `self.` を付け忘れるとローカル変数になる**）、**`self`**（そのインスタンス自身・`note.f()` は `Product.f(note)` と同じ・予約語ではないが慣習・**書き忘れると `takes 0 positional arguments but 1 was given`**）、メソッド（第1引数は `self`・呼び出し側は `self` を数えない・メソッドから `self.別のメソッド()` を呼べる・`raise` をそのまま書ける・`str` / `list` のメソッドも同じ仕組み）、クラス変数とインスタンス変数（`UPPER_SNAKE_CASE`・クラス名からもインスタンスからも読める・**属性の探索順は「インスタンス → クラス → 親クラス」**・`self.count += 1` は読みがクラス変数・書きがインスタンス変数・**クラス変数にリスト／辞書を置かない**（5.2.4 と同じ共有事故）＝ `__init__` の中で作る）、継承（`class 子(親):`・親の属性とメソッドをすべて引き継ぐ・**親クラス／子クラス**・`isinstance`・クラス変数1行の上書きだけで動きが変わる・**オーバーライド**（同名メソッドは子が優先）・**違うクラスを同じ `for` で回せる＝呼び出し側から `if` が消える**）、**`super()`**（`super().__init__(...)` を最初に書く・`self` は渡さない・`super().メソッド名()` はどのメソッドにも使える（`__str__` も）・**忘れると `AttributeError`**）、継承を使いすぎない（**「子は親の一種である」と言えるか**・`class Cart(list)` は誤り・違いが値だけなら引数（デフォルト引数）にする・階層は1段まで・`class ConfigError(Exception):`（7.6.2）はこの継承だった）、特殊メソッド（`__str__`（利用者向け・`print` / f-string）と `__repr__`（開発者向け・**リストや辞書の中身の表示**）・どちらか1つなら `__repr__`・必ず文字列を `return`（`print` すると `TypeError: __str__ returned non-string`）・`__eq__`（引数は `self` と `other`・**まず `isinstance` で弾く**・`==` と `in` の判定が変わる・定義すると `set` に入れられない（`unhashable`））・`__len__`（`len()` と `if x:` の判定）・`__contains__`（`in`）・**アンダースコア2つの名前を自作しない**）、**`@dataclass`**（`from dataclasses import dataclass, field`・**デコレータ**＝下のクラスに機能を足す印・`__init__` / `__repr__` / `__eq__` を自動生成・**項目には型の注記が必須**（`name: str`。詳細は第9章）・メソッドは普通に書ける・デフォルト値・**リストは `field(default_factory=list)`**（`ValueError: mutable default ... use default_factory`）・**既定値のある項目は後ろ**（`TypeError: non-default argument follows default argument`）・**JSON → インスタンスのリスト → `sorted(key=lambda p: p.メソッド())` → 表示 → 辞書に戻して `json.dump`** の流れ・`json.dump` は自作クラスを書き出せない（`TypeError: Object of type ... is not JSON serializable`）ので辞書に変換する）、いつクラスを使うか（**`self.` が出てこないメソッドしかないなら関数でよい**・状態と振る舞いがセットならクラスにする・同じデータを引数で渡し続けているのが合図・判断フローチャートとチェックリスト・**まず関数と辞書で書いて、困ってから直してよい**） | `__init__` の中で `self.` を書き忘れる、メソッドの第1引数 `self` の書き忘れ（`takes 0 positional arguments but 1 was given`）、クラス変数にリストを置いて全インスタンスで共有される、`super().__init__(...)` の忘れ、`__str__` だけ定義してリストの表示に効かない、`field(default_factory=list)` を使わない |
| 9 | 型ヒントとモダン Python | 型ヒント（型がないことの3つの問題：引数の順番違いが静かに通る・文字列を渡すと離れた場所で `TypeError`・呼ぶ側が何を渡すか分からない）、書き方（変数・引数は `名前: 型`、デフォルト値は型が先で `=` が後、戻り値は `-> 型`、**何も返さない関数は `-> None`**）、`int` / `float` / `str` / `bool`、**型ヒントは実行時に強制されない**（`repeat(5)` が `15` を返す・`__annotations__` に記録されるだけ・読むのは型チェッカー / エディタ / 人間の3者）、中身のある型（`list[str]` / `dict[str, int]` / `tuple[int, int]` / `tuple[str, ...]` / **`list[dict[str, str]]`**（`csv.DictReader` の形）・`List[str]` は 3.8 以前の書き方）、**`型 \| None`**（`Optional[型]` は同じ意味・使う前に `is None` で確認する・`[union-attr]` と `[operator]` の報告・`items: list[str] \| None = None` が 5.2.4 の正しい形）、`int \| str` と `isinstance` での絞り込み（`Union[int, str]` は同じ意味・候補を増やしすぎない）、自作クラスの型（`list[Product]` / `-> Product \| None`・**クラスの中で自分自身を書くときだけ `-> "Product"` と引用符**（`NameError` を避ける））、型チェッカー（**静的型チェック**・mypy はターミナル、pyright / Pylance はエディタ）、`pip install mypy` と `mypy ファイル名`（`ファイル名:行番号: error: 内容 [ルール名]` の4部構成・`Success: no issues found`・**型ヒントのない関数の中身は既定では検査されない**・`disallow_untyped_defs = true` で書き忘れも報告（`[no-untyped-def]`）・**`# type: ignore` で黙らせない**・`[import-untyped]` は `ignore_missing_imports` で消す・`python-lesson` では `mypy .` を使わずファイル名を1つ指定する）、VS Code の `"python.analysis.typeCheckingMode": "basic"`、**リンタ**と**フォーマッタ**の区別、ruff（`ruff check`（報告）/ `ruff check --fix`（自動修正）/ `ruff format`（整形）・`F401` 未使用 import・`I001` import の並び順・報告は「ルール名・内容・`--> ファイル:行:桁`・help」・**Black の置き換え**）、**`pyproject.toml`**（TOML の読み方・`[tool.ruff]` の `line-length` / `target-version`・`[tool.ruff.lint]` の `select = ["E", "F", "I", "UP"]`・`[tool.mypy]` の `python_version` / `ignore_missing_imports` / `disallow_untyped_defs`）、VS Code の保存時自動整形（Ruff 拡張機能 `charliermarsh.ruff` / `source.fixAll.ruff` / `source.organizeImports.ruff`）、**空のリストに代入するときは型注記が要る**（`[var-annotated]`）、venv + pip のつらさ（有効化忘れ・`pip freeze` が直接入れたものと依存を区別しない・遅い）、uv の紹介（`uv venv` / `uv pip install` / `uv run` / `uv.lock`。**入れなくても読み進められる**）、道具の選び方（このテキストは venv + pip のまま・チームに合わせる・ruff と mypy は入れる価値がある） | 「型ヒントを書いたのに間違った型でエラーにならない」（それが正しい動作）、変数の型ヒントを `=` のうしろに書く、`# type: ignore` で報告を消す、`python-lesson` で `mypy .` を実行して大量に報告される、`mypy` / `ruff` が `command not found`（仮想環境の有効化忘れ） |
| 10 | 実践：データ処理スクリプト | 作るものを先に決める進め方（完成イメージ → 処理の流れを日本語で分解 → 一方向のデータの流れ → 動く小さなものを少しずつ育てる）、**新しいプロジェクト `sales-analyzer`**（`python-lesson` とは別。`.venv` を作り直し、`pip install requests ruff mypy types-requests`）、CSV の1行を `@dataclass` で受ける形（`csv.DictReader` の行から `Sale(date=..., quantity=int(...))` をキーワード引数で組み立てる・読み込み時に `int()` して以降は変換を考えない）、**日付の文字列比較**（`YYYY-MM-DD` は桁が固定で大きい単位が左にあるので `<` / `>` がそのまま使える。`2024/7/4` の形では使えない）、早期 `continue` を並べた絞り込み、`shop: str \| None = None`（`None` ＝ 絞り込まない）、**`defaultdict(int)`** による集計（`defaultdict[str, int]` の型注記・**返すときは `dict()` に戻す**（打ち間違いを `KeyError` にするため））、`sorted(辞書)` と `sorted(辞書.items(), key=lambda pair: pair[1], reverse=True)`、**`requests`**（外部ライブラリ・API とは・`requests.get(URL, params=辞書, timeout=10)`・**`timeout` は必ず書く**・`params` の値は `str()` で文字列にそろえる・`response.status_code`（200 / 400 / 404 / 500）・`response.json()` は `json.loads` と同じ・**`raise_for_status()` を書かないと 404 / 500 が成功として通る**）、**2本の並行リストを辞書に組み立てる**（`enumerate` で番号を取り、もう一方のリストを同じ番号で引く）、通信の失敗（`requests.RequestException` が `ConnectionError` / `Timeout` / `HTTPError` の親・`for attempt in range(1, RETRY_COUNT + 1)` によるリトライ・`time.sleep`・**最後の失敗のあとは待たない**・`except` の中の `continue` 忘れ・**取得できなければ `None` を返して処理を続ける**）、書き出し（**組み立てる関数とファイルに書く関数を分ける**・すべての値を `str()` にそろえて `list[dict[str, str]]` にする・`csv.DictWriter` と `fieldnames` / `writeheader`・`newline=""`・`path.parent.mkdir(parents=True, exist_ok=True)`・**`strftime("%Y%m%d")` でファイル名に日付を入れる**・`Path` は `/` でつなぐ）、**`argparse`**（`ArgumentParser(description=..., epilog=...)`・位置引数とオプション引数・**`type=int` を書かないと文字列のまま**・`default`（省略時は `None`）・`metavar`・`help`・**`action="store_true"`**・`args.no_weather`（`-` は `_` に変わる）・`parse_args()`・足りない／型違いを argparse が検査する・`--help` の自動生成・`for _ in range(...)` の `_`）、`if not target:` による早期 `return`、`main` は流れを並べるだけにする関数分割（11関数の呼び出し図）、`pyproject.toml` に `disallow_untyped_defs = true` を追加、**`types-requests`**（`requests` の型情報だけを配ったパッケージ）、動作確認のチェックリスト（うまくいく道と、いかない道の両方を試す）、**捕まえる例外と捕まえない例外の分け方**（そのあとどうするか決められるかどうか） | ターミナルの現在地が `sales-analyzer` になっていない、`raise_for_status()` を書かず 404 / 500 が成功として通る、`timeout` の書き忘れ、`except` の中の `continue` 忘れで集計に落ちる、`Path` と文字列を `+` でつなぐ、`type=int` を書かず文字列のまま比較する、`key=lambda pair: pair[1]` の添字違い |
| 11 | 次のステップ | 上記すべてに加えて、到達度チェックリスト（27項目・戻る場所つき）、JavaScript / Python 対応表の答え合わせ（`try/catch`→`try/except`、`throw`→`raise`、オブジェクトリテラル→`@dataclass`、TypeScript の `type`→型ヒント、`tsc`→`mypy`、`npm install`→`pip install`）、**pytest の入口**（`pip install pytest`・`test_` で始まるファイル名と関数名という発見規則・`assert` 文と `AssertionError`・失敗時の出力の読み方・テスト用データを関数で作る・境界（0件・端の日付）を試す・`import` しても `main()` が動かないのは `if __name__ == "__main__":` のおかげ・テストしやすい関数（値を渡すと値が返る）としにくい関数（ファイル・通信・実行日時）の区別・`pytest -q`・`pytest ファイル::関数名`）、**pandas の入口**（`import pandas as pd`・`read_csv`（列ごとに型を推測するので `int()` 変換が不要）・DataFrame と Series・`head()`・列どうしの計算で列を足す・`groupby("列")[列].sum()`・`df[df["列"] == 値]` による絞り込み・`sort_values(ascending=False)`・`Series.sum()`・向き不向き）、**自動化**（タスクスケジューラ（Windows）と cron（macOS）の登録手順・仮想環境を有効化せず `.venv` の Python を直接指す・絶対パス・`>> run.log 2>&1` でのログ・DRY RUN の考え方・`pathlib` での仕分けスクリプト（`iterdir` / `is_file` / `is_dir` / `suffix` / `lstrip` / `lower` / `rename` / `exists` / `Path.home()`）・`list[tuple[Path, Path]]`）、次の本（FastAPI）へ渡す動機づけ | `pytest` が `collected 0 items`（`test_` の命名規則から外れている）、`ModuleNotFoundError`（ターミナルの現在地）、ファイルを動かすスクリプトを DRY RUN せずに実行する、対応表に載っているものほど細部が違うことの見落とし |
| 12 | 解答編 | — | — |

> **注意**：第1章の学習者は、まだ変数・型・条件分岐・繰り返し・関数を学んでいません（第2章以降）。
> 第1章の範囲は「環境を用意し、REPL とファイル実行で Python を動かし、venv と pip を扱える」までです。
> 環境構築・エラー（トレースバック）・venv・pip のトラブルは、レベル C（第7節）として全部解決してあげてください。

> **注意**：第2章の学習者は、**条件分岐・繰り返し・リスト・辞書・関数をまだ学んでいません**。
> 第2章の範囲は「変数・型・演算子・文字列・入出力・インデント」までです。
> 第2章 2.6 では、インデントの説明のために `if` を**書き方だけ**先取りして使っています
> （`if 条件:` と字下げ）。`else` / `elif` / `for` / `while` は第3章、
> リスト・辞書は第4章、関数（`def`）は第5章です。**それより先の道具を使った回答をしないでください。**
> 第2章までで使えるのは `print` / `input` / `int` / `float` / `str` / `type` / `len` / `round` と、
> 文字列メソッド（`strip` / `upper` / `lower` / `replace` / `startswith` / `endswith` / `count` / `zfill`）です。

> **注意**：第3章の学習者は、**リストの操作・辞書・関数（`def`）・例外処理をまだ学んでいません**。
> 第3章の範囲は「条件分岐・繰り返し・ループの制御・ネストの解消」までです。
> 第3章 3.1.4 では、`in` と `for` のために**リストを次の3つの使い方だけ**先取りしています。
> - `[ ]` に値を並べて**作る**（`["S", "M", "L"]`）
> - `in` / `not in` で**含まれているか調べる**
> - `for` で**1つずつ取り出す**、`len()` で**個数を数える**
>
> `append` / `remove` / `sort` / スライスなどのリスト操作、タプル・辞書・集合・内包表記は第4章です。
> 3.2.3 の `enumerate` で使う「変数を2つ書く」書き方（アンパック）も、詳細は第4章（4.2.3）です。
> 関数（`def` / `return`）は第5章、`try` / `except` は第7章なので、**それより先の道具を使った回答をしないでください。**
> 入力値の検査は、**`try` / `except` ではなく `str.isdigit()` と `if` で行います**（3.3.2）。
> 第3章までで新しく使えるようになったのは `bool()` / `range()` / `enumerate()` と `str.isdigit()` です。

> **注意**：第4章の学習者は、**関数（`def` / `return`）・ラムダ式・例外処理・`import` をまだ学んでいません**。
> 第4章の範囲は「リスト・タプル・辞書・集合・内包表記と、その使い分け」までです。
> 第4章までで新しく使えるようになったのは
> `sum()` / `max()` / `min()` / `list()` / `set()` / `sorted()` と、
> リストのメソッド（`append` / `insert` / `remove` / `pop` / `sort` / `copy`）、
> 辞書のメソッド（`get` / `keys` / `values` / `items`）、`del` です。
> **`sorted()` の `key`（並べ替えの基準指定）は第5章（5.5.3）なので使わないでください。**
> 「点数の高い順に生徒を並べる」のような処理は、
> 第4章の範囲では**値だけを取り出して並べる**形で答えてください。
> 関数は第5章、`import` と標準ライブラリは第6章、`try` / `except` は第7章です。
> f-string の中で辞書を読むときは、**外側と違う引用符**を使わせてください
> （`f"{student['name']}"`。同じ引用符は Python 3.11 以前でエラーになります）。

> **注意**：第5章の学習者は、**`import`・標準ライブラリ・クラス・例外処理・型ヒントをまだ学んでいません**。
> 第5章の範囲は「関数の定義・引数・戻り値・スコープ・関数を値として渡すこと・関数の切り出し方」までです。
> 第5章までで新しく使えるようになったのは
> `def` / `return` / `lambda` / `is` / `is not` / `help()` と、
> `sorted()` / `max()` / `min()` の **`key` 引数**、`*args` / `**kwargs` です。
> **`import` と標準ライブラリ（`datetime` / `random` / `math` / `collections`）は第6章**、
> `open` / `with` / `try` / `except` は第7章、`class` は第8章、型ヒントは第9章です。
> **それより先の道具を使った回答をしないでください。**
> とくに、`collections.Counter` で数えたり、`operator.itemgetter` を `key` に渡したりしないでください。
> 数えるのは `dict.get`（4.3.3）、並べ替えの基準はラムダ式（5.5.3）で書かせてください。
> `global` は 5.4.3 で「使わない」と決めています。
> 外の変数を変えたい相談には、**引数で受け取り `return` で返す**形を提案してください。

> **注意**：第6章の学習者は、**ファイル操作・例外処理・クラス・型ヒントをまだ学んでいません**。
> 第6章の範囲は「モジュール・`import`・標準ライブラリ・`if __name__ == "__main__":`・パッケージ」までです。
> 第6章までで新しく使えるようになったのは、`import` と
> **標準ライブラリのうち次の4つだけ**です。
> - `datetime`（`date` / `datetime` / `timedelta` / `strftime` / `weekday`）
> - `random`（`randint` / `choice` / `sample` / `shuffle` / `seed`）
> - `math`（`floor` / `ceil` / `sqrt` / `pi`）
> - `collections`（`Counter` / `defaultdict`）
>
> あわせて `str.join` と `sys.path`（6.1.3 の確認用途のみ）が使えます。
> **`open` / `with` / `pathlib` / `csv` / `json` / `try` / `except` は第7章**、
> `class` は第8章、型ヒントと `ruff` / `mypy` / `uv` は第9章です。
> **それより先の道具を使った回答をしないでください。**
> とくに、ファイルにデータを保存する相談には、**まだファイル操作を教えないでください**。
> 第6章の範囲では「実行するたびにデータを書く」形で答えてください。
> `from ... import *` は 6.2.4 で「使わない」と決めています。
> 相対 import（`from .taxes import ...`）も 6.5.3 で「読めれば十分」としているので、
> **回答のコードは絶対 import（`from shop.taxes import ...`）で書いてください。**
> `itertools` / `functools` / `secrets` など、6.3 で扱っていない標準ライブラリも使わないでください。

> **注意**：第7章の学習者は、**クラス・型ヒントをまだ学んでいません**。
> 第7章の範囲は「ファイルの読み書き・`pathlib`・CSV / JSON・例外処理」までです。
> 第7章までで新しく使えるようになったのは、`open` / `with` / `repr()` と、
> **標準ライブラリのうち次の3つ**です。
> - `pathlib`（`Path` / `read_text` / `write_text` / `cwd` / `exists` / `is_file` / `is_dir` /
>   `mkdir` / `iterdir` / `glob`。**`unlink` は使わせないでください**）
> - `csv`（`reader` / `DictReader` / `writer` / `DictWriter`）
> - `json`（`load` / `loads` / `dump` / `dumps`）
>
> あわせて `str.rstrip` / `str.splitlines` と `next()` が使えます。
> **`class` は第8章**、型ヒントと `ruff` / `mypy` / `uv` は第9章、
> `requests` と `argparse` は第10章です。
> **それより先の道具を使った回答をしないでください。**
> とくに `pandas` や `openpyxl` のような外部ライブラリを勧めないでください。
> CSV は `csv` モジュール、集計は `collections`（6.3.5）の範囲で答えてください。
>
> 7.6.2 で `class ConfigError(Exception):` だけは先取りしていますが、
> **「例外の種類を1つ増やすための決まった書き方」としてのみ**扱っています。
> `__init__` や `self`、属性・メソッドの説明はまだしないでください（第8章）。
> ファイルを開く回答には、**必ず `with` と `encoding="utf-8"` を付けてください**（7.1.3 / 7.1.4）。
> CSV を開く回答には `newline=""` も付けてください（7.4.1）。
> `except:` と裸で書いた例や `except: pass` は、7.5.5 で「使わない」と決めています。

> **注意**：第8章の学習者は、**型ヒントをまだ学んでいません**（第9章）。
> 第8章の範囲は「クラス・インスタンス・属性・メソッド・クラス変数・継承・特殊メソッド・`dataclass`」までです。
> 型の注記は **`@dataclass` の項目を書くためだけ**に、8.5.2 で先取りしています。
> 使ってよいのは `str` / `int` / `float` / `bool` / `list` の5つと、`項目名: 型` の形だけです。
> **関数の引数や戻り値に型を書いた回答をしないでください**（`def f(x: int) -> str:` は第9章）。
> `Optional` / `Union` / `|` / `list[str]` のような書き方も第9章です。

> **注意**：第9章の学習者は、**外部ライブラリ（`requests` など）と `argparse` をまだ学んでいません**（第10章）。
> 第9章の範囲は「型ヒント・型チェッカー（mypy / Pylance）・ruff・`pyproject.toml`・uv の紹介」までです。
> 第9章までで新しく使えるようになったのは、**型ヒントの記法**（`名前: 型` / `-> 型` /
> `list[str]` / `dict[str, int]` / `tuple[int, int]` / `型 | None` / `"自クラス名"`）と、
> ターミナルから動かす **mypy** と **ruff** です。
> **`typing` から `List` / `Dict` / `Optional` / `Union` を import した書き方はさせないでください。**
> Python 3.13 を使っているので、`list[str]` と `str | None` の形で答えてください
> （古いコードを読むときの知識としてだけ 9.2.1 / 9.2.2 で触れています）。
> `TypedDict` / `Protocol` / `Generic` / `TypeVar` / `Literal` / `cast` は扱っていません。
> **`# type: ignore` で報告を消す回答をしないでください**（9.3.2 で「使わない」と決めています）。
> mypy の報告が出たら、**コードのほうを直す**形で答えてください。
> `python-lesson` には第1章からのファイルが全部あるため、
> **`mypy .` や `ruff check .` ではなく、ファイル名を1つ指定**させてください。
> uv は 9.5.2 で「紹介」しただけです。**環境構築の相談は venv + pip（1.5 / 1.6）で答えてください。**
>
> 特殊メソッドは 8.4 で扱った **`__init__` / `__str__` / `__repr__` / `__eq__` /
> `__len__` / `__contains__` の6つだけ**にしてください。
> `__lt__` による `sorted` の並べ替え、`@property`、`@classmethod` / `@staticmethod`、
> 抽象基底クラス（`abc`）、多重継承、`__slots__` は扱っていません。
> デコレータも、**`@dataclass` を「そう書くもの」として使う**ところまでで、
> 自作の仕方は説明していません。
>
> `dataclasses` から使ってよいのは `dataclass` と `field(default_factory=...)` だけです
> （`asdict` は演習 8.4 の別解で名前だけ出しています）。
> `frozen=True` / `order=True` / `__post_init__` は扱っていません。
> クラスを勧めるときは、**8.6 の判断基準（`self.` が出てこないなら関数でよい）**に
> 反していないか確かめてください。継承は「子は親の一種である」と言えるときだけ、1段までです。

> **注意**：第10章の学習者は、**この本の内容をひととおり終えています。**
> 第10章の範囲は「CSV の集計・`requests` による API 呼び出し・`argparse`・
> スクリプトの関数分割と型チェック」までです。
> 第10章までで新しく使えるようになったのは、**`requests`**（`get` / `params` /
> `timeout` / `status_code` / `raise_for_status` / `json()` / `RequestException`）、
> **`argparse`**（`ArgumentParser` / `add_argument` / `type` / `default` /
> `metavar` / `help` / `action="store_true"` / `parse_args`）、`time.sleep`、
> `defaultdict(int)` の3つ（＋`types-requests`）です。
> **第10章では、新しいプロジェクト `sales-analyzer` を作ります**
> （`python-lesson` ではありません）。この本のファイルの場所を尋ねられたら、
> 第9章までは `python-lesson`、第10章は `sales-analyzer` と答えてください。
> `pandas` / `numpy` / `openpyxl` / `httpx` / `click` / `typer` は扱っていません。
> **CSV は `csv` モジュール、集計は `defaultdict`、API は `requests`、
> コマンドライン引数は `argparse` の範囲で答えてください。**
> 非同期処理（`async` / `await` / `asyncio`）、`logging`、テスト（pytest）も扱っていません
> （テストとデータ分析は第11章で「次に学ぶとよいもの」として紹介するだけです）。
> 10.3 の API（Open-Meteo）は**インターネット接続が必要**です。
> 通信できない環境の相談には、`--no-weather` を付けて進める形（10.5.2）を案内してください。
> 例外を捕まえるかどうかは、10.6.3 の基準
> （**そのあとどうするかを、その場で決められるか**）で判断させてください。

> **注意**：第11章の学習者は、**この本を読み終えています。**
> 11 章で扱う pytest / pandas / 自動化は、いずれも**入口だけ**です。
> pytest は「`test_` で始まる関数に `assert` を書いて `pytest` を実行する」までで、
> フィクスチャ（`@pytest.fixture`）・`parametrize`・モック・カバレッジは扱っていません。
> pandas は `read_csv` / 列の追加 / `groupby(...).sum()` / 条件による絞り込み /
> `sort_values` / `sum` までで、`merge` / `pivot_table` / `apply` / 欠損値の扱い /
> matplotlib による可視化は扱っていません。
> 自動化はタスクスケジューラ・cron への登録と、`pathlib` を使った仕分けスクリプトまでで、
> `logging` / `schedule` などのライブラリは扱っていません。
> **11 章で新しく作るファイルは、すべて `sales-analyzer` の中です**
> （`test_analyze_sales.py` / `pandas_step1〜4.py` / `organize_files.py` / `demo_downloads/`）。
> ファイルを移動するスクリプトの相談には、**必ず DRY RUN（表示だけ）で確かめてから**
> 実行させてください。

## 3. fastapi-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに（前提：python-text 完了） | （コードなし）この本の前提（python-text の venv / pip / リスト / 辞書 / 関数 / `import` / JSON / 例外 / `class` / `@dataclass` / 型ヒントの戻り場所つき対応表）、**型ヒントが FastAPI では実際の動作を決めること**（python-text 第9章との違い）、`python --version` / `python3 --version` による確認（3.10 以上）、この本で作るもの（タスク管理 API →第9章で react-text 第10章のアプリと接続）、全5冊での位置づけ、AI サポートの準備（指示ファイルの読み込み・動作確認・章番号を添える理由）、API 開発ならではの進め方（**サーバーは起動しっぱなし**・`Ctrl` + `C` で停止・**ターミナルを2つ使う**（VS Code の「新しいターミナル」／2つ目では venv の有効化が必要）・確認は「ブラウザ／`/docs`／サーバーのログ」の3つ・書いたらすぐ動かす輪）、詰まったときの確認順（サーバーが動いているか→URL とメソッド→ステータスコード→ターミナルの最終行）と質問テンプレート（章番号・URL とメソッド・返ってきたもの・サーバーの表示） |
| 1 | Web API とは | フロントエンドとバックエンド（1台のパソコンに閉じ込められる限界・データを1か所に置く構図・サーバーは起動しっぱなし）、**Web API＝HTTP で呼び出せる関数**（関数名→URL、引数→URL に付ける値かボディ、戻り値→JSON）、HTTP（リクエスト1つ→レスポンス1つで完結・リクエストの部品＝メソッド／URL／ヘッダー／ボディ・レスポンスの部品＝ステータスコード／ヘッダー／ボディ）、**HTTP メソッド**（`GET`（何度呼んでも変わらない・ボディを付けない）/ `POST`（作る）/ `PUT`（丸ごと置換。送らなかった項目は消える）/ `PATCH`（一部だけ変更）/ `DELETE`（消す）・**URL に動詞を書かない**）、**ステータスコード**（2xx / 3xx / **4xx＝送った側**／**5xx＝受けた側**・`200` / `201` / `204` / `400` / `404` / `422` / `500`・`404` と `422` の違い・`500` はサーバーのターミナルを見る）、ヘッダーとボディ（`Content-Type: application/json`・`Authorization`（存在のみ）・メソッドごとのボディの有無）、**JSON**（**ダブルクォートのみ・キーも引用符・末尾カンマ禁止・コメント禁止**・`true` / `false` / `null` は小文字・Python との対応表（`True`→`true` / `None`→`null` / `dict`→オブジェクト / `list`→配列）・`json.dumps` / `json.loads` と `ensure_ascii=False` で確認）、**REST**（リソース＝数えられるもの・**URL は複数形の名詞**・`/tasks` と `/tasks/3`・入れ子は2段まで（`/members/12/loans`）・動詞を名詞に翻訳する（「借りる」→`POST /loans`）・**`POST` の相手は集まり、`PUT`/`PATCH`/`DELETE` の相手は1件**・メソッド×URL×成功コードの対応表・完璧を目指さなくてよい（ただし **`GET` でデータを変更しない**））、**API を叩く3つの方法**（ブラウザのアドレス欄は `GET` のみ・**開発者ツールの Network タブ**（`F12` / `Command`+`Option`+`I`・`Status Code` / `Headers` / `Response` / `Preview`）・**`curl`**（**PowerShell では `curl.exe`**・`-i`（ヘッダー表示）/ `-X`（メソッド）/ `-H`（ヘッダー）/ `-d "@ファイル名"`（ボディをファイルから）/ `-o`（ファイルに保存）/ `-s`）・JSONPlaceholder（`/posts` / `/posts/1` / `/users`。**保存はされない**）・保存した JSON を `json.load` で読んで Python の集計に繋げる） |
| 2 | FastAPI をはじめる | 第1章の範囲に加えて：**FastAPI が肩代わりする部分**（受け取り・パスの振り分け・戻り値の JSON 変換・レスポンスの組み立て。書くのは呼ばれる関数だけ）、**型ヒントが実際の検査とドキュメントに使われる**（python-text 9.1.4 との違い）、Flask / Django との位置づけの違い（表のみ）、**プロジェクトの作り方**（`mkdir` → `cd` → `python -m venv .venv` → 有効化 → `pip install "fastapi[standard]==0.115.6"` → `pip freeze > requirements.txt`）、`[standard]` の意味（`fastapi` コマンド・`uvicorn` が入る。**ダブルクォートで囲む**）、`pip list` / `fastapi --version` での確認、依存関係に `pydantic` / `uvicorn` / `starlette` が入ること（名前のみ）、**`main.py` の書き方**（`from fastapi import FastAPI` / `app = FastAPI()` / `@app.get("/")` / `def` / 辞書を `return`）、**デコレータの3部品**（登録先 `app`・メソッド `.get`・パス `("/")`）、`.get` / `.post` / `.put` / `.patch` / `.delete` の書き分け（**書き方のみ。パラメータは第3章**）、`app` という変数名を使う理由、関数名は自由（`/docs` の見出しになる）、**戻り値の辞書が自動で JSON になる**（`True`→`true` / `None`→`null`・入れ子・リスト可・`Content-Type` も自動。**日付や自作クラスは第4章**）、**`fastapi dev main.py` での起動**（ターミナルが返ってこない・`Ctrl`+`C` で停止・`127.0.0.1` と `localhost`・ポート 8000・`dev` と `run` の違い）、ブラウザ / `curl` / 開発者ツールでの確認、**アクセスログの読み方**（`INFO: 127.0.0.1:... - "GET /health HTTP/1.1" 200 OK`）、登録していない URL は `404`、**自動リロード**（保存で `Reloading...`・反映されないときの4つの確認）、**新規 API の6ステップ**（作成→venv→install→main.py→dev→確認）、**ポート衝突**（`Address already in use`・`--port 8001`・`netstat -ano \| Select-String ":8000"` + `taskkill /PID <n> /F`・`lsof -i :8000` + `kill <n>`・**ポートを分ければ同時に動かせる**）、**`/docs`（Swagger UI）**（`Try it out` → `Execute`・`Curl` / `Code` / `Response body` の3表示。**Windows の引用符問題を回避できる**）、`/redoc`（読む用）、**`/openapi.json` からコードが一方通行で生成されるのでズレない**、docstring が説明欄になること、**起動しないときの3原因**（有効化忘れ／インストール漏れ・`[standard]` 忘れ／ファイル名・変数名の食い違い）、`which python` / `Get-Command python` での有効化確認、`There is no FastAPI app` / `Path does not exist` の意味 |
| 3 | パラメータを受け取る | 第2章の範囲に加えて：練習用データ（`main.py` の中の `tasks` リスト。**サーバーを止めると消える。保存は第6章**）、**パスパラメータ**（`@app.get("/tasks/{task_id}")` と同名の引数・**型ヒントが無いと文字列のまま**・波括弧の名前と引数名がずれると `missing` の `422`）、**型ヒントによる変換と検査**（`"3"`→`3`・変換できなければ `422` で関数は呼ばれない・使えるのは `int` / `float` / `str` / `bool`）、`404` と `422` の違い（見つからない／形式が違う）、**見つからないときも `200` でメッセージを返す**（`HTTPException` は第5章 5.4.1）、**定義の順序**（**固定のパスを波括弧付きより先に書く**・上から順に照合・型ヒントが無いと気づけない）、**クエリパラメータ**（`?key=value`・`&` 区切り・**パスの波括弧に無い引数が自動的にこれになる**・`bool` は `true`/`1`/`yes`/`on` を受け付ける）、**デフォルト値の有無で必須か省略可能かが決まる**、**`bool \| None = None`** と `is not None`（`if done:` では `false` の指定が効かない）、**`list[int] = Query(default=[])`**（`Query` が無いと値が届かない）、**リクエストボディ**（URL に載せられないもの＝長さ・入れ子・秘密の値・`GET` の約束・**型ヒントを `dict` にすると受け取れる**・`/docs` か `curl` で送る・`-H "Content-Type: application/json"` の付け忘れは `dict_type` の `422`・PowerShell は `-d "@ファイル名"`・**成功しても `200`（`201` の指定は第4章 4.4.3）**）、**`dict` は中身を検査しない**（`KeyError` → `500` → サーバーのターミナルのトレースバック。**解決は第4章の Pydantic**）、**パス・クエリ・ボディの同時利用**（名前と型で自動判定。引数の順番は無関係）、**`Query` / `Path` による条件付け**（`default` / `ge` / `le` / `gt` / `lt` / `min_length` / `max_length`・`/docs` にも反映・**デフォルト値のある引数は後ろに書く**・公式ドキュメントの `Annotated` 記法の存在）、**`422` の読み方**（`detail` はリスト・**`loc` の1つ目（`path` / `query` / `body` / `header`）で直す場所が決まる**・`type` / `msg` / `input` / `ctx`・`missing` / `int_parsing` / `bool_parsing` / `greater_than_equal` / `string_too_short` / `dict_type`・複数の問題はまとめて返る）、**ヘッダー**（`Header(default=None)`・**引数名の `_` はヘッダー名の `-`**・大文字小文字は無視・`User-Agent` は自動で付く・`X-` の慣習）、**クッキー**（`Cookie(default=None)`・`curl -b` で送る・**`/docs` からは送れない**・`Set-Cookie` を返す側は第7章）、パーセントエンコーディング（存在のみ。日本語の確認はブラウザか `/docs` で） |
| 4 | Pydantic | 第3章の範囲に加えて：**Pydantic**（型ヒントを読み取って実行時に検査するライブラリ。FastAPI に同梱・**2 系**で書く。`@validator` / `.dict()` は 1 系なので使わない）、`@dataclass` との違い（検査するかしないか。python-text 9.1.4 の回収）、**モデル**（`class TaskCreate(BaseModel):`・**値は属性で取り出す**（`new_task.title`）・`/docs` の `Request body` に送信例が出る・`dict` の `500` が **`422`** に変わる）、**必須と任意**（デフォルト値の有無で決まる・**`str \| None = None`**・**`str = None` は省略できるが `null` を送ると `string_type` の 422**）、**入れ子のモデル**（内側もモデルにする・`loc` が `["body","owner","email"]` と長くなる・`model_dump()` で辞書に変換）、**リストを持つモデル**（`list[str]`・`list[Model]`・**モデルの項目では `= []` と書いてよい**（python-text 5.2.4 との違い）・`list_type`）、**`Field`**（`Query` / `Path` と同じ兄弟・`default` / `min_length` / `max_length` / `ge` / `le` / `gt` / `lt` / `pattern` / `description`・**`default=` の有無で必須か決まる**）、**正規表現**（`^` / `$` / `\d` / `{3}` / そのままの文字の5つだけ・`r"..."`・**メールアドレスは正規表現で検査しない**（`EmailStr` の存在のみ紹介））、**カスタムバリデータ**（`@field_validator("title")` + `@classmethod`・`raise ValueError(...)` → `value_error`（`msg` に `Value error, ` が付く）・**`return` した値が採用される**・`return` 忘れの罠）、**`422` の追加の `type`**（`string_too_short` / `string_too_long` / `string_pattern_mismatch` / `less_than_equal` / `list_type` / `value_error`・**複数の問題はモデルの定義順に並ぶ**・**モデルに無いキーは黙って捨てられる**（`extra="forbid"` の存在のみ））、**レスポンスモデル**（`response_model=`・**関所は2つ（入力＝`422`／出力＝`500`）**・`loc` の1つ目が **`response`** なら自分のコード・`ResponseValidationError` はサーバーのターミナル）、**返してはいけない項目を落とす**（`OwnerRead` に `email` を書かない・保存側は変えない・包みの形は `TaskListResponse` のようにモデル化・**`404` が返せないので見つからないときは `TaskRead \| None` で `null`**（`HTTPException` は第5章 5.4.1））、**`status_code=`**（**`201`**（3.3.2 からの回収）・**`204`（`return None` で空のボディ）**・`status.HTTP_201_CREATED` の存在のみ）、**入力用と出力用を分ける**（`Create` / `Update` / `Read`・場面ごとに必要な項目が違う・`PUT` をやめて **`PATCH`** に・**`model_dump(exclude_unset=True)`**（送られてきた項目だけ取り出す）・共通部分は `TaskBase` に継承でまとめる（**継承すると JSON の項目の順番が変わる**）・モデルの docstring が `/docs` に出る）、**設定管理**（`pydantic-settings==2.7.0` は**別途 `pip install`**・`BaseSettings`・`SettingsConfigDict(env_file=".env")`・`FastAPI(title=...)`・**環境変数 > `.env` > コードのデフォルト値**・`.env` は VS Code で作る・**再起動しないと反映されない**・型が合わないと**起動時に落ちる**）、**秘密情報をコードに書かない**（`.env` を共有しない・`.gitignore`（Git は用語の紹介のみ）・**`.env.example`**・本番では環境変数を直接渡す・`settings.model_dump()` をそのまま返さない） |
| 5 | プロジェクト構成 | 第4章の範囲に加えて：**分ける基準**（行数ではなく「一緒に変更するかどうか」・役割で分ける（設定 / モデル / データ / 窓口）と機能で分ける（タスク / メモ）の2軸・最初から分けすぎない）、**採用する構成**（`fastapi-lesson/app/` を**パッケージ**にする（`__init__.py`）・`main.py`（組み立て）/ `config.py` / `schemas.py` / `data.py` / `dependencies.py` / `errors.py` / `routers/`・**起動は `fastapi dev app/main.py`**（`Using import string: app.main:app`）・**絶対 import**（`from app.config import settings`。python-text 6.5.3）・`ModuleNotFoundError: No module named 'app'` の2原因（`__init__.py` 忘れ／起動した場所）・**`.env` は起動時の現在地から探される**）、**`APIRouter`**（「窓口をまとめた小さな `app`」・`@router.get` と書く・**`include_router` で登録するまで動かない**（`{"detail":"Not Found"}`）・`from app.routers import tasks` と `from app.data import tasks` の名前衝突（`as` か `from app import data`）・**定義の順序（3.1.4）はルーターの中でも効く**）、**`prefix` と `tags`**（`APIRouter(prefix="/tasks", tags=["tasks"])`・パスから `/tasks` を消し **一覧と登録は `""`** になる・`prefix` の末尾に `/` を書くと `AssertionError`・`prefix` 無しのルーターもある（`misc.py`）・`tags` が `/docs` の見出し・**`/tasks/` は `307`** でリダイレクト）、**`Depends`（依存性注入）**（引数の受け取り方そのものを関数に切り出す・`Depends(関数)`（**括弧を付けない**）・**呼ばれる順は「依存 → 窓口の関数」**で、依存が `422` を出せば窓口は呼ばれない・`/docs` の表示は変わらない・`app/dependencies.py` に置く・**依存は依存を持てる**（`get_owner_name` → `get_visible_tasks`）・同じ依存は1リクエストにつき1回だけ呼ばれる・**`dependency_overrides` で差し替えられる**（テスト用。第8章の伏線）・**HTTP ヘッダーには日本語（非 ASCII）を入れられない**ので担当者はクエリで受け取る）、**`HTTPException`**（**`raise` する**（`return` すると `200` で `{"status_code":...}` が返る）・`status_code=404` と `detail`・**「探して無ければ 404」を依存（`get_task_or_404`）にまとめると窓口から `for` が消える**・`response_model` から `| None` を外せる・`detail` に内部の事情を書かない）、**例外ハンドラ**（`app/errors.py` に `Exception` を継承した自作の例外・`@app.exception_handler(例外)` + `JSONResponse`・**`409 Conflict`**・ルーターに HTTP の都合を書かない・**ハンドラを書き忘れた例外は `500`**）、**エラーレスポンスの統一**（**`{"error": {"status", "message", "detail"}}` に統一。第6章以降もこの形**・`starlette.exceptions.HTTPException` に登録する理由（存在しない URL の `404` は Starlette が投げる）・`RequestValidationError` と **`jsonable_encoder`**（付けないと `500`）・**`422` の `detail` は捨てない**）、**ログ**（`print` の限界（レベル・時刻・出どころ・切り替え・出力先）・`logging.basicConfig(level=..., format=...)` は**アプリで1回だけ**・`logging.getLogger(__name__)`・**`basicConfig` が無いと `INFO` は出ず、`WARNING` だけ素の形で出る**・`%s` と引数で書く・**5段階のレベル**（`DEBUG` / `INFO` / `WARNING` / `ERROR` / `CRITICAL`）・`level=logging.DEBUG if settings.debug else logging.INFO` で `.env` から切り替え・**パスワード / トークン / 個人情報 / ボディ丸ごとを出さない**・`exc_info=True`）、**ミドルウェア**（`@app.middleware("http")` + `async def` + `await call_next(request)`・**行きと帰りの両方を通る**・すべてのリクエスト（`/docs` や `404` も）を通るので重い処理を書かない・`time.perf_counter()` で計測し `response.headers["X-Process-Time"]` を付ける（表示は小文字 `x-process-time`）・`request.method` / `request.url.path` / `response.status_code`・`async` はこの形だけ使う） |
| 6 | データベース連携 | 第5章の範囲に加えて：**永続化**（リストはメモリの上にあり停止で消える・ファイルに書く／データベースを使うの2択と使い分け）、**SQLite → MySQL の進め方**（この本は SQLite（ファイル1つ）。MySQL は docker-text / mysql-text）、**ORM**（データベースの行を Python のオブジェクトとして扱う仕組み・**1クラス＝1テーブル、1インスタンス＝1行**・SQL は mysql-text・`echo=True` で組み立てられた SQL を見られる）、**インストールと接続設定**（`sqlalchemy==2.0.36` を別途 `pip install`・**接続 URL は `.env` の `DATABASE_URL`**（`sqlite:///./app.db`。乗り換えはこの1行）・`.env.example` にも書く・**`app.db` は共有しない**）、**3つの部品**（**エンジン**（`create_engine`。アプリに1つ）・**セッション**（`sessionmaker(bind=engine, autoflush=False)`。1リクエストに1つ）・**`Base`**（`DeclarativeBase` を継承）・**SQLite では `connect_args={"check_same_thread": False}`**）、**テーブルのモデル**（`__tablename__`・`名前: Mapped[型] = mapped_column(...)`・`primary_key=True`（整数は自動採番）・`String(20)`・**`\| None` の有無が `NOT NULL`**・`default=`（**`default=list` / `default=datetime.now` は括弧なし**）・`unique=True`・**リストは `JSON` 型**・Pydantic 側の `max_length` と数を揃える）、**`create_all` の限界**（**まだ無いテーブルを作るだけ。列は増えず、エラーも出ない** → `no such column`。解決は 6.6）、**スキーマとモデルの呼び分け**（`app/schemas.py`＝外とやりとりする形／`app/models.py`＝保存する形・`owner_email` は持つが返さない・**`@property` で入れ子の形に組み立てる**（初出）・**`model_config = ConfigDict(from_attributes=True)`**（無いと `Input should be a valid dictionary` の `500`））、**初期データ**（`app/seed.py` を `python -m app.seed` で実行・`if __name__ == "__main__":`・**何度実行しても増えない**作りにする）、**CRUD**（`add` → **`commit`** → **`refresh`**（`refresh` 忘れで `id` が `None` → `loc` が `["response","id"]` の `500`）・`select(...)` は組み立てるだけで `db.scalar` / `db.scalars(...).all()` で実行・**主キー1件は `db.get(Task, id)`**・`statement = statement.where(...)` の積み上げ・`Task.title.contains(...)` / `Task.id.in_(...)` / `select(func.count()).select_from(...)`・更新は **`setattr` して `commit`**（`add` 不要）・削除は `db.delete` → `commit`（**`commit` 後は消したオブジェクトの属性を読めない**）・**`response_model` の無い窓口からモデルをそのまま返すと `owner_email` が漏れる** → `TaskRead.model_validate(...)`）、**ページネーション**（`skip`（`ge=0`）/ `limit`・**`count` は切り出す前に数える**（`statement.subquery()`）・**`order_by` を必ず付ける**・順番は「絞り込み → 数える → 並べ替え → 切り出し」）、**セッション管理**（`get_db` は **`yield` + `try` / `finally`**・`Iterator[Session]`・**同じリクエスト内では同じセッション**（だから `get_task_or_404` の `task` を `db.commit()` で保存できる）・**`commit`＝確定 / `rollback`＝取り消し**・**`commit` 忘れは `201` が返るのに保存されない**・`IntegrityError` → `db.rollback()` → 自作の例外・**`rollback` 忘れは `PendingRollbackError`**・**窓口の中で `SessionLocal()` を直接呼ばない**）、**マイグレーション**（`alembic==1.14.0` を別途 `pip install`・`alembic init migrations`・**編集するのは `migrations/env.py` の2か所**（`config.set_main_option("sqlalchemy.url", settings.database_url)` と `target_metadata = Base.metadata`・`alembic.ini` の `sqlalchemy.url` は書き換えない）・`revision --autogenerate -m "..."` → **生成物を読む** → `upgrade head`・`revision` / `down_revision` の鎖・`upgrade()` と `downgrade()` は対・`current` / `history` / `downgrade -1`・**あとから足す列は `\| None`**（既存行が `null` になる）・**名前の変更は「削除＋追加」と誤検出される**・**一度渡したマイグレーションは書き換えず、新しい1本を足す**）、`alembic` コマンドは `fastapi-lesson` で実行する |
| 7 | 認証 | パスワードハッシュ、JWT、依存性による認可 |
| 8 | テスト | `TestClient`、pytest |
| 9 | 実践：React と繋ぐ | CORS、フロントとの結合 |
| 10 | 解答編 | — |

> **注意**：第0章の学習者は、**FastAPI をまだ1行も書いていません。**
> 第0章の範囲は「この本の前提の確認・AI の準備・進め方」までで、
> インストールもしていません（第2章）。
> 第0章の相談には、**python-text までの知識だけ**で答えてください。
> 「タスク管理 API を作る」という完成イメージは 0.1.2 で示していますが、
> エンドポイントの書き方（`@app.get`）は第2章、パラメータは第3章、
> Pydantic は第4章、データベースは第6章です。**先取りして見せないでください。**
> 環境の相談（`python --version` が通らない・PATH・venv）は、
> python-text 1.2 / 1.5 / 1.6 に戻す形で、**手順を全部出して解決してあげてください。**
>
> **注意**：第1章の学習者は、**FastAPI をまだ1行も書いていません。**
> インストールもしていません（第2章）。
> 第1章の範囲は「Web API の考え方・HTTP・JSON・REST・公開 API を叩いてみる」までで、
> 書いた Python コードは **`json.dumps` / `json.loads` の確認（1.3.3）と、
> `curl` で保存した JSON を `json.load` で読むところ（1.5.3）だけ**です。
> **`@app.get` などのエンドポイントの書き方を先取りして見せないでください**（第2章）。
> パラメータの受け取り方は第3章、Pydantic は第4章、データベースは第6章です。
> 第1章の相談には、**python-text までの知識＋この章の HTTP / JSON / REST の語彙**で答えてください。
>
> **注意（第2章）**：第2章の学習者は、**引数を1つも持たない窓口しか作っていません。**
> `@app.get("/tasks/{task_id}")` のようなパスパラメータ、`?done=true` のクエリパラメータ、
> `POST` のボディ受け取りは**すべて第3章**です。先取りして見せないでください。
> Pydantic の `BaseModel` は第4章、`APIRouter` によるファイル分割は第5章です。
> **第2章の範囲でファイルは `main.py` 1つだけ**なので、
> 「ルーターに分けましょう」という助言はしないでください。
> 返せる値は**辞書・リスト・文字列・数値・真偽値・`None`** の範囲です
> （日付や自作クラスを返す相談には「第4章で扱います」と伝えてください）。
> インストールのバージョンは **`fastapi[standard]==0.115.6`**（2.2.2）で統一されています。
> 起動コマンドは **`fastapi dev main.py`** です。`uvicorn main:app --reload` は
> **このテキストでは使っていない**ので、そちらを案内しないでください。
>
> ターミナルから API を叩く道具は **`curl` だけ**です。
> `requests`（python-text 第10章）を使っても構いませんが、
> **仮想環境と `pip install` が必要になる**ので、
> 第1章の時点では `curl` で答えるほうが確実です。
> **Windows の相談には、必ず `curl` ではなく `curl.exe` と書いてください**（1.5.3）。
> ボディを送るコマンドは、引用符の扱いが OS で違うため、
> **`-d "@ファイル名"` の形（1.5.3）で案内してください。**
>
> 1.5 は**インターネット接続が必要**です（JSONPlaceholder）。
> 接続できない環境の相談には、1.5 を飛ばして第2章に進み、
> 自分で作った API に対して同じことを試す形を案内してください。
> REST の設計に正解を求められたら、**1.4.4 のとおり「完璧を目指さなくてよい」**と伝え、
> 候補と利点を示して**選ぶのは学習者自身**にさせてください。
>
> **注意**：第3章の学習者は、**Pydantic のモデルをまだ知りません**（第4章）。
> ボディは **`dict` で受け取る**ところまでしか学んでいないので、
> **`class TaskCreate(BaseModel):` のような書き方を先取りして見せないでください。**
> 同じ理由で、次のものもまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | `BaseModel` / `Field` / `response_model` / `status_code=201` | 第4章 |
> | `HTTPException`（`404` を返す）・`APIRouter`・`Depends` | 第5章 |
> | データの保存（データベース・ファイル） | 第6章 |
> | `Set-Cookie`（クッキーを渡す側）・認証 | 第7章 |
>
> 「`500` になる」「`title` が無くても登録できてしまう」という相談は、
> **第3章の時点では想定どおりの動作**です（3.3.2）。
> `.get("キー", 既定値)` で落ちないようにする対処までを案内し、
> **「本当の解決は第4章」だと伝えてください。**
> 「見つからないときに `404` を返したい」という相談も同じで、
> **第5章 5.4.1 まで待つ**ように案内してください。
>
> `422` の相談には、**`loc` の1つ目**（`path` / `query` / `body` / `header`）を
> 一緒に読むところから始めてください（3.4.2）。
> **「作ったはずの窓口が動かない」**という相談では、
> まず **`/tasks/{task_id}` より後ろに固定パスを書いていないか**を確認させてください（3.1.4）。
>
> **注意**：第4章の学習者は、**Pydantic の 2 系だけ**を知っています。
> 検索して出てくる **1 系の書き方（`@validator` / `.dict()` / `class Config:`）を見せないでください。**
> `@field_validator` / `.model_dump()` / `model_config = ConfigDict(...)` が 2 系の書き方です。
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | `HTTPException`（`404` を返す）・`APIRouter`・`Depends`・ミドルウェア | 第5章 |
> | データの保存（データベース・ファイル）・**ファイルを分けること** | 第6章 |
> | 認証・パスワードのハッシュ化・JWT | 第7章 |
> | `TestClient` / pytest | 第8章 |
> | CORS・React との接続 | 第9章 |
>
> **`main.py` が 200 行を超えて読みにくい**という相談は、**第4章の時点では想定どおり**です。
> ファイルを分けたくなった気持ちを肯定したうえで、**第5章まで待つ**ように案内してください。
> 「見つからないときに `null` が返るのが気持ち悪い」という相談も同じで、
> **第5章 5.4.1 の `HTTPException`** まで待たせてください（4.4.2 に明記してあります）。
>
> `422` の相談では、**`loc` の1つ目**を見る手順は第3章と同じですが、
> ボディの場合は **`loc` が3つ以上になる**ことを伝えてください（`["body","owner","email"]`）。
> **`500` の相談では、`loc` の1つ目が `response` かどうか**を必ず確認させてください。
> `response` なら、送った側ではなく**学習者自身のコードが返している値**が原因です（4.4.1）。
>
> `.env` の相談では、まず **サーバーを再起動したか**を確認してください（4.6.2）。
> `pydantic-settings` は**別途インストールが必要**です（4.6.1）。
> **秘密の値をコードや `.env.example` に書いた状態のコードを、そのまま肯定しないでください**（4.6.3）。
>
> **注意**：第5章の学習者のファイル構成は **`fastapi-lesson/app/`（パッケージ）**です。
> 起動コマンドは **`fastapi dev app/main.py`**（`fastapi-lesson` の中で実行）に変わっています。
> `main.py` 1つだけの前提で答えないでください。
> import は**絶対 import**（`from app.config import settings`）で統一しています。
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | データの保存（データベース・ファイル）・SQLAlchemy・ORM | 第6章 |
> | 認証・パスワードのハッシュ化・JWT | 第7章 |
> | `TestClient` / pytest（`dependency_overrides` は 5.3.4 で**存在と使い方だけ**紹介済み） | 第8章 |
> | CORS・React との接続 | 第9章 |
>
> **エラーレスポンスは `{"error": {"status", "message", "detail"}}` に統一済み**です（5.4.3）。
> 相談に答えるときも、`{"detail": ...}` のままの形に戻さないでください。
> `ModuleNotFoundError: No module named 'app'` の相談では、
> **`app/__init__.py` の有無**と**起動した場所**の2つを確認させてください（5.1.2）。
> **`.env` が効かない**という相談も、まず起動した場所（`app` の中に入っていないか）を疑わせてください。
> ログが出ないという相談は、**`logging.basicConfig` の書き忘れ**が原因のことがほとんどです（5.5.1）。
> `async` / `await` は、**ミドルウェアのこの形以外では使っていません。**
> 窓口の関数を `async def` に書き換える助言はしないでください。

> **注意**：第6章の学習者のデータは、**SQLite（`fastapi-lesson/app.db`）**に入っています。
> `app/data.py` のリストは、この章で役目を終えています。
> ORM は **SQLAlchemy 2.0 系**（`DeclarativeBase` / `Mapped` / `mapped_column` / `select()`）で統一しています。
> **1.x 系の書き方（`declarative_base()` / `Column(...)` / `db.query(Task).filter(...)`）を見せないでください。**
> 検索するとこちらが多く出てきますが、このテキストの構成とは合いません。
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | 認証・パスワードのハッシュ化・JWT・ログイン中のユーザー | 第7章 |
> | `TestClient` / pytest（`dependency_overrides` は 5.3.4 で紹介済み） | 第8章 |
> | CORS・React との接続 | 第9章 |
> | SQL そのもの・テーブル設計・結合（`JOIN`）・インデックス | mysql-text |
> | MySQL への接続・複数テーブルの関連（`relationship`） | docker-text / mysql-text |
>
> **テーブルは1つのモデルにつき1つ**で、テーブル同士の関連（外部キー・`relationship`）は
> **この本では扱いません。** 担当者は `owner_name` / `owner_email` という平らな列で持ち、
> 入れ子の形は `@property` で組み立てています（6.3.3）。
> 「`Owner` テーブルを分けて `relationship` で繋ぐべきでは」という相談には、
> **その考え方が正しいことを認めたうえで、mysql-text で扱うと伝えてください。**
>
> よくある相談と、最初に確認させるべきことは次のとおりです。
>
> | 症状 | 最初に疑うもの |
> |------|--------------|
> | `201` は返るのに一覧に出ない | **`db.commit()` の書き忘れ**（6.4.1・6.5.2） |
> | `loc` が `["response","id"]` の `500` | **`db.refresh(...)` の書き忘れ**（6.4.1） |
> | `Input should be a valid dictionary` の `500` | **`from_attributes=True` の書き忘れ**（6.3.3） |
> | `no such column: tasks.○○` | **`create_all` では列は増えない。** マイグレーションが要る（6.3.2・6.6.1） |
> | `no such table: tasks` | `alembic upgrade head` を実行していない（6.6.3） |
> | `PendingRollbackError` | `IntegrityError` のあとに **`db.rollback()`** をしていない（6.5.2） |
> | `ModuleNotFoundError: No module named 'app'`（`alembic` 実行時） | `fastapi-lesson` で実行していない（6.6.2） |
> | `SQLite objects created in a thread ...` | `connect_args={"check_same_thread": False}` が無い（6.2.3） |
>
> **`app.db` を消して作り直す**という助言は、最後の手段にしてください。
> 6.6.1 で「本番ではできないやり方」として明記してあります。
> まず `alembic current` / `alembic history` で、いまどこまで適用されているかを確認させてください。

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
