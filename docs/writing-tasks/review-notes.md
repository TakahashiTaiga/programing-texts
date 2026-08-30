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

---

## 用語のブレ

（`-FIN` タスクで見つかったものを記録します）
