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
- [ ] `fastapi-text` 1.5.3 `curl` の実機確認（**Windows で `curl.exe` と打つ必要がある**点、`-i` の1行目が `HTTP/2 200` / `HTTP/2 404` の形で出るか、`-d "@new_post.json"` で `201` が返るか）。PowerShell 5.1 と PowerShell 7 系の両方で確認してください
- [ ] `fastapi-text` 1.5 で使っている外部サービス **JSONPlaceholder**（`https://jsonplaceholder.typicode.com/`）が公開され続けているか。`/posts` が 100 件・`/users` が 10 件（演習 1.2 の解答が「10 人 × 10 件」前提）であることも、公開前に確認してください。停止していた場合は、代替 API に差し替えるか 1.5 全体を第2章の自作 API に置き換える必要があります
- [ ] `fastapi-text` 1.5.2 開発者ツールの **Network タブの日本語 UI 文言**（Chrome / Edge / Safari で変わります）。本文は英語表記を主にして日本語を併記しています
- [ ] `fastapi-text` 2.2〜2.4 インストールと開発サーバー起動コマンド。**F-02 で書いた内容は実機未確認です。**公開前に次を必ず通しで実行してください
  - [ ] 2.2.2 `pip install "fastapi[standard]==0.115.6"` が現在も成功するか（**本文はこのバージョンで固定**。新しい版に更新する場合は、2.2.3・2.2.2 の出力例・まとめ・演習 2.2 の完成条件・解答編の `fastapi==0.115.6` をすべて直す必要があります）。あわせて `Successfully installed ...` に並ぶ**依存パッケージ名とバージョン**を実際の出力に合わせてください
  - [ ] 2.2.2 macOS の zsh で `pip install fastapi[standard]`（クォート無し）が本当に `zsh: no matches found:` になるか
  - [ ] 2.2.3 `fastapi --version` の出力形式（本文は `FastAPI CLI version: 0.0.7`）
  - [ ] 2.4.1 **`fastapi dev main.py` の起動時の表示**。本文は枠線付きの表示を要点だけ抜粋した形で載せています。実機の出力と大きく違うようなら差し替えてください
  - [ ] 2.4.4 ポート衝突時のメッセージ（macOS `[Errno 48] Address already in use` / Windows `[Errno 10048] ...`）と、`netstat -ano | Select-String ":8000"` / `lsof -i :8000` の出力形式
  - [ ] 2.6.3 **変数名を `app` 以外にしたときのエラー文言**（本文は `There is no FastAPI app or you haven't used a supported type.`）と、ファイルが無いときの `Error: Path does not exist main.py`。**この2つは版によって文言が変わりやすい箇所です**（解答編 演習 2.3 にも同じ文言を書いています）
  - [ ] 2.6.2 `Get-Command python | Select-Object Source` の出力形式（Windows 実機）
  - [ ] 2.5.1〜2.5.3 `/docs` `/redoc` `/openapi.json` の表示と、`Try it out` → `Execute` の UI 文言
- [ ] `fastapi-text` 第3章のコードと出力（**執筆時に `fastapi==0.115.6` / `pydantic==2.10.4` / Python 3.11 で全エンドポイントを実行し、本文・解答編に載せた JSON はその実行結果です**）。公開前に、学習者と同じ Windows / macOS の実機で次の3点だけ確認してください。①`curl.exe -d "@new_task.json"`（3.3.2）と `-b "theme=dark"`（3.5.2）が PowerShell で通るか、②`422` の `msg`（英語）が Pydantic のバージョン更新で変わっていないか、③3.4.1 の「よくある間違い」の `SyntaxError` の文言（Python 3.12 以降と 3.11 以前で異なる旨は本文に注記済み）
- [ ] `fastapi-text` 3.5.2 **`/docs` の `Try it out` からクッキーを送れない**という記述（Swagger UI の仕様変更で変わる可能性があります。変わっていた場合は `curl` 限定という書き方を緩めてください）
- [ ] `fastapi-text` 第4章のコードと出力（**執筆時に `fastapi==0.115.6` / `pydantic==2.13.5` / `pydantic-settings==2.7.0` / Python 3.11 で、本文と解答編の全エンドポイント・全 `422` を実際に実行し、載せた JSON はその実行結果です**）。公開前に、学習者と同じ Windows / macOS の実機で次の4点だけ確認してください
  - ① **4.1.2 の `pip list` に出る `pydantic` のバージョン**。本文は「`2.` で始まっていれば数字が違ってよい」と注記済みですが、`pydantic` 3 系が出ていた場合は `pip install` の指定ごと見直しが必要です
  - ② **4.6.1 の `pip install "pydantic-settings==2.7.0"` の出力**（本文は `python-dotenv` が一緒に入る前提で書いています）
  - ③ **4.4.3 の `DELETE` が `204` を返し、ボディが空になること**（`return None` で空になることは確認済みですが、`curl -i` の表示は環境差があります）
  - ④ **`/docs` に表示される送信例・レスポンス例の値**（4.2.1 / 4.4.1）。Swagger UI が自動生成する見本なので、UI の更新で値が変わります。本文には「値そのものは手元と違って構わない」と注記済みです
