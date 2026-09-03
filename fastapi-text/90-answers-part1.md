---
title: "解答編 その1（第1章〜第5章）"
---

# 解答編 その1（第1章〜第5章）

**先に自分で解いてから読んでください。**

解けなかった問題は、この解答を読んだあと、
**何も見ずにもう一度自分で書き直して**ください。それで定着します。

API の演習は、**動かして確かめるまでが1問**です。
解答のコードを読んで納得しても、自分のターミナルで同じ結果が出るところまで進めてください。

第6章以降の解答は、解答編 その2 にあります（執筆後に追加されます）。

> **解答を読んでも納得できないとき**
> AI に次のように聞いてください。
>
> ```text
> fastapi-text の演習 1.3 の解答を読みましたが、
> 「予約する」という操作を POST /reservations と書く理由が納得できません。
> ```

---

## 第1章

### 理解度チェック

**問 1.1 の解答**

- ① **リクエスト**
- ② **レスポンス**
- ③ **ステータスコード**

**解説**

HTTP は「リクエスト1つ → レスポンス1つ」で完結する、とても単純な決まりです（1.2.1）。
サーバーは、そのやり取りが終わると相手のことを覚えていません。

ステータスコードは、レスポンスの先頭に付く3桁の数字です（1.2.3）。
ボディを1文字も読まなくても、**この数字を見るだけで結果の大枠が分かる**ようになっています。
だからこそ、1.5.2 の開発者ツールや `curl -i` で「まず数字を見る」習慣が効いてきます。

---

**問 1.2 の解答**

**3** の `{"name": "田中", "active": true, "memo": null}`

**解説**

ほかの3つが不正な理由は、次のとおりです（1.3.2）。

| 選択肢 | 何が違うか |
|-------|-----------|
| 1 | **シングルクォート**を使っている。JSON の文字列はダブルクォートのみ |
| 2 | `30,` と**末尾にカンマ**が残っている |
| 4 | **キーが裸**（`name` が引用符で囲まれていない）。さらに `True` が大文字始まり |

選択肢 1 と 4 は、どちらも**Python の辞書としては正しい**書き方です。
`{'name': '田中'}` も `{"active": True}` も、Python では問題なく動きます。
**Python の感覚のまま JSON を手で書くと、この2つで必ず引っかかります。**

`null` は、Python の `None` にあたる値です（1.3.3）。
JSON 側は `null`、Python 側は `None`。この変換は `json.dumps` / `json.loads` が
自動でやってくれます。

---

**問 1.3 の解答**

**2** の「自分が送ったデータの中身」

**解説**

`422` は `4xx`、つまり**クライアント側のエラー**です（1.2.3）。
「依頼した側の書き方がおかしい」とサーバーが言っている状態なので、
まず疑うべきは自分が送ったものです。

`404` との違いも押さえてください。

- `404`：**URL が違う**（そんなものは無い）
- `422`：**URL は合っているが、送ったデータの中身が条件を満たしていない**

選択肢 1 は `500`（サーバー側のエラー）を疑うべき場面です。
選択肢 3 のように接続そのものが切れている場合は、
そもそもステータスコードが返ってきません（`curl` が接続エラーを出します）。

> **よくある間違い**
> `422` を見て、サーバー側のコードを読み始めてしまうことがあります。
> `4xx` が返ってきている時点で、**サーバーのプログラムは正常に動いています。**
> 送った URL・メソッド・ボディの3つを、順に見直してください。

---

**問 1.4 の解答**

- メソッドと URL：**`DELETE /products/7`**
- `GET /deleteProduct/7` が避けられる理由：
  **動詞はメソッドが担当するので、URL には名詞（対象）だけを書くから。**
  さらに `GET` は「何度呼んでも結果が変わらない」約束なので、
  データを消す操作に使ってはいけない。

**解説**

REST の原則は、突き詰めると次の1行です（1.4.2）。

> **URL は「何に対して」、メソッドは「何をするか」。**

`/deleteProduct/7` は、URL の中に `delete` という動詞が入っています。
この書き方をすると、同じ対象に対して
`/getProduct/7` `/updateProduct/7` `/deleteProduct/7` と URL が増え続け、
呼ぶ側は**その API のためだけの URL 一覧**を覚えることになります。

`DELETE /products/7` なら、`/products/7` という1つの住所を覚えるだけで、
`GET` / `PUT` / `PATCH` / `DELETE` の4つの操作が言い分けられます。

`GET` を使ってはいけない理由も重要です（1.4.4）。
`GET` は「読むだけ」の約束なので、ブラウザや通信の途中にある仕組みが、
**勝手にもう一度リクエストを送ったり、結果を保存したりすることがあります。**
削除が勝手に実行されると、取り返しがつきません。

---

**問 1.5 の解答**

**`PUT` は送らなかった項目が消えるが、`PATCH` は送った項目だけが変わり、
送らなかった項目はそのまま残る。**

**解説**

1.2.2 の言い方をもう一度確認します。

- `PUT`：**丸ごと置き換える**（全部送るのが前提）
- `PATCH`：**一部だけ変える**

たとえば、`{"title": "買い物", "done": false, "memo": "牛乳"}` というタスクに対して
`{"done": true}` だけを送った場合、こうなります。

| メソッド | 結果 |
|---------|------|
| `PUT` | `title` と `memo` が**消える**（送られなかったので） |
| `PATCH` | `done` だけが `true` になり、`title` と `memo` は**残る** |

「完了ボタンを押したらタイトルが消えた」という事故は、
`PATCH` のつもりで `PUT` を使ったときに起きます。

---

**問 1.6 の解答**

**`GET` 以外の操作（`POST` / `PUT` / `PATCH` / `DELETE`）は試せない。
アドレス欄に URL を入力したときにブラウザが送るのは、`GET` だけだから。**

**解説**

1.5.1 のとおりです。
アドレス欄は「そのページを見せてください」という道具なので、
送られるメソッドは `GET` に固定されています。

このことは、デバッグのときに効いてきます。
**「ブラウザで開いたら動くのに、アプリからだと動かない」**というとき、
両者は違うメソッドを送っている可能性があります。
1.5.2 の Network タブで `Request Method` を確認すれば、すぐに分かります。

`GET` 以外を試す方法は、この章で扱った `curl`（1.5.3）と、
第2章で扱う `/docs` の2つです。

---

### 演習問題

### 演習 1.1 の解答

**1. ユーザー全体の一覧**

**Windows（PowerShell）**

```powershell
curl.exe -i https://jsonplaceholder.typicode.com/users
```

**macOS / Linux**

```bash
curl -i https://jsonplaceholder.typicode.com/users
```

実行結果（一部を省略しています）:

```text
HTTP/2 200
content-type: application/json; charset=utf-8

[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    ...
  },
  ...
]
```

**2. 5 番のユーザー1件**

**Windows（PowerShell）**

```powershell
curl.exe -i https://jsonplaceholder.typicode.com/users/5
```

**macOS / Linux**

```bash
curl -i https://jsonplaceholder.typicode.com/users/5
```

実行結果（一部を省略しています）:

```text
HTTP/2 200
content-type: application/json; charset=utf-8

{
  "id": 5,
  "name": "Chelsey Dietrich",
  "username": "Kamren",
  ...
}
```

**メモに書き出す内容**

| | 送ったもの | ステータスコード | ボディの形 |
|---|-----------|--------------|-----------|
| 1 | `GET https://jsonplaceholder.typicode.com/users` | `200` | **配列**（`[` で始まる） |
| 2 | `GET https://jsonplaceholder.typicode.com/users/5` | `200` | **オブジェクト**（`{` で始まる） |

**解説**

この演習で確かめてほしかったのは、**1.4.1 の「集まり」と「1件」の違いが、
返ってくるデータの形にそのまま現れる**ということです。

- `/users` は**集まり**を指すので、返ってくるのは**配列**
- `/users/5` は**その中の1件**を指すので、返ってくるのは**オブジェクト**

この対応は、この本で作る API でもまったく同じです。
第3章以降、`/tasks` は配列を、`/tasks/3` はオブジェクトを返すように作ります。

`-i` を付けなければステータスコードは表示されません（1.5.3）。
`curl` を打っただけで `200` かどうかは分からない、という点に注意してください。

> **別解**
> ブラウザで同じ URL を開き、開発者ツールの Network タブ（1.5.2）で
> `Status Code` を確認する方法でも構いません。
> ただし、ブラウザからは `GET` しか送れない（1.5.1）ので、
> `curl` に慣れておくほうが、このあとの章で楽になります。

> **よくある間違い**
> `https://jsonplaceholder.typicode.com/user/5` のように、
> **リソース名を単数形**にしてしまう間違いが起きやすい場所です。
> この場合は `404` が返ってきます。
> `404` を見たら「URL が違う」と読み替えてください（1.2.3）。

