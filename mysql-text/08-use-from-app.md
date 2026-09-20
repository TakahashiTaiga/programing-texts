---
title: "第8章 アプリから使う"
---

# 第8章 アプリから使う

第7章までで、**`mysql` に自分の手で打つ SQL** はひととおり書けるようになりました。
絞り込み、並べ替え、変更、設計、結合、集計、そして速さの確認。

しかし、実際のサービスで SQL を打つのは、人ではありません。**アプリケーション**です。

fastapi-text 第6章で、あなたはこういうコードを書きました。

```python
tasks = db.scalars(select(Task)).all()
```

このとき「SQL の中身は mysql-text で扱います」と書かれていました。
**いまなら、この1行が裏で何を送っているか読めます。**

この章では、その API の**接続先を SQLite から MySQL に切り替えます。**
そのうえで、**アプリからデータベースを使うときにだけ現れる4つの問題**を扱います。

| 覚えること | 何が困るのか |
|-----------|------------|
| **接続とユーザー** | アプリに root を使わせてしまう |
| **N+1 問題** | 1件のはずの取得が、何百本もの `SELECT` に化ける |
| **SQL インジェクション** | 入力欄に書かれた文字が、SQL の一部として実行される |
| **コネクションプール** | 接続そのものが重く、数にも上限がある |

最後に、**バックアップとリストア**（`mysqldump`）を扱います。
第4章 4.2.3 で「`WHERE` を忘れると全件更新される」と書きました。
**やってしまったあとに戻す方法**が、この章で手に入ります。

## この章で学ぶこと

- アプリ専用の**データベースとユーザー**を作って**権限を絞り**、
  FastAPI の接続先を **SQLite から MySQL に切り替え**られるようになる。
  そのうえで、SQLAlchemy が作ったテーブルを `SHOW CREATE TABLE` で読めるようになる
- **外部キーとリレーション**を定義し、`ForeignKey`（データベース側の制約）と
  `relationship`（Python 側の対応づけ）の違いを説明できるようになる
- **N+1 問題**を、発行された SQL の本数を**数えて**見つけ、`selectinload` で直せるようになる
- **SQL インジェクション**を自分の手元で再現し、**プレースホルダ**で防げるようになる
- **コネクションプール**の設定項目と MySQL 側の上限の関係を、測りながら決められるようになる
- **`mysqldump` でダンプを取り、事故のあとに元へ戻せる**ようになる

## この章の前提

- [第7章 インデックスと実行計画](./07-index-and-explain.md) を終えていること
- **fastapi-text 第6章まで**を終えていること（`fastapi-lesson` が手元にあり、`GET /tasks` が動く）
- MySQL が起動していて、`shop` に接続できること（2.1.2 / 2.2.1）

**この章では、2つの場所を行き来します。** 最初に確認しておいてください。

| 場所 | 何があるか | この章で何をするか |
|------|----------|------------------|
| `mysql-lesson` | Docker で動く MySQL（第2章 2.1.1） | データベースとユーザーを作る。`mysql` で中身を確かめる |
| `fastapi-lesson` | FastAPI のアプリ（fastapi-text 第2章） | 接続先を切り替える。Python のコードを書く |

**ターミナルは2つ開いておくと楽です。** 片方を `mysql-lesson`、もう片方を `fastapi-lesson` にしておきます。

MySQL が起動しているか確認します。

**Windows（PowerShell）**

```powershell
cd ~\Documents\mysql-lesson
docker compose up -d
docker compose ps
```

**macOS / Linux**

```bash
cd ~/Documents/mysql-lesson
docker compose up -d
docker compose ps
```

`STATUS` が `Up` になっていれば起動しています。

> **注意：docker-text 第6章の `fullstack-lesson` とは別に進めます**
> docker-text 第6章では、`fullstack-lesson` という別のプロジェクトを作り、
> **コンテナの中の API** から **コンテナの中の MySQL** に繋ぎました。
>
> この章では、**手元でそのまま動く `fastapi-lesson`** から、
> **`mysql-lesson` の MySQL** に繋ぎます。
> 確認するものを1つずつにするためです（コンテナを2つ相手にすると、
> エラーが出たときに「どちらの中の話か」が分からなくなります）。
>
> **2つの繋ぎ方の違いは 8.1.3 でまとめます。** 先に読んでも構いません。
> docker-text 第6章をまだ読んでいなくても、この章は読み進められます。

> **つまずいたら**
> この章でいちばん多いつまずきは、**接続できない**（8.1）です。
> エラーメッセージが4種類しかないので、8.1.2 の最後の表で切り分けられます。
>
> 次に多いのが、**Python 側と MySQL 側のどちらを見ているか分からなくなる**ことです。
> 迷ったら、必ず次の2つを打って現在地を確かめてください。
>
> - Python 側：`python -c "from app.config import settings; print(settings.database_url)"`
> - MySQL 側：`SELECT DATABASE(), USER();`
>
> AI に相談するときは、**打ったコマンド・エラーメッセージ全文・
> 上の2つの出力**を出してください（レベル C：環境の問題として扱ってよい範囲です）。
>
> ```text
> mysql-text の 8.1.2 を読んでいます。
> alembic upgrade head が失敗します。
> 打ったコマンド / エラー全文 / DATABASE_URL の値 / SELECT DATABASE(), USER(); の結果は次のとおりです。
> （4つを貼り付ける）
> 原因の切り分けの手順を教えてください。
> ```

---

## 8.1 FastAPI から MySQL に接続する

### 8.1.1 接続設定

アプリを繋ぐ前に、**アプリ用のデータベースとユーザー**を作ります。

第2章からずっと、練習では **root** で接続してきました。
root は**何でもできる管理者**です（2.4.1）。
自分ひとりで練習するぶんには楽ですが、**アプリに root を使わせてはいけません。**

理由は2つあります。

| 理由 | 具体的に何が起きるか |
|------|-------------------|
| **事故の範囲が広い** | アプリのバグ1つで、`shop` も `design` も `mysql` も消えうる |
| **漏れたときの被害が広い** | 接続情報が漏れたら、そのサーバーのすべてのデータが読める |

**アプリには、そのアプリが使うデータベースだけを触れるユーザーを割り当てます。**
これを**最小権限の原則**（必要な権限だけを与える、という考え方）と呼びます。

**データベースを作る**

タスク管理アプリのデータは、練習用の `shop` とは**別のデータベース**に入れます。
アプリの `DELETE` のバグで練習用データが消えては困ります。

`mysql` に root で接続してください（2.2.1）。

**Windows（PowerShell）**

```powershell
docker compose exec db mysql -u root -p --default-character-set=utf8mb4
```

**macOS / Linux**

```bash
docker compose exec db mysql -u root -p --default-character-set=utf8mb4
```

パスワード（`.env` の `MYSQL_ROOT_PASSWORD`。2.1.1 では `root_pass_1234`）を入れます。
**データベース名を付けずに接続している**ので、`SELECT DATABASE();` は `NULL` になります。

`taskapp` というデータベースを作ります。

```sql
CREATE DATABASE taskapp CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
```

実行結果:

```text
Query OK, 1 row affected (0.01 sec)
```

`CHARACTER SET` と `COLLATE` は、第2章 2.6.2 / 2.6.3 で扱ったものです。
**日本語と絵文字を扱うので `utf8mb4`**、照合順序は MySQL 8 の既定の `utf8mb4_0900_ai_ci` を明示しています。

できたことを確かめます。

```sql
SHOW CREATE DATABASE taskapp\G
```

実行結果:

```text
*************************** 1. row ***************************
       Database: taskapp
Create Database: CREATE DATABASE `taskapp` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */
```

**ユーザーを作る**

次に、`taskapp` だけを触れるユーザーを作ります。

```sql
CREATE USER 'task_user'@'%' IDENTIFIED BY 'task_pass_1234';
```

実行結果:

```text
Query OK, 0 rows affected (0.01 sec)
```

**`'ユーザー名'@'接続元'`** という形でユーザーを指定します。
MySQL のユーザーは、**名前だけでなく「どこから繋いでくるか」もセットで1人**です。

| 書き方 | 意味 |
|-------|------|
| `'task_user'@'localhost'` | **MySQL と同じマシンから**繋いでくる `task_user` |
| `'task_user'@'%'` | **どこから繋いできてもよい** `task_user`（`%` は「何でも」。3.3.1 の `LIKE` と同じ記号） |

この章では **`'%'`** を使います。
MySQL はコンテナの中で動いていて、アプリは**コンテナの外**から繋いでくるためです。
MySQL から見ると、それは「同じマシン」ではありません（詳しくは 8.1.3）。

> **注意：このパスワードも練習用です**
> `task_pass_1234` は読みやすさのための値です。
> 2.1.1 の注意と同じで、**手元のパソコンの中だけで動かす練習用だから**許される決め方です。

**権限を与える**

作ったばかりのユーザーは、**まだ何もできません。** 権限を与えます。

```sql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, DROP, INDEX, REFERENCES
    ON taskapp.* TO 'task_user'@'%';
```

実行結果:

```text
Query OK, 0 rows affected (0.01 sec)
```

**`GRANT 権限の一覧 ON 対象 TO ユーザー;`** という形です。
`ON taskapp.*` の `taskapp.*` が「**`taskapp` の中のすべてのテーブル**」を表します。

与えた9つの権限は、この章で実際に使うものだけです。

| 権限 | 何ができるようになるか | この章で使う場面 |
|------|-------------------|---------------|
| `SELECT` | 行を読む | `GET /tasks`（第3章） |
| `INSERT` | 行を追加する | `POST /tasks`（4.1） |
| `UPDATE` | 行を書き換える | `PATCH /tasks/{id}`（4.2） |
| `DELETE` | 行を削除する | `DELETE /tasks/{id}`（4.3） |
| `CREATE` | テーブルを作る | Alembic のマイグレーション（2.4.3） |
| `ALTER` | テーブルの形を変える | 同上（5.6.1） |
| `DROP` | テーブルを消す | 同上（`downgrade` のとき） |
| `INDEX` | インデックスを張る／外す | 同上（7.3.1） |
| `REFERENCES` | 外部キーを定義する | 8.2.1（5.4.2） |

与えたものを確かめます。

```sql
SHOW GRANTS FOR 'task_user'@'%';
```

実行結果:

```text
+-------------------------------------------------------------------------------------------------------------+
| Grants for task_user@%                                                                                      |
+-------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `task_user`@`%`                                                                       |
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, REFERENCES, INDEX, ALTER ON `taskapp`.* TO `task_user`@`%` |
+-------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

1行目の **`USAGE ON *.*`** は「**接続はできるが、何もできない**」という権限です。
`CREATE USER` したときに自動で付きます。
2行目が、いま与えたものです。**並び順は MySQL が決めた順に変わります**（打った順とは違います）。

**絞れていることを確かめる**

いったん `mysql` を抜けて（`exit`）、**`task_user` で接続し直します。**

**Windows（PowerShell）**

```powershell
docker compose exec db mysql -u task_user -p --default-character-set=utf8mb4
```

**macOS / Linux**

```bash
docker compose exec db mysql -u task_user -p --default-character-set=utf8mb4
```

パスワードは `task_pass_1234` です。見えるデータベースを確かめます。

```sql
SHOW DATABASES;
```

実行結果:

```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| taskapp            |
+--------------------+
3 rows in set (0.00 sec)
```

**`shop` も `design` も `mysql` も、一覧に出てきません。**
`SHOW DATABASES` は「**そのユーザーが権限を持つデータベースだけ**」を見せます
（`information_schema` と `performance_schema` は MySQL 自身の情報が入った特別なもので、誰でも見えます）。

触ろうとすると、はっきり断られます。

```sql
USE shop;
```

実行結果:

```text
ERROR 1044 (42000): Access denied for user 'task_user'@'%' to database 'shop'
```

**これが狙った状態です。** `exit` で抜けてください。

> **よくある間違い**
> **権限を絞りすぎて、マイグレーションが失敗する**間違いです。
>
> 「アプリは読み書きしかしないから」と考えて、次のように与えたとします。
>
> ```sql
> GRANT SELECT, INSERT, UPDATE, DELETE ON taskapp.* TO 'task_user'@'%';
> ```
>
> 8.1.2 で `alembic upgrade head` を打つと、こうなります。
>
> ```text
> pymysql.err.OperationalError: (1142, "CREATE command denied to user 'task_user'@'localhost' for table 'alembic_version'")
> ```
>
> **`1142` は「その操作の権限が無い」**というエラーです。
> **テーブルを作り変えるのはアプリではなく Alembic ですが、接続するユーザーは同じ**です。
> 上の9つを与え直してください。権限は、あとから `GRANT` を打ち足せます。

**接続 URL の形**

fastapi-text 6.2.2 で、SQLite の接続先をこう書きました。

```text
sqlite:///./app.db
```

MySQL では、次の形になります。**この1行が、この節のゴールです。**

```text
mysql+pymysql://task_user:task_pass_1234@localhost:3306/taskapp?charset=utf8mb4
```

部分ごとに読みます。

| 部分 | 意味 | どこで決めたか |
|------|------|-------------|
| `mysql+pymysql` | **MySQL に、PyMySQL というライブラリを使って**繋ぐ | 8.1.2 で入れます |
| `://` | 区切り | — |
| `task_user` | ユーザー名 | この項の `CREATE USER` |
| `:task_pass_1234` | パスワード | 同上 |
| `@localhost` | **接続先のホスト名。** 手元のパソコン自身 | 8.1.3 で詳しく |
| `:3306` | ポート番号 | 2.1.1 の `ports: "3306:3306"` の**左側** |
| `/taskapp` | データベース名 | この項の `CREATE DATABASE` |
| `?charset=utf8mb4` | **通信に使う文字コード** | 2.6.2。日本語のために必ず付ける |

**`?charset=utf8mb4` を忘れると、日本語が `????` になって保存されることがあります。**
第2章 2.6.1 で `--default-character-set=utf8mb4` を付けたのと、まったく同じ理由です。