- [ ] `fastapi-text` 4.6.2 の **`.env` に日本語（全角括弧を含む）をクォート無しで書く**例。Linux では確認済みですが、Windows のメモ帳などで **UTF-8 以外の文字コードで保存された場合**の挙動は未確認です（本文は VS Code で作る前提。必要なら「文字コードは UTF-8」の注記を足してください）
- [ ] `fastapi-text` 第8章のコードと出力（**執筆時に `pytest==9.1.1` / `httpx==0.28.1` / `fastapi==0.115.6` / `sqlalchemy==2.0.36` / `bcrypt==4.2.1` / `pyjwt==2.10.1` / Python 3.11 で、本文16件・演習14件のテストを実際に実行し、載せた出力はその結果です**）。公開前に、学習者と同じ Windows / macOS の実機で次の4点だけ確認してください
  - ① **`pip install pytest==9.1.1` が成功するか**（本文はこのバージョンで固定。更新する場合は 8.2.1 の出力例・まとめ・解答編の実行結果もあわせて直す必要があります）。python-text 11.2.1 も pytest 9.1 前提なので、両方を揃えてください
  - ② **実行結果の1〜2行目**（`platform win32` / `platform darwin` と `rootdir`）。本文には「環境によって変わる」と注記済みですが、**Windows で日本語のテスト名が文字化けしないか**は実機で確認してください
  - ③ **`pytest "tests/test_security.py::test_正しいパスワードなら検証に通る"` が PowerShell で通るか**（8.2.3。日本語を含む引数の引用符の扱い）
  - ④ **`warnings summary` に出る `DeprecationWarning`**（starlette / anyio の版によって出たり出なかったりします）。本文には「合否に影響しない」と注記済みですが、文言が変わっていたら 8.2.3 の補足を差し替えてください
- [ ] `fastapi-text` 8.4.2 の **`Base.metadata.drop_all` を使った後片付け**が、Windows でも問題なく動くか（`test.db` のファイルは残したまま、テーブルだけ作り直す形にしてあります。ファイルを削除する形にしていないのは、Windows でファイルが掴まれたままになるのを避けるためです）
- [ ] `fastapi-text` 10.2.2 の **`fastapi run app/main.py` の起動時の表示**（本文は `production mode` / `Server started at http://0.0.0.0:8000` の形で記載）。Linux で確認したものなので、Windows / macOS の表示と食い違わないか確認してください。**このコマンドを実行すると、同じネットワークの他の端末から届く状態になります**（本文にも注意を書いていますが、社内ネットワークなどで試す場合はご注意ください）
- [ ] `fastapi-text` 演習 10.2 の前提：**第9章で `.env` に足した `CORS_ORIGINS` が、`.env.example` に足されていません。**
      演習はこの抜けを見つけて直させる形にしてありますが、第9章の 9.1.3 に `.env.example` への追記を足すという直し方もあります。
      その場合は演習 10.2 の「足りなかったのは `CORS_ORIGINS` です」（解答編）も合わせて直してください
- [ ] `fastapi-text` `.env.example` の **`SECRET_KEY` の行が2つになる**問題（4.6.3 で `SECRET_KEY=` を書き、7.4.2 でもう一度追記しているため）。
      解答編 演習 10.2 の「よくある間違い」で触れていますが、7.4.2 側を「置き換える」と書き直すほうが親切かもしれません
- [ ] `docker-text` 第2章（D-02）。**本文・解答編に載せた `docker` コマンドの出力は、
      Docker Engine 29.3.1（linux/amd64）で実際に実行した結果です**（`hello-world` / `nginx:1.27` /
      `python:3.13-slim` / `python:3.12-slim`）。ただし**インストールと OS 固有のトラブル対処は実機未確認**です。
      **この章で環境が立ち上がらないと以降の章がすべて読めなくなるため、公開前に必ず通しで実行してください。**
  - [ ] 2.1.1 Windows のインストーラの画面。とくに **「Use WSL 2 instead of Hyper-V (recommended)」**という
        チェックボックスの文言と、完了時の **「Close and restart」**のボタン名（版によって変わります）
  - [ ] 2.1.2 macOS の `.dmg` の画面と、初回起動時の **「Docker Desktop needs privileged access」**の文言。
        あわせて、**Docker アカウントのサインインを求める画面をスキップして進めるか**（本文は「不要」と書いています）
  - [ ] 2.1.3 クジラのアイコンのメニューに出る状態表示（本文は `Docker Desktop is running` /
        `starting` / `stopped` の3つで書いています）
  - [ ] 2.2.1 `wsl --status` / `wsl -l -v` / `wsl --install` / `wsl --update` の**実際の出力**（Windows 実機）。
        本文の `wsl -l -v` の例は `docker-desktop` の1行だけを載せています
  - [ ] 2.2.1 Docker Desktop が出す **「WSL 2 installation is incomplete」**の文言
  - [ ] 2.2.2 タスクマネージャーの **「仮想化:」**の行の日本語表記（Windows の版で変わります）と、
        「Windows の機能の有効化または無効化」の項目名（本文は「仮想マシン プラットフォーム」
        「Linux 用 Windows サブシステム」）
  - [ ] 2.2.3 Apple Silicon での **platform 不一致の警告文**（本文は
        `WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested`）と、
        Docker Desktop の設定 General にある **「Use Rosetta for x86_64/amd64 emulation on Apple Silicon」**の項目名
  - [ ] 2.2.4 **デーモンが止まっているときのメッセージ**。本文には macOS 版
        （`Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`）と
        Windows 版（`open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.`）を
        載せていますが、**この2つだけは Docker Desktop 実機で未確認**です
        （Linux で確認した `failed to connect to the docker API at ...` は別途 2.2.4 に併記しています）
  - [ ] 2.2.5 Docker Desktop の設定 **Resources → Proxies** の項目名と、プロキシ環境でのエラー文言3種
  - [ ] 2.5.1 **`docker images` の列**。Docker 29 では `IMAGE` / `ID` / `DISK USAGE` / `CONTENT SIZE` に変わっており、
        本文はこちらを主にして 28 以前の `REPOSITORY` / `TAG` / `IMAGE ID` / `CREATED` / `SIZE` を補足で併記しています。
        **学習者がインストールする Docker Desktop がどちらの表示になるかを確認し、必要なら主従を入れ替えてください**
  - [ ] 2.5.2 Docker Hub の**取得回数の上限**（本文は数値を書かず「公式サイトを確認」としています）
  - [ ] 2.1.1 の「大企業での業務利用は有料」という記述（ライセンス条件は変わります。公開前に公式の記載を確認してください）
