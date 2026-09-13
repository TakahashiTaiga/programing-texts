---
title: "第7章 認証"
---

# 第7章 認証

第6章の最後に、大きな穴が空いていることを確認しました。

**いまの API は、誰でも全部のタスクを読めて、誰でも消せます。**

`?owner=山田` は「山田さんのタスクを見たい」という**お願い**でしかありません。
鈴木さんが `?owner=山田` と書けば、山田さんのタスクがそのまま見えます。
`DELETE /tasks/1` は、誰が送っても通ります。

この章では、**「あなたは誰か」を確かめる仕組み**を作ります。
パスワードを安全に保存し、ログインした人にだけ操作を許す形にします。

## この章で学ぶこと

- 認証（誰か）と認可（何をしてよいか）の違いを、`401` と `403` の使い分けとして説明できるようになる
- パスワードを**ハッシュ化**して保存し、平文を一度も保存しない形が書けるようになる
- **JWT**（ジョット）を発行し、その中身を読み、改ざんを検出できるようになる
- ログインの窓口を作り、`/docs` の「Authorize」からログインして API を試せるようになる
- `Depends` で「いまログインしている人」を取り出し、**本人だけが操作できる窓口**が書けるようになる

## この章の前提

- [第6章](./06-database.md) を読み終え、`fastapi-lesson` が第6章の最終形（6.6.3）になっていること
- `alembic upgrade head` が済んでいて、`GET /tasks` が3件以上のタスクを返すこと
- 第6章の演習（メモをデータベースに移す）を解き終えていること
- python-text 第7章（`try` / `except`）と第8章（クラス）を読み終えていること

> **注意**
> この章で作るのは、**学習用の認証**です。
> 実際のサービスに必要なもの（トークンの失効、パスワードの再設定、
> ログイン試行の制限など）は、この章では作りません。
> **何が足りないのかは 7.6 で1つずつ挙げます。** そこまで必ず読んでください。
> 「動いたからこれで本番も大丈夫」と考えるのが、この分野で最も危険です。

> **つまずいたら**
> この章で最も多いのは、**「`401` が返り続ける」**という詰まり方です。
> 原因は、トークンの付け忘れ・`Bearer ` の書き忘れ・有効期限切れの3つがほとんどです（7.5.2）。
> 第0章の 0.2 で準備した AI には、次の4つを添えて聞いてください。
>
> ```text
> fastapi-text の 7.5.2 を読んでいます。
> GET /users/me が 401 になります。
> ・実行したコマンド（Authorization ヘッダーを含む。トークン本体は先頭 20 文字だけ）
> ・返ってきた JSON
> ・サーバーのターミナルの最後の5行
> ・.env に SECRET_KEY を書いたかどうか（値は書かないこと）
> ```
>
> **トークンとパスワードそのものを、AI に貼らないでください。**
> トークンは「その人になりすませる文字列」です（7.6.2）。

---

## 7.1 認証と認可

### 7.1.1 2つの違い

似た言葉が2つあります。**先にここで区別しておくと、この章の残りが読みやすくなります。**

| 用語 | 読み | 決めること | 質問の形 |
|------|------|-----------|---------|
| **認証**（Authentication） | にんしょう | **あなたは誰か** | 「あなたは本当に山田さんですか？」 |
| **認可**（Authorization） | にんか | **何をしてよいか** | 「山田さんは、このタスクを消してよいですか？」 |

建物にたとえると、**認証は入口で身分証を見せること**、
**認可はその身分証で入れる部屋が決まっていること**です。
社員証を見せて中に入れても（認証）、社長室には入れない（認可）、という関係です。

順番は必ず「認証 → 認可」です。**誰か分からないうちは、何をしてよいかも決められません。**

HTTP では、この2つがステータスコードで区別されています。

| コード | 名前 | 意味 | このアプリでの場面 |
|-------|------|------|-----------------|
| **`401`** | Unauthorized | **誰か分からない。** ログインしていない・トークンが不正・期限切れ | トークンなしで `POST /tasks` |
| **`403`** | Forbidden | **誰かは分かった。でもその操作は許さない** | 鈴木さんが山田さんのタスクを消そうとした |

```mermaid
flowchart TD
    A["リクエストが届く"] --> B{"トークンがあり<br/>正しいか"}
    B -->|いいえ| C["401 Unauthorized<br/>（誰か分からない）"]
    B -->|はい| D{"その人に<br/>この操作を許すか"}
    D -->|いいえ| E["403 Forbidden<br/>（許可がない）"]
    D -->|はい| F["窓口の関数を実行する"]
```

> **補足：`401` の名前は Unauthorized ですが、意味は「未認証」です**
> HTTP の仕様上の名前が「Unauthorized（認可されていない）」なので紛らわしいのですが、
> **`401` は認証の話、`403` は認可の話**です。
> 名前ではなく、上の表の「意味」で覚えてください。

### 7.1.2 このテキストで実装する範囲

この章で作るのは、次の流れです。

```mermaid
sequenceDiagram
    participant U as 利用者
    participant A as API
    participant DB as app.db
    U->>A: POST /users（名前・メール・パスワード）
    A->>DB: ハッシュ化したパスワードを保存
    A-->>U: 201（パスワードは返さない）
    U->>A: POST /auth/token（名前・パスワード）
    A->>DB: 名前でユーザーを探す
    DB-->>A: ハッシュ化されたパスワード
    A->>A: 入力と照合する
    A-->>U: アクセストークン
    U->>A: POST /tasks（Authorization: Bearer トークン）
    A->>A: トークンを検証して、誰かを特定する
    A-->>U: 201（登録者はトークンの人になる）
```

この方式を **ベアラー認証**（トークンを持っている人を、その本人とみなす認証方式）と呼びます。
`bearer` は英語で「持参人」という意味です。

作るものと、作らないものを先に示します。

| この章で作るもの | この章で作らないもの（7.6 で理由を説明します） |
|----------------|----------------------------------------|
| ユーザー登録（`POST /users`） | パスワードの再設定・メールアドレスの確認 |
| ログイン（`POST /auth/token`） | ログアウト（トークンの失効） |
| ログイン中のユーザーの取得（`GET /users/me`） | リフレッシュトークン（期限切れの自動延長） |
| トークンが要る窓口・本人だけが操作できる窓口 | 権限の役割分け（管理者・一般。演習 7.4 で入口だけ扱います） |
| パスワードのハッシュ化 | ログイン試行回数の制限・2要素認証 |

この章で新しく入れるライブラリは2つです。**どちらも自作してはいけない部分**を引き受けてくれます。

| ライブラリ | 役割 | 入れるバージョン（執筆時点） |
|-----------|------|------------------------|
| **bcrypt**（ビークリプト） | パスワードのハッシュ化と照合 | `4.2.1` |
| **PyJWT**（パイジョット） | JWT の発行と検証 | `2.10.1` |

---

## 7.2 パスワードの扱い

### 7.2.1 平文で保存してはいけない

**平文**（ひらぶん。暗号化もハッシュ化もしていない、そのままの文字列）でパスワードを保存すると、
何が起きるのかを先に確認します。

第6章で、`app.db` の中身は誰でも読めることを見ました（6.3.2）。
もう一度、いまのタスクを覗いてみてください。**`fastapi-lesson` で**実行します。

**Windows（PowerShell）**

```powershell
python -c "import sqlite3; print(sqlite3.connect('app.db').execute('select id, title, owner_email from tasks').fetchall())"
```

**macOS / Linux**

```bash
python -c "import sqlite3; print(sqlite3.connect('app.db').execute('select id, title, owner_email from tasks').fetchall())"
```

実行結果:

```text
[(1, '牛乳を買う', 'yamada@example.com'), (2, 'レポートを書く', 'suzuki@example.com'), (3, '部屋を片づける', 'yamada@example.com')]
```

**パスワードの列があれば、同じように読めます。**
つまり、`app.db` のファイルを手に入れた人は、全員のパスワードを手に入れます。

被害はこのアプリの中では終わりません。

| 起きること | 理由 |
|-----------|------|
| このアプリのアカウントが乗っ取られる | パスワードがそのまま分かるため |
| **他のサービスのアカウントも乗っ取られる** | 多くの人が、同じパスワードを複数のサービスで使い回しているため |
| 開発者自身が「見てしまう」 | データベースを覗く作業のたびに、他人のパスワードが目に入る |

2番目が最も深刻です。**あなたのアプリの事故が、あなたのアプリの外にまで広がります。**

そこで、パスワードは**元に戻せない形に変えてから**保存します。

| やり方 | 保存されるもの | 元に戻せるか | 使ってよいか |
|-------|-------------|------------|------------|
| 平文 | `password123` | 戻すまでもない | **絶対にだめ** |
| 暗号化 | 鍵で戻せる形 | 鍵があれば戻せる | パスワードには使わない（7.6.3） |
| **ハッシュ化** | `$2b$12$XynIN...` | **戻せない** | **これを使う** |

### 7.2.2 ハッシュ化する

**ハッシュ関数**（元のデータから決まった長さの値を作る関数。同じ入力からは必ず同じ値が出るが、
出た値から元の入力は求められない）を使います。
その出力を**ハッシュ値**と呼びます。

ミンチにたとえられます。**肉をミンチにはできますが、ミンチから肉の形には戻せません。**
それでいて、同じ肉からは同じミンチができるので、「同じ肉かどうか」は比べられます。

パスワード向けのハッシュ関数として広く使われているのが **bcrypt** です。
入れてください。**`fastapi-lesson` で、仮想環境を有効にした状態**で実行します
（有効化のしかたは 2.2.1 です）。

**Windows（PowerShell）**

```powershell
pip install bcrypt==4.2.1
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip install bcrypt==4.2.1
pip freeze > requirements.txt
```

```text
Successfully installed bcrypt-4.2.1
```

どんな値が出るのか、実際に見ます。
`python` と打って **REPL**（対話モードのこと。python-text 1.3.1）を起動し、
次の3行を打ってください。

```python
>>> import bcrypt
>>> bcrypt.hashpw("password123".encode("utf-8"), bcrypt.gensalt())
b'$2b$12$XynINyVsVMRXBGxXs2zp3.gInle9EGBuJp/OgT6v0txZ8vCgRKsau'
>>> bcrypt.hashpw("password123".encode("utf-8"), bcrypt.gensalt())
b'$2b$12$0ci53ymNiXuF1wAOQdpCfOKD9U6Jq6.BXZChmP1Q8WLeM.XBTMhx.'
```

**同じ `password123` なのに、結果が違います。**
これは壊れているのではなく、そういう仕組みです。

`bcrypt.gensalt()` が、毎回違う**ソルト**（ハッシュ化のときに混ぜる、
利用者ごとに違うランダムな値）を作っています。
ソルトを混ぜることで、**同じパスワードを使っている2人が、違うハッシュ値で保存されます。**