---

### 演習 1.2 の解答

**手順1：投稿一覧をファイルに保存する**

**Windows（PowerShell）**

```powershell
curl.exe -s -o posts.json https://jsonplaceholder.typicode.com/posts
```

**macOS / Linux**

```bash
curl -s -o posts.json https://jsonplaceholder.typicode.com/posts
```

**手順2：Python で読み込んで数える**

`count_posts.py`

```python
import json
from collections import Counter

with open("posts.json", encoding="utf-8") as f:
    posts = json.load(f)

# JSON の配列はリスト、その中身のオブジェクトは辞書として読み込まれる（1.3.3）
user_ids = [post["userId"] for post in posts]
counts = Counter(user_ids)

# sorted(辞書) はキーを小さい順に並べたリストを返す（python-text 4.3）
for user_id in sorted(counts):
    print(f"{user_id}：{counts[user_id]}件")

print("合計:", sum(counts.values()))
```

**Windows（PowerShell）**

```powershell
python count_posts.py
```

**macOS / Linux**

```bash
python3 count_posts.py
```

実行結果:

```text
1：10件
2：10件
3：10件
4：10件
5：10件
6：10件
7：10件
8：10件
9：10件
10：10件
合計: 100
```

**解説**

この演習の狙いは、**API から取ってきたデータが、
python-text で扱ってきたデータとまったく同じものだ**と体感することです。

`json.load` を通した時点で、`posts` はただの**リスト**です。
その中身は、ただの**辞書**です（1.3.3）。
そこから先は、python-text 第4章と第6章の道具がそのまま使えます。
**「API のデータだから特別な扱いが必要」ということは、一切ありません。**

`Counter` は python-text 6.3 で扱ったものです。
リストを渡すと、値ごとの出現回数を数えた辞書のようなものを返します。

合計が 100 になることを確認する条件を入れたのは、
**数え漏らしに自分で気づけるようにする**ためです。
1.5.3 で `len(posts)` が `100` だったことと突き合わせれば、
「10 人 × 10 件」で辻褄が合っていると分かります。

> **別解：`Counter` を使わない書き方**
>
> ```python
> counts = {}
> for post in posts:
>     user_id = post["userId"]
>     counts[user_id] = counts.get(user_id, 0) + 1
> ```
>
> python-text 4.3.3 の「数える型」です。
> `Counter` を忘れていても、こちらで書けていれば正解です。

> **よくある間違い**
> `encoding="utf-8"` を書き忘れると、Windows で
> `UnicodeDecodeError` になることがあります（python-text 7.1）。
> JSONPlaceholder の投稿は英語なので今回は通ってしまいますが、
> **日本語を含む API を叩いた瞬間に壊れます。**
> ファイルを開くときは必ず付ける、と決めてしまってください。
>
> もう1つ。ブラウザの画面から本文をコピー&ペーストして `posts.json` を作ると、
> 表示用に整形された文字が混ざって `json.JSONDecodeError` になることがあります。
> **`-o` でファイルに保存する**（1.5.3）のは、これを避けるためでもあります。

---

### 演習 1.3 の解答

**手順1：リソースを挙げる**

「数えられるものは何か」と考えます（1.4.1）。

- 会議室が何室かある → **会議室**（`rooms`）
- 予約が何件かある → **予約**（`reservations`）

（利用者を管理するなら `users` も挙がりますが、
今回のやりたいこと6つには出てこないので、2つで十分です。）

**手順2：表にまとめる**

| # | やりたいこと | メソッド | URL | 成功時のコード |
|---|------------|---------|-----|--------------|
| 1 | 会議室の一覧を見る | `GET` | `/rooms` | `200` |
| 2 | 会議室を1つ登録する | `POST` | `/rooms` | `201` |
| 3 | 予約を1件入れる | `POST` | `/reservations` | `201` |
| 4 | 予約を1件キャンセルする | `DELETE` | `/reservations/301` | `204` |
| 5 | 3 番の会議室の情報を書き換える | `PUT` | `/rooms/3` | `200` |
| 6 | 3 番の会議室の予約一覧を見る | `GET` | `/rooms/3/reservations` | `200` |

**「予約を1件キャンセルする」で `204` を選ぶ理由**

**削除が成功したときに、返すべきデータが何も無いから**です（1.2.3）。
`204 No Content` は「成功したが、ボディは空」という意味の番号です。

**解説**

この演習で確かめてほしかったのは、次の3点です。

**1つ目：動詞を名詞に翻訳できたか**

「予約する」という動詞を、そのまま `/reserve` や `/createReservation` にすると、
1.4.2 の表の ❌ の側に落ちます。
**「予約というリソースを1件新しく作る」**と読み替えて `POST /reservations` になれば正解です。
1.4.2 の図書館の例で「本を借りる → `POST /loans`」としたのと、まったく同じ翻訳です。

**2つ目：`POST` の相手は「集まり」だと分かっているか**

3 番を `POST /reservations/301` と書いてしまうと誤りです。
**まだ番号が決まっていないものを、番号で指すことはできません**（1.4.3）。
番号はサーバーが決めて、レスポンスで返してくれます（1.5.3 の `"id": 101`）。

**3つ目：入れ子の向きが正しいか**

6 番を `/reservations/rooms/3` と書くと、
「予約の中の、会議室の3番」と読めてしまいます。
**左から右へ「3 番の会議室の、予約」と読める順**にします（1.4.2 の手順3）。

```text
/rooms/3/reservations
 └ 親    └ 番号 └ その下にあるもの
```

> **別解**
> 4 番のキャンセルを `PATCH /reservations/301`（状態を「キャンセル済み」に変える）
> としても正解です。この場合、成功時のコードは `200` になります
> （変更後のデータを返すため）。
>
> 「本当に消す」のか「キャンセル済みという印を付けて残す」のかは、
> **業務の要件で決まる**もので、REST が決めることではありません。
> 実際の予約システムでは、あとから履歴を確認できるように
> 後者（`PATCH`）を選ぶことが多くあります。
> **どちらを選んだかを説明できれば、それで十分です**（1.4.4）。

> **よくある間違い**
> 5 番を `PATCH` と書いた場合について。
> 課題文が「情報を**書き換える**」としか言っていないので、
> `PUT` と `PATCH` のどちらもあり得ます。
> ただし、**選んだ理由を言えるかどうか**が分かれ目です（問 1.5）。
>
> - 会議室名・定員・場所を**すべて送り直す**なら `PUT`
> - 定員**だけ**を直したいなら `PATCH`
>
> 「なんとなく `PUT`」で選んでいた場合は、1.2.2 に戻ってください。

---

## 第2章

### 理解度チェック

**問 2.1 の解答**

- ① **デコレータ**
- ② **メソッド**（HTTP メソッド）
- ③ **パス**（URL のドメインより後ろの部分）

**解説**

`@app.post("/tasks")` は、3つの部品に分解できます（2.3.2）。

| 部品 | 意味 |
|------|------|
| `app` | 登録先のアプリ本体（`app = FastAPI()` で作ったもの） |
| `post` | HTTP メソッド。第1章 1.2.2 の「新しく作る」 |
| `"/tasks"` | パス。第1章 1.4.2 で「複数形の名詞」と決めたもの |

この1行が「`POST /tasks` が来たら、すぐ下の関数を呼んでください」という登録になります。

② を「動詞」と書いた場合も、意味としては合っています。
ただし第1章から一貫して使っている呼び名は **HTTP メソッド**なので、こちらで覚えてください。

---

**問 2.2 の解答**

**2. `fastapi` コマンドが入らず、`fastapi dev` で起動できない**

**解説**

`[standard]` は、**FastAPI 本体と一緒に入れる追加部品の指定**です（2.2.2）。
本体だけなら `pip install fastapi` でも入りますが、
その場合 `fastapi-cli`（`fastapi` コマンドの正体）や
`uvicorn`（実際に HTTP を待ち受けるサーバー本体）が入りません。

1 が誤りなのは、`fastapi` 自体は入るからです。
3 と 4 が誤りなのは、`/docs` の生成も JSON への変換も **FastAPI 本体の機能**で、
追加部品とは関係がないからです。

> **よくある間違い**
> `pip list` に `fastapi` があるのに `fastapi --version` が動かない、
> という状況がまさにこれです。
> **`pip list` に `fastapi-cli` があるか**を見れば、すぐ切り分けられます（2.6.1 の表の3番）。

---

**問 2.3 の解答**

**3. 別のターミナルで、同じサーバーがまだ動いている**

**解説**