- [ ] `docker-text` 第3章（D-03）。**本文・解答編に載せた `docker build` 系の出力は、実機未確認です。**
      執筆環境で Docker デーモンが動かせなかったため、`[+] Building ...` の表示・`CACHED` の行・
      `docker history` の表示は、**形式に沿って書いた例**です。公開前に、次を通しで実行して差し替えてください
  - [ ] 3.3.1 `docker build -t greeting-api .` の**出力全体**（BuildKit の表示形式・`[n/m]` の番号の付き方・
        `transferring context` の数値）と、**初回ビルドにかかる時間**（本文は「20〜60 秒」と書いています）
  - [ ] 3.3.1 `.` を忘れたときのエラー文言（本文は `ERROR: "docker buildx build" requires exactly 1 argument.`）と、
        `Dockerfile` が見つからないときの文言（本文は `failed to read dockerfile: open Dockerfile: no such file or directory`）
  - [ ] 3.3.1 / 演習 3.1 の `docker images` の**イメージのサイズ**（本文は `greeting-api` を 281MB / 88.4MB として、
        `python:3.13-slim` の 189MB との差を「`pip install` で入った 92 MB」と説明しています）
  - [ ] 3.3.3 `fastapi run main.py --port 8000` の**起動時の表示**（本文は要点だけを抜粋した形。
        fastapi-text 2.4.1 の `fastapi dev` の表示と同じ扱いです）と、`docker ps` の `COMMAND` 列の省略のされ方
  - [ ] 3.4.1 **`docker history` の出力**（列・`<missing>` の並び・`CREATED BY` の省略のされ方・
        各レイヤのサイズ。本文は `RUN pip install` を 92.1MB としています）
  - [ ] 3.2.3 コンテキスト外を `COPY` したときのエラー文言（本文は
        `ERROR: failed to solve: failed to compute cache key: "/secret.txt": not found`）
  - [ ] 3.6.2 **`.dockerignore` の有無によるビルドコンテキストの差**（本文は 412.83MB → 84.21kB、
        送信時間 31 秒 → 0.1 秒。**`.venv` の大きさで変わるので、桁が合っていれば十分**です）
  - [ ] 3.6.3 `no such table: tasks` のときの**ログの出方**（本文は `sqlalchemy.exc.OperationalError` の行と
        `500 Internal Server Error` の行を抜粋）、および **コンテナの中での `alembic upgrade head` /
        `python -m app.seed` が通ること**（`fastapi-lesson` を実際にイメージ化して確認してください）
  - [ ] 解答編 演習 3.3 の、`CMD` だけで書いたときのエラー文言（本文は
        `exec: "世界": executable file not found in $PATH` を含む長いメッセージ）。
        **日本語の引数がそのまま表示されるか**もあわせて確認してください
- [ ] `docker-text` 第4章（D-04）。**本文・解答編のコマンドと出力は、Docker Engine 29.3.1（linux/amd64）で
      実際に実行した結果です**（マウント・ボリューム・ポート衝突・`0.0.0.0` / `127.0.0.1`・ネットワーク・
      CRLF の実演と、演習 4.1〜4.4 の通し実行）。**未確認なのは、Docker Desktop 固有の挙動と 4.6 の Windows 手順**です
  - [ ] 4.2.3 macOS の **Settings → Resources → File sharing** の項目名（`/Users` の外をマウントするときの案内）
  - [ ] 4.2.4 **Windows / macOS の Docker Desktop で、コンテナが作ったファイルの持ち主がどう見えるか**
        （本文は「ほとんど問題にならない」と書いています。Linux で `root` になることは確認済み）
  - [ ] 4.4.2 **`Get-NetTCPConnection -LocalPort 8080 -State Listen`** の実際の出力（Windows 実機）。
        macOS の `lsof -i :8080` は一般的な形で書いています
  - [ ] 4.4.3 `127.0.0.1` で待っているコンテナに繋ごうとしたときの、**ブラウザ側の表示**
        （本文は「接続がリセットされました」「ページが動作していません」と幅を持たせています。
        `curl` での `Recv failure: Connection reset by peer` は確認済み）
  - [ ] 4.5.4 **`host.docker.internal`** が Docker Desktop で解決できること（本文では名前の紹介のみ）
  - [ ] 4.6.1 CRLF のときの **`exec ./start.sh: no such file or directory`**（Linux では確認済み。
        **Windows で実際に CRLF のまま保存してビルドし、同じ文言になるか**を確認してください）
  - [ ] 4.6.2 VS Code の設定 **`files.eol`** の項目名と、`git add --renormalize .` の実行結果
  - [ ] 4.6.3 **Windows のバインドマウントでファイル監視が届かないこと**そのもの
        （本文は「効かないことがある」と書いています。**WSL2 側に置いた場合との差**もあわせて確認してください）。
        `WATCHFILES_FORCE_POLLING=true` で再起動がかかることは Linux で確認済み
