---
title: "第6章 データベース連携"
---

# 第6章 データベース連携

第2章からずっと、同じただし書きを付けてきました。

> 練習用のデータ。サーバーを止めると消える（保存する方法は第6章）

その第6章です。

いまの `app/data.py` の `tasks` は、ただの Python のリストです。
`POST /tasks` で登録したタスクは、**サーバーを `Ctrl` + `C` で止めた瞬間に消えます。**
これでは、他の人に使ってもらうアプリにはなりません。

この章では、データを**データベース**（データを整理して保存し、
必要なものを素早く取り出せるようにした仕組み）に移します。
サーバーを何度再起動しても、登録したタスクが残るようになります。

## この章で学ぶこと

- データがメモリの上にある状態と、ファイルに保存されている状態の違いを説明できるようになる
- SQLAlchemy を使って、Python のクラスとデータベースのテーブルを対応づけられるようになる
- 一覧・1件取得・登録・更新・削除の5つを、すべてデータベース越しに書けるようになる
- `Depends` で1リクエストに1つのセッションを配り、確実に閉じられるようになる
- テーブルの構造を変えたくなったとき、**マイグレーション**で安全に変更できるようになる

## この章の前提

- [第5章](./05-project-structure.md) を読み終え、`fastapi-lesson/app/` が第5章の最終形（5.6.2）になっていること
- 第5章の演習（メモの窓口）を解き終えていること
- python-text の第8章（クラス・インスタンス・属性・継承）を読み終えていること
- python-text の第9章（型ヒント・`Mapped[int]` のような書き方の読み方）を読み終えていること

> **注意**
> この章は**データベースの入門ではありません。**
> SQL（データベースに命令を出すための言語）そのものは、5冊目の mysql-text で扱います。
> この章では、**SQL を書かずにデータベースを使う道具**を通して、
> 「保存されるとはどういうことか」を先に体験します。

> **つまずいたら**
> この章で最も多いのは、**「登録したのに、次に見たら消えている」**という詰まり方です。
> 原因はほぼ `commit` の書き忘れ（6.5.2）です。
> 第0章の 0.2 で準備した AI には、次の3つを添えて聞いてください。
>
> ```text
> fastapi-text の 6.4.1 を読んでいます。
> POST /tasks は 201 が返るのに、GET /tasks に出てきません。
> ・app/routers/tasks.py の create_task の全体
> ・サーバーのターミナルの最後の10行
> ・app.db がどこにあるか（ls / dir の結果）
> ```

---

## 6.1 データを永続化する

### 6.1.1 サーバーを止めるとデータが消える

まず、いま何が起きているのかを、目で見て確認します。

サーバーを起動して（`fastapi dev app/main.py`）、`/docs` から `POST /tasks` を1件実行してください。

```json
{
  "title": "郵便を出す",
  "owner": {"name": "山田", "email": "yamada@example.com"}
}
```

`GET /tasks` を見ると、4件になっています。

```json
{"count":4,"tasks":[...]}
```

ここで、サーバーのターミナルで `Ctrl` + `C` を押して**止めて**ください。
もう一度 `fastapi dev app/main.py` で起動し、`GET /tasks` を開きます。

```json
{"count":3,"tasks":[...]}
```

**3件に戻りました。** 登録したタスクは消えています。

なぜかというと、`app/data.py` の `tasks` は**メモリ**（プログラムが動いている間だけ
値を置いておく場所）の上にあるからです。
プログラムが終わると、メモリの中身は解放されます。

```mermaid
flowchart LR
    A["起動"] --> B["app/data.py の tasks が<br/>メモリに読み込まれる"]
    B --> C["append で増える<br/>（メモリの中だけ）"]
    C --> D["Ctrl + C で停止"]
    D --> E["メモリが解放され<br/>増えた分は消える"]
    E --> A
```

データを、プログラムが終わっても残る場所——**ディスク**（電源を切っても消えない保存領域）——に
置くことを、**永続化**（えいぞくか。プログラムが終わってもデータが残るようにすること）と呼びます。

永続化のやり方は、大きく2つあります。

| やり方 | 内容 | 向いていること |
|-------|------|--------------|
| ファイルに書く | JSON や CSV として保存する（python-text 第7章） | 数十件までの小さなデータ |
| データベースを使う | 専用の仕組みに保存を任せる | 件数が多い・検索する・同時に複数人が使う |

このテキストではデータベースを使います。理由は3つです。

- **一部だけ**を素早く取り出せる（「山田さんの、終わっていないタスク」だけを読む）
- **同時に2人が書き込んでも壊れない**ようにする仕組み（トランザクション）が最初からある
- **件数が増えても遅くならない**（ファイルは全部読み直すことになります）

> **補足**
> 「ファイルに書く」が悪いわけではありません。
> 設定ファイルや、1回書いたら読むだけのデータなら、JSON で十分です。
> **書き換えが頻繁で、条件で絞り込みたいもの**がデータベース向きです。

### 6.1.2 このテキストでの進め方（SQLite → MySQL）

データベースにはいくつも種類があります。このテキストで使うのは次の2つです。

| 名前 | 形 | 使う場面 |
|------|---|---------|
| **SQLite**（エスキューライト） | **ファイル1つ**。専用のソフトを立ち上げなくていい | この章〜第9章の練習 |
| **MySQL**（マイエスキューエル） | **サーバー**。別のプログラムとして常に動かしておく | 5冊目の mysql-text、4冊目の docker-text |

第2章で `fastapi dev` を動かしっぱなしにする必要があったのと同じで、
MySQL は「もう1つ動かしっぱなしにするサーバー」が増えます。
いまの段階でそれを足すと、**確認することが一度に増えすぎます。**

そこで、この本では **SQLite** で進めます。
`app.db` というファイルが1つできるだけで、起動するものは増えません。

```mermaid
flowchart LR
    A["この章〜第9章<br/>SQLite（ファイル1つ）"] --> B["4冊目 docker-text<br/>MySQL をコンテナで動かす"]
    B --> C["5冊目 mysql-text<br/>SQL とテーブル設計"]
```

**あとで MySQL に乗り換えられるように書きます。**
この章で使う SQLAlchemy（6.2.1）は、**接続先の文字列を1行変えるだけ**で
SQLite と MySQL を切り替えられる作りになっています。
そのため、この章では接続先を `.env` に置きます（6.2.2）。

> **注意**
> SQLite と MySQL は、できることが完全に同じではありません。
> このテキストの範囲では違いが表に出ないように書いていますが、
> 「SQLite で動いたから MySQL でも必ず動く」とは言えません。
> 実際の乗り換えは docker-text で扱います。

---

## 6.2 SQLAlchemy の基本

### 6.2.1 ORM とは

データベースに命令を出す言語は **SQL** です。たとえば、こう書きます。

```sql
SELECT * FROM tasks WHERE done = 0;
```

SQL は5冊目で本格的に学びます。この章の時点では、まだ読み書きできません。

そこで使うのが **ORM**（オーアールエム。データベースの行を、
プログラムのオブジェクトとして扱えるようにする仕組み）です。
Python で最も広く使われている ORM が **SQLAlchemy**（エスキューエルアルケミー）です。

ORM を使うと、さきほどの SQL は次のように書けます。

```python
db.scalars(select(Task).where(Task.done == False)).all()
```

**Python のコードのまま**、データベースを読み書きできます。

| 手で SQL を書く | ORM を使う |
|---------------|-----------|
| `SELECT * FROM tasks` | `select(Task)` |
| `WHERE done = 0` | `.where(Task.done == False)` |
| 結果は**タプルのリスト** | 結果は **`Task` オブジェクトのリスト** |
| 列名の打ち間違いは**実行するまで**分からない | `Task.dnoe` と書けばエディタが指摘してくれる |

`Task` は、これから 6.3 で自分で書くクラスです。
**1つのクラスが1つのテーブルに、1つのインスタンスが1つの行に対応します。**

```mermaid
flowchart LR
    A["窓口の関数<br/>（Python）"] --> B["SQLAlchemy（ORM）"]
    B -->|"SQL に翻訳"| C["SQLite<br/>app.db"]
    C -->|"行を返す"| B
    B -->|"Task オブジェクトに変換"| A
```

> **補足：ORM を使えば SQL を知らなくていい、ではありません**
> 複雑な集計や、遅いときの原因調べは、結局 SQL を読むことになります。
> このテキストの並び（fastapi-text → mysql-text）は、
> **先に ORM で動かして体感し、あとで SQL をきちんと学ぶ**という順番です。
> ORM が組み立てた SQL を実際に目で見る方法は、6.2.3 の補足で紹介します。

### 6.2.2 インストールと接続設定

SQLAlchemy を入れます。**`fastapi-lesson` で、仮想環境を有効にした状態**で実行してください
（有効化のしかたは 2.2.1 です）。

**Windows（PowerShell）**

```powershell
pip install sqlalchemy==2.0.36
```

**macOS / Linux**

```bash
pip install sqlalchemy==2.0.36
```

```text
Successfully installed sqlalchemy-2.0.36
```

入ったことを確認します。

**Windows（PowerShell）**

```powershell
pip list | Select-String sqlalchemy
```

**macOS / Linux**

```bash
pip list | grep -i sqlalchemy
```

```text
SQLAlchemy        2.0.36
```

記録も更新しておきます（2.2.3）。

**Windows（PowerShell）**

```powershell
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip freeze > requirements.txt
```

次に、**接続先**を設定に足します。
データベースの場所は「どのパソコンで動かすか」で変わるので、
コードに直接書かず `.env` から読みます（4.6.2）。

`app/config.py`（`debug` の下に1行足す）

```diff
      app_name: str = "タスク管理 API"
      debug: bool = False
+     database_url: str = "sqlite:///./app.db"
```

`.env` にも書きます。

`.env`（1行追記）

```text
DATABASE_URL=sqlite:///./app.db
```

`.env.example` にも、**値を伏せずに**同じ行を書いておきます
（この値は秘密ではないためです。秘密の値の扱いは 4.6.3）。

`.env.example`（1行追記）

```text
DATABASE_URL=sqlite:///./app.db
```

この `sqlite:///./app.db` という文字列を、**接続 URL** と呼びます。読み方はこうです。

| 部分 | 意味 |
|------|------|
| `sqlite` | どの種類のデータベースか |
| `://` | 区切り |
| `/./app.db` | **どこにあるか。** ここでは「起動したときの現在地の `app.db`」 |