`Address already in use` は、**そのポートを別のプログラムが先に使っている**という意味です（2.4.4）。
ポートは「1台のコンピュータの中で、どのプログラムに繋ぐかを表す番号」なので、
1つの番号を2つのプログラムで共有することはできません。

いちばん多い原因は、**前に起動したサーバーの止め忘れ**です。
VS Code のターミナルはタブで増やせるため、
裏のタブでサーバーが動いたままになっていることがよくあります。

1・2・4 は、いずれも別のエラーメッセージになります。

- 文法が間違っている → トレースバックが表示される（2.6.3）
- 有効化していない → `command not found` / `ModuleNotFoundError`（2.6.1）
- インターネット → **自分のパソコンの中の通信なので、そもそも関係ありません**（2.4.1）

---

**問 2.4 の解答**

```json
{"ready": true, "error": null, "count": 0}
```

**解説**

Python の値と JSON の値の対応は、第1章 1.3.3 の表のとおりです（2.3.3）。

| Python | JSON |
|--------|------|
| `True` | `true` |
| `None` | `null` |
| `0` | `0` |

**`True` → `true` の書き換えが起きる**ことがこの問題の要点です。
`0` はそのまま `0` です。数値は書き換えられません。

> **よくある間違い**
> `{"ready": True, "error": None, "count": 0}` と、Python の書き方のまま答えた場合。
> これは JSON としては不正です（第1章 1.3.2 の「真偽値はすべて小文字」）。
> **書き換えるのは FastAPI の仕事**ですが、**何に書き換わるかは知っている必要があります。**

---

**問 2.5 の解答**

**サーバーは、リクエストが来るのを待ち続けるプログラムだから**です。

**解説**

python-text で書いたスクリプトは、最後の行まで実行したら終了しました。
サーバーは違います。第1章 1.1.2 で説明したとおり、
**止めるまでずっと動き続け、依頼が来るたびに応答します**（2.4.1）。

プロンプトが返ってこないのは、**プログラムがまだ終わっていない**からです。
故障でも固まっているのでもありません。

止めるときは、そのターミナルで `Ctrl` + `C` を押します。
確認作業は、**ブラウザか、もう1つ開いたターミナル**で行います（2.4.2）。

---

**問 2.6 の解答**

**`/docs` は `/openapi.json` から作られ、その `/openapi.json` はコードから自動で作られるから**です。

**解説**

流れは一方通行です（2.5.4）。

```text
main.py  →  /openapi.json  →  /docs ・ /redoc
```

人間が仕様書を手で書き写す工程が、どこにもありません。
だから「コードを直したのに仕様書が古いまま」という状態が、原理的に起こりません。

逆に言えば、**`/docs` の表示がおかしいときは、コードのほうを直します。**
`/docs` を直接編集することはできません。

---

**問 2.7 の解答**

**仮想環境が有効化されているかを確認する。**
確認方法は、ターミナルの行頭に `(.venv)` が付いているかを見る。
より確実に調べるなら、次を実行する。

- Windows（PowerShell）：`Get-Command python | Select-Object Source`
- macOS / Linux：`which python`

表示されたパスに `.venv` が含まれていれば、有効化できています。

**解説**

2.6.1 の表の1番です。**このエラーの原因で、いちばん多いものです。**

ターミナルを開き直したり、VS Code を再起動したりすると有効化は解除されます。
「さっきまで動いていたのに」という場合は、まずここを疑ってください。

`(.venv)` は表示されているのに動かない、という場合は 2.6.2 の方法で
**実際に使われている Python の場所**を確認します。
別のプロジェクトの `.venv` を有効化していた、という取り違えもあります。

---

### 演習問題

### 演習 2.1 の解答

`main.py`（末尾に追記）

```python
@app.get("/about")
def read_about():
    return {
        "title": "FastAPI の練習用プロジェクト",
        "chapter": 2,
        "finished": False,
    }
```

保存すると、サーバー側のターミナルに次が出ます。

```text
WARNING:  WatchFiles detected changes in 'main.py'. Reloading...
INFO:     Application startup complete.
```

ブラウザで `http://127.0.0.1:8000/about` を開いた結果:

```json
{"title":"FastAPI の練習用プロジェクト","chapter":2,"finished":false}
```

**解説**

やっていることは 2.3.2 で `/health` を足したときと同じです。
確認すべき点は3つあります。

**1つ目：`False` が `false` になっている**

Python の `False` を、そのまま `return` しています。
JSON への書き換えは FastAPI が行うので、
**コードのほうを `false` と書く必要はありません**（2.3.3）。
むしろ Python では `false` は未定義の名前なので、`NameError` になります。

**2つ目：サーバーを再起動していない**

`fastapi dev` は保存を検知して読み直します（2.4.3）。
`Ctrl` + `C` で止めて起動し直しても結果は同じですが、
**その必要が無いことを体験するのがこの演習の狙い**です。

**3つ目：`/docs` に自動で行が増える**

`/docs` を再読み込みすると、`GET /about` が並んでいます。
**この行を追加する作業を、あなたは一度もしていません**（2.5.4）。
関数名 `read_about` から `Read About` という見出しも自動で作られています。

開発者ツールの Network タブ（第1章 1.5.2）では、次が確認できます。

```text
Request Method:  GET
Status Code:     200 OK
content-type:    application/json
```

`content-type` を自分で指定していないことにも注目してください（2.3.3）。

> **よくある間違い**
> ブラウザで `/about` を開いて `404` が返る場合、原因はほぼ次の2つです。
>
> 1. `@` を書き忘れている（2.3.2 の「よくある間違い」）
> 2. **ファイルを保存していない**（VS Code のタブに `●` が付いています。2.4.3）
>
> どちらも `Reloading...` が出ていないので、**ターミナルを見れば区別できます。**

---

### 演習 2.2 の解答

**手順（2.4.3 の6ステップのとおり）**

**Windows（PowerShell）**

```powershell
cd $HOME\Desktop
mkdir weather-api
cd weather-api
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install "fastapi[standard]==0.115.6"
```

**macOS / Linux**

```bash
cd ~/Desktop
mkdir weather-api
cd weather-api
python3 -m venv .venv
source .venv/bin/activate
pip install "fastapi[standard]==0.115.6"
```

`weather-api/main.py`

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"service": "weather-api"}


@app.get("/today")
def read_today():
    # 辞書・リスト・真偽値・None を混ぜても、まとめて JSON になる（2.3.3）
    return {
        "city": "東京",
        "temperature": 28,
        "rain": False,
        "warning": None,
        "hours": [9, 12, 15],
    }
```

起動（**8000 番のサーバーは止めずに、新しいターミナルで**）:

**Windows（PowerShell）**

```powershell
fastapi dev main.py --port 8001
```

**macOS / Linux**

```bash
fastapi dev main.py --port 8001
```

確認（さらにもう1つターミナルを開いて実行します）:

**Windows（PowerShell）**

```powershell
curl.exe -i http://127.0.0.1:8001/today
```

**macOS / Linux**

```bash
curl -i http://127.0.0.1:8001/today
```

実行結果:

```text
HTTP/1.1 200 OK
content-type: application/json

{"city":"東京","temperature":28,"rain":false,"warning":null,"hours":[9,12,15]}
```

書き出し（`weather-api` を有効化しているターミナルで実行します）:

**Windows（PowerShell）**

```powershell
pip freeze > requirements.txt
```

**macOS / Linux**

```bash
pip freeze > requirements.txt
```

**解説**

この演習は、**2.2 から 2.4 までを1本につなげられるか**を見るものです。
新しく覚えることは何もありません。詰まりやすいのは次の4点です。

**1つ目：仮想環境はプロジェクトごとに作り直す**

`fastapi-lesson` の `.venv` を使い回すことはしません。
**プロジェクトごとにライブラリを分けるのが `venv` の目的**だからです（python-text 1.5）。
`weather-api` の中でもう一度 `pip install` が必要になります。
「さっき入れたのに」と感じたら、それは正しい感覚です。**それでも入れ直します。**

**2つ目：`--port 8001` を付けたら、URL も 8001 にする**

`http://127.0.0.1:8000/today` を開いてしまい、
「8000 番のほうの API には `/today` が無いので `404`」という結果になりがちです（2.4.4）。
**エラーが `404` なら URL の間違い、`Address already in use` ならポートの重複**です。

**3つ目：ターミナルが3つ必要になる**

| ターミナル | 用途 | 有効化 |
|-----------|------|-------|
| 1 | `fastapi-lesson` のサーバー（8000） | 必要 |
| 2 | `weather-api` のサーバー（8001） | 必要 |
| 3 | `curl` で確認する | 不要 |

サーバーを起動したターミナルは**返ってこない**ので（2.4.1）、
確認には別のターミナルが要ります。

