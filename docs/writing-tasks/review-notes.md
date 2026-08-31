# 人間向け申し送りメモ

クラウドルーティンで執筆した内容のうち、**人間が確認・対応すべきこと**を記録します。
`-FIN` タスクや各章の執筆時に、気づいたことをここに追記していきます。

---

## 使い方

- ルーティン（AI）は、**自信がない記述・検証が必要な手順**をここに追記する
- 人間は、対応が終わったらチェックを入れる
- 対応不要と判断したものは、理由を添えて `~~取り消し線~~` にする

---

## 要検証（実機で動かして確認が必要）

- [ ] `react-text` 1.5 Node.js のインストール手順（Windows / macOS 両方）
- [ ] `react-text` 6.2 Vite でのプロジェクト作成コマンドとバージョン
- [ ] `python-text` 1.2 Python インストール（Windows / macOS 両方）とバージョン表記（執筆時点 3.13 系）
- [ ] `python-text` 1.2.3 「Add python.exe to PATH」を入れ忘れたときの復旧手順（Windows 実機。インストーラーの Modify → Advanced Options →「Add Python to environment variables」／`py --version` での確認）
- [ ] `python-text` 1.5.3 / 1.5.5 PowerShell の実行ポリシー（`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`）と venv の有効化（`Activate.ps1`）
- [ ] `python-text` 1.6.2 / 演習 1.3 `pip install cowsay==6.1` と `import cowsay; cowsay.cow(...)` の実行結果（cowsay のバージョン・API が変わっていないか要確認。牛のアスキーアートの見た目は代表例で記載）
- [ ] `python-text` 1.7 VS Code の Python 拡張・インタプリタ選択・▷ 実行・デバッガのステップ実行（UI 文言は変わりやすい）
- [ ] `python-text` 2.6.4 VS Code の `settings.json` 設定（`editor.insertSpaces` / `tabSize` / `detectIndentation` / `renderWhitespace`）と、コマンドパレットの日本語メニュー名「基本設定: ユーザー設定を開く (JSON)」「インデントをスペースに変換」（UI 文言は変わりやすい）
- [ ] `python-text` 2.7.3 VS Code 拡張機能 **Black Formatter**（発行元 Microsoft、識別子 `ms-python.black-formatter`）のインストールと、保存時の自動整形（`editor.formatOnSave`）が実際に効くか
- [ ] `python-text` 9.3.2 / 9.4.1 mypy と ruff の**バージョン表記**（本文は 2026年8月時点の mypy 1.19 / ruff 0.15 で確認した出力を載せています）。公開前に `mypy --version` / `ruff --version` で確認し、必要なら本文の数字と `pip install` の出力例を更新してください
- [ ] `python-text` 9.4.1 ruff の報告の**表示形式**（本文は `F401 [*] ...` の下に `--> ファイル:行:桁` と枠が続く新しい形式で記載）。古い ruff では1行形式になります
- [ ] `python-text` 9.4.3 VS Code 拡張機能 **Ruff**（発行元 Astral Software、識別子 `charliermarsh.ruff`）のインストールと、保存時の `source.fixAll.ruff` / `source.organizeImports.ruff` が実際に効くか。あわせて 2.7.3 で入れた Black Formatter を無効化する手順の UI 文言
- [ ] `python-text` 9.3.3 Pylance の `"python.analysis.typeCheckingMode": "basic"` と、赤い波線に表示される**日本語のメッセージ文言**（VS Code の表示言語設定で変わります）
- [ ] `python-text` 9.5.2 uv のインストールコマンド（Windows の `irm ... | iex` / macOS の `curl ... | sh`）。**この2つは Windows / macOS 実機で未確認**です。`uv venv` / `uv pip install` の出力例は Linux で確認したものを Windows 向けに書き換えてあります
- [ ] `fastapi-text` 2.2〜2.4 インストールと開発サーバー起動コマンド
- [ ] `docker-text` 2.1〜2.2 Docker Desktop のインストールとトラブル対処
- [ ] `mysql-text` 2.1 Docker での MySQL 起動と接続

