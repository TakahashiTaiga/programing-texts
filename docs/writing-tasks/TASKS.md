# 執筆タスク キュー

**このファイルは、クラウドルーティンが毎回読み込む「作業指示書」です。**
実行手順と完成条件は [`RUNBOOK.md`](./RUNBOOK.md) にあります。

---

## 使い方（ルーティン向け）

1. 状態が **`進行中`** のタスクがあれば、それを継続する
2. なければ、上から見て最初の **`未着手`** のタスクを**1件だけ**実行する
3. 終わったら、そのタスクの状態を **`完了`** に更新してコミットする

**1回の実行で1タスクだけ。** 次のタスクに進まないでください。

各タスクが書くべき節・項の一覧は、**対象の本の `README.md`** にあります。
このファイルには重複して書きません。

---

## 進捗

| 本 | 完了 / 全体 |
|----|-----------|
| react-text | **13 / 13（完成）** |
| python-text | **13 / 13（完成）** |
| fastapi-text | **12 / 12（完成）** |
| docker-text | **10 / 10（完成）** |
| mysql-text | 2 / 11 |
| **合計** | **50 / 59** |

---

## 1. react-text

章立て: [`react-text/README.md`](../../react-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| R-00 | 完了 | 第0章 はじめに | `react-text/00-introduction.md` | — | 中 | |
| R-01 | 完了 | 第1章 Web の仕組みと開発環境 | `react-text/01-web-and-environment.md` | part1 | 大 | |
| R-02 | 完了 | 第2章 HTML | `react-text/02-html.md` | part1 | 大 | |
| R-03 | 完了 | 第3章 CSS | `react-text/03-css.md` | part1 | 大 | |
| R-04 | 完了 | 第4章 JavaScript の基礎（前半） | `react-text/04-javascript-basics.md` | part1 | 大 | ★このテキストの山場。下の注記を必ず読む |
| R-05 | 完了 | 第5章 JavaScript の基礎（後半） | `react-text/05-javascript-advanced.md` | part1 | 大 | ★山場。5.7.7 が第6章への橋渡し |
| R-06 | 完了 | 第6章 React をはじめる | `react-text/06-react-start.md` | part2 | 中 | |
| R-07 | 完了 | 第7章 props と state | `react-text/07-props-and-state.md` | part2 | 大 | ★React の中核 |
| R-08 | 完了 | 第8章 状態設計と副作用 | `react-text/08-state-design-and-effects.md` | part2 | 大 | |
| R-09 | 完了 | 第9章 ルーティングと全体設計 | `react-text/09-routing-and-architecture.md` | part2 | 中 | |
| R-10 | 完了 | 第10章 実践：タスク管理アプリ | `react-text/10-practice-task-app.md` | part2 | 大 | |
| R-11 | 完了 | 第11章 次のステップ | `react-text/11-next-steps.md` | — | 小 | |
| R-FIN | 完了 | 通し確認 | — | — | 小 | 章立てとの一致・リンク・解答の対応を機械的に確認。用語のブレを修正し、glossary に未登録の 24 語を追記。残りの申し送りは `review-notes.md` へ |

### R-04 の注記

- 学習者が**生まれて初めてプログラムを書く章**です。ここでの離脱が最も多くなります
- 1つの項につき、**必ず「動かして結果を見る」ところまで**書いてください
- `console.log` は 4.1.3 で導入し、以降すべての例で結果を示すこと
- **`==` と `===`、`let` と `const`、スコープ**は、初学者が確実に詰まる箇所です。
  それぞれに「よくある間違い」の囲み記事を置いてください
- 4.6 の関数は次章以降のすべての土台です。分量を惜しまないでください
- **`map` / `filter` / 配列メソッド / 非同期は第5章の内容なので使わないこと**

### R-05 の注記

- **5.4.3「イミュータブルな更新の考え方」は第7章の state 更新の伏線**です。必ず入れてください
- 5.7.7「なぜこれが大変なのか」で、DOM 直接操作のつらさを体験させてから第6章に渡します。
  ここが React を学ぶ動機になるので、具体的なコード量の比較を見せてください

---

## 2. python-text

章立て: [`python-text/README.md`](../../python-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| P-00 | 完了 | 第0章 はじめに | `python-text/00-introduction.md` | — | 中 | 0.3 の JS 対応表が肝 |
| P-01 | 完了 | 第1章 環境構築 | `python-text/01-environment.md` | part1 | 大 | ★要検証（下の注記）。PATH 復旧・実行ポリシー・pip 例は review-notes.md に検証依頼あり |
| P-02 | 完了 | 第2章 基本文法 | `python-text/02-basics.md` | part1 | 大 | 2.6 でインデント説明のため `if` を書き方だけ先取り（詳細は第3章）。第3章のスタブを新規作成 |
| P-03 | 完了 | 第3章 制御構文 | `python-text/03-control-flow.md` | part1 | 中 | 3.1.4 で `in` / `for` のためリストを「作る・含むか調べる・回す」だけ先取り（詳細は第4章）。第4章のスタブを新規作成 |
| P-04 | 完了 | 第4章 データ構造 | `python-text/04-data-structures.md` | part1 | 大 | 4.4.2 のベン図を SVG→PNG で追加。第5章のスタブを新規作成 |
| P-05 | 完了 | 第5章 関数 | `python-text/05-functions.md` | part1 | 中 | 4章で先送りした `sorted()` の `key` を 5.5.3 で回収。第6章のスタブを新規作成 |
| P-06 | 完了 | 第6章 モジュールとパッケージ | `python-text/06-modules.md` | part2 | 中 | 解答編 その2（`91-answers-part2.md`）を新規作成。6.3.5 で `str.join` を追加説明。第7章のスタブを新規作成 |
| P-07 | 完了 | 第7章 ファイル操作と例外 | `python-text/07-files-and-exceptions.md` | part2 | 中 | 7.6.2 で `class ...(Exception):` のみ「例外を増やす決まった書き方」として先取り（詳細は第8章）。第8章のスタブを新規作成 |
| P-08 | 完了 | 第8章 オブジェクト指向 | `python-text/08-oop.md` | part2 | 大 | 8.5.2 で `@dataclass` のために型の注記を先取り（詳細は第9章）。第9章のスタブを新規作成 |
| P-09 | 完了 | 第9章 型ヒントとモダン Python | `python-text/09-typing-and-tools.md` | part2 | 中 | 9.4 の ruff / mypy の出力は実機で確認済み（バージョンは review-notes.md に検証依頼あり）。第10章のスタブを新規作成 |
| P-10 | 完了 | 第10章 実践：データ処理スクリプト | `python-text/10-practice-data-script.md` | part2 | 中 | 新規プロジェクト `sales-analyzer` を作る章。`requests`（Open-Meteo API）と `argparse` を導入。10.1.2 / 10.3.1 / 10.3.2 / 10.3.3 / 10.6.1 に Mermaid 図。第11章のスタブを新規作成 |
| P-11 | 完了 | 第11章 次のステップ | `python-text/11-next-steps.md` | part2 | 小 | 到達度チェックリスト・JS 対応表の答え合わせに加え、pytest / pandas / 自動化を入口だけ紹介。解答編 その2 を「第6章〜第11章」に改題し、第11章を追記 |
| P-FIN | 完了 | 通し確認 | — | — | 小 | 章立てとの一致・リンク（相対 729 件／アンカー 954 件）・解答の対応を機械的に確認。curriculum-map の表崩れ（未エスケープの `\|`）と 1.2.3 への切れたアンカー3件を修正し、「よくあるつまずき」列を追加。用語の重複定義5件を整理し、glossary に未登録の 31 語を追記。解答編の区切り見出しを react-text と統一。残りの申し送りは `review-notes.md` へ |

### P-00 / P-01 の注記

- **P-00 では、react-text を読んだ人向けの JavaScript 対応表（0.3.2）が価値の中心**です。
  `let/const` → 代入のみ、`===` → `==`、`{}` → インデント、配列 → リスト、
  オブジェクト → 辞書、`map` → 内包表記、`null/undefined` → `None` などを表にしてください
- **P-01 は必ず人間が実機で検証してください。** とくに次の2点：
  - Windows のインストーラで「Add Python to PATH」を入れ忘れたときの復旧手順
  - PowerShell の実行ポリシーで venv の有効化が失敗するときの対処

---

## 3. fastapi-text

章立て: [`fastapi-text/README.md`](../../fastapi-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| F-00 | 完了 | 第0章 はじめに | `fastapi-text/00-introduction.md` | — | 小 | 0.1.1 に python-text の戻り場所つき前提表と `--version` 確認、0.1.2 に完成形の Mermaid 図、0.3.1 に API 開発ならではの進め方（サーバー常駐・ターミナル2つ・確認の輪）。コードを書かない章のため理解度チェック／演習はなし（R-00 / P-00 と同じ扱い）。第1章のスタブを新規作成 |
| F-01 | 完了 | 第1章 Web API とは | `fastapi-text/01-web-api.md` | part1 | 中 | 解答編 その1（`90-answers-part1.md`）を新規作成。1.5 は JSONPlaceholder を使用（要ネット接続）。1.1.2 / 1.2.1 / 1.3.1 に Mermaid 図。第2章のスタブを新規作成 |
| F-02 | 完了 | 第2章 FastAPI をはじめる | `fastapi-text/02-getting-started.md` | part1 | 中 | ★要検証（`fastapi[standard]==0.115.6` の出力・`fastapi dev` の表示・エラー文言。`review-notes.md` に検証依頼あり）。2.1.1 / 2.3.2 / 2.4.3 / 2.5.4 に Mermaid 図。第3章のスタブを新規作成 |
| F-03 | 完了 | 第3章 パラメータを受け取る | `fastapi-text/03-parameters.md` | part1 | 中 | 練習用データ `tasks` を `main.py` に持つ形に統一（保存は第6章）。3.1.4 / 3.3.3 / 3.4.2 に Mermaid 図。ボディは `dict` で受け取り、**`500` になる体験を第4章 Pydantic の動機**として残した。`404` は第5章 5.4.1、`201` は第4章 4.4.3 へ先送り（本文に明記）。glossary にパスパラメータ・クエリパラメータ・リクエストボディ・クッキー・パーセントエンコーディングを追加。第4章のスタブを新規作成 |
| F-04 | 完了 | 第4章 Pydantic | `fastapi-text/04-pydantic.md` | part1 | 中 | v2 系で執筆。**本文・解答編のコードと JSON は `fastapi==0.115.6` / `pydantic==2.13.5` / `pydantic-settings==2.7.0` / Python 3.11 で実際に実行して確認済み**。第3章から先送りしていた `201`（3.3.2）を 4.4.3 で回収。`404` は第5章 5.4.1 のままなので、見つからないときは `TaskRead \| None` で `null` を返す形にした。`PUT` を `PATCH` に変更（`model_dump(exclude_unset=True)`）。4.1.1 / 4.4.1 / 4.5.1 / 4.6.2 に Mermaid 図。`pydantic-settings` の追加インストールが必要。glossary に Pydantic・モデル・レスポンスモデル・バリデータ・正規表現・設定を追加。第5章のスタブを新規作成 |
| F-05 | 完了 | 第5章 プロジェクト構成 | `fastapi-text/05-project-structure.md` | part1 | 中 | `main.py` 1ファイルから **`app/` パッケージ**（`config.py` / `schemas.py` / `data.py` / `dependencies.py` / `errors.py` / `routers/`）へ移行する章。**起動コマンドが `fastapi dev app/main.py` に変わる**。第4章から先送りしていた `404`（`HTTPException`）を 5.4.1 で回収。**エラーレスポンスを `{"error": {"status", "message", "detail"}}` に統一した（5.4.3）。第6章以降もこの形を使うこと**。5.1.1 / 5.2.2 / 5.2.3 / 5.3.1 / 5.3.3 / 5.3.4 / 5.6.1 に Mermaid 図。**本文・解答編のコードと出力は `fastapi==0.115.6` / `pydantic==2.13.5` / `pydantic-settings==2.7.0` / Python 3.11 で実際に実行して確認済み**。glossary にルーター・依存性注入の補足・ミドルウェア・ログ・ログレベル・ロガー・例外ハンドラ・`409` を追加。第6章のスタブを新規作成 |
| F-06 | 完了 | 第6章 データベース連携 | `fastapi-text/06-database.md` | part2 | 大 | SQLite + SQLAlchemy 2.0 系（`DeclarativeBase` / `Mapped` / `select()`）。**本文・解答編のコードと出力は `sqlalchemy==2.0.36` / `alembic==1.14.0` / `fastapi==0.115.6` / `pydantic==2.13.5` / Python 3.11 で実際に実行して確認済み**。`app/database.py` / `app/models.py` / `app/seed.py` を新規追加し、`app/data.py` は役目を終える。**エンティティ間の関連（外部キー・`relationship`）は扱わない**（担当者は `owner_name` / `owner_email` の平らな列＋`@property`。理由は curriculum-map に明記）。6.6 で `create_tables.py` を捨てて Alembic に移行（`app.db` を作り直す）。`@property` はここが初出（python-text では未習）。解答編 その2（`91-answers-part2.md`）を新規作成。6.1.1 / 6.1.2 / 6.2.1 / 6.2.3 / 6.3.3 / 6.4.5 / 6.5.1 / 6.6.1 に Mermaid 図。glossary に永続化・ORM・SQLAlchemy・SQLite・エンジン・セッション・モデル・スキーマ・コミット・ロールバック・ページネーション・Alembic を追加。第7章のスタブを新規作成 |
| F-07 | 完了 | 第7章 認証 | `fastapi-text/07-authentication.md` | part2 | 大 | パスワードは **bcrypt**（`bcrypt==4.2.1` を直接使用。`passlib` は使わない）、トークンは **PyJWT**（`pyjwt==2.10.1` / `HS256`）。**本文・解答編のコードと出力は `fastapi==0.115.6` / `sqlalchemy==2.0.36` / `alembic==1.14.0` / `bcrypt==4.2.1` / `pyjwt==2.10.1` / Python 3.11 で実際に実行して確認済み**。`app/security.py` / `app/routers/users.py` / `app/routers/auth.py` を新規追加し、`users` テーブルのマイグレーションを1本足す。**`TaskCreate` から `owner` を外し、登録者はトークンから決める形に変更**（`Owner` スキーマは不要になる。第9章の React 側もこの前提）。**読むのは誰でも、書き換えるのは本人だけ**という方針（`GET /tasks` は認証不要）。5.4.3 の `handle_http_exception` に **`headers=exc.headers` を追加**（無いと `WWW-Authenticate` が落ちる）。7.3.1 に「既存テーブルに空にできない列を足すと `Cannot add a NOT NULL column`」→ `server_default` の注意（演習 7.4 で実際に踏む）。7.1.1 / 7.1.2 / 7.2.3 / 7.4.1 / 7.5.2 / 7.5.3 に Mermaid 図。glossary に認証・認可・平文・ハッシュ関数・ハッシュ値・ソルト・bcrypt・トークン・JWT・ペイロード・署名・秘密鍵・ベアラー認証・アカウント列挙・XSS・CSRF を追加。第8章のスタブを新規作成 |
| F-08 | 完了 | 第8章 テスト | `fastapi-text/08-testing.md` | part2 | 中 | pytest（`pytest==9.1.1` を別途 `pip install`）＋ `TestClient`。**本文・解答編のコードと出力は `fastapi==0.115.6` / `pytest==9.1.1` / `httpx==0.28.1` / `sqlalchemy==2.0.36` / `bcrypt==4.2.1` / `pyjwt==2.10.1` / Python 3.11 で実際に実行して確認済み**（本文16件・演習14件のテストがすべて green）。`fastapi-lesson/pytest.ini`（`pythonpath = .` / `testpaths = tests`）と `tests/`（`conftest.py` / `test_security.py` / `test_schemas.py` / `test_tasks.py`）を新規追加。**テストは `test.db` を使い、`app.dependency_overrides[get_db]` で差し替える**（5.3.4 の伏線を回収）。8.3 ではあえて `app.db` を使ってデータが増える・消えるのを体験させ、8.4 で分離する構成。8.3.4 で `get_my_task` の `!=` を `==` に壊してテストが捕まえる実演あり。8.1.1 / 8.3.1 / 8.4.1 / 8.4.2 / 8.4.3 / 8.5.2 に Mermaid 図。glossary にリグレッション・正常系・異常系・テストクライアント・fixture・`conftest.py`・カバレッジ・httpx を追加。第9章のスタブを新規作成 |
| F-09 | 完了 | 第9章 実践：React と繋ぐ | `fastapi-text/09-practice-connect-react.md` | part2 | 大 | react-text 第10章の `task-app` を API に繋ぎ変える章。**`src/api/client.js` / `src/api/tasks.js` を新設**し、`localStorage` によるタスク保存をやめる（トークンの保存にだけ `localStorage` を使う）。`toTask` で API の形（`done` / `owner.name` / `{count, tasks}`）をアプリの形（`isDone` / `ownerName`）へ変換。**`created_at` が既存データで `null` のため、並べ替えは `id` に変更**。FastAPI 側の変更は `app/config.py` の `cors_origins` と `app/main.py` の `CORSMiddleware` のみ（`.env` に `CORS_ORIGINS`）。**React 側の上限 30 文字と API 側の 20 文字の食い違いを、9.3.3 の題材として意図的に使ってから 20 にそろえる**。9.1.2（2つ）/ 9.2.1 / 9.2.2 / 9.3.1 / 9.4.1 に Mermaid 図。glossary にオリジン・同一オリジンポリシー・プリフライトリクエストを追加し、CORS の定義を「制限する仕組み」から「許可を出す仕組み」に修正。第10章のスタブを新規作成 |
| F-10 | 完了 | 第10章 次のステップ | `fastapi-text/10-next-steps.md` | part2 | 小 | 到達度チェックリスト（32項目）、デプロイの概観（`fastapi dev` と **`fastapi run`** の違い・手元と本番の対応表・**死活監視と `503`**）、公開前チェック8項目、`.env.example` の突き合わせ（**第9章で `CORS_ORIGINS` が抜けていたのを演習で回収**）、手順書の書き方と docker-text への橋渡し。**特定のデプロイ先の手順は意図的に書いていない**（10.2.4）。演習の `/health`（`200` と `503`）と `conftest.py` の fixture は `fastapi==0.115.6` / `sqlalchemy==2.0.36` / `pytest==9.1.1` / `httpx==0.28.1` / Python 3.11 で実行して確認済み。10.1.2 / 10.2.2 / 10.3.2 と解答編に Mermaid 図。解答編 その2 を「第6章〜第10章」に改題し第10章を追記 |
| F-FIN | 完了 | 通し確認 | — | — | 小 | 章立てとの一致（11章・55 節・165 項）・リンク（本の中の相対／アンカー 112 件）・解答の対応（106 問すべてに解説あり）を機械的に確認。curriculum-map の表崩れ（未エスケープの `\|` 3箇所）を修正し、「よくあるつまずき」列を追加。デプロイの定義文を glossary に統一し、バックエンドの重複定義に参照を付与。解答編の区切り見出しを react-text / python-text と統一。glossary に未登録の 12 語を追記。残りの申し送りは `review-notes.md` へ |

### F-07 の注記

認証はセキュリティに直結します。次を必ず守ってください。

- パスワードのハッシュ化は**必ず専用ライブラリ**を使う。自作しない
- 秘密鍵は環境変数から読む。**コード中にベタ書きした例を載せない**
- 「学習用なので簡略化」する場合は、**本番で何が足りないかを明記**する
- 7.6「やってはいけないこと」を省略しない

---

## 4. docker-text

章立て: [`docker-text/README.md`](../../docker-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| D-00 | 完了 | 第0章 はじめに | `docker-text/00-introduction.md` | — | 小 | 0.1.1 に react-text / python-text / fastapi-text の戻り場所つき前提表と `node --version` / `python --version` の確認（**入っていなくても第5章までは読める**と明記）、0.1.2 に3冊で踏んだつまずきの一覧と「渡す相手に伝える7手順」→ `docker compose up` 1行の比較（Mermaid 図）、0.2.2 に **OS だけでなく CPU（Apple Silicon か否か）を伝える**指示と環境依存の要因表・質問テンプレート・秘密情報の伏せ方、0.3 に進め方（壊して作り直せる／空き容量 20 GB／確認の輪の Mermaid 図）。**`docker` コマンドは `docker compose up` をゴールとして名前だけ見せ、実行はさせない**（インストールは第2章）。コードを書かない章のため理解度チェック／演習はなし（R-00 / P-00 / F-00 と同じ扱い）。README の 0.3 に項（0.3.1〜0.3.3）を追加。第1章のスタブを新規作成 |
| D-01 | 完了 | 第1章 Docker が解決する問題 | `docker-text/01-why-docker.md` | あり | 中 | **コマンドを1つも打たない章**（インストールは第2章）。1.1 でアプリが動く土台を **① 設定 / ② ライブラリ / ③ ランタイム / ④ OS** の4層に整理し、3冊で踏んだトラブルを各層に割り当てる。組み合わせ爆発（162 通り）→「組み合わせを1つに固定して環境ごと配る」へ接続。1.1.3 で `requirements.txt` / `venv` が揃えられるのは②までと明示。1.2 は仮想マシンとコンテナの積層図を Mermaid で対比し、**Windows / macOS では Linux の仮想マシンを1つだけ動かしてその中にコンテナを並べる**ことを補足（第2章で WSL2 が必要になる理由）。1.3 でイメージ＝設計図／コンテナ＝実物、`名前:タグ`、Docker Hub の読み方（`nginx` を例に Compressed size まで）、**`latest` は「最新」ではない**（詳細は 2.5.5）。1.4 は手順の **A / B / C 仕分け**を別題材（議事録ツール）で実演してから演習に渡す。**解答編 `90-answers.md` を新規作成**（docker-text は分割せず1ファイル）。1.1.1 / 1.1.2 / 1.1.3 / 1.2.1 / 1.2.2 / 1.3.2 / 1.3.3 / 1.4.2 / 1.4.3 に Mermaid 図（SVG→PNG は使用せず）。演習はすべて**ブラウザとメモだけで完結**する形にした（演習 1.2 は Docker Hub の閲覧のみ）。glossary に仮想マシン・ホスト OS・ゲスト OS・ハイパーバイザ・カーネル・プロセス・ランタイム・組み合わせ爆発・タグ・レジストリ・Docker Hub・本番環境・開発環境を追加。README の解答編にリンクを追加。第2章のスタブを新規作成 |
| D-02 | 完了 | 第2章 インストールと基本操作 | `docker-text/02-install-and-basics.md` | あり | 大 | ★要検証（インストール画面・WSL2 / BIOS / Apple Silicon / プロキシの手順は実機未確認。`review-notes.md` に検証依頼あり）。**本文・解答編のコマンドと出力は Docker Engine 29.3.1（linux/amd64）で実際に実行して確認済み**（`hello-world` / `nginx:1.27` / `python:3.13-slim` / `python:3.12-slim`）。**`docker images` の表示が Docker 29 で変わった**ため、29 の列を本文に載せ、28 以前の列を補足で併記（2.5.1）。D-02 の注記の5項目は 2.2.1〜2.2.5 で網羅（**プロキシ用に 2.2.5 を新設し、README の章立てにも追加**）。`stop` / `rm` の違いは 2.4.5 の状態遷移図に集約。`--rm`（2.4.3）と `docker run イメージ コマンド`（2.5.4）を演習 2.3 の下敷きとして本文に用意。**`docker system prune -a --volumes` はこの本では最後まで使わない方針**を 2.7.1 で明記。2.1.1 / 2.1.3 / 2.2 / 2.3.2 / 2.3.3 / 2.4.5 / 2.5.3 / 2.7.1 / 2.7.2 に Mermaid 図（SVG→PNG は使用せず）。glossary にデーモン・CLI・nginx・Web サーバー・フォアグラウンド実行・バックグラウンド実行・アーキテクチャ・Rosetta 2・プロキシ・prune を追加。第3章のスタブを新規作成 |
| D-03 | 完了 | 第3章 Dockerfile | `docker-text/03-dockerfile.md` | あり | 大 | ★要検証（**`docker build` 系の出力は実機未確認**。`review-notes.md` に検証依頼あり）。練習用に `docker-lesson`（`main.py` + `requirements.txt`）を新規に作り、3.2 で1命令ずつ Dockerfile を育てる構成。**`ENTRYPOINT` の確認は 3.3.3**（ビルドを学んだあと）に置き、3.2.7 は説明のみ。`CMD` は **`fastapi dev` ではなく `fastapi run`**（`127.0.0.1` 問題。`0.0.0.0` の詳細は第4章 4.4.3 に送った）。3.4.3 は**わざと順番を入れ替えてビルド時間を比較**する体験に。3.6 は `fastapi-lesson` に `.dockerignore` と `Dockerfile` の2ファイルを足すだけの形にし、**`app.db` を焼き込まない**ため `no such table: tasks` になるところを見せて、`docker exec` → `alembic upgrade head` → `python -m app.seed` で回復させ、**作り直すと消える**ことを第4章（ボリューム）への動機にした。3.1.1 / 3.1.2 / 3.3.1 / 3.4.1 / 3.4.2 / 3.4.3 / 3.5.1 / 3.6.3 に Mermaid 図（SVG→PNG は使用せず）。演習 3.2 は**第2章 演習 2.2（`docker cp`）の回収**。glossary にベースイメージ・ビルド（Docker）・ビルドコンテキスト・`.dockerignore`・ビルドキャッシュを追加。第4章のスタブを新規作成 |
| D-04 | 完了 | 第4章 ボリュームとネットワーク | `docker-text/04-volumes-and-networks.md` | あり | 大 | **本文・解答編のコマンドと出力は Docker Engine 29.3.1（linux/amd64）で実際に実行して確認済み**（マウント・ボリューム・ポート・ネットワーク・CRLF の全実演と、演習4問の通し実行）。4.1.1 は `fastapi-lesson` が無くても読めるよう `python:3.13-slim` + `sleep 600` の小さな実演に差し替え。**`app.db` は `-v api-data:/data -e DATABASE_URL=sqlite:////data/app.db`（スラッシュ4本）で残す**形に統一（第6章以降もこの形）。**コードのパスに名前付きボリュームを被せると古い中身が残り続ける**ことを 4.3.3 の「よくある間違い」に明記。4.4.3 で `fastapi dev` の `--host 0.0.0.0` を回収（3.2.5 からの先送り）。4.6 は Windows 向け（CRLF / `.gitattributes` / ファイル監視）。**インストール系と同じく OS 固有部分は実機未確認**（`review-notes.md` に検証依頼あり）。4.1.1 / 4.1.2 / 4.2.2 / 4.3.1 / 4.3.3 / 4.4.1 / 4.4.3 / 4.5.1 / 4.5.3 / 4.5.4 に Mermaid 図（SVG→PNG は使用せず）。glossary にマウント・名前付きボリューム・匿名ボリューム・書き込み層・`:ro`・デフォルトブリッジ・名前解決・`host.docker.internal`・改行コード・シバン・ポーリング・`.gitattributes` を追加。第5章のスタブを新規作成 |
| D-05 | 完了 | 第5章 Docker Compose | `docker-text/05-compose.md` | あり | 大 | 第4章の長い `docker run`（`--network` + `-p` + `-v` + `-e`）を `compose.yaml` に書き写す構成。5.1 で動機、5.2 で YAML の3ルール（コロン後の空白・スペース2つ・タブ禁止）と最小構成（nginx）→ `build`/`image` → `ports`/`volumes`/`environment`、5.3 で2つ目のサービス `db`（`mysql:8.4`）を並べる（**この章では API を MySQL に繋がない**＝第6章。`db` に `ports` を書かない・サービス名で名前解決・ネットワーク自動生成）、5.4 で `up`/`down`/`down -v`/`ps`/`logs`/`exec`/`--build`、5.5 で `.env` + `${...}` と `.gitignore`/`.env.example`、5.6 で `depends_on` の限界 → ヘルスチェック（`mysqladmin ping`）→ アプリ側リトライ（`wait_for_db.py`）。**名前付きボリュームの2か所宣言**と**プロジェクト名の接頭辞**（`fastapi-lesson_api-data` ≠ 第4章の `api-data`）を「よくある間違い」に。5.1.2 / 5.3.1 / 5.6.1 / 5.5.3 に Mermaid 図（SVG→PNG は使用せず）。★要検証（**Compose / MySQL 系の出力は実機未確認**。とくに `mysqladmin ping` のヘルスチェックと MySQL 8.4 の起動ログ。`review-notes.md` に検証依頼あり）。glossary に YAML・サービス・ヘルスチェックを追加。第6章のスタブを新規作成 |
| D-06 | 完了 | 第6章 実践：React + FastAPI + MySQL | `docker-text/06-practice-full-stack.md` | あり | 大 | ★3冊分の成果物を統合。新規プロジェクト `fullstack-lesson/` を作り、`api/`（`fastapi-lesson` のコピー）と `web/`（`task-app` のコピー）を置く構成。6.1 で3サービスと**通信経路**（**ブラウザは Docker のネットワークの外**＝React の `fetch` は `localhost:8000`、`api` → `db` はサービス名。`http://api:8000` は `ERR_NAME_NOT_RESOLVED`）、6.2 で MySQL 公式イメージ（`MYSQL_ROOT_PASSWORD` 無しで落ちる様子を**わざと見せる**・`MYSQL_USER` を使わせる・`/var/lib/mysql` に永続化・`mysql` コマンドで接続確認）、6.3 で **PyMySQL + cryptography**（MySQL 8 の `caching_sha2_password` のため）・`check_same_thread` の分岐（`startswith("sqlite")`）・`pool_pre_ping`・接続 URL（`?charset=utf8mb4`）・**`command:` で `sh -c "wait_for_db && alembic upgrade head && fastapi run"`**、6.4 でフロントの開発用 Dockerfile（`node:22-slim` / `npm ci` / `--host`）・**`- /app/node_modules`**（バインドマウントで `node_modules` が隠れる問題）・`VITE_API_BASE_URL`、6.5 で37行の完成版 `compose.yaml` と通し確認（下から4段階）、6.6 で切り分け（**エラーの場所と原因の場所は違う**）。6.1.2 に2点、6.3.2 / 6.3.3 / 6.4.2 / 6.6.2 に Mermaid 図（SVG→PNG は使用せず）。**`web` に `depends_on` を書かない**理由を 6.5.1 に明記。★要検証（**3サービスの実機通し確認は未実施**。とくに MySQL 8.4 + PyMySQL の接続、Vite の `usePolling` 設定、`adminer` の演習。`review-notes.md` に検証依頼あり）。**docker-text 4.6.3 の補足（Vite は `CHOKIDAR_USEPOLLING` で切り替わる）は誤りなので 6.4.2 で正しい方法を示し、4.6.3 の修正依頼を `review-notes.md` に追記した** |
| D-07 | 完了 | 第7章 イメージの最適化と本番運用 | `docker-text/07-optimization.md` | あり | 中 | **本文・解答編のコマンドと出力は Docker Engine 29.3.1（linux/amd64）で実際に実行して確認済み**（イメージサイズ・`docker history`・マルチステージビルド・`useradd` / `USER` と `Permission denied`・`docker save` による層からの秘密の取り出し・alpine での `pyodbc` のビルド失敗・`docker compose config` のマージ結果）。7.2 で `web/Dockerfile.prod`（`node:22-slim AS builder` → `nginx:1.27-alpine`）と `web/nginx.conf` を新規追加し、**560 MB → 74 MB**。**`-f Dockerfile.prod` はここが初出**。**Vite の環境変数はビルド時に焼き付く**ため `ARG` + `--build-arg`（実行時の `-e` は効かない）を 7.2.2 で扱った。7.3.2 の alpine の落とし穴は、**「pip が遅い」という古い説明を採らず**、`musllinux` のホイールが無い場合（`pyodbc`）に絞って実測で示した（この本の `requirements.txt` は alpine でも入る）。`useradd` が無い（`adduser`）ことも 7.3.2 の表に入れ、演習 7.3 で踏ませる。7.4.1 で `api` を **`appuser`（uid 1001）実行に変更**（`Dockerfile` を書き換え。**`USER` は `pip install` より後**）。7.5 は `compose.prod.yaml` を追加し、**リストは足し算される**ことと **`!override` / `!reset`** を実測出力で示した。第8章のスタブを新規作成。7.2.1 / 7.2.2 / 7.4.1 / 7.4.2 / 7.5.1 / 7.5.3 と 7.1.2 に Mermaid 図（SVG→PNG は使用せず）。★要検証（`task-app` 実物でのサイズ・ビルド時間の秒数・Windows での `docker save` 検証・Docker Scout の画面・本番構成での通し起動。`review-notes.md` に検証依頼あり）。**3.2.1 の `python:3.13`「約 1 GB」（実測 1.62 GB）と、6章末の「1 GB を超えています」（実測 916 MB）の食い違いも review-notes に申し送り** |
| D-08 | 完了 | 第8章 次のステップ | `docker-text/08-next-steps.md` | あり | 小 | **コマンドを新しく学ばない締めの章**（8.1.1 と 8.2 と 8.3 は読み物、手を動かすのは 8.1.3 と演習）。README の 8.1 / 8.2 / 8.3 に項（8.1.1〜8.1.4 / 8.2.1〜8.2.4 / 8.3.1〜8.3.3）を追加。8.1.1 に**戻る場所つき到達度チェックリスト35項目**（第1章〜第7章。F-10 の32項目と同じ形式）。8.1.3 が演習の土台で、**引き継ぎ用 `README.md` の見本**と、**別ディレクトリにコピーして「揃っていない状態」から再現する**方法（**プロジェクト名が変わるのでボリュームも別になる**＝5.2.4 の回収。元のデータを壊さずに初回起動をやり直せる）。`.env` が `.gitignore` にあるため Git 経由のコピーで付いてこないこと、そのとき `db` が 6.2.1 と同じ `MYSQL_ROOT_PASSWORD` エラーで落ちることを「よくある間違い」に。**fastapi-text 10.3.1 の「要るもの8個」が2個（Docker Desktop と `.env`）に減った**ことを表で回収。8.2 は**道具の名前と担当する困りごとだけ**を示し、インストールはさせない（Kubernetes / レジストリ / CI/CD / ログ・監視・バックアップ / `swarm`・`profiles`・`buildx`・`secrets`・`compose watch`）。**7.5.2 の「足りないもの」5つ全部に担当を割り当てた表**を 8.2.4 に置き、これが演習 8.4 の土台。**バックアップの具体的な取り方（`mysqldump` など）は mysql-text 第8章に送った**（この本では扱わない）。8.2.3 に**`down` / `down -v` / `up -d --build` で何が消えるかの表**（`--build` では消えない）。8.3.1 で **SQL の文とテーブルの形を自分で書いていない**ことを図で見せ、5冊目へ渡す。8.1.2 / 8.2.1 / 8.2.2 / 8.3.1 に Mermaid 図（SVG→PNG は使用せず）。glossary に Kubernetes・CI/CD・CI・リバースプロキシ・監視・バックアップを追加。**この章は新しいコマンドを導入しないため実機確認は不要だが、8.1.3 と演習 8.3 に載せた `db` の起動失敗ログと `docker volume ls` の出力は実機未確認**（`review-notes.md` に検証依頼あり） |
| D-FIN | 完了 | 通し確認 | — | — | 小 | PR #47（`task/D-FIN-review`）で実施済み。**この行は M-01 の実行時に、RUNBOOK 4.6 に従って状態の反映漏れを補正したもの**（`main` 側は PR 未マージのため「未着手」のままだった）。内容は PR #47 を参照 |

> docker-text の解答編は分割せず `docker-text/90-answers.md` の1ファイルにまとめます。

### D-02 の注記

**この章で環境が立ち上がらないと、以降の章がすべて読めなくなります。**
2.2「よくあるトラブル」を厚く書いてください。最低限これらを網羅すること：

- WSL2 が未インストール／未更新（Windows）
- BIOS/UEFI で仮想化が無効
- Apple Silicon での `platform` 指定
- Docker Desktop 未起動での `Cannot connect to the Docker daemon`
- 企業プロキシ環境での `docker pull` 失敗

---

## 5. mysql-text

章立て: [`mysql-text/README.md`](../../mysql-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| M-00 | 完了 | 第0章 はじめに | `mysql-text/00-introduction.md` | — | 小 | PR #48（`task/M-00-introduction`）で実施済み。**この行は M-01 の実行時に、RUNBOOK 4.6 に従って状態の反映漏れを補正したもの**（`main` 側は PR 未マージのため「未着手」のままだった）。内容は PR #48 を参照 |
| M-01 | 完了 | 第1章 データベースとは | `mysql-text/01-what-is-database.md` | part1 | 中 | **SQL を1文も実行しない章**（MySQL の起動は第2章）。演習3問はすべて紙・テキストエディタ・表計算ソフトだけで完結する設計。1.1 は「データベースを使わずに `tasks.json` に保存したら」から入り、**遅い / 同時書き込みで消える・壊れる / 整合性が崩れる**の3問題を、それぞれの担当（第7章インデックス・第4章 4.5 ロック・4.4 トランザクション・第5章 5.1 データ型 / 5.2 制約 / 5.4 外部キー）に割り当てた表で締める。**型と制約では表記ゆれを止められない**ことを 1.1.3 に明記し、1.2.2 の「表を分ける」へ繋いだ（演習 1.1 の最後の完成条件の土台）。1.2.2 に**分ける手順4ステップ**と、**それを2回回す例（`owner_id` + `project_id`）**を置いた（演習 1.2 が「相手の主キーを持つ列が2つ以上」を求めるため。3.6 の自己点検で追加）。1.2.3 は python-text の `for` と `SELECT` を並べて**宣言型**を説明。1.3.2 の**「行に順番が無い → 必ず `ORDER BY`」**が第3章 3.4 の伏線。1.4 は fastapi-text 6.3 の `primary_key=True` を回収し、**3条件の判定フロー図**＋社員テーブルでの評価表（演習 1.3 の雛形）。**自然キー／代理キーは名前と方針まで**（詳細は第5章 5.3.3）、**複合主キー**は第6章 6.7.1 への予告。1.5 は**サーバー型とファイル型**の対比で、fastapi-text の `app.db` が SQLite だった回収（`dir` / `ls -l` の確認手順を Windows / macOS 両方で記載）。**解答編 その1（`90-answers-part1.md`）を新規作成。** 1.1.1 / 1.1.2 / 1.1.3 / 1.2.2 / 1.2.3 / 1.4.2 / 1.5.1 に Mermaid 図（SVG→PNG は使用せず）。glossary に値・一意・自然キー・代理キー・複合主キー・データ型・`NULL`・整合性・1対多・結合・正規化・DBMS・クエリ・宣言型・方言・全件走査・ロック・サーバー型・ファイル型・PostgreSQL・NoSQL を追加。curriculum-map に第1章の行と第1章向けの AI 向け注意書きを追記。README の解答編にリンクを追加。第2章のスタブを新規作成 |
| M-02 | 未着手 | 第2章 環境構築 | `mysql-text/02-environment.md` | part1 | 中 | ★練習用データを確定させる |
| M-03 | 未着手 | 第3章 データを取り出す（SELECT） | `mysql-text/03-select.md` | part1 | 大 | |
| M-04 | 未着手 | 第4章 データを変更する | `mysql-text/04-modify-data.md` | part1 | 中 | |
| M-05 | 未着手 | 第5章 テーブル設計 | `mysql-text/05-table-design.md` | part1 | 大 | |
| M-06 | 未着手 | 第6章 結合と集計 | `mysql-text/06-join-and-aggregate.md` | part2 | 大 | ★この本の山場 |
| M-07 | 未着手 | 第7章 インデックスと実行計画 | `mysql-text/07-index-and-explain.md` | part2 | 中 | |
| M-08 | 未着手 | 第8章 アプリから使う | `mysql-text/08-use-from-app.md` | part2 | 中 | |
| M-09 | 未着手 | 第9章 次のステップ | `mysql-text/09-next-steps.md` | — | 小 | フェーズ1完走の締め |
| M-FIN | 未着手 | 通し確認 | — | — | 小 | |

### M-02 の注記

**2.5 で決めた練習用テーブルを、第3章以降すべてで使い回します。**
ここで決めた構成は後から変えられないので、次を満たすものにしてください。

- 最低3テーブル（1対多と多対多の両方が作れること）
- 日本語データを含む（文字コードの問題を実感させるため）
- 集計の練習ができる数値列と日付列がある
- 投入用の SQL をコードブロックで全文提示する（学習者がコピペで再現できること）

第3章以降のタスクは、**この章で定義したテーブルだけを使って**例題を作ってください。

---

## `-FIN` タスクの内容

`R-FIN` / `P-FIN` / `F-FIN` / `D-FIN` / `M-FIN` は、1冊を書き終えたあとの仕上げです。
本文の新規執筆はせず、次を行います。

1. 全章を通しで読み、**用語のブレを統一する**（`docs/glossary.md` に照らす）
2. 同じ説明の重複を削る／前の章への参照に置き換える
3. 章間リンク・解答編へのリンクがすべて有効か確認する
4. `docs/style-guide.md` の「章を書き終えたらチェックリスト」を全章分で確認する
5. `config.yaml` の `chapters` と実ファイルが一致しているか確認する
6. `ai/curriculum-map.md` の該当セクションを最終版に更新する
7. その本の `README.md` の「執筆状況」を更新する
8. **人間向けの申し送りを `docs/writing-tasks/review-notes.md` に書く**
   （検証が必要な箇所、スクリーンショットが必要な箇所、自信のない記述）

---

## 状態の凡例

| 状態 | 意味 |
|------|------|
| `未着手` | まだ手をつけていない |
| `進行中` | 前回の実行で途中まで書いた。備考に到達点を記載 |
| `完了` | 本文・解答編・curriculum-map まで終わっている |
| `保留` | 意図的に飛ばす。備考に理由を記載 |

---

## 実行順序について

上から順に実行するのが基本ですが、**依存関係は次のとおり**です。

```
react-text  R-04 → R-05 → R-06 → R-07 → R-08 → R-09 → R-10 → R-11
                                                          ↓
python-text P-00 → ... → P-11                     （F-09 が R-10 の成果物を使う）
                    ↓
fastapi-text F-00 → ... → F-09 ←──────────────────────────┘
                    ↓
docker-text  D-00 → ... → D-06 ← fastapi-text と react-text の成果物を使う
                    ↓
mysql-text   M-00 → ... → M-08 ← docker-text 2章、fastapi-text 6章を前提
```

**冊をまたいで順番を入れ替えないでください。**
後の本は、前の本で作ったアプリを題材にします。