MySQL に乗り換えるときは、この1行が
`mysql+pymysql://user:password@localhost:3306/tasks` のような形に変わります。
**変えるのはこの行だけ**で、6.3 以降に書くコードは変わりません。

> **注意**
> `sqlite:///./app.db` の**スラッシュは3本＋`./`** です。数を間違えると接続できません。
> また、`.env` は**起動したときの現在地から探されます**（5.1.2）。
> `fastapi-lesson` で起動していれば、`app.db` も `fastapi-lesson` の直下にできます。

> **注意：`app.db` は共有しない**
> `app.db` には、登録したデータが**そのまま**入っています。
> `.env` と同じで（4.6.3）、他の人に渡すファイルには含めません。
> Git を使う場合は、`.gitignore` に `app.db` と `.env` の2行を書いておいてください。

### 6.2.3 エンジンとセッション

SQLAlchemy を使うには、部品を3つ用意します。

| 部品 | 役割 | いくつ作るか |
|------|------|------------|
| **エンジン** | データベースへの接続口。接続 URL を持つ | アプリ全体で**1つ** |
| **セッション** | 読み書きの作業台。**1回の仕事**をここで進める | **1リクエストにつき1つ** |
| **`Base`** | テーブルに対応するクラスの共通の親 | アプリ全体で**1つ** |

たとえるなら、エンジンは**店の入口**、セッションは**レジ1回分の会計**です。
入口は店に1つですが、会計はお客さんごとに始めて、終わったら閉じます。

`app/database.py` を新しく作ってください。

`app/database.py`（ファイル全体）

```python
"""データベースへの接続をまとめる場所。"""

from sqlalchemy import create_engine
from sqlalchemy.orm import DeclarativeBase, sessionmaker

from app.config import settings

# データベースへの接続口。アプリ全体で1つだけ作る
engine = create_engine(
    settings.database_url,
    # SQLite のときだけ必要な設定（下の補足）
    connect_args={"check_same_thread": False},
)

# セッションを作るための工場
SessionLocal = sessionmaker(bind=engine, autoflush=False)


class Base(DeclarativeBase):
    """テーブルに対応するクラスの、共通の親。"""
```

3つを順に見ます。

**`create_engine(...)`**

接続先を渡して、エンジンを作ります。
**この時点ではまだ接続していません。** 実際に繋がるのは、最初に読み書きしたときです。

**`sessionmaker(bind=engine, autoflush=False)`**

`SessionLocal` は、**セッションそのものではなく、セッションを作る工場**です。
`SessionLocal()` と呼ぶと、新しいセッションが1つできます。

`bind=engine` は「この工場が作るセッションは、このエンジンを使う」という指定です。
`autoflush=False` は「勝手に書き込みに行かない」という指定で、
**いつデータベースに書かれたかを、自分で把握できる**ようにするために付けます（6.5.2）。

**`class Base(DeclarativeBase):`**

これから作る `Task` クラスの**親**です（python-text 8.3.2 の継承）。
`Base` を継承したクラスを SQLAlchemy が覚えていて、
「どんなテーブルを作ればいいか」の一覧（`Base.metadata`）を組み立てます。

```mermaid
flowchart TD
    C["app/config.py<br/>DATABASE_URL"] --> E["engine<br/>接続口（1つ）"]
    E --> S["SessionLocal<br/>セッションを作る工場"]
    S --> S1["セッション<br/>リクエストA 用"]
    S --> S2["セッション<br/>リクエストB 用"]
    B["Base<br/>テーブル定義の登録先"] --> M["app/models.py の Task"]
```

> **補足：`check_same_thread` とは**
> SQLite には「作ったときと違うスレッドから触ってはいけない」という決まりがあります。
> ところが FastAPI は、`def` で書いた窓口の関数を**別のスレッドで動かします。**
> そのままだと `SQLite objects created in a thread can only be used in that same thread`
> というエラーになるので、この決まりを外しています。
> **SQLite のときだけ必要な設定**で、MySQL に乗り換えるときは消します。

> **補足：ORM が組み立てた SQL を見たいとき**
> `create_engine(..., echo=True)` を足すと、
> 実行された SQL がそのままターミナルに出ます。
>
> ```text
> INFO sqlalchemy.engine.Engine SELECT tasks.id, tasks.title, ... FROM tasks
> ```
>
> 「この Python のコードは、どんな SQL になるのか」を知りたいときに使ってください。
> 量が多いので、普段は付けません。

---

## 6.3 モデルを定義する

### 6.3.1 テーブルとクラスの対応

データベースは、データを**テーブル**（行と列からなる表）の形で持ちます。
用語を3つ、ここで揃えます。

| 用語 | 意味 | 表計算ソフトでいうと |
|------|------|-------------------|
| **テーブル** | 1種類のデータを入れる表。名前を持つ（`tasks`） | シート1枚 |
| **行**（レコード） | 1件分のデータ | 1行 |
| **列**（カラム） | 項目。名前と型を持つ（`title` は文字列） | 1列 |

ORM は、これを Python の側にそのまま写します。

| データベース | Python |
|------------|--------|
| テーブル `tasks` | クラス `Task` |
| 行1つ | インスタンス1つ |
| 列 `title` | 属性 `task.title` |

`app/models.py` を新しく作ります。

`app/models.py`（ファイル全体）

```python
"""データベースのテーブルに対応するクラス。"""

from sqlalchemy import JSON, String
from sqlalchemy.orm import Mapped, mapped_column

from app.database import Base


class Task(Base):
    """tasks テーブルの1行。"""

    __tablename__ = "tasks"

    id: Mapped[int] = mapped_column(primary_key=True)
    title: Mapped[str] = mapped_column(String(20))
    done: Mapped[bool] = mapped_column(default=False)
    code: Mapped[str | None] = mapped_column(String(10), unique=True, default=None)
    priority: Mapped[int] = mapped_column(default=3)
    tags: Mapped[list[str]] = mapped_column(JSON, default=list)
    owner_name: Mapped[str] = mapped_column(String(20))
    owner_email: Mapped[str] = mapped_column(String(100))
```

`__tablename__` は、**対応するテーブルの名前**です。これだけは必ず書きます。

各行の `名前: Mapped[型] = mapped_column(...)` が、**1つの列**です。
`Mapped[int]` は python-text 第9章で学んだ型ヒントの一種で、
SQLAlchemy はこれを読んで「この列は整数だ」と判断します。

テーブルを実際に作ります。**この章の間だけ使う、使い捨ての小さなスクリプト**を書きます。

`create_tables.py`（`fastapi-lesson` の直下。ファイル全体）

```python
"""テーブルを作る（6.3.1 の一時的な方法。6.6 で Alembic に置き換える）。"""

from app.database import Base, engine
from app.models import Task  # noqa: F401  Base に Task を登録するための import

Base.metadata.create_all(bind=engine)
print("テーブルを作りました。")
```

`Base.metadata.create_all(...)` は、**`Base` を継承したクラスのぶんだけ、
まだ無いテーブルを作る**メソッドです。

`app/models.py` を `import` しているのは、**`Task` を読み込ませるため**です。
読み込まないと `Base` は `Task` の存在を知らないままで、テーブルが1つも作られません。

実行します。**`fastapi-lesson` で**実行してください。

**Windows（PowerShell）**

```powershell
python create_tables.py
```

**macOS / Linux**

```bash
python create_tables.py
```

```text
テーブルを作りました。
```

`fastapi-lesson` の中に **`app.db` というファイルができています。**
VS Code のファイル一覧で確認してください。中身は開いても読めない形式ですが、それで正常です。

> **よくある間違い**
> **`app` の中で `python create_tables.py` を実行する**間違いです。
> `.env` の探し方と同じで（5.1.2）、`app.db` は**実行したときの現在地**にできます。
> `app/app.db` ができていたら、それを削除して、`fastapi-lesson` でやり直してください。
> `ModuleNotFoundError: No module named 'app'` が出た場合も原因は同じです。

### 6.3.2 カラムの型と制約

`mapped_column(...)` に渡すもので、その列の**型**と**制約**（守らせる決まり）が決まります。
6.3.1 で書いた `Task` を、1行ずつ読みます。

| 書いたもの | 意味 |
|-----------|------|
| `Mapped[int] = mapped_column(primary_key=True)` | **主キー**（その行を1つに特定できる列）。整数の主キーは**自動で 1, 2, 3 …と振られる** |
| `Mapped[str] = mapped_column(String(20))` | 文字列。**最大 20 文字**まで |
| `Mapped[bool] = mapped_column(default=False)` | 真偽値。**値を指定しなければ `False`** |
| `Mapped[str \| None]` | **`null` を入れてよい**列（`\| None` が無い列は、空にできない） |
| `mapped_column(unique=True)` | **同じ値の行を2つ作れない**。管理番号の重複を、データベース側でも止める |
| `Mapped[list[str]] = mapped_column(JSON, default=list)` | リストは列にそのまま入らないので、**JSON の文字列**として保存する |

**`String(20)` の数字**は、`title` の `Field(max_length=20)`（4.3.2）と合わせています。
Pydantic 側だけで検査すると、`/docs` を通さずにデータベースへ直接書かれたときに素通りします。
**両方に書いて二重に守る**のが基本です。

**`| None` の有無**が、そのまま「空を許すかどうか」になります。
`owner_name: Mapped[str]` には `| None` が無いので、名前の無いタスクは作れません。

**`default=list`** は、`default=[]` ではないことに注意してください。
`list` と書くと「**行を作るたびに、新しい空のリストを呼び出して使う**」という意味になります。
`[]` と書くと1つのリストを全部の行で共有してしまいます（python-text 5.2.4 と同じ話です）。

作られたテーブルの形を、実際に見てみます。

**Windows（PowerShell）**

```powershell
python -c "import sqlite3; print(sqlite3.connect('app.db').execute(\"select sql from sqlite_master where name='tasks'\").fetchone()[0])"
```

**macOS / Linux**

```bash
python -c "import sqlite3; print(sqlite3.connect('app.db').execute(\"select sql from sqlite_master where name='tasks'\").fetchone()[0])"
```

```text
CREATE TABLE tasks (
	id INTEGER NOT NULL,
	title VARCHAR(20) NOT NULL,
	done BOOLEAN NOT NULL,
	code VARCHAR(10),
	priority INTEGER NOT NULL,
	tags JSON NOT NULL,
	owner_name VARCHAR(20) NOT NULL,
	owner_email VARCHAR(100) NOT NULL,
	PRIMARY KEY (id),
	UNIQUE (code)
)
```