### 8.1.2 SQLite から切り替える

ここからは **`fastapi-lesson` 側**の作業です。
もう1つのターミナルで、`fastapi-lesson` に移動し、仮想環境を有効化してください
（fastapi-text 2.2.1）。

**Windows（PowerShell）**

```powershell
cd ~\Desktop\fastapi-lesson
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
cd ~/Desktop/fastapi-lesson
source .venv/bin/activate
```

行の先頭に `(.venv)` が付いていることを確認します。

**① MySQL と通信するライブラリを入れる**

fastapi-text で使った **SQLAlchemy** は、「データベースへの言い方」を組み立てる道具でした。
**実際に MySQL と通信する部分は、別のライブラリ**が担当します。
SQLite のときは Python に最初から入っていたので、意識せずに済んでいました。

**Windows（PowerShell）**

```powershell
pip install PyMySQL==1.1.1 cryptography==44.0.0
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip install PyMySQL==1.1.1 cryptography==44.0.0
pip freeze > requirements.txt
```

```text
Successfully installed PyMySQL-1.1.1 cryptography-44.0.0
```

| 入れたもの | 何をするか |
|-----------|----------|
| **PyMySQL**（パイマイエスキューエル） | Python から MySQL と通信する部分を受け持つ |
| **cryptography**（クリプトグラフィー） | MySQL 8 のパスワード認証（暗号を使う方式）に必要 |

**`cryptography` は「おまじない」ではありません。** 入れないと、接続のときにこうなります。

```text
RuntimeError: 'cryptography' package is required for sha256_password or caching_sha2_password auth methods
```

MySQL 8 は、パスワードのやり取りに暗号を使う方式（`caching_sha2_password`）を既定にしています。
PyMySQL 単体はその計算ができないので、**計算を担当するライブラリを別に入れる**必要があります。

**② SQLite 専用の設定を外す**

`app/database.py` を開いてください。fastapi-text 6.2.3 で、こう書きました。

```python
engine = create_engine(
    settings.database_url,
    # SQLite のときだけ必要な設定
    connect_args={"check_same_thread": False},
)
```

`check_same_thread` は **SQLite にしか無い設定**です。MySQL に渡すと動きません。
ただし**消してしまうと SQLite に戻せなくなる**ので、
**接続 URL を見て、必要なときだけ渡す**形にします。

`fastapi-lesson/app/database.py`（`engine = create_engine(...)` の部分を置き換え）

```python
# SQLite のときだけ必要な設定（MySQL に渡すとエラーになる）
connect_args = {}
if settings.database_url.startswith("sqlite"):
    connect_args["check_same_thread"] = False

# データベースへの接続口。アプリ全体で1つだけ作る
engine = create_engine(
    settings.database_url,
    connect_args=connect_args,
    # 使い回した接続が切れていないか、使う前に確かめる
    pool_pre_ping=True,
)
```

`settings.database_url.startswith("sqlite")` は、
**接続 URL が `sqlite` で始まるかどうか**を調べています（python-text 2.4.3 の文字列メソッドです）。

`pool_pre_ping=True` の意味は 8.5.2 で扱います。
いまは「**使い回す接続が生きているか、使う直前に確かめる指定**」と読んでおいてください。

> **docker-text 第6章を済ませている場合**
> この書き換えは、docker-text 6.3.2 でやったものと同じです。
> `fullstack-lesson/api/app/database.py` では済んでいますが、
> **もとの `fastapi-lesson/app/database.py` はまだ古いまま**のことがあります。
> 開いて確認し、`connect_args={"check_same_thread": False}` が直接書かれていたら、
> 上の形に置き換えてください。

**③ 接続先を書き換える**

`.env` の `DATABASE_URL` を、8.1.1 の形に書き換えます。
**古い行は消さずに、`#` を付けてコメントにしておきます。** あとで SQLite に戻せるようにするためです。

`fastapi-lesson/.env`（`DATABASE_URL` の行を置き換え）

```text
# DATABASE_URL=sqlite:///./app.db
DATABASE_URL=mysql+pymysql://task_user:task_pass_1234@localhost:3306/taskapp?charset=utf8mb4
```

`.env.example` は、**値を伏せて**同じ項目を書きます（fastapi-text 4.6.3）。

`fastapi-lesson/.env.example`（`DATABASE_URL` の行を置き換え）

```text
DATABASE_URL=mysql+pymysql://ユーザー名:パスワード@localhost:3306/taskapp?charset=utf8mb4
```

**`.env` にはパスワードが入りました。** `.gitignore` に `.env` が入っているか、必ず確認してください。

読み込めているかを確かめます。

**Windows（PowerShell）**

```powershell
python -c "from app.config import settings; print(settings.database_url)"
```

**macOS / Linux**

```bash
python -c "from app.config import settings; print(settings.database_url)"
```

```text
mysql+pymysql://task_user:task_pass_1234@localhost:3306/taskapp?charset=utf8mb4
```

**`sqlite:///./app.db` と出た場合は、`.env` が読めていません。**
`fastapi-lesson` の直下で実行しているか確認してください（fastapi-text 5.1.2）。

**④ テーブルを作る**

`taskapp` は、いま**空っぽ**です。テーブルを作ります。
使うのは fastapi-text 6.6 で入れた **Alembic** です。

**Windows（PowerShell）**

```powershell
alembic upgrade head
```

**macOS / Linux**

```bash
alembic upgrade head
```

実行結果:

```text
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 80a127079a0b, create tasks table
```

**1行目に注目してください。** fastapi-text では `Context impl SQLiteImpl.` と出ていました。
いまは **`MySQLImpl`** です。Alembic は接続 URL を見て、相手に合わせた SQL を組み立てています。

2行目の `Will assume non-transactional DDL.` は、
**「テーブルを作る系の命令は、ロールバックできないものとして扱う」**という宣言です。
第4章 4.4.2 で `ROLLBACK` を扱いましたが、**MySQL では `CREATE TABLE` を打ち消せません。**
`BEGIN` の中に書いても、そこで自動的にコミットされます。

`migrations/versions/` に置いてあるファイルが**古いものから順に**適用されます。
fastapi-text 第6章・第7章の演習を進めた人は、行が何本も出ます。

> **よくある間違い**
> **`alembic upgrade head` を打たずに、いきなりサーバーを起動する**間違いです。
> 接続はできるので起動はしますが、最初のリクエストでこうなります。
>
> ```text
> pymysql.err.ProgrammingError: (1146, "Table 'taskapp.tasks' doesn't exist")
> ```
>
> **`1146` は「そのテーブルが無い」**です。
> SQLite のときは `no such table: tasks` と出ていました（fastapi-text 6.6.1）。
> **メッセージの文面は違いますが、原因は同じ**です。

**⑤ できたテーブルを SQL の目で見る**

ここが、この本を読んできた意味がいちばん出るところです。
**Python で書いたモデルが、どんな `CREATE TABLE` になったのか**を確かめます。

`mysql-lesson` 側のターミナルで、`taskapp` に接続します。

**Windows（PowerShell）**

```powershell
docker compose exec db mysql -u root -p --default-character-set=utf8mb4 taskapp
```

**macOS / Linux**

```bash
docker compose exec db mysql -u root -p --default-character-set=utf8mb4 taskapp
```

```sql
SHOW TABLES;
```

実行結果:

```text
+-------------------+
| Tables_in_taskapp |
+-------------------+
| alembic_version   |
| tasks             |
+-------------------+
2 rows in set (0.00 sec)
```

**`alembic_version`** は Alembic が作った管理用のテーブルで、
「**どの変更まで適用したか**」を1行だけ持っています。中を見てみてください。

```sql
SELECT * FROM alembic_version;
```

実行結果:

```text
+--------------+
| version_num  |
+--------------+
| 80a127079a0b |
+--------------+
1 row in set (0.00 sec)
```

この値が、`migrations/versions/` のファイル名の先頭 12 文字と一致しています。
**`upgrade head` が「ここまで適用済み」と書き込んだ印**です。

本題の `tasks` を見ます。

```sql
SHOW CREATE TABLE tasks\G
```

実行結果:

```text
*************************** 1. row ***************************
       Table: tasks
Create Table: CREATE TABLE `tasks` (
  `id` int NOT NULL AUTO_INCREMENT,
  `title` varchar(20) NOT NULL,
  `done` tinyint(1) NOT NULL,
  `code` varchar(10) DEFAULT NULL,
  `priority` int NOT NULL,
  `tags` json NOT NULL,
  `owner_name` varchar(20) NOT NULL,
  `owner_email` varchar(100) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `code` (`code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

**この章でいちばん読み返してほしい出力です。**
第5章で自分の手で書いた `CREATE TABLE` と、同じものが出ています。

`app/models.py` の書き方と、並べてみます。

| `app/models.py` の書き方 | できた列 | 対応する本文 |
|----------------------|---------|-----------|
| `Mapped[int] = mapped_column(primary_key=True)` | `int NOT NULL AUTO_INCREMENT` + `PRIMARY KEY` | 5.3.1 / 5.3.2 |
| `Mapped[str] = mapped_column(String(20))` | `varchar(20) NOT NULL` | 5.1.2 / 5.2.1 |
| `Mapped[bool] = mapped_column(default=False)` | **`tinyint(1) NOT NULL`** | 5.1.4 |
| `Mapped[str \| None] = mapped_column(String(10), unique=True)` | `varchar(10) DEFAULT NULL` + `UNIQUE KEY` | 5.2.1 / 5.2.3 |
| `Mapped[list[str]] = mapped_column(JSON)` | `json NOT NULL` | — |

**`\| None` の有無が、そのまま `NOT NULL` の有無**になっています。
fastapi-text 6.3.2 で「`| None` が無い列は空にできない」と書いてあったのは、
**`NOT NULL` 制約が付く**ということでした（5.2.1）。

**`bool` が `tinyint(1)` になっている**ことにも注目してください。
第5章 5.1.4 で「MySQL に真偽値型は無く、`TINYINT(1)` で代用する」と書いたとおりです。

末尾の `ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci` は、
`CREATE DATABASE` で指定した文字コードが**テーブルに引き継がれた**ものです（2.6.2）。

> **注意：`tinyint(1)` は 0 と 1 に限りません**
> `TINYINT` は「-128 〜 127 の整数」です。`(1)` は表示の幅で、**入る値を制限しません。**
> 試しに、こう打ってみてください。
>
> ```sql
> UPDATE tasks SET done = 2 WHERE id = 1;
> ```
>
> **エラーになりません。** そして API から読むと、`done` は `true` として返ってきます。
> Python 側が「0 以外は真」として扱うためです。
>
> 戻しておいてください。
>
> ```sql
> UPDATE tasks SET done = 0 WHERE id = 1;
> ```
>
> 値を 0 と 1 に限りたい場合は、第5章 5.2.4 の `CHECK` 制約を足します。

**⑥ データを入れて動かす**

`fastapi-lesson` 側に戻ります。練習用データを入れます。

**Windows（PowerShell）**

```powershell
python -m app.seed
```

**macOS / Linux**

```bash
python -m app.seed
```

```text
3 件のタスクを登録しました。
```

**`app/seed.py` のコードは、1行も変えていません。**
fastapi-text 6.2.2 で「乗り換えは接続 URL の1行」と書いてあったのは、このことです。

MySQL 側で見てみます。

```sql
SELECT id, title, done, tags, owner_name FROM tasks;
```

実行結果:

```text
+----+--------------------+------+------+------------+
| id | title              | done | tags | owner_name |
+----+--------------------+------+------+------------+
|  1 | 牛乳を買う         |    0 | []   | 山田       |
|  2 | レポートを書く     |    1 | []   | 鈴木       |
|  3 | 部屋を片づける     |    0 | []   | 山田       |
+----+--------------------+------+------+------------+
3 rows in set (0.00 sec)
```

**`done` が `false` / `true` ではなく `0` / `1` で入っています。**
`tags` は空のリストが `[]` という JSON の文字列として入っています。

サーバーを起動します。

**Windows（PowerShell）**

```powershell
fastapi dev app/main.py
```

**macOS / Linux**

```bash
fastapi dev app/main.py
```

ブラウザで `http://127.0.0.1:8000/docs` を開き、
`GET /tasks` を実行してください。**3件返ってくれば成功です。**

`POST /tasks` で1件登録してから、MySQL 側で数え直してみてください。

```sql
SELECT COUNT(*) FROM tasks;
```

**`4` になっています。** これで、
「アプリが書いたものを SQL で読む／SQL で書いたものをアプリが読む」が繋がりました。

**確認できたら、いま足した1件を消して3件に戻してください。**
`/docs` の `DELETE /tasks/{task_id}` に、足した `id`（`4` のはずです）を指定して実行します。

```sql
SELECT COUNT(*) FROM tasks;
```

```text
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
```

**以降の節と演習は、`tasks` が3件の状態を前提に数字を書いています。**
件数が違っても進められますが、本文の本数・件数とはずれます。

**⑦ 接続できないときの切り分け**

エラーメッセージで原因が分かります。**メッセージの番号を見てください。**

| エラー | 番号 | 原因 | 対処 |
|-------|------|------|------|
| `Can't connect to MySQL server on 'localhost' ([Errno 111] Connection refused)` | 2003 | **MySQL が動いていない**、またはポートが違う | `docker compose up -d` と `docker compose ps` |
| `Access denied for user 'task_user'@'localhost' (using password: YES)` | 1045 | **ユーザー名かパスワードが違う** | `.env` の値と `CREATE USER` を見比べる |
| `Access denied for user 'task_user'@'%' to database 'nosuch'` | 1044 | **そのデータベースを触る権限が無い**（名前の打ち間違いを含む） | `SHOW GRANTS` と `SHOW DATABASES` |
| `Table 'taskapp.tasks' doesn't exist` | 1146 | **テーブルがまだ無い** | `alembic upgrade head` |
| `CREATE command denied to user ... for table 'alembic_version'` | 1142 | **権限が足りない** | 8.1.1 の `GRANT` を打ち直す |
| `TypeError: Connection.__init__() got an unexpected keyword argument 'check_same_thread'` | — | **②の書き換えをしていない** | `app/database.py` を 8.1.2 ② の形に |

