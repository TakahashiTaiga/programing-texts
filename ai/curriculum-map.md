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

> **この本は全章（第0章〜第10章＋解答編）が完成しています。**
> 学習者が章番号を伝えてきたら、その章までの累積範囲だけで答えてください。

| 章 | タイトル | 既習範囲（累積） | よくあるつまずき |
|----|---------|----------------|----------------|
| 0 | はじめに（前提：python-text 完了） | （コードなし）この本の前提（python-text の venv / pip / リスト / 辞書 / 関数 / `import` / JSON / 例外 / `class` / `@dataclass` / 型ヒントの戻り場所つき対応表）、**型ヒントが FastAPI では実際の動作を決めること**（python-text 第9章との違い）、`python --version` / `python3 --version` による確認（3.10 以上）、この本で作るもの（タスク管理 API →第9章で react-text 第10章のアプリと接続）、全5冊での位置づけ、AI サポートの準備（指示ファイルの読み込み・動作確認・章番号を添える理由）、API 開発ならではの進め方（**サーバーは起動しっぱなし**・`Ctrl` + `C` で停止・**ターミナルを2つ使う**（VS Code の「新しいターミナル」／2つ目では venv の有効化が必要）・確認は「ブラウザ／`/docs`／サーバーのログ」の3つ・書いたらすぐ動かす輪）、詰まったときの確認順（サーバーが動いているか→URL とメソッド→ステータスコード→ターミナルの最終行）と質問テンプレート（章番号・URL とメソッド・返ってきたもの・サーバーの表示） | AI の準備をせずに進んでしまう、2つ目のターミナルで仮想環境を有効にし忘れる、`python` と `python3` のどちらが通るか分からない |
| 1 | Web API とは | フロントエンドとバックエンド（1台のパソコンに閉じ込められる限界・データを1か所に置く構図・サーバーは起動しっぱなし）、**Web API＝HTTP で呼び出せる関数**（関数名→URL、引数→URL に付ける値かボディ、戻り値→JSON）、HTTP（リクエスト1つ→レスポンス1つで完結・リクエストの部品＝メソッド／URL／ヘッダー／ボディ・レスポンスの部品＝ステータスコード／ヘッダー／ボディ）、**HTTP メソッド**（`GET`（何度呼んでも変わらない・ボディを付けない）/ `POST`（作る）/ `PUT`（丸ごと置換。送らなかった項目は消える）/ `PATCH`（一部だけ変更）/ `DELETE`（消す）・**URL に動詞を書かない**）、**ステータスコード**（2xx / 3xx / **4xx＝送った側**／**5xx＝受けた側**・`200` / `201` / `204` / `400` / `404` / `422` / `500`・`404` と `422` の違い・`500` はサーバーのターミナルを見る）、ヘッダーとボディ（`Content-Type: application/json`・`Authorization`（存在のみ）・メソッドごとのボディの有無）、**JSON**（**ダブルクォートのみ・キーも引用符・末尾カンマ禁止・コメント禁止**・`true` / `false` / `null` は小文字・Python との対応表（`True`→`true` / `None`→`null` / `dict`→オブジェクト / `list`→配列）・`json.dumps` / `json.loads` と `ensure_ascii=False` で確認）、**REST**（リソース＝数えられるもの・**URL は複数形の名詞**・`/tasks` と `/tasks/3`・入れ子は2段まで（`/members/12/loans`）・動詞を名詞に翻訳する（「借りる」→`POST /loans`）・**`POST` の相手は集まり、`PUT`/`PATCH`/`DELETE` の相手は1件**・メソッド×URL×成功コードの対応表・完璧を目指さなくてよい（ただし **`GET` でデータを変更しない**））、**API を叩く3つの方法**（ブラウザのアドレス欄は `GET` のみ・**開発者ツールの Network タブ**（`F12` / `Command`+`Option`+`I`・`Status Code` / `Headers` / `Response` / `Preview`）・**`curl`**（**PowerShell では `curl.exe`**・`-i`（ヘッダー表示）/ `-X`（メソッド）/ `-H`（ヘッダー）/ `-d "@ファイル名"`（ボディをファイルから）/ `-o`（ファイルに保存）/ `-s`）・JSONPlaceholder（`/posts` / `/posts/1` / `/users`。**保存はされない**）・保存した JSON を `json.load` で読んで Python の集計に繋げる） | URL に動詞を書いてしまう（`/deleteTask`）、JSON の末尾カンマとシングルクォート、`curl` で `@ファイル名` を使うときのパス間違い、`GET` でデータを変えようとする |
| 2 | FastAPI をはじめる | 第1章の範囲に加えて：**FastAPI が肩代わりする部分**（受け取り・パスの振り分け・戻り値の JSON 変換・レスポンスの組み立て。書くのは呼ばれる関数だけ）、**型ヒントが実際の検査とドキュメントに使われる**（python-text 9.1.4 との違い）、Flask / Django との位置づけの違い（表のみ）、**プロジェクトの作り方**（`mkdir` → `cd` → `python -m venv .venv` → 有効化 → `pip install "fastapi[standard]==0.115.6"` → `pip freeze > requirements.txt`）、`[standard]` の意味（`fastapi` コマンド・`uvicorn` が入る。**ダブルクォートで囲む**）、`pip list` / `fastapi --version` での確認、依存関係に `pydantic` / `uvicorn` / `starlette` が入ること（名前のみ）、**`main.py` の書き方**（`from fastapi import FastAPI` / `app = FastAPI()` / `@app.get("/")` / `def` / 辞書を `return`）、**デコレータの3部品**（登録先 `app`・メソッド `.get`・パス `("/")`）、`.get` / `.post` / `.put` / `.patch` / `.delete` の書き分け（**書き方のみ。パラメータは第3章**）、`app` という変数名を使う理由、関数名は自由（`/docs` の見出しになる）、**戻り値の辞書が自動で JSON になる**（`True`→`true` / `None`→`null`・入れ子・リスト可・`Content-Type` も自動。**日付や自作クラスは第4章**）、**`fastapi dev main.py` での起動**（ターミナルが返ってこない・`Ctrl`+`C` で停止・`127.0.0.1` と `localhost`・ポート 8000・`dev` と `run` の違い）、ブラウザ / `curl` / 開発者ツールでの確認、**アクセスログの読み方**（`INFO: 127.0.0.1:... - "GET /health HTTP/1.1" 200 OK`）、登録していない URL は `404`、**自動リロード**（保存で `Reloading...`・反映されないときの4つの確認）、**新規 API の6ステップ**（作成→venv→install→main.py→dev→確認）、**ポート衝突**（`Address already in use`・`--port 8001`・`netstat -ano \| Select-String ":8000"` + `taskkill /PID <n> /F`・`lsof -i :8000` + `kill <n>`・**ポートを分ければ同時に動かせる**）、**`/docs`（Swagger UI）**（`Try it out` → `Execute`・`Curl` / `Code` / `Response body` の3表示。**Windows の引用符問題を回避できる**）、`/redoc`（読む用）、**`/openapi.json` からコードが一方通行で生成されるのでズレない**、docstring が説明欄になること、**起動しないときの3原因**（有効化忘れ／インストール漏れ・`[standard]` 忘れ／ファイル名・変数名の食い違い）、`which python` / `Get-Command python` での有効化確認、`There is no FastAPI app` / `Path does not exist` の意味 | `pip install "fastapi[standard]"` のダブルクォートを外して zsh で失敗する、ターミナルを開き直すたびの有効化忘れ、`main.py` と違うディレクトリで `fastapi dev` を実行する、ポート 8000 の使用中、デコレータとその下の関数の間に空行以外を挟む |
| 3 | パラメータを受け取る | 第2章の範囲に加えて：練習用データ（`main.py` の中の `tasks` リスト。**サーバーを止めると消える。保存は第6章**）、**パスパラメータ**（`@app.get("/tasks/{task_id}")` と同名の引数・**型ヒントが無いと文字列のまま**・波括弧の名前と引数名がずれると `missing` の `422`）、**型ヒントによる変換と検査**（`"3"`→`3`・変換できなければ `422` で関数は呼ばれない・使えるのは `int` / `float` / `str` / `bool`）、`404` と `422` の違い（見つからない／形式が違う）、**見つからないときも `200` でメッセージを返す**（`HTTPException` は第5章 5.4.1）、**定義の順序**（**固定のパスを波括弧付きより先に書く**・上から順に照合・型ヒントが無いと気づけない）、**クエリパラメータ**（`?key=value`・`&` 区切り・**パスの波括弧に無い引数が自動的にこれになる**・`bool` は `true`/`1`/`yes`/`on` を受け付ける）、**デフォルト値の有無で必須か省略可能かが決まる**、**`bool \| None = None`** と `is not None`（`if done:` では `false` の指定が効かない）、**`list[int] = Query(default=[])`**（`Query` が無いと値が届かない）、**リクエストボディ**（URL に載せられないもの＝長さ・入れ子・秘密の値・`GET` の約束・**型ヒントを `dict` にすると受け取れる**・`/docs` か `curl` で送る・`-H "Content-Type: application/json"` の付け忘れは `dict_type` の `422`・PowerShell は `-d "@ファイル名"`・**成功しても `200`（`201` の指定は第4章 4.4.3）**）、**`dict` は中身を検査しない**（`KeyError` → `500` → サーバーのターミナルのトレースバック。**解決は第4章の Pydantic**）、**パス・クエリ・ボディの同時利用**（名前と型で自動判定。引数の順番は無関係）、**`Query` / `Path` による条件付け**（`default` / `ge` / `le` / `gt` / `lt` / `min_length` / `max_length`・`/docs` にも反映・**デフォルト値のある引数は後ろに書く**・公式ドキュメントの `Annotated` 記法の存在）、**`422` の読み方**（`detail` はリスト・**`loc` の1つ目（`path` / `query` / `body` / `header`）で直す場所が決まる**・`type` / `msg` / `input` / `ctx`・`missing` / `int_parsing` / `bool_parsing` / `greater_than_equal` / `string_too_short` / `dict_type`・複数の問題はまとめて返る）、**ヘッダー**（`Header(default=None)`・**引数名の `_` はヘッダー名の `-`**・大文字小文字は無視・`User-Agent` は自動で付く・`X-` の慣習）、**クッキー**（`Cookie(default=None)`・`curl -b` で送る・**`/docs` からは送れない**・`Set-Cookie` を返す側は第7章）、パーセントエンコーディング（存在のみ。日本語の確認はブラウザか `/docs` で） | `{ }` の中の名前と引数名が食い違う、型ヒントを付けず `422` すら出ない、`/tasks/{task_id}` を `/tasks/summary` より先に定義して取り違える、`Query(default=[])` の書き忘れ、`-H "Content-Type: application/json"` の付け忘れ、PowerShell で `&` を含む URL を囲み忘れる |
| 4 | Pydantic | 第3章の範囲に加えて：**Pydantic**（型ヒントを読み取って実行時に検査するライブラリ。FastAPI に同梱・**2 系**で書く。`@validator` / `.dict()` は 1 系なので使わない）、`@dataclass` との違い（検査するかしないか。python-text 9.1.4 の回収）、**モデル**（`class TaskCreate(BaseModel):`・**値は属性で取り出す**（`new_task.title`）・`/docs` の `Request body` に送信例が出る・`dict` の `500` が **`422`** に変わる）、**必須と任意**（デフォルト値の有無で決まる・**`str \| None = None`**・**`str = None` は省略できるが `null` を送ると `string_type` の 422**）、**入れ子のモデル**（内側もモデルにする・`loc` が `["body","owner","email"]` と長くなる・`model_dump()` で辞書に変換）、**リストを持つモデル**（`list[str]`・`list[Model]`・**モデルの項目では `= []` と書いてよい**（python-text 5.2.4 との違い）・`list_type`）、**`Field`**（`Query` / `Path` と同じ兄弟・`default` / `min_length` / `max_length` / `ge` / `le` / `gt` / `lt` / `pattern` / `description`・**`default=` の有無で必須か決まる**）、**正規表現**（`^` / `$` / `\d` / `{3}` / そのままの文字の5つだけ・`r"..."`・**メールアドレスは正規表現で検査しない**（`EmailStr` の存在のみ紹介））、**カスタムバリデータ**（`@field_validator("title")` + `@classmethod`・`raise ValueError(...)` → `value_error`（`msg` に `Value error, ` が付く）・**`return` した値が採用される**・`return` 忘れの罠）、**`422` の追加の `type`**（`string_too_short` / `string_too_long` / `string_pattern_mismatch` / `less_than_equal` / `list_type` / `value_error`・**複数の問題はモデルの定義順に並ぶ**・**モデルに無いキーは黙って捨てられる**（`extra="forbid"` の存在のみ））、**レスポンスモデル**（`response_model=`・**関所は2つ（入力＝`422`／出力＝`500`）**・`loc` の1つ目が **`response`** なら自分のコード・`ResponseValidationError` はサーバーのターミナル）、**返してはいけない項目を落とす**（`OwnerRead` に `email` を書かない・保存側は変えない・包みの形は `TaskListResponse` のようにモデル化・**`404` が返せないので見つからないときは `TaskRead \| None` で `null`**（`HTTPException` は第5章 5.4.1））、**`status_code=`**（**`201`**（3.3.2 からの回収）・**`204`（`return None` で空のボディ）**・`status.HTTP_201_CREATED` の存在のみ）、**入力用と出力用を分ける**（`Create` / `Update` / `Read`・場面ごとに必要な項目が違う・`PUT` をやめて **`PATCH`** に・**`model_dump(exclude_unset=True)`**（送られてきた項目だけ取り出す）・共通部分は `TaskBase` に継承でまとめる（**継承すると JSON の項目の順番が変わる**）・モデルの docstring が `/docs` に出る）、**設定管理**（`pydantic-settings==2.7.0` は**別途 `pip install`**・`BaseSettings`・`SettingsConfigDict(env_file=".env")`・`FastAPI(title=...)`・**環境変数 > `.env` > コードのデフォルト値**・`.env` は VS Code で作る・**再起動しないと反映されない**・型が合わないと**起動時に落ちる**）、**秘密情報をコードに書かない**（`.env` を共有しない・`.gitignore`（Git は用語の紹介のみ）・**`.env.example`**・本番では環境変数を直接渡す・`settings.model_dump()` をそのまま返さない） | モデルを定義しただけで型ヒントを書き換えない、`str \| None = None` と `str = None` の違い、モデルに無いキーが黙って捨てられる、`response_model` の必須項目を返し忘れて `500`、設定をそのまま全部返す窓口を作る |
| 5 | プロジェクト構成 | 第4章の範囲に加えて：**分ける基準**（行数ではなく「一緒に変更するかどうか」・役割で分ける（設定 / モデル / データ / 窓口）と機能で分ける（タスク / メモ）の2軸・最初から分けすぎない）、**採用する構成**（`fastapi-lesson/app/` を**パッケージ**にする（`__init__.py`）・`main.py`（組み立て）/ `config.py` / `schemas.py` / `data.py` / `dependencies.py` / `errors.py` / `routers/`・**起動は `fastapi dev app/main.py`**（`Using import string: app.main:app`）・**絶対 import**（`from app.config import settings`。python-text 6.5.3）・`ModuleNotFoundError: No module named 'app'` の2原因（`__init__.py` 忘れ／起動した場所）・**`.env` は起動時の現在地から探される**）、**`APIRouter`**（「窓口をまとめた小さな `app`」・`@router.get` と書く・**`include_router` で登録するまで動かない**（`{"detail":"Not Found"}`）・`from app.routers import tasks` と `from app.data import tasks` の名前衝突（`as` か `from app import data`）・**定義の順序（3.1.4）はルーターの中でも効く**）、**`prefix` と `tags`**（`APIRouter(prefix="/tasks", tags=["tasks"])`・パスから `/tasks` を消し **一覧と登録は `""`** になる・`prefix` の末尾に `/` を書くと `AssertionError`・`prefix` 無しのルーターもある（`misc.py`）・`tags` が `/docs` の見出し・**`/tasks/` は `307`** でリダイレクト）、**`Depends`（依存性注入）**（引数の受け取り方そのものを関数に切り出す・`Depends(関数)`（**括弧を付けない**）・**呼ばれる順は「依存 → 窓口の関数」**で、依存が `422` を出せば窓口は呼ばれない・`/docs` の表示は変わらない・`app/dependencies.py` に置く・**依存は依存を持てる**（`get_owner_name` → `get_visible_tasks`）・同じ依存は1リクエストにつき1回だけ呼ばれる・**`dependency_overrides` で差し替えられる**（テスト用。第8章の伏線）・**HTTP ヘッダーには日本語（非 ASCII）を入れられない**ので担当者はクエリで受け取る）、**`HTTPException`**（**`raise` する**（`return` すると `200` で `{"status_code":...}` が返る）・`status_code=404` と `detail`・**「探して無ければ 404」を依存（`get_task_or_404`）にまとめると窓口から `for` が消える**・`response_model` から `\| None` を外せる・`detail` に内部の事情を書かない）、**例外ハンドラ**（`app/errors.py` に `Exception` を継承した自作の例外・`@app.exception_handler(例外)` + `JSONResponse`・**`409 Conflict`**・ルーターに HTTP の都合を書かない・**ハンドラを書き忘れた例外は `500`**）、**エラーレスポンスの統一**（**`{"error": {"status", "message", "detail"}}` に統一。第6章以降もこの形**・`starlette.exceptions.HTTPException` に登録する理由（存在しない URL の `404` は Starlette が投げる）・`RequestValidationError` と **`jsonable_encoder`**（付けないと `500`）・**`422` の `detail` は捨てない**）、**ログ**（`print` の限界（レベル・時刻・出どころ・切り替え・出力先）・`logging.basicConfig(level=..., format=...)` は**アプリで1回だけ**・`logging.getLogger(__name__)`・**`basicConfig` が無いと `INFO` は出ず、`WARNING` だけ素の形で出る**・`%s` と引数で書く・**5段階のレベル**（`DEBUG` / `INFO` / `WARNING` / `ERROR` / `CRITICAL`）・`level=logging.DEBUG if settings.debug else logging.INFO` で `.env` から切り替え・**パスワード / トークン / 個人情報 / ボディ丸ごとを出さない**・`exc_info=True`）、**ミドルウェア**（`@app.middleware("http")` + `async def` + `await call_next(request)`・**行きと帰りの両方を通る**・すべてのリクエスト（`/docs` や `404` も）を通るので重い処理を書かない・`time.perf_counter()` で計測し `response.headers["X-Process-Time"]` を付ける（表示は小文字 `x-process-time`）・`request.method` / `request.url.path` / `response.status_code`・`async` はこの形だけ使う） | 最初から細かく分けすぎる、`ModuleNotFoundError: No module named 'app'`（実行位置か `__init__.py`）、`Depends(list_params())` と括弧を付ける、`raise` すべきところで `return HTTPException(...)` と書く、`include_router` の登録忘れ、`print` のままログにしない |
| 6 | データベース連携 | 第5章の範囲に加えて：**永続化**（リストはメモリの上にあり停止で消える・ファイルに書く／データベースを使うの2択と使い分け）、**SQLite → MySQL の進め方**（この本は SQLite（ファイル1つ）。MySQL は docker-text / mysql-text）、**ORM**（データベースの行を Python のオブジェクトとして扱う仕組み・**1クラス＝1テーブル、1インスタンス＝1行**・SQL は mysql-text・`echo=True` で組み立てられた SQL を見られる）、**インストールと接続設定**（`sqlalchemy==2.0.36` を別途 `pip install`・**接続 URL は `.env` の `DATABASE_URL`**（`sqlite:///./app.db`。乗り換えはこの1行）・`.env.example` にも書く・**`app.db` は共有しない**）、**3つの部品**（**エンジン**（`create_engine`。アプリに1つ）・**セッション**（`sessionmaker(bind=engine, autoflush=False)`。1リクエストに1つ）・**`Base`**（`DeclarativeBase` を継承）・**SQLite では `connect_args={"check_same_thread": False}`**）、**テーブルのモデル**（`__tablename__`・`名前: Mapped[型] = mapped_column(...)`・`primary_key=True`（整数は自動採番）・`String(20)`・**`\| None` の有無が `NOT NULL`**・`default=`（**`default=list` / `default=datetime.now` は括弧なし**）・`unique=True`・**リストは `JSON` 型**・Pydantic 側の `max_length` と数を揃える）、**`create_all` の限界**（**まだ無いテーブルを作るだけ。列は増えず、エラーも出ない** → `no such column`。解決は 6.6）、**スキーマとモデルの呼び分け**（`app/schemas.py`＝外とやりとりする形／`app/models.py`＝保存する形・`owner_email` は持つが返さない・**`@property` で入れ子の形に組み立てる**（初出）・**`model_config = ConfigDict(from_attributes=True)`**（無いと `Input should be a valid dictionary` の `500`））、**初期データ**（`app/seed.py` を `python -m app.seed` で実行・`if __name__ == "__main__":`・**何度実行しても増えない**作りにする）、**CRUD**（`add` → **`commit`** → **`refresh`**（`refresh` 忘れで `id` が `None` → `loc` が `["response","id"]` の `500`）・`select(...)` は組み立てるだけで `db.scalar` / `db.scalars(...).all()` で実行・**主キー1件は `db.get(Task, id)`**・`statement = statement.where(...)` の積み上げ・`Task.title.contains(...)` / `Task.id.in_(...)` / `select(func.count()).select_from(...)`・更新は **`setattr` して `commit`**（`add` 不要）・削除は `db.delete` → `commit`（**`commit` 後は消したオブジェクトの属性を読めない**）・**`response_model` の無い窓口からモデルをそのまま返すと `owner_email` が漏れる** → `TaskRead.model_validate(...)`）、**ページネーション**（`skip`（`ge=0`）/ `limit`・**`count` は切り出す前に数える**（`statement.subquery()`）・**`order_by` を必ず付ける**・順番は「絞り込み → 数える → 並べ替え → 切り出し」）、**セッション管理**（`get_db` は **`yield` + `try` / `finally`**・`Iterator[Session]`・**同じリクエスト内では同じセッション**（だから `get_task_or_404` の `task` を `db.commit()` で保存できる）・**`commit`＝確定 / `rollback`＝取り消し**・**`commit` 忘れは `201` が返るのに保存されない**・`IntegrityError` → `db.rollback()` → 自作の例外・**`rollback` 忘れは `PendingRollbackError`**・**窓口の中で `SessionLocal()` を直接呼ばない**）、**マイグレーション**（`alembic==1.14.0` を別途 `pip install`・`alembic init migrations`・**編集するのは `migrations/env.py` の2か所**（`config.set_main_option("sqlalchemy.url", settings.database_url)` と `target_metadata = Base.metadata`・`alembic.ini` の `sqlalchemy.url` は書き換えない）・`revision --autogenerate -m "..."` → **生成物を読む** → `upgrade head`・`revision` / `down_revision` の鎖・`upgrade()` と `downgrade()` は対・`current` / `history` / `downgrade -1`・**あとから足す列は `\| None`**（既存行が `null` になる）・**名前の変更は「削除＋追加」と誤検出される**・**一度渡したマイグレーションは書き換えず、新しい1本を足す**）、`alembic` コマンドは `fastapi-lesson` で実行する | `app` の中で `python create_tables.py` を実行する、`models.py` を書き換えてもテーブルが変わらない（`create_all` は作るだけ）、`db.commit()` を書かず「成功したように見える」、窓口の中で `SessionLocal()` を直接呼ぶ、`count` が常に `limit` と同じになる、`app.db` を消さずに Alembic へ移る |
| 7 | 認証 | 第6章の範囲に加えて：**認証と認可**（誰か／何をしてよいか・順番は認証 → 認可・**`401`＝誰か分からない / `403`＝許可がない**・`401` の英語名 Unauthorized は紛らわしい）、**ベアラー認証**（この本で実装する範囲と、しない範囲の一覧）、**パスワードの扱い**（**平文で保存しない**（`app.db` は誰でも読める・使い回しにより被害が外に広がる）・**ハッシュ化**（一方向・元に戻せない）・**ソルト**（同じパスワードでも別の値になる・`$2b$12$` の読み方・コスト 12 はわざと遅い）・`bcrypt==4.2.1` を別途 `pip install`・`bcrypt.hashpw` / `bcrypt.checkpw`・`.encode("utf-8")` / `.decode("utf-8")`（バイト列と文字列）・**`app/security.py` に閉じ込める**（外に `bcrypt` の名前を出さない）・ハッシュ値は常に 60 文字・**72 バイトを超える分は黙って捨てられる**（日本語は 24 文字で上限）・**`==` で比べても一致しない**）、**ユーザー登録**（`User` モデル（`name` / `email` は `unique=True`・**列名は `hashed_password`**）・Alembic で `create users table`・**`UserRead` に `hashed_password` を書かない**（`response_model` が無いと漏れる）・`UserCreate` の `Field(min_length=8, max_length=72)` と **バイト数を数える `@field_validator`**・`or_(...)` での重複チェック・`IntegrityError` → `rollback` → `DuplicateUserError` → **`409`**・**アカウント列挙**を避けるため「名前かメールアドレス」とあいまいに返す・`app/seed.py` にユーザー2人（`password123`）・**既存テーブルにあとから「空にできない列」を足すと `Cannot add a NOT NULL column with default value NULL`** → 生成物に **`server_default=sa.text('0')`** を足す）、**JWT**（`pyjwt==2.10.1` を別途 `pip install`（`jwt` / `python-jwt` は別物）・`ヘッダー.ペイロード.署名` の3部構成・**中身は誰でも読める（暗号化ではない）** → **秘密の情報を入れない**・`sub` / `iat` / `exp`・時刻は 1970 年からの秒数・**秘密鍵は `secrets.token_hex(32)`** で作り `.env` の `SECRET_KEY` に置く（`config.py` では**デフォルト値を書かない**ので、書き忘れると起動時に落ちる）・`ACCESS_TOKEN_EXPIRE_MINUTES=30`・`jwt.encode(payload, key, algorithm="HS256")`・`datetime.now(timezone.utc)`・**発行したトークンは取り消せない**ので期限を短くする・`jwt.decode(..., algorithms=[ALGORITHM])`（**`algorithms` を必ず渡す。`none` は論外**）・`ExpiredSignatureError` は `InvalidTokenError` の一種なので**細かいほうを先に `except`**・**改ざんすると `InvalidSignatureError`**・**鍵を変えると全トークンが無効**（漏洩時の唯一の手段））、**ログイン**（`app/routers/auth.py` の `POST /auth/token`・**`OAuth2PasswordRequestForm = Depends()`（括弧付き）**・**JSON ではなくフォーム形式**（`python-multipart` は `fastapi[standard]` に同梱）・`form.username` / `form.password`・**「ユーザーが無い」と「パスワードが違う」を区別しない**・`WWW-Authenticate: Bearer`・**ログにパスワードを出さない**・**`curl` では `--data-urlencode` を項目ごとに**（日本語を `-d` にそのまま書くと `401`））、**現在のユーザーの取得**（`OAuth2PasswordBearer(tokenUrl="auth/token")`・`get_current_user`（トークン → 名前 → **データベースを引き直す**）・`GET /users/me`・**`/docs` の「Authorize」ボタン**（日本語の入力も可・Logout してから入り直す）・**`401` の2種類**（`Not authenticated`＝ヘッダー無し／`トークンが正しくないか...`＝中身がだめ）・**`Bearer ` の後ろは半角スペース1つ**・**5.4.3 のハンドラに `headers=exc.headers` を足す**（無いと `WWW-Authenticate` が落ちる））、**保護されたエンドポイント**（**読むのは誰でも、書き換えるのは本人だけ**という方針・**`TaskCreate` から `owner` を外し、`current_user` から入れる**（`Owner` は不要になり `OwnerRead` は残す）・`get_my_task`（`get_task_or_404` と `get_current_user` を組み合わせ、他人のものなら **`403`**）・**判定の順番は依存の引数を書いた順**（`task` が先なので存在しない `id` はトークン無しでも `404`）・**認可の判定は依存にまとめる**（窓口ごとに `if` を書かない））、**やってはいけないこと**（秘密鍵をコードに書く／`.env` を共有する／`.env.example` に本物を書く／推測できる鍵にする／ログに出す・`.gitignore` は `.env` / `app.db` / `.venv/`・**上げてしまったら鍵を作り直す**・本番は環境変数を直接渡す・**トークンの置き場所**（`localStorage`＝XSS に弱い／`HttpOnly` クッキー＝CSRF 対策が要る／メモリ＝再読み込みで消える。**第9章では `localStorage` を使う**）・**トークンを URL に入れない**・**暗号やハッシュを自作しない**（`SHA-256` 1回は速すぎる・パスワードを暗号化して保存しない）・**足りていないもの**（HTTPS / ログアウト（失効）/ リフレッシュトークン / 試行回数の制限 / パスワード再設定 / メール確認 / 権限の役割分け / 2要素）） | `verify_password` で `==` を使って比べる、`UserRead` を作らず `response_model` なしでハッシュまで返す、ログインのボディを JSON で送る（`OAuth2PasswordRequestForm` はフォーム形式）、`secret_key` をコードに書く、`401` と `403` の取り違え、トークンの有効期限切れに気づかない |
| 8 | テスト | 第7章の範囲に加えて：**テストの必要性**（手で確認できる範囲は変更の影響範囲より狭い・**リグレッション（回帰）**・テストは仕様の記録にもなる・何度も直すものほど元が取れる）、**優先順位**（壊れると被害が大きいもの → 異常系 → 決めごと → 正常系。**正常系より異常系が先**・FastAPI / Pydantic 自身、`/docs` の見た目、ログの文言、外部サービスはテストしない）、**テストの3部構成**（準備・実行・確認。**1つのテストで確かめるのは1つ**）、**pytest**（`pytest==9.1.1` を別途 `pip install`・`tests/` に置く（`app/` の中には置かない）・**ファイル名も関数名も `test_` で始める**（外れると `collected 0 items` で静かに無視される）・関数名は日本語でよい・**`pytest.ini` の `pythonpath = .` と `testpaths = tests`**（無いと `ModuleNotFoundError: No module named 'app'`）・実行は `fastapi-lesson` で）、**`assert`**（合否はこれだけで決まる・メッセージは書かなくてよい・`is True` / `is None` / `in` / `len`・**`with pytest.raises(例外):`**・**アプリ本体では `assert` を使わない**（`python -O` で消える））、**失敗の読み方**（`>` の行が失敗した `assert`・`E` の行・**`+ where` に実際の値が出る**・`ファイル名::関数名` が住所・`short test summary info` を先に読む・`-q` / `-v` / `-x` / `-k` / ファイル指定・日本語の関数名は引用符で囲む・`warnings summary` はライブラリ側の事情で合否に影響しない）、**`TestClient`**（`from fastapi.testclient import TestClient` と `from app.main import app`・**サーバーを起動しない**が、依存・例外ハンドラ・ミドルウェアは本物が動く・httpx は `fastapi[standard]` に同梱・`response.status_code` / `response.json()`（辞書になる）/ `response.headers` / `response.text`・**渡すのはモジュールではなくアプリ**）、**API のテスト**（`200` と `404`・**エラーの形 `{"error": {...}}` の確認**・ログインは `POST /auth/token` に **`data=`（フォーム形式）**、ほかは `json=`・`headers={"Authorization": f"Bearer {token}"}`・補助関数は `test_` で始めない・**`422` は `msg` ではなく `type` を確かめる**（`detail` はリストなので `[0]["type"]`））、**テスト用のデータベース**（`app.db` を使うとデータが増える・消える・件数が分からない・実行順で結果が変わる・**本番を指していれば利用者のデータが消える**・`sqlite:///./test.db` に分ける・`.gitignore` に `test.db` と `.pytest_cache/`）、**fixture**（**`tests/conftest.py`**（名前は変えられない）・`@pytest.fixture` と `yield`（前が準備・後ろが後片付け）・**引数に名前を書くだけで使える**（`import` 不要）・fixture は fixture を引数に取れる・**同じ fixture は1テストにつき1回**（`Depends` と同じ）・毎回 `Base.metadata.create_all` / `drop_all`（**テストでは `create_all` でよい**。空から作り直すため）・`db` / `client` / `sato` / `yamada_task` / `sato_headers` / `login_as_sato`・**id を決め打ちしない**）、**`dependency_overrides`**（5.3.4 の回収・`app.dependency_overrides[get_db] = get_test_db`・**アプリのコードは1行も変えない**・`finally` で `clear()`・`get_current_user` を `lambda: sato` で差し替えると速いが、**認証そのものは確かめられなくなる**）、**続けるコツ**（**カバレッジを目標にしない**・「壊れたら困るもの」だけ書く・書きやすいもの／書きにくいものの区別・`assert` が5個以上なら分ける・同じ準備が3回以上なら fixture に・**バグを見つけたら、直す前に失敗するテストを書く**・テスト名はバグの内容が分かる形に・CI は docker-text へ） | `pytest.ini` を作らずに実行して `ModuleNotFoundError`、ファイル名・関数名を `test_` で始めない、`id` が 1 から始まる前提で書く、テスト用データベースを分けず `app.db` を壊す、依存性の差し替えを戻し忘れて次のテストに影響する |
| 9 | 実践：React と繋ぐ | 第8章の範囲に加えて：**オリジン**（スキーム・ホスト・ポート番号の3つ。パスは含まない・`localhost` と `127.0.0.1` は別のオリジン）、**同一オリジンポリシー**（別オリジンのデータを勝手に読ませない・ネット銀行の例）、**CORS**（サーバー側が許可を出す仕組み・**許可を出せるのはサーバーだけ**・`curl` / `/docs` / `TestClient` では起きない理由）、**症状の読み方**（サーバーのログは `-> 200` なのにブラウザは `blocked by CORS policy`＝届いて処理された上でブラウザが捨てた）、**プリフライトリクエスト**（`OPTIONS`・`GET` では飛ばず、JSON の `POST` / `Authorization` 付き / `PATCH` / `DELETE` で飛ぶ）、**`CORSMiddleware`**（`allow_origins` / `allow_credentials` / `allow_methods` / `allow_headers`・**`Authorization` を許可し忘れるとログイン後の操作だけ失敗する**・`add_middleware` はあとに足したものが外側・**`.env` を直したらサーバー再起動**（`settings` は起動時に1回だけ作られる）・`.env` のリストは JSON 記法・**末尾の `/` を付けない**）、`allow_origins=["*"]` を本番で使わない（`allow_credentials=True` と併用できない・**CORS はセキュリティの守りではない**＝ブラウザ以外からは呼べる）、**React 側の実装**（`src/api/client.js` と `src/api/tasks.js` に分離・`request` 関数に決めごとを集約・`response.ok` / `204` はボディなし / エラーは `{"error": {...}}` / `error.status = response.status` で番号を持たせる・**`toTask` による形の変換**（`done`→`isDone`、`owner.name`→`ownerName`、包み `{count, tasks}` の1階層）・**`?limit=100`**（既定は10件）・`created_at` が無いので並べ替えは `id`）、**ログイン**（`POST /auth/token` は `URLSearchParams` でフォーム形式・`Authorization: Bearer <トークン>`（半角スペース1つ）・トークンは `localStorage`（7.6.2 の弱点つき）・`GET /users/me` で名前を確かめる・依存配列 `[token]` での再取得・**期限切れなら捨ててログインフォームに戻す**）、登録・更新・削除（`JSON.stringify`・`PATCH` は変えたい項目だけ・更新は「返ってきた1件で置き換える」／削除は「一覧を取り直す」・**画面だけ書き換えて API を呼ばないとリロードで戻る**・`403` がそのまま画面に出る）、**2つのサーバー**（`fastapi dev app/main.py`＝8000 と `npm run dev`＝5173・ターミナル2つ・起動のたびに仮想環境の有効化・ポートの取り違え）、**画面の4状態**（読み込み中／エラー／0件／表示）と `finally` での `setIsLoading(false)`、**`error.status` が `undefined`＝届いていない／数値＝届いて拒否された**の出し分け、**二重送信の防止**（`disabled` と「送信中...」）、**`422` の表示**（`detail[0].loc` の最後と `type` から日本語を組み立てる・`msg` は版で変わるので使わない・`FIELD_LABELS[field] \|\| field`）、**入力チェックは両側**（フロントは親切・**サーバーが守り**）、**切り分け**（リクエストが出ているか→ブラウザの外（`/docs` / `curl` / `pytest`）でも失敗するか）、**Network タブ**（Status / Request Headers / Payload / Response・`OPTIONS` の行がプリフライト）と**サーバーのログ**の突き合わせ、症状別の早見表 | CORS のエラーを見て FastAPI 側のコードを疑って探し回る、許可するオリジンの末尾に `/` を付ける、ポート番号を書き忘れる、`Bearer ` を付け忘れる、トークンを `localStorage` に置いたまま XSS の注意を読み飛ばす、サーバーを再起動していない |
| 10 | 次のステップ | 第9章の範囲に加えて：到達度チェックリスト（32項目・戻る場所つき）、3冊が1本に繋がった図（追加操作がどの章の部品を通るか）、**デプロイ**（「置く・動かし続ける・安全に外へ出す」の3つ・`127.0.0.1` は自分だけ・手元と本番の対応表）、**`fastapi run`**（`production mode` と表示・自動リロードなし・`0.0.0.0` で待ち受ける・**これだけでは公開されない**）、手元と本番の違い（DB は SQLite → MySQL / PostgreSQL・設定は `.env` → 環境変数・鍵は本番専用・CORS は本番の画面の URL・HTTP → HTTPS・落ちたら自動で起動し直す）、**死活監視**（`/health`・認証を付けない（監視はログインできない）・短い JSON を返す）、**`503`**（Service Unavailable。**`500`＝バグ / `503`＝一時的な不調**で受け取る側の行動が変わる）、`select(Task.id).limit(1)` による「読めるかどうか」だけの確認、**`SQLAlchemyError`**（`sqlalchemy.exc`。`IntegrityError` / `OperationalError` の親）、公開前チェック8項目（鍵の作り直し・`.env` を出さない・`debug` は `False`・`cors_origins`・練習用データを流さない・HTTPS・`detail` に内部情報を入れない・バックアップ）、**`.env.example` は放っておくと古くなる**（`Settings` と突き合わせる手順。第9章の `CORS_ORIGINS` が実際に抜けている）、置き場所の3分類（VPS / PaaS / コンテナ実行サービス。**具体的な手順は出さない**）、他人に渡すのに要るもの8個の棚卸し、**手順書の書き方**（「必要なもの」と「動かし方」を分ける・コピペできる形・成功の判断基準・**手順書は作業を無くさない**）、Docker で何が置き換わるか、`.env` の `DATABASE_URL` 1行で MySQL に差し替えられること。**この章のよくあるつまずき**：`/health` を作って `include_router` を書き忘れる（`404` になる）、`try` の範囲を広げすぎる／`except Exception:` にしてバグを `503` で隠す、`.env.example` に本物の鍵を書く、`.env.example` を作って `.env` を消す（アプリが読むのは `.env` だけ）、`.venv` をコピーして持ち回る | `127.0.0.1` のままで外から呼べると思う、`.env` をそのまま公開先に持っていく、手元と同じ `secret_key` を本番でも使う、CORS の許可先を `*` のままにする、手順書に「どうなれば成功か」を書かない |
| 解答編 | その1（第1章〜第5章）／その2（第6章〜第10章） | — | — |

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