**4つ目：`requirements.txt` は有効化した状態で書き出す**

有効化していないターミナルで `pip freeze` を実行すると、
**パソコン全体に入っているものの一覧**が書き出されてしまいます（2.6.2）。
`fastapi==0.115.6` の行があるかどうかで確かめてください。

> **別解**
> 8000 番のサーバーを `Ctrl` + `C` で止めてから、
> `weather-api` を 8000 番で起動しても、API そのものは動きます。
> ただし、それでは**同時に2つ動かす**という完成条件を満たしていません。
> 第9章では、React と FastAPI を**必ず同時に**起動することになります。
> ここで一度体験しておいてください。

---

### 演習 2.3 の解答

**表示される内容は環境によって少し違います。以下は例です。**

**1. 仮想環境を有効化していない状態で起動した**

エラーメッセージ:

```text
zsh: command not found: fastapi
```

Windows では次のようになります。

```text
fastapi : 用語 'fastapi' は、コマンドレット、関数、スクリプト ファイル、
または操作可能なプログラムの名前として認識されません。
```

- **原因**：`fastapi` コマンドは `.venv` の中に入っているため、
  有効化していないターミナルからは見えない（2.6.1）
- **直し方**：`.\.venv\Scripts\Activate.ps1`（macOS / Linux は `source .venv/bin/activate`）を実行し、
  行頭に `(.venv)` が付いたことを確認してから、もう一度起動する
- **確認コマンド**：`Get-Command python | Select-Object Source`（macOS / Linux は `which python`）を実行し、
  表示されたパスに `.venv` が含まれているかを見る（2.6.2）

**2. `app` を `application` に書き換えた**

エラーメッセージ:

```text
There is no FastAPI app or you haven't used a supported type.
```

- **原因**：`fastapi dev main.py` は、**`main.py` の中の `app` という名前の変数**を探しに行く。
  名前が違うと見つけられない（2.6.3）
- **直し方**：変数名を `app` に戻す。デコレータも `@app.get(...)` に戻す必要がある

**3. 8000 番で起動したまま、もう一度同じコマンドを実行した**

エラーメッセージ:

```text
ERROR:    [Errno 48] Address already in use
```

- **原因**：ポート 8000 を、1つ目のサーバーがすでに使っている。
  **1つのポートは1つのプログラムしか使えない**（2.4.4）
- **直し方（A）**：`fastapi dev main.py --port 8001` のように、別のポートを指定する。
  確認する URL も `http://127.0.0.1:8001` に変える
- **直し方（B）**：使用中のプログラムを止める
  - Windows：`netstat -ano | Select-String ":8000"` で PID を調べ、`taskkill /PID <番号> /F`
  - macOS / Linux：`lsof -i :8000` で PID を調べ、`kill <番号>`

**解説**

この演習の狙いは、**エラーメッセージと原因を結びつけた記憶を作っておく**ことです。

実際の開発では、この3つが**組み合わさって**現れます。

> ターミナルを開き直した（→ 有効化が外れる）
> → `command not found` が出た
> → 慌てて別のターミナルで起動した
> → 前のサーバーが生きていて `Address already in use` が出た

こうなったとき、**メッセージを見ただけで原因を1つに絞れるか**が分かれ目です。

見分け方をまとめます。

| 出たメッセージ | 疑うところ | 参照 |
|--------------|-----------|------|
| `command not found` / `認識されません` | 有効化・インストール | 2.6.1 |
| `ModuleNotFoundError` | 同上 | 2.6.1 |
| `Path does not exist` | ディレクトリ・ファイル名 | 2.6.3 |
| `There is no FastAPI app` | 変数名（`app`） | 2.6.3 |
| `Address already in use` | ポートの重複 | 2.4.4 |
| トレースバックが出る | `main.py` の中身（文法・インデント） | 2.6.3 |

> **補足：2 番で `@app.get` を直し忘れた場合**
> `application = FastAPI()` に変えたのに `@app.get("/")` を残していると、
> 起動する前に次のエラーになります。
>
> ```text
> NameError: name 'app' is not defined
> ```
>
> python-text 1.4 で学んだトレースバックです。
> **これも正しい観察結果なので、そのまま記録して構いません。**
> `app` という名前が2か所で使われていることに気づけたなら、この演習の目的は達成できています。

> **注意**
> `taskkill` / `kill` を使うときは、**必ず自分で調べた PID を確認してから**打ってください（2.4.4）。
> 練習だからといって、適当な番号を打たないでください。

---

## 第3章

### 理解度チェック

**問 3.1 の解答**

- ① **パス**（パスパラメータ）
- ② **クエリ**（クエリパラメータ）
- ③ **ボディ**（リクエストボディ）

**解説**

3つの使い分けは、次の一文で覚えてください（3.3.1）。

| 乗せ場所 | 役割 | 例 |
|---------|------|---|
| パス | **どれか1つ**を指す | `/tasks/3` |
| クエリ | **どう絞り込むか**を指定する | `?done=true&limit=10` |
| ボディ | **登録・更新する中身そのもの** | `{"title": "牛乳を買う"}` |

迷ったときは、「**この値が変わったら、別のものを指すことになるか**」と考えます。
`/tasks/3` の `3` が変われば別のタスクになるので、パスです。
`?done=true` が変わっても対象は「タスクの一覧」のままなので、クエリです。

---

**問 3.2 の解答**

**2** の `{"item_id":"5"}`

**解説**

型ヒントが書かれていないため、`item_id` は**文字列のまま**関数に渡されます（3.1.1）。
URL はただの文字列なので、これが元々の姿です。

1 になるのは `item_id: int` と書いた場合です（3.1.2）。
3 の `422` は、`int` と書いたうえで `/items/abc` のように変換できない値が来たときです（3.1.3）。

> **よくある間違い**
> この違いは、**画面では気づきにくい**ものです。
> `{"item_id":"5"}` と `{"item_id":5}` は、ダブルクォートの有無しか違いません。
> しかし、`if task["id"] == item_id` のような比較を書くと、
> **文字列と数値は決して一致しない**ので、「なぜか見つからない」という症状になります。
> **JSON を見るときは、ダブルクォートの有無を必ず確認してください。**

---

**問 3.3 の解答**

**3** の `422` が返る

**解説**

FastAPI は、**登録された順に上から照合します**（3.1.4）。
`/users/me` は `/users/{user_id}` の形にも当てはまるため、
**先に書いてある `/users/{user_id}` が捕まえてしまいます。**

そのうえで `"me"` を `int` に変換しようとして失敗し、次が返ります。

```json
{"detail":[{"type":"int_parsing","loc":["path","user_id"],"msg":"Input should be a valid integer, unable to parse string as an integer","input":"me"}]}
```

直し方は、**`/users/me` を `/users/{user_id}` より前に移動する**ことです。

2 になるのは、`user_id` に型ヒントを付けていない場合です。
このときは `{"user_id":"me"}` が `200` で返り、**エラーが出ないぶん気づくのが遅れます。**

4 が誤りなのは、この並びでもサーバーは正常に起動するからです。
**起動時には何も警告されません。** 実行して初めて分かります。

---

**問 3.4 の解答**

`done: bool = False` では、**「指定されなかった」と「`False` を指定した」を区別できない**ためです。
省略時に「絞り込まない（全件返す）」という第3の動作をさせたい場合は、
`bool | None = None` にして `None` を「指定なし」の印として使います。

**解説**

必要な状態が3つあるのに、`bool` は2つの値しか持てない、というのが問題の本質です（3.2.3）。

| URL | `bool = False` の場合 | `bool \| None = None` の場合 |
|-----|---------------------|---------------------------|
| `/tasks` | `False` → **未完了だけ**返る | `None` → **全件**返る |
| `/tasks?done=true` | `True` → 完了だけ | `True` → 完了だけ |
| `/tasks?done=false` | `False` → 未完了だけ | `False` → 未完了だけ |

判定は、必ず `is not None` で行います。

```python
if done is not None:
    result = [task for task in result if task["done"] == done]
```

> **よくある間違い**
> `if done:` と書くと、**`done=false` を指定したときにも絞り込みが行われません。**
> `None` も `False` も、`if` の条件としては同じ「偽」だからです（python-text 3.1.5）。
> 画面上は「`?done=false` を付けても全件返ってくる」という症状になります。

---

**問 3.5 の解答**

**URL の `?` 以降に、`keyword` という名前のクエリパラメータを追加する。**

**解説**

読み方の順序は次のとおりです（3.4.2）。

1. `loc` の1つ目が `query` → **直すのは URL の `?` 以降**
2. `loc` の2つ目が `keyword` → 直す名前は `keyword`
3. `type` が `missing`、`input` が `null` → **そもそも送られていない**