**1045 と 1044 は、どちらも `Access denied` ですが別のもの**です。
番号か、**`to database` という語があるか**で見分けてください。

- **1045**：門の前で追い返された（**あなたが誰か分からない**）
- **1044**：門は通れたが、その部屋の鍵が無い（**あなたは分かるが権限が無い**）

最後の `TypeError` は、**エンジンを作った時点では起きません。**
**最初に実際に接続しようとしたとき**に出ます。
そのため「起動は成功したのに、最初のリクエストで 500」という形で現れます。

> **SQLite に戻したいとき**
> `.env` の2行の `#` を入れ替えるだけです。
>
> ```text
> DATABASE_URL=sqlite:///./app.db
> # DATABASE_URL=mysql+pymysql://task_user:task_pass_1234@localhost:3306/taskapp?charset=utf8mb4
> ```
>
> `app.db` は消していないので、**SQLite 側のデータもそのまま残っています。**
> ただし、この章の 8.2 以降で足すテーブルは MySQL 側にしか無い状態になります。
> 戻したあとに `alembic upgrade head` を打てば、SQLite 側にも同じ形が作られます。

### 8.1.3 Docker Compose で繋ぐ

ここまでは、**手元でそのまま動く Python** から、
**コンテナの中の MySQL** に繋ぎました。接続先は `localhost:3306` でした。

docker-text 第6章では、**コンテナの中の API** から繋ぎました。
そのときの接続先は **`db:3306`** でした。**なぜ違うのかを、ここで整理します。**

```mermaid
flowchart TB
    subgraph PC["あなたのパソコン"]
        P["Python<br/>fastapi dev"]
        subgraph NET["Docker のネットワーク"]
            A["api コンテナ"]
            D["db コンテナ<br/>（MySQL）"]
        end
    end
    P -->|"① localhost:3306<br/>（公開ポート経由）"| D
    A -->|"② db:3306<br/>（サービス名で直接）"| D
```

| 繋ぐ人 | ホスト名 | 必要なもの | どこで扱ったか |
|-------|---------|----------|-------------|
| ① 手元の Python | **`localhost`** | `compose.yaml` の **`ports`** の公開 | この章 8.1.2 |
| ② `api` コンテナ | **サービス名の `db`** | 同じ Compose のネットワークにいること | docker-text 6.3.2 |
| ③ コンテナの中の `mysql` コマンド | `localhost`（コンテナ自身） | `docker compose exec` で中に入る | 2.2.1 |

**①が動くのは、`ports: "3306:3306"` を書いているからです**（2.1.1）。
これは「**パソコンの 3306 番に来たものを、コンテナの 3306 番に流す**」という設定でした
（docker-text 4.4.1）。この1行が無いと、手元の Python からは繋げません。

**②では `ports` は要りません。** 同じ Compose のネットワークにいるコンテナ同士は、
**サービス名で直接呼び合えます**（docker-text 5.3.1）。
docker-text 第6章の `compose.yaml` で `db` に `ports` を書かなかったのは、これが理由です。

**よくある取り違え**を2つ挙げます。

| やってしまうこと | 出るエラー | なぜ |
|---------------|----------|------|
| コンテナの中の API から `localhost:3306` に繋ぐ | `2003 Connection refused` | コンテナにとっての `localhost` は**自分自身**。MySQL は別のコンテナ |
| 手元の Python から `db:3306` に繋ぐ | `2005 Unknown MySQL server host 'db'` | `db` という名前は **Docker のネットワークの中でしか通じない** |

8.1.1 で `'task_user'@'%'`（どこからでも）にしたのは、①のためです。
`'task_user'@'localhost'` にすると、**①では繋げません。**
MySQL から見ると、公開ポート経由で来た接続は「同じマシンから」には見えないためです。

> **補足：docker-text 第6章の `MYSQL_USER` との違い**
> `compose.yaml` の `MYSQL_USER` / `MYSQL_PASSWORD` を使うと、
> MySQL の公式イメージが**初回起動時に**ユーザーを作ってくれます（2.1.1）。
> そのユーザーは `MYSQL_DATABASE` に対して権限を持ちます。
>
> 手軽ですが、**すでにデータが入っているボリュームには効きません。**
> 「`.env` を書き換えたのにユーザーが増えない」と悩むのは、これが原因です。
> **あとから足すときは、この章のように `CREATE USER` と `GRANT` を自分で打ちます。**

---

## 8.2 リレーションを扱う

### 8.2.1 SQLAlchemy でのリレーション定義

タスクに**コメント**を付けられるようにします。
1つのタスクに、コメントは何件でも付きます。**1対多**です（1.2.2 / 5.4.1）。

まず、**なぜテーブルを増やすのか**を確かめます。
`tasks` には `tags` という **JSON の列**があり、リストがそのまま入っていました。
コメントも同じように JSON の列に入れれば、テーブルは増えません。

**それでも分けるのは、次の3つが必要になるからです。**

| やりたいこと | JSON の列だと | 別テーブルなら |
|------------|-------------|-------------|
| 「『図』を含むコメントを探す」 | 列の中の文字を探すことになり、**インデックスが効かない**（7.5.2） | `WHERE body LIKE '図%'` に索引が効く |
| 「コメント数の多いタスク順」 | 数えるために全部読む | `COUNT` と `GROUP BY`（6.5.2） |
| 「投稿日時を1件ごとに持つ」 | 自分で形を決めて守る必要がある | `DATETIME` 型と `DEFAULT`（5.1.3 / 5.2.2） |

**「行が増えていくもの」「検索・集計したいもの」は、列ではなく行にする**——
これが第5章 5.5 の判断です。

**モデルを書く**

`fastapi-lesson/app/models.py` を開いてください。まず `import` を足します。

`fastapi-lesson/app/models.py`（先頭）

```diff
+ from datetime import datetime
+
- from sqlalchemy import JSON, String
+ from sqlalchemy import JSON, ForeignKey, String, func
- from sqlalchemy.orm import Mapped, mapped_column
+ from sqlalchemy.orm import Mapped, mapped_column, relationship
```

`Task` クラスに、**コメントの一覧**を足します。

`fastapi-lesson/app/models.py`（`Task` の中、`owner_email` の下に追記）

```python
    comments: Mapped[list["Comment"]] = relationship(
        back_populates="task",
        cascade="all, delete-orphan",
        order_by="Comment.id",
    )
```

ファイルの末尾に、`Comment` クラスを足します。

`fastapi-lesson/app/models.py`（末尾に追記）

```python
class Comment(Base):
    """comments テーブルの1行。1つのタスクに何件でも付く。"""

    __tablename__ = "comments"

    id: Mapped[int] = mapped_column(primary_key=True)
    task_id: Mapped[int] = mapped_column(ForeignKey("tasks.id", ondelete="CASCADE"))
    body: Mapped[str] = mapped_column(String(200))
    created_at: Mapped[datetime] = mapped_column(server_default=func.now())

    task: Mapped["Task"] = relationship(back_populates="comments")
```

**書いたものを1つずつ**読みます。

| 書いたもの | 意味 | 対応する本文 |
|-----------|------|-----------|
| `ForeignKey("tasks.id", ...)` | **`tasks` の `id` を指す外部キー**。テーブル名.列名で書く | 5.4.2 |
| `ondelete="CASCADE"` | **親のタスクが消えたら、コメントも一緒に消す** | 5.4.4 |
| `server_default=func.now()` | **既定値をデータベース側に持たせる**（`DEFAULT CURRENT_TIMESTAMP`） | 5.2.2 |
| `relationship(...)` | **Python 側の対応づけ。** `task.comments` / `comment.task` と書けるようにする | — |
| `back_populates=` | 両方向の `relationship` を**同じ関係として結びつける** | — |
| `cascade="all, delete-orphan"` | **Python 側**でタスクを削除したときに、コメントも削除の対象にする | — |
| `order_by="Comment.id"` | `task.comments` を**取り出すときの並び順**（`ORDER BY comments.id`） | 3.4.1 |

**`ForeignKey` と `relationship` は、別のものです。** ここが混ざりやすいところです。

| | `ForeignKey` | `relationship` |
|---|-------------|----------------|
| 誰のためのもの | **MySQL** | **Python** |
| できること | 存在しない相手を指す値を**拒否する**（5.4.3） | `task.comments` と書いて**取り出せる** |
| 消えると何が起きるか | 不整合なデータが入る | コードが `AttributeError` になる |
| `SHOW CREATE TABLE` に出るか | **出る**（`CONSTRAINT` の行） | **出ない** |

**`ForeignKey` を書いただけでは `task.comments` と書けません。**
**`relationship` を書いただけでは、データベース側は何も守ってくれません。**
**両方書きます。**

**マイグレーションを作る**

Alembic に `Comment` を教えます。`migrations/env.py` の `import` を直します。

`fastapi-lesson/migrations/env.py`（`from app.models import ...` の行）

```diff
- from app.models import Task  # noqa: F401  Base に Task を登録するための import
+ from app.models import Comment, Task  # noqa: F401  Base にモデルを登録するための import
```

**この1行を忘れると、`Comment` の追加が検出されません**（fastapi-text 6.3.1 と同じ理由です）。

差分を書き出します。

**Windows（PowerShell）**

```powershell
alembic revision --autogenerate -m "create comments table"
```

**macOS / Linux**

```bash
alembic revision --autogenerate -m "create comments table"
```

実行結果:

```text
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.autogenerate.compare] Detected added table 'comments'
Generating .../migrations/versions/0de1c1b6364b_create_comments_table.py ...  done
```

**できたファイルを必ず開いて読んでください**（fastapi-text 6.6.3 の約束です）。
先頭 12 文字は毎回変わります。

`migrations/versions/0de1c1b6364b_create_comments_table.py`（`upgrade` の部分）

```python
def upgrade() -> None:
    op.create_table('comments',
    sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('task_id', sa.Integer(), nullable=False),
    sa.Column('body', sa.String(length=200), nullable=False),
    sa.Column('created_at', sa.DateTime(), server_default=sa.text('now()'), nullable=False),
    sa.ForeignKeyConstraint(['task_id'], ['tasks.id'], ondelete='CASCADE'),
    sa.PrimaryKeyConstraint('id')
    )
```

**`sa.ForeignKeyConstraint` の行があること**を確認してください。
`relationship` の指定（`back_populates` や `order_by`）は、**ここには出てきません。**
Python 側の話なので、テーブルの形には関係しないからです。

適用します。

**Windows（PowerShell）**

```powershell
alembic upgrade head
```

**macOS / Linux**

```bash
alembic upgrade head
```

```text
INFO  [alembic.runtime.migration] Running upgrade 80a127079a0b -> 0de1c1b6364b, create comments table
```

**MySQL 側で確かめる**

```sql
SHOW CREATE TABLE comments\G
```

実行結果:

```text
*************************** 1. row ***************************
       Table: comments
Create Table: CREATE TABLE `comments` (
  `id` int NOT NULL AUTO_INCREMENT,
  `task_id` int NOT NULL,
  `body` varchar(200) NOT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `task_id` (`task_id`),
  CONSTRAINT `comments_ibfk_1` FOREIGN KEY (`task_id`) REFERENCES `tasks` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

3つ、読み取ってください。

1. **`CONSTRAINT ... FOREIGN KEY ... ON DELETE CASCADE`** が付いている（5.4.2 / 5.4.4）。
   名前は `comments_ibfk_1` のように MySQL が自動で付けます
2. **`KEY task_id (task_id)` が勝手に増えている。**
   MySQL は**外部キーの列に、インデックスを自動で作ります**（第7章 7.3.1）。
   制約を守るために、毎回「その `id` が `tasks` にあるか」を引く必要があるためです
3. `created_at` が **`DEFAULT CURRENT_TIMESTAMP`** になっている（`server_default=func.now()` の結果）

守られていることを確かめます。**存在しないタスクへのコメント**を入れてみてください。

```sql
INSERT INTO comments (task_id, body) VALUES (999, 'ないタスクへのコメント');
```

実行結果:

```text
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`taskapp`.`comments`, CONSTRAINT `comments_ibfk_1` FOREIGN KEY (`task_id`) REFERENCES `tasks` (`id`) ON DELETE CASCADE)
```

**第5章 5.4.3 の参照整合性が、アプリのテーブルでも効いています。**
アプリのバグで存在しない `task_id` を書こうとしても、**データベースが止めてくれます。**

> **よくある間違い**
> **`relationship` は書いたのに `ForeignKey` を書き忘れる**間違いです。
> `Mapped[int]` だけにして `ForeignKey(...)` を付けないと、
> `alembic revision --autogenerate` のときにこうなります。
>
> ```text
> sqlalchemy.exc.NoForeignKeysError: Could not determine join condition between parent/child tables
> ```
>
> **「どの列で繋ぐのか分からない」**というエラーです。
> `relationship` は繋ぎ方を `ForeignKey` から読み取っているので、片方だけでは成立しません。

### 8.2.2 関連データを取得する

コメントを**登録する窓口**と、**タスクと一緒に返す形**を作ります。

**受け取る形と返す形**

`fastapi-lesson/app/schemas.py`（末尾に追記）

```python
class CommentCreate(BaseModel):
    """コメントを登録するときに受け取る形。"""

    body: str = Field(min_length=1, max_length=200, description="コメントの本文")


class CommentRead(BaseModel):
    """コメントを返すときの形。"""

    model_config = ConfigDict(from_attributes=True)

    id: int
    body: str
    created_at: datetime
```

`import` を足します。

`fastapi-lesson/app/schemas.py`（先頭）

```diff
+ from datetime import datetime
+
  from pydantic import BaseModel, ConfigDict, Field, field_validator