これが、SQLAlchemy が組み立てた SQL です。**書いた覚えのある言葉が並んでいます。**

- `VARCHAR(20)` が `String(20)`
- `NOT NULL` が「`| None` が無い列」
- `UNIQUE (code)` が `unique=True`

> **よくある間違い**
> **`app/models.py` を書き換えても、テーブルは変わりません。**
> `create_all` は「**まだ無いテーブルを作る**」だけで、
> すでにあるテーブルの列を増やしたりはしません。
>
> 列を1つ足して `python create_tables.py` をもう一度実行しても、
> エラーも出ないまま**何も起きません。**
> そのあと窓口を呼ぶと `no such column: tasks.○○` で `500` になります。
>
> これを正しく扱うのが、**6.6 のマイグレーション**です。
> この章では、6.6 に進むまで `app/models.py` の列を増やさないでください。

### 6.3.3 Pydantic モデルとの違い

ここで、紛らわしいものが2つ揃いました。

| | `app/schemas.py`（Pydantic） | `app/models.py`（SQLAlchemy） |
|---|---|---|
| 何のためのもの | **外とやりとりする形** | **保存する形** |
| 親クラス | `BaseModel` | `Base` |
| 決めていること | 受け取ってよい値・返す値 | 列の型・制約 |
| 例 | `TaskCreate` / `TaskRead` | `Task` |
| 変えると影響するもの | `/docs` の表示・`422` の内容 | **テーブルの形**（6.6 が必要） |

**どちらも「モデル」と呼ばれます。** このテキストでは、次のように呼び分けます。

- Pydantic のほう →「**スキーマ**」または「Pydantic のモデル」
- SQLAlchemy のほう →「**モデル**」または「テーブルのモデル」

```mermaid
flowchart LR
    A["リクエストの JSON"] --> B["TaskCreate<br/>（スキーマ）"]
    B --> C["Task<br/>（テーブルのモデル）"]
    C --> D["app.db"]
    D --> C2["Task"]
    C2 --> E["TaskRead<br/>（スキーマ）"]
    E --> F["レスポンスの JSON"]
```

**分けている理由は、4.4.2 と同じです。**
`Task` には `owner_email` があります。**これは返してはいけません。**
`TaskRead` に `owner_email` を書かないことで、外に出ないようにします。

ただし、そのままでは1つ足りません。
`TaskRead` は `owner: OwnerRead`（`name` を持つ入れ子）を期待していますが、
`Task` が持っているのは `owner_name` と `owner_email` という**2つの平らな列**です。

そこで `Task` に、**組み立て直すための属性**を1つ足します。

`app/models.py`（`Task` クラスの末尾に追記）

```python
    @property
    def owner(self) -> dict:
        """schemas.OwnerRead に渡すための形に組み立てる。"""
        return {"name": self.owner_name, "email": self.owner_email}
```

**`@property`**（プロパティ。メソッドを、属性のように括弧なしで呼べるようにする印）は、
ここで初めて出てきます。

```python
task.owner        # ✅ @property を付けたので、括弧なしで呼べる
task.owner()      # ❌ @property を付けると、この書き方はできなくなる
```

中身はメソッドですが、外からは `task.title` などの列と**同じ見た目**で読めます。
`TaskRead` は「`owner` という項目がある」としか知らないので、
それが列なのか組み立てた結果なのかを、気にせずに済みます。

これで `task.owner` と書けば `{"name": "山田", "email": "..."}` が返ります。
`TaskRead` の `owner: OwnerRead` は `name` しか受け取らないので、`email` は落ちます。

最後に、`TaskRead` に**1行だけ**足します。

`app/schemas.py`（`TaskRead` の中）

```diff
  class TaskRead(TaskBase):
      """返すときの形。email を持たない。"""
  
+     model_config = ConfigDict(from_attributes=True)
+ 
      id: int
      owner: OwnerRead
```

```diff
- from pydantic import BaseModel, Field, field_validator
+ from pydantic import BaseModel, ConfigDict, Field, field_validator
```

`from_attributes=True` は、
**「辞書ではなく、オブジェクトの属性から値を読んでよい」**という設定です。
これが無いと、`Task` オブジェクトを `TaskRead` に渡したときに
`Input should be a valid dictionary` という `500` になります。

**テーブルのモデルを返す窓口の `response_model` には、これが要る**と覚えてください。

練習用のデータを、データベースに入れておきます。

`app/seed.py`（ファイル全体）

```python
"""練習用のデータをデータベースに入れる。

python -m app.seed で実行する。
"""

from sqlalchemy import select

from app.database import SessionLocal
from app.models import Task

def initial_tasks() -> list[Task]:
    """最初に入れておくタスクを組み立てて返す。"""
    return [
        Task(title="牛乳を買う", done=False,
             owner_name="山田", owner_email="yamada@example.com"),
        Task(title="レポートを書く", done=True,
             owner_name="鈴木", owner_email="suzuki@example.com"),
        Task(title="部屋を片づける", done=False,
             owner_name="山田", owner_email="yamada@example.com"),
    ]


def main() -> None:
    db = SessionLocal()
    try:
        if db.scalar(select(Task)) is not None:
            print("すでにデータが入っているので、何もしませんでした。")
            return
        tasks = initial_tasks()
        for task in tasks:
            db.add(task)
        db.commit()
        print(f"{len(tasks)} 件のタスクを登録しました。")
    finally:
        db.close()


if __name__ == "__main__":
    main()
```

`if __name__ == "__main__":` は python-text 6.4.1 の「**直接実行したときだけ動かす**」書き方です。

`db.scalar(select(Task))` は「`tasks` から1件だけ取り出す」という意味で、
1件でもあれば `Task` が、空なら `None` が返ります。
これで「もう入っているかどうか」を判定しています。

実行します。**`fastapi-lesson` で**実行してください。

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

もう一度実行すると、こうなります。

```text
すでにデータが入っているので、何もしませんでした。
```

**何度実行しても、データが3件ずつ増えたりしません。**
このように「何度実行しても結果が同じ」ことは、準備用のスクリプトでは大事な性質です。

---

## 6.4 CRUD を実装する

データの基本操作4つは、頭文字を取って **CRUD**（クラッド）と呼ばれます。

| 頭文字 | 操作 | HTTP メソッド |
|-------|------|-------------|
| **C**reate | 作る | `POST` |
| **R**ead | 読む | `GET` |
| **U**pdate | 変える | `PATCH` |
| **D**elete | 消す | `DELETE` |

第3章〜第5章で作った窓口を、リストからデータベースに置き換えていきます。
**URL もレスポンスの形も変えません。** 中身だけを差し替えます。

### 6.4.1 作成（Create）

まず、**セッションを窓口に配る依存**を用意します。
`app/dependencies.py` の先頭に追記してください。

`app/dependencies.py`（先頭部分）

```python
"""窓口どうしで共通して使う部品。"""

from collections.abc import Iterator

from fastapi import Depends, HTTPException, Path, Query
from sqlalchemy.orm import Session

from app.database import SessionLocal
from app.models import Task


def get_db() -> Iterator[Session]:
    """1リクエストにつき1つ、セッションを作って配る。"""
    db = SessionLocal()
    try:
        yield db
    finally:
        # 窓口の関数が終わったら、必ず閉じる
        db.close()
```

`return` ではなく **`yield`** を使っています。
「ここで値を渡して、窓口の関数が終わったら続きを実行する」という意味です。
**この形の詳しい説明は 6.5.1 で行います。**
いまは「セッションを配って、終わったら閉じる係」と読んでおいてください。

戻り値の型ヒント `Iterator[Session]` は、
**「`yield` で `Session` を1つ渡す関数」**という意味です（python-text 第9章の型ヒント）。
書かなくても動きますが、`return` ではないことが読んだ人に伝わります。

これを使って、`create_task` を書き換えます。

`app/routers/tasks.py`（`create_task` の部分）

```python
@router.post("", response_model=TaskRead, status_code=201)
def create_task(new_task: TaskCreate, db: Session = Depends(get_db)):
    if new_task.code is not None:
        found = db.scalar(select(Task).where(Task.code == new_task.code))
        if found is not None:
            raise DuplicateCodeError(new_task.code)

    task = Task(
        title=new_task.title,
        done=new_task.done,
        code=new_task.code,
        priority=new_task.priority,
        tags=new_task.tags,
        owner_name=new_task.owner.name,
        owner_email=new_task.owner.email,
    )
    db.add(task)
    db.commit()
    db.refresh(task)
    logger.info("タスクを登録しました id=%s title=%s", task.id, task.title)
    return task
```

登録は3手順です。

| 手順 | 意味 |
|------|------|
| `db.add(task)` | セッションに「これを保存したい」と**申し出る**。まだ書かれていない |
| `db.commit()` | **確定する。** ここで初めてファイルに書かれる |
| `db.refresh(task)` | 書かれた結果を**読み直す。** 自動で振られた `id` が入る |

**`db.refresh(task)` を忘れると、`task.id` が `None` のまま**で、
`TaskRead` に `id` が入らず `500` になります。
「**登録したものをそのまま返すなら `refresh`**」と覚えてください。

重複の確認は、リストの `for` から `select` に変わりました。

```python
found = db.scalar(select(Task).where(Task.code == new_task.code))
```

| 部分 | 意味 |
|------|------|
| `select(Task)` | `tasks` テーブルから `Task` を取り出す、という指示を組み立てる |
| `.where(Task.code == ...)` | 条件を足す |
| `db.scalar(...)` | 実行して、**1件だけ**返す（無ければ `None`） |

`select(...)` を書いただけでは**何も起きません。**
`db.scalar(...)` や `db.scalars(...)`（6.4.2）に渡して、初めて実行されます。

`import` を足します。

`app/routers/tasks.py`（先頭）

```diff
  from fastapi import APIRouter, Depends, Query
+ from sqlalchemy import select
+ from sqlalchemy.orm import Session
  
- from app.data import tasks
- from app.dependencies import get_task_or_404, get_visible_tasks, list_params
+ from app.dependencies import get_db, get_task_or_404, list_params
  from app.errors import DuplicateCodeError
+ from app.models import Task
  from app.schemas import TaskCreate, TaskListResponse, TaskRead, TaskUpdate
```

**まだ他の窓口が `tasks`（リスト）を使っているので、この時点ではサーバーが起動しません。**
6.4.2 まで書き進めてから動かしてください。