したがって `/tasks/search?keyword=買う` のように付け足せば通ります。

`missing` が出たときに疑うことは、次の2つです。

| 疑うこと | 確認方法 |
|---------|---------|
| そもそも付け忘れている | URL に `?keyword=` があるか |
| **名前の綴りが違う** | `keywords` や `Keyword` になっていないか |

> **よくある間違い**
> `loc` が `["query", ...]` なのに、**パスのほうを直そうとする**間違いがよくあります。
> `loc` の1つ目は「URL のどの部分か」を表しています。
> `path` なら `/` で区切られた部分、`query` なら `?` 以降です（3.4.2 の図）。

---

**問 3.6 の解答**

**最初に見るべき場所：サーバーを起動しているターミナル**（トレースバックが出ています）。

`422` にならなかったのは、ボディを `dict` で受け取っているためです。
`dict` は「JSON のオブジェクトであること」しか検査しないので、
`title` というキーが無くても FastAPI は通してしまい、
関数の中で `new_task["title"]` を実行した時点で `KeyError` になって落ちます。
**落ちたのはサーバー側なので `500` です。**

**解説**

`500` は「サーバー側の作りが悪い」ことを意味します（第1章 1.2.3、3.3.2）。
レスポンスには `Internal Server Error` としか書かれていないので、
**原因はターミナルでしか分かりません。**

```text
INFO:     127.0.0.1:52190 - "POST /tasks HTTP/1.1" 500 Internal Server Error
ERROR:    Exception in ASGI application
Traceback (most recent call last):
  ...
KeyError: 'title'
```

python-text 1.4.4 のとおり、**トレースバックはいちばん下の行が本当の原因**です。

その場しのぎの対処は `.get("title", "（無題）")` を使うことですが、
これは「送られてこなかったこと」を見逃しているだけで、正しい直し方ではありません。
**正しくは、送る側の間違いとして `422` を返すべき**です。
その方法が、第4章の Pydantic です。

---

**問 3.7 の解答**

`X-Token: abc123` のように、**アンダースコアをハイフンに変えた名前**で送ります。

**Windows（PowerShell）**

```powershell
curl.exe -s http://127.0.0.1:8000/whoami -H "X-Token: abc123"
```

**macOS / Linux**

```bash
curl -s http://127.0.0.1:8000/whoami -H "X-Token: abc123"
```

**解説**

Python の変数名にハイフンは使えないため、
FastAPI は**引数名の `_` をヘッダー名の `-` に読み替えます**（3.5.1）。

| 引数名 | ヘッダー名 |
|-------|-----------|
| `x_token` | `X-Token` |
| `user_agent` | `User-Agent` |

大文字・小文字は区別されないので、`-H "x-token: abc123"` でも届きます。
一方、`-H "x_token: abc123"` のように**アンダースコアのまま送ると届きません**（`null` になります）。

---

### 演習問題

### 演習 3.1 の解答

`main.py`（末尾に追記）

```python
@app.get("/users/{user_id}")
def read_user(user_id: int, verbose: bool = False):
    result = {"user_id": user_id, "name": f"ユーザー{user_id}"}
    if verbose:
        # 詳しい情報が要求されたときだけ、キーを1つ足す
        result["detail"] = "詳細情報"
    return result
```

`http://127.0.0.1:8000/users/5`

```json
{"user_id":5,"name":"ユーザー5"}
```

`http://127.0.0.1:8000/users/5?verbose=true`

```json
{"user_id":5,"name":"ユーザー5","detail":"詳細情報"}
```

`http://127.0.0.1:8000/users/abc`

```json
{"detail":[{"type":"int_parsing","loc":["path","user_id"],"msg":"Input should be a valid integer, unable to parse string as an integer","input":"abc"}]}
```

**解説**

新しい要素は1つもありません。組み合わせているのは次の2つです。

| 部分 | どこで学んだか |
|------|--------------|
| `user_id: int`（パスパラメータ＋型） | 3.1.2 |
| `verbose: bool = False`（クエリ＋デフォルト値） | 3.2.1・3.2.2 |

`user_id` は**パスの波括弧に同じ名前がある**のでパスパラメータになり、
`verbose` は**波括弧に無い**のでクエリパラメータになります（3.2.1）。
**引数の書き方はどちらもほとんど同じで、パスに書いたかどうかだけで決まります。**

`f"ユーザー{user_id}"` は f-string（python-text 2.4.4）です。

> **別解**
> `if verbose:` の代わりに、辞書を2通り作って返しても構いません。
>
> ```python
> if verbose:
>     return {"user_id": user_id, "name": f"ユーザー{user_id}", "detail": "詳細情報"}
> return {"user_id": user_id, "name": f"ユーザー{user_id}"}
> ```
>
> 動作は同じです。ただし、共通部分が2か所に書かれているので、
> 項目が増えたときに直し忘れが起きます。**解答の形のほうが安全です。**

> **よくある間違い**
> `/users/5` を開いて `{"user_id":"5"}` と返ってきた場合、**型ヒントの書き忘れ**です（3.1.2）。
> `/users/5?verbose=True` と**大文字で書いても動きます**（3.2.1 の表）。
> 逆に `?verbose=1` でも `True` になります。

---

### 演習 3.2 の解答

`main.py`（**`@app.get("/tasks/{task_id}")` より前**に追記）

```python
@app.get("/tasks/search")
def search_tasks(
    keyword: str = Query(min_length=2, max_length=10),
    done: bool | None = None,
):
    result = tasks
    if done is not None:
        result = [task for task in result if task["done"] == done]
    # タイトルにキーワードを含むものだけ残す
    result = [task for task in result if keyword in task["title"]]
    return {"keyword": keyword, "count": len(result), "tasks": result}
```

`http://127.0.0.1:8000/tasks/search?keyword=買う`

```json
{"keyword":"買う","count":1,"tasks":[{"id":1,"title":"牛乳を買う","done":false}]}
```

`http://127.0.0.1:8000/tasks/search`（`keyword` なし）

```json
{"detail":[{"type":"missing","loc":["query","keyword"],"msg":"Field required","input":null}]}
```

`http://127.0.0.1:8000/tasks/search?keyword=あ`

```json
{"detail":[{"type":"string_too_short","loc":["query","keyword"],"msg":"String should have at least 2 characters","input":"あ","ctx":{"min_length":2}}]}
```

`http://127.0.0.1:8000/tasks/search?keyword=買う&done=true`

```json
{"keyword":"買う","count":0,"tasks":[]}
```

**解説**

この演習の要点は3つです。

**1つ目：書く場所（いちばん大事）**

`/tasks/search` は固定のパスなので、**`/tasks/{task_id}` より前に書きます**（3.1.4）。
後ろに書くと、`"search"` を `int` に変換しようとして次が返ります。

```json
{"detail":[{"type":"int_parsing","loc":["path","task_id"],"msg":"Input should be a valid integer, unable to parse string as an integer","input":"search"}]}
```

**「作ったはずの窓口が動かない」ときは、まず定義の順番を疑ってください。**

**2つ目：必須のまま条件を付ける**

`Query(min_length=2, max_length=10)` には、**`default=` を書いていません。**
`default=` を書かなければ必須のままです（3.4.1）。

```python
keyword: str = Query(min_length=2, max_length=10)          # 必須
keyword: str | None = Query(default=None, min_length=2)    # 省略可能
```

`= Query(...)` という見た目のせいで「デフォルト値がある」ように見えますが、
**`Query(...)` は条件を書くための指定であって、値ではありません。**

**3つ目：絞り込みの順番**

`done` の絞り込みと `keyword` の絞り込みは、**どちらを先にしても結果は同じ**です。
`result` を上書きしながら、条件を1つずつ重ねていく書き方に慣れてください。
第6章でデータベースを使うようになっても、考え方は同じです。

> **よくある間違い**
> `keyword` を `Query` なしで `keyword: str` と書いた場合、
> **必須にはなりますが、文字数の条件が効きません。**
> `?keyword=あ` が `200` で通ってしまいます。
> **条件を付けるには `Query` が必要です**（3.4.1）。

> **別解**
> 最後の完成条件（`?keyword=買う&done=true` が0件）は、
> **「牛乳を買う」が未完了だから**です。
> `?keyword=買う&done=false` にすると1件返ります。
> 0件のときに `404` を返したくなるかもしれませんが、
> **「検索した結果、該当が無かった」は正常な結果なので `200` のまま**にします
> （第1章 1.2.3）。

---

### 演習 3.3 の解答

`main.py`（`tasks` の下に追記）

```python
notes = [
    {"id": 1, "text": "会議は水曜に変更", "pinned": False},
]
```