> **注意**：第7章の学習者のアプリには、**ユーザーとログイン**があります。
> パスワードのハッシュ化は **bcrypt**、トークンは **PyJWT（`HS256`）**で統一しています。
> `passlib` / `python-jose` を使った例（検索するとこちらが多く出てきます）は、
> **このテキストの構成とは合わないので見せないでください。**
> ログインの窓口は `POST /auth/token`（**フォーム形式**。`OAuth2PasswordRequestForm`）、
> 現在のユーザーは `Depends(get_current_user)` で受け取る形です。
>
> **セキュリティに関わる相談では、次を絶対に肯定しないでください。**
>
> | 見かけたら止めるもの | 正しい形 | 参照 |
> |------------------|---------|------|
> | パスワードを平文で保存する | `hash_password`（bcrypt） | 7.2.1 |
> | `SHA-256` / `MD5` を1回かけて保存する | **パスワード用の関数を使う**（速すぎる・ソルトが無い） | 7.6.3 |
> | ハッシュ同士を `==` で比べる | `verify_password`（`bcrypt.checkpw`） | 7.2.3 |
> | 秘密鍵をコードに書く | `.env` の `SECRET_KEY` | 7.4.2・7.6.1 |
> | `jwt.decode` の `algorithms` を省く・`"none"` にする | `algorithms=["HS256"]` | 7.4.4 |
> | JWT にパスワードなどの秘密を入れる | **中身は誰でも読める** | 7.4.1 |
> | `UserRead` に `hashed_password` を入れる | 返す形に書かない | 7.3.1 |
> | トークンを URL に付ける | `Authorization` ヘッダー | 7.6.2 |
>
> **学習者が貼ったトークン・パスワード・`SECRET_KEY` は、そのまま使わせないでください。**
> 「その値は公開されたものとして扱い、作り直してください」と伝えてください。
>
> よくある相談と、最初に確認させるべきことは次のとおりです。
>
> | 症状 | 最初に疑うもの |
> |------|--------------|
> | `{"message":"Not authenticated"}` の `401` | **`Authorization` ヘッダーの付け忘れ**か `Bearer ` の書き忘れ（7.5.2） |
> | `トークンが正しくないか、有効期限が切れています` の `401` | 期限切れ（既定 30 分）か、`SECRET_KEY` を変えた（7.4.3・7.4.4） |
> | ログインが必ず `401` になる | `curl` で日本語を `-d` に直接書いている（**`--data-urlencode`**。7.5.1） |
> | 起動時に `Field required ... secret_key` | `.env` に `SECRET_KEY` が無い（7.4.2） |
> | `Cannot add a NOT NULL column with default value NULL` | あとから足す列に **`server_default`** が要る（7.3.1） |
> | 他人のタスクを消せてしまう | `Depends` が `get_task_or_404` のままで `get_my_task` になっていない（7.5.3） |
> | 誰も正しいパスワードでログインできない | `verify_password` を `==` で書いている（7.2.3） |
>
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | `TestClient` / pytest（`dependency_overrides` は 5.3.4 で紹介済み） | 第8章 |
> | CORS・React との接続・ブラウザ側でのトークンの保持 | 第9章 |
> | ログアウト（トークンの失効）・リフレッシュトークン・パスワード再設定・2要素認証 | **このテキストでは扱わない**（7.6.3 に一覧） |
> | テーブル同士の関連（外部キーで `tasks` と `users` を結ぶ） | mysql-text |
>
> **本人かどうかの判定は `owner_name` の一致**で行っています（外部キーを使っていないため）。
> 「`user_id` を持たせるべきでは」という相談には、
> **その考え方が正しいことを認めたうえで、mysql-text で扱うと伝えてください**（7.5.3 の補足）。