- [ ] `docker-text` 第5章（D-05）。**本文・解答編の `docker compose` / MySQL 系の出力は、実機未確認です。**
      Compose の挙動は概ね安定していますが、次は実機で通してください（`fastapi-lesson` の `compose.yaml` は
      第3章 3.6 の `Dockerfile` が前提）。
  - [ ] 5.1.2 **`docker compose version`** の表示（`v2.31.0` は執筆時点の想定。実際のバージョンで置き換える）
  - [ ] 5.2.2 / 5.2.3 / 5.4.3 の **`docker compose up` / `ps` の出力**（コンテナ名 `プロジェクト名-サービス名-連番`、
        `Network ... Created` の行、`PORTS` 列の形）が、手元の Compose v2 と一致するか
  - [ ] 5.2.4 **名前付きボリュームの接頭辞**（`fastapi-lesson_api-data`）と、宣言忘れ時の
        `refers to undefined volume api-data` の実際の文言
  - [ ] 5.3.1 **`mysql:8.4` の起動ログ**（`ready for connections` の行・`Version` と `port: 3306` の表記）。
        初回初期化にかかる時間の実測
  - [ ] 5.3.2 **`socket.gethostbyname('db')`** が返す IP（`172.18.0.2` は例）と、存在しない名前での
        `socket.gaierror: [Errno -2] Name or service not known`
  - [ ] 5.6.1 **`depends_on: - db` だけのとき、起動直後に `ConnectionRefusedError: [Errno 111] Connection refused`**
        になる瞬間があること（タイミング依存。`down -v` 直後に試すこと）
  - [ ] 5.6.2 **`healthcheck`（`mysqladmin ping -h 127.0.0.1`）と `condition: service_healthy`** の動作。
        `db` が `Healthy` になってから `api` が `Started` になること・`docker compose ps` の `(healthy)` 表示。
        **`mysqladmin ping` が認証なしでも「応答あり（exit 0）」を返すか**を、使っている MySQL 8.4 のバージョンで確認
  - [ ] 5.6.3 **`docker compose run --rm --build api python wait_for_db.py`** が、`まだ繋がりません` を経て
        `db:3306 に繋がりました` に至ること
- [ ] `docker-text` 第6章（D-06）。**3サービス（React + FastAPI + MySQL）の通し確認は実機未実施です。**
      この章は前の5章ぶんを一度に使うため、**通しで1回動かしてもらえると効果がいちばん大きい章**です。
      前提は、fastapi-text 第9章まで進めた `fastapi-lesson` と react-text 第10章の `task-app` です。
  - [ ] 6.2.1 **`MYSQL_ROOT_PASSWORD` を渡さずに `mysql:8.4` を起動したときのエラー文**
        （`Database is uninitialized and password option is not specified` と、続く3つの候補の並び）
  - [ ] 6.2.3 **初期化ログの文言**（`Creating database appdb` / `Creating user appuser` /
        `Giving user appuser access to schema appdb`）と、初回初期化にかかる時間の実測
  - [ ] 6.2.4 **`docker compose exec db mysql -u appuser -p appdb` の対話**（`Enter password:` が出るか・
        `mysql>` プロンプトの表示・`SHOW TABLES;` の `Empty set`）。TTY の割り当てがうまくいくか
  - [ ] 6.3.2 **`PyMySQL==1.1.1` / `cryptography==44.0.0` で MySQL 8.4 に実際に接続できるか**（この章の最重要）。
        バージョンは執筆時点の想定です。`cryptography` を入れずに起動したときの
        `RuntimeError: 'cryptography' package is required for ...` の文言も確認してください
  - [ ] 6.3.2 **`?charset=utf8mb4` を付けた接続 URL で、日本語が化けずに保存されるか**（6.5.3 の段階3の `SELECT` 結果）
  - [ ] 6.3.2 `check_same_thread` を渡したままにしたときの
        `TypeError: Invalid argument(s) 'check_same_thread' sent to create_engine()` の実際の文言
  - [ ] 6.3.3 **`command: sh -c "python wait_for_db.py && alembic upgrade head && fastapi run app/main.py --port 8000"`**
        が通り、`api` のログに3段階（`db:3306 に繋がりました` → `Running upgrade` → `Uvicorn running`）が並ぶか。
        `alembic upgrade head` が **MySQL に対して**問題なく適用されるか（`String(20)` / `JSON` 型の列）
  - [ ] 6.4.1 / 6.4.2 **`node:22-slim` + `npm ci` + `npm run dev -- --host`** で Vite が起動し、
        `- ./web:/app` と `- /app/node_modules` の組み合わせでホットリロードが効くか。
        `- /app/node_modules` を外したときに `vite: not found` になることも確認
  - [ ] 6.4.2 **`vite.config.js` の `server.watch.usePolling` を `process.env.VITE_USE_POLLING` で切り替える形**が、
        使っている Vite のバージョンで有効か（**Windows 実機での確認が必要**）
  - [ ] 6.4.3 **`import.meta.env.VITE_API_BASE_URL` が、`compose.yaml` の `environment` から渡した値を拾うか。**
        Vite が `process.env` の `VITE_` 付き変数を `import.meta.env` に載せる挙動に依存しています。
        **拾わない場合は、`web/.env` を用意する形に本文を直す必要があります**（要確認）
  - [ ] 6.5.2 **`docker compose up -d --build` の出力**（`Healthy` → `Started` の順・`docker compose ps` の3行と
        `db` の `PORTS` が `3306/tcp` だけになること）と、初回ビルドにかかる時間の実測
  - [ ] 6.6.1 3つの「起動できた印」の**実際の文言**（`ready for connections` / `Uvicorn running on http://0.0.0.0:8000` /
        `Local:   http://localhost:5173/`）
  - [ ] 演習 6.3 の3つの壊し方で、**ブラウザに出るエラー名**（`ERR_NAME_NOT_RESOLVED` /
        `blocked by CORS policy` / `Access denied for user`）が解答編どおりになるか
  - [ ] 演習 6.4 の **`adminer:4.8.1`** が起動し、`ADMINER_DEFAULT_SERVER: db` でログイン画面に `db` が入るか
        （バージョンは執筆時点の想定。Docker Hub で現行版を確認してください）