`main.py`（末尾に追記）

```python
@app.post("/notes")
def create_note(new_note: dict):
    new_id = max([note["id"] for note in notes]) + 1
    created = {
        "id": new_id,
        # 送られてこなかった項目は、決めておいた値で埋める
        "text": new_note.get("text", "（本文なし）"),
        "pinned": new_note.get("pinned", False),
    }
    notes.append(created)
    return created


@app.put("/notes/{note_id}")
def update_note(
    new_note: dict,
    note_id: int = Path(ge=1),
    notify: bool = False,
    x_token: str | None = Header(default=None),
):
    for note in notes:
        if note["id"] == note_id:
            note["text"] = new_note.get("text", note["text"])
            note["pinned"] = new_note.get("pinned", note["pinned"])
            return {"updated": note, "notified": notify, "token": x_token}
    return {"message": f"id {note_id} のメモは見つかりませんでした"}
```

`POST /notes` に `{"text": "牛乳を買い忘れた"}` を送った結果:

```json
{"id":2,"text":"牛乳を買い忘れた","pinned":false}
```

`POST /notes` に `{}` を送った結果:

```json
{"id":3,"text":"（本文なし）","pinned":false}
```

`PUT /notes/1?notify=true`（ボディは `{"pinned": true}`、ヘッダーは `X-Token: abc123`）:

**Windows（PowerShell）**

`update_note.json`

```json
{"pinned": true}
```

```powershell
curl.exe -i -X PUT "http://127.0.0.1:8000/notes/1?notify=true" -H "Content-Type: application/json" -H "X-Token: abc123" -d "@update_note.json"
```

**macOS / Linux**

```bash
curl -i -X PUT "http://127.0.0.1:8000/notes/1?notify=true" -H "Content-Type: application/json" -H "X-Token: abc123" -d '{"pinned": true}'
```

実行結果:

```text
HTTP/1.1 200 OK
content-type: application/json

{"updated":{"id":1,"text":"会議は水曜に変更","pinned":true},"notified":true,"token":"abc123"}
```

`PUT /notes/0` の結果:

```json
{"detail":[{"type":"greater_than_equal","loc":["path","note_id"],"msg":"Input should be greater than or equal to 1","input":"0","ctx":{"ge":1}}]}
```

**解説**

`update_note` の引数は4つありますが、**FastAPI は名前と型だけで振り分けています**（3.3.3）。

| 引数 | 判定 | 理由 |
|------|------|------|
| `new_note` | **ボディ** | 型が `dict` |
| `note_id` | **パス** | パスの波括弧に同じ名前がある |
| `notify` | **クエリ** | 波括弧に無く、型が `bool` |
| `x_token` | **ヘッダー** | `Header(...)` と書いてある |

**引数の順番に注意してください。**
`note_id: int = Path(ge=1)` はデフォルト値を持つ引数になるため、
デフォルト値の無い `new_note: dict` を**前に**書く必要があります（3.4.1 の「よくある間違い」）。
順番を逆にすると、起動する前に次のエラーになります。

```text
SyntaxError: parameter without a default follows parameter with a default
```

**FastAPI は名前と型で振り分けるので、順番を変えても動作は変わりません。**

**2つ目の完成条件（`{}` を送っても `500` にならない）が、この演習の肝です。**

`.get("text", "（本文なし）")` を使っているので、キーが無くても落ちません（3.3.2）。
`new_note["text"]` と書いていた場合は `KeyError` になり、`500` が返ります。

ただし、**これは正しい解決ではありません。**
本文が無いメモを `"（本文なし）"` として登録してしまうのは、
おそらく利用者が期待した動作ではないからです。
**「`text` は必須。無ければ `422`」と宣言できるようにするのが第4章**です。

> **よくある間違い**
> `curl` で `X-Token` を送ったのに `token` が `null` になる場合、原因は次のどちらかです。
>
> 1. 引数名を `x-token` と書いた → **Python の変数名にハイフンは使えません**（3.5.1）
> 2. `-H "x_token: abc123"` と、**アンダースコアのまま送った**
>
> 引数名は `x_token`、送るヘッダー名は `X-Token` です。**読み替えは FastAPI がします。**

> **別解**
> `PUT` ではなく `PATCH` を使う設計も考えられます（第1章 1.2.2）。
> 実際、この解答の `update_note` は「送られてきた項目だけ更新する」動きなので、
> **意味としては `PATCH` に近い**ものです。
>
> `PUT` は本来「丸ごと置き換える」メソッドで、
> 送らなかった項目は消えるのが筋です。
> このテキストでは、第9章まで `PUT` を「更新」として使いますが、
> **どちらの意味で作ったかを説明できることのほうが大切です**（1.4.4）。

---

## 第4章

### 理解度チェック

**問 4.1 の解答**

- ① **`BaseModel`**
- ② **`Field`**
- ③ **`response_model`**

**解説**

3つの役割を整理しておきます。

| 書くもの | 何を宣言するか | 出てきた項 |
|---------|--------------|-----------|
| `class TaskCreate(BaseModel):` | **受け取るデータの形** | 4.2.1 |
| `title: str = Field(min_length=1)` | **項目ごとの条件** | 4.3.1 |
| `@app.post("/tasks", response_model=TaskRead)` | **返すデータの形** | 4.4.1 |

`Field` は、第3章の `Query` / `Path` と同じ作りの兄弟です（4.3.1 の表）。
**値がクエリにあるなら `Query`、パスにあるなら `Path`、ボディの中の項目なら `Field`** と覚えてください。

---

**問 4.2 の解答**

**3** の `422` が返り、`loc` が `["body", "done"]` になる

**解説**

`done: bool` には**デフォルト値が書かれていない**ので、必須の項目です（4.2.2）。
必須の項目が送られてこなければ `422` になります。

```json
{"detail":[{"type":"missing","loc":["body","done"],"msg":"Field required","input":{"title":"買い物"}}]}
```

1 になるのは `done: bool = False` と書いた場合です。
2 の「`None` になる」ことは起こりません。`bool` は `None` を受け付けないからです。
4 の `500` は、**第3章のように `dict` で受け取っていた**ときの結果です（3.3.2）。
モデルで受け取るようにしたことで、同じ入力が `500` から `422` に変わった——
これがこの章の出発点です（4.1.1）。

---

**問 4.3 の解答**

**2** の「どちらも省略できるが、`null` を明示的に送ると下だけ `422` になる」

- `code: str | None = None` … 省略できる。`null` を送ってもよい
- `code: str = None` … **省略はできるが、`null` を明示的に送ると `422`**

**解説**

Pydantic は、**省略されたときのデフォルト値は検査しません。**
そのため `code: str = None` でも、省略しただけなら `None` が入って通ってしまいます。

問題は、呼ぶ側が `{"code": null}` と**明示的に送ったとき**です。

```json
{"detail":[{"type":"string_type","loc":["body","code"],"msg":"Input should be a valid string","input":null}]}
```

「省略はできるのに `null` は送れない」という、説明のつかない窓口になります。
さらに `/docs` の表示も「文字列」のままなので、**呼ぶ側からは何が正しいのか読み取れません。**

**省略できる項目には、必ず型のほうに `| None` を書いてください**（4.2.2 のよくある間違い）。

---

**問 4.4 の解答**

**保存しているデータの中の、外に出してはいけない情報が、うっかり漏れる事故**を防げます。

**解説**

4.4.2 の `owner.email` がその例でした。
`TaskRead` の `owner` を `OwnerRead`（`name` だけ）にした瞬間に、
**`create_task` のコードを1文字も変えずに** `email` が返らなくなりました。

`return` する直前に毎回 `del task["email"]` と書く方法もありますが、
**窓口が増えるたびに書き忘れる危険**があり、書き忘れても誰も教えてくれません。
「返してよいものを1か所で宣言する」ほうが、**書き忘れが起こりようがない**ぶん安全です。

第7章では、パスワードを保存する仕組みを扱います。
そこでは、この性質がそのまま**事故を防ぐ仕掛け**になります。

---

**問 4.5 の解答**

**作った側（自分のコード）に原因があります。**
根拠は、`loc` の1つ目が **`response`** になっているからです。

**解説**

`loc` の1つ目は「どこで問題が起きたか」を表します（3.4.2・4.4.1）。

| `loc` の1つ目 | 誰の問題か | どこを直すか |
|--------------|-----------|------------|
| `path` / `query` / `body` / `header` | **送った側** | リクエストの内容 |
| **`response`** | **作った側** | **自分の関数が返している値** |

このエラーは「`owner` を返すと宣言したのに、返す辞書に `owner` が無い」という意味です。
ブラウザ側には `500 Internal Server Error` としか出ないので、
**サーバー側のターミナルを見る**必要があります（第1章 1.2.3）。