ソルトが無いと、こうなります。

| ソルト無し | ソルトあり |
|-----------|-----------|
| 同じパスワードの人は、同じハッシュ値になる | 同じパスワードでも、別々のハッシュ値になる |
| 1人分を破れば、同じ値の人を**全員まとめて破れる** | 1人ずつ試すしかない |
| よくあるパスワードのハッシュ値の一覧表と照合できる | 一覧表が使えない（ソルトが未知のため） |

出てきた文字列は、`$` で区切られた4つの部分でできています。

```text
$2b$12$XynINyVsVMRXBGxXs2zp3.gInle9EGBuJp/OgT6v0txZ8vCgRKsau
 ~~~ ~~ ~~~~~~~~~~~~~~~~~~~~~~ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  1   2          3                        4
```

| 番号 | 部分 | 意味 |
|------|------|------|
| 1 | `2b` | bcrypt の版 |
| 2 | `12` | **コスト。** 計算の重さ。1増えると計算時間が2倍になる |
| 3 | 22 文字 | **ソルト**（毎回変わる） |
| 4 | 31 文字 | ハッシュ値の本体 |

**ソルトがハッシュ値の中に一緒に入っている**ことが大事な点です。
これのおかげで、ソルトを別の列に保存しておく必要がありません。

`.encode("utf-8")` は、**文字列を「バイト列」に変換する**メソッドです。
bcrypt は文字ではなくバイト（コンピュータが扱う数値の並び）を受け取るので、変換が必要です。
python-text 7.1.4 でファイルを開くときに `encoding="utf-8"` と書いたのと、同じ話です。

> **補足：コストの数字**
> `12` は、`bcrypt` の既定値です。1回のハッシュ化に 0.2〜0.4 秒ほどかかります。
> **わざと遅くしてあります。** 総当たりで試す側の速度を落とすためです。
> 「ログインが一瞬で終わらない」のは正常な動作です。

### 7.2.3 検証する

保存されているのはハッシュ値だけで、元のパスワードには戻せません。
では、ログインのときにどうやって照合するのでしょうか。

**入力されたパスワードを、保存されているハッシュと同じソルトでハッシュ化し、値を比べます。**
これを1行でやってくれるのが `bcrypt.checkpw` です。

REPL で続けて確認してください。

```python
>>> hashed = bcrypt.hashpw("password123".encode("utf-8"), bcrypt.gensalt())
>>> bcrypt.checkpw("password123".encode("utf-8"), hashed)
True
>>> bcrypt.checkpw("password124".encode("utf-8"), hashed)
False
```

**正しいパスワードなら `True`、違えば `False`。** これだけです。

ソルトを自分で取り出す必要はありません。
7.2.2 で見たとおり、**ソルトはハッシュ値の中に入っている**ので、
`checkpw` がそこから読み取って使います。

```mermaid
flowchart LR
    A["入力された<br/>パスワード"] --> C["checkpw"]
    B["保存されている<br/>ハッシュ値<br/>（ソルト入り）"] --> C
    C --> D["True / False"]
```

この2つを、アプリから使いやすい形にまとめます。
`app/security.py` を新しく作ってください。

`app/security.py`（ファイル全体。この章の中で、あとから追記します）

```python
"""パスワードとトークンの取り扱いをまとめる場所。"""

import bcrypt


def hash_password(password: str) -> str:
    """パスワードをハッシュ化して、保存できる文字列にする。"""
    hashed = bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt())
    return hashed.decode("utf-8")


def verify_password(password: str, hashed_password: str) -> bool:
    """入力されたパスワードが、保存されているハッシュと合うか調べる。"""
    return bcrypt.checkpw(password.encode("utf-8"), hashed_password.encode("utf-8"))
```

**引数も戻り値も `str`（文字列）に揃えている**ことがポイントです。
bcrypt はバイト列を返しますが、そのままではデータベースの文字列の列に入れにくいので、
`.decode("utf-8")` で文字列に戻しています（`.encode` の逆です）。

**このファイルの外では、`bcrypt` という名前が一度も出てきません。**
将来もっと良い方式に乗り換えるとき、直すのはこのファイルだけで済みます。

動くことを確かめます。**`fastapi-lesson` で**実行してください。

**Windows（PowerShell）**

```powershell
python -c "from app.security import hash_password, verify_password; h = hash_password('password123'); print(h); print(verify_password('password123', h)); print(verify_password('password124', h))"
```

**macOS / Linux**

```bash
python -c "from app.security import hash_password, verify_password; h = hash_password('password123'); print(h); print(verify_password('password123', h)); print(verify_password('password124', h))"
```

実行結果:

```text
$2b$12$qgLM/NLzxcpX3UDg4l5m1u1HwAdr5jHHVUGuSkmQiG.NZ5EBmNyDK
True
False
```

ハッシュ値の長さは、**常に 60 文字**です。
パスワードの長さに関係なく同じ長さになるので、データベースの列も1つの長さで足ります。

> **注意：bcrypt は 72 バイトまでしか見ない**
> bcrypt には「パスワードの **73 バイト目から先を無視する**」という決まりがあります。
> しかも**エラーは出ません。黙って捨てられます。**
>
> ```python
> >>> a = "a" * 72 + "XXXX"
> >>> b = "a" * 72 + "YYYY"
> >>> h = bcrypt.hashpw(a.encode("utf-8"), bcrypt.gensalt())
> >>> bcrypt.checkpw(b.encode("utf-8"), h)
> True
> ```
>
> **違うパスワードなのに `True` になりました。** 先頭 72 バイトが同じだからです。
> 数えるのは**文字数ではなくバイト数**で、日本語は1文字 3 バイトです。
> つまり**日本語なら 24 文字で上限**に達します。
> 受け取る時点で弾くようにします（7.3.1 で `UserCreate` に検査を入れます）。

> **よくある間違い**
> **`verify_password` で `==` を使って比べる**間違いです。
>
> ```python
> def verify_password(password: str, hashed_password: str) -> bool:
>     return hash_password(password) == hashed_password      # ❌ 常に False
> ```
>
> `hash_password` は**毎回新しいソルト**を作るので（7.2.2）、
> 同じパスワードでも違うハッシュ値が出ます。**この書き方では誰もログインできません。**
> 照合は必ず `bcrypt.checkpw`（＝ `verify_password`）を通してください。

---

## 7.3 ユーザー登録

### 7.3.1 モデルとエンドポイント

ユーザーを保存するテーブルを作ります。書き方は第6章の `Task` とまったく同じです。

`app/models.py`（末尾に追記）

```python
class User(Base):
    """users テーブルの1行。"""

    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(20), unique=True)
    email: Mapped[str] = mapped_column(String(100), unique=True)
    hashed_password: Mapped[str] = mapped_column(String(100))
    created_at: Mapped[datetime | None] = mapped_column(default=datetime.now)
```

決めたことを、1つずつ確認します。

| 列 | 決めたこと | 理由 |
|----|-----------|------|
| `name` | `unique=True` | **ログインに使う名前。** 2人が同じ名前だと、どちらか分からなくなる |
| `email` | `unique=True` | 連絡先。同じメールアドレスで2つ登録できないようにする |
| `hashed_password` | `String(100)` | ハッシュ値は 60 文字（7.2.3）。将来の余裕を見て 100 |
| `created_at` | `datetime \| None` | 6.6.3 の `Task` と同じ形 |

**列の名前を `password` ではなく `hashed_password` にしている**ことに注目してください。
名前で「ここに入っているのは平文ではない」と分かるようにしています。
`password` という名前だと、あとから読む人が平文を入れてしまいます。

テーブルを作ります。**マイグレーションを1本足します**（6.6.3）。
`migrations/env.py` の `import` に `User` を足すのを忘れないでください。

`migrations/env.py`（`from app.models import Task` の行）

```diff
- from app.models import Task  # noqa: F401  Base に Task を登録するための import
+ from app.models import Task, User  # noqa: F401  Base に登録するための import
```

**Windows（PowerShell）**

```powershell
alembic revision --autogenerate -m "create users table"
```

**macOS / Linux**

```bash
alembic revision --autogenerate -m "create users table"
```

```text
INFO  [alembic.autogenerate.compare] Detected added table 'users'
Generating .../migrations/versions/e8da293f486f_create_users_table.py ...  done
```

生成されたファイルを**必ず開いて読んでください**（6.6.3）。

`migrations/versions/e8da293f486f_create_users_table.py`（一部）

```python
def upgrade() -> None:
    op.create_table('users',
    sa.Column('id', sa.Integer(), nullable=False),
    sa.Column('name', sa.String(length=20), nullable=False),
    sa.Column('email', sa.String(length=100), nullable=False),
    sa.Column('hashed_password', sa.String(length=100), nullable=False),
    sa.Column('created_at', sa.DateTime(), nullable=True),
    sa.PrimaryKeyConstraint('id'),
    sa.UniqueConstraint('email'),
    sa.UniqueConstraint('name')
    )


def downgrade() -> None:
    op.drop_table('users')
```

**`tasks` に触る行が1つもない**ことを確認してください。
`users` を作るだけなので、いま入っているタスクは消えません。

> **注意：あとから「空にできない列」を足すときは、もう1つ指定が要ります**
> ここで作った `users` は**新しいテーブル**なので、そのまま適用できます。
> しかし、**すでに行が入っているテーブル**に「空にできない列」を足すときは、
> `alembic upgrade head` が次のように失敗します。
>
> ```text
> sqlalchemy.exc.OperationalError: (sqlite3.OperationalError)
> Cannot add a NOT NULL column with default value NULL
> [SQL: ALTER TABLE users ADD COLUMN is_admin BOOLEAN NOT NULL]
> ```
>
> 「空にできない列なのに、**すでにある行に入れる値が無い**」という意味です。
> 6.6.3 では `| None`（空を許す）にして避けましたが、
> 空を許したくない列では、**生成されたファイルに `server_default` を自分で足します。**
>
> ```python
> def upgrade() -> None:
>     op.add_column(
>         'users',
>         # 既存の行に入れる値が要るので、server_default を自分で足す
>         sa.Column('is_admin', sa.Boolean(), nullable=False, server_default=sa.text('0')),
>     )
> ```
>
> `server_default` は「**データベース側の既定値**」で、
> すでにある行にはこの値が入ります（`0` は偽、`1` は真です）。
> これが、6.6.3 で言った**「生成されたファイルを必ず読んで調整する」**の実例です。
> 演習 7.4 で、実際にこの場面に出会います。

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
INFO  [alembic.runtime.migration] Running upgrade a2a7ceae0f87 -> e8da293f486f, create users table
```

次に、受け取る形と返す形を決めます（4.5.1 の「入力用と出力用を分ける」です）。

`app/schemas.py`（末尾に追記）

```python
class UserCreate(BaseModel):
    """ユーザー登録で受け取る形。"""

    name: str = Field(min_length=1, max_length=20, description="ログインに使う名前")
    email: str = Field(min_length=3, max_length=100)
    password: str = Field(min_length=8, max_length=72, description="8文字以上")

    @field_validator("password")
    @classmethod
    def password_must_fit_72_bytes(cls, value: str) -> str:
        # bcrypt は 73 バイト目から先を黙って捨てるため、ここで止める（7.2.3）
        if len(value.encode("utf-8")) > 72:
            raise ValueError("パスワードは UTF-8 で 72 バイト以内にしてください")
        return value