- [ ] **`docker-text` 4.6.3 の補足の修正（D-06 で判明した誤り）**
      4.6.3 の末尾の補足に「React（Vite）でも同じことが起きます。そのときは Vite 用の環境変数
      （`CHOKIDAR_USEPOLLING=true`）を使います」と書かれていますが、**これは誤りです。**
      `CHOKIDAR_USEPOLLING` は Create React App（webpack-dev-server）の慣習で、
      **Vite はこの環境変数を読みません。**
      第6章 6.4.2 では、`vite.config.js` の `server.watch.usePolling` に書く正しい形を示し、
      4.6.3 との違いを補足で説明しています。
      **4.6.3 側の補足を「対処は第6章 6.4.2 で扱います」に書き換えてください**（D-04 の PR に手を入れるか、
      マージ後に修正するかは、レビューの都合で決めてください）。D-06 からは 4.6.3 の本文を変更していません
- [ ] **`docker-text` 第7章（D-07）の実機確認**
      本文・解答編のコマンドと出力は、**Docker Engine 29.3.1（linux/amd64）で実際に実行して確認済み**です
      （イメージサイズ、マルチステージビルド、`useradd` / `USER`、`docker save` による層の中身の取り出し、
      alpine での `pyodbc` のビルド失敗、`docker compose config` のマージ結果）。
      **未確認・確認をお願いしたいのは次の5点**です。
  - [ ] 7.1.1 / 7.2.3 / 7.3.1 の**サイズの数字**。`web` のサイズ（560 MB → 74 MB）は、
        **Vite の素のテンプレートで測った値**です。react-text 第10章の `task-app`
        （ルーティングなどを足したもの）では、開発用イメージがこれより大きくなります。
        **桁と比率（約 7.5 分の1）が変わらなければ、本文の主張は成立します**が、
        気になる場合は実際の `task-app` で測り直して数字を差し替えてください
  - [ ] 7.2.2 の `docker build` の**出力に出ている秒数**（`[+] Building 52.7s` など）。
        パソコンと回線で大きく変わるため、**目安として記載**しています（本文にもその旨を書いています）。
        実機の値と大きく違う場合は差し替えてください
  - [ ] 7.4.2 の **`docker save` を使った層の取り出し**（`${PWD}` / `$(pwd)` のマウント）が、
        **Windows（PowerShell）で動くか**。Linux では動作を確認済みです。
        PowerShell の引用符の扱いで詰まる場合は、`-v "${PWD}:/work"` の書き方を実機に合わせてください
  - [ ] 7.4.3 の補足の **Docker Scout**（Docker Desktop の GUI から見る脆弱性の一覧）。
        **執筆環境に Docker Desktop が無いため、画面と表示内容は未確認**です。
        現行の Docker Desktop で名称・場所が変わっていないか確認してください
  - [ ] 7.5.2 の `docker compose -f compose.yaml -f compose.prod.yaml up -d --build` の**通し確認**。
        マージ結果（`config`）は確認済みですが、**3サービスを本番用の構成で起動した通し確認は未実施**です
        （第6章と同じく、`api` と `db` の実機確認が前提になるため）
- [ ] **`docker-text` 3.2.1 の補足のサイズ表記（D-07 で判明）**
      3.2.1 の補足に `python:3.13`（約 1 GB）とありますが、**実測は 1.62 GB** でした
      （Docker 29 の DISK USAGE。CONTENT SIZE は 431 MB）。
      第7章 7.3.1 では実測値の 1.62 GB を載せているため、**2か所で数字が食い違っています。**
      3.2.1 側を「約 1.6 GB」に直すか、7.3.1 に合わせてください
- [ ] **`docker-text` 第6章の「次の章へ」の記述（D-07 で判明）**
      6章末に「`api` と `web` を合わせると 1 GB を超えています」とありますが、
      **実測では 560 MB + 356 MB = 約 916 MB** で、1 GB をわずかに下回りました
      （`task-app` の依存が増えれば超えます）。
      第7章 7.1.1 では「3つ合わせて 2 GB 前後」（`mysql:8.4` の 1.12 GB を含む）と書いています。
      6章末を「2つ合わせて 900 MB 前後、`db` を含めると 2 GB 前後」に直すと、7.1.1 と揃います
- [ ] **`docker-text` 第8章（D-08）の出力例**
      この章は**新しいコマンドを導入しない締めの章**ですが、次の2か所に出力例を載せています。
      **どちらも実機未確認**です（執筆環境に Docker Desktop と MySQL の実機がないため）。
  - [ ] 8.1.3 の「よくある間違い」と、解答編 演習 8.3 の **`.env` が無いまま起動したときの `db` のログ**
        （`Database is uninitialized and password option is not specified` と、続く3行の環境変数名）。
        6.2.1 で載せたものと同じ内容ですが、**行の並びとタイムスタンプの形は `mysql:8.4` の実機で確認してください**
  - [ ] 解答編 演習 8.3 の **`docker compose ps` の表示**
        （`fullstack-rehearsal-db-1` が `Exited (1)`、`api` が `Created`、`web` が `Up` になること）。
        **`api` が `Created` で止まる**のは `depends_on` の `service_healthy` 待ちという想定ですが、
        Compose のバージョンによっては表示が変わる可能性があります
  - [ ] 解答編 演習 8.3 の **`docker volume ls` に2つのプロジェクトのボリュームが並ぶ出力**
        （`fullstack-lesson_db-data` と `fullstack-rehearsal_db-data`）
  - [ ] 解答編 演習 8.1 の `docker ps` / `docker ps -a` の出力例（`nginx:1.27` を `-p 8090:80` で起動）
- [ ] `mysql-text` 2.1 Docker での MySQL 起動と接続

> **とくに各本の第1〜2章（環境構築）は、必ず自分で通しで実行してください。**
> AI が書いた手順は、コマンド名やオプションが実在しないことがあります。

---

## スクリーンショットが必要な箇所