直し方は、次のどちらかです。

1. 返す辞書に `owner` を入れる（多くの場合はこちら）
2. `TaskRead` の `owner` を任意の項目にする（本当に無いことがある場合）

---

**問 4.6 の解答**

`title` など**送っていない項目が `None` として書き込まれ、値が消えます。**
`TaskRead` の `title` は `None` を許していないため、実際にはレスポンスの関所で `500` になります。

**解説**

`model_dump()` は、**モデルの全項目**を辞書にします（4.2.3）。
`TaskUpdate` はすべての項目が `| None = None` なので、
`{"done": true}` だけを送っても、次の辞書ができます。

```python
{"title": None, "done": True, "priority": None, "tags": None}
```

これをそのまま反映すると、`title` が `None` で上書きされます。

```python
{"title": None, "done": True, ...}
```

`exclude_unset=True` を付けると、**実際に送られてきた項目だけ**が残ります（4.5.2）。

```python
{"done": True}
```

「送られてこなかった」と「`null` を送られた」を区別している点が肝です。
第3章 3.2.3 で、`bool | None = None` と `is not None` を使って
**「指定されなかった」を表した**のと同じ考え方です。

---

**問 4.7 の解答**

**`本番用`** になります。**環境変数のほうが `.env` より優先される**からです。

**解説**

読み込みの順番は、強いほうから次のとおりです（4.6.2）。

```text
環境変数  >  .env ファイル  >  クラスに書いたデフォルト値
```

この順番には理由があります。
`.env` は**開発中のパソコンで使う道具**で、本番のサーバーでは置きません。
本番では、サーバーの設定画面や Docker（第4冊目）から**環境変数として直接渡します**。
そのとき、うっかり残っていた `.env` に負けてしまっては困るからです（4.6.3 の注意）。

---

### 演習問題

### 演習 4.1 の解答

`main.py`（モデルの定義の並びに追記）

```python
class NoteCreate(BaseModel):
    text: str = Field(min_length=1, max_length=100, description="メモの本文")
    pinned: bool = False
```

`main.py`（`create_note` を次に差し替え）

```python
@app.post("/notes")
def create_note(new_note: NoteCreate):
    new_id = max([note["id"] for note in notes]) + 1
    created = {
        "id": new_id,
        # dict のキー指定ではなく、属性で取り出す
        "text": new_note.text,
        "pinned": new_note.pinned,
    }
    notes.append(created)
    return created
```

`/docs` から `{"text": "牛乳を買い忘れた"}` を送った結果

```json
{"id":2,"text":"牛乳を買い忘れた","pinned":false}
```

`{}` を送った結果

```json
{"detail":[{"type":"missing","loc":["body","text"],"msg":"Field required","input":{}}]}
```

`{"text": ""}` を送った結果

```json
{"detail":[{"type":"string_too_short","loc":["body","text"],"msg":"String should have at least 1 character","input":"","ctx":{"min_length":1}}]}
```

101文字の `text` を送った結果

```json
{"detail":[{"type":"string_too_long","loc":["body","text"],"msg":"String should have at most 100 characters","input":"あああ……","ctx":{"max_length":100}}]}
```

**解説**

第3章の演習 3.3 では、`.get("text", "（本文なし）")` を使って
**本文が無くても落ちないようにする**という対処をしていました（3.3.2）。

その解答の最後に、こう書きました。

> ただし、**これは正しい解決ではありません。**
> 本文が無いメモを `"（本文なし）"` として登録してしまうのは、
> おそらく利用者が期待した動作ではないからです。

いま、その正しい解決ができました。
**「`text` は必須」と宣言する**だけで、本文の無いメモは登録できなくなります。
`.get(...)` も `if` も書いていないことに注目してください。

| 第3章の書き方 | この章の書き方 |
|-------------|--------------|
| `new_note.get("text", "（本文なし）")` | `text: str = Field(min_length=1, ...)` |
| 落ちないようにするだけ | **そもそも受け付けない** |
| 呼ぶ側は間違いに気づけない | `422` で**何が悪いか伝わる** |

`pinned: bool = False` は、デフォルト値があるので任意です（4.2.2）。

> **よくある間違い**
> **`{"text": " "}`（空白1文字）は、この解答では通ります。**
> `min_length=1` は「1文字以上」なので、空白も1文字だからです。
> 空白だけを弾きたい場合は、4.3.3 の `@field_validator` が必要になります。
> **`Field` の条件で表せるのは「長さ」までで、「中身が意味を持つか」は表せません。**

> **よくある間違い**
> `/docs` の `Request body` に `{}` としか表示されない場合、
> **関数の型ヒントが `dict` のまま**です（4.2.1）。
> モデルを定義しただけでは何も変わりません。

---

### 演習 4.2 の解答

`main.py`（モデルの定義の並びに追記・`NoteCreate` の前に `Author` を置く）

```python
class Author(BaseModel):
    name: str
    email: str


class AuthorRead(BaseModel):
    # email を書かない。書かなかったものは返らない
    name: str


class NoteCreate(BaseModel):
    text: str = Field(min_length=1, max_length=100, description="メモの本文")
    pinned: bool = False
    author: Author


class NoteRead(BaseModel):
    id: int
    text: str
    pinned: bool
    author: AuthorRead
```

`main.py`（`create_note` を次に差し替え）

```python
@app.post("/notes", response_model=NoteRead, status_code=201)
def create_note(new_note: NoteCreate):
    new_id = max([note["id"] for note in notes]) + 1
    created = {
        "id": new_id,
        "text": new_note.text,
        "pinned": new_note.pinned,
        # 入れ子のモデルは辞書に変換してから保存する
        "author": new_note.author.model_dump(),
    }
    notes.append(created)
    return created
```

練習用の `notes` にも `author` を足しておきます。

```python
notes = [
    {"id": 1, "text": "会議は水曜に変更", "pinned": False,
     "author": {"name": "山田", "email": "yamada@example.com"}},
]
```

**Windows（PowerShell）**

`new_note.json` を作ってから実行します。

```json
{"text": "買い物", "author": {"name": "山田", "email": "yamada@example.com"}}
```

```powershell
curl.exe -i -X POST http://127.0.0.1:8000/notes -H "Content-Type: application/json" -d "@new_note.json"
```

**macOS / Linux**

```bash
curl -i -X POST http://127.0.0.1:8000/notes -H "Content-Type: application/json" -d '{"text": "買い物", "author": {"name": "山田", "email": "yamada@example.com"}}'
```

実行結果:

```text
HTTP/1.1 201 Created
content-type: application/json

{"id":2,"text":"買い物","pinned":false,"author":{"name":"山田"}}
```

`author` を送らなかった場合

```json
{"detail":[{"type":"missing","loc":["body","author"],"msg":"Field required","input":{"text":"買い物"}}]}
```

`author` の `email` だけを抜いた場合

```json
{"detail":[{"type":"missing","loc":["body","author","email"],"msg":"Field required","input":{"name":"山田"}}]}
```

**解説**

この演習の山は、**`Author` と `AuthorRead` を分けたこと**です。

保存されている辞書には、`email` が**そのまま入っています。**
消しているわけではありません。
**`NoteRead` の `author` に `AuthorRead` と書いた**ため、
返す直前の関所（4.4.1）で `email` が落ちています。

```text
保存: {"id":2,"text":"買い物","author":{"name":"山田","email":"yamada@example.com"}}
                                                   ↓ NoteRead の関所を通る
返す: {"id":2,"text":"買い物","pinned":false,"author":{"name":"山田"}}
```

`loc` が3つになる理由も確認しておいてください（4.2.3）。

```text
["body", "author", "email"]
  │        │         └ その中の email が
  │        └ ボディの中の author の
  └ 足りない
```

> **よくある間違い**
> **`AuthorRead` を作らずに、`NoteRead` の `author` を `Author` のままにする**間違いです。
> この場合、`email` は返り続けます。**エラーにはならないので気づけません。**
> 演習の完成条件に「`author` が `{"name": "山田"}` **だけ**になっている」と書いたのは、
> 目で確認しないと分からないからです。

> **よくある間違い**
> `status_code=201` を `response_model` と**並べて書く**ことに気づかず、
> `@app.post("/notes", status_code=201)` だけにしてしまうと、`email` が返ります。
> 逆に `response_model` だけにすると、`200` のままです。
> **デコレータには、両方をカンマで区切って書きます**（4.4.3）。