> **よくある間違い**
> **`db.commit()` を書き忘れる**間違いです。
> `201` は返り、レスポンスにも中身が入るので、**成功したように見えます。**
> ところが `GET /tasks` には出てきません。セッションが閉じるときに、
> 確定していない変更は**捨てられる**からです（6.5.2）。
>
> 「登録できたはずなのに一覧に出ない」ときは、**まず `commit` を探してください。**

### 6.4.2 一覧と1件取得（Read）

**1件取得**から書き換えます。`get_task_or_404`（5.4.1）を、データベースを見る形にします。

`app/dependencies.py`（`get_task_or_404` を差し替え）

```python
def get_task_or_404(
    task_id: int = Path(ge=1),
    db: Session = Depends(get_db),
) -> Task:
    """id でタスクを探す。見つからなければ 404 で止める。"""
    task = db.get(Task, task_id)
    if task is None:
        raise HTTPException(
            status_code=404,
            detail=f"id {task_id} のタスクは見つかりませんでした",
        )
    return task
```

**`db.get(Task, task_id)` は「主キーで1件引く」専用のメソッド**です。
`select` を組み立てるより短く書けます。見つからなければ `None` が返ります。

**依存が、別の依存（`get_db`）を要求している**ことに注目してください。
5.3.3 で学んだ入れ子の依存が、そのまま役に立っています。

窓口側は、`dict` を受け取っていたところが `Task` に変わるだけです。

`app/routers/tasks.py`（`read_task` の部分）

```diff
  @router.get("/{task_id}", response_model=TaskRead)
- def read_task(task: dict = Depends(get_task_or_404)):
+ def read_task(task: Task = Depends(get_task_or_404)):
      return task
```

次に**一覧**です。5.3.3 で作った `get_visible_tasks` は、ここで役目を終えます。

```python
def get_visible_tasks(owner: str | None = Depends(get_owner_name)) -> list[dict]:
    if owner is None:
        return tasks
    return [task for task in tasks if task["owner"]["name"] == owner]
```

**この形は、データベースには向きません。**
全件を Python 側に読み込んでから絞り込むことになるからです。
1万件あるうちの3件が欲しいときも、1万件を運ぶことになります。

**絞り込みは、データベースにやらせます。**
`app/dependencies.py` から `get_visible_tasks` を**削除**し、
`get_owner_name` はそのまま残してください（名前を取り出す係としては変わらず有用です）。

`app/routers/tasks.py`（`read_tasks` の部分）

```python
@router.get("", response_model=TaskListResponse)
def read_tasks(
    params: dict = Depends(list_params),
    owner: str | None = Depends(get_owner_name),
    db: Session = Depends(get_db),
):
    statement = select(Task)
    if owner is not None:
        statement = statement.where(Task.owner_name == owner)
    if params["done"] is not None:
        statement = statement.where(Task.done == params["done"])
    if params["keyword"] is not None:
        statement = statement.where(Task.title.contains(params["keyword"]))

    found = db.scalars(statement).all()
    return {"count": len(found), "tasks": found}
```

**`statement = statement.where(...)` を重ねている**のがポイントです。
`where` は元の `statement` を書き換えず、**条件を足した新しい指示を返します。**
そのため、`if` で条件を積み上げていく形がそのまま書けます。

これは、第3章から書いてきた

```python
result = [task for task in result if task["done"] == done]
```

の積み上げと、**まったく同じ考え方**です。絞り込む場所が Python からデータベースに移っただけです。

| 使ったもの | 意味 |
|-----------|------|
| `db.scalars(statement)` | 実行して、**複数件**を返す |
| `.all()` | 結果をリストにする |
| `Task.title.contains("買")` | `title` に「買」を**含む**行（`keyword in task["title"]` に相当） |

`import` に `get_owner_name` を足します。

```diff
- from app.dependencies import get_db, get_task_or_404, list_params
+ from app.dependencies import get_db, get_owner_name, get_task_or_404, list_params
```

残っている2つの窓口も直します。

`app/routers/tasks.py`（`read_summary` と `read_tasks_by_ids`）

```python
@router.get("/summary")
def read_summary(db: Session = Depends(get_db)):
    total = db.scalar(select(func.count()).select_from(Task))
    done_count = db.scalar(select(func.count()).select_from(Task).where(Task.done))
    return {"total": total, "done": done_count, "remaining": total - done_count}


@router.get("/by-ids")
def read_tasks_by_ids(
    task_id: list[int] = Query(default=[]),
    db: Session = Depends(get_db),
):
    found = db.scalars(select(Task).where(Task.id.in_(task_id))).all()
    return {
        "requested": task_id,
        "count": len(found),
        "tasks": [TaskRead.model_validate(task) for task in found],
    }
```

```diff
- from sqlalchemy import select
+ from sqlalchemy import func, select
```

新しく出てきたものが3つあります。

| 部分 | 意味 |
|------|------|
| `select(func.count()).select_from(Task)` | **件数だけ**を数える。全件を運ばずに数字1つだけ受け取る |
| `Task.id.in_(task_id)` | `id` がリストの**どれかに一致する**行（`task["id"] in task_id` に相当） |
| `TaskRead.model_validate(task)` | `Task` を `TaskRead` に**手で変換する** |

**`model_validate` を書いている理由**を説明します。
`/tasks/by-ids` には `response_model` が付いていません（包みの形が独自なためです。3.2.4）。
`response_model` が無いと、FastAPI は受け取ったものを**そのまま JSON にしようとします。**
その結果がこれです。

```json
{"requested":[1],"count":1,"tasks":[{"done":false,"priority":3,"owner_name":"山田","title":"牛乳を買う","id":1,"code":null,"tags":[],"owner_email":"yamada@example.com"}]}
```

**`owner_email` が出ています。** 4.4.2 で外したはずのメールアドレスが、外に出てしまいました。

`TaskRead.model_validate(task)` は、`Task` を `TaskRead` の形に通し直すメソッドです
（`from_attributes=True` を書いたので、属性から読めます。6.3.3）。
これを通すと `owner_email` は落ちます。

> **注意**
> **`response_model` の無い窓口から、テーブルのモデルをそのまま返さないでください。**
> `response_model` がある窓口では FastAPI が形を絞ってくれますが（4.4.1）、
> 無い窓口では**何も守ってくれません。**
> 独自の包みで返すときは、中身を必ずスキーマに通してください。

ここまでで、`app/routers/tasks.py` から `from app.data import tasks` が消えました。
`app/main.py` の `/info` も直します。

`app/main.py`（`read_info` の部分）

```python
@app.get("/info")
def read_info(db: Session = Depends(get_db)):
    return {
        "app_name": settings.app_name,
        "debug": settings.debug,
        "task_count": db.scalar(select(func.count()).select_from(Task)),
    }
```

```diff
- from fastapi import FastAPI, Request
+ from fastapi import Depends, FastAPI, Request
+ from sqlalchemy import func, select
+ from sqlalchemy.orm import Session
  
- from app import data
  from app.config import settings
+ from app.dependencies import get_db
  from app.errors import DuplicateCodeError
+ from app.models import Task
  from app.routers import misc, tasks
```

> **補足：`app/data.py` はどうなるのか**
> これで、タスクの `tasks` リストを読むコードは1つも無くなりました。
> ただし、第5章の演習で作った**メモの窓口はまだ `notes` リストを使っています。**
> `app/data.py` は、**演習 6.2 でメモを移すまで残しておいてください。**
> `tasks` のほうは、もう消して構いません。

**ここで一度、動かします。** サーバーを起動してください。

**Windows（PowerShell）**

```powershell
fastapi dev app/main.py
```

**macOS / Linux**

```bash
fastapi dev app/main.py
```

```text
http://127.0.0.1:8000/tasks
```

```json
{"count":3,"tasks":[{"title":"牛乳を買う","done":false,"code":null,"priority":3,"tags":[],"id":1,"owner":{"name":"山田"}},{"title":"レポートを書く","done":true,"code":null,"priority":3,"tags":[],"id":2,"owner":{"name":"鈴木"}},{"title":"部屋を片づける","done":false,"code":null,"priority":3,"tags":[],"id":3,"owner":{"name":"山田"}}]}
```

**第5章までとまったく同じ結果**です。データの置き場所だけが変わりました。

絞り込みも、今までどおり効きます。

```text
http://127.0.0.1:8000/tasks?done=true
```

```json
{"count":1,"tasks":[{"title":"レポートを書く","done":true,"code":null,"priority":3,"tags":[],"id":2,"owner":{"name":"鈴木"}}]}
```

```text
http://127.0.0.1:8000/tasks?owner=山田
```

```json
{"count":2,"tasks":[{"title":"牛乳を買う","done":false,"code":null,"priority":3,"tags":[],"id":1,"owner":{"name":"山田"}},{"title":"部屋を片づける","done":false,"code":null,"priority":3,"tags":[],"id":3,"owner":{"name":"山田"}}]}
```

**そして、この章の本題を確かめます。**
`/docs` から `POST /tasks` を1件実行し、サーバーを `Ctrl` + `C` で止めて、
もう一度 `fastapi dev app/main.py` で起動してください。

```json
{"count":4,"tasks":[...]}
```

**4件のままです。** 6.1.1 で消えたタスクが、今度は残っています。

### 6.4.3 更新（Update）

更新は、**取り出したオブジェクトの属性を書き換えて、`commit` する**だけです。

`app/routers/tasks.py`（`update_task` の部分）

```python
@router.patch("/{task_id}", response_model=TaskRead)
def update_task(
    new_task: TaskUpdate,
    task: Task = Depends(get_task_or_404),
    db: Session = Depends(get_db),
):
    changes = new_task.model_dump(exclude_unset=True)
    for key, value in changes.items():
        setattr(task, key, value)
    db.commit()
    db.refresh(task)
    logger.info("タスクを更新しました id=%s 変更=%s", task.id, list(changes))
    return task
```

第5章では、辞書だったので `task[key] = value` と書けました。
`Task` はオブジェクトなので、`task.title` のように**属性**でアクセスします。
ところが `key` は `"title"` という**文字列**なので、`task.key` とは書けません。

そこで **`setattr(task, key, value)`** を使います。
「`task` の、`key` という名前の属性に `value` を入れる」という組み込み関数です。

| 書き方 | 使う場面 |
|-------|---------|
| `task.title = "新しい題名"` | 属性の名前が、書くときに決まっている |
| `setattr(task, key, value)` | 属性の名前が**変数に入っている** |

**`db.add(task)` は要りません。**
`get_task_or_404` がセッションから取り出したオブジェクトなので、
セッションが「この行は自分が管理している」と覚えており、
属性を書き換えた時点で**変更が追跡されています。** `commit` すれば書き込まれます。