- [ ] `react-text` 1.3.2 VS Code のインストール画面
- [ ] `react-text` 1.3.3 拡張機能の検索画面
- [ ] `react-text` 1.6.4 開発者ツールの画面
- [ ] `react-text` 3.3.1 ボックスモデルの図（開発者ツール。もしくは下記「図解の変換待ち」で SVG 化してもよい）
- [ ] `fastapi-text` 2.5.1 `/docs`（Swagger UI）の画面（`F-FIN` で検出。この本の価値の中心なので、1枚あると効きます）
- [ ] `fastapi-text` 9.4.2 開発者ツールの Network タブ（`F-FIN` で検出。プリフライトの `OPTIONS` の行が見えるもの）
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
- ~~`python-text` **1.2.3 へのリンクが切れています**（第2章の作業中に発見。第2章の変更ではないため未修正）。
  `01-environment.md` 内の `#123-add-python-to-path-を必ずチェックする` は、
  見出し `### 1.2.3 「Add Python to PATH」を必ずチェックする` の実際のアンカー
  `#123-add-python-to-pathを必ずチェックする`（`path` の直後にハイフンが入らない）と一致していません。
  同ファイル内の2か所と目次から参照されています。`P-FIN` での一括修正か、単独の修正 PR をお願いします。~~
  → **`P-FIN` で修正済み**（3箇所）。あわせて python-text のアンカー付きリンク 954 件を検証し、
  他に切れているものが無いことを確認しました。

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

### 第10章（P-10）について

- 第10章のコード・実行結果は、すべて **Python 3.11 で実際に実行して確認**しました
  （本文の例・演習4問の解答すべて。書き出された CSV の中身も実物です）。
  **3.13 の実機での確認をお願いします。**
- **この章は、章の中で1か所だけインターネット接続を必要とします**（10.3）。
  使っている API は **Open-Meteo の Historical Weather API**
  （`https://archive-api.open-meteo.com/v1/archive`、利用者登録・API キー不要）です。
  次の点の確認をお願いします。
  - このサービスが引き続き無料・鍵なしで使えるか（公開前に一度アクセスして確認）
  - 会社・学校のプロキシ環境で失敗したときの案内（本文は `--no-weather` で回避する形にしています）
  - 本文に載せた気温の値（2024年7月1日〜7日の東京の最高気温 28.2 / 31.4 / 31.4 /
    33.2 / 33.7 / 33.7 / 32.9）は**実際に取得した値**ですが、
    過去データは再解析で微修正されることがあります。読者の手元と1桁ずれても問題ない旨の
    注記を足すべきか、判断をお願いします
- **バージョン表記の確認をお願いします。** 本文の実行結果は次の環境で取得しました。
  - requests 2.34.2 / ruff 0.16.5 / mypy 2.3.1 / types-requests 2.33.0
  - 10.3.1 の `pip show requests` の出力例に `Version: 2.34.2` と書いています
- **mypy のバージョンによって、`types-requests` を入れていないときの挙動が違います。**
  - mypy 1.19：`ignore_missing_imports = true` があっても
    `Library stubs not installed for "requests"  [import-untyped]` が出る
  - mypy 2.3：報告されない（`requests` の検査を諦めるだけ）
  本文（10.6.2）は「バージョンによっては報告が出る。対処は `pip install types-requests`」
  という書き方にしてあります。第9章 9.3.2 の
  「`[import-untyped]` は `ignore_missing_imports` で消す」という記述と
  食い違って見えないか、`P-FIN` で確認をお願いします。
- 10.5.3 の `--help` の実行結果は、**日本語の `metavar` を使っているため列がそろっていません**
  （`argparse` が表示幅ではなく文字数で計算するため）。本文にその旨の補足を入れてあります。
  そろえたほうがよいと判断される場合は、`metavar` を半角（`DATE` / `PATH` / `SHOP`）に
  変更してください。その場合、本文3か所と演習 10.2 の完成条件も直す必要があります。
- **この章では、`python-lesson` ではなく新しいプロジェクト `sales-analyzer` を作ります。**
  第9章までとディレクトリが変わるので、通しで読んだときに混乱しないか確認をお願いします
  （`ai/curriculum-map.md` の第10章の注意にも明記済み）。
- 10.6.3 のチェックリスト7番は、**わざとトレースバックで止まる**確認項目です。
  「エラーを出させる」ことを完成条件に含めているので、読者が失敗だと受け取らないか、
  表現の確認をお願いします。
- 演習で作ったファイル（`out/` 以下の CSV、`weather_check.py`、`retry_check.py`、
  `greet.py`）は手元に残ります。第7章と同じく、後始末の案内が要るか判断をお願いします。

### 第11章（P-11）について

- **バージョン表記と実行結果の確認をお願いします。** 11章の実行結果は、
  **pytest 9.1.1 / pandas 3.0.5 / requests 2.33.1** で実際に動かして取得したものです。
  ただし、pytest のセッション行（`platform win32 -- Python 3.13.1, pytest-9.1.1, pluggy-1.6.0`
  と `rootdir: C:\Users\taro\sales-analyzer`）だけは、
  **他の章と同じく Windows の表示に合わせて書き換えてあります。**
  Windows の実機で1回実行し、`platform` と Python のバージョンの行が
  実際の表示と食い違わないか確認をお願いします。
- **11.2.3 の定期実行の手順は、実機での確認をお願いします。**
  - Windows：タスクスケジューラの「基本タスクの作成」の画面項目名
    （「プログラム/スクリプト」「引数の追加」「開始（オプション）」）が、
    現在の Windows 11 の表示と一致しているか
  - macOS：`crontab -e` の初回実行時に、ターミナルへのフルディスクアクセスの許可を
    求められる場合があります。本文には書いていないので、実機で再現するようなら
    注記を足すべきか判断をお願いします