```

**`max_length=200` は、`String(200)`（8.2.1）と数を揃えています。**
fastapi-text 6.3.2 の「**両方に書いて二重に守る**」です。

`TaskRead` に、コメントの一覧を足します。

`fastapi-lesson/app/schemas.py`（`TaskRead` の中）

```diff
  class TaskRead(TaskBase):
      """返すときの形。email を持たない。"""
  
      model_config = ConfigDict(from_attributes=True)
  
      id: int
      owner: OwnerRead
+     comments: list[CommentRead] = []
```

**`= []` を付けているので、既定は空のリスト**です。
コメントが1件も無いタスクでも `"comments": []` が返り、形が崩れません。

**窓口を足す**

`fastapi-lesson/app/routers/tasks.py`（末尾に追記）

```python
@router.post("/{task_id}/comments", response_model=CommentRead, status_code=201)
def create_comment(
    new_comment: CommentCreate,
    task: Task = Depends(get_task_or_404),
    db: Session = Depends(get_db),
):
    comment = Comment(task_id=task.id, body=new_comment.body)
    db.add(comment)
    db.commit()
    db.refresh(comment)
    return comment
```

`import` を足します。

`fastapi-lesson/app/routers/tasks.py`（先頭）

```diff
- from app.models import Task
+ from app.models import Comment, Task
- from app.schemas import TaskCreate, TaskListResponse, TaskRead, TaskUpdate
+ from app.schemas import (
+     CommentCreate,
+     CommentRead,
+     TaskCreate,
+     TaskListResponse,
+     TaskRead,
+     TaskUpdate,
+ )
```

`add` → `commit` → `refresh` の3手順は、fastapi-text 6.4.1 と同じです。
**`refresh` が要るのは `created_at` のため**です。
この値を決めているのは**データベース側**（`DEFAULT CURRENT_TIMESTAMP`）なので、
読み直さないと Python 側は値を知りません。

**`get_task_or_404` を使っている**ので、存在しないタスクへの `POST` は `404` になります。
**8.2.1 で確かめた `1452` のエラーは、アプリからは起きません。**
外部キー制約は「アプリが正しく書けているかの最後の砦」であって、
**日常のエラー処理はアプリ側で行います。**

**動かして確かめる**

サーバーを起動して（`fastapi dev app/main.py`）、`http://127.0.0.1:8000/docs` を開きます。

`POST /tasks/{task_id}/comments` に、`task_id` は `1`、ボディはこうして実行してください。

```json
{"body": "低脂肪のものにする"}
```

レスポンス（`201`）:

```json
{
  "id": 1,
  "body": "低脂肪のものにする",
  "created_at": "2026-09-17T05:13:32"
}
```

同じ手順で、あと5件入れておきます。**8.3 で使います。**

| `task_id` | `body` |
|-----------|--------|
| 1 | `特売日は火曜` |
| 2 | `図を1枚入れる` |
| 3 | `本棚から始める` |
| 3 | `不要な本は売る` |
| 3 | `金曜までに終わらせる` |

MySQL 側で確かめます。

```sql
SELECT id, task_id, body, created_at FROM comments;
```

実行結果:

```text
+----+---------+--------------------------------+---------------------+
| id | task_id | body                           | created_at          |
+----+---------+--------------------------------+---------------------+
|  1 |       1 | 低脂肪のものにする             | 2026-09-17 05:13:32 |
|  2 |       1 | 特売日は火曜                   | 2026-09-17 05:13:32 |
|  3 |       2 | 図を1枚入れる                  | 2026-09-17 05:13:32 |
|  4 |       3 | 本棚から始める                 | 2026-09-17 05:13:32 |
|  5 |       3 | 不要な本は売る                 | 2026-09-17 05:13:32 |
|  6 |       3 | 金曜までに終わらせる           | 2026-09-17 05:13:32 |
+----+---------+--------------------------------+---------------------+
6 rows in set (0.00 sec)
```

`GET /tasks/1` を実行すると、コメントが付いて返ってきます。

```json
{
  "title": "牛乳を買う",
  "done": false,
  "code": null,
  "priority": 3,
  "tags": [],
  "id": 1,
  "owner": {"name": "山田"},
  "comments": [
    {"id": 1, "body": "低脂肪のものにする", "created_at": "2026-09-17T05:13:32"},
    {"id": 2, "body": "特売日は火曜", "created_at": "2026-09-17T05:13:32"}
  ]
}
```

**`GET /tasks/1` の窓口のコードは、1行も変えていません。**
`db.get(Task, task_id)` で取ってきた `task` を返しているだけです（fastapi-text 6.4.2）。

では、`comments` はいつ取ってきたのでしょうか。

**答えは「`TaskRead` が `task.comments` を読んだ瞬間」です。**
`relationship` は、**触られるまで取りに行きません。**
この振る舞いを**遅延読み込み**（レイジーロード。必要になった時点で読み込むこと）と呼びます。

```mermaid
sequenceDiagram
    participant R as 窓口の関数
    participant S as セッション
    participant M as MySQL
    R->>S: db.get(Task, 1)
    S->>M: SELECT ... FROM tasks WHERE id = 1
    M-->>S: 1行
    Note over R: return task（ここまでで SQL は1本）
    R->>S: task.comments を読む（TaskRead が読む）
    S->>M: SELECT ... FROM comments WHERE 1 = task_id
    M-->>S: 2行
```

**便利な仕組みですが、これが 8.3 の問題を生みます。**

---

## 8.3 N+1 問題

### 8.3.1 何が起きているのか

`GET /tasks`（一覧）を実行してみてください。3件とも、コメントが付いて返ってきます。

`TaskListResponse` の中身は `list[TaskRead]` で、`TaskRead` は `comments` を持ちます。
つまり、**一覧を返すとき、タスク1件ごとに `task.comments` が読まれます。**

8.2.2 の最後で確かめたとおり、`task.comments` を読むと `SELECT` が1本飛びます。
ということは——

```mermaid
flowchart TB
    Q1["① SELECT * FROM tasks<br/>（一覧を取る。1本）"]
    Q1 --> Q2["② SELECT * FROM comments WHERE 1 = task_id"]
    Q1 --> Q3["③ SELECT * FROM comments WHERE 2 = task_id"]
    Q1 --> Q4["④ SELECT * FROM comments WHERE 3 = task_id"]
```

**タスク3件で、SQL は4本**です。

これを **N+1 問題**（エヌプラスワンもんだい）と呼びます。
**一覧を取る1本と、件数ぶんの N 本**を足して「N+1」です。

| タスクの件数 | 発行される SQL の本数 |
|------------|------------------|
| 3 件 | **4 本** |
| 20 件 | **21 本** |
| 100 件 | **101 本** |
| 1000 件 | **1001 本** |

**1本あたりは速い**ことに注意してください。
`comments` の `task_id` にはインデックスが付いています（8.2.1 の `KEY task_id`）。
`EXPLAIN` で見れば `type: ref` / `rows: 2` の、何も問題の無い SQL です（7.4.2）。

**遅くなるのは、1本1本の往復**です。
アプリと MySQL は別のプロセス（docker-text 第6章ではコンテナも別）なので、
1本ごとに「送る・待つ・受け取る」が発生します。
これが 1001 回積み上がると、**1本が 1 ミリ秒でも 1 秒**になります。

**第7章で扱った「遅い」とは、種類が違う遅さ**です。

| | 第7章の遅さ | この節の遅さ |
|---|-----------|------------|
| 原因 | **1本の SQL が重い**（全件走査） | **軽い SQL が何百本も飛ぶ** |
| 見つけ方 | `EXPLAIN` の `rows` / スロークエリログ | **本数を数える**（8.3.2） |
| 直し方 | インデックス・書き換え | **まとめて取る**（8.3.3） |

**スロークエリログでは見つかりません。** 1本1本は速いからです（7.4.3）。
**本数を数える道具**が必要です。

### 8.3.2 発行された SQL を見る

数え方を2つ用意します。**Python 側**と **MySQL 側**の両方から見ます。

**① Python 側：SQLAlchemy に SQL を印字させる**

`fastapi-lesson` の直下に、確認用のスクリプトを作ります。

`fastapi-lesson/check_n_plus_1.py`（ファイル全体）

```python
"""発行された SQL を印字して数える（mysql-text 8.3.2）。"""

import logging

from sqlalchemy import select
from sqlalchemy.orm import selectinload

from app.database import SessionLocal
from app.models import Task

# SQLAlchemy が組み立てた SQL を画面に出す設定
logging.basicConfig()
logging.getLogger("sqlalchemy.engine").setLevel(logging.INFO)


def show(label: str, statement) -> None:
    """渡された select を実行して、タスクごとのコメント件数を出す。"""
    print(f"\n===== {label} =====")
    db = SessionLocal()
    try:
        for task in db.scalars(statement):
            print(f"  {task.title}: コメント {len(task.comments)} 件")
    finally:
        db.close()


show("A いつもの書き方", select(Task))
show("B selectinload を付けた書き方", select(Task).options(selectinload(Task.comments)))
```

`logging.getLogger("sqlalchemy.engine").setLevel(logging.INFO)` の2行が、
**SQLAlchemy に「組み立てた SQL を印字せよ」と指示している**部分です。
`create_engine(..., echo=True)` と同じ効果ですが、
**`app/database.py` を書き換えずに済む**ので、確認用のスクリプトに向いています。

実行します。**サーバーは止めなくて構いません。**

**Windows（PowerShell）**

```powershell
python check_n_plus_1.py
```

**macOS / Linux**

```bash
python check_n_plus_1.py
```

実行結果（A の部分。**接続直後の `SELECT DATABASE()` などは省いています**）:

```text
===== A いつもの書き方 =====
INFO:sqlalchemy.engine.Engine:BEGIN (implicit)
INFO:sqlalchemy.engine.Engine:SELECT tasks.id, tasks.title, tasks.done, tasks.code, tasks.priority, tasks.tags, tasks.owner_name, tasks.owner_email
FROM tasks
INFO:sqlalchemy.engine.Engine:[generated in 0.00015s] {}
INFO:sqlalchemy.engine.Engine:SELECT comments.id AS comments_id, comments.task_id AS comments_task_id, comments.body AS comments_body, comments.created_at AS comments_created_at
FROM comments
WHERE %(param_1)s = comments.task_id ORDER BY comments.id
INFO:sqlalchemy.engine.Engine:[generated in 0.00011s] {'param_1': 1}
  牛乳を買う: コメント 2 件
INFO:sqlalchemy.engine.Engine:SELECT comments.id AS comments_id, ...
INFO:sqlalchemy.engine.Engine:[cached since 0.001257s ago] {'param_1': 2}
  レポートを書く: コメント 1 件
INFO:sqlalchemy.engine.Engine:SELECT comments.id AS comments_id, ...
INFO:sqlalchemy.engine.Engine:[cached since 0.002055s ago] {'param_1': 3}
  部屋を片づける: コメント 3 件
INFO:sqlalchemy.engine.Engine:ROLLBACK
```

**`SELECT ... FROM comments` が3回出ています。** `tasks` の1回と合わせて4本です。

読み方のポイントを3つ挙げます。

| 出力 | 意味 |
|------|------|
| `BEGIN (implicit)` | **トランザクションが自動で始まった**（4.4.3）。SQLAlchemy は既定で `autocommit` を切っている |
| `{'param_1': 1}` | **その SQL に渡された値。** SQL の本体と値が**別に**送られている（8.4.3 で重要になります） |
| `[cached since ...]` | **同じ形の SQL を組み立て直さずに使い回した**という印。SQL の本数は減っていない |
| `ROLLBACK` | 読むだけで何も変えなかったので、`close()` のときに打ち消した（fastapi-text 6.5.2） |

**`[cached since ...]` を「キャッシュが効いたから速い」と読み違えないでください。**
キャッシュされているのは **Python 側の組み立て作業**だけで、
**MySQL への往復は3回とも発生しています。**

**② MySQL 側：general_log で数える**

Python 側の出力は「SQLAlchemy がそう言っている」という話です。
**MySQL 側でも数えて、突き合わせます。**

第7章 7.4.3 で**スロークエリログ**を使いました。同じ仕組みで、
**すべての SQL を記録する**設定があります。**general_log**（一般クエリログ）です。

`mysql` に root で接続して、記録を始めます。

```sql
SET GLOBAL log_output = 'TABLE';
SET GLOBAL general_log = ON;
TRUNCATE TABLE mysql.general_log;
```

| 打ったもの | 意味 |
|-----------|------|
| `log_output = 'TABLE'` | 記録先をテーブル（`mysql.general_log`）にする（7.4.3 と同じ） |
| `general_log = ON` | **すべての SQL の記録を始める** |
| `TRUNCATE TABLE mysql.general_log` | いままでの記録を空にする（4.3.2） |

この状態で、**ブラウザか別のターミナルから `GET /tasks` を1回だけ**実行します。

**Windows（PowerShell）**

```powershell
curl.exe http://127.0.0.1:8000/tasks
```

**macOS / Linux**

```bash
curl http://127.0.0.1:8000/tasks
```

すぐに記録を止めます。**付けっぱなしにしないでください**（理由は下の注意）。

```sql
SET GLOBAL general_log = OFF;
```

数えます。

```sql
SELECT COUNT(*) AS 本数 FROM mysql.general_log
WHERE command_type = 'Query'
  AND CONVERT(argument USING utf8mb4) LIKE 'SELECT%FROM%';
```

実行結果:

```text
+------+
| 本数 |
+------+
|    4 |
+------+
1 row in set (0.00 sec)
```

**4本。** Python 側の出力と一致しました。中身も見ます。

```sql
SELECT LEFT(CONVERT(argument USING utf8mb4), 32) AS 先頭32文字
FROM mysql.general_log
WHERE command_type = 'Query'
  AND CONVERT(argument USING utf8mb4) LIKE 'SELECT%FROM%';
```

実行結果:

```text
+----------------------------------+
| 先頭32文字                       |
+----------------------------------+
| SELECT tasks.id, tasks.title, ta |
| SELECT comments.id AS comments_i |
| SELECT comments.id AS comments_i |
| SELECT comments.id AS comments_i |
+----------------------------------+
4 rows in set (0.00 sec)
```

使っている道具は、すべて既習です。

| 使ったもの | どこで扱ったか |
|-----------|-------------|
| `CONVERT(argument USING utf8mb4)` | 7.4.3（ログの列はバイト列なので変換が必要） |
| `LIKE 'SELECT%FROM%'` | 3.3.1（`mysql` 自身が打つ `select @@version_comment` を除くため） |
| `LEFT(..., 32)` | 3.6.1（長い SQL を切って読みやすくする） |
| `COUNT(*)` | 6.5.1 |
| `WHERE command_type = 'Query'` | 3.2.1（接続や切断の記録を除く） |

> **注意：general_log は付けっぱなしにしないでください**
> スロークエリログは「遅いものだけ」でしたが、**general_log はすべての SQL を記録します。**
> 本番のサーバーで付けたままにすると、記録だけでディスクを使い切ります。
> **数え終わったら必ず `OFF` に戻してください。**
>
> なお `SET GLOBAL` はコンテナを作り直すと既定値に戻ります（7.4.3 の注意と同じ）。
> 念のため、確かめる1行を覚えておくと安心です。
>
> ```sql
> SHOW VARIABLES LIKE 'general_log';
> ```

### 8.3.3 まとめて取得する

直し方は、**「1件ずつ聞く」をやめて「まとめて聞く」**です。

やりたいことを SQL で書くなら、こうなります。

```sql
SELECT * FROM comments WHERE task_id IN (1, 2, 3) ORDER BY id;
```

**`IN` は第3章 3.3.2 で扱いました。** 3本を1本にまとめられます。

SQLAlchemy にこれをさせる指定が **`selectinload`** です。
8.3.2 のスクリプトの B が、それです。

```python
select(Task).options(selectinload(Task.comments))
```

`check_n_plus_1.py` の実行結果（B の部分）:

```text
===== B selectinload を付けた書き方 =====
INFO:sqlalchemy.engine.Engine:BEGIN (implicit)
INFO:sqlalchemy.engine.Engine:SELECT tasks.id, tasks.title, tasks.done, tasks.code, tasks.priority, tasks.tags, tasks.owner_name, tasks.owner_email
FROM tasks
INFO:sqlalchemy.engine.Engine:[generated in 0.00008s] {}
INFO:sqlalchemy.engine.Engine:SELECT comments.task_id AS comments_task_id, comments.id AS comments_id, comments.body AS comments_body, comments.created_at AS comments_created_at
FROM comments
WHERE comments.task_id IN (%(primary_keys_1)s, %(primary_keys_2)s, %(primary_keys_3)s) ORDER BY comments.id
INFO:sqlalchemy.engine.Engine:[generated in 0.00012s] {'primary_keys_1': 1, 'primary_keys_2': 2, 'primary_keys_3': 3}
  牛乳を買う: コメント 2 件
  レポートを書く: コメント 1 件
  部屋を片づける: コメント 3 件
```

**`IN (...)` が出てきて、SQL は2本になりました。**
`{'primary_keys_1': 1, 'primary_keys_2': 2, 'primary_keys_3': 3}` が、
1本目で取れた `tasks.id` の一覧です。

**窓口に適用する**

`fastapi-lesson/app/routers/tasks.py` の一覧の窓口を直します。
fastapi-text 6.4.2 で書いたコードに、**`.options(...)` を足すだけ**です。

`fastapi-lesson/app/routers/tasks.py`（一覧の窓口の中。`statement` を組み立てている行の下）

```diff
  statement = select(Task)
+ # コメントをまとめて取る（1件ずつ取ると N+1 になる。8.3.1）
+ statement = statement.options(selectinload(Task.comments))
```

`import` を足します。

`fastapi-lesson/app/routers/tasks.py`（先頭）

```diff
- from sqlalchemy.orm import Session
+ from sqlalchemy.orm import Session, selectinload
```

> **注意：ページネーションと組み合わせるときの順番**
> fastapi-text 6.4.5 で、`skip` / `limit` / `order_by` を積み上げる形にしました。
> **`.options(...)` は、その積み上げのどこに入れても構いません。**
> 「絞り込み → 数える → 並べ替え → 切り出し」という順番（6.4.5）に影響しないためです。
>
> ただし、**`count` を数えるための `statement.subquery()` には `.options(...)` を付けないでください。**
> 数えるだけの SQL でコメントまで取るのは無駄です。

もう一度、8.3.2 の手順で数えてみてください。

```sql
SET GLOBAL general_log = ON;
TRUNCATE TABLE mysql.general_log;
```

`GET /tasks` を実行してから、

```sql
SET GLOBAL general_log = OFF;
SELECT COUNT(*) AS 本数 FROM mysql.general_log
WHERE command_type = 'Query'
  AND CONVERT(argument USING utf8mb4) LIKE 'SELECT%FROM%';
```

実行結果:

```text
+------+
| 本数 |
+------+
|    2 |
+------+
```

**4本が2本になりました。** タスクが 100 件でも **2本のまま**です。

**`selectinload` と `joinedload`**

まとめて取る指定は、もう1つあります。**`joinedload`** です。

```python
from sqlalchemy.orm import joinedload

select(Task).options(joinedload(Task.comments))
```

こちらは **`LEFT JOIN` を使って1本にまとめます**（第6章 6.3.1）。

| | `selectinload` | `joinedload` |
|---|---------------|-------------|
| 発行される SQL | **2本**（`IN` を使う） | **1本**（`LEFT JOIN`） |
| 1対多で使ったとき | 行数はそのまま | **親の行が、子の件数ぶん重複して返る**（6.2.2） |
| `LIMIT` との相性 | よい | **悪い**（`LIMIT` が結合後の行に掛かる） |
| 向いている関係 | **1対多**（コメント・明細） | **多対1**（タスク → 登録者） |

このテキストでは、**1対多には `selectinload`** を使います。
**`joinedload` を1対多で `LIMIT` と一緒に使うと、件数が狂います。**
第6章 6.2.2 で「結合すると行が増える」と扱ったことが、そのまま効いてきます。

> **よくある間違い**
> **`.options(...)` を付けたのに N+1 が直らない**、という相談がよくあります。
> 原因はたいてい次の2つです。
>
> 1. **別の関係を触っている。** `selectinload(Task.comments)` は `task.comments` だけに効きます。
>    `comment.task` を読めば、そこで新しい `SELECT` が飛びます
> 2. **セッションを閉じたあとに触っている。**
>    `db.close()` のあとに `task.comments` を読むと、取りに行けずに次のエラーになります
>
>    ```text
>    sqlalchemy.orm.exc.DetachedInstanceError: Parent instance <Task at 0x...> is not bound to a Session
>    ```
>
> **どちらも、8.3.2 の方法で本数を数えれば区別できます。**
> 「直ったつもり」で止めず、**必ず数えて確かめてください。**

---

## 8.4 SQL インジェクション

### 8.4.1 何が起きるのか

タスクを**タイトルで検索する窓口**を作ります。
**まず、わざと危ない書き方で作ります。** 名前にも `-ng` と付けておきます。

`fastapi-lesson/app/routers/tasks.py`（末尾に追記）

```python
@router.get("/search-ng", response_model=list[dict])
def search_tasks_ng(q: str = Query(min_length=1), db: Session = Depends(get_db)):
    """【危険な例】文字列を繋いで SQL を組み立てている（mysql-text 8.4.1）。"""
    sql = f"SELECT id, title FROM tasks WHERE title LIKE '%{q}%'"
    print("組み立てられた SQL:", sql)
    rows = db.execute(text(sql)).mappings().all()
    return [dict(row) for row in rows]
```

`import` を足します。

`fastapi-lesson/app/routers/tasks.py`（先頭）

```diff
- from sqlalchemy import select
+ from sqlalchemy import select, text
```

**`text(...)`** は、「**この文字列を、そのまま SQL として実行する**」という関数です。
`select(Task)` のように組み立てるのではなく、SQL を直接書きたいときに使います。

`.mappings().all()` は、**結果を「列名と値の辞書」の形で受け取る**指定です。
`response_model=list[dict]` にしているので、そのまま JSON になります。

> **注意：窓口を書く順番に気をつけてください**
> `@router.get("/{task_id}")` より**下**にこの窓口を書くと、
> `/tasks/search-ng` が「`task_id` に `search-ng` が来た」と解釈され、`422` になります。
> **`/{task_id}` より上に置いてください。**
> FastAPI は、**書いた順に上から**合うものを探します。

サーバーを起動して、**まずは普通に**使ってみます。

**Windows（PowerShell）**

```powershell
curl.exe "http://127.0.0.1:8000/tasks/search-ng?q=牛乳"
```

**macOS / Linux**

```bash
curl "http://127.0.0.1:8000/tasks/search-ng?q=牛乳"
```

```json
[{"id":1,"title":"牛乳を買う"}]
```

サーバーを動かしているターミナルには、組み立てられた SQL が出ています。

```text
組み立てられた SQL: SELECT id, title FROM tasks WHERE title LIKE '%牛乳%'
```

**狙ったとおりです。** 次に、**普通ではない文字**を入れます。

**Windows（PowerShell）**

```powershell
curl.exe --get --data-urlencode "q=' OR '1'='1" http://127.0.0.1:8000/tasks/search-ng
```

**macOS / Linux**

```bash
curl --get --data-urlencode "q=' OR '1'='1" http://127.0.0.1:8000/tasks/search-ng
```

`--data-urlencode` は、**記号を URL に載せられる形に変換してから送る**指定です。

```json
[{"id":1,"title":"牛乳を買う"},{"id":2,"title":"レポートを書く"},{"id":3,"title":"部屋を片づける"}]
```

**「牛乳」も何も含まないのに、全件返ってきました。**
サーバー側の出力を見てください。

```text
組み立てられた SQL: SELECT id, title FROM tasks WHERE title LIKE '%' OR '1'='1%'
```

**送った文字が、値ではなく SQL の文法になっています。**

```mermaid
flowchart TB
    A["f文字列で組み立てる<br/>...LIKE '%{q}%'"] --> B["q に ' OR '1'='1 が入る"]
    B --> C["...LIKE '%' OR '1'='1%'"]
    C --> D["WHERE が2つの条件の OR になった<br/>（3.2.3）"]
    D --> E["'1'='1' は常に真 → 全件"]
```

`'` を1つ送るだけで、**文字列の終わりをこちらで決められてしまいます。**
そこから先は、攻撃側が自由に SQL を書けます。

このように、**入力された文字が SQL の一部として実行されてしまうこと**を
**SQL インジェクション**（SQL 注入）と呼びます。

**返してはいけないものが漏れます**

「全件見えるだけなら」と思うかもしれません。**もっと深刻なことができます。**

fastapi-text では、`owner_email` を**絶対に外に出さない**ように作ってありました。
`TaskRead` に `owner_email` を書かないことで、レスポンスから落としていました
（fastapi-text 4.4.2 / 6.3.3）。

**その仕組みを、この窓口は素通りさせます。**

**Windows（PowerShell）**

```powershell
curl.exe --get --data-urlencode "q=' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '" http://127.0.0.1:8000/tasks/search-ng
```

**macOS / Linux**

```bash
curl --get --data-urlencode "q=' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '" http://127.0.0.1:8000/tasks/search-ng
```

組み立てられた SQL:

```text
SELECT id, title FROM tasks WHERE title LIKE '%' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '%'
```

```json
[{"id":1,"title":"牛乳を買う"},
 {"id":2,"title":"レポートを書く"},
 {"id":3,"title":"部屋を片づける"},
 {"id":1,"title":"yamada@example.com"},
 {"id":2,"title":"suzuki@example.com"},
 {"id":3,"title":"yamada@example.com"}]
```

**`title` という名前で、メールアドレスが返ってきました。**

`UNION` は「**2つの `SELECT` の結果を縦に繋ぐ**」演算子です。
列の数と型が合っていれば、**どのテーブルのどの列でも繋げます。**
`comments` の本文も、`users` テーブルがあればそのハッシュ化されたパスワードも、同じ方法で読めます。

**Pydantic のスキーマは、SQL の中身を守りません。**
`TaskRead` が守っているのは「**モデルのオブジェクトを JSON にするとき**」だけです。
SQL が返した値をそのまま返す窓口には、**何の効き目もありません。**

> **補足：`; DROP TABLE` は、この構成では通りません**
> インジェクションの説明では、`'; DROP TABLE tasks; --` という例をよく見ます。
> この窓口で試すと、次のエラーになります。
>
> ```text
> pymysql.err.ProgrammingError: (1064, "You have an error in your SQL syntax; ...")
> ```
>
> **PyMySQL は、1回の送信に複数の文を載せない**設定で動いているためです。
>
> **「だから安全」ではありません。** いま見たとおり、
> **1文のままでも、`OR` と `UNION` で読み放題**です。
> 「消される」より「**気づかれずに読まれる**」ほうが、実際には多い被害です。

### 8.4.2 文字列連結で SQL を組み立てない

**原因は1か所です。**

```python
sql = f"SELECT id, title FROM tasks WHERE title LIKE '%{q}%'"
```

f 文字列（python-text 2.3.3）は、**`{}` の中身をそのまま文字として埋め込みます。**
`q` が `'` を含んでいても、`UNION` を含んでいても、**区別せずに埋め込みます。**

**やってはいけない書き方**を、形で覚えてください。

| 書き方 | 判定 |
|-------|------|
| `f"... WHERE title = '{q}'"` | **危険。** f 文字列で SQL を組み立てている |
| `"... WHERE title = '" + q + "'"` | **危険。** `+` で繋いでいるだけで同じ |
| `"... WHERE title = '%s'" % q` | **危険。** `%` 演算子も同じ |
| `"... WHERE title = '{}'".format(q)` | **危険。** `.format()` も同じ |

**共通しているのは、「SQL の文字列に、外から来た値を混ぜている」ことです。**