動かして確かめます。

**Windows（PowerShell）**

```powershell
curl.exe -X PATCH http://127.0.0.1:8000/tasks/1 -H "Content-Type: application/json" -d "{\"done\":true,\"tags\":[\"買い物\"]}"
```

**macOS / Linux**

```bash
curl -X PATCH http://127.0.0.1:8000/tasks/1 -H "Content-Type: application/json" -d '{"done":true,"tags":["買い物"]}'
```

実行結果:

```json
{"title":"牛乳を買う","done":true,"code":null,"priority":3,"tags":["買い物"],"id":1,"owner":{"name":"山田"}}
```

サーバーを再起動しても、`done` は `true` のままです。

> **補足：Windows での引用符**
> PowerShell では、JSON の中のダブルクォートを `\"` と書く必要があります。
> 面倒であれば、**`/docs` から実行してください**（2.5.2）。同じことができます。

### 6.4.4 削除（Delete）

削除も同じ形です。

`app/routers/tasks.py`（`delete_task` の部分）

```python
@router.delete("/{task_id}", status_code=204)
def delete_task(task: Task = Depends(get_task_or_404), db: Session = Depends(get_db)):
    db.delete(task)
    db.commit()
    logger.info("タスクを削除しました id=%s", task.id)
    return None
```

`db.delete(task)` で「消したい」と申し出て、`db.commit()` で確定します。
`db.add` と対になる形です。

動かして確かめます。

**Windows（PowerShell）**

```powershell
curl.exe -i -X DELETE http://127.0.0.1:8000/tasks/3
```

**macOS / Linux**

```bash
curl -i -X DELETE http://127.0.0.1:8000/tasks/3
```

実行結果:

```text
HTTP/1.1 204 No Content
x-process-time: 0.0044
```

**もう一度同じコマンドを実行してください。**

```text
HTTP/1.1 404 Not Found
content-type: application/json

{"error":{"status":404,"message":"id 3 のタスクは見つかりませんでした","detail":null}}
```

1回目は `204`、2回目は `404`。**本当に消えている**ことが確認できました。

> **注意**
> `logger.info(...)` を `db.delete(task)` の**前**に書いてください、という話ではありません。
> 上のコードでは `db.commit()` のあとに `task.id` を読んでいますが、これは動きます。
> ただし、**削除したオブジェクトの他の属性（`task.title` など）は、`commit` 後に読めません。**
> `Instance ... has been deleted` というエラーになります。
> 消したものについてログを残したいときは、**`commit` の前に必要な値を変数に取っておいて**ください。

### 6.4.5 ページネーション

件数が増えると、`GET /tasks` が一度に全件を返すのは現実的ではなくなります。
1万件のタスクを毎回返していたら、画面が表示されるまで待たされます。

そこで、**何件目から何件返すか**を指定できるようにします。
これを**ページネーション**（一覧を一定件数ずつに区切って返す仕組み）と呼びます。

必要なのは2つの数字です。

| 名前 | 意味 | 例（1ページ 10 件のとき） |
|------|------|------------------------|
| `skip` | **先頭から何件飛ばすか** | 1ページ目は `0`、2ページ目は `10`、3ページ目は `20` |
| `limit` | **何件返すか** | `10` |

`limit` は 5.3.1 の `list_params` にすでにあります。`skip` を足します。

`app/dependencies.py`（`list_params` の部分）

```python
def list_params(
    done: bool | None = None,
    keyword: str | None = Query(default=None, min_length=1, max_length=20),
    skip: int = Query(default=0, ge=0),
    limit: int = Query(default=10, ge=1, le=100),
) -> dict:
    """一覧を絞り込むための、共通のクエリパラメータ。"""
    return {"done": done, "keyword": keyword, "skip": skip, "limit": limit}
```

`ge=0` にしているのは、**0件飛ばす（＝先頭から）が正しい指定**だからです。
`limit` の `ge=1` とは違います。

`read_tasks` を仕上げます。

`app/routers/tasks.py`（`read_tasks` の部分）

```python
@router.get("", response_model=TaskListResponse)
def read_tasks(
    params: dict = Depends(list_params),
    owner: str | None = Depends(get_owner_name),
    db: Session = Depends(get_db),
):
    statement = select(Task)
    if owner is not None:
        statement = statement.where(Task.owner_name == owner)
    if params["done"] is not None:
        statement = statement.where(Task.done == params["done"])
    if params["keyword"] is not None:
        statement = statement.where(Task.title.contains(params["keyword"]))

    # 絞り込んだ「全体の件数」を、切り出す前に数える
    count = db.scalar(select(func.count()).select_from(statement.subquery()))
    logger.debug("一覧の条件 params=%s owner=%s 該当=%s件", params, owner, count)

    statement = statement.order_by(Task.id).offset(params["skip"]).limit(params["limit"])
    found = db.scalars(statement).all()
    return {"count": count, "tasks": found}
```

3つ、大事な点があります。

**1つ目：`count` は「返した件数」ではなく「該当した全体の件数」**

`skip` / `limit` で切り出す**前**に数えています。
呼ぶ側（第9章の React）は、この数字を見て「全 42 件中の 11〜20 件目」と表示できます。
切り出したあとの件数を返すと、**何ページあるのか分からなくなります。**

`statement.subquery()` は、「いま組み立てた絞り込みの結果」をそのまま数える対象にする書き方です。

**2つ目：`order_by` を必ず付ける**

**並び順を指定しないと、返る順番は保証されません。**
順番が決まっていないのに「先頭から 10 件飛ばす」と言っても、何が返るか分かりません。
ここでは `id` の順にしています。

**3つ目：順番は「絞り込み → 並べ替え → 切り出し」**

`where` をすべて足してから `order_by`、最後に `offset` / `limit` です。
切り出してから絞り込むと、結果が変わってしまいます。

並べ方は、`order_by` に渡すものを変えるだけで切り替えられます。

| 書き方 | 並び |
|-------|------|
| `order_by(Task.id)` | `id` の**昇順**（小さい順）。既定 |
| `order_by(Task.id.desc())` | `id` の**降順**（大きい順） |
| `order_by(Task.priority, Task.id)` | まず `priority` 順、**同じなら** `id` 順 |

**`.desc()`**（descending の略。降順）を付けると、逆向きになります。

`order_by` に**2つ以上渡せる**ことも覚えておいてください。
1つ目が同じ値だったときに、2つ目で順番が決まります。
`priority` のように同じ値が並びやすい列で並べるときは、
**`id` を2つ目に足しておかないと、同じ順位のタスクの順番が決まりません。**

```mermaid
flowchart LR
    A["tasks テーブル<br/>全件"] --> B["where<br/>絞り込み"]
    B --> C["count<br/>ここで数える"]
    B --> D["order_by<br/>id 順に並べる"]
    D --> E["offset / limit<br/>切り出す"]
    E --> F["レスポンスの tasks"]
    C --> G["レスポンスの count"]
```

動かして確かめます。
ここまでの手順どおりに進めていれば、いまタスクは3件（`id` は 1・2・4）あります。
6.4.2 で1件登録し、6.4.4 で `id` が 3 のものを削除したからです。

```text
http://127.0.0.1:8000/tasks?skip=1&limit=2
```

```json
{"count":3,"tasks":[{"title":"レポートを書く","done":true,"code":null,"priority":3,"tags":[],"id":2,"owner":{"name":"鈴木"}},{"title":"郵便を出す","done":false,"code":null,"priority":3,"tags":[],"id":4,"owner":{"name":"山田"}}]}
```

**`count` は 3、`tasks` は 2件。** 「全体で3件あり、そのうち2件目から2件を返した」という意味です。
件数が合わない場合は、`/docs` から何件登録したかを数え直してください。**手順自体は同じです。**

> **よくある間違い**
> **`count` を `len(found)` のままにする**間違いです。
>
> ```python
> return {"count": len(found), "tasks": found}     # ❌ ページネーションを足したあと
> ```
>
> `?skip=0&limit=2` のとき、`count` が常に `2` になります。
> 画面には「全2件」と出て、**3件目以降があることが誰にも分からなくなります。**

---

## 6.5 セッション管理

### 6.5.1 `Depends` でセッションを配る

6.4.1 で書いた `get_db` を、あらためて読みます。

```python
def get_db() -> Iterator[Session]:
    """1リクエストにつき1つ、セッションを作って配る。"""
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**`return` ではなく `yield`** です。違いはここです。

| 書き方 | 動き |
|-------|------|
| `return db` | 値を返して、**その場で関数は終わる** |
| `yield db` | 値を渡して、**いったん止まる。** 窓口の関数が終わったら、続きが動く |

FastAPI は、`yield` を使った依存を見つけると、次のように動きます。

```mermaid
sequenceDiagram
    participant R as リクエスト
    participant D as get_db
    participant F as 窓口の関数
    participant DB as app.db
    R->>D: 依存の解決
    D->>D: SessionLocal() でセッションを作る
    D->>F: yield でセッションを渡す
    F->>DB: 読み書き
    DB-->>F: 結果
    F-->>D: 関数が終わる
    D->>D: finally で db.close()
    D-->>R: レスポンス