class UserRead(BaseModel):
    """ユーザーを返すときの形。パスワードは持たない。"""

    model_config = ConfigDict(from_attributes=True)

    id: int
    name: str
    email: str
    created_at: datetime | None = None
```

**`UserRead` に `password` も `hashed_password` も書いていない**ことが、この節でいちばん大事な点です。
4.4.2 で `owner_email` を落としたのと同じ仕組みで、**パスワードは絶対に外に出ません。**

`@field_validator`（4.3.3）で**バイト数**を数えているのは、7.2.3 の注意への対策です。
`max_length=72` は**文字数**の上限なので、日本語のパスワードでは足りません。
`len(value.encode("utf-8"))` が**バイト数**になります。

窓口を作ります。ユーザー用のルーターを新しく作ってください（5.2.1）。

`app/routers/users.py`（ファイル全体。`/me` は 7.5.2 で足します）

```python
"""ユーザーに関する窓口。"""

import logging

from fastapi import APIRouter, Depends
from sqlalchemy import or_, select
from sqlalchemy.exc import IntegrityError
from sqlalchemy.orm import Session

from app.dependencies import get_db
from app.errors import DuplicateUserError
from app.models import User
from app.schemas import UserCreate, UserRead
from app.security import hash_password

logger = logging.getLogger(__name__)

router = APIRouter(prefix="/users", tags=["users"])


@router.post("", response_model=UserRead, status_code=201)
def create_user(new_user: UserCreate, db: Session = Depends(get_db)):
    found = db.scalar(
        select(User).where(or_(User.name == new_user.name, User.email == new_user.email))
    )
    if found is not None:
        raise DuplicateUserError("名前かメールアドレス")

    user = User(
        name=new_user.name,
        email=new_user.email,
        hashed_password=hash_password(new_user.password),
    )
    db.add(user)
    try:
        db.commit()
    except IntegrityError:
        db.rollback()
        raise DuplicateUserError("名前かメールアドレス")
    db.refresh(user)
    logger.info("ユーザーを登録しました id=%s name=%s", user.id, user.name)
    return user
```

**`hashed_password=hash_password(new_user.password)`** の1行が、この章の核心です。
受け取った平文は、この1行を通ったあと、**どこにも保存されません。**

`or_` は「**どちらかに当てはまる**」という条件を作る関数です（`where` を2回書くと「両方」になります）。
重複の詳しい話は 7.3.2 で扱います。

例外を追加します。

`app/errors.py`（末尾に追記）

```python
class DuplicateUserError(Exception):
    """すでに使われている名前かメールアドレスで登録しようとしたときの例外。"""

    def __init__(self, field: str) -> None:
        self.field = field
```

`app/main.py` に、ルーターの登録と例外ハンドラを足します（5.2.2・5.4.2）。

`app/main.py`（`import` と `include_router` の部分）

```diff
- from app.errors import DuplicateCodeError
+ from app.errors import DuplicateCodeError, DuplicateUserError
  from app.models import Task
- from app.routers import misc, tasks
+ from app.routers import misc, tasks, users
```

```diff
  app.include_router(tasks.router)
+ app.include_router(users.router)
  app.include_router(misc.router)
```

`app/main.py`（末尾に追記）

```python
@app.exception_handler(DuplicateUserError)
def handle_duplicate_user(request: Request, exc: DuplicateUserError) -> JSONResponse:
    """名前やメールアドレスが重複したときの返し方を、1か所にまとめる。"""
    logger.warning("ユーザーの登録が重複しました field=%s", exc.field)
    return JSONResponse(
        status_code=409,
        content={
            "error": {
                "status": 409,
                "message": f"その{exc.field}は、すでに使われています",
                "detail": None,
            }
        },
    )
```

最後に、練習用のユーザーを `app/seed.py` に足します。
**タスクの担当者（`owner_name`）と同じ名前**にしておきます。

`app/seed.py`（ファイル全体）

```python
"""練習用のデータをデータベースに入れる。

python -m app.seed で実行する。
"""

from sqlalchemy import select

from app.database import SessionLocal
from app.models import Task, User
from app.security import hash_password


def initial_users() -> list[User]:
    """最初に入れておくユーザーを組み立てて返す。"""
    return [
        User(name="山田", email="yamada@example.com",
             hashed_password=hash_password("password123")),
        User(name="鈴木", email="suzuki@example.com",
             hashed_password=hash_password("password123")),
    ]


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
        if db.scalar(select(User)) is None:
            users = initial_users()
            for user in users:
                db.add(user)
            db.commit()
            print(f"{len(users)} 人のユーザーを登録しました。")
        else:
            print("ユーザーはすでに入っているので、何もしませんでした。")

        if db.scalar(select(Task)) is None:
            tasks = initial_tasks()
            for task in tasks:
                db.add(task)
            db.commit()
            print(f"{len(tasks)} 件のタスクを登録しました。")
        else:
            print("タスクはすでに入っているので、何もしませんでした。")
    finally:
        db.close()


if __name__ == "__main__":
    main()
```

（メモの演習を解いている場合は、第6章の演習 6.1 で足した `initial_notes()` の判定も、
同じ形でそのまま残してください。）

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
2 人のユーザーを登録しました。
タスクはすでに入っているので、何もしませんでした。
```

サーバーを起動して、`/docs` から `POST /users` を実行してください。

```json
{
  "name": "佐藤",
  "email": "sato@example.com",
  "password": "password123"
}
```

実行結果:

```json
{"id":3,"name":"佐藤","email":"sato@example.com","created_at":"2026-09-05T07:13:52.587963"}
```

**`password` が返っていません。** `UserRead` に書いていないからです。

保存されている中身も確認します。**`fastapi-lesson` で**実行してください。

**Windows（PowerShell）**

```powershell
python -c "import sqlite3; [print(row) for row in sqlite3.connect('app.db').execute('select id, name, hashed_password from users')]"
```

**macOS / Linux**

```bash
python -c "import sqlite3; [print(row) for row in sqlite3.connect('app.db').execute('select id, name, hashed_password from users')]"
```

実行結果:

```text
(1, '山田', '$2b$12$qgLM/NLzxcpX3UDg4l5m1u1HwAdr5jHHVUGuSkmQiG.NZ5EBmNyDK')
(2, '鈴木', '$2b$12$Gju6W/gzuPOxd.e5SLf3H.3h5lOeGshTIE63HJ5Q8ws38aFmzW33C')
(3, '佐藤', '$2b$12$rur2xyepF99P.8lTe0s39OEyik2Ve2VRhDiuR7OXB3tHXcgEQONai')
```

**山田さんと鈴木さんは同じ `password123` ですが、保存されている値は違います。**
7.2.2 のソルトが効いていることが、目で確認できました。

短すぎるパスワードは、ここまで来る前に弾かれます。

```json
{"name": "田中", "email": "tanaka@example.com", "password": "abc"}
```

実行結果:

```json
{"error":{"status":422,"message":"リクエストの形式が正しくありません","detail":[{"type":"string_too_short","loc":["body","password"],"msg":"String should have at least 8 characters","input":"abc","ctx":{"min_length":8}}]}}
```

`input` に **`"abc"` がそのまま出ている**ことに気づいた人もいるかもしれません。
これは開発中は便利ですが、本番では出さないほうがよい情報です。
**`422` の `detail` をそのまま画面やログに出さない**、という判断が要ります（7.6.1）。

> **よくある間違い**
> **`UserRead` を作らず、`response_model` を付けない**間違いです。
> `response_model` が無い窓口は、返したものをそのまま JSON にします（6.4.2）。
>
> ```json
> {"id":3,"name":"佐藤","email":"sato@example.com","hashed_password":"$2b$12$rur2xye...","created_at":"..."}
> ```
>
> **ハッシュ値が外に出ました。** 平文よりはましですが、
> 手元に持ち帰ってゆっくり試せる状態を渡したことになります。
> **ユーザーを返す窓口には、必ず `response_model=UserRead` を付けてください。**

### 7.3.2 重複チェック

`create_user` には、重複への備えが**2つ**入っています。
第6章の `create_task`（6.4.1・6.5.2）とまったく同じ組み合わせです。

| 備え | 役割 |
|------|------|
| 事前の `select` | 普通の場合に、**分かりやすいエラー**（`409`）を返す |
| `unique=True` + `IntegrityError` | 確認の直後に別のリクエストが登録した場合に、**`500` にしない** |

同じ名前でもう一度登録してみてください。

```json
{"name": "佐藤", "email": "sato2@example.com", "password": "password123"}
```

実行結果:

```json
{"error":{"status":409,"message":"その名前かメールアドレスは、すでに使われています","detail":null}}
```

**メッセージが「名前かメールアドレス」と、あいまいになっている**ことに気づいたでしょうか。
これは手抜きではなく、意図的な書き方です。

「そのメールアドレスは登録済みです」と正確に返すと、
**「このサービスに、このメールアドレスの人が登録しているか」を誰でも調べられます。**
これを**アカウント列挙**（登録されている利用者を、外から総当たりで洗い出すこと）と呼びます。

| 返し方 | 親切さ | 危険 |
|-------|-------|------|
| 「そのメールアドレスは登録済みです」 | 分かりやすい | **登録の有無が外から分かる** |
| 「その名前かメールアドレスは、すでに使われています」 | やや不親切 | どちらが原因かは分からない |

**このテキストでは後者を選びます。** ログイン API でも同じ考え方を使います（7.5.1）。

> **補足：使い分けは、サービスの性質で決まります**
> 社内向けのツールのように、利用者の一覧がそもそも公開されているなら、
> 正確なメッセージのほうが親切です。
> **「隠す情報を決めるのは開発者の判断」**であって、常に隠すのが正解ではありません。
> ただし、**判断せずに全部を返してしまう**のが、いちばんよくありません。

> **よくある間違い**
> **事前の `select` だけ書いて、`IntegrityError` を受け止めない**間違いです。
> 練習中は1人で操作するので、まず起きません。
> ところが利用者が増えると、**確認した直後・保存する直前に別の人が同じ名前で登録する**ことが
> 現実に起こり、`500` になります（6.5.2）。
> `db.rollback()` の書き忘れにも注意してください。

---