> **とくに各本の第1〜2章（環境構築）は、必ず自分で通しで実行してください。**
> AI が書いた手順は、コマンド名やオプションが実在しないことがあります。

---

## スクリーンショットが必要な箇所

- [ ] `react-text` 1.3.2 VS Code のインストール画面
- [ ] `react-text` 1.3.3 拡張機能の検索画面
- [ ] `react-text` 1.6.4 開発者ツールの画面
- [ ] `react-text` 3.3.1 ボックスモデルの図（開発者ツール。もしくは下記「図解の変換待ち」で SVG 化してもよい）
- [ ] `docker-text` 2.1 Docker Desktop の画面

画像は `<book>/images/` に置き、`docs/style-guide.md` の命名規則に従ってください。

---

## 図解の変換待ち（SVG → PNG）

ルーティンが SVG ソースは作成したが、実行環境で PNG に変換できなかった図。
`docs/writing-guidelines.md` 6.2 の手順で変換し、本文中の
`<!-- TODO(review-notes): ... -->` プレースホルダを実際の画像参照に差し替えてください。

- [ ] （ここにルーティンが `SVGパス` / `埋め込み予定のPNGパス` / `本文中の箇所` を追記します）

---

## 内容に自信がない・要検討

- `python-text` 1.6.2 の `cowsay`：本文は `cowsay==6.1` の `cowsay.cow("...")` を前提にしています。
  cowsay は過去に API が変わったことがあるため、実機で `pip install cowsay==6.1` → `import cowsay` →
  `cowsay.cow("test")` が動くこと、および表示される牛のアスキーアートを確認してください。
  もし API が違っていた場合は、本文（1.6.2）・解答編（演習 1.3）・別解の `python -m cowsay -t` を合わせて修正が必要です。
- `python-text` 1.4.4 のトレースバック例：Python 3.11 以降は `NameError` に
  `Did you mean: 'print'?` の候補行が付くことがあります（本文では読みやすさのため省略）。
  実機の出力と1文字単位で一致させる必要はありませんが、気になる場合は候補行を補ってください。
- `python-text` 第2章のエラーメッセージ・実行結果は、**Python 3.11 で実際に実行して確認**しました。
  本文が想定する 3.13 系とは、トレースバックの `^` の位置など細部が異なる可能性があります。
  3.13 の実機で通しで確認していただけると確実です。とくに 2.6.3 の
  `IndentationError` 3種と `TabError` の文言・キャレット位置。
- `python-text` **1.2.3 へのリンクが切れています**（第2章の作業中に発見。第2章の変更ではないため未修正）。
  `01-environment.md` 内の `#123-add-python-to-path-を必ずチェックする` は、
  見出し `### 1.2.3 「Add Python to PATH」を必ずチェックする` の実際のアンカー
  `#123-add-python-to-pathを必ずチェックする`（`path` の直後にハイフンが入らない）と一致していません。
  同ファイル内の2か所と目次から参照されています。`P-FIN` での一括修正か、単独の修正 PR をお願いします。

- `python-text` 第3章のコード・実行結果・エラーメッセージは、**Python 3.11 で実際に実行して確認**しました
  （`SyntaxError: expected ':'` / `IndentationError: expected an indented block after 'if' statement on line 3` /
  `ZeroDivisionError: division by zero` / `TypeError: '>=' not supported between instances of 'str' and 'int'` /
  `KeyboardInterrupt`）。3.13 系では文言が変わる可能性があります。
- `python-text` 3.2.5 の**無限ループの止め方は、実機での確認をお願いします**。
  とくに次の2点は VS Code のバージョンや設定で見え方が変わります。
  - ターミナルにフォーカスがない状態で `Ctrl` + `C` を押すと「コピー」になる、という記述
  - 「ターミナル右上のゴミ箱アイコン」でターミナルごと閉じられる、という記述（アイコンの位置・有無）