```

**`yield` の前が「行き」、後ろが「帰り」**です。
5.6.1 のミドルウェアと同じ構造ですが、こちらは**その依存を使う窓口だけ**が対象です。

`try` / `finally` は python-text 7.5.4 で学んだ書き方です。
**`finally` は、途中で例外が起きても必ず実行されます。**
`get_task_or_404` が `404` を投げても、セッションは閉じられます。

**セッションは1リクエストにつき1つ**です。
同じリクエストの中で `get_db` を何度要求しても、返るのは同じセッションです
（同じ依存は1リクエストにつき1回しか呼ばれない、という 5.3.3 の補足のとおりです）。

これは大事な性質です。`update_task` を見てください。

```python
def update_task(
    new_task: TaskUpdate,
    task: Task = Depends(get_task_or_404),   # ← この中でも get_db を使っている
    db: Session = Depends(get_db),           # ← ここでも使っている
):
```

**`get_task_or_404` が取り出した `task` と、`db` は、同じセッションのもの**です。
だからこそ、`task` の属性を書き換えて `db.commit()` すれば保存されます。
別々のセッションだったら、この書き方は動きません。

> **補足：なぜリクエストごとに作るのか**
> セッションを1つだけ作ってアプリ全体で使い回すと、
> **ある人の失敗が、別の人の処理を巻き込みます**（6.5.2 のロールバックが全員に効いてしまいます）。
> 「1リクエスト＝1つの仕事」と区切っておくと、失敗の影響もそこで止まります。

### 6.5.2 コミットとロールバック

データベースは、複数の書き込みを**まとめて1つの単位**として扱えます。
これを**トランザクション**（複数の操作を「全部成功」か「全部なかったこと」にまとめる仕組み）と呼びます。

| 命令 | 意味 |
|------|------|
| `db.commit()` | **確定する。** ここまでの変更をすべて保存する |
| `db.rollback()` | **取り消す。** ここまでの変更をすべて無かったことにする |

まず、`commit` しないとどうなるかを実際に見ます。
`fastapi-lesson` で、次を実行してください（サーバーは動いたままで構いません）。

**Windows（PowerShell）**

```powershell
python -c "from app.database import SessionLocal; from app.models import Task; from sqlalchemy import select, func; db = SessionLocal(); before = db.scalar(select(func.count()).select_from(Task)); db.add(Task(title='commit忘れ', owner_name='山田', owner_email='y@example.com')); db.close(); db2 = SessionLocal(); print('前:', before, '/ 後:', db2.scalar(select(func.count()).select_from(Task))); db2.close()"
```

**macOS / Linux**

```bash
python -c "from app.database import SessionLocal; from app.models import Task; from sqlalchemy import select, func; db = SessionLocal(); before = db.scalar(select(func.count()).select_from(Task)); db.add(Task(title='commit忘れ', owner_name='山田', owner_email='y@example.com')); db.close(); db2 = SessionLocal(); print('前:', before, '/ 後:', db2.scalar(select(func.count()).select_from(Task))); db2.close()"
```

実行結果:

```text
前: 3 / 後: 3
```

（数字は、それまでに登録した件数によって変わります。
**大事なのは、前と後が同じ数になること**です。）

**増えていません。** `db.add` はしましたが `db.commit()` をしていないので、
セッションを閉じた時点で**捨てられました。**

これが 6.4.1 の「よくある間違い」の正体です。
`db.add` は「申し出る」だけで、**保存を決めるのは `db.commit()`** です。

次に、**失敗したとき**です。
`code` には `unique=True` を付けたので（6.3.2）、同じ管理番号は2つ作れません。

6.4.1 の `create_task` には、事前に `select` で調べる処理が入っています。
それでも、**確認した直後・保存する直前に、別のリクエストが同じ番号で登録する**ことがあり得ます。
そのときはデータベース側が拒否し、`IntegrityError`（制約に反したときの例外）が起きます。

`create_task` を、それに耐える形にします。

`app/routers/tasks.py`（`create_task` の `db.commit()` の部分）

```diff
      db.add(task)
-     db.commit()
+     try:
+         db.commit()
+     except IntegrityError:
+         # データベース側で弾かれた。取り消してから、同じ扱いにする
+         db.rollback()
+         raise DuplicateCodeError(new_task.code)
      db.refresh(task)
```

```diff
+ from sqlalchemy.exc import IntegrityError
```

**`db.rollback()` を必ず書いてください。**
失敗したあとのセッションは、**取り消すまで次の仕事を受け付けません。**

実際、`rollback` を書かずに同じセッションで次の `commit` をすると、こうなります。

```text
sqlalchemy.exc.PendingRollbackError: This Session's transaction has been rolled back
due to a previous exception during flush. To begin a new transaction with this Session,
first issue Session.rollback().
```

**「先に `Session.rollback()` を呼んでください」**と、そのまま書かれています。

この状態を実際に確かめるには、6.4.1 で書いた事前の `select` の確認を
一時的にコメントアウトして、同じ `code` で2回 `POST /tasks` してください。
`try` / `except` を書いた後なら、**`409` が返ります。**

```json
{"error":{"status":409,"message":"管理番号 T-001 は、すでに使われています","detail":null}}
```

**事前の確認と、`IntegrityError` の受け止め。両方を書きます。**
前者は普通の場合に分かりやすいエラーを返すため、後者は取りこぼしを `500` にしないためです。

> **補足：トランザクションが効く場面**
> ここでは1件だけ保存しているので、ありがたみが分かりにくいところです。
> 「タスクを1件登録し、同時に履歴を1件残す」ような処理では、
> **片方だけ保存された状態**が最悪です。
> `db.add` を2回してから `db.commit()` を1回呼べば、**両方入るか、両方入らないか**になります。

### 6.5.3 セッションの閉じ忘れ

`get_db` の `finally` を、もし書かなかったらどうなるかを考えます。

```python
def get_db():
    db = SessionLocal()
    yield db
    db.close()          # ❌ finally が無い
```

普段は動きます。**例外が起きたときだけ**壊れます。

`get_task_or_404` が `404` を投げると、`yield` の続き（`db.close()`）に**到達しません。**
セッションが閉じられないまま残ります。

これが積み重なると、次のことが起きます。

| 起きること | 症状 |
|-----------|------|
| 接続が返却されない | しばらく動いたあと、**急に応答しなくなる** |
| SQLite のファイルが掴まれたまま | `database is locked` が出る |
| 確定していない変更が残り続ける | 他のセッションから見えたり見えなかったりする |

**厄介なのは、すぐには症状が出ないこと**です。
開発中の数十リクエストでは何も起きず、しばらく動かしてから急に止まります。

だから、**`try` / `finally` は「例外が出るかもしれないから」ではなく、
「必ず閉じるため」に最初から書きます。**

```python
def get_db() -> Iterator[Session]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()      # ✅ 404 でも 500 でも必ず通る
```

> **よくある間違い**
> **窓口の関数の中で `SessionLocal()` を直接呼ぶ**間違いです。
>
> ```python
> @router.get("/summary")
> def read_summary():
>     db = SessionLocal()          # ❌ 閉じる保証がない
>     return {"total": db.scalar(...)}
> ```
>
> このコードは動きますが、**閉じていません。**
> 例外が起きても起きなくても、閉じるコードがどこにもありません。
> **セッションは必ず `Depends(get_db)` で受け取ってください。**

---

## 6.6 マイグレーション

### 6.6.1 テーブル定義を変更したくなったら

「タスクを、登録した日時の順に並べたい」という要望が来たとします。
いまの `Task` には日時の列がないので、足すことになります。

6.3.2 の「よくある間違い」で予告した問題が、ここで表に出ます。
`app/models.py` に列を1つ足して `python create_tables.py` をもう一度実行しても、
**何も起きません。**

```text
テーブルを作りました。
```

メッセージは出ますが、`create_all` は「**まだ無いテーブルを作る**」だけだからです。
`tasks` テーブルはすでにあるので、素通りします。

そのまま窓口を呼ぶと、こうなります。

```text
sqlite3.OperationalError: no such column: tasks.created_at
```

**「そんな列はない」**。
Python 側のクラスと、データベース側のテーブルが**食い違った**状態です。

やり方は3つ考えられます。

| やり方 | 結果 |
|-------|------|
| `app.db` を削除して作り直す | **入っているデータが全部消える。** 練習中はできても、本番ではできない |
| データベースに手で列を足す | 自分のパソコンでは直るが、**他の人の環境は直らない** |
| **マイグレーション** | **変更を1つずつファイルに記録し、どの環境でも同じ順番で適用する** |

**マイグレーション**（テーブルの構造の変更を、記録して順番に適用する仕組み）を使います。
Python では **Alembic**（アレンビック）という道具が標準的で、SQLAlchemy と同じ作者が作っています。

```mermaid
flowchart LR
    A["<base><br/>何もない"] --> B["001<br/>tasks を作る"]
    B --> C["002<br/>created_at を足す"]
    C --> D["003<br/>（将来の変更）"]
```

変更の記録が**1本の鎖**になっていて、
どの環境でも「いまどこまで適用したか」を見て、足りない分だけ順に適用します。

### 6.6.2 Alembic の初期設定

Alembic を入れます。**`fastapi-lesson` で、仮想環境を有効にした状態**で実行してください。

**Windows（PowerShell）**

```powershell
pip install alembic==1.14.0
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip install alembic==1.14.0
pip freeze > requirements.txt
```

```text
Successfully installed alembic-1.14.0
```

**この章はここで一度、データベースを作り直します。**
`create_all` で作ったテーブルは「どの変更まで適用したか」の記録を持っていないので、
Alembic から見ると素性が分からないためです。
練習用のデータは `app/seed.py` で戻せます。

`fastapi-lesson` の `app.db` を**削除**してください（VS Code のファイル一覧から削除できます）。
`create_tables.py` も、もう使わないので削除して構いません。

Alembic の置き場所を作ります。

**Windows（PowerShell）**

```powershell
alembic init migrations
```

**macOS / Linux**

```bash
alembic init migrations
```

```text
Creating directory '/Users/yamada/Desktop/fastapi-lesson/migrations' ...  done
Creating directory '/Users/yamada/Desktop/fastapi-lesson/migrations/versions' ...  done
Generating /Users/yamada/Desktop/fastapi-lesson/migrations/script.py.mako ...  done
Generating /Users/yamada/Desktop/fastapi-lesson/migrations/README ...  done
Generating /Users/yamada/Desktop/fastapi-lesson/migrations/env.py ...  done
Generating /Users/yamada/Desktop/fastapi-lesson/alembic.ini ...  done
Please edit configuration/connection/logging settings in
'/Users/yamada/Desktop/fastapi-lesson/alembic.ini' before proceeding.
```

できたものは次のとおりです。

```text
fastapi-lesson/
├── alembic.ini           Alembic の設定ファイル
├── app.db                （削除した。次の 6.6.3 で作り直す）
├── app/
└── migrations/
    ├── env.py            ★ここを編集する
    ├── script.py.mako    変更ファイルの雛形
    └── versions/         ★変更の記録がここに溜まっていく
```

編集するのは **`migrations/env.py` の2か所だけ**です。

1か所目。ファイルの先頭付近、`from alembic import context` の**下**に追記します。

`migrations/env.py`（`from alembic import context` の下）

```python
from app.config import settings
from app.database import Base
from app.models import Task  # noqa: F401  Base に Task を登録するための import
```

2か所目。`target_metadata = None` と書かれた行を探して、**置き換えます。**

`migrations/env.py`（`target_metadata = None` の行）

```python
config.set_main_option("sqlalchemy.url", settings.database_url)