## 7.4 JWT

### 7.4.1 JWT とは

ログインができたとして、**その次のリクエストで「さっきの人だ」と分かる必要があります。**

素直に考えると、毎回パスワードを送る方法があります。**しかし、これはだめです。**

| 毎回パスワードを送る場合の問題 |
|---------------------------|
| ブラウザやアプリが、パスワードを**ずっと持っておく**ことになる |
| リクエストのたびに、パスワードがネットワークを流れる |
| 通信ログや履歴に、パスワードが残る可能性が増える |
| パスワードには有効期限を付けられない |

そこで、ログインのときに1回だけパスワードを送り、
代わりに**トークン**（本人であることを示す、期限付きの引換券のような文字列）を受け取ります。
以降はトークンだけを送ります。

**コインロッカーの鍵**にたとえられます。
中身（あなたが誰か）を毎回説明しなくても、鍵を見せれば通ります。
落とせば拾った人に使われますし（7.6.2）、期限が来れば使えなくなります（7.4.3）。

このトークンの形式として広く使われているのが **JWT**（JSON Web Token。ジョット、
またはジェイダブリューティー）です。
7.4.2 で発行しますが、先に**どんな形なのか**を見ておきます。

JWT は `.`（ドット）で区切られた**3つの部分**でできています。

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJcdTRmNTBcdTg1ZTQiLCJpYXQiOjE3ODg1OTI0MzIsImV4cCI6MTc4ODU5NDIzMn0.CP9P-jTHyzucM86oA1oLuSTVLteiLkhBXR8N77y5jiE
```

```mermaid
flowchart LR
    A["ヘッダー<br/>どの方式で署名したか"] --> D["."]
    D --> B["ペイロード<br/>誰か・いつまで有効か"]
    B --> E["."]
    E --> C["署名<br/>改ざんされていない証拠"]
```

| 部分 | 中身 | 読めるか |
|------|------|---------|
| ヘッダー | 署名の方式（`HS256` など） | **読める** |
| ペイロード | **誰か・いつまで有効か**などの情報 | **読める** |
| 署名 | 秘密鍵で作った、改ざん検出用の値 | 読めても意味がない |

**「読める」というのは、誰でも読めるという意味です。**
実際に中身を覗いてみます（7.4.2 でトークンを発行したら、自分のもので試してください）。

**Windows（PowerShell） / macOS / Linux 共通**

```python
>>> import base64
>>> token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJcdTRmNTBcdTg1ZTQiLCJpYXQiOjE3ODg1OTI0MzIsImV4cCI6MTc4ODU5NDIzMn0.CP9P-jTHyzucM86oA1oLuSTVLteiLkhBXR8N77y5jiE"
>>> payload = token.split(".")[1]
>>> base64.urlsafe_b64decode(payload + "=" * (-len(payload) % 4)).decode()
'{"sub":"\\u4f50\\u85e4","iat":1788592432,"exp":1788594232}'
```

**鍵も何も無しで、中身が読めました。**
`佐藤` は「佐藤」を数字で表した書き方です（JSON の決まりで、
日本語はこの形に置き換えられることがあります。1.3.2）。

ここから、2つの大事な結論が出ます。

| 結論 | 理由 |
|------|------|
| **JWT に秘密の情報を入れてはいけない** | 誰でも読めるため。**暗号化ではありません** |
| **中身を書き換えても、サーバーは気づく** | 署名が合わなくなるため（7.4.4 で確かめます） |

`sub` / `iat` / `exp` は、JWT で名前が決まっている項目です。

| 項目 | 読み | 意味 |
|------|------|------|
| `sub` | サブジェクト | **このトークンが誰のものか** |
| `iat` | イシュードアット | いつ発行されたか |
| `exp` | エクスパイア | **いつまで有効か**（7.4.3） |

時刻が `1788592432` という数字になっているのは、
**1970年1月1日からの経過秒数**で表す決まりだからです（コンピュータで時刻を表す標準的なやり方です）。

### 7.4.2 トークンを発行する

PyJWT を入れます。**`fastapi-lesson` で、仮想環境を有効にした状態**で実行してください。

**Windows（PowerShell）**

```powershell
pip install pyjwt==2.10.1
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip install pyjwt==2.10.1
pip freeze > requirements.txt
```

```text
Successfully installed PyJWT-2.10.1
```

> **注意：似た名前のパッケージがあります**
> `pip install jwt` や `pip install python-jwt` は**別のライブラリ**です。
> `import jwt` と書くところは同じなので、入れ違えると
> 「`encode` に `algorithm` という引数がない」といった食い違いで詰まります。
> **入れるのは `pyjwt` です。**

署名に使う**秘密鍵**（署名を作るときに使う、外に出してはいけない文字列）を用意します。
**自分で考えた文字列を使わないでください。** ランダムに作ります。

**Windows（PowerShell）**

```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```

**macOS / Linux**

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

```text
32e8191bf370e4a2b6743d7c4eff61ebba9425ae57c3d3b450e943d81de12740
```

`secrets` は、**推測されにくい値を作るための標準ライブラリ**です。
`token_hex(32)` は 32 バイト分をランダムに作り、16 進数の 64 文字で返します。

**出てきた値は、人によって違います。** あなたの画面に出た値を使ってください。

これを `.env` に書きます（4.6.2）。

`.env`（2行追記）

```text
SECRET_KEY=ここに、いま生成した値を貼る
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

`.env.example` にも書きますが、**値は伏せます**（4.6.3）。

`.env.example`（2行追記）

```text
SECRET_KEY=python -c "import secrets; print(secrets.token_hex(32))" で作った値を入れる
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

設定に読み込みます。

`app/config.py`（`database_url` の下に2行）

```diff
      database_url: str = "sqlite:///./app.db"
+     secret_key: str
+     access_token_expire_minutes: int = 30
```

**`secret_key` にだけ、デフォルト値を書いていません。**
これは意図的です。デフォルト値が無い項目は、
**`.env` に書き忘れると、サーバーが起動した瞬間に落ちます。**

```text
pydantic_core._pydantic_core.ValidationError: 1 validation error for Settings
secret_key
  Field required [type=missing, input_value={}, input_type=dict]
```

「動くけれど、誰も知らない適当な鍵で動いている」という状態を作らないための書き方です。

トークンを作る関数を、`app/security.py` に足します。

`app/security.py`（`import` を3行足し、末尾に関数を1つ追記）

```diff
  """パスワードとトークンの取り扱いをまとめる場所。"""
  
+ from datetime import datetime, timedelta, timezone
+ 
  import bcrypt
+ import jwt
+ 
+ from app.config import settings
+ 
+ ALGORITHM = "HS256"
```

`app/security.py`（末尾に追記）

```python
def create_access_token(user_name: str) -> str:
    """ログインしたユーザーの名前を入れたトークンを作る。"""
    now = datetime.now(timezone.utc)
    payload = {
        "sub": user_name,
        "iat": now,
        "exp": now + timedelta(minutes=settings.access_token_expire_minutes),
    }
    return jwt.encode(payload, settings.secret_key, algorithm=ALGORITHM)
```

| 部分 | 意味 |
|------|------|
| `payload` | トークンに入れる中身（7.4.1）。**辞書で書く** |
| `settings.secret_key` | 署名に使う秘密鍵。`.env` から読む |
| `algorithm=ALGORITHM` | 署名の方式。`HS256` は「秘密鍵1つで、作るのも確かめるのも行う」方式 |

`datetime.now(timezone.utc)` の **`timezone.utc`** は、
「**世界共通の時刻で数える**」という指定です。
パソコンの時刻設定が国ごとに違っても、有効期限がずれないようにするために付けます。

動かしてみます。**`fastapi-lesson` で**実行してください。

**Windows（PowerShell）**

```powershell
python -c "from app.security import create_access_token; print(create_access_token('佐藤'))"
```

**macOS / Linux**

```bash
python -c "from app.security import create_access_token; print(create_access_token('佐藤'))"
```

実行結果:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJcdTRmNTBcdTg1ZTQiLCJpYXQiOjE3ODg1OTI0MzIsImV4cCI6MTc4ODU5NDIzMn0.CP9P-jTHyzucM86oA1oLuSTVLteiLkhBXR8N77y5jiE
```

7.4.1 で覗いたトークンが、これで自分の手で作れました。

> **よくある間違い**
> **`sub` に、ユーザー名ではなく `User` オブジェクトを入れる**間違いです。
>
> ```python
> payload = {"sub": user}          # ❌ Object of type User is not JSON serializable
> ```
>
> ペイロードは JSON になるので、**入れられるのは文字列・数値・真偽値・リスト・辞書だけ**です（1.3.3）。
> このテキストでは `sub` に**名前**を入れています。`id` を入れる作りもよく使われます。

### 7.4.3 有効期限

`exp` を入れたので、トークンには期限があります。
このテキストでは 30 分にしました（`ACCESS_TOKEN_EXPIRE_MINUTES=30`）。

**なぜ短くするのか。** 理由は1つです。

**発行したトークンは、あとから取り消せないからです。**

7.4.4 で見るとおり、トークンの検証は**署名と期限を確かめるだけ**で行います。
データベースを見ません。だから速いのですが、その裏返しとして
**「このトークンはもう無効にしたい」と思っても、方法がありません。**

| 期限 | 盗まれたときに使われ続ける時間 | 使い勝手 |
|------|--------------------------|---------|
| 5分 | 5分 | 頻繁にログインし直すことになる |
| **30分**（このテキスト） | 30分 | 練習にはちょうどよい |
| 30日 | **30日** | 楽だが、事故のときの被害が大きい |

実際のサービスでは、「短い有効期限のトークン」と
「それを作り直すためのリフレッシュトークン」を組み合わせます。
**このテキストでは扱いません**（7.6 の表を見てください）。

`.env` の値を変えれば、期限を変えられます。**サーバーの再起動が必要です**（4.6.2）。

```text
ACCESS_TOKEN_EXPIRE_MINUTES=1
```

演習で期限切れを試すときは、この形で短くしてください。
**試したあとは 30 に戻すのを忘れないでください。**

### 7.4.4 トークンを検証する

受け取ったトークンが**本物かどうか**を確かめます。確かめることは3つです。

| 確かめること | だめだったときに起きること |
|------------|------------------------|
| 形が JWT になっているか | `InvalidTokenError` |
| **署名が、自分の秘密鍵で作られたものか** | `InvalidSignatureError` |
| **`exp` が過ぎていないか** | `ExpiredSignatureError` |

`app/security.py` に追記します。

`app/security.py`（末尾に追記）

```python
def read_token(token: str) -> str | None:
    """トークンを検証して、中のユーザー名を取り出す。だめなら None。"""
    try:
        payload = jwt.decode(token, settings.secret_key, algorithms=[ALGORITHM])
    except jwt.ExpiredSignatureError:
        return None
    except jwt.InvalidTokenError:
        return None
    return payload.get("sub")
```

