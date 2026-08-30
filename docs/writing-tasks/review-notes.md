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

---

## 図解（作成済み・確認のみ）

- `python-text` 4.4.2 集合の和・積・差のベン図
  - SVG 原本：`python-text/images/svg-src/04-set-operations.svg`
  - 本文が参照する PNG：`python-text/images/04-set-operations.png`（幅 880px、cairosvg で変換済み）
  - モノクロ前提で作図しています（実線の円が A、破線の円が B、灰色が結果の範囲）。
    日本語は IPAGothic で描画されています。**フォントの見え方だけ確認をお願いします。**

---

## 用語のブレ

（`-FIN` タスクで見つかったものを記録します）