**自分でエスケープするのも、やめてください**

「`'` を `''` に置き換えればいいのでは」と考えたくなります。

```python
# これはやらないでください
q = q.replace("'", "''")
```

**この方法は、すぐに破れます。** 理由を3つ挙げます。

1. **`\` の扱いが、データベースによって違う。** MySQL では `\'` も文字列の中の `'` として扱われます
2. **文字コードによって、置き換えをすり抜ける並びがある**
3. **数値を受け取る場所では、そもそも `'` が要らない。**
   `WHERE id = {q}` に `1 OR 1=1` を渡せば、`'` を1つも使わずに全件取れます

**「正しくエスケープする」のは、自分でやる仕事ではありません。**
データベースのライブラリがやる仕事です。

**ORM を使っているなら、すでに守られています**

fastapi-text 第6章で書いた形は、**もともと安全**です。確かめておきます。

`fastapi-lesson` の直下で、次を実行してください。

`fastapi-lesson/check_orm_safe.py`（ファイル全体）

```python
"""ORM の書き方が組み立てる SQL を見る（mysql-text 8.4.2）。"""

import logging

from sqlalchemy import select

from app.database import SessionLocal
from app.models import Task

logging.basicConfig()
logging.getLogger("sqlalchemy.engine").setLevel(logging.INFO)

q = "' OR '1'='1"

db = SessionLocal()
try:
    rows = db.scalars(select(Task).where(Task.title.like(f"%{q}%"))).all()
    print("件数:", len(rows))
finally:
    db.close()
```

**Windows（PowerShell）**

```powershell
python check_orm_safe.py
```

**macOS / Linux**

```bash
python check_orm_safe.py
```

実行結果（抜粋）:

```text
INFO:sqlalchemy.engine.Engine:SELECT tasks.id, tasks.title, tasks.done, tasks.code, tasks.priority, tasks.tags, tasks.owner_name, tasks.owner_email
FROM tasks
WHERE tasks.title LIKE %(title_1)s
INFO:sqlalchemy.engine.Engine:[generated in 0.00013s] {'title_1': "%' OR '1'='1%"}
件数: 0
```

**2つのことが起きています。**

1. SQL の本体に値が入っていない。**`%(title_1)s` という場所取りだけ**がある
2. 値は `{'title_1': "%' OR '1'='1%"}` として**別に**渡されている

だから **`' OR '1'='1` は「そういう文字を含むタイトル」として扱われ、0件**になります。

**この場所取りを、プレースホルダと呼びます。** 次の項で正面から扱います。

> **`f"%{q}%"` を使っているのに安全なのはなぜか**
> f 文字列を使っていますが、**SQL を組み立てていません。**
> 作っているのは `LIKE` に渡す**値**（`%...%` という検索パターン）です。
>
> **危ないのは「SQL の文を作ること」で、「値を作ること」は危なくありません。**
> この違いが、8.4 でいちばん大事な区別です。

### 8.4.3 プレースホルダを使う

SQL を自分で書きたい場合（`text(...)` を使う場合）も、安全に書けます。
**値を書くところに `:名前` を置いて、値は別に渡します。**

8.4.1 の危ない窓口を、安全な形で作り直します。

`fastapi-lesson/app/routers/tasks.py`（末尾に追記）

```python
@router.get("/search", response_model=list[dict])
def search_tasks(q: str = Query(min_length=1), db: Session = Depends(get_db)):
    """タイトルで探す（プレースホルダを使う。mysql-text 8.4.3）。"""
    sql = text("SELECT id, title FROM tasks WHERE title LIKE :pattern")
    rows = db.execute(sql, {"pattern": f"%{q}%"}).mappings().all()
    return [dict(row) for row in rows]
```

**違いは2行だけ**です。

| | 危ない書き方（8.4.1） | 安全な書き方 |
|---|-------------------|------------|
| SQL | `f"... LIKE '%{q}%'"` | `text("... LIKE :pattern")` |
| 値 | SQL の中に埋め込む | `{"pattern": f"%{q}%"}` として**別に渡す** |
| `'` の扱い | 自分が書く | **書かない**（ライブラリが付ける） |

**`:pattern` の前後に `'` を書いていない**ことに注目してください。
プレースホルダを使うときは、**引用符を自分で書きません。**

試してみます。

**Windows（PowerShell）**

```powershell
curl.exe "http://127.0.0.1:8000/tasks/search?q=牛乳"
curl.exe --get --data-urlencode "q=' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '" http://127.0.0.1:8000/tasks/search
```

**macOS / Linux**

```bash
curl "http://127.0.0.1:8000/tasks/search?q=牛乳"
curl --get --data-urlencode "q=' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '" http://127.0.0.1:8000/tasks/search
```

```json
[{"id":1,"title":"牛乳を買う"}]
[]
```

**普通の検索は動き、攻撃は 0 件**です。

**なぜ安全なのか**

プレースホルダを使うと、**SQL の形が、値を知る前に確定します。**

```mermaid
flowchart TB
    subgraph NG["危ない書き方（8.4.1）"]
        N1["値を文字列に混ぜる"] --> N2["できあがった文字列を解釈する"]
        N2 --> N3["値だったものが文法として読まれる"]
    end
    subgraph OK["プレースホルダ"]
        O1["SQL の形だけを先に解釈する<br/>WHERE title LIKE ?"] --> O2["あとから値を当てはめる"]
        O2 --> O3["値は、何が書かれていても値のまま"]
    end
```

**先に「ここには値が1つ来る」と決まっている**ので、
そこに `OR` と書いても `UNION` と書いても、**「OR という文字列」として扱われます。**
文法として読まれる余地がありません。

この「形を先に決めておく」仕組みを**プリペアドステートメント**（準備された文）と呼びます。
8.3.2 のログに出ていた `{'param_1': 1}` も、同じ仕組みでした。

> **よくある間違い**
> **テーブル名や列名をプレースホルダにしようとする**間違いです。
>
> ```python
> # これは動きません
> sql = text("SELECT id, title FROM tasks ORDER BY :column")
> rows = db.execute(sql, {"column": "priority"})
> ```
>
> エラーにはなりませんが、**並び替わりません。**
> `ORDER BY 'priority'` という**文字列で並べる**という意味になり、
> 全部の行が同じ値になるためです。
>
> **プレースホルダに入れられるのは値だけ**です。列名は SQL の**形**の一部なので入りません。
>
> 外から来た文字で列名を決めたい場合は、**許可リスト**（あらかじめ決めた候補以外を受け付けない形）にします。
>
> ```python
> ALLOWED_ORDER = {"id": "id", "priority": "priority", "title": "title"}
>
> def build_order_sql(order: str) -> str:
>     """許可した列名だけを SQL に入れる。"""
>     column = ALLOWED_ORDER.get(order)
>     if column is None:
>         raise HTTPException(status_code=422, detail=f"{order} では並べ替えられません")
>     return f"SELECT id, title FROM tasks ORDER BY {column}"
> ```
>
> **f 文字列を使っていますが、埋め込まれるのは `ALLOWED_ORDER` の値だけ**です。
> 外から来た文字がそのまま SQL に入ることはありません。
> 「**外から来た文字を SQL に入れない**」という原則は、これでも守られています。

**危ない窓口を消しておく**

`search-ng` は、**説明のためにわざと作ったもの**です。
確認が終わったら、`app/routers/tasks.py` から**削除してください。**

**残したままにしないでください。** docker-text 第7章で本番用のイメージを作りましたが、
**そこにこの窓口が入っていると、そのまま攻撃できる窓口を公開することになります。**

---

## 8.5 コネクションプール

### 8.5.1 接続は高コスト

第7章では「SQL 1本の速さ」を測りました。
実は、**SQL を打つ前の「接続する」という作業にも時間がかかります。**

接続のときに起きていることを並べると、次のようになります。

```mermaid
flowchart LR
    A["① TCP を繋ぐ"] --> B["② サーバーの<br/>あいさつを受ける"]
    B --> C["③ パスワードを<br/>暗号で検証する"]
    C --> D["④ MySQL 側で<br/>スレッドを1つ用意する"]
    D --> E["⑤ 文字コードなどの<br/>初期設定を送る"]
    E --> F["やっと SELECT が打てる"]
```

③は、8.1.2 で `cryptography` を入れた理由そのものです。**暗号の計算が入ります。**
④で MySQL 側は、接続1本につき**処理の担当を1つ**用意します。これにも上限があります。

**測ってみます。** `fastapi-lesson` の直下にスクリプトを作ります。

`fastapi-lesson/check_pool.py`（ファイル全体）

```python
"""接続の作り直しと使い回しを比べる（mysql-text 8.5.1）。"""

import time

from sqlalchemy import create_engine, text
from sqlalchemy.pool import NullPool

from app.config import settings

TIMES = 50


def measure(label: str, engine) -> None:
    """SELECT 1 を TIMES 回打って、かかった時間を出す。"""
    # 1回目は捨てる（最初の接続だけ余計に時間がかかるため）
    with engine.connect() as conn:
        conn.execute(text("SELECT 1"))

    start = time.perf_counter()
    for _ in range(TIMES):
        with engine.connect() as conn:
            conn.execute(text("SELECT 1"))
    elapsed = time.perf_counter() - start
    print(f"{label}: {TIMES} 回で {elapsed:.3f} 秒（1回あたり {elapsed / TIMES * 1000:.1f} ミリ秒）")


measure("毎回つなぎ直す ", create_engine(settings.database_url, poolclass=NullPool))
measure("使い回す（既定）", create_engine(settings.database_url))
```

**Windows（PowerShell）**

```powershell
python check_pool.py
```

**macOS / Linux**

```bash
python check_pool.py
```

実行結果（**執筆時の1台での値**です。第7章と同じく、絶対値ではなく比を見てください）:

```text
毎回つなぎ直す : 50 回で 0.049 秒（1回あたり 1.0 ミリ秒）
使い回す（既定）: 50 回で 0.013 秒（1回あたり 0.3 ミリ秒）
```

**約3倍の差**が出ました。
`SELECT 1` は「何もしない SQL」なので、**この差はほぼ接続の作業ぶん**です。

`poolclass=NullPool` が「**使い回さず、毎回つなぎ直す**」指定です。
指定しない既定が「**使い回す**」です。
この「使い回すための待機所」を**コネクションプール**（接続の溜め場）と呼びます。

**MySQL 側から見ても確かめられます**

MySQL は「これまでに何回接続されたか」を数えています。

```sql
SHOW GLOBAL STATUS LIKE 'Connections';
```

実行結果:

```text
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Connections   | 243   |
+---------------+-------+
```

この数を**前後で比べます。** まず `NullPool` のほうを 10 回だけ動かします。

`fastapi-lesson/check_connections.py`（ファイル全体）

```python
"""接続が何回作られるかを確かめる（mysql-text 8.5.1）。"""

import sys

from sqlalchemy import create_engine, text
from sqlalchemy.pool import NullPool

from app.config import settings

# 引数に null を渡したら NullPool、何も渡さなければ既定のプール
if len(sys.argv) > 1 and sys.argv[1] == "null":
    engine = create_engine(settings.database_url, poolclass=NullPool)
    print("毎回つなぎ直す設定で 10 回打ちます。")
else:
    engine = create_engine(settings.database_url)
    print("使い回す設定で 10 回打ちます。")

for _ in range(10):
    with engine.connect() as conn:
        conn.execute(text("SELECT 1"))
print("終わりました。")
```

**Windows（PowerShell）**

```powershell
python check_connections.py null
```

**macOS / Linux**

```bash
python check_connections.py null
```

もう一度、MySQL 側で数えます。

```sql
SHOW GLOBAL STATUS LIKE 'Connections';
```

```text
| Connections   | 254   |
```

**11 増えました。** Python からの 10 回ぶんと、
**いま `mysql` で打ったコマンド自身の1回**です
（`docker compose exec db mysql ...` を打つたびに1本増えます）。

次に、使い回す設定で動かします。

**Windows（PowerShell）**

```powershell
python check_connections.py
```

**macOS / Linux**

```bash
python check_connections.py
```

```text
| Connections   | 256   |
```

**2つしか増えていません。** Python 側は**1本しか作らず**、それを 10 回使い回しました。

**アプリでは、これが決定的に効きます。**
1リクエストにつき1つセッションを作る作り（fastapi-text 6.5.1）なので、
**プールが無ければ、リクエストごとに接続をつなぎ直すことになります。**

### 8.5.2 設定項目

プールの状態は、Python から読めます。

`fastapi-lesson/check_pool_status.py`（ファイル全体）

```python
"""プールの状態を見る（mysql-text 8.5.2）。"""

from sqlalchemy import create_engine

from app.config import settings

engine = create_engine(settings.database_url)

print("種類:", engine.pool.__class__.__name__)
print("最初 :", engine.pool.status())

borrowed = [engine.connect() for _ in range(3)]
print("3本借りた:", engine.pool.status())

for conn in borrowed:
    conn.close()
print("返した後 :", engine.pool.status())
```

**Windows（PowerShell）**

```powershell
python check_pool_status.py
```

**macOS / Linux**

```bash
python check_pool_status.py
```

実行結果:

```text
種類: QueuePool
最初 : Pool size: 5  Connections in pool: 0 Current Overflow: -5 Current Checked out connections: 0
3本借りた: Pool size: 5  Connections in pool: 0 Current Overflow: -2 Current Checked out connections: 3
返した後 : Pool size: 5  Connections in pool: 3 Current Overflow: -2 Current Checked out connections: 0
```

| 表示 | 意味 |
|------|------|
| `Pool size: 5` | **溜め場の大きさ**（既定は 5） |
| `Connections in pool` | **いま待機している**接続の本数 |
| `Checked out connections` | **いま貸し出し中**の本数 |
| `Current Overflow` | **溜め場を超えて作った本数**（負の数は「まだ余裕がこれだけある」） |

**「返した後」に `Connections in pool: 3` になっている**ことが、プールの本質です。
`close()` しても**切断していません。** 待機所に戻しただけです。