> **注意**：第8章の学習者のアプリには、**テスト**があります。
> `fastapi-lesson/tests/` に `conftest.py` と `test_*.py` を置き、
> `fastapi-lesson/pytest.ini` に `pythonpath = .` と `testpaths = tests` を書いた形で統一しています。
> テストは **`test.db`**（`app.db` とは別のファイル）を使い、
> `app.dependency_overrides[get_db]` で差し替えています（8.4.3）。
>
> **`unittest` / `nose` の書き方（`class TestXxx(unittest.TestCase):` や `self.assertEqual(...)`）は
> 見せないでください。** 検索すると出てきますが、このテキストは **pytest の関数と `assert`** で統一しています。
> `pytest-asyncio` / `AsyncClient` も使いません（窓口はすべて `def` で書いているため。5.6.2 の注記と同じ理由です）。
>
> よくある相談と、最初に確認させるべきことは次のとおりです。
>
> | 症状 | 最初に疑うもの |
> |------|--------------|
> | `ModuleNotFoundError: No module named 'app'` | `pytest.ini` の `pythonpath = .` か、**実行した場所**（8.2.1） |
> | `collected 0 items` | ファイル名・関数名が **`test_` で始まっていない**（8.2.1） |
> | `fixture 'client' not found` | `conftest.py` の名前か置き場所（`tests/` の直下か）（8.4.2） |
> | テストのたびに `app.db` が増える | `client` fixture を使わず `TestClient(app)` を直接作っている（8.4.2） |
> | `count` が想定と合わない | 前のテストのデータが残っている（`drop_all` まで書けているか）（8.4.2） |
> | ログインのテストだけ `422` | `json=` で送っている。**ログインは `data=`**（8.3.3・7.5.1） |
> | `422` のテストが版上げで落ちる | `msg`（英語）を確かめている。**`type` を見る**（8.3.4） |
> | `401` が返るはずのテストが `201` になる | `get_current_user` を差し替えたままにしている（8.4.3） |
>
> **「テストが通らないので、テストのほうを消す・易しくする」という助言はしないでください。**
> まず、そのテストが何を守っているのかを本人に言わせてください（8.5.2）。
>
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う章 |
> |------------|-------|
> | CORS・React との接続・ブラウザ側でのトークンの保持 | 第9章 |
> | カバレッジの計測（`pytest-cov`）・CI（GitHub Actions） | **このテキストでは扱わない**（8.5.1・8.5.2 で名前だけ） |
> | モック（`unittest.mock` / `monkeypatch`） | **このテキストでは扱わない。** 差し替えは `dependency_overrides` で行う |
> | テーブル同士の関連（外部キーで `tasks` と `users` を結ぶ） | mysql-text |