target_metadata = Base.metadata
```

それぞれの意味です。

| 書いたもの | 意味 |
|-----------|------|
| `config.set_main_option("sqlalchemy.url", ...)` | **接続先を `.env` から読む。** `alembic.ini` に書かずに済む |
| `target_metadata = Base.metadata` | **あるべきテーブルの形**を Alembic に教える |
| `from app.models import Task` | `Base` に `Task` を登録する（6.3.1 と同じ理由） |

`target_metadata` が「**こうあってほしい形**」、
接続先のデータベースが「**いまの形**」です。
Alembic は、この2つを見比べて差分を見つけます。

> **注意**
> **`alembic.ini` の `sqlalchemy.url` は書き換えないでください。**
> `alembic.ini` には既定で `sqlalchemy.url = driver://user:pass@localhost/dbname` と
> 書かれていますが、`env.py` の `set_main_option` が**あとから上書きします。**
> 接続先が2か所にあると、どちらが効いているのか分からなくなります。
> また、`alembic.ini` は共有するファイルなので、**パスワードを書いてはいけません**（4.6.3）。

> **注意**
> `alembic` コマンドは、**`fastapi-lesson` で実行してください。**
> `from app.config import settings` が `app` パッケージを探すので、
> 場所が違うと `ModuleNotFoundError: No module named 'app'` になります（5.1.2 と同じ原因です）。

### 6.6.3 マイグレーションを作成・適用する

**1本目**を作ります。「`tasks` テーブルを作る」という記録です。

**Windows（PowerShell）**

```powershell
alembic revision --autogenerate -m "create tasks table"
```

**macOS / Linux**

```bash
alembic revision --autogenerate -m "create tasks table"
```

```text
INFO  [alembic.runtime.migration] Context impl SQLiteImpl.
INFO  [alembic.autogenerate.compare] Detected added table 'tasks'
Generating /Users/yamada/Desktop/fastapi-lesson/migrations/versions/93078221040e_create_tasks_table.py ...  done
```

`--autogenerate` が、**モデルとデータベースを見比べて差分を書き出す**指定です。
`Detected added table 'tasks'`（`tasks` テーブルの追加を検出した）と出ています。

できたファイルを開いてください。ファイル名の先頭 12 文字は、**毎回変わります**（自動で振られる ID です）。

`migrations/versions/93078221040e_create_tasks_table.py`（一部）

```python
revision: str = '93078221040e'
down_revision: Union[str, None] = None


def upgrade() -> None:
    op.create_table('tasks',
    sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('title', sa.String(length=20), nullable=False),
    sa.Column('done', sa.Boolean(), nullable=False),
    sa.Column('code', sa.String(length=10), nullable=True),
    sa.Column('priority', sa.Integer(), nullable=False),
    sa.Column('tags', sa.JSON(), nullable=False),
    sa.Column('owner_name', sa.String(length=20), nullable=False),
    sa.Column('owner_email', sa.String(length=100), nullable=False),
    sa.PrimaryKeyConstraint('id'),
    sa.UniqueConstraint('code')
    )


def downgrade() -> None:
    op.drop_table('tasks')
```

| 部分 | 意味 |
|------|------|
| `revision` | この変更の ID |
| `down_revision` | **1つ前の変更の ID。** `None` なので、これが1本目 |
| `upgrade()` | **進めるとき**にすること |
| `downgrade()` | **戻すとき**にすること |

`upgrade` と `downgrade` が**対になっている**ことを確認してください。
作るなら消す、足すなら外す。これが揃っているから、あとで戻せます。

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
INFO  [alembic.runtime.migration] Running upgrade  -> 93078221040e, create tasks table
```

`head` は「**鎖のいちばん先まで**」という意味です。
`app.db` が作り直されました。練習用のデータを入れ直します。

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

**では、本題の列を足します。**

`app/models.py`（`owner_email` の下に1行、先頭に1行）

```diff
  """データベースのテーブルに対応するクラス。"""
  
+ from datetime import datetime
+ 
  from sqlalchemy import JSON, String
```

```diff
      owner_email: Mapped[str] = mapped_column(String(100))
+     created_at: Mapped[datetime | None] = mapped_column(default=datetime.now)
```

`datetime` は python-text 6.3.2 で学んだ標準ライブラリで、`datetime.now()` は現在の日時を返します。

`default=datetime.now` は、`datetime.now()` **ではありません**（括弧を付けません）。
括弧を付けると、**サーバーを起動した瞬間の時刻**が全部の行に入ってしまいます。
括弧を付けなければ、「行を作るたびに `datetime.now` を呼ぶ」という意味になります
（6.3.2 の `default=list` と同じ話です）。

`| None` を付けているのは、**すでに入っている3件をどうするか**という問題があるからです。
あとから足す列に「空にできない」と書くと、既存の行に入れる値が無くて失敗します。

**2本目**を作ります。

**Windows（PowerShell）**

```powershell
alembic revision --autogenerate -m "add created_at to tasks"
```

**macOS / Linux**

```bash
alembic revision --autogenerate -m "add created_at to tasks"
```

```text
INFO  [alembic.autogenerate.compare] Detected added column 'tasks.created_at'
Generating /Users/yamada/Desktop/fastapi-lesson/migrations/versions/80daba50699c_add_created_at_to_tasks.py ...  done
```

中身は2行です。

`migrations/versions/80daba50699c_add_created_at_to_tasks.py`（一部）

```python
revision: str = '80daba50699c'
down_revision: Union[str, None] = '93078221040e'


def upgrade() -> None:
    op.add_column('tasks', sa.Column('created_at', sa.DateTime(), nullable=True))


def downgrade() -> None:
    op.drop_column('tasks', 'created_at')
```

**`down_revision` が、1本目の ID になっています。** ここで鎖が繋がりました。

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
INFO  [alembic.runtime.migration] Running upgrade 93078221040e -> 80daba50699c, add created_at to tasks
```

**データは消えていません。** 列だけが足されました。

返すようにします。`TaskRead` に1行足してください。

`app/schemas.py`（`TaskRead` の中）

```diff
      id: int
      owner: OwnerRead
+     created_at: datetime | None = None
```

```diff
+ from datetime import datetime
+ 
  from pydantic import BaseModel, ConfigDict, Field, field_validator
```

サーバーを起動して確かめます。

```text
http://127.0.0.1:8000/tasks
```

```json
{"count":3,"tasks":[{"title":"牛乳を買う","done":false,"code":null,"priority":3,"tags":[],"id":1,"owner":{"name":"山田"},"created_at":null},{"title":"レポートを書く","done":true,"code":null,"priority":3,"tags":[],"id":2,"owner":{"name":"鈴木"},"created_at":null},{"title":"部屋を片づける","done":false,"code":null,"priority":3,"tags":[],"id":3,"owner":{"name":"山田"},"created_at":null}]}
```

**既存の3件は `created_at` が `null`** です。列が無かった頃に入ったデータだからです。

`/docs` から `POST /tasks` を1件実行してください。

```json
{"title":"郵便を出す","done":false,"code":null,"priority":3,"tags":[],"id":4,"owner":{"name":"山田"},"created_at":"2026-09-04T07:23:27.781248"}
```

**新しい行には時刻が入っています。**

いまどこまで適用されているかは、いつでも確認できます。

**Windows（PowerShell）**

```powershell
alembic current
alembic history
```

**macOS / Linux**

```bash
alembic current
alembic history
```

```text
80daba50699c (head)
```

```text
93078221040e -> 80daba50699c (head), add created_at to tasks
<base> -> 93078221040e, create tasks table
```

**1つ戻す**こともできます。

**Windows（PowerShell）**

```powershell
alembic downgrade -1
```

**macOS / Linux**

```bash
alembic downgrade -1
```

```text
INFO  [alembic.runtime.migration] Running downgrade 80daba50699c -> 93078221040e, add created_at to tasks
```

`created_at` の列が消えます。**確認したら、必ず戻してください。**

**Windows（PowerShell）**

```powershell
alembic upgrade head
```

**macOS / Linux**

```bash
alembic upgrade head
```

> **注意**
> **`--autogenerate` が作ったファイルは、必ず目で読んでください。**
> Alembic が差分を取り違えることがあります。よくあるのは次の2つです。
>
> - **列の名前を変えた**とき、「古い列を削除して新しい列を追加する」と書かれる
>   （そのまま適用すると、**その列のデータが消えます**）
> - 型を少し変えただけなのに、SQLite では表現できず**何も生成されない**
>
> ファイル名の下に `# ### commands auto generated by Alembic - please adjust! ###`
> （自動生成しました。調整してください）と書かれているのは、この確認を促すためです。

> **注意**
> **一度適用して他の人に渡したマイグレーションは、書き換えないでください。**
> 相手の環境ではすでに古い内容が適用済みで、記録と実物が食い違います。
> 直したいときは、**新しいマイグレーションをもう1本足します。**

---

## まとめ

- リストのデータは**メモリの上**にあり、サーバーを止めると消える。
  ディスクに残すことを**永続化**という（6.1.1）
- このテキストでは **SQLite**（ファイル1つ）で進め、MySQL は docker-text 以降で扱う（6.1.2）
- **ORM** は、データベースの行を Python のオブジェクトとして扱う仕組み。
  **1クラス＝1テーブル、1インスタンス＝1行**（6.2.1・6.3.1）
- 接続先は `.env` の **`DATABASE_URL`** に置く。
  乗り換えるときはこの1行だけを変える（6.2.2）
- 部品は3つ。**エンジン**（アプリに1つ）・**セッション**（1リクエストに1つ）・**`Base`**（6.2.3）
- 列は `名前: Mapped[型] = mapped_column(...)`。**`| None` の有無が「空を許すか」**（6.3.2）
- **`create_all` は、まだ無いテーブルを作るだけ。** 列は増やせない（6.3.2・6.6.1）
- **スキーマ（Pydantic）とモデル（SQLAlchemy）は別物。**
  テーブルのモデルを返すには **`from_attributes=True`** が要る（6.3.3）
- 登録は **`add` → `commit` → `refresh`**。
  **`commit` を忘れると、201 が返るのに保存されない**（6.4.1）
- 取得は `select(...)` を組み立てて `db.scalars(...)` / `db.scalar(...)` で実行する。
  主キー1件なら **`db.get(Task, id)`**（6.4.1・6.4.2）
- **絞り込みはデータベースにやらせる。** 全件読んでから Python で絞らない（6.4.2）
- **`response_model` の無い窓口から、テーブルのモデルをそのまま返さない。**
  `owner_email` が漏れる（6.4.2）
