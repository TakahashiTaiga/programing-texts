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

---

## 第8章

### 理解度チェック

**問 8.1 の解答**

- A = **`__init__`**
- B = **いま作られたインスタンスそのもの**（`Product("ノート", 180)` の結果として返ってくるもの）

**解説**

`Product("ノート", 180)` と書いたときに起きることを、順番に並べると次のようになります。

1. Python が空のインスタンスを1つ作る
2. そのインスタンスを第1引数にして `__init__` を呼ぶ（`self` がそれを受け取る）
3. `__init__` の中で `self.name = name` などが実行され、属性が付く
4. できあがったインスタンスが返り、`note` に代入される

**`__init__` を自分で呼び出す必要はありません。**
書いておけば、インスタンスを作るたびに自動で呼ばれます
（[8.2.1](./08-oop.md#821-__init__-で初期化する)）。

「`self` に何が入るのか」が曖昧なら、
[8.2.2](./08-oop.md#822-self-とは何か) の
`Product.price_with_tax(note)` と書いた例をもう一度見てください。
**ドットの左側にあるものが `self` に入る**、と覚えると混乱しません。

---

**問 8.2 の解答**

```text
2 0 0
```

**理由**：`self.count += 1` は「読む」と「書く」を同時にしている。
読むときはクラス変数の `0` が使われるが、**書くときはインスタンス変数が新しく作られる**ため、
`a` にだけ `count` が生え、クラス変数と `b` は `0` のまま。

**解説**

この問題は、[8.2.4](./08-oop.md#824-クラス変数とインスタンス変数) の
**属性を探す順番**を理解しているかを確かめるものです。

`self.count += 1` は、次の2行に分けて書いたのと同じ意味です。

```python
self.count = self.count + 1
```

右辺と左辺で、起きていることが違います。

| | 何が起きるか |
|--|------------|
| 右辺の `self.count` | インスタンスに `count` がないので、**クラス変数の `0`** が読まれる |
| 左辺の `self.count =` | **インスタンス変数 `count` が新しく作られて**、`1` が入る |

1回目の `a.add()` で `a` に `count = 1` が生えます。
2回目の `a.add()` では、もう `a` に `count` があるので、
今度はインスタンス変数の `1` が読まれて `2` になります。

`b` は一度も `add()` を呼んでいないので、インスタンス変数を持ちません。
`b.count` はクラス変数を読みに行き、`0` が返ります。
`Counter.count` も、誰も書き換えていないので `0` のままです。

> **よくある間違い**
> 「全インスタンスで数を共有したい」つもりでこのコードを書くと、期待どおりに動きません。
> 共有したいなら、クラス変数を**クラス名で**書き換えます。
>
> ```python
> def add(self):
>     Counter.count += 1
> ```
>
> ただし、この形が本当に必要になる場面はまれです。
> **クラス変数は書き換えない値のために使う**、という原則
> （[8.2.4](./08-oop.md#824-クラス変数とインスタンス変数)）を守るほうが安全です。

---

**問 8.3 の解答**

`price_with_tax` の**第1引数に `self` を書く**。

```python
    def price_with_tax(self):
        return round(self.price * 1.1)
```

**解説**

出るエラーはこれです。

```text
TypeError: Product.price_with_tax() takes 0 positional arguments but 1 was given
```

「引数を0個しか受け取らないのに、1個渡された」と言われています。
呼び出し側は `note.price_with_tax()` と、**何も渡していないように見える**のが厄介なところです。

渡している1個は `note` そのものです。
`note.price_with_tax()` は、Python の中では
`Product.price_with_tax(note)` として実行されるからです
（[8.2.2](./08-oop.md#822-self-とは何か)）。

**引数を書いていないのにこのメッセージが出たら、`self` の書き忘れ。**
これはそのまま覚えてしまってかまいません。

なお、`self` を書き忘れたまま `self.price` を読もうとしても
`NameError` にはなりません。**その手前の `TypeError` で止まる**ためです。

---

**問 8.4 の解答**

```text
#文具
[<__main__.Tag object at 0x000001F3A2B4C7D0>, <__main__.Tag object at 0x000001F3A2B4C910>]
```

（`0x...` の部分は実行のたびに変わります）

**解説**

1行目は `print(tags[0])` なので、`Tag` のインスタンスをそのまま `print` しています。
`__str__` が定義してあるので `#文具` になります。

2行目は**リストを `print`** しています。
このとき、リストの中身の表示に使われるのは
**`__repr__` のほう**です（[8.4.1](./08-oop.md#841-__str__-と-__repr__)）。
`Tag` には `__repr__` がないので、既定の `<__main__.Tag object at ...>` が出ます。

`__str__` を書いたのに効かない、と悩むのはこの場面です。

**直し方**：`__repr__` も定義します。どちらか1つだけ書くなら `__repr__` にしてください。
`__str__` がないときは `__repr__` が代わりに使われるため、`print(tags[0])` も動きます。

```python
    def __repr__(self):
        return f"Tag(name='{self.name}')"
```

```text
#文具
[Tag(name='文具'), Tag(name='食品')]
```

---

**問 8.5 の解答**

```text
商品です
食品です
```

**解説**

`Food` には `show` メソッドがないので、親の `Base` の `show` が使われます
（[8.3.2](./08-oop.md#832-親クラスを継承する)）。

ポイントは、その `show` の中の `self.LABEL` が**どこを見るか**です。
`Food().show()` のとき、`self` は `Food` のインスタンスなので、
[8.2.4](./08-oop.md#824-クラス変数とインスタンス変数) の順番どおりに探されます。

1. インスタンスに `LABEL` はない
2. **そのインスタンスのクラス（`Food`）に `LABEL` がある** → `"食品"` を使う

**メソッドは親のものを使いながら、値だけ子のものが使われた**わけです。

これが、[8.3.2](./08-oop.md#832-親クラスを継承する) の `FoodProduct` が
`TAX_RATE = 0.08` の1行だけで税率を変えられた理由です。

> **補足**
> `Base().show()` のように、**変数に入れずにその場でインスタンスを作って**
> メソッドを呼ぶ書き方もできます。
> 1回しか使わないインスタンスなら、この書き方で問題ありません。

---

**問 8.6 の解答**

**理由**：`dataclass` では、リストのように**中身を変えられる値**を
デフォルト値に直接書けないため。

```text
ValueError: mutable default <class 'list'> for field tags is not allowed: use default_factory
```

**正しい書き方**：

```python
from dataclasses import dataclass, field


@dataclass
class Member:
    name: str
    tags: list = field(default_factory=list)
```

**解説**

もし `tags: list = []` が許されると、
**1回だけ作られたリストが全インスタンスで共有される**ことになります。
1人の会員にタグを付けると、全員に付いてしまう状態です。

これはこのテキストで3回目に出てくる、同じ形の落とし穴です。

| どこで出たか | 形 |
|------------|----|
| [5.2.4](./05-functions.md#524-デフォルト引数にリストを使ってはいけない) | `def f(items=[]):` |
| [8.2.4](./08-oop.md#824-クラス変数とインスタンス変数) | クラス変数の `tags = []` |
| ここ | `dataclass` の `tags: list = []` |

原因はすべて同じで、**リストが1回しか作られないこと**です。

`dataclass` だけは、実行した瞬間にエラーで止めてくれます。
`field(default_factory=list)` は
「インスタンスを作るたびに `list()` を呼んで、**新しい空のリストを作れ**」という指定です
（[8.5.3](./08-oop.md#853-デフォルト値と型)）。

> **よくある間違い**
> `field(default_factory=list())` と、**`list` にかっこを付けてしまう**間違いがあります。
> ここで渡すのは「呼び出した結果」ではなく**関数そのもの**です
> （[5.5.1](./05-functions.md#551-関数を変数に入れる)）。かっこは付けません。

---

**問 8.7 の解答**

**クラスにする必要はありません。** 関数で十分です。

```python
def to_yen(amount):
    """金額を「1,234円」の形の文字列にして返す。"""
    return f"{amount:,}円"
```

**理由**：`Formatter` には属性が1つもなく、`to_yen` の中に `self.` が出てこない。
つまりインスタンスは何も覚えておらず、作る意味がない。

**解説**

判断の基準は [8.6.1](./08-oop.md#861-関数で足りるならクラスは不要) のこれです。

> **メソッドの中に `self.` が1つも出てこないなら、それは関数でよい。**

クラスのままだと、使う側は毎回こう書くことになります。

```python
formatter = Formatter()
print(formatter.to_yen(1234))
```

関数なら1行です。

```python
print(to_yen(1234))
```

```text
実行結果:
1,234円
```

**インスタンスを作る行が、まるごと不要になっています。**

`f"{amount:,}"` の `:,` は、3桁ごとにカンマを入れる書式指定です
（[2.4.4](./02-basics.md#244-f-string)）。

> **補足**
> 「関連する関数をまとめたいからクラスにする」という動機もありますが、
> Python にはそのための仕組みが別にあります。**モジュール**（第6章）です。
> `formatters.py` というファイルに関数を並べ、
> `from formatters import to_yen` で使うほうが Python らしい書き方です。

---

### 演習 8.1 の解答

`python-lesson/member.py`

```python
class Member:
    """会員を表すクラス。"""

    SHOP_NAME = "みどり文具店"
    POINT_RATE = 100

    def __init__(self, member_id, name):
        """会員番号と名前を受け取り、ポイントを0で初期化する。"""
        self.member_id = member_id
        self.name = name
        self.points = 0

    def earn(self, amount):
        """購入金額 amount 円に応じてポイントを加える。"""
        self.points += amount // self.POINT_RATE

    def use(self, points):
        """ポイントを使う。足りなければ ValueError を投げる。"""
        if points > self.points:
            raise ValueError(f"{self.name}さんのポイントが足りません（残り{self.points}ポイント）")
        self.points -= points

    def rank(self):
        """現在のポイントからランク名を返す。"""
        if self.points >= 1000:
            return "ゴールド"
        if self.points >= 500:
            return "シルバー"
        return "ブロンズ"

    def label(self):
        """一覧表示用の1行を返す。"""
        return f"[{self.SHOP_NAME}] {self.member_id} {self.name}さん: {self.points}ポイント（{self.rank()}）"


def main():
    """会員を2人作り、ポイントを増減して一覧表示する。"""
    sato = Member("M001", "佐藤")
    suzuki = Member("M002", "鈴木")

    sato.earn(62000)
    suzuki.earn(153000)
    suzuki.use(200)

    for member in [sato, suzuki]:
        print(member.label())

    try:
        sato.use(1000)
    except ValueError as e:
        print(f"使えませんでした: {e}")


if __name__ == "__main__":
    main()
```

```text
実行結果:
[みどり文具店] M001 佐藤さん: 620ポイント（シルバー）
[みどり文具店] M002 鈴木さん: 1330ポイント（ゴールド）
使えませんでした: 佐藤さんのポイントが足りません（残り620ポイント）
```

**解説**

**1. `points` は引数で受け取らない**

`__init__` の引数は `member_id` と `name` の2つだけで、
`self.points = 0` は**中で決め打ち**しています。

「作った直後は必ず0ポイント」というルールを、`__init__` に閉じ込めた形です
（[8.2.3](./08-oop.md#823-メソッドを定義する) の `self.stock = 0` と同じ考え方）。
外から `Member("M001", "佐藤", 99999)` と好きなポイントを渡せてしまうと、
このルールが守られません。

**2. `//` を使う**

`62000 // 100` は `620` です。`/` を使うと `620.0` という**小数**になり
（[2.3.2](./02-basics.md#232--と--と-)）、表示が `620.0ポイント` になってしまいます。
ポイントのように「個数」を数えるものには `//` を使ってください。

**3. `self.POINT_RATE` と書く**

`100` を直接書かず、クラス変数にしています。
こうしておくと、演習 8.3 で**この1行を書き換えるだけ**でポイント2倍の会員が作れます
（[8.2.4](./08-oop.md#824-クラス変数とインスタンス変数)）。

**4. `use` は自分で対処しない**

ポイントが足りないとき、`use` は `print` もしなければ `return` もせず、
`ValueError` を投げるだけです。

「足りなかったらどうするか」は、呼び出す側でないと決められません
（[7.6.3](./07-files-and-exceptions.md#763-どこで例外を捕まえるべきか)）。
画面にメッセージを出すのか、別の支払い方法に切り替えるのかは、`use` の知るところではないからです。

**5. `rank` は `elif` を使わなくてよい**

`return` を実行した時点で関数は終わるので
（[5.1.3](./05-functions.md#513-return-がないとどうなるか)）、
`if` を並べるだけで `elif` と同じ動きになります。
もちろん `elif` で書いてもかまいません。

> **よくある間違い**
> `earn` の中で `self.points = amount // self.POINT_RATE` と、
> **`+=` ではなく `=` を書いてしまう**間違いが多いです。
> これだと2回目の買い物でポイントが上書きされ、たまっていきません。
> 「増やす」なのか「置き換える」なのかを、毎回はっきりさせてください。

> **よくある間違い**
> `label()` の中で `Member.SHOP_NAME` と書いてもこの演習では動きますが、
> **`self.SHOP_NAME` と書くほうを勧めます。**
> 演習 8.3 で継承したとき、`self.` なら子クラスで上書きした値が使われるからです
> （[8.3.2](./08-oop.md#832-親クラスを継承する)）。

---

### 演習 8.2 の解答

`python-lesson/member.py`（演習 8.1 から `label` を `__str__` に置き換え、2つ追加）

```python
class Member:
    """会員を表すクラス。"""

    SHOP_NAME = "みどり文具店"
    POINT_RATE = 100

    def __init__(self, member_id, name):
        """会員番号と名前を受け取り、ポイントを0で初期化する。"""
        self.member_id = member_id
        self.name = name
        self.points = 0

    def earn(self, amount):
        """購入金額 amount 円に応じてポイントを加える。"""
        self.points += amount // self.POINT_RATE

    def use(self, points):
        """ポイントを使う。足りなければ ValueError を投げる。"""
        if points > self.points:
            raise ValueError(f"{self.name}さんのポイントが足りません（残り{self.points}ポイント）")
        self.points -= points

    def rank(self):
        """現在のポイントからランク名を返す。"""
        if self.points >= 1000:
            return "ゴールド"
        if self.points >= 500:
            return "シルバー"
        return "ブロンズ"

    def __str__(self):
        """利用者向けの1行を返す。"""
        return f"[{self.SHOP_NAME}] {self.member_id} {self.name}さん: {self.points}ポイント（{self.rank()}）"

    def __repr__(self):
        """開発者向けの表示を返す。"""
        return f"Member(member_id='{self.member_id}', name='{self.name}', points={self.points})"

    def __eq__(self, other):
        """会員番号が同じなら、同じ会員とみなす。"""
        if not isinstance(other, Member):
            return False
        return self.member_id == other.member_id


def main():
    """会員の表示と、同一判定を確認する。"""
    sato = Member("M001", "佐藤")
    suzuki = Member("M002", "鈴木")
    sato.earn(62000)
    suzuki.earn(153000)

    members = [sato, suzuki]
    for member in members:
        print(member)

    print(members)

    sato_again = Member("M001", "佐藤")
    print(sato == sato_again)
    print(sato == suzuki)
    print(sato_again in members)
    print(sato == "M001")


if __name__ == "__main__":
    main()
```

```text
実行結果:
[みどり文具店] M001 佐藤さん: 620ポイント（シルバー）
[みどり文具店] M002 鈴木さん: 1530ポイント（ゴールド）
[Member(member_id='M001', name='佐藤', points=620), Member(member_id='M002', name='鈴木', points=1530)]
True
False
True
False
```

**解説**

**1. `label()` を `__str__` にすると、呼び出し側が短くなる**

```python
print(member.label())   # 前
print(member)           # あと
```

やっていることは同じですが、**`Member` を使う側が `label` という名前を覚えなくてよくなりました。**
`print` すればいい、というのは Python を使う人なら誰でも知っています。
特殊メソッドの価値は、**自作クラスを Python の標準の書き方になじませる**ところにあります。

**2. リストの表示に使われたのは `__repr__`**

3行目の `print(members)` の結果を見てください。
`[みどり文具店] ...` ではなく `Member(member_id=...)` のほうが並んでいます。

**リストの中身の表示には `__repr__` が使われる**からです
（[8.4.1](./08-oop.md#841-__str__-と-__repr__)）。
`__repr__` は開発者が中身を確認するためのものなので、
**属性の値がそのまま読める形**にしてあります。

**3. `__eq__` の判定は「会員番号だけ」でよい**

会員は、名前が同じでも別人のことがあり、
ポイントは買い物のたびに変わります。
**変わらず、その人を一意に決めるもの**は会員番号だけです。

```python
return self.member_id == other.member_id
```

一方、[8.4.2](./08-oop.md#842-__eq__-で比較する) の `Product` では
`name` と `price` の両方を比べていました。
**何が同じなら「同じもの」なのかは、扱う対象ごとに違います。**
`__eq__` は、その判断を自分で書き込む場所です。

**4. `isinstance` のガードを忘れない**

```python
if not isinstance(other, Member):
    return False
```

これがないと、`sato == "M001"` のときに
`"M001".member_id` を読もうとして `AttributeError` になります。
比較で例外が飛ぶのは想定外の動きなので、**必ず先に弾いてください**
（[5.3.3](./05-functions.md#533-早期-return) のガード節）。

> **よくある間違い**
> `__str__` や `__repr__` の中で `print` してしまう間違いがあります。
>
> ```python
> def __str__(self):
>     print(f"...")      # ← return ではない
> ```
>
> ```text
> TypeError: __str__ returned non-string (type NoneType)
> ```
>
> `print` は画面に出すだけで、戻り値は `None` です
> （[5.1.3](./05-functions.md#513-return-がないとどうなるか)）。
> **特殊メソッドは、必ず文字列を `return` してください。**

> **補足**
> `__eq__` を定義したので、`Member` は集合（set）に入れられなくなりました
> （[8.4.2](./08-oop.md#842-__eq__-で比較する) の補足）。
>
> ```text
> TypeError: unhashable type: 'Member'
> ```
>
> この演習では使っていないので問題ありませんが、
> 会員を `set` で重複除去したくなったときは、演習 8.4 のように
> `@dataclass` にすると解決します。

---

### 演習 8.3 の解答

`python-lesson/premium_member.py`

```python
from member import Member


class PremiumMember(Member):
    """有料会員を表すクラス。ポイントが2倍たまる。"""

    POINT_RATE = 50

    def __init__(self, member_id, name, expires_on):
        """親の初期化に加えて、会員資格の有効期限を保存する。"""
        super().__init__(member_id, name)
        self.expires_on = expires_on

    def rank(self):
        """親のランク名の先頭に「プレミアム」を付けて返す。"""
        return f"プレミアム{super().rank()}"

    def __str__(self):
        """親の1行に、有効期限を足して返す。"""
        return f"{super().__str__()} / 有効期限 {self.expires_on}"


def main():
    """一般会員と有料会員を並べて表示する。"""
    sato = Member("M001", "佐藤")
    tanaka = PremiumMember("P001", "田中", "2027-03-31")

    for member in [sato, tanaka]:
        member.earn(62000)

    for member in [sato, tanaka]:
        print(member)

    print(isinstance(tanaka, PremiumMember), isinstance(tanaka, Member))


if __name__ == "__main__":
    main()
```

```text
実行結果:
[みどり文具店] M001 佐藤さん: 620ポイント（シルバー）
[みどり文具店] P001 田中さん: 1240ポイント（プレミアムゴールド） / 有効期限 2027-03-31
True True
```

**解説**

**1. `earn` を1文字も書いていないのに、ポイントが2倍になっている**

`PremiumMember` に書いたのは `POINT_RATE = 50` の1行だけです。
`earn` は親から引き継いだものがそのまま動いています。

なぜ変わったかというと、親の `earn` が
`amount // self.POINT_RATE` と、**`self.` 経由で読んでいる**からです。
`self` が `PremiumMember` のインスタンスなら、
探す順番（[8.2.4](./08-oop.md#824-クラス変数とインスタンス変数)）にしたがって
`PremiumMember.POINT_RATE` が先に見つかります。

`62000 // 50` は `1240`。一般会員の2倍になりました。

**2. `super()` を3回使っている**

| 場所 | 何のため |
|------|---------|
| `super().__init__(member_id, name)` | 親に `member_id` / `name` / `points` の設定を任せる |
| `super().rank()` | ランクの**判定条件**を書き直さずに、結果だけもらう |
| `super().__str__()` | 表示の**組み立て方**を書き直さずに、結果だけもらう |

`rank` を見てください。「1000以上ならゴールド」という条件は、子クラスに1つも書いていません。
もし条件を書き写していたら、**判定を変えたいときに2か所直す**ことになります
（[8.3.1](./08-oop.md#831-共通部分をまとめる) で避けたかった状態そのものです）。

`__str__` も同じで、`super().__str__()` と**メソッド名をそのまま書く**だけです
（[8.3.4](./08-oop.md#834-super) の補足）。

**3. 2種類のインスタンスが、同じ `for` で回せる**

```python
for member in [sato, tanaka]:
    member.earn(62000)
```

`sato` は `Member`、`tanaka` は `PremiumMember` です。
それでも同じループで扱えて、**それぞれが自分にふさわしい動きをします**
（[8.3.3](./08-oop.md#833-メソッドを上書きする)）。

`if isinstance(member, PremiumMember):` のような分岐は要りません。
これが継承を使う目的です。

**4. `isinstance` が両方 `True` になる**

`tanaka` は `PremiumMember` であり、同時に `Member` でもあります。
「有料会員は会員の一種である」——
[8.3.5](./08-oop.md#835-継承を使いすぎない) の
「〜は〜の一種である」がきれいに成り立つので、継承してよい場面でした。

> **よくある間違い**
> `super().__init__(member_id, name)` を忘れると、次のようになります。
>
> ```text
> AttributeError: 'PremiumMember' object has no attribute 'points'
> ```
>
> しかも、このエラーが出るのは**インスタンスを作った行ではなく、`earn()` を呼んだ行**です。
> **子クラスで `__init__` を書いたら、まず `super().__init__(...)` を書く。**
> 自分の分の代入は、そのあとに足してください（[8.3.4](./08-oop.md#834-super)）。

> **よくある間違い**
> `super().__init__(self, member_id, name)` と、**`self` を渡してしまう**間違いがあります。
>
> ```text
> TypeError: Member.__init__() takes 3 positional arguments but 4 were given
> ```
>
> `super()` を通すときは、`self` は自動的に渡されます。書いてはいけません。

> **別解：`rank` を上書きしない**
> 「プレミアム」を付けるのを `__str__` 側だけで行い、`rank` は触らない書き方もできます。
>
> ```python
> def __str__(self):
>     return f"{super().__str__()} / プレミアム / 有効期限 {self.expires_on}"
> ```
>
> どちらが良いかは、**「ランク名そのものが変わる」のか
> 「表示に情報を足したいだけ」なのか**で決まります。
> 会員証にランク名として印字するなら、解答例のように `rank` を上書きするほうが素直です。

---

### 演習 8.4 の解答

`python-lesson/data/members.json`

```json
[
  {"member_id": "M001", "name": "佐藤", "points": 620},
  {"member_id": "M002", "name": "鈴木", "points": 1530},
  {"member_id": "M003", "name": "高橋", "points": 180},
  {"member_id": "M004", "name": "伊藤", "points": 940}
]
```

`python-lesson/member_report.py`

```python
import json
from dataclasses import dataclass


@dataclass
class Member:
    """会員を表すクラス。"""

    member_id: str
    name: str
    points: int = 0

    def rank(self):
        """現在のポイントからランク名を返す。"""
        if self.points >= 1000:
            return "ゴールド"
        if self.points >= 500:
            return "シルバー"
        return "ブロンズ"


def load_members(path):
    """JSON ファイルを読み込んで、Member のリストを返す。"""
    with open(path, encoding="utf-8") as f:
        rows = json.load(f)
    return [Member(row["member_id"], row["name"], row["points"]) for row in rows]


def to_rows(members):
    """Member のリストを、書き出し用の辞書のリストに変換して返す。"""
    return [
        {"member_id": m.member_id, "name": m.name, "points": m.points, "rank": m.rank()}
        for m in members
    ]


def main():
    """会員をポイントの多い順に表示し、結果を JSON に書き出す。"""
    members = load_members("data/members.json")
    members = sorted(members, key=lambda m: m.points, reverse=True)

    for i, member in enumerate(members, start=1):
        print(f"{i}. {member.name}: {member.points}ポイント（{member.rank()}）")

    with open("data/member_report.json", "w", encoding="utf-8") as f:
        json.dump(to_rows(members), f, ensure_ascii=False, indent=2)
    print("data/member_report.json に書き出しました")


if __name__ == "__main__":
    main()
```

```text
実行結果:
1. 鈴木: 1530ポイント（ゴールド）
2. 伊藤: 940ポイント（シルバー）
3. 佐藤: 620ポイント（シルバー）
4. 高橋: 180ポイント（ブロンズ）
data/member_report.json に書き出しました
```

`python-lesson/data/member_report.json`（先頭部分）

```json
[
  {
    "member_id": "M002",
    "name": "鈴木",
    "points": 1530,
    "rank": "ゴールド"
  },
```

**解説**

**1. `@dataclass` で消えた行**

演習 8.2 の `Member` と見比べてください。
`__init__` の5行と `__repr__` の2行が、**まるごと消えています。**
それでいて `print(members)` すれば中身が読める形で表示されます
（[8.5.2](./08-oop.md#852-dataclass-の使い方)）。

一方、`rank()` は自分で書いています。
`@dataclass` が用意してくれるのは**定型のメソッドだけ**で、
そのクラス独自の処理には手を出しません。

**2. `points: int = 0` は最後に置く**

既定値のある項目は、**必ずうしろにまとめます**（[8.5.3](./08-oop.md#853-デフォルト値と型)）。
`member_id` より前に書くと、こうなります。

```text
TypeError: non-default argument 'member_id' follows default argument
```

[5.2.3](./05-functions.md#523-デフォルト引数) の「省略できるものは後ろ」と同じルールです。

**3. 読み込みと書き出しは、逆向きの変換**

| 関数 | 変換の向き |
|------|-----------|
| `load_members` | 辞書のリスト → `Member` のリスト |
| `to_rows` | `Member` のリスト → 辞書のリスト |

`to_rows` が必要なのは、`json.dump` が**自作クラスを書き出せない**からです
（[8.5.3](./08-oop.md#853-デフォルト値と型)）。

```text
TypeError: Object of type Member is not JSON serializable
```

そして `to_rows` では、元のデータになかった `rank` を足しています。
**読み込む形と書き出す形は、同じである必要がありません。**

**4. `key` にメソッドではなく属性を渡している**

```python
sorted(members, key=lambda m: m.points, reverse=True)
```

[8.5.3](./08-oop.md#853-デフォルト値と型) の例では
`key=lambda p: p.stock_value()` とメソッドを呼んでいましたが、
今回は `points` という属性をそのまま使うので `()` は付けません。
**計算が要るならメソッド、そのままの値なら属性**、と使い分けてください。

> **よくある間違い**
> `json.dump(members, f, ...)` と、**`Member` のリストをそのまま渡してしまう**間違いが多いです。
>
> ```text
> TypeError: Object of type Member is not JSON serializable
> ```
>
> 「シリアライズできない」は「**この型は JSON の形に直せない**」という意味です。
> `to_rows` のように、**自分で辞書に変換してから**渡してください。

> **別解：`dataclasses.asdict` を使う**
>
> ```python
> from dataclasses import asdict
>
> rows = [asdict(m) for m in members]
> ```
>
> `asdict` は `dataclass` のインスタンスを辞書に変換する関数です。
> ただし、これで得られる辞書に `rank` は入りません（`rank` は項目ではなくメソッドだからです）。
> **計算結果も一緒に書き出したい**今回の課題では、解答例のように自分で組み立てるほうが確実です。

> **よくある間違い**
> `ensure_ascii=False` を書き忘れると、書き出した JSON がこうなります。
>
> ```json
> "name": "\u9234\u6728"
> ```
>
> 壊れているわけではなく、日本語が番号で書かれているだけですが、
> 人が開いて確認できません。
> **`ensure_ascii=False` と `encoding="utf-8"` はセットで指定してください**
> （[7.4.3](./07-files-and-exceptions.md#743-日本語を含む-json-の書き出し)）。