> **注意**：第10章の学習者は、**この本を読み終えています。**
> 第9章までのアプリ（`fastapi-lesson` と `task-app`）が、手元で通しで動く状態です。
>
> **この章は概観です。実際にデプロイはしていません。**
> 本文が扱ったのは、手元と本番の違い・公開前のチェック8項目・死活監視（`/health`）・
> 手順書の書き方までで、**特定のサービスへの配置手順は意図的に書いていません**（10.2.4）。
> 「どこに置けばいいか」と聞かれたら、**サービスごとの手順を並べるのではなく**、
> 10.2.4 の3分類（VPS / PaaS / コンテナ実行サービス）を示し、
> **先に docker-text へ進むほうが速い**と伝えてください。
>
> **`/health` のコードは本文にありません。演習 10.1 と演習 10.4 の課題です。**
> `select(Task.id).limit(1)` と `SQLAlchemyError` と `503` は 10.2.2 に出ていますが、
> **窓口のコード全体を先に出さないでください**（`ai/hint-policy.md` の段階に従ってください）。
>
> また、次のものはまだ使えません。
>
> | 使えないもの | 扱う本・章 |
> |------------|-----------|
> | Docker（`Dockerfile` / `docker compose`） | docker-text（**名前だけ 10.3.2 で出る**） |
> | MySQL への接続・SQL の読み書き | mysql-text（**差し替えの話だけ 10.3.2 で出る**） |
> | Nginx などリバースプロキシの設定 | このテキストでは扱わない（10.2.2 で図と名前だけ） |
> | HTTPS 証明書の取得・設定 | このテキストでは扱わない |
> | CI（GitHub Actions）・カバレッジ | このテキストでは扱わない |
> | Git / GitHub の操作 | react-text 11.4（**この本では前提にしない**） |
>
> **`fastapi run` を「これで公開できる」と説明しないでください**（10.2.2 の注意）。
> `0.0.0.0` で待ち受けても、自宅のパソコンならインターネットからは届きません。
>
> 相談が「本番の鍵」「`.env` を GitHub に上げてしまった」といった内容だった場合は、
> **10.2.3 と 7.6.1 に沿って、鍵の作り直しを最優先で案内してください。**