- `python-text` 3.2.5 は**学習者が意図的に無限ループを実行する項**です。
  止め方の説明が実機と食い違っていると、そこで完全に詰まります。
  第1〜2章と同じ優先度で確認してください。

- `python-text` 第4章のコード・実行結果・エラーメッセージは、**Python 3.11 で実際に実行して確認**しました
  （`IndexError: list index out of range` / `ValueError: list.remove(x): x not in list` /
  `IndexError: pop from empty list` / `TypeError: '<' not supported between instances of 'str' and 'int'` /
  `TypeError: object of type 'NoneType' has no len()` / `TypeError: 'tuple' object does not support item assignment` /
  `ValueError: not enough values to unpack (expected 3, got 2)` / `KeyError: 'age'` /
  `TypeError: 'set' object is not subscriptable` / `SyntaxError: expected 'else' after 'if' expression`）。
  本文が想定する 3.13 系では、トレースバックの `^` の位置や文言が変わる可能性があります。
- `python-text` 4.3.5 の「よくある間違い」で、**f-string の中で外側と同じ引用符を使うと
  `SyntaxError: f-string: unmatched '['` になる**と書いていますが、
  **これは Python 3.11 以前の挙動**です。3.12 以降は同じ引用符でも書けます
  （本文にもその旨を明記済み）。3.13 の実機では**エラーにならない**ため、
  読者が試したときに文面と食い違わないか確認をお願いします。
- `python-text` 4.4.2 の縦棒 `|` の入力方法（Windows は `Shift` + `¥` の左隣、macOS は `Shift` + `¥`）は、
  **キーボードの配列によって位置が変わります。** 日本語配列以外を使う読者向けの注記が要るか、判断をお願いします。
- `python-text` 4.4.1 / 4.4.3 で「集合の表示順は決まっていない」と書き、
  表示が必要な箇所はすべて `sorted()` を通す形にしてあります。
  **本文の実行結果に、集合をそのまま `print` した例が残っていないか**（整数の `{1, 2, 3}` を除く）、
  レビュー時に確認していただけると確実です。

- `python-text` 第5章のコード・実行結果・エラーメッセージも、**Python 3.11 で実際に実行して確認**しました
  （`NameError: name 'greet' is not defined` /
  `TypeError: introduce() missing 1 required positional argument: 'age'` /
  `SyntaxError: positional argument follows keyword argument` /
  `UnboundLocalError: cannot access local variable 'count' where it is not associated with a value` /
  `TypeError: len() takes exactly one argument (0 given)` / `help()` の出力）。
  **1か所だけ、3.11 と本文の文言が違います。**
  5.2.3 の「初期値のある引数を先に書いた」ときの `SyntaxError` を、本文では 3.12 以降の
  `parameter without a default follows parameter with a default` で書いています
  （3.11 では `non-default argument follows default argument`）。
  本文にはバージョンで文言が変わる旨の注記を入れてありますが、
  **3.13 の実機でこの文言になるか確認をお願いします。**
- `python-text` 5.5.1 の実行結果に `<function double at 0x0000023F1C2A4C20>` と書いています。
  **`0x...` の値は実行のたびに変わる**ため、本文にもその旨を明記してあります。
  読者が「同じ数字が出ない」と混乱しないか、表現の確認をお願いします。

### 第6章（P-06）について

- 第6章のコードは、すべて **Python 3.11 で実行して出力を確認済み**です
  （本文の例・演習4問の解答すべて）。エラーメッセージも実際の出力を貼っています
  （`ModuleNotFoundError: No module named 'price_utils'` /
  `NameError: name 'with_tax' is not defined` /
  `ImportError: attempted relative import with no known parent package` /
  `TypeError: first argument must be callable or None` /
  `TypeError: sequence item 0: expected str instance, int found`）。
  **3.13 の実機での確認をお願いします。**
- 6.3.3 `random` の実行結果は、**この章で唯一、実行するたびに変わります。**
  本文にその旨を明記し、「一例」と書いてありますが、
  読者が「違う値が出た＝失敗」と受け取らないか、表現の確認をお願いします。