**設定できる項目**

`create_engine(...)` に渡します。

| 設定 | 既定 | 意味 |
|------|------|------|
| `pool_size` | 5 | 待機所に置いておく本数 |
| `max_overflow` | 10 | **足りないときに、一時的に増やせる本数**（合計 15 本まで） |
| `pool_timeout` | 30 | 空きが出るのを**何秒待つか**。超えると例外 |
| `pool_recycle` | -1（無効） | **この秒数を超えた接続は、使う前に作り直す** |
| `pool_pre_ping` | `False` | **使う前に生きているか確かめる**（8.1.2 で `True` にしました） |

**`pool_pre_ping` と `pool_recycle` は、MySQL 側の設定とセットで考えます。**

MySQL には「**一定時間使われない接続を切る**」決まりがあります。

```sql
SHOW VARIABLES LIKE 'wait_timeout';
```

実行結果:

```text
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wait_timeout  | 28800 |
+---------------+-------+
```

**28800 秒（8時間）**です。8時間放置された接続は、MySQL 側から切られます。
**プールはそれを知りません。** 次に使おうとしたときに、こうなります。

```text
pymysql.err.OperationalError: (2013, 'Lost connection to MySQL server during query')
```

**「朝いちばんのリクエストだけ失敗する」**という、原因の分かりにくい不具合の正体がこれです。

| 対策 | どう効くか |
|------|----------|
| `pool_pre_ping=True` | **使う直前に確認**し、死んでいたら作り直す。確実だが毎回1往復増える |
| `pool_recycle=3600` | **1時間を超えた接続は、問答無用で作り直す。** 確認の往復が要らない |

このテキストでは、**`pool_pre_ping=True`** を使います（8.1.2）。
`wait_timeout` は環境によって変わるため、**秒数を当てにしない**ほうが安全です。

**本数の上限は、MySQL 側にもあります**

```sql
SHOW VARIABLES LIKE 'max_connections';
```

実行結果:

```text
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
```

**同時に接続できるのは 151 本まで**です。超えるとこうなります。

```text
pymysql.err.OperationalError: (1040, 'Too many connections')
```

**掛け算で見積もってください。**

```text
(pool_size + max_overflow) × アプリのプロセス数 < max_connections
```

既定のままなら 1 プロセスで最大 15 本です。10 プロセス動かすと 150 本になり、
**151 本の上限にほぼ届きます。**
docker-text 第7章で本番用の構成を作りましたが、
**`api` コンテナを増やすときは、この掛け算を確かめてください。**

いま何本繋がっているかは、こう見ます。

```sql
SHOW STATUS LIKE 'Threads_connected';
```

実行結果:

```text
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| Threads_connected | 2     |
+-------------------+-------+
```

> **よくある間違い**
> **`pool_size` を大きくすれば速くなる**と考える間違いです。
>
> 接続を増やしても、**MySQL が同時に処理できる量は増えません。**
> 増えるのは「待っている接続」の数だけで、
> `max_connections` に近づくほど**全体が遅くなります。**
>
> **プールは「接続の作り直しを減らす」ための仕組み**であって、
> 処理を速くする仕組みではありません。
> 遅いときにまず見るのは、第7章の `EXPLAIN` と、この章の 8.3（本数）です。

---

## 8.6 バックアップ

### 8.6.1 ダンプを取る

第4章 4.2.3 で「`WHERE` を忘れると全件更新される」と書きました。
4.2.4 で「先に `SELECT` して確かめる」という予防も扱いました。

**それでも、いつか事故は起きます。** 最後の備えが**バックアップ**です。

MySQL には、**データベースの中身をまるごと SQL の文に書き出す**道具があります。
**`mysqldump`**（マイエスキューエルダンプ）です。書き出したファイルを**ダンプ**と呼びます。

**ボリュームをコピーするのとは違います**

docker-text 第4章で、データは名前付きボリュームに入っていると学びました。
「ボリュームをコピーすればいいのでは」と思うかもしれません。

| | ボリュームのコピー | `mysqldump` |
|---|-----------------|------------|
| 中身 | MySQL 独自の形式のファイル群 | **読める SQL の文**（`CREATE TABLE` と `INSERT`） |
| 動いている最中に取れるか | **危険**（書き込み中の状態が混ざる） | **取れる**（`--single-transaction`） |
| バージョンをまたげるか | 難しい | **やりやすい**（ただの SQL なので） |
| 一部だけ戻せるか | 難しい | **できる**（ファイルを編集すればよい） |
| 大きさ | そのまま | **小さい**（インデックスは含まれず、定義だけ） |

**この本では `mysqldump` を使います。** 中身が読めることが、学習中はいちばん役に立ちます。

**取ってみる**

`shop` のダンプを取ります。**書き出し先は、第2章 2.1.1 でマウントした `sql` ディレクトリ**にします
（`./sql:/sql`）。コンテナの中の `/sql` に書けば、**手元の `mysql-lesson/sql/` に現れます。**

**Windows（PowerShell）**

```powershell
cd ~\Documents\mysql-lesson
docker compose exec db sh -c "mysqldump -u root -proot_pass_1234 --single-transaction --no-tablespaces --default-character-set=utf8mb4 shop > /sql/shop_backup.sql"
```

**macOS / Linux**

```bash
cd ~/Documents/mysql-lesson
docker compose exec db sh -c "mysqldump -u root -proot_pass_1234 --single-transaction --no-tablespaces --default-character-set=utf8mb4 shop > /sql/shop_backup.sql"
```

**パスワードは `-p` に続けて、空白を空けずに**書きます（`-proot_pass_1234`）。
2.1.1 で決めた `MYSQL_ROOT_PASSWORD` の値です。

実行結果（警告が1行出ますが、成功しています）:

```text
mysqldump: [Warning] Using a password on the command line interface can be insecure.
```

`mysql-lesson/sql/shop_backup.sql` ができています。確認してください。

指定した4つのオプションの意味です。

| オプション | 意味 |
|-----------|------|
| `--single-transaction` | **取り始めた時点の姿を、1つのトランザクションとして読む**（4.4.2）。動かしたまま取れる |
| `--no-tablespaces` | 余分な権限（`PROCESS`）を要求しないようにする。**root 以外で取るときに必要** |
| `--default-character-set=utf8mb4` | **日本語のために必須**（2.6.1 と同じ理由） |
| `shop` | ダンプするデータベースの名前 |

> **注意：`sh -c "..."` で囲んでいる理由**
> 次のように書きたくなりますが、**やめてください。**
>
> ```text
> docker compose exec db mysqldump ... shop > sql\shop_backup.sql
> ```
>
> この `>` は、**Docker ではなく、あなたのシェルが**解釈します。
> そして **PowerShell の `>` は、既定で UTF-16 という文字コードでファイルを書きます。**
> でき上がったファイルは、`SOURCE` で読み込めません（2.5.2 の補足と同じ問題です）。
>
> `sh -c "..."` で囲むと、**`>` を解釈するのはコンテナの中のシェル**になります。
> **Windows と macOS で、まったく同じコマンドが使えます。**

**中身を読む**

VS Code で `mysql-lesson/sql/shop_backup.sql` を開いてください。
**第2章で自分が書いた `shop.sql` と、驚くほど似ています。**

```sql
-- MySQL dump 10.13  Distrib 8.0.46, for Linux (x86_64)
--
-- Host: localhost    Database: shop
-- ------------------------------------------------------
-- Server version	8.0.46

/*!50503 SET NAMES utf8mb4 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;

--
-- Table structure for table `categories`
--

DROP TABLE IF EXISTS `categories`;
CREATE TABLE `categories` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

--
-- Dumping data for table `categories`
--

LOCK TABLES `categories` WRITE;
INSERT INTO `categories` VALUES (1,'食器'),(2,'キッチン'),(3,'収納'),(4,'文房具');
UNLOCK TABLES;
```

読み取れることを挙げます。

| 出ているもの | 意味 | 対応する本文 |
|------------|------|-----------|
| `DROP TABLE IF EXISTS` | **何度流し込んでも同じ状態になる**ようにしている | 2.5.2 |
| `CREATE TABLE ...` | **テーブルの形も入っている**（データだけではない） | 2.4.3 |
| `INSERT INTO ... VALUES (...),(...)` | **複数件をまとめて追加する形**で書かれている | 4.1.2 |
| `SET NAMES utf8mb4` | 読み込むときの文字コードを、ファイル自身が指定している | 2.6.2 |
| `FOREIGN_KEY_CHECKS=0` | **読み込む順番で外部キー制約に引っかからない**ようにしている | 5.4.3 |
| `/*! ... */` で囲まれた行 | MySQL だけが読むコメント。他のデータベースでは無視される | — |

**`FOREIGN_KEY_CHECKS=0` が入っている**のは大事な工夫です。
`order_items` は `orders` を参照していますが（第5章 5.4.2 で制約を付けた場合）、
**ダンプはテーブル名のアルファベット順に並びます。**
そのままでは「まだ無い `orders` を指す行を入れようとした」で `1452` になってしまいます。

### 8.6.2 リストアする

**戻せることを確かめていないバックアップは、バックアップではありません。**
**必ず、戻すところまで練習してください。**

**わざと事故を起こす**

`shop` に接続します（2.2.1）。

```sql
SELECT COUNT(*) FROM products;
```

```text
+----------+
| COUNT(*) |
+----------+
|       20 |
+----------+
```

文房具（`category_id = 4`）を、**うっかり消してしまったことにします。**

```sql
DELETE FROM products WHERE category_id = 4;
SELECT COUNT(*) FROM products;
```

実行結果:

```text
Query OK, 5 rows affected (0.01 sec)

+----------+
| COUNT(*) |
+----------+
|       15 |
+----------+
```

**5件消えました。** `COMMIT` 済みなので、`ROLLBACK` では戻りません（4.4.2）。

**戻す**

ダンプを流し込みます。**第2章 2.5.2 で使った `SOURCE` と同じ**です。

```sql
SOURCE /sql/shop_backup.sql;
```

`Query OK` が並びます。確認します。

```sql
SELECT COUNT(*) FROM products;
SELECT COUNT(*) FROM order_items;
```

実行結果:

```text
+----------+
| COUNT(*) |
+----------+
|       20 |
+----------+

+----------+
| COUNT(*) |
+----------+
|       37 |
+----------+
```

**戻りました。**

**戻すときの注意**

| 注意 | 理由 |
|------|------|
| **接続先のデータベースを間違えない** | ダンプに `USE shop;` は**入っていません**。いま繋いでいるデータベースに流し込まれます |
| **ダンプ以降の変更は失われる** | 戻るのは「ダンプを取った時点」です。その後に入った注文は消えます |
| **`DROP TABLE` から始まる** | 戻すとき、既存のテーブルは**いったん消えます**。途中で失敗すると中途半端な状態になります |

1つ目は、特に気をつけてください。
`taskapp` に繋いだまま `shop_backup.sql` を流し込むと、
**`taskapp` の中に `shop` のテーブルが作られます。**

流す前に、必ず現在地を確かめる習慣を付けてください（2.4.2）。

```sql
SELECT DATABASE();
```

> **補足：データベース名ごと入れたいとき**
> `mysqldump` に `--databases shop` と書くと、
> ダンプの先頭に **`CREATE DATABASE` と `USE shop;` が入ります。**
>
> ```text
> docker compose exec db sh -c "mysqldump -u root -proot_pass_1234 --single-transaction --no-tablespaces --default-character-set=utf8mb4 --databases shop > /sql/shop_full.sql"
> ```
>
> 接続先を間違える事故は防げますが、**`shop` を作り直すので、より広い範囲が消えます。**
> どちらを使うかは「**戻したい範囲**」で決めてください。

**これで十分か**

**十分ではありません。** この章で扱ったのは「手で1回取る」までです。
実際の運用に必要なものは、docker-text 8.2.4 の表に並んでいました。

| 足りないもの | どこが担当するか |
|------------|---------------|
| **定期的に自動で取る** | CI/CD やスケジュール実行（docker-text 8.2.2） |
| **別の場所に置く**（同じディスクにあると一緒に失う） | クラウドのストレージ |
| **取れているかの監視** | 監視の道具（docker-text 8.2.2） |
| **戻す手順の訓練** | **人間**。定期的に 8.6.2 をやる |

**ダンプを同じパソコンの `sql/` に置いただけでは、そのパソコンが壊れたら一緒に失います。**
第9章 9.2.2 で、この先の学び方を案内します。

---

## まとめ

- **アプリに root を使わせない。** アプリ専用のデータベースを作り、
  **`CREATE USER` と `GRANT` で、そのデータベースだけを触れるユーザー**を割り当てる（最小権限の原則）
- MySQL のユーザーは **`'名前'@'接続元'` で1人。** コンテナの外から繋ぐなら `'%'`
- **マイグレーションを動かすには DDL の権限（`CREATE` / `ALTER` / `DROP` / `INDEX` / `REFERENCES`）も必要。**
  読み書きの4つだけでは `1142 CREATE command denied` になる
- 接続 URL は **`mysql+pymysql://ユーザー:パスワード@ホスト:3306/DB名?charset=utf8mb4`**。
  **`?charset=utf8mb4` を忘れない。** **PyMySQL と cryptography** を入れ、
  **`check_same_thread` は接続 URL で分岐**させる
- **接続先のホスト名は、誰が繋ぐかで変わる。**
  手元の Python は **`localhost`**（`ports` の公開が必要）、コンテナの API は**サービス名の `db`**
- **`SHOW CREATE TABLE` で、SQLAlchemy が作ったテーブルを読める。**
  `Mapped[bool]` は **`tinyint(1)`**、`| None` の有無が **`NOT NULL`** になる
- **`ForeignKey` はデータベース側の制約、`relationship` は Python 側の対応づけ。** 別のもので、両方書く。
  **外部キーの列には、MySQL がインデックスを自動で作る**（`SHOW CREATE TABLE` の `KEY` 行）