## 4. docker-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに（前提：react-text と fastapi-text で「動くアプリ」を作った経験） | （コードなし）この本の前提（react-text の `cd` / 拡張子の表示 / `npm run dev`、python-text の venv / `pip`、fastapi-text の `requirements.txt` / サーバー起動 / `.env` の戻り場所つき対応表）、`node --version` / `python --version`（`python3 --version`）による確認（**入っていなくても第5章までは読める。第6章までに必要**）、**この本が解決する問題**（3冊で踏んだ環境構築のつまずきの一覧・「自分の環境では動く」問題＝渡す相手に7手順を伝えることになる・パソコンを買い替えると自分でやり直す羽目になる）、**ゴールの形**（起動が `docker compose up` の1コマンドになる。**コマンド自体は第5章、実際に組むのは第6章**）、**手順書を人間の記憶からファイルへ移すのがこの本のテーマ**、Docker は速くする道具ではない、全5冊での位置づけ（4冊目・mysql-text が Docker のあとに来る理由）、章ごとに身につくこと（**山場は第2章と第6章**）、AI サポートの準備（`ai/instructions.md` の読み込み・**OS に加えて CPU（Apple Silicon か否か）を伝える**・動作確認の2つの質問・章番号を添える意味）、**Docker で環境依存が出やすい要因**（OS／Windows のエディション／WSL2／BIOS・UEFI の仮想化設定／CPU／ネットワーク／ディスク空き容量）と用語の顔見せ（**仮想化・WSL2・BIOS/UEFI は言葉の紹介だけ。仕組みは第1章・第2章**）、**この本のトラブルはほぼレベル C**（AI に手順を最後まで出させてよい）、質問テンプレート（章番号／環境／やろうとしたこと／打ったコマンド／**メッセージ全文**／試したこと）と**秘密情報を書き換えてから貼る**、進め方（ターミナルが主役・打つ場所を確認する・**壊して作り直せる**が「消す」対象は確認する・書く→ビルド→起動→確認→ログの輪・**空き容量 20 GB**）、詰まったときの順番（Docker Desktop の起動 → ディレクトリ → コンテナの状態 → メッセージの最終行 → AI → 飛ばす。**第2章だけは飛ばせない**）、★の意味と解答編の使い方 |
| 1 | Docker が解決する問題 | （コマンドなし）**アプリが動く土台の4層**（① 設定（`.env` / PATH）／② ライブラリ（`npm install` / `pip install`）／③ ランタイム（Node.js・Python 本体）／④ OS（パスの区切り・改行コード・シェル）・**コードを渡すのは①だけ**・3冊で踏んだトラブルを4層に割り当てる表）、**組み合わせ爆発**（OS × CPU × Python × Node.js × シェルで 162 通り・**全部の確認は不可能**・だから「組み合わせを減らす」ではなく**1つに固定して環境ごと配る**）、**既存の解決策の限界**（手順書／`package.json`・`requirements.txt`／`venv`／セットアップスクリプトが揃えられるのは②まで。**③ランタイムと④OS は揃わない**・`venv` でも Python 本体はパソコンのものを使う）、**仮想マシン**（ホスト OS／ゲスト OS／ハイパーバイザ／**カーネル**（OS の中心部分）・**アプリごとにゲスト OS を丸ごと持つ**・数 GB・起動は数十秒〜数分・メモリを先に割り当てる・隔離は強い）、**コンテナ**（**カーネルはホストの1つを共有**・ファイル／プロセス／ネットワークだけ分ける・**「自分専用のファイル一式を持たされたプロセス」**・数十〜数百 MB・起動は数秒・**Windows / macOS では Linux の仮想マシンを1つだけ動かし、その中にコンテナを並べる**（WSL2 が第2章で必要になる理由））、**比較表**（サイズ・起動時間・メモリ・隔離の強さ・OS の自由度）と**「コンテナは軽い仮想マシンではない」**（中身は原則 Linux・隔離は仮想マシンほど強くない）、**イメージ**（コンテナの設計図・**読み取り専用**・レイヤの存在は名前だけ（第3章）・**`名前:タグ`**（`python:3.13-slim`）・**レジストリ / Docker Hub**（ブラウザで見られる）・公式イメージ・Tags タブの **Compressed size** の読み方（`nginx` の例）・**`latest` は「最新」ではなく中身が変わりうる名前**（詳細は 2.5.5））、**コンテナ**（イメージを動かした実物・**設計図と建物**のたとえ・イメージの上に**書き込み層が1枚**乗る・**削除すると書き込み層ごと消える**・**停止と削除は別**（2.4）・**中でした手作業はイメージに残らない**（第3章 Dockerfile の動機））、**1つのイメージから何個でもコンテナを作れる**（起動直後の中身は必ず同じ・設定だけ変えて並べられる）、**Docker で変わること**（手順の **A（Docker が肩代わり）／B（人が1回だけ）／C（それでも人がやる）** への仕分け・**パスワードなど人が決めて渡すものは C**・**Docker Desktop の導入という手順は増える（B）**・「1コマンド」の前に**誰かが設定ファイルを書く**／**消して作り直せる**（必ず起動直後に戻る。ただし**コンテナ内のデータは戻らない**（第4章 4.1）・パソコン上のファイルは Git の領域）／**本番環境と同じイメージを手元で動かせる**（デプロイ・開発環境／本番環境。ただし **CPU の種類（Apple Silicon / Intel）の違いは残る**（2.2.3）・GUI アプリや Windows / macOS 専用ソフトは動かない）） |
| 2 | インストールと基本操作 | 第1章の範囲に加えて：**Docker Desktop の正体**（**CLI（注文する側）／デーモン（作る側）／GUI** の詰め合わせ・デーモンは画面を持たず裏で動き続ける）、**インストール**（Windows：`Docker Desktop Installer.exe`・**「Use WSL 2 instead of Hyper-V」にチェック**・再起動が必須／macOS：**先に  →「このMacについて」で Apple Silicon か Intel かを確認**してから `.dmg` を選ぶ・初回起動で Mac のログインパスワード（**Docker アカウントは不要**）／**大企業での業務利用は有料**）、**3段階の起動確認**（クジラのアイコンが `running` → **`docker --version`（CLI だけ。デーモンが死んでいても通る）** → **`docker version` で `Client:` と `Server:` の両方**。**`--version` だけで成功と判断しないこと**）、**トラブル**（切り分けフローチャート・**2.2.1 WSL2**（`wsl --status` / `wsl -l -v` の **VERSION が 2**・`wsl --install` / `wsl --update` は管理者 PowerShell・`wsl --set-default-version 2`）・**2.2.2 BIOS の仮想化**（タスクマネージャー → パフォーマンス → CPU → **「仮想化: 有効」**・`Intel VT-x` / `SVM Mode`・Windows の機能「仮想マシン プラットフォーム」「Linux 用 Windows サブシステム」・**2.2.2 → 2.2.1 の順でしか解決しない**）・**2.2.3 Apple Silicon**（**arm64 / amd64**・`docker version` の `OS/Arch`・`The requested image's platform ... does not match` の警告・`--platform linux/amd64` は**遅くなる最後の手段**・Rosetta 2）・**2.2.4 Cannot connect to the Docker daemon**（＝デーモンが動いていない、の一言・起動待ち → Restart → 再起動の順）・**2.2.5 プロキシ**（`TLS handshake timeout` / `x509: certificate signed by unknown authority` / `no such host`・Settings → Resources → Proxies・**テザリングで切り分ける**・**証明書の検証を切る回避策は使わない**））、**`docker run`**（**①取得 ②作成 ③起動の3つをまとめて実行**・`Unable to find image ... locally` はエラーではない・**2回目はダウンロードが走らない**・`hello-world` → `nginx:1.27`・`--name` / **`-p 外側:内側`**（**変えてよいのは左だけ**）/ `-d`（バックグラウンド）/ `--rm`（終了と同時に削除）・**イメージ名のあとにコマンドを書くと、そのコマンドが実行される**（`docker run --rm nginx:1.27 nginx -v`））、**コンテナの操作**（`docker ps` / **`docker ps -a`（停止中はこちらにしか出ない）**・列の意味と **`STATUS` の読み方**（`Up ...` / `Exited (0)` / `Exited (127)` / `Created`）・`stop`（**10 秒待って強制終了**）/ `start`（**`-p` や `--name` は覚えている**）/ `rm` / `rm -f`・**動いているコンテナは `rm` できない**・**状態遷移図（実行中 ⇄ 停止中 → 削除）**・**停止中のコンテナは書き込み層・名前・ディスクを押さえ続ける**（`Conflict. The container name "/web" is already in use`）・`port is already allocated`）、**イメージの操作**（`docker images`（**Docker 29 は `IMAGE` / `ID` / `DISK USAGE` / `CONTENT SIZE`、28 以前は `REPOSITORY` / `TAG` / `IMAGE ID` / `CREATED` / `SIZE`。列ではなく見出しを読む**）・`docker pull`（`docker.io/library/...` の正式名・`Image is up to date`・**取得回数の上限**）・`docker rmi`（**コンテナが使っていると消せない**（停止中でも）・`Untagged:` と `Deleted:` の違い・**`rm` → `rmi` の順**）・**タグは中身と1対1ではない**（`nginx:1.27` の中身は `1.27.5`）・ダイジェスト（`sha256:` は名前だけ）・**`latest` を使わない理由**（中身が黙って変わる／手元と相手で違う／原因調査ができない））、**中を見る**（`docker exec 名前 コマンド`・**`docker exec -it 名前 bash`**（`-i`＝入力を送り続ける／`-t`＝ターミナルとして扱う・**`bash` が無いイメージは `sh`**・`exit` で抜けてもコンテナは止まらない・**中身は Debian などの Linux**・**中でした変更はイメージに残らない**）・`docker logs` / `--tail N` / `-f`（**`-f` の `Ctrl`+`C` はログ表示をやめるだけ**）・**落ちたコンテナは消す前にログを読む**・`docker cp`（**両方向**・`コンテナ名:パス` の形・**その場しのぎ**であり、恒久的な変更は `COPY`（第3章）かバインドマウント（第4章）））、**掃除**（`docker system df`（`TOTAL` / `ACTIVE` / `SIZE` / **`RECLAIMABLE`**）・`container prune` / `image prune` / `image prune -a` / `system prune` / `system prune -a` / **`system prune -a --volumes`** の**危険度の違いと包含関係**・確認メッセージ `[y/N]` は**そのまま Enter だと「いいえ」**・**`--volumes` は保存データを消すのでこのテキストでは使わない**・**測る → 見る → 消す → もう一度測る**の順・`prune -a` より `rmi` で1つずつ） |
| 3 | Dockerfile | 第2章の範囲に加えて：**Dockerfile**（イメージの作り方を書いたファイル・**拡張子なしの `Dockerfile`**・1行1命令・命令は大文字・上から順に実行）、**練習用プロジェクト `docker-lesson`**（`main.py` + `requirements.txt` + `Dockerfile` + `.dockerignore`。**パソコン側には `pip install` しない**）、**基本の命令**（**`FROM`**（1行目・ベースイメージ・**ここで OS とランタイム（第1章の③④）が確定**・`python:3.13-slim`・**タグを省略しない**・`slim` の意味は名前だけ、選び方は第7章 7.3）／**`WORKDIR`**（コンテナの中の `cd`・無ければ自動で作られる・**このテキストは `/code` で統一**（`fastapi-lesson` の `app/` と紛らわしいため））／**`COPY 元 先`**（**ビルド時**に入り、**イメージに残る**（`docker cp`（2.6.3）との比較表）・コピー先に絶対パスも書ける・**ビルドコンテキストの外は指定できない**（`COPY ../secret.txt` は `not found`））／**`RUN`**（**ビルド時に1回**・`pip install --no-cache-dir -r requirements.txt`・**コンテナの中では venv を作らない**・**標準ライブラリだけなら `RUN` は不要**・サーバー起動を書くとビルドが終わらない）／**`CMD ["a", "b"]`**（**起動のたび**・角かっこの形で書く（シェル形式は `docker stop` の合図が届かない）・**書かないとベースイメージの `CMD` を引き継ぐ**・**`fastapi dev` ではなく `fastapi run`**（`dev` は `127.0.0.1` で待つためコンテナの外から繋がらない。`0.0.0.0` の詳細は 4.4.3））／**`ENV 名前=値`**（`docker run -e` で上書きできる・**イメージに残り誰でも読めるので秘密の値を書かない**）／**`EXPOSE`**（**申告だけ。公開するのは `-p`**）／**`ENTRYPOINT`**（**`CMD` は丸ごと差し替えられ、`ENTRYPOINT` は後ろに追加される**・2つ併用で「実行は固定・引数に既定値」・**このテキストは原則 `CMD` のみ**））、**ビルド**（**`docker build -t 名前:タグ .`**・**最後の `.` はビルドコンテキスト**（材料一式をデーモンに送る。`COPY` できる範囲もこれ）・出力の読み方（`[n/m]` の行・`FINISHED`・`naming to`・**`ENV` / `EXPOSE` / `CMD` は工程に出ない**）・`.` 忘れと `Dockerfile.txt` のエラー文言・**自分のイメージにもバージョンのタグを付ける**（`latest` を作らない。2.5.5）・`Untagged:` だけが出る意味・`docker run -e` での上書き・**イメージ名のあとにコマンドを書くと `CMD` が差し替わる**（`python --version` / `pip list` で中身を調べる））、**レイヤとキャッシュ**（**1命令 = 1レイヤ**・**`docker history`**（下から上に読む・容量を食うのは `RUN` だけ・`ENV` / `EXPOSE` / `CMD` は 0B・ベースイメージの命令も見える）・**キャッシュの判定**（命令の文字列が同じか／`COPY` はファイルの中身が同じか）・**1つ崩れると以降は全部作り直し**（`CACHED` は必ず上から連続する）・**依存インストールを先に書く**（`COPY requirements.txt .` → `RUN pip install` → `COPY . .`。順番を入れ替えると、コードを1文字変えるたびに `pip install` が道連れで再実行される。**変わりにくいものを先に、変わりやすいものを後に**）・**`--no-cache`**（`apt-get` を含むイメージの更新・原因不明の切り分け・**まず順番を疑う**）・`docker builder prune`）、**`.dockerignore`**（**ビルドコンテキストから外す**・`.venv/` / `node_modules/` / `.git/` / **`.env`** / `*.db` / `__pycache__/`・書き方は `.gitignore` とほぼ同じ（`#` コメント・`*`・`!` で取り消し）・**置く場所は `docker build` に指定したディレクトリ**・`.gitignore` があっても別途必要・**ビルドが遅いときは `transferring context` の数字と、どの `[n/m]` で止まっているかを見る**）、**`fastapi-lesson` のイメージ化**（追加するのは `.dockerignore` と `Dockerfile` の2ファイルだけ・`CMD ["fastapi", "run", "app/main.py", "--port", "8000"]`・`.env` を除外するので `app/config.py` の既定値が使われる（**既定値の無い設定があるときは `docker run --env-file .env`**）・**`.dockerignore` の有無で送る量が数百 MB 変わる**・起動後の `GET /tasks` は **`no such table: tasks` で `500`**（`app.db` を焼き込んでいないため。**焼き込まないのが正しい**）→ `docker exec -it api bash` から **`alembic upgrade head`** と **`python -m app.seed`**（fastapi-text 6.6）→ 動く → **コンテナを作り直すと消える**（書き込み層にあるため。`RUN alembic upgrade head` でも解決しない）→ **第4章のボリュームへ**） |
| 4 | ボリュームとネットワーク | 第3章の範囲に加えて：**マウント**（コンテナの中のパスを外の入れ物に繋ぎ替える・**`-v 外側:内側`**・**左がパスならバインドマウント、左が名前なら名前付きボリューム**・`-p 外側:内側` と同じ「左が外・右が中」）、**データが消える理由の再確認**（`docker run -d python:3.13-slim sleep 600` で作ったファイルは `docker rm` で書き込み層ごと消える・**コンテナはコマンドが終わると終了する**ので `sleep` で生かす）、**バインドマウント**（`-v "${PWD}/site:/usr/share/nginx/html"` で nginx のページを差し替える（**ビルド不要**・演習 3.2 の `COPY` との対比表）・**存在しないパスを書いてもエラーにならず空のディレクトリが繋がる**・**開発モード**＝`-v "${PWD}:/code"` + `fastapi dev app/main.py --host 0.0.0.0 --port 8000`（`CMD` の差し替え・`Will watch for changes in these directories: ['/code']` / `WatchFiles detected changes ...` の読み方）・**マウントで置き換わるのはマウントしたパスだけ**（`site-packages` は消えない）・**配るイメージは `COPY` + `fastapi run` のまま**・パスは絶対パス（**Windows は `${PWD}`、macOS / Linux は `$(pwd)`**・空白があれば `"` で囲む・macOS の `/Users` 外は File sharing の設定）・**`docker inspect コンテナ名 --format '{{json .Mounts}}'`**（`Type` / `Source` / `Destination` / `RW`）・**権限**（コンテナが作ったファイルは Linux では `root` のもの・`__pycache__`・**`:ro` はコンテナ側の書き込みだけを禁止する**（手元での編集は反映される）・**`.dockerignore` はマウントには効かない**））、**名前付きボリューム**（`-v api-data:/data`・**無ければ自動で作られる**・`docker volume create` / `ls` / `inspect`（`Mountpoint` は **Windows / macOS では Linux 仮想マシンの中**で開けない）/ `rm`（**使用中は `volume is in use`**）・**`docker volume prune` は匿名ボリュームしか消さない**（名前を付ける利点）・`docker system df` の `Local Volumes`）、**使い分け**（**人間が直接開くならバインドマウント、開かないならボリューム**・`fastapi-lesson` は **`-v api-data:/data -e DATABASE_URL=sqlite:////data/app.db`**（**スラッシュ4本＝絶対パス**）で `app.db` を残す（3.6.3 の宿題の回収。作り直しても `alembic` / `seed` が不要になる）・**コードのパスに名前付きボリュームを被せない**（空のときだけイメージの中身がコピーされ、以降は古いまま残る））、**ポート公開**（`-p` は通信版のマウント・**左は自由、右はアプリが待つ番号**・**同じイメージから複数のコンテナを別ポートで同時に動かせる**・`-p` は何度でも書ける・**`port is already allocated`** → `docker ps -a`（**停止中も `-p` を押さえている**）→ OS 側のプロセス（`Get-NetTCPConnection` / `lsof -i`）→ **左の番号を変える**・左右の書き間違いは**エラーなしで繋がらない**）、**`0.0.0.0` と `127.0.0.1`**（コンテナの中の `127.0.0.1` は**コンテナ自身**・`docker ps` も `docker logs` も正常に見えるのに繋がらない・**判定は `Uvicorn running on http://...` の行**・**`fastapi run` は既定で `0.0.0.0`、`fastapi dev` は `127.0.0.1`**・公開範囲を決めているのは `0.0.0.0` ではなく `-p`）、**ネットワーク**（**既定ではコンテナ名を引けない**（`Name or service not known`）・`docker network create` / `ls`（`bridge` / `host` / `none` + 自作）/ `inspect --format '{{json .Containers}}'` / `connect` / `disconnect` / `rm`・**`--network` は起動時に指定**（作り直しになるが、データはボリュームにあるので平気）・**自作ネットワークではコンテナ名で名前解決できる**・**コンテナ同士の通信に `-p` は関係ない**（相手の**中のポート**を書く）・**コンテナの中の `localhost` はそのコンテナ自身**（`Connection refused`）・`host.docker.internal`・**React から呼ぶ URL は `localhost` のまま**（ブラウザはコンテナではない）・**第5章の Compose では自動化される**）、**Windows 特有の問題**（**CRLF**（`exec ./start.sh: no such file or directory` は**スクリプトではなく `/bin/sh\r` が無い**という意味・VS Code のステータスバーで `CRLF` → `LF`・`cat -A` の `^M$`・`.env` の値の末尾にも入りうる）・**`.gitattributes`**（`* text=auto eol=lf` / `*.sh text eol=lf`・`git add --renormalize .`・VS Code の `files.eol` は自分にだけ効く）・**ファイル監視が届かない**（`WatchFiles detected changes` が出ない → **WSL2 側にプロジェクトを置く**（速度も上がる）か **`WATCHFILES_FORCE_POLLING=true`**（CPU を使う）・Vite は `CHOKIDAR_USEPOLLING`（第6章））） |
| 5 | Docker Compose | 第4章の範囲に加えて：**Compose の動機**（第4章の長い `docker run`（`--network` + `-p` + `-v` + `-e` + イメージ）と事前の `docker network create` が、記憶とコマンド履歴にしか残らない＝第1章 1.1.3 の「手順書」への逆戻り・**構成をファイルに書き残す**のがこの章）、**`docker compose`**（Docker Desktop に同梱・`docker compose version` で確認・**スペース区切り**（旧 `docker-compose` ハイフンは読み替える）・`docker run` との対応表）、**YAML**（**「項目名: 値」＋コロンのあと半角スペース**・**入れ子は字下げ（スペース2つ・波かっこの代わり）**・**タブ禁止**・リストは行頭 `-`・`#` コメント・**エラーには行番号が出るのでその行の字下げを見る**）、**最小構成**（`compose-lesson/compose.yaml`・**`services:` → サービス名 → `image:` / `ports:`**・**コンテナ＝サービス**・`docker compose up`（ログを流す・`Ctrl`+`C` で停止だが削除はしない）・`Network プロジェクト名_default Created` が自動で出る・タブ混入時のエラーと VS Code の「スペース: 2」）、**`build` と `image`**（`image: 名前:タグ`（既存）／**`build: .`**（`Dockerfile` からその場でビルド・`.` はビルドコンテキスト＝3.3.1 と同じ）・イメージ名は `プロジェクト名-サービス名` が自動付与・`up` はイメージが無ければ自動ビルド・`build:` と `image:` の併用でビルド結果に名前を付けられる）、**`ports` / `volumes` / `environment`**（`docker run` の `-p` / `-v` / `-e` と1対1・**名前付きボリュームは「サービス内の `volumes:`」と「ファイル末尾の `volumes:` 宣言」の2か所**（片方忘れは `refers to undefined volume`）・`environment` は「項目名: 値」か「`- 名前=値`」・**Compose のボリューム/ネットワークにはプロジェクト名の接頭辞**（`fastapi-lesson_api-data` は第4章の `api-data` と別物））、**複数サービス**（`api` に加え **`db`（`mysql:8.4`）** を並べる・**この章では API を MySQL に繋がない**（api は SQLite のまま・接続は第6章）・**公開しない `db` に `ports` を書かない**（コンテナ同士は 4.5 のとおり公開不要）・MySQL は初回 `ready for connections` まで十数秒〜数十秒・`MYSQL_ROOT_PASSWORD` / `MYSQL_DATABASE`）、**サービス名で通信**（`docker compose exec api python -c "socket.gethostbyname('db')"` で IP が引ける・存在しない名前は `Name or service not known`（4.5.1 と同じ）・相手は `localhost` でなく**サービス名**（4.5.4））、**ネットワークは自動**（`プロジェクト名_default`・`docker network create` も `--network` も不要・第4章の手作業が全部自動）、**基本コマンド**（`up` / `up -d` / **`down`（ボリュームは残す）** / **`down -v`（ボリュームごと消す＝データが消える）** / `ps` / `logs [サービス]` / `-f` / **`exec サービス名`**（4.3.3 の `alembic upgrade head` / `python -m app.seed` の Compose 版）/ `run --rm`（1回だけ）/ `config`（`.env` 差し込み後の表示）・**コードを直したら `up -d --build`**（付け忘れは古いイメージのまま＝4.3.3 とは別の「反映されない」原因））、**環境変数**（`environment` の2記法・**`.env` を同ディレクトリに置くと自動で読み `${...}` を置換**（`docker compose config` で確認）・`env_file:`（中身をコンテナに渡す）との違い・**`.env` は `.gitignore` で共有せず、`.env.example` で項目だけ共有**（fastapi 4.6 と同じ）・第7章で本番向けの厳密化）、**起動順の制御**（**`depends_on: - db` は「コンテナが起動した」だけを保証し「準備できた」は保証しない**（起動直後は `Connection refused` の瞬間がある）→ **ヘルスチェック**（`healthcheck: test/interval/timeout/retries/start_period`・`mysqladmin ping -h 127.0.0.1`・「応答するか」だけを見る）＋ **`depends_on: condition: service_healthy`**（`db` が `Healthy` になってから `api` が `Started`・`ps` に `(healthy)`）→ **アプリ側のリトライ**（`socket.create_connection` を繰り返す `wait_for_db.py`・`docker compose run --rm --build`・3つは重ねて使う・第6章でアプリに組み込む）） |
| 6 | 実践：React + FastAPI + MySQL を1コマンドで起動 | 第5章の範囲に加えて：**3サービスの構成**（`web`（React・`build`）／`api`（FastAPI・`build`）／`db`（MySQL・`image`）・MySQL は**インストールせず公式イメージ**で用意する）、**通信経路**（**ブラウザは Docker のネットワークの外にいる**・React の `fetch` の住所は **`localhost:8000`**（`ports` 経由）／`api` → `db` は**サービス名 `db:3306`**・`http://api:8000` と書くと `ERR_NAME_NOT_RESOLVED`・`db` に `ports` は書かない）、**ディレクトリ構成**（`fullstack-lesson/` に `api/`（`fastapi-lesson` のコピー）と `web/`（`task-app` のコピー）・**`.venv` と `node_modules` と `app.db` は必ず消す**（OS 向けに作られたもの。`@rollup/rollup-linux-x64-gnu` エラー））、**MySQL 公式イメージ**（`mysql:8.4`・`MYSQL_ROOT_PASSWORD` が無いと**起動せずに落ちる**・`MYSQL_DATABASE` / `MYSQL_USER` / `MYSQL_PASSWORD`・**アプリには `root` を使わせない**・データは `/var/lib/mysql` に名前付きボリューム・**初期化のログ（`Creating database` / `Creating user`）は最初の1回だけ** → `.env` のパスワードを変えても効かず `down -v` が必要）、**`mysql` コマンド**（`docker compose exec db mysql -u appuser -p appdb`・`-p` は値を書かず聞かれてから入力・`SHOW TABLES;` / `SELECT ... FROM tasks;`・`Empty set` の意味・SQL は5冊目）、**MySQL への接続**（`PyMySQL==1.1.1` と **`cryptography==44.0.0`**（MySQL 8 の `caching_sha2_password` の計算に必要）・`check_same_thread` は **SQLite 専用** → `settings.database_url.startswith("sqlite")` で分岐・`pool_pre_ping=True`（放置された接続が切られるため）・接続 URL **`mysql+pymysql://ユーザー:パスワード@db:3306/データベース?charset=utf8mb4`**・**乗り換えは URL の1行で、`app/` のコードは1行も変えない**）、**設定の渡し方**（`DATABASE_URL` / `SECRET_KEY`（fastapi-text 第7章以降は既定値が無く**必須**）/ `CORS_ORIGINS`（`'["http://localhost:5173"]'` と**シングルクォートで囲む**。囲まないと YAML のリストになる）・`.env` は `.dockerignore` で除外済みなので**設定は環境変数だけから来る**）、**`command:`**（`Dockerfile` の `CMD` を上書きする・`sh -c "python wait_for_db.py && alembic upgrade head && fastapi run app/main.py --port 8000"`・**`&&` はシェルの機能なので `sh -c` で包む**・`;` ではなく `&&`（失敗したら止めて気づく））、**起動順の3段構え**（`healthcheck` + `condition: service_healthy` → `wait_for_db.py` → `alembic upgrade head` → `fastapi run`）、**フロントの開発用イメージ**（`node:22-slim`・**`npm ci`**（`package-lock.json` どおりに入れる。`npm install` ではない）・`CMD ["npm", "run", "dev", "--", "--host"]`（`--` で `vite` に引数を渡す・`--host` が無いと `ports` を書いても繋がらない））、**ホットリロード**（`- ./web:/app` だけだと `npm ci` した `node_modules` が隠れて `vite: not found` → **`- /app/node_modules`** で守る・`package.json` を変えたら `up -d --build web`・Vite のポーリングは**環境変数だけでは切り替わらない** → `vite.config.js` の `server.watch.usePolling` に `process.env.VITE_USE_POLLING === 'true'` を自分で書く）、**API の URL の外出し**（`import.meta.env.VITE_API_BASE_URL \|\| 'http://127.0.0.1:8000'`・**`VITE_` で始まらない環境変数は `import.meta.env` に入らない**・**`VITE_` の値はブラウザから見えるので秘密を入れない**・`environment` を変えたら `up -d web`）、**`web` に `depends_on` を書かない**（`api` を呼ぶのはブラウザなので起動時に不要）、**完成形**（37行の `compose.yaml` + `.env` + `.env.example` + `.gitignore`・**`docker compose up -d --build` の1行**で3冊ぶんの環境構築が済む）、**通しの確認は下から**（`db` のテーブル → `api` の `/docs` と `python -m app.seed` → MySQL で `SELECT`（**日本語が化けないのは `?charset=utf8mb4` のおかげ**・`done` は `0`/`1`） → React の画面 → `down` → `up -d` でデータが残る）、**ログの読み方**（`logs --tail N` / `logs -f api`・3つの「起動できた印」（`ready for connections` / `Uvicorn running on http://0.0.0.0:8000` / `Local: http://localhost:5173/`））、**切り分け**（**エラーが出た場所と原因の場所は違う**・`logs -f api` に**流れなければ届いていない**（`web` の住所・`ports`）／**`200 OK` が流れるのに画面に出ないのは CORS**／`500` なら `api` の中・Network タブの `ERR_NAME_NOT_RESOLVED` / `ERR_CONNECTION_REFUSED` / `blocked by CORS policy` / `500` / `401` の対応表）、**作り直しは弱い順**（`restart` → `up -d` → `up -d --build` → `down` → **`down -v`（データが消える）**・`build --no-cache`・**`docker system prune -a --volumes` は使わない**・`down -v` のあとテーブルは `command:` で戻るが**行は `seed` を打つまで戻らない**） |
| 7 | イメージの最適化と本番運用 | 第6章の範囲に加えて：**サイズの測り方**（`docker images` の DISK USAGE と CONTENT SIZE（2.5.1 の再確認）・`docker compose images`・`docker system df`・**大きいのは「ベースイメージ」と「ライブラリを入れた `RUN`」の2つだけ**でソースコードはほぼ影響しない・小さくする理由は「転送が速い／起動が速い／**入っているものが少ないほど安全**」）、**`docker history` の読み方**（3.4.1 の再利用。`RUN npm ci` が 152 MB / `RUN pip install` が 130 MB）、**マルチステージビルド**（**`FROM ... AS builder`** と **`COPY --from=builder`**・**最後の `FROM` から作られたものだけが残る**・React は「ビルドすると実行時に道具が要らなくなる」タイプ／Python は「実行時も道具が要る」タイプ・`web/Dockerfile.prod`（`node:22-slim AS builder` → `nginx:1.27-alpine`）と `web/nginx.conf`（**`try_files $uri $uri/ /index.html;` が無いと `/tasks/1` の再読み込みで `404`**）・`docker build -f Dockerfile.prod`（**`-f` はここが初出**）・**560 MB → 74 MB**・`node --version` が `not found` になることで「入っていない」ことを確認)、**ビルド時の値と実行時の値**（**Vite の `import.meta.env` は `npm run build` の時点で JavaScript に文字として焼き付く**・`ARG` + **`--build-arg`**（`build.args`）で渡す・実行時の `-e` では変わらない・**`--build-arg` の値は `docker history` に残るので秘密は渡さない**）、**ベースイメージの選び方**（フル版 / `slim` / `alpine` の実測（`python:3.13` 1.62 GB / `slim` 189 MB / `alpine` 79.2 MB・`nginx:1.27` 282 MB / `-alpine` 74.5 MB）・**Alpine は musl**（glibc ではない）・ホイールの種類（`manylinux` / `musllinux` / 無し）・**この本の `requirements.txt` は alpine でも入る**が、`musllinux` 版が無いライブラリ（例：`pyodbc`）ではソースからのビルドになり **`g++` が無くて失敗する**・コマンドの違い（**`apt-get` → `apk add`**／**`useradd` → `adduser -D -u 1001`**／`bash` は入っていない）・指針＝**ライブラリを入れて動かすものは `slim`、出来たファイルを配るだけなら `alpine`**・**試してから決める**）、**セキュリティの基本**（**`root` で動かさない**：`whoami` で確認 → `RUN useradd --create-home --uid 1001 appuser && chown -R appuser:appuser /code` + **`USER appuser`**・**`USER` は `pip install` より後**・`Permission denied` の確認・**nginx 公式イメージはマスターが root、ワーカーが `nginx` 利用者**／**秘密をイメージに焼き込まない**：`COPY` / `ENV` / `--build-arg` の3つとも残る・**`RUN rm .env` では消えない**（層は積み重なるだけ）・**`docker save` で取り出して層の中身を読める**（`blobs/sha256/`）・秘密は**実行時の環境変数**で渡す／**ベースイメージの更新**：**タグの中身は動く**・`docker pull` の `Image is up to date`・**`docker compose build --pull`**・`<none>` と `docker image prune`（`docker system prune -a` とは別物）・更新後は**通しで動作確認**が必要）、**開発用と本番用の分離**（**`-f` を2枚重ねる**（`docker compose -f compose.yaml -f compose.prod.yaml`）・**マッピングは上書き、リスト（`ports` / `volumes`）は足し算**・**`!override` と `!reset`**・**`up` の前に必ず `docker compose config`**・`compose.prod.yaml`（`dockerfile: Dockerfile.prod` / `build.args` / `ports: !override` / `volumes: !reset []` / **`restart: always`**（壊れたものは直らない））・本番でやらないこと（開発サーバーの公開・`latest`・`db` の `ports`・`.env` の同梱・`root` 実行・管理画面の公開）・**足りないもの**（HTTPS・バックアップ・ログ・監視・秘密情報の管理・データベースの運用））、**デプロイ先の概観**（VPS / コンテナ実行サービス / PaaS / Kubernetes の4分類・共通するのは「イメージを渡す」「設定は環境変数」「秘密はその場所の仕組みで預ける」・**本番の DB はマネージドを選び、`DATABASE_URL` を書き換えるだけ**・**具体的なデプロイ手順は書いていない**（7.5.3）） |
| 8 | 次のステップ | 第7章の範囲に加えて（**新しいコマンドは無し。この本の締めの章**）：**到達度チェックリスト35項目**（第1章〜第7章。判断は「何も見ずにできるか」で、「読めば思い出せる」は ✅ にしない・全項目に**戻る場所の番号**が付いている・優先して埋めたい5つ＝コンテナの起動停止削除／ログを読む／**`down` と `down -v` の違い**／秘密を外に出す／3サービスを1コマンドで起動）、**4冊の成果物の対応**（`docker compose up -d --build` → `compose.yaml` 37行 → `web` / `api` / `db`・**`api/app/` の Python と `web/src/` の React はこの本でほとんど変えていない**（接続先の文字列と `check_same_thread` の分岐、API の住所の環境変数化だけ）＝**この本でやったのはアプリの作り替えではなく「アプリが動く場所」の書き出し**・次に別のアプリを作るときも「`FROM` → `RUN` → `COPY` → `CMD` → 並べる」の順は変わらない）、**引き継ぎ用 `README.md`**（見本つき。**「必要なもの」「動かし方」「確認」「止めるとき」「困ったとき」**・必要なものは **Docker Desktop と空き容量の2つだけ**（Node.js / Python は書かない）・コマンドはコピペできる形・**「どうなれば成功か」＝タスクが3件表示される**を独立した見出しに・**`down -v` の危険を書く**・`.env` の作成は `Copy-Item` / `cp` を両 OS 分・**fastapi-text 10.3.1 の「要るもの8個」が7個消えて `.env` の1個だけ残る**（第1章 1.4 の A / B / C 仕分けで7個が A に移った））、**手順書の正しさは自分の環境では確かめられない**（揃っているので成功してしまう → **別ディレクトリにコピーし、`.env` / `api/.venv` / `web/node_modules` を消して「揃っていない状態」を作る**（6.1.3 と同じ理由）→ **コピー先はディレクトリ名が変わるのでプロジェクト名もボリュームも別**（5.2.4）＝元のデータを壊さずに初回起動をやり直せる・**`.env` は `.gitignore` にあるので Git 経由のコピーでは付いてこない** → `db` が `Database is uninitialized and password option is not specified` で `Exited (1)`（6.2.1 で見たものと同じ）・`api` は `service_healthy` 待ちで `Created`、**`web` だけ `Up`**（`depends_on` を書いていないため。6.5.1）・**`.env.example` は項目名だけ書き値は空にする**（`#` 行がコメント）・`compose.yaml` の `${...}` は5つ（`MYSQL_ROOT_PASSWORD` / `MYSQL_DATABASE` / `MYSQL_USER` / `MYSQL_PASSWORD` / `SECRET_KEY`。**`MYSQL_DATABASE` と `MYSQL_USER` は `db` と `api` の両方から参照される**）・突き合わせは `docker compose config`）、**実力の確認**（AI に構成課題を出させる（**答えを書かせない指定が必須**）・書く前に紙に3つ＝**サービスの一覧／通信経路（`ports` を書くのはブラウザから呼ばれるものだけ）／消えてはいけないデータ**）、**この本で触れなかった道具**（**名前と「どの困りごとの担当か」だけ。インストールはさせない**・**Kubernetes（k8s）**＝落ちたら立て直す／同じものを何個も並べる／複数台に自動配置／止めずに入れ替える。**Compose は「どう起動するか」、Kubernetes は「どうあってほしいか」を書く**・**覚える量がこの本の何倍もあるので、困りごとが無いうちは入れない**（前提知識はこの本で全部済んでいる）・**レジストリと CI/CD**＝本番でビルドしない／**タグがあるから1つ前に戻せる**（2.5.5 の回収）／GitHub Actions などが `git push` を起点にテスト → `docker build` → レジストリ・**fastapi-text 第8章の `pytest` が効いてくるのはここ**・**ログ / 監視 / バックアップ**＝`logs` の保存版／`healthcheck` を外から継続的に／ボリュームの中身の複製。**Docker はバックアップを取ってくれない**・`docker swarm` / Compose の `profiles` / `docker buildx` の複数 CPU 向けビルド / Compose の `secrets` / `docker compose watch`）、**消えるものの表**（`down` は残る／**`down -v` は消える**／**`up -d --build` は消えない**（作り直すのはイメージ）／`restart` は残る・**`--build` とデータの削除を混同しない**）、**7.5.2 の「足りないもの」5つ全部への担当の割り当て**（HTTPS＝**Docker の外**。デプロイ先が用意するか、**リバースプロキシ**（nginx など）を前に立てる／バックアップ＝自分で作る（MySQL 側の取り出し方は5冊目）／ログ・監視＝専用の道具／秘密情報の管理＝実行時の環境変数 → `secrets` やデプロイ先の仕組み。**取り返せるかで優先順位を付ける**＝データと鍵は手遅れになる、HTTPS は公開と同時に必須、ログと監視はあとから足せる）、**`db` が黒い箱のままであること**（**SQL の文は SQLAlchemy が組み立て、テーブルの形は Alembic が `app/models.py` から作った**＝どちらも自分で書いていない・見た SQL は `SHOW TABLES;` と `SELECT ...` の2行だけ（6.2.4 / 6.5.3）・手が止まる5つの場面＝遅くなった原因／複数テーブルの集計／データの取り出し／型の選択／`unique=True` の理由）、**5冊目の位置づけ**（mysql-text の章と困りごとの対応・**mysql-text 第2章の環境構築は `docker compose up -d` で終わる**＝Docker を5冊目の前に置いた理由・**練習用の MySQL はアプリの `db` とは別に立てる**（プロジェクト名が違えばボリュームも別）・5冊目でフェーズ1「作れるようになる」が完了・その先は `docs/roadmap.md`） |
| 解答編 | — | — |