`jwt.decode` は、**署名の確認と期限の確認を両方やってから**中身を返します。
`algorithms=[ALGORITHM]` は「**この方式で署名されたものだけ受け付ける**」という指定です
（リストで渡す決まりになっています）。

`try` / `except` は python-text 7.5 で学んだ形です。
2つの例外を別々に書いていますが、**`ExpiredSignatureError` は `InvalidTokenError` の一種**なので、
順番を入れ替えると期限切れのほうが下に隠れます。**細かいほうを先に書きます**（python-text 7.5.3）。

いまは戻り値を `None` にまとめていますが、
**7.5.2 で「期限切れ」と「そもそも不正」を区別したくなったときは、
この関数を分ければよい**という形にしてあります。

本当に改ざんを検出できるのか、確かめます。**`fastapi-lesson` で**実行してください。

**Windows（PowerShell） / macOS / Linux 共通**

```python
>>> import jwt, base64, json
>>> from app.config import settings
>>> from app.security import ALGORITHM, create_access_token, read_token
>>> token = create_access_token("佐藤")
>>> read_token(token)
'佐藤'
>>> head, payload, sig = token.split(".")
>>> bad = base64.urlsafe_b64encode(json.dumps({"sub": "山田"}).encode()).decode().rstrip("=")
>>> jwt.decode(f"{head}.{bad}.{sig}", settings.secret_key, algorithms=[ALGORITHM])
Traceback (most recent call last):
  ...
jwt.exceptions.InvalidSignatureError: Signature verification failed
```

**「佐藤」を「山田」に書き換えたトークンは、はねられました。**

ペイロードは誰でも書き換えられます（7.4.1）。
しかし**署名を作り直すには秘密鍵が要る**ので、鍵を知らない人は正しいトークンを作れません。
これが JWT の仕組みです。

秘密鍵が違うだけでも、同じように失敗します。

```python
>>> jwt.decode(token, "wrong-key", algorithms=[ALGORITHM])
Traceback (most recent call last):
  ...
jwt.exceptions.InvalidSignatureError: Signature verification failed
```

**`SECRET_KEY` を変えると、それまでに発行したトークンは全部無効になります。**
不便に見えますが、**鍵が漏れたときに全員を強制ログアウトさせる唯一の手段**でもあります。

期限切れも確かめておきます。

```python
>>> from datetime import datetime, timedelta, timezone
>>> now = datetime.now(timezone.utc)
>>> expired = jwt.encode({"sub": "佐藤", "iat": now - timedelta(hours=2), "exp": now - timedelta(hours=1)}, settings.secret_key, algorithm=ALGORITHM)
>>> jwt.decode(expired, settings.secret_key, algorithms=[ALGORITHM])
Traceback (most recent call last):
  ...
jwt.exceptions.ExpiredSignatureError: Signature has expired
>>> read_token(expired)
>>>
```

最後の `read_token(expired)` は、**何も表示されていません。**
`None` が返ったときは REPL に何も出ないためです（python-text 2.2.4）。

> **注意：`jwt.decode` に `algorithms` を必ず渡してください**
> 省略すると `DecodeError` になりますが、
> 「面倒だから」と `algorithms=["none"]` のような指定を見かけても、**真似しないでください。**
> `none` は「署名なし」を意味し、**誰でも好きな中身のトークンを作れる**ようになります。
> 検索して出てきた古いコードに、この書き方が残っていることがあります。

---

## 7.5 ログインと保護されたエンドポイント

### 7.5.1 ログイン API

ログインの窓口を作ります。やることは3つです。

1. 名前でユーザーを探す
2. パスワードを照合する（7.2.3）
3. 合っていればトークンを発行して返す（7.4.2）

まず、返す形を決めます。

`app/schemas.py`（末尾に追記）

```python
class Token(BaseModel):
    """ログインに成功したときに返す形。"""

    access_token: str
    token_type: str = "bearer"
```

`token_type` は「このトークンの使い方」を表す項目で、
ベアラー認証（7.1.2）では `bearer` という決まった文字列を返します。

ルーターを新しく作ります。

`app/routers/auth.py`（ファイル全体）

```python
"""ログインの窓口。"""

import logging

from fastapi import APIRouter, Depends, HTTPException
from fastapi.security import OAuth2PasswordRequestForm
from sqlalchemy import select
from sqlalchemy.orm import Session

from app.dependencies import get_db
from app.models import User
from app.schemas import Token
from app.security import create_access_token, verify_password

logger = logging.getLogger(__name__)

router = APIRouter(prefix="/auth", tags=["auth"])


@router.post("/token", response_model=Token)
def login(
    form: OAuth2PasswordRequestForm = Depends(),
    db: Session = Depends(get_db),
):
    user = db.scalar(select(User).where(User.name == form.username))
    if user is None or not verify_password(form.password, user.hashed_password):
        logger.warning("ログインに失敗しました name=%s", form.username)
        raise HTTPException(
            status_code=401,
            detail="ユーザー名またはパスワードが違います",
            headers={"WWW-Authenticate": "Bearer"},
        )
    logger.info("ログインしました name=%s", user.name)
    return {"access_token": create_access_token(user.name), "token_type": "bearer"}
```

`app/main.py` に登録します。

```diff
- from app.routers import misc, tasks, users
+ from app.routers import auth, misc, tasks, users
```

```diff
  app.include_router(users.router)
+ app.include_router(auth.router)
  app.include_router(misc.router)
```

新しいものが3つあります。順に見ます。

**1つ目：`OAuth2PasswordRequestForm`**

これは FastAPI が用意している、**ログイン用の受け取り方**です。
`Depends()` と**括弧付きで**書く点だけ、いままでの `Depends(関数)` と違います
（クラスなので、その場で作ってもらう形になります）。

受け取れる項目は決まっています。

| 項目 | 中身 |
|------|------|
| `form.username` | ログインに使う名前 |
| `form.password` | パスワード |

**JSON ではなく「フォーム形式」で受け取る**点が、これまでの窓口と違います。
フォーム形式は、`username=佐藤&password=password123` のように
`&` で繋いだ形（3.2.1 のクエリパラメータと同じ書き方）でボディを送るやり方です。

なぜ JSON ではないかというと、**この形が OAuth2 という標準で決められていて、
`/docs` の「Authorize」ボタンがこの形を送るように作られているから**です（7.5.2）。
自分で JSON を受け取る形にすることもできますが、その場合 `/docs` から試せなくなります。

フォーム形式を受け取るには `python-multipart` というライブラリが要りますが、
**第2章で `fastapi[standard]` を入れたときに一緒に入っています。** 確認しておきます。

**Windows（PowerShell）**

```powershell
pip list | Select-String multipart
```

**macOS / Linux**

```bash
pip list | grep -i multipart
```

```text
python-multipart  0.0.32
```

**2つ目：`user is None or not verify_password(...)`**

「**ユーザーが見つからない**」と「**パスワードが違う**」を、**1つの `if` にまとめています。**
そして返すメッセージも1つです。

これは 7.3.2 と同じ理由です。
「そのユーザーは存在しません」と返すと、**存在するユーザー名を外から総当たりで探せます。**

| 返し方 | 攻撃する側から見ると |
|-------|------------------|
| 「ユーザーが存在しません」／「パスワードが違います」を区別 | **まず名前を特定してから、パスワードだけを攻める**ことができる |
| どちらも「ユーザー名またはパスワードが違います」 | 名前とパスワードの両方を同時に当てる必要がある |

**3つ目：`WWW-Authenticate: Bearer` ヘッダー**

`401` を返すときに付ける決まりのヘッダーで、
「このアプリはベアラー認証を使っています」と相手に伝えます。

**ログには `form.password` を絶対に書かないでください。**
上のコードでは、失敗したときに `name` だけを残しています（5.5.3）。

動かします。`/docs` からでも試せますが、**まず `curl` で送って形を見てください。**

**Windows（PowerShell）**

```powershell
curl.exe -X POST http://127.0.0.1:8000/auth/token --data-urlencode "username=佐藤" --data-urlencode "password=password123"
```

**macOS / Linux**

```bash
curl -X POST http://127.0.0.1:8000/auth/token --data-urlencode "username=佐藤" --data-urlencode "password=password123"
```

実行結果:

```json
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJcdTRmNTBcdTg1ZTQiLCJpYXQiOjE3ODg1OTI0MzIsImV4cCI6MTc4ODU5NDIzMn0.CP9P-jTHyzucM86oA1oLuSTVLteiLkhBXR8N77y5jiE","token_type":"bearer"}
```

**トークンが返ってきました。** この文字列を 7.5.2 で使います。

パスワードを間違えると、こうなります。

```text
{"error":{"status":401,"message":"ユーザー名またはパスワードが違います","detail":null}}
```

存在しない名前で試しても、**まったく同じ**です。

> **よくある間違い**
> **`-d "username=佐藤&password=password123"` と書く**間違いです。
> 日本語を `-d` にそのまま書くと、**サーバー側で名前が一致せず `401` になります。**
> 日本語（ASCII 以外の文字）は、送る前に変換が必要だからです（3.5.2 のパーセントエンコーディング）。
>
> `--data-urlencode` は、その変換をしてから送ってくれるオプションです。
> **項目ごとに1つずつ**書きます（`&` で繋がず、`--data-urlencode` を2回書く）。
> 面倒であれば、**`/docs` から実行してください**（7.5.2）。

### 7.5.2 `Depends` で現在のユーザーを取得する

トークンを受け取って、**いまログインしている人**を取り出す部品を作ります。
5.3 で学んだ `Depends` が、ここで最も効いてきます。

`app/dependencies.py`（`import` の部分）

```diff
  from fastapi import Depends, HTTPException, Path, Query
+ from fastapi.security import OAuth2PasswordBearer
+ from sqlalchemy import select
  from sqlalchemy.orm import Session
  
  from app.database import SessionLocal
- from app.models import Task
+ from app.models import Task, User
+ from app.security import read_token
```

`app/dependencies.py`（`get_db` の上に追記）

```python
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/token")
```

`app/dependencies.py`（末尾に追記）

```python
def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: Session = Depends(get_db),
) -> User:
    """トークンから、いまログインしているユーザーを取り出す。"""
    name = read_token(token)
    if name is None:
        raise HTTPException(
            status_code=401,
            detail="トークンが正しくないか、有効期限が切れています",
            headers={"WWW-Authenticate": "Bearer"},
        )
    user = db.scalar(select(User).where(User.name == name))
    if user is None:
        raise HTTPException(
            status_code=401,
            detail="トークンが正しくないか、有効期限が切れています",
            headers={"WWW-Authenticate": "Bearer"},
        )
    return user
```