- **N+1 問題**は、一覧の1本と件数ぶんの N 本。
  **1本1本は速いのでスロークエリログでは見つからない**。**本数を数える**
- 数え方は2つ。**Python 側は `sqlalchemy.engine` のログ**、
  **MySQL 側は `general_log`**（`log_output='TABLE'` → `mysql.general_log`。**付けっぱなしにしない**）
- 直し方は **`selectinload`**（`IN` で2本にまとめる）。
  **1対多で `joinedload` と `LIMIT` を併用すると件数が狂う**
- **SQL インジェクション**は、入力された文字が SQL の文法として実行されること。
  `' OR '1'='1` で全件、`UNION` で**返さないはずの列まで読める**
- **f 文字列・`+`・`%`・`.format()` で SQL を組み立てない。** 自分でエスケープもしない。
  **プレースホルダ（`:名前`）を使い、値は別に渡す**（引用符は自分で書かない）
- **プレースホルダに入れられるのは値だけ。** 列名・テーブル名は**許可リスト**で決める
- **接続は高コスト**（この章では約3倍）。**コネクションプール**が接続を使い回す。
  `pool_pre_ping=True` は **MySQL の `wait_timeout`（既定 8 時間）で切られた接続**への対策
- **`(pool_size + max_overflow) × プロセス数 < max_connections`（既定 151）** を確かめる
- **`mysqldump` はデータベースを読める SQL に書き出す。**
  `--single-transaction` で動かしたまま取れる。`sh -c "..."` で囲めば Windows でも同じコマンドで書ける
- **戻せることを確かめていないバックアップは、バックアップではない。**
  リストアは `SOURCE`。**流し込む前に `SELECT DATABASE();`**

---

## 理解度チェック

**問 8.1**（穴埋め）

アプリ専用のユーザーを作るには（　①　）を打ち、権限を与えるには（　②　）を打つ。
与えた権限は（　③　）で確認できる。MySQL のユーザーは、名前だけでなく
（　④　）とセットで1人として扱われる。

**問 8.2**（選択）

`GRANT SELECT, INSERT, UPDATE, DELETE ON taskapp.* TO 'task_user'@'%';` だけを与えた状態で
`alembic upgrade head` を実行しました。**起きることを1つ選んでください。**

1. 正常に終わる。マイグレーションは読み書きの権限だけで動く
2. `1142 CREATE command denied` になる。テーブルを作る権限が無い
3. `1045 Access denied` になる。パスワードが違う扱いになる
4. `2003 Connection refused` になる。接続そのものができない

**問 8.3**（選択）

タスクが 50 件あり、一覧の窓口が各タスクのコメントを返します。
`selectinload` を**付けていない**とき、発行される `SELECT` の本数は何本ですか。

1. 1 本
2. 2 本
3. 50 本
4. 51 本

**問 8.4**（記述）

N+1 問題が**スロークエリログでは見つからない**理由を1行で書いてください。

**問 8.5**（記述）

次の2行は、どちらも f 文字列を使っています。
**一方は危険で、もう一方は安全です。** どちらが危険かを答え、その理由を1行で書いてください。

```python
# A
sql = text(f"SELECT id FROM tasks WHERE title LIKE '%{q}%'")

# B
rows = db.scalars(select(Task).where(Task.title.like(f"%{q}%"))).all()
```

**問 8.6**（記述）

`pool_pre_ping=True` が必要になるのは、MySQL 側のどの設定のためですか。
**設定の名前と、それが何をするか**を1行で書いてください。

**問 8.7**（記述）

`mysqldump` で取ったダンプを `SOURCE` で流し込む前に、
**必ず確かめるべきこと**を1つ挙げ、確かめるための SQL を1行で書いてください。

---

## 演習問題

この章の演習は、**`mysql-lesson`（MySQL 側）と `fastapi-lesson`（アプリ側）を行き来します。**
どちらで作業しているか、常に意識してください。

**始める前に、次の3つを確認してください。**

```sql
SELECT DATABASE(), USER();
SELECT COUNT(*) FROM tasks;
SELECT COUNT(*) FROM comments;
```

- `taskapp` に接続していること
- `tasks` が **3件以上**、`comments` が **6件**あること（8.2.2 の表のとおり入れた場合）

件数が違っても構いませんが、**解答編の数字とはずれます。**
自分の件数で読み替えてください。

### 演習 8.1 ★☆☆ 生成されたテーブルを SQL の目で読む

**課題**

`taskapp` の3つのテーブル（`tasks` / `comments` / `alembic_version`）を
`SHOW CREATE TABLE` で表示し、次の表を埋めてください。

| 調べること | 答え |
|-----------|------|
| `comments` の主キーの列と、自動採番されるか | |
| `comments` の外部キーの制約名と、参照先 | |
| `comments` に、自分で作っていないインデックスが何本あるか | |
| `tasks` の `done` 列の型 | |
| `alembic_version` の `version_num` 列の型と桁数 | |

そのうえで、**`comments` の `created_at` に値を入れずに `INSERT` してみて**、
何が入るかを確かめてください。

**完成条件**

- 表の5行すべてが埋まっている
- 外部キーの制約名が **`comments_ibfk_1`** のように、**自分が名付けていない名前**であることを確認した
- 自分で作っていないインデックスが **1本**（`KEY task_id`）で、
  それが**外部キーのために MySQL が自動で作ったもの**だと説明できる
- `done` の型が **`tinyint(1)`** である
- `INSERT INTO comments (task_id, body) VALUES (1, '型を確かめる');` を実行し、
  `created_at` に**実行した時刻**が入ったことを `SELECT` で確認した
- 確認が終わったら、入れた行を `DELETE` で消した（**`WHERE` を付けて**。4.2.3）

**ヒント**

表示は 8.1.2 ⑤ と 8.2.1 で使ったものと同じです。
`created_at` に何が入るかは、`SHOW CREATE TABLE` の `DEFAULT` の部分に書いてあります（5.2.2）。

---

### 演習 8.2 ★☆☆ N+1 を数えて、直ったことを数えて確かめる

**課題**

`GET /tasks`（一覧）と `GET /tasks/1`（1件）について、
**発行される `SELECT` の本数を MySQL 側で数えて**、次の表を埋めてください。

| 窓口 | `selectinload` 無し | `selectinload` 有り |
|------|-------------------|-------------------|
| `GET /tasks` | | |
| `GET /tasks/1` | | |

そのうえで、**`GET /tasks/1` は N+1 問題と呼べるか**を、理由を添えて1行で書いてください。

**完成条件**

- 4つのマスすべてが数字で埋まっている
- `GET /tasks` が **4本 → 2本**に減っている
- `GET /tasks/1` は、**`selectinload` の有無で本数が変わらない**ことを確認した
- 数えるたびに `TRUNCATE TABLE mysql.general_log;` を打ち、**前の回の記録が混ざっていない**
- 最後に **`SET GLOBAL general_log = OFF;` を打った**
- 「N+1 と呼べるか」の答えに、**タスクの件数が増えたときに本数が増えるか**という観点が入っている

**ヒント**

数え方は 8.3.2 の②です。`selectinload` を外した状態に戻すには、
8.3.3 で足した `.options(...)` の行を一時的にコメントアウトします。

`GET /tasks/1` の SQL の流れは、8.2.2 の最後のシーケンス図にあります。

---

### 演習 8.3 ★★☆ 危ない検索窓口を、自分で安全にする

**課題**

8.4.1 で作った `search-ng` を消す前に、**安全な版を自分で書いてください。**
ただし、8.4.3 の `search` をそのまま写すのではなく、**機能を1つ足します。**

**探す列を、`title` と `owner_name` から選べるようにしてください。**

```text
GET /tasks/search2?q=山田&field=owner_name
```

- `field` が `title` のときは `title` を探す
- `field` が `owner_name` のときは `owner_name` を探す
- `field` を省略したら `title` を探す
- **それ以外の値が来たら `422` で断る**

**完成条件**

- `?q=牛乳` で **1件**（`牛乳を買う`）返る
- `?q=山田&field=owner_name` で **2件**返る（`owner_name` が `山田` のタスク）
- `?q=山田&field=owner_email` で **`422`** が返る（許可していない列）
- `q` に `' OR '1'='1` を渡しても **0件**である
- `q` に `' UNION SELECT id, owner_email FROM tasks WHERE title LIKE '` を渡しても **0件**である
- **`q` の値が、SQL の文字列に一度も埋め込まれていない**（プレースホルダで渡している）
- 確認が終わったら、`search-ng` を**削除した**

**ヒント**

値と列名は、**扱い方が違います**（8.4.3 の「よくある間違い」）。
`q` はプレースホルダ、`field` は**許可リスト**です。
許可リストの書き方は、8.4.3 の `ALLOWED_ORDER` と同じ形になります。

`422` を返す方法は fastapi-text 5.4.1 の `HTTPException` です。

確かめるときは、**サーバー側のターミナルに出る SQL** も毎回見てください。
`q` の中身が SQL に現れていたら、まだ安全ではありません。

---

### 演習 8.4 ★★☆ 事故からの復旧を、通しで1回やる

**課題**

`taskapp` のダンプを取り、**わざと事故を起こして、戻してください。**

1. `taskapp` のダンプを `/sql/taskapp_backup.sql` に取る
2. ダンプを開き、**`comments` の外部キー制約が入っている**ことを確認する
3. `tasks` と `comments` の件数を記録する
4. **`DELETE FROM tasks WHERE id = 3;`** を実行する
5. `tasks` と `comments` の件数をもう一度数え、**なぜコメントも減ったのか**を1行で書く
6. ダンプから戻し、件数が3の状態に戻ったことを確認する

**完成条件**

- `mysql-lesson/sql/taskapp_backup.sql` ができていて、VS Code で中身が読める
- ダンプの中に **`CONSTRAINT ... FOREIGN KEY ... ON DELETE CASCADE`** の行がある
- 4のあと、`tasks` が **1件減り**、`comments` が **3件減っている**
- 5の説明に、**`ON DELETE CASCADE`** という語が入っている
- 6のあと、`tasks` と `comments` の件数が、3で記録した値と**同じ**になっている
- 戻す前に **`SELECT DATABASE();` で `taskapp` にいることを確認した**

**ヒント**

ダンプの取り方は 8.6.1、戻し方は 8.6.2 です。**データベース名を `shop` から変えるだけ**です。
5の理由は 8.2.1 の表にあります。`SHOW CREATE TABLE comments` で確かめられます。

**`shop` のダンプと同じファイル名にしないでください。** 上書きしてしまいます。

---

### 演習 8.5 ★★☆ プールを小さくして、枯渇するところを見る

**課題**

`pool_size` と `max_overflow` を小さくして、**接続が足りなくなる瞬間**を再現してください。

1. `pool_size=2` / `max_overflow=0` / `pool_timeout=3` のエンジンを作る
2. 接続を **2本借りて、返さずに持っておく**
3. **3本目を借りようとして**、何が起きるか記録する
4. `MySQL` 側で `SHOW STATUS LIKE 'Threads_connected';` を見て、本数を確認する
5. **`max_overflow=1` に変えるとどうなるか**を確かめ、違いを1行で書く

**完成条件**

- 2のあと、`engine.pool.status()` が
  **`Checked out connections: 2`** と表示される
- 3で **`TimeoutError`** が発生し、メッセージに
  **`QueuePool limit of size 2 overflow 0 reached`** が含まれている
- 3のエラーが、**約3秒待ってから**起きたことを、時間を測って確認した
- 5で、`max_overflow=1` にすると **3本目は借りられる**ことを確認した
- 5の説明に、**`pool_size` と `max_overflow` の合計が上限**であることが書かれている

**ヒント**

エンジンの作り方と `status()` の読み方は 8.5.2 です。
`create_engine` に渡せる設定は、8.5.2 の表にあります。

時間を測るには `time.perf_counter()` を使います（8.5.1 の `check_pool.py` と同じ形です）。

**借りた接続は、最後に必ず `close()` してください。** 忘れると次の実験に影響します。

---

解答は [解答編 その2](./91-answers-part2.md#第8章) にあります。
**必ず自分で手を動かしてから**見てください。

---

## 次の章へ

この章で、**アプリとデータベースのあいだ**を扱いました。

| できるようになったこと | 使う道具 |
|--------------------|---------|
| アプリ専用のユーザーを作る | `CREATE USER` / `GRANT`（8.1.1） |
| 接続先を MySQL に切り替える | 接続 URL 1行と PyMySQL（8.1.2） |
| テーブル同士を関連づける | `ForeignKey` と `relationship`（8.2.1） |
| 発行された SQL の本数を数える | `sqlalchemy.engine` のログ / `general_log`（8.3.2） |
| N+1 を直す | `selectinload`（8.3.3） |
| インジェクションを防ぐ | プレースホルダと許可リスト（8.4.3） |
| 接続の数を見積もる | `pool_size` / `max_connections`（8.5.2） |
| 事故から戻す | `mysqldump` と `SOURCE`（8.6） |

**そして、ここが5冊の合流点でした。**

第7章の終わりで「`db.scalars(select(Task))` の中身が読める」と書きました。
いまは、それだけではありません。

- その SQL が**何本**飛んでいるかを数えられます（8.3.2）
- その SQL が**速いか**を `EXPLAIN` で確かめられます（7.4.2）
- そのテーブルが**どういう形**かを `SHOW CREATE TABLE` で読めます（8.1.2）
- 事故のあと**戻せます**（8.6.2）

react-text で画面を作り、python-text で言葉を覚え、fastapi-text で API を書き、
docker-text で配れるようにして、mysql-text でデータの置き場所を自分で設計しました。

**次の章は、この5冊の締めです。**
新しい文法は出てきません。代わりに、

- ここまでで身についたことの**到達度チェック**
- 自分の実力を**測る方法**
- これから何を学ぶか（**データベース設計・ネットワーク・設計原則・クラウド**）

を扱います。

→ [第9章 次のステップ](./09-next-steps.md) に進む