> **注意**：**第0章・第1章の学習者は、Docker をまだインストールしていません。**
> インストールは第2章です。`docker` で始まるコマンドは、`docker compose up` を
> 「第5章・第6章で扱うゴールの形」として名前だけ見せてあるほかは、1つも扱っていません。
> 第0章・第1章の相談に、コマンドを打たせる形で答えないでください。
> 「それは第2章で扱います」と伝えたうえで、待たずに確認できることだけを案内してください。
>
> 第0章の学習者に対しては、次の点にも注意してください。
>
> - **仮想化・WSL2・BIOS/UEFI は、0.2.2 で言葉を紹介しただけ**です。
>   仕組みの説明は第1章・第2章なので、第0章の時点では
>   「Docker はパソコンの深いところを使う」以上の説明を前提にしないでください
> - `node --version` / `python --version` が通らない学習者がいても、
>   **第5章までは読み進められます**（0.1.1）。「先に入れ直さないと読めない」とは言わないでください
> - この本の環境トラブルは、ほぼ **レベル C**（`ai/instructions.md` 参照）です。
>   手順を最後まで、コピペできる形で示してください
> - 相談を受けたら、**OS だけでなく CPU（Apple Silicon か Intel か）を確認**してください（0.2.2）
>
> 第1章の学習者に対しては、さらに次の点に注意してください。
>
> - **第1章はコマンドを1つも打たない章**です。演習もブラウザとメモだけで完結します。
>   「まず `docker run` してみましょう」と案内しないでください（インストールは第2章）
> - **レイヤは 1.3.1 で言葉を出しただけ**です。仕組みは第3章 3.4 なので、
>   キャッシュやレイヤの積み方を前提にした説明をしないでください
> - **ボリューム・ネットワーク・Compose は未習**です。
>   「データが消える」問題への答えを聞かれたら、**第4章 4.1 で扱うと伝えるにとどめて**ください
> - `latest` を避ける理由は 1.3.1 で扱っていますが、
>   **`docker pull` などのコマンドを伴う説明は第2章 2.5 の内容**です
> - 演習 1.2 は Docker Hub の表示を読む問題です。**サイズの数字は更新で変わります。**
>   解答例と違っていても誤りではないと伝えてください
>
> 第2章の学習者に対しては、次の点に注意してください。
>
> - **この章の環境トラブルはレベル C です。** 2.2 の該当項を示したうえで、
>   **コピペできる手順で最後まで**案内してください。「環境によります」で終わらせないこと
> - **相談を受けたら、まず OS と CPU（Apple Silicon か Intel か）を確認**してください。
>   Windows か macOS かで、手順がまったく変わります
> - **切り分けの順番を守らせてください**：`docker --version` → `docker version` の `Server:` →
>   `docker run`。**`--version` が通っただけで「インストールできている」と判断させないこと**（2.1.3）
> - **WSL2（2.2.1）と BIOS の仮想化（2.2.2）は、2.2.2 → 2.2.1 の順でしか解決しません。**
>   逆順で案内しないでください
> - `--platform linux/amd64`（2.2.3）を安易に勧めないでください。**遅くなる最後の手段**です。
>   まず arm64 版のイメージがないかを確認させてください
> - **`docker system prune -a --volumes` を提案しないでください。**
>   このテキストは最後まで `--volumes` を使いません（2.7.1）。
>   容量の相談には、`docker system df` → `docker ps -a` → `container prune` → `rmi` の順で案内してください
> - **プロキシ環境（2.2.5）で、証明書の検証を無効にする回避策を案内しないでください。**
>   管理者への確認か、自宅回線での実行を勧めてください
> - **`Dockerfile`・`docker build`・ボリューム・ネットワーク・Compose は未習**です。
>   「イメージに焼き込めばよい」と答えたくなる場面では、**第3章で扱うと伝えるにとどめて**ください
> - `docker cp` で入れた変更が作り直すと消えるのは**仕様どおり**です（2.6.3）。
>   恒久化の方法を聞かれたら、第3章 3.2.3（`COPY`）と第4章 4.2（バインドマウント）を案内してください
> - **コマンドの出力は Docker のバージョンで変わります**（とくに `docker images` の列。2.5.1）。
>   本文と表示が違っても誤りではないと伝え、**列の見出しで読ませて**ください
>
> 第3章の学習者に対しては、次の点に注意してください。
>
> - **ボリューム・バインドマウント・ネットワーク・Compose はまだ未習**です。
>   3.6.3 で「コンテナを作り直すとデータが消える」ところまでは体験していますが、
>   **解決策は第4章**です。`-v` や `--mount` を使った答えを出さないでください
> - **`docker compose` を使った答えを出さないでください**（第5章）。
>   コマンドが長いという相談には、この段階では**そのまま長い形**で答えてください
> - **マルチステージビルド・`alpine`・非 root 実行は第7章**です。
>   「イメージが大きい」という相談に、`FROM ... AS builder` を出さないでください。
>   3.5（`.dockerignore`）と 3.4.3（順番）の範囲で答えられます
> - ビルドが遅い・キャッシュが効かないという相談は、**まず順番（3.4.3）を疑わせてください。**
>   `--no-cache` を最初の答えにしないこと（3.4.4）
> - **`CMD` に `fastapi dev` を書かせないでください**（3.2.5）。
>   `127.0.0.1` で待つため、`-p` を付けてもブラウザから繋がりません。**`fastapi run` です**
> - **`ENV` や Dockerfile に秘密の値を書かせないでください**（3.2.6）。
>   `docker history` で読めます。起動時に渡す形（`-e` / `--env-file`）を案内してください
> - `.dockerignore` は **`docker build` に指定したディレクトリのもの**しか読まれません（3.5.2）。
>   「書いたのに効かない」という相談では、まず置き場所を確認させてください
> - 3.6 は **fastapi-text 第6章まで進めた `fastapi-lesson`** を前提にしています。
>   手元に無い学習者には、**3.6 と演習 3.4 を飛ばしてよい**と伝えてください
> - 3.6.3 の `no such table: tasks` は**想定どおりの結果**です（`app.db` を焼き込んでいないため）。
>   エラーとして直そうとせず、`docker exec` からの `alembic upgrade head` に案内してください
>
> 第4章の学習者に対しては、次の点に注意してください。
>
> - **`docker compose` を使った答えを出さないでください**（第5章）。
>   コマンドが長いという相談には、**そのまま長い形**で答えてください。
>   4.5 で手作業のネットワーク作成を体験させることが、第5章の下地になります
> - **マウントの不具合は、まずパスを疑わせてください**（4.2.3）。
>   `docker inspect コンテナ名 --format '{{json .Mounts}}'` の **`Source`** を確認させるのが最短です。
>   **存在しないパスを書いてもエラーは出ません**（空のディレクトリが繋がります）
> - **`-v` の左側にスラッシュが混じっていないか**を確認させてください。
>   `-v api-data/:/data` は名前付きボリュームではなく**バインドマウント**として扱われます（4.1.2）
> - 「ビルドしたのに直したコードが反映されない」という相談では、
>   **コードのパスに名前付きボリュームを被せていないか**を確認させてください（4.3.3）。
>   `docker exec コンテナ名 cat /code/...` で実物を見せるのが確実です
> - **`docker volume prune` は匿名ボリュームしか消しません**（4.3.2）。
>   「消えない」という相談には `docker volume rm 名前` を案内してください。
>   **`--all` や `docker system prune -a --volumes` は、このテキストでは使いません**（2.7.1）
> - 「起動しているのにブラウザから繋がらない」は、**まず `docker logs` の
>   `Uvicorn running on http://...` の行**を読ませてください（4.4.3）。
>   `127.0.0.1` なら `--host 0.0.0.0` が答えです。**`fastapi dev` には必ず必要**です
> - コンテナ同士の通信で **`-p` の左側の番号を書かせないでください**（4.5.3）。
>   書くのは**相手のコンテナ名と、相手の中のポート**（`http://api:8000`）です
> - **React（ブラウザ）から呼ぶ URL は `localhost` のまま**です（4.5.4）。
>   ブラウザはコンテナではないので、`http://api:8000` にすると繋がりません
> - Windows で `exec ...: no such file or directory` が出たら、**改行コードを疑ってください**（4.6.1）。
>   `permission denied` は別の原因（実行権限）です。混同しないこと
> - 自動再起動が効かない Windows の相談では、**まず WSL2 側にプロジェクトを置く案内**を、
>   次に `WATCHFILES_FORCE_POLLING=true` を案内してください（4.6.3）。逆順にしないこと
> - **マルチステージビルド・非 root 実行・`alpine` は第7章**、
>   **MySQL などのデータベースコンテナは第6章**です。第4章の相談で先取りしないでください
> - 4.3.3 と演習 4.3 は **fastapi-text 第6章まで進めた `fastapi-lesson`** を前提にしています。
>   手元に無い学習者には、**その2か所を飛ばしてよい**と伝えてください
>
> 第5章の学習者に対しては、次の点に注意してください。
>
> - **YAML のエラーは、まず行番号の出ている行の字下げを見させてください**（5.2.1）。
>   `did not find expected key` はほぼ字下げのずれ、`cannot start any token` はタブ混入です。
>   **`compose.yaml` は一部でなく丸ごと**貼ってもらってから答えてください
> - **`docker compose`（スペース区切り）で案内してください。** 旧 `docker-compose`（ハイフン）を
>   案内された学習者には、スペース区切りに読み替えるよう伝えてください（5.1.2）
> - 名前付きボリュームで `refers to undefined volume` が出たら、**ファイル末尾の `volumes:` 宣言**の
>   書き忘れです（5.2.4）。「サービス内」と「末尾」の**2か所**が要ります
> - 「第4章では残っていたデータが空になった」という相談は、**接頭辞付きの別ボリューム**を
>   見ているだけです（5.2.4）。`docker volume ls` で `プロジェクト名_api-data` と `api-data` の
>   両方があることを確認させてください。**中身は消えていません**
> - **`down -v` を安易に勧めないでください。** ボリュームごと（＝データごと）消えます（5.4.2）。
>   「まっさらにしたい」と明言された場合にだけ案内してください
> - 「直したのに反映されない」は、**まず `up -d --build` の付け忘れ**を疑わせてください（5.4.4）。
>   第4章 4.3.3 の「コードのパスにボリュームを被せた」とは**別の原因**です。混同しないこと
> - **この章では API を MySQL に繋ぎません**（5.3.1）。`db` は「2つ目のサービス」の練習用です。
>   MySQL への接続設定（ドライバ・接続 URL）を聞かれたら、**第6章 / mysql-text**と伝えてください
> - **公開しない `db` に `ports` を書かせないでください**（5.3.1）。コンテナ同士の通信に公開は不要です（4.5）
> - 「`depends_on` を書いたのに接続に失敗する」は**仕様どおり**です（5.6.1）。
>   `depends_on` は「起動した」までで「準備できた」は保証しません。
>   **ヘルスチェック（5.6.2）→ アプリ側のリトライ（5.6.3）**の順で案内してください
> - **秘密の値は `.env` + `${...}`** に寄せさせ、**`.env` は `.gitignore` に入れさせてください**（5.5）。
>   `compose.yaml` にパスワードをベタ書きしたまま共有する形を勧めないこと
> - **マルチステージビルド・非 root・本番向けの秘密情報管理は第7章**、
>   **React・FastAPI・MySQL の統合は第6章**です。第5章の相談で先取りしないでください
> - 5.2.3 以降の中心の例は **`fastapi-lesson`（第3章 3.6 で `Dockerfile` を作成済み）** を前提にします。
>   手元に無い学習者には、**5.2.2（nginx の例）までで基本を身につけ、以降は読み物として進めてよい**と伝えてください