- 6.3.3 の `random.seed(42)` の例は、**あえて具体的な出力値を書いていません。**
  `random` のアルゴリズムは Python のバージョン間で変わる可能性があると
  公式ドキュメントに記載があるためです。「同じ数字が出る」ことだけを説明しています。
- 6.3.2 の `date.today()` を使った例（`dt_today.py`）の実行結果は、
  **執筆日（2026-08-27）に実行したもの**です。本文にも実行日で変わる旨を書いてあります。
- 6.1.3 で `other` ディレクトリを作らせて `ModuleNotFoundError` をわざと出させています。
  **最後に「削除してかまいません」と書いてありますが、
  読者の手元にゴミが残らないか**、通しで実行して確認をお願いします。
- 6.5.2 で「Python 3.3 以降は `__init__.py` がなくてもディレクトリを読み込める」と書いています
  （実際に確認済み）。「それでも置く」という書き方にしてありますが、
  初学者に対してこの説明が必要かどうか、判断をお願いします。

### 第7章（P-07）について

- 第7章のコードは、すべて **Python 3.11 で実行して出力を確認済み**です
  （本文の例・演習4問の解答すべて）。エラーメッセージも実際の出力を貼っています
  （`FileNotFoundError` / `ValueError: I/O operation on closed file.` /
  `UnicodeDecodeError: 'cp932' codec can't decode byte 0x94 ...` /
  `FileExistsError` / `TypeError: unsupported operand type(s) for /: 'str' and 'str'` /
  `TypeError: write() argument must be str, not list` /
  `AttributeError: 'str' object has no attribute 'write_text'` /
  `json.decoder.JSONDecodeError: Expecting property name enclosed in double quotes: ...`）。
  **3.13 の実機での確認をお願いします。**
- **Windows でしか再現しない記述が3か所あります。実機での確認をお願いします。**
  1. 7.1.4 の文字化け例（`繧翫ｓ縺`）は、UTF-8 のバイト列を cp932 として解釈した結果を
     Linux 上で再現したものです。Windows の実環境で `encoding` を省略したときに
     `UnicodeDecodeError` になるか文字化けになるかは環境によります。
     本文は「どちらも起こりうる」と書いてあります
  2. 7.3.2 / 7.3.4 の「Windows の場合」の実行結果（`data\sales.csv` など）は、
     **Linux では確認できないため、仕様に基づいて記載**しています
  3. 7.4.1 の「`newline=""` を忘れると Windows で空行が入る」は、
     `csv` が `\r\n` を書くこと（確認済み）とテキストモードの改行変換から導いた記述です。
     **実際に Excel で開いて空行が入ることの確認をお願いします**
- 7.1.4 に「将来のバージョンで UTF-8 が既定になることが検討されている」と書いています。
  執筆時点（Python 3.13）の状況に基づく記述なので、**時点の妥当性の確認をお願いします。**
- 7.6.2 で `class ConfigError(Exception):` を先取りしています。
  「例外の種類を1つ増やすための決まった書き方」と明示し、
  `class` と継承の詳細は第8章に送っていますが、
  **第7章の学習者にこの先取りが重すぎないか、判断をお願いします。**
- 7.3.3 で `Path.unlink()` を「このテキストでは使わない」としています。
  読者が演習で作ったファイル（`out/` `notes/` `report.txt` など）は手元に残ります。
  **章末に「削除してよいファイル」の案内を足すべきか、判断をお願いします。**
- 7.2.1 の `append_log.py` は、**3回実行させる**手順になっています。
  `log.txt` が残るので、上と同じく後始末の扱いをご確認ください。

---

## 図解（作成済み・確認のみ）

- `python-text` 4.4.2 集合の和・積・差のベン図
  - SVG 原本：`python-text/images/svg-src/04-set-operations.svg`
  - 本文が参照する PNG：`python-text/images/04-set-operations.png`（幅 880px、cairosvg で変換済み）
  - モノクロ前提で作図しています（実線の円が A、破線の円が B、灰色が結果の範囲）。
    日本語は IPAGothic で描画されています。**フォントの見え方だけ確認をお願いします。**