> **別解**
> `AuthorRead` を作らず、`author` の型を `str`（名前だけ）にする設計もあり得ます。
>
> ```python
> class NoteRead(BaseModel):
>     id: int
>     text: str
>     pinned: bool
>     author: str          # 名前だけを文字列で返す
> ```
>
> ただしこの場合、`create_note` の中で `"author": new_note.author.name` のように
> **形を作り直す**必要があります。
> 保存している形と返す形が離れるほど、変換のコードが増えます。
> **入れ子の形を保ったまま項目だけ減らす**解答のほうが、変換のコードが要らないぶん手数が少なくて済みます。

---

### 演習 4.3 の解答

`main.py`（モデルの定義の並びに追記）

```python
class NoteUpdate(BaseModel):
    # 更新では「送られてこなかった項目は変えない」ので、すべて任意にする
    text: str | None = Field(default=None, min_length=1, max_length=100)
    pinned: bool | None = None
```

`main.py`（第3章の `@app.put("/notes/{note_id}")` を、次にまるごと差し替え）

```python
@app.patch("/notes/{note_id}", response_model=NoteRead | None)
def update_note(new_note: NoteUpdate, note_id: int = Path(ge=1)):
    for note in notes:
        if note["id"] == note_id:
            # 実際に送られてきた項目だけを取り出す
            changes = new_note.model_dump(exclude_unset=True)
            for key, value in changes.items():
                note[key] = value
            return note
    # 見つからない場合は null。404 を返す方法は第5章 5.4.1
    return None
```

`{"pinned": true}` だけを送った結果

```json
{"id":1,"text":"会議は水曜に変更","pinned":true,"author":{"name":"山田"}}
```

`{"text": "新しい本文"}` だけを送った結果

```json
{"id":1,"text":"新しい本文","pinned":true,"author":{"name":"山田"}}
```

`{}` を送った結果（何も変わらない）

```json
{"id":1,"text":"新しい本文","pinned":true,"author":{"name":"山田"}}
```

`PATCH /notes/0`

```json
{"detail":[{"type":"greater_than_equal","loc":["path","note_id"],"msg":"Input should be greater than or equal to 1","input":"0","ctx":{"ge":1}}]}
```

`PATCH /notes/99`

```json
null
```

**解説**

第3章の解答では、更新を次のように書いていました（3.3.3）。

```python
note["text"] = new_note.get("text", note["text"])
note["pinned"] = new_note.get("pinned", note["pinned"])
```

**項目が増えるたびに、この行を書き足す**必要がありました。
書き忘れると、その項目だけ更新できない窓口になります。

`model_dump(exclude_unset=True)` を使うと、**項目の数に関係なく同じコード**で済みます（4.5.2）。

| 送ったボディ | `model_dump()` | `model_dump(exclude_unset=True)` |
|------------|---------------|--------------------------------|
| `{"pinned": true}` | `{"text": None, "pinned": True}` | **`{"pinned": True}`** |
| `{}` | `{"text": None, "pinned": None}` | **`{}`** |

`{}` を送ったときに `changes` が空の辞書になるので、
`for` の中身が一度も実行されず、**何も変わりません。**
これが「`{}` を送っても `500` にならない」という完成条件の理由です。

`response_model=NoteRead | None` にした理由も確認しておいてください（4.4.2）。
見つからないときに `{"message": "..."}` を返していると、
**`NoteRead` の形と違う**ため `500` になります。

> **よくある間違い**
> **`exclude_unset=True` を書き忘れる**間違いです。
> `{"pinned": true}` だけを送ったのに、`text` が消えます。
>
> ```json
> {"detail":"Internal Server Error"}
> ```
>
> `text` に `None` が入り、`NoteRead` の `text: str` に合わないため `500` になります。
> **サーバー側のターミナル**に `loc: ('response', 'text')` と出ていれば、この間違いです（4.4.1）。

> **よくある間違い**
> `PUT` を `PATCH` に変えるとき、**デコレータだけ変えて `@app.put` を消し忘れる**と、
> 同じパスに2つの窓口が残ります。
> `/docs` に `PUT /notes/{note_id}` の行が残っていたら、消し忘れです。

> **別解**
> `note.update(changes)` と1行で書くこともできます。
> 辞書の `update` は、別の辞書の内容をまとめて反映するメソッドです。
>
> ```python
> note.update(new_note.model_dump(exclude_unset=True))
> ```
>
> 動作は同じです。**短く書けますが、何が起きているかは `for` のほうが読み取れます。**

---

### 演習 4.4 の解答

`main.py`（`Settings` に2項目を追加）

```python
class Settings(BaseSettings):
    """アプリ全体の設定。"""

    app_name: str = "タスク管理 API"
    debug: bool = False
    notes_title: str = "メモ"
    notes_max: int = 100

    model_config = SettingsConfigDict(env_file=".env", env_file_encoding="utf-8")
```

`main.py`（`/notes/{note_id}` より**前**に追記）

```python
@app.get("/notes/info")
def read_notes_info():
    return {
        "title": settings.notes_title,
        "max": settings.notes_max,
        "count": len(notes),
    }
```

`.env`

```text
APP_NAME=タスク管理 API（開発用）
DEBUG=true
NOTES_TITLE=今日のメモ
NOTES_MAX=5
```

`.env.example`

```text
APP_NAME=
DEBUG=false
NOTES_TITLE=
NOTES_MAX=
```

`.env` を書く前

```json
{"title":"メモ","max":100,"count":1}
```

`.env` を書いて**サーバーを再起動したあと**

```json
{"title":"今日のメモ","max":5,"count":1}
```

環境変数で上書きした場合

**Windows（PowerShell）**

```powershell
$env:NOTES_TITLE = "環境変数のメモ"
fastapi dev main.py
```

**macOS / Linux**

```bash
NOTES_TITLE="環境変数のメモ" fastapi dev main.py
```

```json
{"title":"環境変数のメモ","max":5,"count":1}
```

**解説**

`.env` に書く名前とクラスの項目名の対応は、次のとおりです（4.6.2）。

| クラスの項目 | `.env` の名前 |
|------------|--------------|
| `notes_title` | `NOTES_TITLE` |
| `notes_max` | `NOTES_MAX` |

**大文字・小文字の違いは自動で読み替えられます。**
アンダースコアは、そのままアンダースコアです（第3章 3.5.1 のヘッダーとは違います）。

`NOTES_MAX=5` と**文字列で書いたのに `5`（数値）として返る**点にも注目してください。

```json
{"title":"今日のメモ","max":5,"count":1}
```

`.env` の中身はただの文字列ですが、`notes_max: int` と宣言してあるので、
**Pydantic が整数に変換しています。** 第3章 3.1.2 の型ヒントと同じ働きです。

`NOTES_MAX=abc` と書いた場合は、変換できないので**サーバーの起動時に止まります。**

```text
pydantic_core._pydantic_core.ValidationError: 1 validation error for Settings
notes_max
  Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='abc', input_type=str]
```

`type=int_parsing` は、第3章 3.1.3 で `/tasks/abc` を開いたときと**同じ種類**です。
**設定も、リクエストと同じ仕組みで検査されている**ことが分かります。

**起動時に止まるのは良いこと**です。
設定の間違いを、利用者からの最初のリクエストで気づくのではなく、
**起動した瞬間に気づける**からです。

> **よくある間違い**
> **`.env` を書き換えたのに反映されない**というつまずきが、この演習で最も多く起きます。
> 設定は `settings = Settings()` の行で**起動時に1回だけ**読み込まれます（4.6.2）。
> `main.py` の保存による自動リロード（2.4.3）は `.env` の変更を見ていません。
> **`Ctrl` + `C` で止めて、起動し直してください。**

> **よくある間違い**
> `.env` の値をクォートで囲む間違いです。
>
> ```text
> NOTES_TITLE="今日のメモ"
> ```
>
> この書き方でも読めますが、**囲む必要はありません。**
> 値に空白が入っていても、そのまま書けます（`APP_NAME=タスク管理 API`）。
> 一方、**`=` の前後に空白を入れると読めなくなります**（`NOTES_MAX = 5` は不可）。

> **よくある間違い**
> **`.env.example` に本物の値を書いてしまう**間違いです。
> `.env.example` は**共有してよいファイル**なので、
> 値を書いた時点で、それは共有される情報になります（4.6.3）。
> 名前だけを並べて、値は空にしてください。

> **別解**
> `/notes/info` ではなく、既存の `/info` に項目を足す作りでも構いません。
>
> ```python
> @app.get("/info")
> def read_info():
>     return {
>         "app_name": settings.app_name,
>         "debug": settings.debug,
>         "task_count": len(tasks),
>         "note_count": len(notes),
>     }
> ```
>
> どちらが良いかは、**「タスクとメモを別の機能として分けるかどうか」**で決まります。
> 分けるなら窓口も分けたほうが、第5章でファイルを分割するときに素直になります。