## 5. mysql-text

| 章 | タイトル | 既習範囲（累積） |
|----|---------|----------------|
| 0 | はじめに | — |
| 1 | データベースとは | （SQL を1文も実行しない章）自分でファイルに保存すると起きる3つの問題（**全件走査**で遅くなる／同時書き込みで片方の変更が消える・書き込み途中で壊れる／型・表記ゆれ・項目の欠け・`id` 重複で**整合性**が崩れる。それぞれの担当が第7章インデックス・第4章 4.5 ロック・第4章 4.4 トランザクション・第5章 5.1 データ型 / 5.2 制約 / 5.4 外部キー、という対応表）、**書き込む経路は1つではないのでチェックはデータベース側に1箇所置く**、**DBMS**（「データベース」は保存されたデータとソフトウェアの両方を指すこと）、**RDB**（すべての行が必ず同じ列を持つ・1テーブル1種類・第2章以降は罫線付きの表と `1 row in set` の件数表示）、**表を分けて相手の主キーを持つ**（`tasks.owner` → `owners` + `tasks.owner_id`。**分ける手順4ステップ**＝繰り返しを見つける→くっついてくる情報ごと別表にする→主キーを用意する→もとの表は主キーを持つ。**繰り返しが2種類あれば手順を2回回す**＝`owner_id` と `project_id`。Mermaid の `erDiagram` で1対多）、分けると表記ゆれ・修正漏れが消える代わりに**結合（JOIN、第6章）**が要る、**正規化**は語のみ（詳細は第5章 5.5）、**SQL** と**クエリ**、**宣言型**（Python の `for` との比較。手順ではなく条件を書き、取り出し方は DBMS が決める＝第7章 7.4 実行計画）、SQL の3分類（`CREATE TABLE` / `SELECT` / `INSERT`・`UPDATE`・`DELETE`）と**方言**、用語の対応（テーブル・行・列・値。レコード／カラムの読み替え、Python の辞書のリスト・JS のオブジェクトの配列・fastapi-text 6.3 の `class Task(Base)` との対応表）、**データ型**（`INT` / `VARCHAR` / `DATE` / `BOOLEAN` は名前だけ。選び方は第5章 5.1）、**`NULL`**（空文字とも `0` とも別物。比べ方は第3章 3.3.4）、**表計算ソフトとの違い7点**（とくに**行に順番が無い → 並びが必要なら必ず `ORDER BY`**、第3章 3.4）、**主キー**（3条件＝重複しない（**一意**）・空にできない・変わらない。無いと `UPDATE` が複数行に当たる＝第4章 4.2.3 の伏線。fastapi-text 6.3 の `primary_key=True` を回収）、**主キーの選び方**（3条件を順に当てる判定フロー図。**自然キー**と**代理キー**は名前と方針（迷ったら代理キー）まで。詳細は第5章 5.3.3、`AUTO_INCREMENT` は 5.3.2）、**複合主キー**は名前と例のみ（第6章 6.7.1）、**サーバー型とファイル型**（MySQL / PostgreSQL / SQLite の比較表。fastapi-text 第6章の `app.db` が SQLite だった回収。`dir` / `ls -l` で確認）、**NoSQL**（4種類の表と「整合性の見張りをデータベースがやるかアプリがやるか」という軸）、MySQL を選ぶ理由と **MySQL 8.4**（docker-text 第6章と同じ）、MariaDB は名前のみ |
| 2 | 環境構築（Docker で MySQL） | 接続、クライアント |
| 3 | SELECT | `WHERE`、`ORDER BY`、`LIMIT`、関数 |
| 4 | データの追加・更新・削除 | `INSERT`/`UPDATE`/`DELETE`、トランザクション |
| 5 | テーブル設計 | データ型、主キー、外部キー、制約、正規化の入口 |
| 6 | 結合と集計 | `JOIN`、`GROUP BY`、サブクエリ |
| 7 | インデックスと実行計画 | `EXPLAIN`、遅いクエリの直し方 |
| 8 | アプリからの利用 | FastAPI からの接続、N+1 問題 |
| 9 | 解答編 | — |

