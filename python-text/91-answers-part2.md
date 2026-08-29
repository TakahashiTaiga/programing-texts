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

---

## 第7章

### 理解度チェック

**問 7.1 の解答**

- A = **ターミナルがいまいるディレクトリ**（カレントディレクトリ）
- B = **実行した `.py` ファイルが置かれているディレクトリ**

**解説**

**この2つは基準が違います。** ここが第7章で最初につまずくところです。

| 何を探すとき | 基準になる場所 | 確認方法 |
|-------------|--------------|---------|
| `open("memo.txt")` のファイル | ターミナルの現在地 | `Path.cwd()`（[7.3.2](./07-files-and-exceptions.md#732-パスを組み立てる)） |
| `import price_utils` のモジュール | 実行した `.py` の場所 | `sys.path[0]`（[6.1.3](./06-modules.md#613-同じディレクトリにないと読み込めない問題)） |

そのため、`python-lesson` の1つ上のディレクトリから

```powershell
python python-lesson/read_all.py
```

と実行すると、**スクリプトそのものは動くのに `memo.txt` だけが見つからない**、
という一見不可解な状態になります。

対策は、**必ず `cd` してから実行する**ことです。
どうしても分からなくなったら、プログラムの先頭に次の2行を入れて確かめてください。

```python
from pathlib import Path

print(Path.cwd())
```

---

**問 7.2 の解答**

**直す点**：`open("memo.txt")` を `open("memo.txt", encoding="utf-8")` にする。

**直さないと何が起きるか**：Windows では既定の文字コードが cp932 になることがあり、
UTF-8 で保存されたファイルを読むと `UnicodeDecodeError` になるか、文字化けした文字列が返ってくる。

**解説**

厄介なのは、**書いた本人の環境では動いてしまう**ことです。
macOS と Linux の既定は UTF-8 なので、このコードは問題なく動きます。
壊れるのは、それを Windows の人に渡したときです（[7.1.4](./07-files-and-exceptions.md#714-文字コードの指定encodingutf-8)）。

```text
UnicodeDecodeError: 'cp932' codec can't decode byte 0x94 in position 8: illegal multibyte sequence
```

さらに悪いのは、**エラーにならず文字化けするだけ**の場合です。
`繧翫ｓ縺` のような文字列がそのまま集計に流れ込み、
結果を見るまで誰も気づきません。

> **よくある間違い**
> 書き込み側だけ `encoding="utf-8"` を付けて、読み込み側を忘れるパターンが多いです。
> **読むときも書くときも、両方に付けてください。**

---

**問 7.3 の解答**

```text
① ['りんご', 'みかん', 'ぶどう']
② ['りんご\n', 'みかん\n', 'ぶどう\n']
```

**解説**

どちらもリストを返しますが、**改行を残すかどうか**が違います
（[7.1.2](./07-files-and-exceptions.md#712-1行ずつ読む)）。

- `splitlines()` … 改行で区切り、**改行は捨てる**
- `readlines()` … 1行ずつに分けるが、**改行は付いたまま**

②のまま `print` すると、行のあいだに空行が入ります。
また、`"りんご\n" == "りんご"` は `False` なので、
**比較や `in` による検索も期待どおりに動きません。**

このテキストでは、**行の一覧がほしいときは `splitlines()`** を使います。
`readlines()` を使ってしまった場合は、
`[line.rstrip() for line in f.readlines()]` のように内包表記
（[4.5.1](./04-data-structures.md#451-リスト内包表記)）で取り除いてください。

---

**問 7.4 の解答**

**中身**：空になる（0文字のファイルになる）。

**理由**：`"w"` は**開いた時点で中身を空にする**モードだから。書き込む前に消える。

**解説**

`pass` は「何もしない」という文（[3.3.4](./03-control-flow.md#334-pass)）なので、
一見すると何も起きないように見えます。
しかし、消しているのは `pass` ではなく **`open` のほう**です
（[7.2.2](./07-files-and-exceptions.md#722-w-で開くと中身が消える)）。

**しかもエラーは1つも出ません。** ここが `"w"` の怖いところです。

| やりたいこと | 使うモード |
|-------------|-----------|
| 末尾に書き足したい | `"a"` |
| 作り直したい（消えてよい） | `"w"` |
| すでにあるなら止まってほしい | `"x"`（`FileExistsError` が出る） |

> **よくある間違い**
> 「ファイルを読んでから、同じファイルに追記しよう」と考えて
> `open(path, "w")` と書いてしまう事故が非常に多いです。
> **読んだ内容を変数に持っていても、`"w"` で開いた時点で元ファイルは空になります。**
> 元に戻せません。大事なファイルを扱うときは、
> まず `"x"` で別名に書いてみて、意図どおりか確かめてください。

---

**問 7.5 の解答**

**理由**：`csv.DictReader` が返す値は**すべて文字列**なので、
`row["price"]` は `180` ではなく `"180"` であり、
`"180" * 3` は掛け算ではなく**文字列の3回繰り返し**になるため。

**実際の結果**

```text
180180180
```

**正しい書き方**

```python
print(int(row["price"]) * 3)
```

**解説**

`*` は、左が数値なら掛け算、左が文字列なら繰り返しになります（[2.4.1](./02-basics.md#241-連結と繰り返し)）。
**そのため、エラーが出ません。** これが最もたちの悪いパターンです。

CSV から読んだ値を計算に使うときは、必ず `int()` か `float()` で変換してください
（[2.2.6](./02-basics.md#226-型を変換する) / [7.4.1](./07-files-and-exceptions.md#741-csv-モジュール)）。

> **補足**
> `+` でも同じことが起きます。
> `row["price"] + row["quantity"]` は `"1803"` という文字列になります。
> 「合計がやたら大きい」「数字が横に並んでいる」と感じたら、
> **`int()` の付け忘れ**を疑ってください。

---

**問 7.6 の解答**

```text
memo.txt: 3行
----
menu.txt: なし
----
```

**解説**

1回目（`memo.txt`）は例外が起きないので、`except` は飛ばされ **`else` が実行**されます。
2回目（`menu.txt`）は `FileNotFoundError` が起きるので **`except` が実行**され、`else` は飛ばされます。

そして **`finally` は、どちらの場合も実行されます**
（[7.5.4](./07-files-and-exceptions.md#754-else-と-finally)）。だから `----` が2回出ます。

| ブロック | 1回目（成功） | 2回目（失敗） |
|---------|-------------|-------------|
| `try` | 実行される | 実行される（途中で中断） |
| `except` | 飛ばされる | **実行される** |
| `else` | **実行される** | 飛ばされる |
| `finally` | **実行される** | **実行される** |

`else` と `except` は、**必ずどちらか片方だけ**が動きます。両方動くことはありません。

---

**問 7.7 の解答**

問題は次の2つです。

1. **`except:` と種類を書いていない（裸の `except`）**
   … `KeyboardInterrupt`（`Ctrl` + `C` による中断）まで捕まえてしまい、
   プログラムを止められなくなることがある。想定外の例外も一緒に隠してしまう
2. **`except` の中が `pass` で、`return 0` を返している**
   … 失敗したのに何も記録されず、`0` という**それらしい値**が返るため、
   呼び出し側は「ファイルが空だった」のか「読めなかった」のかを区別できない

**解説**

このコードでファイル名を打ち間違えると、次のようになります。

```python
print(read_count("shoping.txt"))   # p が1つ足りない
```

```text
0
```

**エラーも警告も出ません。** 「0行のファイルだった」という顔をして返ってきます
（[7.5.5](./07-files-and-exceptions.md#755-握りつぶさないexcept-pass-の危険)）。

書き直すなら、次のどちらかです。

**案1：捕まえない（呼び出し側に任せる）**

```python
def read_count(path):
    """ファイルの行数を返す。読めなければ例外はそのまま呼び出し元へ渡す。"""
    with open(path, encoding="utf-8") as f:
        return len(f.read().splitlines())
```

**案2：種類を指定して、何が起きたかを残す**

```python
def read_count(path):
    """ファイルの行数を返す。ファイルがなければメッセージを出して -1 を返す。"""
    try:
        with open(path, encoding="utf-8") as f:
            return len(f.read().splitlines())
    except FileNotFoundError as e:
        print(f"読み込めませんでした: {e}")
        return -1
```

案2で `0` ではなく `-1` を返しているのは、
**「正常な結果としてありえない値」**にして、失敗と区別できるようにするためです。
どちらを選ぶかは、[7.6.3](./07-files-and-exceptions.md#763-どこで例外を捕まえるべきか) の基準
（その場で対処を決められるか）で判断してください。

---

### 演習 7.1 の解答

`python-lesson/shopping.txt`

```text
牛乳
食パン
たまご
バター
```

`python-lesson/line_report.py`

```python
with open("shopping.txt", encoding="utf-8") as f:
    items = f.read().splitlines()

lines = []
for i, item in enumerate(items, start=1):
    lines.append(f"{i}. {item}（{len(item)}文字）")
lines.append(f"合計: {len(items)}件")

for line in lines:
    print(line)

# "w" で開く（"a" だと実行のたびに内容が積み上がってしまうため）
with open("report.txt", "w", encoding="utf-8") as f:
    f.write("\n".join(lines) + "\n")
```

```text
実行結果:
1. 牛乳（2文字）
2. 食パン（3文字）
3. たまご（3文字）
4. バター（3文字）
合計: 4件
```

`report.txt` の中身も同じ5行になります。

**解説**

この演習の要点は3つです。

**1. 表示用の行を、いったんリストに貯める**

画面表示とファイル書き出しで**同じ内容**を使うので、
先に `lines` に組み立ててしまうと、
表示も書き出しもそのリストを使うだけで済みます。
`print` の中で文字列を組み立ててしまうと、書き出し側でもう一度同じ式を書くことになります。

**2. モードは `"w"`**

`"a"` にすると、2回目の実行で `report.txt` が10行になります。
「2回実行しても2倍にならないこと」という条件が、
`"w"` を指定させるための条件でした（[7.2.1](./07-files-and-exceptions.md#721-上書きモードと追記モード)）。

**3. 末尾の改行を自分で足す**

`"\n".join(lines)` は**要素のあいだ**にだけ改行を入れるので、
最後の行のうしろには何も付きません（[7.2.3](./07-files-and-exceptions.md#723-改行の扱い)）。
`+ "\n"` を忘れると、`report.txt` の最終行に改行がない状態になります。

> **別解：1行ずつ `write` する**
>
> ```python
> with open("report.txt", "w", encoding="utf-8") as f:
>     for line in lines:
>         f.write(line + "\n")
> ```
>
> こちらでも結果は同じです。行数が非常に多い場合（数十万行など）は、
> `join` で巨大な文字列を1つ作らずに済むこちらのほうが向いています。

> **よくある間違い**
> `f.write(lines)` と、**リストをそのまま渡す**間違いが多いです。
>
> ```text
> TypeError: write() argument must be str, not list
> ```
>
> `write` に渡せるのは**文字列だけ**です。
> リストを渡したいときは、`join` で1本の文字列にしてください。

---

### 演習 7.2 の解答

`python-lesson/make_notes.py`

```python
from pathlib import Path

notes_dir = Path("notes")
# exist_ok=True: 2回目以降の実行で FileExistsError にしないため
notes_dir.mkdir(exist_ok=True)

(notes_dir / "monday.txt").write_text("会議は10時から\n資料を印刷する\n", encoding="utf-8")
(notes_dir / "tuesday.txt").write_text("見積もりを送る\n", encoding="utf-8")
(notes_dir / "wednesday.txt").write_text("在庫を数える\n発注する\n", encoding="utf-8")
(notes_dir / "settings.json").write_text("{}\n", encoding="utf-8")

print("作成しました")
```

`python-lesson/list_notes.py`

```python
from pathlib import Path

notes_dir = Path("notes")

if not notes_dir.exists():
    print("notes ディレクトリがありません")
else:
    # sorted で名前順に固定する（glob の順番は決まっていないため）
    txt_paths = sorted(notes_dir.glob("*.txt"))
    for path in txt_paths:
        first_line = path.read_text(encoding="utf-8").splitlines()[0]
        print(f"{path.name}: {first_line}")
    print(f"テキストファイルは{len(txt_paths)}個です")
```

```text
python make_notes.py の実行結果:
作成しました

python list_notes.py の実行結果:
monday.txt: 会議は10時から
tuesday.txt: 見積もりを送る
wednesday.txt: 在庫を数える
テキストファイルは3個です
```

**解説**

**`(notes_dir / "monday.txt")` のかっこ**

かっこがないと `notes_dir / "monday.txt".write_text(...)` と解釈され、
**文字列に対して `write_text` を呼ぼうとして** `AttributeError` になります。
`/` でつないだ結果に対してメソッドを呼ぶときは、かっこで囲んでください
（[7.3.2](./07-files-and-exceptions.md#732-パスを組み立てる)）。

**`exist_ok=True`**

これがないと、2回目の実行で次のエラーになります
（[7.3.3](./07-files-and-exceptions.md#733-存在確認作成一覧)）。

```text
FileExistsError: [Errno 17] File exists: 'notes'
```

**`glob("*.txt")` と `sorted`**

`iterdir()` を使うと `settings.json` まで一覧に入ってしまい、
「3個です」の条件を満たせません。`glob("*.txt")` で絞り込みます。

そして `glob` の**取り出し順は決まっていません。**
環境によって順番が変わるので、`sorted()` で囲んで名前順に固定します。

**`len()` を使うために変数に入れる**

`for path in sorted(notes_dir.glob("*.txt")):` と直接書いてしまうと、
あとで件数を数えられません。**いったん `txt_paths` に入れる**のがポイントです。

> **よくある間違い**
> `path.read_text(...).splitlines()[0]` は、**中身が空のファイルだと `IndexError`** になります。
>
> ```text
> IndexError: list index out of range
> ```
>
> この演習では中身が必ずあると分かっているのでこのままで問題ありませんが、
> 実際のデータを扱うときは、空ファイルが混ざる可能性を考えてください。
> 第7章の道具だけでも、次のように書けます。
>
> ```python
> lines = path.read_text(encoding="utf-8").splitlines()
> first_line = lines[0] if lines else "（空です）"
> ```

---

### 演習 7.3 の解答

`python-lesson/item_summary.py`

```python
import csv
import json
from collections import defaultdict
from pathlib import Path


def read_sales(path):
    """売上 CSV を読み込んで、辞書のリストで返す。"""
    with open(path, encoding="utf-8", newline="") as f:
        return list(csv.DictReader(f))


def summarize(sales):
    """商品ごとの個数と金額を集計し、金額の多い順のリストで返す。"""
    # 合計したい値が2種類あるので、defaultdict も2つ用意する
    quantities = defaultdict(int)
    amounts = defaultdict(int)
    for row in sales:
        quantity = int(row["quantity"])
        price = int(row["price"])
        quantities[row["item"]] += quantity
        amounts[row["item"]] += quantity * price

    summary = []
    for item in quantities:
        summary.append({
            "item": item,
            "quantity": quantities[item],
            "amount": amounts[item],
        })
    return sorted(summary, key=lambda row: row["amount"], reverse=True)


def main():
    sales = read_sales(Path("data") / "sales.csv")
    summary = summarize(sales)

    for i, row in enumerate(summary, start=1):
        print(f"{i}. {row['item']}: {row['quantity']}個 / {row['amount']}円")

    out_path = Path("data") / "item_summary.json"
    with open(out_path, "w", encoding="utf-8") as f:
        json.dump(summary, f, ensure_ascii=False, indent=2)

    print(f"{out_path} に書き出しました")


if __name__ == "__main__":
    main()
```

```text
実行結果:
1. ボールペン: 14個 / 1680円
2. ノート: 6個 / 1080円
3. 消しゴム: 5個 / 450円
data/item_summary.json に書き出しました
```

`data/item_summary.json`

```json
[
  {
    "item": "ボールペン",
    "quantity": 14,
    "amount": 1680
  },
  {
    "item": "ノート",
    "quantity": 6,
    "amount": 1080
  },
  {
    "item": "消しゴム",
    "quantity": 5,
    "amount": 450
  }
]
```

**解説**

[7.4.3](./07-files-and-exceptions.md#743-日本語を含む-json-の書き出し) の `json_records.py` と、
**まったく同じ5段階**で書けます。題材が図書室の貸出記録から売上に変わっただけです。

| 段階 | やること | 本文の対応 |
|------|---------|-----------|
| 1 | CSV を辞書のリストにする | [7.4.1](./07-files-and-exceptions.md#741-csv-モジュール) の `DictReader` |
| 2 | `defaultdict` を2つ用意して集計する | [7.4.3](./07-files-and-exceptions.md#743-日本語を含む-json-の書き出し) の段階1 |
| 3 | 辞書のリストに組み立て直す | 同 段階2 |
| 4 | `key` を指定して並べ替える | [5.5.3](./05-functions.md#553-sorted-の-key-に渡す) |
| 5 | `ensure_ascii=False, indent=2` で書き出す | [7.4.3](./07-files-and-exceptions.md#743-日本語を含む-json-の書き出し) |

**なぜ `defaultdict` を2つ使うのか**

「個数の合計」と「金額の合計」は**別々の集計**です。
1つの辞書に両方入れようとすると、
`totals[item] = {"quantity": ..., "amount": ...}` のような入れ子になり、
`defaultdict(int)` では扱えなくなります。
**集計したい値の種類だけ `defaultdict` を並べる**のが、いちばん素直な書き方です。

**なぜ辞書ではなくリストで返すのか**

辞書のままだと、`sorted` で並べ替えた結果を保持しにくくなります。
**辞書のリスト**（[4.3.5](./04-data-structures.md#435-辞書のリスト実務で最頻出の形)）にすれば、
`key=lambda row: row["amount"]` で好きな列を基準に並べられ、
`json.dump` にもそのまま渡せます。
JSON の世界でも、この形（オブジェクトの配列）が最も一般的です。

**`newline=""` を忘れると**

読み込みだけなら、多くの場合は動いてしまいます。
しかし値の中に改行を含む CSV では行がずれるため、
**CSV を開くときは常に付ける**と決めておくのが安全です
（[7.4.1](./07-files-and-exceptions.md#741-csv-モジュール)）。

> **よくある間違い**
> `int()` を忘れると、次のように**エラーが出ないまま**おかしな結果になります。
>
> ```python
> amounts[row["item"]] += row["quantity"] * row["price"]
> ```
>
> ```text
> TypeError: can't multiply sequence by non-int of type 'str'
> ```
>
> この場合は運よく `TypeError` になりますが（文字列どうしは掛けられないため）、
> `row["quantity"] * 3` のように**片方が数値**だと、
> 文字列の繰り返しとして成立してしまい、エラーが出ません。
> **CSV から取り出したら、まず `int()`。** これを習慣にしてください。

> **別解：`summarize` の中で内包表記を使う**
>
> ```python
> summary = [
>     {"item": item, "quantity": quantities[item], "amount": amounts[item]}
>     for item in quantities
> ]
> ```
>
> リスト内包表記（[4.5.1](./04-data-structures.md#451-リスト内包表記)）で書けますが、
> 1行が長くなるので、**読みにくいと感じたら `for` のままでかまいません**
> （[4.5.4](./04-data-structures.md#454-読みにくくなったら-for-に戻す)）。

---

### 演習 7.4 の解答

`python-lesson/load_settings.py`

```python
import json
from pathlib import Path


class SettingsError(Exception):
    """設定ファイルの内容が正しくないことを表す例外。"""


def load_settings(path):
    """設定ファイルを読み込んで辞書で返す。内容が不正なら SettingsError を投げる。"""
    text = Path(path).read_text(encoding="utf-8")
    settings = json.loads(text)

    for key in ["shop_name", "tax_rate"]:
        if key not in settings:
            raise SettingsError(f"{key} がありません")

    if settings["tax_rate"] < 0 or settings["tax_rate"] > 1:
        raise SettingsError(f"tax_rate は0以上1以下にしてください: {settings['tax_rate']}")

    return settings


def main():
    paths = [
        "settings/shop_ok.json",
        "settings/shop_missing.json",
        "settings/shop_bad_rate.json",
        "settings/shop_none.json",
    ]
    for path in paths:
        try:
            settings = load_settings(path)
        except FileNotFoundError:
            print(f"{path}: ファイルが見つかりません")
        except SettingsError as e:
            print(f"{path}: 設定を直してください（{e}）")
        else:
            print(f"{path}: {settings['shop_name']} / 税率 {settings['tax_rate']}")
        finally:
            print("  --- 1件終わり ---")


if __name__ == "__main__":
    main()
```

```text
実行結果:
settings/shop_ok.json: みどり文具店 / 税率 0.1
  --- 1件終わり ---
settings/shop_missing.json: 設定を直してください（shop_name がありません）
  --- 1件終わり ---
settings/shop_bad_rate.json: 設定を直してください（tax_rate は0以上1以下にしてください: 1.5）
  --- 1件終わり ---
settings/shop_none.json: ファイルが見つかりません
  --- 1件終わり ---
```

**解説**

**なぜ `load_settings` の中で `try` を書かないのか**

`load_settings` は「設定を読んで返す係」です。
ファイルが見つからなかったときに、
**既定値を使うのか、利用者に知らせるのか、プログラムを止めるのかを決める材料を持っていません。**
決められるのは `main` だけです（[7.6.3](./07-files-and-exceptions.md#763-どこで例外を捕まえるべきか)）。

`Path(path).read_text(...)` が投げた `FileNotFoundError` は、
`load_settings` を素通りして `main` の `except` まで戻ってきます。
**途中の関数に `try` を書かなくても、例外はちゃんと届きます。**

**なぜ自作の例外にするのか**

`ValueError` を投げてしまうと、`main` 側の `except ValueError:` が
「設定が不正だった」のか「JSON の中の数値変換に失敗した」のかを区別できません。
`SettingsError` という専用の種類を作ることで、
**この処理が意図的に投げたものだけ**を狙って捕まえられます
（[7.6.2](./07-files-and-exceptions.md#762-自作の例外クラス)）。

クラスの本体は docstring 1行だけで、処理は書きません。
`(Exception)` の意味（継承）は第8章で扱います。

**`except` を2つ並べる順番**

`FileNotFoundError` と `SettingsError` は、
どちらも `Exception` の下にありますが**親子関係ではない**ので、順番はどちらが先でもかまいません。
ただし `except Exception:` を先に書くと両方ともそこに吸い込まれます
（[7.5.3](./07-files-and-exceptions.md#753-例外の種類を指定する)）。

**`else` と `finally` の使い分け**

- 正常なときだけ表示したい → **`else`**
- どの場合でも表示したい → **`finally`**

`try` のブロックには `load_settings(path)` の**1行だけ**を入れています。
表示の `print` を `try` の中に入れてしまうと、
`settings['shop_name']` の `KeyError` まで巻き込みかねません
（[7.5.4](./07-files-and-exceptions.md#754-else-と-finally)）。

> **よくある間違い**
> `tax_rate` の範囲チェックを、キーの存在チェックより**前**に書くと、
> `shop_missing.json` で `KeyError` になります。
>
> ```text
> KeyError: 'tax_rate'
> ```
>
> **「あるかどうか」を先に、「値が正しいか」をあとに。** この順番は変えられません。
> 第5章のガード節（[5.3.3](./05-functions.md#533-早期-return)）と同じで、
> **前提が崩れる条件から順に弾いていく**のが基本です。

> **別解：`get` を使って1つの `if` にまとめる**
>
> ```python
> rate = settings.get("tax_rate")
> if rate is None or rate < 0 or rate > 1:
>     raise SettingsError(f"tax_rate が正しくありません: {rate}")
> ```
>
> `get`（[4.3.3](./04-data-structures.md#433-get-で安全に取り出す)）を使えば `KeyError` を避けられます。
> ただし「ない」と「範囲外」でメッセージを分けられなくなるため、
> **利用者に何を直せばよいか伝えたい場面では、解答例のように分けたほうが親切です。**