- 更新は **`setattr` で属性を書き換えて `commit`**。`add` は要らない（6.4.3）
- ページネーションは `skip` / `limit`。
  **`count` は切り出す前に数え、`order_by` を必ず付ける**（6.4.5）
- セッションは **`yield` + `try` / `finally`** の依存で配る。
  同じリクエスト内では同じセッションが返る（6.5.1）
- **`commit` は確定、`rollback` は取り消し。**
  失敗したセッションは、`rollback` するまで次を受け付けない（6.5.2）
- **セッションを窓口の中で直接作らない。** 閉じる保証が無くなる（6.5.3）
- テーブルの変更は **Alembic** で記録する。
  `revision --autogenerate` → **中身を読む** → `upgrade head`（6.6.2・6.6.3）
- 生成されたマイグレーションは**必ず目で確認する。**
  一度渡したものは書き換えず、新しい1本を足す（6.6.3）

---

## 理解度チェック

**問 6.1**（穴埋め）

SQLAlchemy でデータベースを使うとき、接続口である（　①　）はアプリ全体で1つ作り、
読み書きの作業台である（　②　）は1リクエストにつき1つ作る。
（　②　）の変更を確定するには（　③　）を呼ぶ。

**問 6.2**（選択）

`POST /tasks` が `201` を返すのに、`GET /tasks` に出てきません。
最も疑うべきものを1つ選んでください。

1. `response_model` の指定が間違っている
2. `db.commit()` を書いていない
3. `app.db` が作られていない
4. `from_attributes=True` を書いていない

**問 6.3**（選択）

`app/models.py` に列を1つ足して `python create_tables.py` を実行しましたが、
窓口を呼ぶと `no such column` になります。理由として正しいものを1つ選んでください。

1. `create_tables.py` の実行に失敗している
2. `create_all` は、すでにあるテーブルには列を足さないから
3. `Mapped[]` の型ヒントが間違っているから
4. サーバーを再起動していないから

**問 6.4**（記述）

`app/schemas.py` の `TaskRead` と、`app/models.py` の `Task` は、
それぞれ何を決めるためのものですか。1行ずつ書いてください。

**問 6.5**（記述）

一覧にページネーションを付けたとき、`count` に `len(found)`（切り出したあとの件数）を
使ってはいけないのはなぜですか。1〜2行で書いてください。

**問 6.6**（記述）

`get_db` を、`try` / `finally` を使わずに書くと、
どんなときに問題が起きますか。1〜2行で書いてください。

**問 6.7**（記述）

`alembic revision --autogenerate` で作られたファイルを、
そのまま適用せずに必ず読むべき理由を1つ挙げてください。

---

## 演習問題

第5章の演習で作った**メモの窓口**を、この章の内容でデータベースに移します。
第5章の演習を解いていない場合は、先に
[解答編 その1](./90-answers-part1.md#第5章) のコードを写してから始めてください。

前提として、いまのメモは次の形だとします（第4章・第5章の演習で作ったものです）。

`app/data.py`（メモの部分）

```python
notes = [
    {"id": 1, "text": "会議は水曜に変更", "pinned": False,
     "author": {"name": "山田", "email": "yamada@example.com"}},
]
```

---

### 演習 6.1 ★☆☆ メモのテーブルを作る

**課題**

メモを保存するテーブルを作ってください。まだ窓口は書き換えません。

- `app/models.py` に `Note` クラスを足す
  - テーブル名は `notes`
  - `id`：整数の主キー
  - `text`：文字列。**最大 100 文字**。空にできない
  - `pinned`：真偽値。**指定しなければ `False`**
  - `author_name`：文字列。最大 20 文字。空にできない
  - `author_email`：文字列。最大 100 文字。空にできない
  - `author` という名前の `@property` を足し、`{"name": ..., "email": ...}` を返す
- `app/schemas.py` の `NoteRead` に `from_attributes=True` を足す
- `app/seed.py` に、上の `notes` の1件を入れる処理を足す
  - すでにメモが入っているときは、**何もしない**
- Alembic でマイグレーションを1本作り、適用する
  - メッセージは `-m "create notes table"` とする

**完成条件**

- `alembic revision --autogenerate -m "create notes table"` の出力に
  `Detected added table 'notes'` が出る
- 生成されたファイルの `upgrade()` に `op.create_table('notes', ...)` があり、
  `downgrade()` に `op.drop_table('notes')` がある
- `alembic upgrade head` が成功し、**`tasks` のデータが消えていない**
  （`GET /tasks` が今までどおり返る）
- `python -m app.seed` を実行するとメモが1件入り、**2回目は増えない**
- `alembic current` の結果が、いま作ったマイグレーションの ID になっている

**ヒント**

`Task` の書き方（6.3.1・6.3.2）が、そのまま当てはまります。
`app/seed.py` の「すでに入っていたら何もしない」判定は、タスク側と同じ形で書けます。

---

### 演習 6.2 ★★☆ メモの CRUD をデータベースにする

**課題**

メモの窓口を、`app/data.py` の `notes` ではなくデータベースを見るように書き換えてください。
**URL もレスポンスの形も変えません。**

- `app/dependencies.py` の `get_note_or_404`（演習 5.2）を、`db.get` を使う形にする
  - セッションは `Depends(get_db)` で受け取る
- `POST /notes`：`Note` を作って保存する。**`id` を返せるようにする**
  - 同じ `text` の重複チェック（演習 5.3）も、データベースを見る形にする
- `GET /notes/{note_id}`・`PATCH /notes/{note_id}`・`DELETE /notes/{note_id}` を書き換える
- `app/routers/notes.py` から `from app.data import notes` を消す
- 書き換えが済んだら、`app/data.py` から `notes` を削除する

**完成条件**

- `POST /notes` が `201` を返し、レスポンスの `id` が `null` ではない
- **サーバーを止めて起動し直しても、登録したメモが残っている**
- `GET /notes/99` が
  `{"error":{"status":404,"message":"id 99 のメモは見つかりませんでした","detail":null}}` を返す
- `PATCH /notes/1` で `pinned` を `true` にすると、再起動後も `true` のまま
- `DELETE /notes/1` が `204` を返し、**2回目は `404`** になる
- 同じ `text` で2回 `POST /notes` すると、2回目は `409` が返る
- `app/routers/notes.py` に `notes`（Python のリスト）が**1つも出てこない**

**ヒント**

タスク側の `create_task` / `read_task` / `update_task` / `delete_task`（6.4.1〜6.4.4）と、
`get_task_or_404`（6.4.2）が、そのまま対応します。
`id` が `null` で返るときは、6.4.1 の3手順のどれかが抜けています。

---

### 演習 6.3 ★★☆ メモの一覧にページネーションを付ける

**課題**

`GET /notes`（演習 5.1）に、絞り込みとページネーションを付けてください。

- クエリパラメータを4つ受け取る
  - `pinned`：真偽値。省略可能（`bool | None = None`）
  - `keyword`：`text` に含まれる文字。省略可能。1〜20 文字
  - `skip`：0 以上。既定値 0
  - `limit`：1〜100。既定値 10
- **`count` は、絞り込んだ全体の件数**にする（切り出したあとの件数ではない）
- `id` の順に並べる
- `DEBUG` のログを1行出す（条件と該当件数）

**完成条件**

- メモを5件登録した状態で `GET /notes?skip=0&limit=2` を開くと、
  `count` が `5`、`notes` が2件になる
- `GET /notes?skip=4&limit=2` で、`count` は `5` のまま、`notes` は1件になる
- `GET /notes?pinned=false` が、`pinned` が `false` のメモだけを返す
  （`pinned` が `true` のメモを1件作って確認する）
- `GET /notes?limit=0` が `422` を返し、`detail` の `type` が `greater_than_equal` になる
- `.env` の `DEBUG=true` のときだけ、条件と件数の `DEBUG` ログが出る
- `notes` の中に `author_email` が含まれていない

**ヒント**

`read_tasks`（6.4.5）の組み立て方が、そのまま当てはまります。
`count` が常に `limit` と同じ数になってしまうときは、6.4.5 の「よくある間違い」を読み返してください。

---

### 演習 6.4 ★★☆ Alembic でメモに `created_at` を足す

**課題**

メモにも登録日時を足し、**新しい順**に並べられるようにしてください。

- `app/models.py` の `Note` に `created_at` を足す（`datetime | None`、既定値は登録時の時刻）
- Alembic でマイグレーションを1本作り、適用する
  - メッセージは `-m "add created_at to notes"` とする
- `NoteRead` に `created_at` を足す
- `GET /notes` に `newest` というクエリパラメータ（真偽値、既定値 `False`）を足す
  - `true` のときは **`created_at` の新しい順**に並べる
  - `false`（既定）のときは、いままでどおり `id` の順に並べる

**完成条件**

- 生成されたマイグレーションの `upgrade()` に
  `op.add_column('notes', sa.Column('created_at', ...))` の1行だけがある
- `alembic upgrade head` のあと、**演習 6.2 で登録したメモが消えていない**
- 演習 6.2 以前に入っていたメモは、`created_at` が `null` で返る
- 新しく `POST /notes` したメモには、`created_at` に時刻が入っている
- `GET /notes?newest=true` で、`created_at` が `null` ではないメモが上に来る
- `alembic downgrade -1` で列が消え、`alembic upgrade head` で戻せる

**ヒント**

`created_at` の足し方は 6.6.3 とまったく同じです。
新しい順の並べ替えは、`order_by` に渡すものを変えるだけで書けます（6.4.5 の表）。
同じ時刻のメモが並んだときのことも考えてください。

---

解答は [解答編 その2](./91-answers-part2.md#第6章) にあります。
**必ず自分で手を動かしてから**見てください。

---

## 次の章へ

データが残るようになりました。
サーバーを何度止めても、登録したタスクは `app.db` の中にあります。
テーブルの形を変えたくなっても、マイグレーションで安全に変えられます。

これで、**アプリとして人に使ってもらえる土台**ができました。

しかし、まだ大きな穴が空いています。
**いまの API は、誰でも全部のタスクを読めて、誰でも消せます。**

`?owner=山田` は「山田さんのタスクを見たい」という**お願い**でしかありません。
鈴木さんが `?owner=山田` と書けば、山田さんのタスクがそのまま見えます。
`DELETE /tasks/1` は、誰が送っても通ります。

次の章では、**「あなたは誰か」を確かめる仕組み**を作ります。
パスワードを安全に保存し、ログインした人にだけ操作を許す形にします。

→ [第7章 認証](./07-authentication.md)
