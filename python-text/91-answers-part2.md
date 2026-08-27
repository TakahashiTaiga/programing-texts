---
title: "解答編 その2（第6章〜第10章）"
---

# 解答編 その2（第6章〜第10章）

**先に自分で解いてから読んでください。**

解けなかった問題は、この解答を読んだあと、
**何も見ずにもう一度自分で書き直して**ください。それで定着します。

第1章〜第5章の解答は [解答編 その1](./90-answers-part1.md) にあります。

> **解答を読んでも納得できないとき**
> AI に次のように聞いてください。
>
> ```text
> python-text の演習 6.4 の解答を読みましたが、
> なぜ report.py の中で from bookshop.prices import ... と書けるのかが納得できません。
> ```

---

## 第6章

### 理解度チェック

**問 6.1 の解答**

- A = **`price_utils.py`**
- B = **モジュール名**（`price_utils`）

**解説**

`import` に渡すのは、**拡張子 `.py` を取ったファイル名**です
（[6.1.2](./06-modules.md#612-自作モジュールを読み込む)）。
`import price_utils.py` と書くと、
`ModuleNotFoundError: No module named 'price_utils.py'` になります。

`import モジュール` の形で読み込んだときは、
中の関数を呼ぶのに**モジュール名から書く**必要があります。
`with_tax(1000)` だけで呼びたい場合は、
`from price_utils import with_tax` の形（[6.2.2](./06-modules.md#622-from--import-)）を使います。

---

**問 6.2 の解答**

エラーになるのは、**①と③**です。

| コード | 結果 | 理由 |
|-------|------|------|
| ① `import price_utils` → `with_tax(1000)` | `NameError` | 取り込まれたのはモジュール名だけ。関数名は直接使えない |
| ② `from price_utils import with_tax` → `with_tax(1000)` | 正常に動く | 関数名そのものが取り込まれている |
| ③ `from price_utils import with_tax` → `price_utils.with_tax(1000)` | `NameError` | `from` の形ではモジュール名が取り込まれない |

**解説**

①のエラーメッセージは次のとおりです。

```text
NameError: name 'with_tax' is not defined
```

③のエラーメッセージは次のとおりです。

```text
NameError: name 'price_utils' is not defined
```

**同じ `NameError` でも、どちらの名前が見つからないのかが違います。**
エラーメッセージのクォートの中を読むと、どちらの間違いかがすぐわかります。

- `'with_tax' is not defined` … `import モジュール` の形なのに、関数名だけで呼んでいる
- `'price_utils' is not defined` … `from` の形なのに、モジュール名から書いている

> **よくある間違い**
> 「両方の書き方を混ぜて書けば安全だろう」と、
> `import price_utils` と `from price_utils import with_tax` を両方書く人がいます。
> 動きはしますが、同じファイルの中で `with_tax(...)` と `price_utils.with_tax(...)` が
> 混在することになり、読む人が混乱します。
> **1つのファイルの中では、どちらかに揃えてください**（[6.2.2](./06-modules.md#622-from--import-)）。

---

**問 6.3 の解答**

**理由**：Python がモジュールを探す1番目の場所は、
**実行した `.py` ファイルが置かれているディレクトリ**（この場合は `other`）であり、
`price_utils.py` はそこにないため。

**対処**（どれか1つ）：

- `price_utils.py` を `other` の中にも置く（またはコピーではなく移動する）
- `main.py` を `python-lesson` の直下に移し、`python main.py` として実行する
- `other` をパッケージにして、`python-lesson` 側から読み込む形にする（[6.5](./06-modules.md#65-パッケージ)）

**解説**

いちばん多い誤解は、**「`cd` した場所が基準になる」**というものです。
基準になるのは `cd` した場所ではなく、**`python` に渡したファイルの場所**です
（[6.1.3](./06-modules.md#613-同じディレクトリにないと読み込めない問題)）。

`show_path.py` で確かめたとおり、`other` の中に移動してから
`python ../show_path.py` と実行しても、探す場所は `python-lesson` のままでした。
逆に、`python-lesson` にいながら `python other/main.py` と実行すると、
探す場所は `other` になります。

**「どこにいるか」ではなく「どのファイルを動かしたか」。** ここだけ押さえておいてください。

> **よくある間違い**
> ファイル名のつづり間違いでも、まったく同じ `ModuleNotFoundError` が出ます。
> `price_utils.py` と `price_util.py`（`s` がない）、
> `Price_utils.py`（大文字）はすべて別物です。
> macOS では大文字小文字を区別しない設定のこともあるため、
> **Windows と macOS で動きが変わる**という形で現れることもあります。

---

**問 6.4 の解答**

```text
2
3
2
```

**解説**

- `math.floor(2.7)` … **切り捨て**なので `2`
- `math.ceil(2.1)` … **切り上げ**なので `3`
- `round(2.5)` … **`3` ではなく `2`**

3つめが、この問題の本題です。
Python の `round()` は、**ちょうど中間（`.5`）のときは偶数側に丸めます**
（[6.3.4](./06-modules.md#634-math--数学関数)）。
`round(2.5)` は `2`、`round(3.5)` は `4` です。

学校で習った四捨五入とは違う動きなので、
**「四捨五入したい」つもりで `round()` を使うと、`.5` のときだけ答えがずれます。**
金額の計算では致命的になりかねません。

やりたいことがはっきりしているなら、名前でそれを指定してください。

- 切り上げたい → `math.ceil`
- 切り捨てたい → `math.floor`

---

**問 6.5 の解答**

**理由**：`import` は、読み込んだファイルを**上から下まで実行する**ため。
`print(hello("テスト"))` の行も、そのときに実行される。

**書き換え**：

```python
def hello(name):
    return f"こんにちは、{name}さん"


if __name__ == "__main__":
    print(hello("テスト"))
```

**解説**

`import greeting` は「関数の定義だけを持ってくる」のではありません。
**`greeting.py` を1行目から最後まで実行**します
（[6.4.1](./06-modules.md#641-if-__name__--__main__-の意味)）。
`def` の行を実行するから関数が使えるようになるわけで、
`print` の行も同じように実行されてしまいます。

`if __name__ == "__main__":` を挟むと、次のように分かれます。

| 実行のしかた | `greeting.py` の `__name__` | `if` の判定 | `print` は |
|------------|--------------------------|-----------|-----------|
| `python greeting.py` | `"__main__"` | 真 | 動く |
| 別のファイルから `import greeting` | `"greeting"` | 偽 | 動かない |

動作確認をファイルの中に残したまま、モジュールとしても安全に使える——これが目的です
（[6.4.2](./06-modules.md#642-なぜ必要なのか)）。

> **よくある間違い**
> `__main__` のアンダースコアは、**前後に2つずつ**です。
> `_main_` や `"main"` と書いてもエラーにはならず、
> **条件が偽になって何も表示されない**という形で現れます。
> 「何も出ない」と思ったら、`print(__name__)` を1行足して値を確かめてください。

---

**問 6.6 の解答**

```text
3
0
[('赤', 3)]
```

**解説**

- `counts["赤"]` … `赤` は3回出てくるので `3`
- `counts["黄"]` … **`KeyError` になりません。`0` が返ります**
- `counts.most_common(1)` … 多い順に1件を、**`(名前, 回数)` のタプルのリスト**で返します

2つめが `Counter` の便利なところです（[6.3.5](./06-modules.md#635-collections--便利なデータ構造)）。
普通の辞書なら、入っていないキーを `[ ]` で読むと `KeyError` になり、
それを避けるために `get(キー, 0)` と書く必要がありました
（[4.3.3](./04-data-structures.md#433-get-で安全に取り出す)）。
`Counter` は、その気配りが最初から入っています。

3つめは、**戻り値の形**に注意してください。
`most_common(1)` が返すのは `('赤', 3)` ではなく、**それを1つ入れたリスト** `[('赤', 3)]` です。
`for` で回すことを前提にした形なので、
1件だけ欲しいときも `counts.most_common(1)[0]` のように取り出します。

---

**問 6.7 の解答**

| 書き方 | 呼び名 | 指しているファイル |
|-------|-------|-----------------|
| `from shop.taxes import with_tax` | 絶対 import | `shop/taxes.py` |
| `from .taxes import with_tax` | 相対 import | `shop/taxes.py`（自分と同じディレクトリの `taxes.py`） |

**解説**

`shop/formatting.py` の中から見た場合、**どちらも同じファイルを指します**
（[6.5.3](./06-modules.md#653-相対-import-と絶対-import)）。

違うのは、**何を基準に書くか**です。

- 絶対 import … パッケージのいちばん外側（`shop`）から書き始める。
  都道府県から書く住所のようなもの
- 相対 import … 先頭の `.` が「このファイルと同じディレクトリ」を表す。
  「同じ階の隣の部屋」という言い方

このテキストでは**絶対 import を基本**とします。
相対 import には、**パッケージの中のファイルを直接実行できなくなる**という落とし穴があるためです。

```text
ImportError: attempted relative import with no known parent package
```

このエラーが出たら、「パッケージの中のファイルを直接実行した」と読んでください。

---

### 演習 6.1 の解答

`python-lesson/text_utils.py`

```python
"""文字列を扱う道具をまとめたモジュール。"""


def shout(text):
    """前後の空白を取り除き、すべて大文字にして返す。"""
    return text.strip().upper()


def initials(names):
    """名前のリストから、各要素の1文字目をつなげた文字列を返す。"""
    result = ""
    for name in names:
        result += name[0]
    return result.upper()


if __name__ == "__main__":
    print(shout("  hello  "))
    print(initials(["abe", "baba", "chiba"]))
```

`python-lesson/main_text.py`

```python
import text_utils

message = "  python  "
names = ["Tanaka", "Sato", "Kimura"]

print(text_utils.shout(message))
print(text_utils.initials(names))
```

```text
python text_utils.py の実行結果:
HELLO
ABC

python main_text.py の実行結果:
PYTHON
TSK
```

**解説**

**1. `shout` のメソッドは、つなげて書けます**

```python
return text.strip().upper()
```

`text.strip()` が「空白を取り除いた**新しい文字列**」を返すので、
その結果に対して続けて `.upper()` を呼べます
（[2.4.3](./02-basics.md#243-よく使うメソッド)）。
分けて書いてもかまいません。

```python
def shout(text):
    """前後の空白を取り除き、すべて大文字にして返す。"""
    trimmed = text.strip()
    return trimmed.upper()
```

**2. `initials` は「累積パターン」です**

空の文字列から始めて、`for` で1文字ずつ足していきます
（[3.2.2](./03-control-flow.md#322-range) の `total.py` と同じ形で、
`total = 0` が `result = ""` になっただけです）。
`name[0]` が1文字目です（[2.4.2](./02-basics.md#242-インデックスとスライス)）。

最後の `.upper()` は、`Tanaka` の `T` はすでに大文字ですが、
動作確認の `abe` のような小文字にも対応するために入れています。

**3. `if __name__ == "__main__":` が、この演習の本題です**

これがないと、`main_text.py` の実行結果が次のようになってしまいます。

```text
HELLO
ABC
PYTHON
TSK
```

`import text_utils` が `text_utils.py` を上から下まで実行し、
動作確認の `print` まで動くためです（[6.4.2](./06-modules.md#642-なぜ必要なのか)）。

**4. 呼び出し方の指定に注意してください**

`main_text.py` は `import text_utils` の形なので、
**`text_utils.shout(...)` とモジュール名から書きます**（[6.2.1](./06-modules.md#621-import-モジュール)）。
`from text_utils import shout` と書いてしまうと、
`text_utils.shout(...)` は `NameError` になります。

> **よくある間違い**
> `initials` を次のように書くと、`TSK` ではなく `TanakaSatoKimura` になります。
>
> ```python
> result += name        # name[0] ではなく name を足している
> ```
>
> `[0]` の付け忘れです。**リストの要素は文字列**なので、
> そのままだと単語まるごとが足されます。

---

### 演習 6.2 の解答

`python-lesson/deadline.py`

```python
from datetime import date, timedelta

WEEKDAY_NAMES = ["月", "火", "水", "木", "金", "土", "日"]

start = date(2026, 4, 1)
deadline = start + timedelta(days=45)
diff = deadline - start

print(f"締め切り: {deadline.strftime('%Y年%m月%d日')}")
print(f"曜日: {WEEKDAY_NAMES[deadline.weekday()]}曜日")
print(f"期間: {diff.days}日間")
print(f"開始: {start.strftime('%Y/%m/%d')}")
```

```text
実行結果:
締め切り: 2026年05月16日
曜日: 土曜日
期間: 45日間
開始: 2026/04/01
```

**解説**

**1. 日付を手で計算しないのが、この問題の目的です**

4月は30日までなので、4月1日の45日後は「4月に29日ぶん進めて、残り16日を5月に」——
と暗算すると、月末の日数を間違えます。
`timedelta(days=45)` を足せば、**月をまたぐ計算は `datetime` がやってくれます**
（[6.3.2](./06-modules.md#632-datetime--日付と時刻)）。

**2. 日数を取り出すのは `.days` です**

`deadline - start` が返すのは数値ではなく `timedelta` です。
そのまま `print` すると `45 days, 0:00:00` と表示されてしまうので、
**`.days` で日数だけを取り出します。**
`days` は関数ではないので `()` は付けません。

**3. 曜日は数字で返ります**

`weekday()` が返すのは `0`〜`6` の数字で、**月曜が `0`** です。
日本語にするには、その順番でリストを用意して、
`WEEKDAY_NAMES[deadline.weekday()]` と**インデックスに使います**
（[4.1.2](./04-data-structures.md#412-要素の取り出しと書き換え)）。

`WEEKDAY_NAMES` を大文字で書いているのは、
**変えない値（定数）** だと示す慣習です（[2.1.4](./02-basics.md#214-定数の慣習)）。

**4. f-string の中の引用符に注意してください**

```python
print(f"締め切り: {deadline.strftime('%Y年%m月%d日')}")
```

外側が `"` なので、中の `strftime` に渡す文字列は `'` にします。
同じ引用符を使うと、そこで文字列が終わったと判断されてエラーになります
（[4.3.5](./04-data-structures.md#435-辞書のリスト実務で最頻出の形) と同じ理由です）。

> **別解：f-string の外で組み立てる**
> 引用符の入れ子が読みにくければ、先に変数にしてもかまいません。
>
> ```python
> deadline_text = deadline.strftime("%Y年%m月%d日")
> print(f"締め切り: {deadline_text}")
> ```
>
> こちらのほうが、あとから読む人には親切です。

> **よくある間違い**
> `import datetime` と書いた場合、`date(2026, 4, 1)` は `NameError` になります。
> その形では `datetime.date(2026, 4, 1)` と書く必要があります。
> 問題の条件（モジュール名を先頭に付けない）から、
> **`from datetime import ...` の形**が指定されています。

---

### 演習 6.3 の解答

`python-lesson/survey.py`

```python
"""アンケートの集計をまとめたモジュール。"""

from collections import Counter, defaultdict


def count_answers(answers):
    """回答のリストから「回答 → 件数」を数えて返す。"""
    return Counter(answers)


def ranking(answers, top):
    """多い順に top 件を (回答, 件数) のリストで返す。"""
    return count_answers(answers).most_common(top)


def group_by_grade(people):
    """「学年 → その学年の名前のリスト」の辞書を返す。"""
    groups = defaultdict(list)
    for person in people:
        groups[person["grade"]].append(person["name"])
    return groups


if __name__ == "__main__":
    print(count_answers(["A", "B", "A"]))
    print(ranking(["A", "B", "A"], 1))
    print(dict(group_by_grade([{"name": "テスト", "grade": 1}])))
```

`python-lesson/main_survey.py`

```python
from survey import count_answers, group_by_grade, ranking

answers = ["カレー", "寿司", "カレー", "ラーメン", "寿司", "カレー"]
people = [
    {"name": "佐藤", "grade": 1},
    {"name": "鈴木", "grade": 2},
    {"name": "高橋", "grade": 1},
    {"name": "田中", "grade": 3},
    {"name": "伊藤", "grade": 2},
]

counts = count_answers(answers)
print(f"カレーは{counts['カレー']}票です")

for rank, (name, count) in enumerate(ranking(answers, 2), start=1):
    print(f"{rank}位: {name}（{count}票）")

groups = group_by_grade(people)
for grade in sorted(groups):
    print(f"{grade}年生: {', '.join(groups[grade])}")
```

```text
実行結果:
カレーは3票です
1位: カレー（3票）
2位: 寿司（2票）
1年生: 佐藤, 高橋
2年生: 鈴木, 伊藤
3年生: 田中
```

**解説**

**1. `ranking` は `count_answers` を呼んでいます**

```python
def ranking(answers, top):
    return count_answers(answers).most_common(top)
```

同じモジュールの中の関数は、`import` なしでそのまま呼べます。
`Counter(answers).most_common(top)` と書いても動きますが、
**数える方法を1か所にまとめておくほうが**、あとで数え方を変えたくなったときに楽です
（[5.6.3](./05-functions.md#563-どこで関数に切り出すか) の「1つの関数は1つのこと」）。

**2. 順位の表示は、タプルのアンパックです**

```python
for rank, (name, count) in enumerate(ranking(answers, 2), start=1):
```

`ranking` が返すのは `[("カレー", 3), ("寿司", 2)]` です。
`enumerate` がそれに番号を付けるので、
1回のループで受け取る値は `(1, ("カレー", 3))` という形になります。

これを `rank` と `(name, count)` の2つに分けています。
**かっこで囲んだ `(name, count)` が、タプルをその場で2つに分ける書き方**です
（[4.2.3](./04-data-structures.md#423-アンパック)）。
[6.3.5](./06-modules.md#635-collections--便利なデータ構造) の `coll_ranking.py` と同じ形です。

**3. 学年の並び順は `sorted(辞書)` で決めます**

`defaultdict` は「最初に出てきた順」に並ぶので、そのまま回すと `1, 2, 3` の順になるとは限りません。
`sorted(groups)` は**キーだけを小さい順に並べたリスト**を返すので
（[4.3.4](./04-data-structures.md#434-キー値両方を回す)）、これで表示順が決まります。

**4. 名前のつなぎ方**

`groups[grade]` は `['佐藤', '高橋']` というリストです。
そのまま f-string に入れるとかっこと引用符が付いてしまうので、
`", ".join(...)` で1つの文字列にします（[6.3.5](./06-modules.md#635-collections--便利なデータ構造)）。

> **よくある間違い**
> `count_answers` を、第4章の書き方のまま提出してしまう例です。
>
> ```python
> def count_answers(answers):
>     counts = {}
>     for answer in answers:
>         counts[answer] = counts.get(answer, 0) + 1
>     return counts
> ```
>
> **動きますが、この演習の条件（`collections` の道具を使うこと）を満たしていません。**
> さらに、この形だと `ranking` で `most_common` が使えず、
> 多い順に並べる処理を自分で書くことになります。
> `Counter` を選ぶ理由は、**数えたあとにやりたいことまで含めて短くなる**ところにあります。

> **よくある間違い**
> `for grade in sorted(groups):` を `for grade in groups:` と書くと、
> 学年の並びがデータの出現順（`1, 2, 3`）になります。
> このデータではたまたま正しく見えますが、
> `people` の並びを変えると崩れます。**表示順は自分で決めてください。**

---

### 演習 6.4 の解答

`python-lesson/bookshop/prices.py`

```python
"""本の価格の計算をまとめたモジュール。"""

import math

TAX_RATE = 0.1


def with_tax(price):
    """税込価格を切り上げた整数で返す。"""
    return math.ceil(price * (1 + TAX_RATE))


def shipping_fee(subtotal):
    """送料を返す（税込小計が3000円以上なら無料）。"""
    if subtotal >= 3000:
        return 0
    return 500
```

`python-lesson/bookshop/report.py`

```python
"""表示用の文字列と集計をまとめたモジュール。"""

from bookshop.prices import shipping_fee, with_tax


def book_line(book):
    """「星の王子さま（税込1408円）」の形の文字列を返す。"""
    return f"{book['title']}（税込{with_tax(book['price'])}円）"


def summary(books):
    """税込小計・送料・合計をまとめた辞書を返す。"""
    subtotal = 0
    for book in books:
        subtotal += with_tax(book["price"])
    fee = shipping_fee(subtotal)
    return {"subtotal": subtotal, "shipping": fee, "total": subtotal + fee}
```

`python-lesson/bookshop/__init__.py`

```python
"""bookshop パッケージ。よく使うものをここから取り出せるようにする。"""

from bookshop.prices import shipping_fee, with_tax
from bookshop.report import book_line, summary
```

`python-lesson/main_bookshop.py`

```python
from bookshop import book_line, summary

books = [
    {"title": "星の王子さま", "price": 1280},
    {"title": "こころ", "price": 693},
    {"title": "銀河鉄道の夜", "price": 605},
]


if __name__ == "__main__":
    ordered = sorted(books, key=lambda book: book["price"], reverse=True)
    for number, book in enumerate(ordered, start=1):
        print(f"{number}. {book_line(book)}")

    result = summary(books)
    print(f"小計: {result['subtotal']}円")
    print(f"送料: {result['shipping']}円")
    print(f"合計: {result['total']}円")
```

```text
実行結果:
1. 星の王子さま（税込1408円）
2. こころ（税込763円）
3. 銀河鉄道の夜（税込666円）
小計: 2837円
送料: 500円
合計: 3337円
```

**解説**

**1. `math.ceil` でなければ、この結果になりません**

| 本 | 税抜 | `price * 1.1` | `math.ceil` | `round` | `int` |
|----|------|--------------|------------|--------|-------|
| こころ | 693 | 762.3000... | **763** | 762 | 762 |
| 銀河鉄道の夜 | 605 | 665.5000... | **666** | 666 | 665 |

`round` でも `int` でも、どこかで1円ずれます
（[6.3.4](./06-modules.md#634-math--数学関数) の比較表）。
**「切り上げる」と決めたなら `math.ceil` を使う**——
これが、この演習で `763` という数字を指定した理由です。

**2. `report.py` が `prices.py` を読み込む形**

```python
from bookshop.prices import shipping_fee, with_tax
```

**同じパッケージの中にあっても、`import` は必要です。**
そして書き方は `bookshop.prices` と、**パッケージ名から**です
（[6.5.3](./06-modules.md#653-相対-import-と絶対-import) の絶対 import）。
`from prices import ...` と書くと、
`main_bookshop.py` から実行したときに `ModuleNotFoundError` になります。

`from .prices import ...`（相対 import）でも動きますが、
このテキストでは絶対 import に統一しています。

**3. `__init__.py` が「窓口」です**

```python
from bookshop.prices import shipping_fee, with_tax
from bookshop.report import book_line, summary
```

これを書いておくことで、使う側は
`from bookshop import book_line, summary` と**1行**で取り込めます
（[6.5.2](./06-modules.md#652-__init__py)）。

書かなかった場合、`main_bookshop.py` はこう書くことになります。

```python
from bookshop.report import book_line, summary
```

こちらでも動きますが、**使う側が「`book_line` は `report.py` にある」と知っている必要があります。**
あとで `report.py` を分割したら、使う側も直すことになります。
`__init__.py` は、その事情を外に見せないための仕組みです。

**4. 番号を付けるのは呼び出す側の仕事です**

`book_line` は `星の王子さま（税込1408円）` までを返し、
`1. ` は `main_bookshop.py` の `enumerate` で付けています
（[3.2.3](./03-control-flow.md#323-enumerate-で番号付きに回す)）。

これは、[5.6.1](./05-functions.md#561-1つの関数は1つのことをする) の
「計算する関数と表示する処理を分ける」と同じ考え方です。
`book_line` の中で番号を付けてしまうと、
番号なしで使いたくなったときに使い回せません。

**5. 3つの値は辞書で返します**

```python
return {"subtotal": subtotal, "shipping": fee, "total": subtotal + fee}
```

`return subtotal, fee, subtotal + fee` とタプルで返すこともできますが、
受け取る側が**順番を覚えていないといけません**。
返す値が3つ以上になったら辞書1つにまとめる、というのが
[5.3.2](./05-functions.md#532-複数の値を返す) の指針です。

> **よくある間違い**
> `summary(books)` に**並べ替えたあとの `ordered`** を渡しても、合計は同じです。
> ただし、`sorted` を使わず `books.sort(...)` と書いてしまうと、
> **元の `books` の並びが変わります**（[4.1.6](./04-data-structures.md#416-sort-と-sorted-の違い破壊的か否か)）。
> 問題の条件に「データを書き換えないこと」とあるので、
> ここは `sorted` を使ってください。

> **よくある間違い**
> `bookshop` ディレクトリを `main_bookshop.py` と**別の場所**に作ると、
> 次のエラーになります。
>
> ```text
> ModuleNotFoundError: No module named 'bookshop'
> ```
>
> パッケージのディレクトリは、**実行するファイルと同じ場所**に置いてください
> （[6.5.1](./06-modules.md#651-ディレクトリでまとめる)）。

> **別解：`summary` の中で `sum` と内包表記を使う**
>
> ```python
> def summary(books):
>     """税込小計・送料・合計をまとめた辞書を返す。"""
>     subtotal = sum([with_tax(book["price"]) for book in books])
>     fee = shipping_fee(subtotal)
>     return {"subtotal": subtotal, "shipping": fee, "total": subtotal + fee}
> ```
>
> 内包表記（[4.5](./04-data-structures.md#45-内包表記)）で税込価格のリストを作り、
> `sum()` で合計しています。3行が1行になりますが、
> **読みにくいと感じるなら `for` のままでかまいません。**