**`OAuth2PasswordBearer`** は、リクエストから
`Authorization: Bearer <トークン>` というヘッダーを取り出す依存です。
ヘッダーが無ければ、**窓口の関数を呼ぶ前に `401` で止めます**（5.3.1 の「依存が先」）。

`tokenUrl="auth/token"` は、**`/docs` に「ここでログインできます」と教えるための指定**です。
7.5.1 で作った窓口の URL を書きます（先頭の `/` は付けません）。

**トークンが正しくても、そのユーザーが消えている**ことがあります
（退会したあとに、期限内のトークンを使われた場合です）。
そのため、**名前を取り出したあとにデータベースを引き直しています。**

依存の関係を図にすると、こうなります。

```mermaid
flowchart LR
    A["Authorization<br/>ヘッダー"] --> B["oauth2_scheme<br/>トークンの文字列を取り出す"]
    B --> C["get_current_user<br/>署名と期限を確かめ<br/>ユーザーを引く"]
    D["get_db<br/>セッション"] --> C
    C --> E["窓口の関数<br/>current_user"]
```

窓口を1つ作って、動きを確かめます。

`app/routers/users.py`（末尾に追記）

```python
@router.get("/me", response_model=UserRead)
def read_me(current_user: User = Depends(get_current_user)):
    return current_user
```

```diff
- from app.dependencies import get_db
+ from app.dependencies import get_current_user, get_db
```

**この窓口には、認証の処理が1行も書かれていません。**
`Depends(get_current_user)` と書いただけで、
「トークンが無ければ `401`、あれば `current_user` が入っている」状態になります。
これが依存性注入の効き目です（5.3.2）。

ここで、**第5章で書いた例外ハンドラを1か所だけ直します。**
いまのままだと、`HTTPException` に付けた `WWW-Authenticate` ヘッダーが消えてしまうためです。

`app/main.py`（`handle_http_exception` の部分）

```diff
  @app.exception_handler(StarletteHTTPException)
  def handle_http_exception(request: Request, exc: StarletteHTTPException) -> JSONResponse:
      """HTTPException を、統一した形に変換して返す。"""
      return JSONResponse(
          status_code=exc.status_code,
          content={
              "error": {"status": exc.status_code, "message": exc.detail, "detail": None}
          },
+         headers=exc.headers,
      )
```

この1行が無いと、`401` は返るのに `WWW-Authenticate` ヘッダーだけが落ちます。
**自分で作ったレスポンスは、自分で組み立てた分しか持っていない**ためです。

動かします。まず、**トークンなしで**呼んでみてください。

**Windows（PowerShell）**

```powershell
curl.exe -i http://127.0.0.1:8000/users/me
```

**macOS / Linux**

```bash
curl -i http://127.0.0.1:8000/users/me
```

実行結果:

```text
HTTP/1.1 401 Unauthorized
www-authenticate: Bearer
content-type: application/json

{"error":{"status":401,"message":"Not authenticated","detail":null}}
```

次に、7.5.1 で受け取ったトークンを付けます。
**`Bearer ` と半角スペースを付けてから**トークンを書きます。

**Windows（PowerShell）**

```powershell
curl.exe http://127.0.0.1:8000/users/me -H "Authorization: Bearer ここにトークンを貼る"
```

**macOS / Linux**

```bash
curl http://127.0.0.1:8000/users/me -H "Authorization: Bearer ここにトークンを貼る"
```

実行結果:

```json
{"id":3,"name":"佐藤","email":"sato@example.com","created_at":"2026-09-05T07:13:52.587963"}
```

**トークンだけで、誰かが分かりました。**

`401` のメッセージが2種類あることに気づいたでしょうか。

| メッセージ | 出どころ | 意味 |
|-----------|---------|------|
| `Not authenticated` | `oauth2_scheme` | **`Authorization` ヘッダーが無い**（付け忘れ） |
| `トークンが正しくないか、有効期限が切れています` | `get_current_user` | ヘッダーはあるが、中身がだめ |

**この2つの区別は、詰まったときにそのまま原因になります。**
前者ならヘッダーの付け忘れ、後者ならトークンそのものの問題です。

毎回トークンを貼り付けるのは大変なので、**`/docs` から使う方法**を覚えてください。

1. ブラウザで `http://127.0.0.1:8000/docs` を開く
2. 右上の **「Authorize」ボタン**を押す
3. `username` と `password` を入力して「Authorize」を押す（`client_id` などは空のままで構いません）
4. 「Close」を押す

これだけで、**以降 `/docs` から実行するリクエストには、自動的にトークンが付きます。**
鍵のアイコンが開いた状態になり、`GET /users/me` を `Execute` すれば、自分の情報が返ります。

**日本語の名前も、`/docs` からなら変換を気にせず入力できます**（7.5.1 のよくある間違い）。
この章の残りは、`/docs` から試すのがいちばん楽です。

> **よくある間違い**
> **`Authorization: ここにトークン` と、`Bearer ` を書き忘れる**間違いです。
> `Not authenticated` の `401` になります。
> `oauth2_scheme` は「`Bearer ` で始まっているか」まで見ているためです。
> **`Bearer` の後ろの半角スペースは1つ**です。

> **補足：`/docs` でログインし直したいとき**
> 有効期限が切れると、`/docs` から実行しても `401` が返るようになります。
> もう一度「Authorize」を押し、**「Logout」してから入り直して**ください。
> ブラウザを再読み込みしただけでは、古いトークンが残っていることがあります。

### 7.5.3 認証が必要なエンドポイントを作る

いよいよ、タスクの窓口を守ります。

**どこを守るのか**を先に決めます。このテキストでは、次のようにします。

| 窓口 | 認証 | 認可 | 理由 |
|------|------|------|------|
| `GET /tasks`・`GET /tasks/{id}` | 不要 | — | 第9章で、ログイン前の画面にも一覧を出すため |
| **`POST /tasks`** | **必要** | — | 登録した人を記録するため |
| **`PATCH /tasks/{id}`** | **必要** | **本人のみ** | 他人のタスクを書き換えられないようにするため |
| **`DELETE /tasks/{id}`** | **必要** | **本人のみ** | 他人のタスクを消せないようにするため |

**「読むのは誰でも、書き換えるのは本人だけ」**という決め方です。
実際のサービスでは一覧も守ることが多いのですが、
**まず認可の考え方をはっきり見せるために、この形にします。**

**まず `POST /tasks` から**です。ここで1つ、設計が変わります。

いままで、登録者は**リクエストのボディで送っていました。**

```json
{"title": "郵便を出す", "owner": {"name": "山田", "email": "yamada@example.com"}}
```

**これは、もう受け取りません。** 誰でも「山田」を名乗れてしまうからです。
**登録者は、トークンから決めます。**

`app/schemas.py`（`TaskCreate` から `owner` の行を消す）

```diff
  class TaskCreate(TaskBase):
      """登録するときに受け取る形。"""
  
-     owner: Owner
- 
      @field_validator("title")
```

`Owner`（`name` と `email` を持つほう）は、これで使われなくなります。
`app/schemas.py` から削除して構いません。
**`OwnerRead`（返すときに使う、`name` だけのほう）は残してください。**

`app/routers/tasks.py`（`create_task` の部分）

```python
@router.post("", response_model=TaskRead, status_code=201)
def create_task(
    new_task: TaskCreate,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db),
):
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
        # 登録者は、送られてきた値ではなくトークンから決める
        owner_name=current_user.name,
        owner_email=current_user.email,
    )
    db.add(task)
    try:
        db.commit()
    except IntegrityError:
        db.rollback()
        raise DuplicateCodeError(new_task.code)
    db.refresh(task)
    logger.info("タスクを登録しました id=%s title=%s", task.id, task.title)
    return task
```

**次に、本人だけが操作できるようにします。**

「見つからなければ `404`」を依存にまとめたように（5.4.1・6.4.2）、
「**自分のものでなければ `403`**」も依存にまとめます。

`app/dependencies.py`（末尾に追記）

```python
def get_my_task(
    task: Task = Depends(get_task_or_404),
    current_user: User = Depends(get_current_user),
) -> Task:
    """自分のタスクだけを取り出す。他人のものなら 403 で止める。"""
    if task.owner_name != current_user.name:
        raise HTTPException(
            status_code=403,
            detail="このタスクを操作する権限がありません",
        )
    return task
```

**依存が、2つの依存を組み合わせている**ことに注目してください。

| 使っている依存 | 役割 | 通らなければ |
|--------------|------|------------|
| `get_task_or_404` | id でタスクを探す（6.4.2） | **`404`** |
| `get_current_user` | トークンから本人を取り出す（7.5.2） | **`401`** |
| `get_my_task` 自身 | 2つを突き合わせる | **`403`** |

5.3.3 で学んだ「依存は依存を持てる」が、**3段の積み重ね**になりました。
`404` → `401` → `403` の判断が、窓口の関数に1行も書かずに揃います。

```mermaid
flowchart TD
    A["DELETE /tasks/1"] --> B["get_task_or_404<br/>あるか"]
    B -->|無い| C["404"]
    B --> D["get_current_user<br/>誰か"]
    D -->|トークンなし・不正| E["401"]
    D --> F["get_my_task<br/>自分のものか"]
    F -->|他人のもの| G["403"]
    F --> H["delete_task が実行される"]
```

**順番は、`get_my_task` の引数を書いた順**で決まります。
`task` を先に書いたので、**存在しない `id` は、トークンが無くても `404`** になります。

```text
DELETE /tasks/999（トークンなし）
{"error":{"status":404,"message":"id 999 のタスクは見つかりませんでした","detail":null}}
```

引数の順番を入れ替えれば、先に `401` を返す形にもできます。
**どちらが正しいということはなく、「何を隠したいか」で決めます。**
このアプリは `GET /tasks/{id}` を誰でも読める設計にしたので（上の表）、
**タスクが存在するかどうかは、そもそも秘密ではありません。**
一覧まで認証必須にするなら、先に `401` を返す順番のほうが筋が通ります。

窓口は、`Depends` を差し替えるだけです。

`app/routers/tasks.py`（`update_task` と `delete_task` の部分）

```diff
  @router.patch("/{task_id}", response_model=TaskRead)
  def update_task(
      new_task: TaskUpdate,
-     task: Task = Depends(get_task_or_404),
+     task: Task = Depends(get_my_task),
      db: Session = Depends(get_db),
  ):
```

```diff
  @router.delete("/{task_id}", status_code=204)
- def delete_task(task: Task = Depends(get_task_or_404), db: Session = Depends(get_db)):
+ def delete_task(task: Task = Depends(get_my_task), db: Session = Depends(get_db)):
```

`import` を整えます。

`app/routers/tasks.py`（`import` の部分）