- **11.2.3 と演習 11.3 は、ファイルを移動するスクリプトです。**
  本文では練習用の `demo_downloads` を作らせ、`APPLY = False`（演習後は `--apply` なし）を
  既定にしていますが、**本物のダウンロードフォルダに向ける読者が出ます。**
  警告の書き方（「注意」の囲み2か所）で足りるか、確認をお願いします。
- pandas 3.0 の表示（`Name: subtotal, dtype: int64` の行、日本語列の桁ぞろえ）は、
  **バージョンが上がると変わる可能性があります。** 公開前に一度実行して確認をお願いします。
- 11.2.1 で `pip install pytest`、11.2.2 で `pip install pandas` を追加で入れています。
  第10章の `requirements.txt` の案内（1.6.3）との整合を、`P-FIN` で確認をお願いします。

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

---

### `P-FIN`（python-text 通し確認）で見つかったもの

**修正済み**（このタスクの PR に含めています）

| 箇所 | 内容 | 対応 |
|------|------|------|
| `ai/curriculum-map.md` python-text の表 | 第4章・第9章の行のセルに、エスケープしていない `\|` が5箇所（集合の `\|`、`型 \| None`、`int \| str`、`-> Product \| None` など）。**表が途中で列に割れて崩れていました** | `\\|` にエスケープ。全 26 行の列数が揃っていることを機械的に確認 |
| `ai/curriculum-map.md` python-text の表 | react-text の表にはある「よくあるつまずき」列が無く、完成した本として粒度が揃っていなかった | 第0章〜第11章の全行に列を追加（各章の「よくある間違い」の囲み記事から採録）。冒頭に「全章完成」の注記も追加 |
| `python-text` 1.2.3 へのリンク3箇所 | アンカーが `#123-add-python-to-path-を必ずチェックする` になっていたが、見出しの `」` が除去されるため実際は `#123-add-python-to-pathを必ずチェックする`。**リンクを踏むと章の先頭に飛んでいました** | 3箇所とも修正。アンカー付きリンク 954 件を機械的に検証し、他に切れているものが無いことを確認 |
| `python-text` 8.6.1 | 「見分け方は簡単です。」（`writing-guidelines` 8章で禁止） | 「見分け方は1つだけです。」に変更 |
| `python-text` 6.2.4 | 「対策は簡単で、〜」（同上） | 「対策は1つ、〜」に変更 |
| `python-text` 4.1.4 | 「メソッド」を 2.4.3 と違う言い回しで再定義していた（`writing-guidelines` 3.2） | 2.4.3 への参照だけを残し、重複した定義を削除 |
| `python-text` 5.3.3 | 「ガード節」を 3.4.2 と違う言い回しで再定義していた | 3.4.2 の定義文に合わせた |
| `python-text` 8.5.2 | 「型ヒント」を「変数や引数に」と定義。glossary と 9.1.2 は「変数や関数に」 | glossary に合わせた |
| `python-text` 5章の「次の章へ」 | 「標準ライブラリ」を 6.3.1 と違う言い回しで先に定義していた | 定義は 6.3.1 に一本化し、予告だけを残した |
| `python-text` 6.3.1 | 「外部ライブラリ」が 9.3.2 と 10.6.2 で説明なしに使われていた（初出時の定義が無い） | 6.3.1 の標準ライブラリの説明の直後に、1文の定義と `cowsay` / `requests` の例を追加 |
| `python-text` 解答編 その1 問 5.5 | 解説が「**理由**：」で始まっており、他 115 問の「**解説**」と揃っていなかった | 「**解説**」に統一 |
| `python-text` 解答編 その1 | 演習の区切りが `### 演習問題`（react-text は `### 演習`） | `### 演習` に統一 |
| `python-text` 解答編 その2 | 第6〜11章に演習の区切り見出しが無かった | 各章に `### 演習` を追加。これで2冊とも同じ構成になった |
| `docs/glossary.md` | python-text で定義したのに未登録だった用語が 31 語（トレースバック、予約語、f-string、PEP 8、ガード節、破壊的、docstring、標準ライブラリ・外部ライブラリ、文字コード、CSV、属性、継承、オーバーライド、特殊メソッド、デコレータ、dataclass、ruff、uv、requests、argparse など） | Python / FastAPI の表に追記。定義文は本文の初出時の表現をそのまま採用 |

**修正しなかったもの（判断を人間に委ねます）**

- [ ] `python-text` 第0章に「囲み記事」の凡例が無い
      → react-text 第0章には「補足／注意／よくある間違い／つまずいたら」の4種類を説明する項があります。
      python-text は「react-text を読んでいなくても読み進められる」と書いているので、
      **1冊目を飛ばした読者は凡例を見ないまま読むことになります。**
      README の章立て（0.4）に無い項を足すことになるため、`P-FIN` では手を入れていません。
      足すなら 0.4.3 のあたりが自然です
- [ ] `python-text` 1.4.1 の「フォルダーを開く」
      → VS Code のメニューの**画面に出る文言そのまま**です（`docs/style-guide.md` 2.1 の補足に従い、そのままにしています）。
      python-text 全体で、表記ゆれに見えるカタカナ語はこの1箇所だけでした
- [ ] `python-text` 4.1.2 の「インデックス」
      → 2.4.2 とほぼ同じ定義文が再掲されています。言い回しは揃っているため実害はありませんが、
      2.4.2 への参照に置き換えるかどうかはご判断ください
- [ ] `python-text` 11.2.2 の「簡単な集計」
      → 「簡単です」の形ではなく、集計の規模を表す形容なので残しました

**機械的に確認して問題が無かったもの**