> **注意：第1章の学習者は、MySQL をまだ起動していません。**
> 起動と接続は第2章です。**第1章の相談に、SQL を打たせる形で答えないでください。**
> 第1章の本文に出てくる `SELECT` / `UPDATE` は、すべて**読むためだけ**に置いたものです。
>
> 第1章の学習者に対しては、次の点にも注意してください。
>
> - **演習3問はすべて、紙かテキストエディタ（表計算ソフト）だけで解く設計**です。
>   「実際にテーブルを作って試してみましょう」と案内しないでください
> - 学習者が知っているのは**テーブル・行・列・値・データ型・`NULL`・主キー・外部キーの存在**までです。
>   **`CREATE TABLE` の書き方（第2章 2.4.3）、`SELECT` の文法（第3章）、
>   `JOIN` の書き方（第6章）は、まだ1つも扱っていません**
> - **外部キーは「別のテーブルの主キーを指す列」という考え方だけ**を 1.2.2 で扱いました。
>   `FOREIGN KEY` の定義のしかたと `ON DELETE` は第5章 5.4 です。先取りしないでください
> - **自然キー／代理キーは名前と「迷ったら代理キー」という方針まで**（1.4.2）。
>   詳しい選択基準は第5章 5.3.3、`AUTO_INCREMENT` は 5.3.2 です
> - **正規化は語を出しただけ**（1.2.2）。第1〜第3正規形の説明は第5章 5.5 です
> - 「行に順番が無い」（1.3.2）は第1章でいちばん定着しにくい点です。
>   相談を受けたら、**並びが必要なら必ず `ORDER BY` を書く**（第3章 3.4）に着地させてください
> - 演習 1.2 / 1.3 は**設計の問題で、正解が1つに決まりません。**
>   学習者の分け方が解答例と違っても、狙いを満たしていれば正解として扱ってください

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