```diff
  from app.dependencies import (
+     get_current_user,
      get_db,
+     get_my_task,
      get_owner_name,
      get_task_or_404,
      list_params,
  )
  from app.errors import DuplicateCodeError
- from app.models import Task
+ from app.models import Task, User
```

（`get_task_or_404` は `GET /tasks/{task_id}` でまだ使うので、残します。）

**通しで動かします。** サーバーを起動し、`/docs` の「Authorize」から
**佐藤さん**（7.3.1 で登録した人）でログインしてください。

**1. トークンなしで登録しようとする**

**Windows（PowerShell）**

```powershell
curl.exe -i -X POST http://127.0.0.1:8000/tasks -H "Content-Type: application/json" -d "{\"title\":\"郵便を出す\"}"
```

**macOS / Linux**

```bash
curl -i -X POST http://127.0.0.1:8000/tasks -H "Content-Type: application/json" -d '{"title":"郵便を出す"}'
```

```text
HTTP/1.1 401 Unauthorized
www-authenticate: Bearer

{"error":{"status":401,"message":"Not authenticated","detail":null}}
```

**2. ログインして登録する**（`/docs` から `POST /tasks` を実行）

```json
{"title": "郵便を出す"}
```

実行結果:

```json
{"title":"郵便を出す","done":false,"code":null,"priority":3,"tags":[],"id":4,"owner":{"name":"佐藤"},"created_at":"2026-09-05T07:13:53.250445"}
```

**`owner` を送っていないのに、`{"name":"佐藤"}` が入っています。**
トークンから決まったからです。

**3. 他人のタスクを消そうとする**（`/docs` から `DELETE /tasks/1` を実行。
`id` が 1 のタスクは山田さんのものです）

```json
{"error":{"status":403,"message":"このタスクを操作する権限がありません","detail":null}}
```

**4. 自分のタスクを消す**（`/docs` から `DELETE /tasks/4` を実行。いま登録したもの）

```text
HTTP/1.1 204 No Content
```

**同じ `DELETE` でも、相手によって `403` と `204` に分かれました。**
7.1.1 で言葉として説明した認可が、これで動く形になりました。

サーバーのターミナルにも、両方が残っています。

```text
2026-09-05 07:13:53,266 INFO app.main: DELETE /tasks/1 -> 403 (0.0046 秒)
2026-09-05 07:13:53,278 INFO app.routers.tasks: タスクを削除しました id=4
2026-09-05 07:13:53,278 INFO app.main: DELETE /tasks/4 -> 204 (0.0057 秒)
```

> **補足：なぜ `owner_name` で本人か判定しているのか**
> 本来は、`tasks` テーブルに「どのユーザーの行か」を指す列（**外部キー**）を持たせ、
> `users` テーブルと結びつけます。
> このテキストでは**テーブル同士の関連を扱わない**方針のため（6.3.3 の補足）、
> 名前が一致するかで判定しています。
> **名前を変更できるようにすると破綻する作り**です。
> 正しいやり方は、5冊目の mysql-text で扱います。

> **よくある間違い**
> **`get_my_task` を作らず、窓口の中で `if` を書く**間違いです。
>
> ```python
> def delete_task(task: Task = Depends(get_task_or_404),
>                 current_user: User = Depends(get_current_user), ...):
>     if task.owner_name != current_user.name:
>         raise HTTPException(status_code=403, detail="...")
> ```
>
> 動きますが、**窓口が増えるたびに同じ4行をコピーする**ことになります。
> そして、**1か所だけ書き忘れたときに気づけません。**
> 認可の判定は、依存にまとめて1か所に置いてください。

---

## 7.6 やってはいけないこと

ここまでで動くようになりました。**しかし、このままでは本番に出せません。**
この節では「やってはいけないこと」と「まだ足りないもの」を並べます。

### 7.6.1 秘密鍵をコードに書く

**最もよくある事故**が、これです。

```python
SECRET_KEY = "my-secret-key"          # ❌ コードに書いてはいけない
```

このコードを GitHub に上げた瞬間、**世界中の誰でもトークンを作れるようになります。**
自分の名前を `sub` に入れた正しい署名のトークンを作れるので、
**誰にでもなりすませます。**

| やってはいけないこと | 正しいやり方 | 参照 |
|-------------------|------------|------|
| コードに直接書く | `.env` に置き、`settings` 経由で読む | 4.6.2 |
| `.env` を共有・アップロードする | `.gitignore` に `.env` を書く | 4.6.3 |
| `.env.example` に本物の値を書く | 例や作り方だけを書く | 4.6.3 |
| 「とりあえず `secret`」のような値にする | `secrets.token_hex(32)` で作る | 7.4.2 |
| ログやエラー画面に出す | `settings` をそのまま出力しない | 5.5.3 |

`.gitignore` は、少なくとも次の3行になっているはずです。

`.gitignore`（`fastapi-lesson` の直下）

```text
.env
app.db
.venv/
```

**「もう GitHub に上げてしまった」場合、鍵の変更が必要です。**
消しても履歴に残るためです。
`.env` の `SECRET_KEY` を作り直してください。
すでに配ったトークンはすべて無効になりますが（7.4.4）、それが正しい動きです。

> **注意：本番では `.env` ファイルも使わないことが多い**
> 本番のサーバーでは、`.env` を置く代わりに**環境変数を直接渡します**（4.6.3）。
> `pydantic-settings` は、**環境変数を `.env` より優先して読む**ので、
> コードを変えずにそのまま動きます。

### 7.6.2 トークンをどこに保存するか

第9章で React と繋ぐとき、**受け取ったトークンをブラウザのどこに置くか**を決めることになります。
先に、選択肢と危険を知っておいてください。

| 置き場所 | 利点 | 弱点 |
|---------|------|------|
| **`localStorage`**（ブラウザの保存領域） | 実装が簡単。JavaScript から自由に読める | **XSS に弱い。** JavaScript が読めるということは、攻撃者のスクリプトも読める |
| **`HttpOnly` のクッキー** | JavaScript から読めないので XSS に強い | **CSRF の対策が別途要る。** 設定も増える |
| メモリの中（変数） | 最も安全に近い | **再読み込みで消える。** 毎回ログインし直しになる |

用語を2つ、ここで説明します。

- **XSS**（クロスサイトスクリプティング。攻撃者が用意した JavaScript を、
  その Web ページの中で実行させてしまう攻撃）
- **CSRF**（クロスサイトリクエストフォージェリ。利用者がログイン中であることを利用して、
  別のサイトから意図しないリクエストを送らせる攻撃）

**このテキストでは、第9章で `localStorage` を使います。**
理由は、学習の焦点を「React と API の接続」に置きたいからです。
**そのぶん、次のことが言えます。**

- このアプリに XSS の穴があれば、トークンは盗まれる
- だから、**有効期限を短くしておくこと**（7.4.3）に意味がある
- 実際のサービスを作るときは、`HttpOnly` のクッキーを第一候補として検討する

> **注意**
> **トークンを URL に入れないでください。**
>
> ```text
> http://example.com/tasks?token=eyJhbGci...     ❌
> ```
>
> URL はブラウザの履歴・サーバーのアクセスログ・
> リンク元の情報（`Referer`）に残ります。
> トークンは**必ず `Authorization` ヘッダー**で送ってください（7.5.2）。

### 7.6.3 自作の暗号化

**自分で暗号やハッシュの仕組みを作らないでください。**
これは「難しいから」ではなく、**正しく作れたかどうかを自分では判定できないから**です。

動いているように見えるコードと、安全なコードは違います。
公開されているライブラリは、**破ろうとした人たちの攻撃に長年さらされて残ったもの**です。

ありがちな自作を3つ挙げます。

| 自作の例 | なぜだめか |
|---------|----------|
| 文字をずらす（`a` → `b`）などの独自変換 | 元に戻せる。速度も速いので総当たりに耐えられない |
| `SHA-256` や `MD5` を1回かけただけ | **速すぎる。** 1秒に何億回も試せるので、総当たりで破れる。ソルトも無い |
| パスワードを「暗号化」して保存する | **戻せる形で持つこと自体が危険。** 鍵が漏れれば全員分が平文に戻る |

2番目が特に紛らわしいところです。**ハッシュ化なら何でもよい、ではありません。**

| | `SHA-256` | **bcrypt** |
|---|---|---|
| 用途 | ファイルの同一性の確認など | **パスワード専用** |
| 速さ | **非常に速い**（それが利点の用途） | **わざと遅い**（7.2.2） |
| ソルト | 自分で用意する必要がある | 自動で付く |

**パスワードには、パスワード用に作られたものを使ってください。**
このテキストでは bcrypt を使いました。

最後に、**この章で作った認証に足りていないもの**を挙げておきます。
「学習用」と最初に書いたのは、この一覧のことです。

| 足りないもの | 何が起きるか |
|------------|------------|
| **HTTPS** | 通信が暗号化されず、トークンもパスワードも途中で読める |
| ログアウト（トークンの失効） | 「ログアウトした」あとも、期限までトークンは有効なまま |
| リフレッシュトークン | 30 分ごとにログインし直すことになる |
| ログイン試行回数の制限 | パスワードを機械で総当たりされても止められない |
| パスワードの再設定 | 忘れたら二度とログインできない |
| メールアドレスの確認 | 他人のメールアドレスで登録できる |
| 権限の役割分け | 全員が同じことをできる（入口は演習 7.4） |
| 2要素認証 | パスワードが漏れたら終わり |

**これらが要るかどうかは、作るものによります。**
社内の練習用ツールと、お金を扱うサービスでは、必要なものが違います。
**「何が足りないかを言える」状態が、この章のゴール**です。

> **つまずいたら**
> 認証の設計で迷ったら、AI にこう聞いてください。
>
> ```text
> fastapi-text の第7章まで（bcrypt + JWT + Depends での認可）を実装しました。
> ○○というサービスを作る場合、7.6 の一覧のうちどれを先に足すべきですか。
> 優先順位と理由を教えてください。実装コードはまだ要りません。
> ```

---

## まとめ

- **認証は「誰か」、認可は「何をしてよいか」。** 順番は認証 → 認可（7.1.1）
- **`401` は誰か分からない、`403` は誰かは分かったが許可がない**（7.1.1）
- パスワードは**平文で保存しない。** 漏れると他のサービスまで巻き込む（7.2.1）
- **ハッシュ化は元に戻せない。** bcrypt はソルトを自動で付け、
  同じパスワードでも違う値になる（7.2.2）
- 照合は **`bcrypt.checkpw`**。ハッシュ同士を `==` で比べても一致しない（7.2.3）
- bcrypt は **72 バイトまで**しか見ない。受け取る側で弾く（7.2.3・7.3.1）
- ユーザーを返す窓口には **`response_model=UserRead`**。
  `hashed_password` を外に出さない（7.3.1）
- 重複は**事前の `select` と `IntegrityError` の両方**で受ける。
  メッセージは**どちらが原因か分かる形にしない**（7.3.2）