- README の章立てと本文の見出し（12章・65 節・236 項）が完全に一致。`####` 以下の見出しは 0 件
- 相対リンク 729 件がすべて実在するファイルを指している
- 理解度チェック 74 問・演習 42 問のすべてに解答があり、すべてに解説が付いている
- コードブロックの言語指定漏れ 0 件。本文からの `.svg` の直接参照 0 件。`TODO(review-notes)` の残り 0 件
- 第0章から第11章まで「次の章へ」が途切れず、最後は `fastapi-text/README.md` に接続している
- `python-text/config.yaml` の `chapters`（14 件）と実ファイルが一致
- 「返り値」「ご存知のとおり」「〜が一般的です」などの禁止表現は 0 件

---

### `F-FIN`（fastapi-text 通し確認）で見つかったもの

**修正済み**（このタスクの PR に含めています）

| 箇所 | 内容 | 対応 |
|------|------|------|
| `ai/curriculum-map.md` fastapi-text の表 | 第5章・第9章の行のセルに、エスケープしていない `\|` が3箇所（`` `\| None` を外せる ``、`` `FIELD_LABELS[field] \|\| field` ``）。**表が途中で列に割れて崩れていました**（`P-FIN` で python-text に見つかったのと同じ不具合） | `\\|` にエスケープ。curriculum-map の全テーブル行の列数が揃っていることを機械的に確認 |
| `ai/curriculum-map.md` fastapi-text の表 | react-text / python-text の表にはある「よくあるつまずき」列が無く、完成した本として粒度が揃っていなかった | 第0章〜第10章の全行に列を追加（各章の「よくある間違い」の囲み記事から採録）。冒頭に「全章完成」の注記も追加 |
| `fastapi-text` 9章の「次の章へ」 | 「デプロイ」を「作ったものを、自分以外の人が使える場所に置くこと」と定義していた。glossary と 10.2.1 は「作ったものを、動かす場所に配置して公開すること」 | glossary の定義文に統一 |
| `fastapi-text` 1.1.2 | 「バックエンド」を 0.1.2 と同じ定義文で再掲していた（参照が無く、重複に見える） | この本が他の再掲で使っている形（定義＋出典）に合わせ、`（…。第0章 0.1.2）` の参照を付けた |
| `fastapi-text` 解答編 その1 | 演習の区切りが `### 演習問題`（react-text / python-text は `### 演習`） | 5箇所を `### 演習` に統一 |
| `fastapi-text` 解答編 その2 | 第6〜10章に演習の区切り見出しが無かった | 各章に `### 演習` を追加。これで3冊とも同じ構成になった |
| `fastapi-text` 解答編 その2 演習 8.1 | 「〜すると簡単にずれます」（`writing-guidelines` 8章の禁止表現に触れる） | 「〜すると、この前提はすぐに崩れます」に変更 |
| `fastapi-text/README.md` | 第10章だけ項（`10.1.1`〜`10.2.4`）が章立てに載っていなかった | 本文の見出しに合わせて7項を追記 |
| `fastapi-text/README.md` | 「🚧 執筆中です」のバナーのままだった | 「✅ 全10章＋解答編が完成しています」に差し替え、執筆状況・通し確認の結果・更新履歴を追記（react-text / python-text と同じ形） |
| `docs/glossary.md` | fastapi-text で定義したのに未登録だった用語が 12 語 | 追記（メモリ、PID、アクセスログ、Starlette、OpenAPI、Swagger UI、ReDoc、CRUD、制約（カラムの）、`@property`、PyJWT）。定義文は本文の初出時の表現をそのまま採用 |

**修正しなかったもの（判断を人間に委ねます）**

- [ ] `fastapi-text` 2.2.2 / 2.4.1 のバージョンと出力
      → `fastapi[standard]==0.115.6` の `pip install` の出力、`fastapi dev` の起動表示、
      エラー文言は **F-02 の時点から実機検証が済んでいません。**
      上の「要検証」に積んである項目と同じものです。`-FIN` では動作確認をしていないため、
      **第2章だけは人間が通しで実行してください**（`RUNBOOK.md` 6章）
- [ ] `fastapi-text` 2.3.1 / 8.2.1 の「フォルダー」
      → VS Code のメニューの**画面に出る文言そのまま**です
      （`docs/style-guide.md` 2.1 の補足に従い、そのままにしています）。
      fastapi-text 全体で、表記ゆれに見えるカタカナ語はこの2箇所だけでした
- [ ] `fastapi-text` 4.3.1 の「バリデーション」
      → 3.4.1 とほぼ同じ定義文が再掲されていますが、
      **`（3.4.1）` の参照がすでに付いている**ため、そのままにしました
- [ ] `fastapi-text` に画像が1枚も無い（`images/` ディレクトリが無い）
      → 図解はすべて Mermaid（53 箇所）でまかなえており、SVG→PNG の変換待ちもありません。
      ただし **2.4.2 の `/docs` の画面、9.4.2 の開発者ツールの Network タブ**は、
      文章と Mermaid だけで説明しています。スクリーンショットを足すかはご判断ください
      （上の「スクリーンショットが必要な箇所」にも追記しています）

**機械的に確認して問題が無かったもの**

- README の章立てと本文の見出し（11章・55 節・165 項）が完全に一致。`####` 以下の見出しは 0 件
- 本の中の相対リンク・アンカー 112 件がすべて到達可能
- 理解度チェック 69 問・演習 37 問のすべてに解答があり、すべてに解説が付いている
- コードブロックの言語指定漏れ 0 件。本文からの `.svg` の直接参照 0 件。`TODO(review-notes)` の残り 0 件
- 第0章から第10章まで「次の章へ」が途切れず、最後は `docker-text/README.md` に接続している
- ターミナル操作は全 11 章で PowerShell と bash が同数（合計 90 組）並記されている
- `fastapi-text/config.yaml` の `chapters`（13 件）と実ファイルが一致
- 「簡単です」「すぐできます」「返り値」「ご存知のとおり」「〜が一般的です」などの禁止表現は 0 件