---

## 図解の追加候補（`R-FIN` で検出）

react-text の第0章〜第3章には図が1つもありません（第4章以降は本文だけで Mermaid を 49 箇所使用）。
`docs/writing-guidelines.md` 6章が「積極的に図解する」と挙げている題材が、次の3箇所で
文章・テキスト装飾のままになっています。**本文の書き換えを伴うため `R-FIN` では手を入れず、
判断を人間に委ねます。**

- [ ] `react-text` 1.2.2 リクエストとレスポンス
      → いまは `text` ブロックの「1.〜6.」の縦並び。ブラウザとサーバーの往復なので
      `sequenceDiagram` で表せます（9.1.1 に同種の図の実例あり）。Mermaid なので画像は不要
- [ ] `react-text` 3.3.2 `padding` / `border` / `margin`（ボックスモデル）
      → Mermaid では表現できないため SVG→PNG が必要。
      上の「スクリーンショットが必要な箇所」の 3.3.1 と合わせて対応するとよい
- [ ] `react-text` 3.5.3 / 3.5.4 Flexbox の主軸・交差軸
      → 同じく SVG→PNG が必要。`flex-direction` を変えると軸が入れ替わることを示す図

---

## 用語のブレ

### `R-FIN`（react-text 通し確認）で見つかったもの

**修正済み**（このタスクの PR に含めています）

| 箇所 | 内容 | 対応 |
|------|------|------|
| `docs/style-guide.md` 2.1 | 表は「コンピューター」を指示していたが、5冊分の本文と `docs/glossary.md` の全 31 箇所が「コンピュータ」だった | 実態と glossary に合わせ、**コンピュータ**（長音なし）を正式表記としてスタイルガイド側を修正。ブラウザ・フォルダ・ディレクトリ・メモリと同じ扱い |
| `react-text` 3.3.4「実践的なおすすめ」／3.5.2 | 「いちばん簡単です」「使い方は簡単です」（`writing-guidelines` 8章で禁止） | 「迷う場面が減ります」「これだけです」に変更 |
| `react-text` 8.2.6 | 「返り値」（style-guide 2.3 は「戻り値」で統一） | 「戻り値」に変更 |
| `react-text` 1.5.1 | 地の文に一人称「私が作るのは」（style-guide 1章） | 「これから作るのは」に変更 |
| `react-text` 6.4「違い4」 | 「キャメルケース」を 4.2.3 と別のたとえで再定義していた（`writing-guidelines` 3.2「1概念1たとえ」） | 4.2.3 への参照だけを残し、重複した定義を削除 |
| `react-text` 第1〜3章の理解度チェック | 解答編へのリンクにアンカーが無く、先頭に飛んでいた（style-guide 5章） | `#第1章`〜`#第3章` を付与。全11章でアンカー付きに統一 |
| `react-text` 解答編 その2 | 第9・10章の区切りが `### 演習問題`、第11章は区切り自体が無かった | 他章と同じ `### 演習` に統一 |
| `docs/glossary.md` | react-text で定義したのに未登録だった用語が 24 語（SPA、CORS、`key`、依存配列、カスタムフック、制御コンポーネント、Context、エラーバウンダリ、`localStorage`、TypeScript など） | Web の表に 8 語、JavaScript / React の表に 16 語を追記。定義文は本文の初出時の表現をそのまま採用 |

**修正しなかったもの（意図的）**

- `react-text` 1.3.2 / 1.4 / 6.3「フォルダーを開く」「エディター」
  → VS Code とインストーラーの**画面に出る文言そのまま**です。表記ルールより画面の実物を優先しました。
  同じ判断ができるよう、`docs/style-guide.md` 2.1 に補足を追記しています
- 「コンピュータ」の 31 箇所
  → 上のとおりスタイルガイド側を実態に合わせたため、本文の書き換えは不要と判断しました。
  **逆（本文を「コンピューター」に直す）を選ぶ場合は、5冊すべてに影響します**のでご判断ください