- **JWT の中身は誰でも読める。** 秘密の情報を入れない（7.4.1）
- 署名があるので、**中身を書き換えると検証に失敗する**（7.4.4）
- **秘密鍵は `secrets.token_hex(32)` で作り、`.env` に置く。**
  デフォルト値を書かないことで、書き忘れに気づける（7.4.2・7.6.1）
- トークンは**あとから取り消せない**ので、有効期限を短くする（7.4.3）
- ログインは **`OAuth2PasswordRequestForm`**（フォーム形式）。
  `/docs` の「Authorize」から試せる（7.5.1・7.5.2）
- **`Depends(get_current_user)` と書くだけで、窓口が守られる**（7.5.2）
- 認可の判定も**依存にまとめる。** 窓口ごとに `if` を書かない（7.5.3）
- **暗号・ハッシュを自作しない。** `SHA-256` 1回はパスワードには使えない（7.6.3）
- この章の認証は学習用。**足りないものを言えることが大事**（7.6.3）

---

## 理解度チェック

**問 7.1**（穴埋め）

「あなたは誰か」を確かめることを（　①　）、
「その人が何をしてよいか」を決めることを（　②　）という。
ログインしていない人には（　③　）を、
ログインはしているが他人のデータを消そうとした人には（　④　）を返す。

**問 7.2**（選択）

`bcrypt.hashpw("password123".encode("utf-8"), bcrypt.gensalt())` を2回実行すると、
違う文字列が返ります。理由として正しいものを1つ選んでください。

1. bcrypt が壊れている
2. 実行するたびに違うソルトが混ぜられるから
3. 時刻がハッシュ値に含まれるから
4. 1回目の結果がキャッシュされていないから

**問 7.3**（選択）

JWT のペイロードに入れてよくないものを1つ選んでください。

1. ユーザーの名前
2. トークンの発行時刻
3. **パスワード**
4. トークンの有効期限

**問 7.4**（記述）

`app/schemas.py` の `UserRead` に `hashed_password` を書いてはいけないのはなぜですか。
1〜2行で書いてください。

**問 7.5**（記述）

ログインに失敗したとき、「そのユーザーは存在しません」ではなく
「ユーザー名またはパスワードが違います」と返すのはなぜですか。1〜2行で書いてください。

**問 7.6**（記述）

`SECRET_KEY` を変更すると、それまでに発行したトークンはすべて使えなくなります。
これが**利点にもなる**のは、どんな場面ですか。1行で書いてください。

**問 7.7**（記述）

`get_my_task` のような依存を作らず、窓口の関数の中で
「自分のタスクか」を `if` で判定すると、どんな問題が起きますか。1〜2行で書いてください。

---

## 演習問題

第5章・第6章の演習で作った**メモの窓口**に、この章の認証を入れます。
メモの演習を解いていない場合は、先に
[解答編 その2](./91-answers-part2.md#第6章) のコードを写してから始めてください。

前提として、いまのメモは次の形になっています。

- `app/models.py` の `Note`（`id` / `text` / `pinned` / `author_name` / `author_email` / `created_at`）
- `app/schemas.py` の `NoteCreate` / `NoteRead` / `NoteUpdate` / `NoteListResponse`
- `app/routers/notes.py` の5つの窓口（一覧・1件・登録・更新・削除）と `GET /notes/info`

---

### 演習 7.1 ★☆☆ メモの登録を認証必須にする

**課題**

`POST /notes` を、ログインした人だけが使えるようにしてください。
そして、**作成者はトークンから決める**ようにします。

- `POST /notes` に `current_user: User = Depends(get_current_user)` を足す
- `app/schemas.py` の `NoteCreate` から `author`（`name` と `email` を持つほう）を消す
- `create_note` の中で、`author_name` と `author_email` を `current_user` から入れる
- `GET /notes`・`GET /notes/{note_id}`・`GET /notes/info` は、**認証なしのまま**にする

**完成条件**

- トークンなしで `POST /notes` を送ると `401` が返り、
  本文が `{"error":{"status":401,"message":"Not authenticated","detail":null}}` になる
- `/docs` の「Authorize」でログインしてから `POST /notes` を実行すると `201` が返る
- そのとき送るボディに `author` が**含まれていない**（`{"text": "..."}` だけ）
- 返ってきた JSON の `author` の `name` が、**ログインした人の名前**になっている
- トークンなしでも `GET /notes` が今までどおり `200` を返す
- `app/routers/notes.py` に、トークンやハッシュを扱うコードが**1行も無い**

**ヒント**

`create_task`（7.5.3）と同じ形です。
`NoteCreate` から項目を消すと、`/docs` の `Request body` の例も変わります。そこで確認できます。

---

### 演習 7.2 ★★☆ 自分のメモだけ編集・削除できるようにする

**課題**

`PATCH /notes/{note_id}` と `DELETE /notes/{note_id}` を、**作成者本人だけ**が使えるようにしてください。

- `app/dependencies.py` に `get_my_note` を作る
  - `get_note_or_404` と `get_current_user` の2つを使う
  - 作成者でなければ `403` を返す。`detail` は `"このメモを操作する権限がありません"`
- `PATCH` と `DELETE` の `Depends` を差し替える
- **`GET /notes/{note_id}` は変えない**（誰でも読める）

**完成条件**

- 山田さんでログインし、鈴木さんが作ったメモに `PATCH` すると、
  `{"error":{"status":403,"message":"このメモを操作する権限がありません","detail":null}}` が返る
- 同じ山田さんが、自分のメモに `PATCH` すると `200` が返って内容が変わる
- ログインした状態で、**存在しない `id`**（`PATCH /notes/999`）を呼ぶと、
  `403` ではなく `404` が返る
- トークンなしで、**存在するメモ**に `DELETE /notes/1` を送ると `401` が返る（`403` ではない）
- `app/routers/notes.py` に、`403` という数字が**1つも出てこない**

**ヒント**

`get_my_task`（7.5.3）がそのまま対応します。
`401` / `404` / `403` のどれが返るかは、**依存が呼ばれる順番**で決まります（7.5.3 の図）。
2人分のユーザーとメモが要るので、先に `POST /users` でもう1人作っておいてください。

---

### 演習 7.3 ★★☆ パスワードを変更する窓口を作る

**課題**

ログイン中の人が、自分のパスワードを変更できるようにしてください。

- `app/schemas.py` に `PasswordUpdate` を作る
  - `current_password`：文字列。必須
  - `new_password`：文字列。**8文字以上**、UTF-8 で 72 バイト以内
- `PATCH /users/me/password` を作る
  - ログイン必須
  - `current_password` が合っていなければ **`401`**、
    `detail` は `"現在のパスワードが違います"`
  - 合っていれば、新しいパスワードをハッシュ化して保存する
  - 成功したら **`204`** を返す（本文なし）

**完成条件**

- トークンなしで呼ぶと `401` が返る
- `current_password` を間違えると `401` が返り、
  メッセージが `"現在のパスワードが違います"` になる
- 正しく変更すると `204` が返る
- **変更後、古いパスワードでは `POST /auth/token` が `401` になる**
- 新しいパスワードでログインでき、`GET /users/me` が自分の情報を返す
- `new_password` に `"abc"` を送ると `422` が返り、`type` が `string_too_short` になる
- データベースの `hashed_password` が、変更前と**違う値**になっている

**ヒント**

照合は `verify_password`、保存は `hash_password` です（7.2.3）。
`204` の返し方は 4.4.3 で扱いました。
属性を書き換えて `commit` する形は 6.4.3 と同じです。

---

### 演習 7.4 ★★☆ 管理者だけが使える窓口を作る

**課題**

「管理者」という区別を足し、**管理者だけがどのタスクでも削除できる**ようにしてください。

- `app/models.py` の `User` に `is_admin` を足す（真偽値。**既定は `False`**）
- Alembic でマイグレーションを1本作り、適用する
  - メッセージは `-m "add is_admin to users"` とする
- `app/dependencies.py` に `get_current_admin` を作る
  - `get_current_user` を使い、管理者でなければ `403`。
    `detail` は `"管理者だけが実行できます"`
- `DELETE /admin/tasks/{task_id}` を作る（`app/routers/admin.py` を新しく作る）
  - 管理者なら、**他人のタスクでも削除できる**
  - 見つからなければ `404`
  - 成功したら `204`
- `UserRead` に `is_admin` を足して、`GET /users/me` で自分が管理者かどうか分かるようにする

**完成条件**

- 生成されたマイグレーションの `upgrade()` が、`users` に列を1つ足すだけになっている
  （`tasks` に触る行が無い）
- `alembic upgrade head` が **`Cannot add a NOT NULL column ...` で失敗しない**
  （失敗した場合は、7.3.1 の注意のとおり `server_default` を足す）
- `alembic upgrade head` のあと、**登録済みのユーザーとタスクが消えていない**
- 既存のユーザーは、`GET /users/me` で `"is_admin": false` になる
- 管理者ではない人が `DELETE /admin/tasks/1` を呼ぶと `403` が返り、
  メッセージが `"管理者だけが実行できます"` になる
- 誰か1人を管理者にすると（下のヒント参照）、その人は他人のタスクを `204` で削除できる
- トークンなしで呼ぶと `401` が返る
- `DELETE /tasks/{task_id}`（管理者用ではないほう）の動きは、7.5.3 のまま変わっていない

**ヒント**

`is_admin` の足し方は、`created_at` を足したとき（6.6.3）と同じです。
**あとから足す列は、既存の行に入る値を考えてください**（真偽値なので `| None` は不要です）。

管理者にする方法は、テキストでは作っていません。
`python -c` から直接データベースを書き換えてください
（`SessionLocal` でセッションを作り、`db.get(User, 1)` の属性を書き換えて `commit` します。6.4.3）。

---

解答は [解答編 その2](./91-answers-part2.md#第7章) にあります。
**必ず自分で手を動かしてから**見てください。

---

## 次の章へ

「誰でも消せる API」ではなくなりました。

パスワードは元に戻せない形で保存され、
ログインした人にはトークンが渡り、
**そのトークンが本人であることの証明**になっています。
そして、他人のタスクには手を出せません。

ただし、ここまでの確認は**すべて手作業**でした。
`/docs` を開き、Authorize を押し、`POST` して、`403` が返ることを目で見る——
この確認を、**窓口を1つ直すたびに全部やり直していますか。**

やっていないはずです。そして、やらなかった場所が壊れます。

たとえば `get_my_task` の `!=` を `==` に書き間違えたら、
**他人のタスクだけが消せる API**になります。
一覧も1件取得も動くので、**気づくのは誰かのデータが消えたあと**です。

次の章では、この確認を**プログラムに任せます。**

→ [第8章 テスト](./08-testing.md)
