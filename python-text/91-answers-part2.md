---
title: "解答編 その2（第6章〜第11章）"
---

# 解答編 その2（第6章〜第11章）

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

### 演習

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

### 演習

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

### 演習

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

---

## 第9章

### 理解度チェック

**問 9.1 の解答**

- A = **`:`**（コロン）
- B = **`->`**（ハイフンと大なり記号）
- C = **`None`**（つまり `-> None` と書く）

**解説**

3つの位置を並べて確認してください。

```python
def with_tax(price: int, rate: float = 0.1) -> int:
    """税込価格を四捨五入して返す。"""
    return round(price * (1 + rate))
```

- 引数は `名前: 型`（[9.1.2](./09-typing-and-tools.md#912-変数と引数に型を書く)）
- 戻り値は、かっこを閉じたあとに `-> 型`、その後ろに `:`（[9.1.3](./09-typing-and-tools.md#913-戻り値の型)）
- デフォルト値がある引数は、**型が先、`=` が後**

`-> None` は「何も返さない」という意味です。
省略しても動きますが、省略すると **mypy がその関数の中身を検査しません**。
`print` するだけの関数にも必ず書いてください。

---

**問 9.2 の解答**

```text
実行結果:
12
```

**解説**

`repeat(4)` は `4 * 3` を計算して `12` を返します。

型ヒント `word: str` は「文字列を渡すはず」という**注記でしかなく、
Python は実行時にこれを検査しません**
（[9.1.4](./09-typing-and-tools.md#914-型ヒントは実行時に強制されない)）。
`4 * 3` は整数どうしの掛け算として成立するので、エラーにもなりません。

戻り値の型も `-> str` と書いてありますが、返ってきたのは整数の `12` です。
**「型ヒントを書いた ＝ 守られる」ではない**ことを、ここで確実に覚えてください。

この間違いを見つけたいなら、mypy を実行します。

```text
error: Argument 1 to "repeat" has incompatible type "int"; expected "str"  [arg-type]
```

---

**問 9.3 の解答**

1. `dict[str, int]`
2. `list[dict[str, str]]`
3. `str | None`

**解説**

1. 辞書は**キーと値の2つ**を書きます（[9.2.1](./09-typing-and-tools.md#921-リスト辞書タプル)）。
   `dict[str: int]` のようにコロンで書くのは誤りです。カンマで区切ります。
2. `csv.DictReader` は1行を辞書にして返し、**値はすべて文字列**でした
   （[7.4.1](./07-files-and-exceptions.md#741-csv-モジュール)）。
   その行が複数集まるので、`list[dict[str, str]]` になります。
   `list[dict[str, int]]` と書きたくなりますが、
   `int()` に通すまで値は文字列のままです。
3. `str | None` です。`Optional[str]` と書いても同じ意味ですが、
   このテキストでは `| None` を使います（[9.2.2](./09-typing-and-tools.md#922-optional-と--none)）。

---

**問 9.4 の解答**

`stock.py` の **14行目**で、`Item` を作るときの**2番目の引数**に文字列を渡しています。
`int()` で整数に変換してから渡すよう直します。

```python
# 直す前
Item(row["name"], row["price"])

# 直したあと
Item(row["name"], int(row["price"]))
```

**解説**

mypy のメッセージは4つの部分でできています
（[9.3.2](./09-typing-and-tools.md#932-導入して実行する)）。

| 部分 | 読み取れること |
|------|--------------|
| `stock.py:14` | `stock.py` の14行目 |
| `Argument 2 to "Item"` | `Item` の**2番目**の引数 |
| `has incompatible type "str"; expected "int"` | 文字列が渡されている。整数のはず |
| `[arg-type]` | 引数の型のルール |

**「2番目の引数」と言われたら、`self` は数えません**（[8.2.3](./08-oop.md#823-メソッドを定義する)）。
`Item("ノート", "180")` なら `"180"` のことです。

なお、`int()` を付ける以外に「`Item` の項目の型を `str` に変える」という直し方も考えられます。
ただし、そのあと計算に使うのであれば、**読み込んだ時点で数値にしておく**ほうが安全です。

---

**問 9.5 の解答**

**問題**：デフォルト値のリスト `[]` は**関数を定義したときに1回だけ作られ、
呼び出しのたびに使い回される**ため、前回追加した値が残ります。

**書き直したもの**

```python
def add_tag(name: str, tags: list[str] | None = None) -> list[str]:
    """tags に name を足したリストを返す。省略すると新しいリストを作る。"""
    if tags is None:
        tags = []
    tags.append(name)
    return tags
```

**解説**

これは [5.2.4](./05-functions.md#524-デフォルト引数にリストを使ってはいけない) で学んだ落とし穴に、
型ヒントを付けただけのものです。型ヒントは**この事故を防いでくれません**
（[9.1.4](./09-typing-and-tools.md#914-型ヒントは実行時に強制されない)）。

直したあとの型が `list[str] | None` になる点が、この章での新しい部分です。
「リストか、`None` か」のどちらかが入るので、`|` でつなぎます
（[9.2.2](./09-typing-and-tools.md#922-optional-と--none)）。

> **よくある間違い**
> `tags: list[str] = None` と書くと、型と初期値が食い違います。
> mypy はこう報告します。
>
> ```text
> error: Incompatible default for argument "tags" (default has type "None", argument has type "list[str]")  [assignment]
> ```

---

**問 9.6 の解答**

- `ruff check` … コードを読んで、**規約違反や怪しい書き方を報告する**（リンタ）
- `ruff format` … 空白・改行・引用符などを、**決められた形に整える**（フォーマッタ）

**解説**

2つは仕事が違います（[9.4.1](./09-typing-and-tools.md#941-ruff--リンタとフォーマッタ)）。

- `ruff check` が見つけるのは、**未使用の `import`**、未定義の名前、`import` の並び順など
- `ruff format` が直すのは、**見た目だけ**。`total=price*2` を `total = price * 2` にする

`ruff check --fix` を付けると、報告のうち `[*]` が付いたものを自動で直します。
`ruff format` は、[2.7.3](./02-basics.md#273-自動整形を設定する) で入れた Black の置き換えにあたります。

---

**問 9.7 の解答**

**`import` の並び順**が報告されるようになります（ルール名は `I001`）。

**解説**

`select` は「どのルールのまとまりを見るか」を選ぶ設定です
（[9.4.2](./09-typing-and-tools.md#942-設定ファイルpyprojecttoml)）。

| 記号 | 見るもの |
|------|---------|
| `E` | PEP 8 の書式 |
| `F` | 明らかな間違い（未使用の `import` など） |
| `I` | **`import` の並び順** |
| `UP` | 古い書き方（`List[str]` など） |

`"I"` を入れると、並びが崩れているときに次が出ます。

```text
I001 [*] Import block is un-sorted or un-formatted
```

`[*]` が付いているので、`ruff check --fix` で自動的に並べ替えられます。
**並び順を自分で覚える必要はありません。**

---

### 演習

### 演習 9.1 の解答

`python-lesson/price_tools.py`

```python
def with_tax(price: int, rate: float = 0.1) -> int:
    """税込価格を四捨五入して返す。"""
    return round(price * (1 + rate))


def total_price(prices: list[int]) -> int:
    """価格のリストを受け取り、税込の合計を返す。"""
    return sum([with_tax(p) for p in prices])


def label(name: str, price: int) -> str:
    """一覧表示用の1行を返す。"""
    return f"{name}: {price:,}円"


def main() -> None:
    """動作確認をする。"""
    prices: list[int] = [180, 120, 90]
    print(label("合計", total_price(prices)))
    print(label("ノート", with_tax(180)))
    print(label("ボールペン", with_tax(120)))


if __name__ == "__main__":
    main()
```

```text
実行結果:
合計: 429円
ノート: 198円
ボールペン: 132円
```

```text
mypy price_tools.py の実行結果:
Success: no issues found in 1 source file
```

**解説**

付けた型を1つずつ確認します。

| 関数 | 書いた型 | 決め方 |
|------|---------|--------|
| `with_tax` | `(price: int, rate: float = 0.1) -> int` | `round()` は整数を返すので戻り値は `int` |
| `total_price` | `(prices: list[int]) -> int` | 受け取るのは価格（整数）のリスト |
| `label` | `(name: str, price: int) -> str` | f-string を返すので `str` |
| `main` | `() -> None` | `print` するだけで何も返さない |

**`rate: float = 0.1` の書き順**に注意してください。
`rate = 0.1: float` とは書けません（[9.1.2](./09-typing-and-tools.md#912-変数と引数に型を書く)）。

`prices` に型ヒントを書いたのは、この演習の指示があったからですが、
実は `[180, 120, 90]` という値が入っているので、
書かなくても mypy は `list[int]` だと推測できます。
**書かないと困るのは、`prices: list[int] = []` のように空で作るとき**です
（[9.4.2](./09-typing-and-tools.md#942-設定ファイルpyprojecttoml) のよくある間違い）。

> **よくある間違い：`main` の `-> None` を忘れる**
> `def main():` のままにすると、mypy は `main` の**中身を検査しません**。
> `print(label(123, "ノート"))` のように引数を逆にしても `Success` と出てしまいます。
> **引数のない関数にも `-> None` を書く**、と決めておくのが安全です
> （[9.1.3](./09-typing-and-tools.md#913-戻り値の型)）。

> **よくある間違い：`round()` の戻り値を `float` と書く**
> `round(180 * 1.1)` の結果は `198`（整数）です。
> `round()` は、桁数を指定しなければ **`int` を返します**。
> `-> float` と書いても mypy は通しますが（`int` は `float` として扱えるため）、
> **実際に返るものを書く**のが型ヒントの目的です。

---

### 演習 9.2 の解答

`python-lesson/product_find.py`

```python
from dataclasses import dataclass


@dataclass
class Product:
    """商品を表すクラス。"""

    name: str
    price: int
    stock: int

    def is_available(self) -> bool:
        """在庫が1以上なら True を返す。"""
        return self.stock >= 1


def find_product(products: list[Product], name: str) -> Product | None:
    """商品名で1件探す。見つからなければ None を返す。"""
    for product in products:
        if product.name == name:
            return product
    return None


def cheapest(products: list[Product]) -> Product | None:
    """いちばん安い商品を返す。リストが空なら None を返す。"""
    if not products:
        return None
    return min(products, key=lambda p: p.price)


def main() -> None:
    """商品を探して表示する。"""
    products: list[Product] = [
        Product("ノート", 180, 12),
        Product("ボールペン", 120, 0),
        Product("消しゴム", 90, 4),
    ]

    for name in ["ノート", "定規"]:
        found = find_product(products, name)
        if found is None:
            print(f"{name}は取り扱っていません")
        elif found.is_available():
            print(f"{found.name}: {found.price}円（在庫あり）")
        else:
            print(f"{found.name}: {found.price}円（在庫切れ）")

    target = cheapest(products)
    if target is None:
        print("商品がありません")
    else:
        print(f"いちばん安いのは {target.name}（{target.price}円）です")

    print(f"空のリストの結果: {cheapest([])}")


if __name__ == "__main__":
    main()
```

```text
実行結果:
ノート: 180円（在庫あり）
定規は取り扱っていません
いちばん安いのは 消しゴム（90円）です
空のリストの結果: None
```

**解説**

この演習の中心は、**`| None` を書いた関数を、呼ぶ側がどう扱うか**です。

`find_product` の戻り値は `Product | None` なので、
返ってきた値は「`Product` かもしれないし `None` かもしれない」状態です。
`found.name` といきなり書くと、mypy に止められます。

```text
error: Item "None" of "Product | None" has no attribute "name"  [union-attr]
```

`if found is None:` を先に書くと、**その先では `Product` に確定した**と
mypy も理解してくれます（[9.2.2](./09-typing-and-tools.md#922-optional-と--none)）。

`cheapest` で `if not products:` を先に書いているのも同じ理由です。
これを書かずに `min(products, key=...)` だけにすると、
空のリストを渡したときに `ValueError` で止まります
（メッセージは Python のバージョンによって
`min() arg is an empty sequence` / `min() iterable argument is empty` と変わります）。

**「空かもしれない」ことに気づいた時点で、戻り値は `Product | None` になる**——
この結び付きが分かれば、この演習の目的は達成です。

> **別解：`for` を使わず1行で探す**
> 慣れてきたら、次の書き方もできます。
>
> ```python
> def find_product(products: list[Product], name: str) -> Product | None:
>     """商品名で1件探す。見つからなければ None を返す。"""
>     matched = [p for p in products if p.name == name]
>     return matched[0] if matched else None
> ```
>
> リスト内包表記（[4.5.1](./04-data-structures.md#451-リスト内包表記)）と
> 条件式（[3.1.6](./03-control-flow.md#316-条件式三項演算子)）の組み合わせです。
> ただし**全件を調べてからリストを作る**ので、
> 解答例の `for` + 早期 `return` のほうが素直です。

> **よくある間違い**
> `is_available` の戻り値に型を書き忘れないでください。
> `return self.stock >= 1` は比較の結果、つまり `True` か `False` なので `-> bool` です。
> `-> int` と書くと mypy に指摘されます。

---

### 演習 9.3 の解答

**修正前に実行すると、こうなります**

```text
実行結果:
佐藤: 8274点
鈴木: 6588点
高橋: 9058点
佐藤さんの合計は 8274点です
```

**止まらないのに、答えが完全に間違っています。**
`82 + 74` ではなく、文字列の `"82"` と `"74"` が**連結**されて `"8274"` になっています
（[2.4.1](./02-basics.md#241-連結と繰り返し)）。

**mypy の報告**

```text
broken_report.py:25: error: Argument 2 to "Score" has incompatible type "str | Any"; expected "int"  [arg-type]
broken_report.py:25: error: Argument 3 to "Score" has incompatible type "str | Any"; expected "int"  [arg-type]
broken_report.py:44: error: Item "None" of "Score | None" has no attribute "total"  [union-attr]
Found 3 errors in 1 file (checked 1 source file)
```

**修正後のファイル**

`python-lesson/broken_report.py`

```python
import csv
from dataclasses import dataclass
from pathlib import Path


@dataclass
class Score:
    """1人ぶんの点数。"""

    name: str
    math: int
    english: int

    def total(self) -> int:
        """2教科の合計点を返す。"""
        return self.math + self.english


def load_scores(path: Path) -> list[Score]:
    """CSV を読み込んで Score のリストを返す。"""
    scores: list[Score] = []
    with open(path, encoding="utf-8", newline="") as f:
        reader = csv.DictReader(f)
        for row in reader:
            scores.append(Score(row["name"], int(row["math"]), int(row["english"])))
    return scores


def find_score(scores: list[Score], name: str) -> Score | None:
    """名前で1件探す。見つからなければ None を返す。"""
    for score in scores:
        if score.name == name:
            return score
    return None


def main() -> None:
    """読み込んで表示する。"""
    scores = load_scores(Path("data/scores.csv"))
    for score in scores:
        print(f"{score.name}: {score.total()}点")

    target = find_score(scores, "佐藤")
    if target is None:
        print("佐藤さんは見つかりませんでした")
    else:
        print(f"佐藤さんの合計は {target.total()}点です")


if __name__ == "__main__":
    main()
```

```text
実行結果:
佐藤: 156点
鈴木: 153点
高橋: 148点
佐藤さんの合計は 156点です
```

**解説**

直したのは2か所です。

**1. 25行目：CSV の値を `int()` に通す**

```diff
-             scores.append(Score(row["name"], row["math"], row["english"]))
+             scores.append(Score(row["name"], int(row["math"]), int(row["english"])))
```

`csv.DictReader` が返す値は**すべて文字列**です
（[7.4.1](./07-files-and-exceptions.md#741-csv-モジュール)）。
`Score` の項目は `math: int` と宣言してあるので、食い違います。

この間違いが厄介なのは、**実行しても止まらない**ことです。
`"82" + "74"` は文字列の連結として成立してしまいます。
mypy は、これを**動かす前に**2件とも報告してくれました。

> **補足：`"str | Any"` と表示されるのはなぜか**
> `row["math"]` の型が `str` ではなく `str | Any` と出るのは、
> `csv.DictReader` の型情報がそう作られているためです。
> **`str` が混ざっている＝整数ではない**、と読めば十分です。

**2. 44行目：`None` かどうかを確認してから使う**

```diff
      target = find_score(scores, "佐藤")
-     print(f"佐藤さんの合計は {target.total()}点です")
+     if target is None:
+         print("佐藤さんは見つかりませんでした")
+     else:
+         print(f"佐藤さんの合計は {target.total()}点です")
```

`find_score` の戻り値は `Score | None` と宣言されています。
いまは `"佐藤"` が必ず入っているファイルなので動きますが、
**CSV から佐藤さんの行が消えた瞬間**に `AttributeError` で落ちます。

```text
AttributeError: 'NoneType' object has no attribute 'total'
```

mypy の `union-attr` は、**まだ起きていないこの事故**を先に報告してくれたことになります
（[9.2.2](./09-typing-and-tools.md#922-optional-と--none)）。

> **よくある間違い：`# type: ignore` で消す**
> 44行目に `# type: ignore` を付ければ報告は消えますが、
> **落ちる可能性はそのまま残ります**（[9.3.2](./09-typing-and-tools.md#932-導入して実行する) の注意）。
> 報告を消すのではなく、分岐を書いてください。

> **よくある間違い：`Score` の項目を `str` に変えて通す**
> `math: str` と書けば mypy は通りますが、
> `total()` の中の足し算が文字列の連結のままなので、
> **表示は `8274点` のまま**です。
> **エラーを消すことと、正しく動かすことは別**だと覚えてください。

---

### 演習 9.4 の解答

`python-lesson/pyproject.toml`

```toml
[tool.ruff]
line-length = 88
target-version = "py313"

[tool.ruff.lint]
select = ["E", "F", "I", "UP"]

[tool.mypy]
python_version = "3.13"
ignore_missing_imports = true
disallow_untyped_defs = true
```

`python-lesson/messy_stock.py`

```python
import csv
import json
from pathlib import Path


def load(path: Path) -> list[dict[str, str]]:
    """CSV を読み込んで、辞書のリストとして返す。"""
    rows: list[dict[str, str]] = []
    with open(path, encoding="utf-8", newline="") as f:
        for row in csv.DictReader(f):
            rows.append({"name": row["name"], "count": row["count"]})
    return rows


def save(rows: list[dict[str, str]], path: Path) -> None:
    """辞書のリストを JSON に書き出す。"""
    with open(path, "w", encoding="utf-8") as f:
        json.dump(rows, f, ensure_ascii=False, indent=2)


def main() -> None:
    """在庫の CSV を読み込んで JSON に書き出す。"""
    rows = load(Path("data/stock.csv"))
    save(rows, Path("data/stock.json"))
    print(f"{len(rows)}件を書き出しました")


if __name__ == "__main__":
    main()
```

```text
ruff check messy_stock.py の実行結果:
All checks passed!
```

```text
ruff format messy_stock.py の実行結果:
1 file left unchanged
```

```text
mypy messy_stock.py の実行結果:
Success: no issues found in 1 source file
```

```text
python messy_stock.py の実行結果:
3件を書き出しました
```

`python-lesson/data/stock.json`

```json
[
  {
    "name": "ノート",
    "count": "12"
  },
  {
    "name": "ボールペン",
    "count": "3"
  },
  {
    "name": "消しゴム",
    "count": "25"
  }
]
```

**解説**

作業の順番が大事なので、通してたどります。

**1. `ruff check --fix messy_stock.py`**

```text
Found 1 error (1 fixed, 0 remaining).
```

直ったのは `import` の並び順（`I001`）です。
`pyproject.toml` の `select` に `"I"` を入れたから報告されました。
入れていなければ、並びが崩れたままでも何も言われません。

```diff
- import json
- from pathlib import Path
  import csv
+ import json
+ from pathlib import Path
```

**2. `ruff format messy_stock.py`**

```text
1 file reformatted
```

`rows=[]` が `rows = []`、`open(path,encoding=...)` が `open(path, encoding=...)` になり、
関数と関数のあいだが2行空きました。
**ここまでは、道具が全部やってくれます。**

**3. `mypy messy_stock.py`**

ここで、道具に任せられない部分が出ます。

```text
messy_stock.py:6: error: Function is missing a type annotation  [no-untyped-def]
messy_stock.py:14: error: Function is missing a type annotation  [no-untyped-def]
messy_stock.py:19: error: Function is missing a return type annotation  [no-untyped-def]
messy_stock.py:19: note: Use "-> None" if function does not return a value
```

`disallow_untyped_defs = true` を書いたので、
**型ヒントを書いていない関数そのもの**が報告されました
（[9.4.2](./09-typing-and-tools.md#942-設定ファイルpyprojecttoml) の補足）。
この設定がないと、3つとも報告されないまま `Success` と出ます。

19行目の `note:` は、mypy からの助け船です。
「何も返さないなら `-> None` と書け」と教えてくれています。

**4. 型ヒントと docstring を書く**

| 関数 | 書いた型 | 決め方 |
|------|---------|--------|
| `load` | `(path: Path) -> list[dict[str, str]]` | `csv.DictReader` の行を集めたもの。**値はすべて文字列**（[9.2.1](./09-typing-and-tools.md#921-リスト辞書タプル)） |
| `save` | `(rows: list[dict[str, str]], path: Path) -> None` | `load` が返したものをそのまま受け取り、書き出すだけ |
| `main` | `() -> None` | 何も返さない |

`rows: list[dict[str, str]] = []` の型ヒントは省略できません。
**空のリストからは中身の型が推測できない**ためです。省くとこうなります。

```text
error: Need type annotation for "rows" (hint: "rows: list[<type>] = ...")  [var-annotated]
```

**5. もう一度3つを実行して、すべて通ることを確認する**

`ruff format` が `1 file left unchanged` になれば、整形すべき箇所がもうない状態です。

> **よくある間違い：`count` を整数にしようとする**
> 書き出された JSON の `"count": "12"` が文字列であることを、
> 間違いだと思って `int(row["count"])` に直したくなるかもしれません。
> しかし、そうすると `load` の戻り値は `list[dict[str, str]]` ではなくなり、
> **mypy に別のエラーを出されます。**
>
> ```text
> error: Dict entry 1 has incompatible type "str": "int"; expected "str": "str"  [dict-item]
> ```
>
> **型ヒントは「こう書きたい」という願望ではなく、「実際にこうなっている」という事実を書くもの**です。
> 整数として扱いたいなら、`list[dict[str, str | int]]` のように
> 型のほうも合わせて変える必要があります
> （[9.4.2](./09-typing-and-tools.md#942-設定ファイルpyprojecttoml) の `to_rows` がその形です）。

> **よくある間違い：`ruff check .` や `mypy .` と打つ**
> `python-lesson` には第1章からのファイルが全部あるので、
> **大量の報告が出て、どれが自分の課題のものか分からなくなります。**
> ファイル名を1つだけ指定してください
> （[9.4.1](./09-typing-and-tools.md#941-ruff--リンタとフォーマッタ) の注意）。

---

## 第10章

### 理解度チェック

**問 10.1 の解答**

- A = **`timeout`**（`requests.get(URL, params=..., timeout=10)` のように秒数を渡す）
- B = **`raise_for_status()`**

**解説**

この2つは、通信するコードを書くときの「必ず書くもの」です。

`timeout` を書かないと、相手が応答しないときに**いつまでも待ち続けます**。
画面には何も出ず、止まったようにしか見えません
（[10.3.1](./10-practice-data-script.md#1031-requests-を使う)）。

`raise_for_status()` を書かないと、`404` や `500` が返ってきても
**「成功」として次の行へ進みます。** `requests` は
「サーバーとやり取りできた」ことを成功とみなすためです。
エラーの JSON をそのまま集計しようとして、ずっと先の行で
意味の分からない `KeyError` になります
（[10.3.3](./10-practice-data-script.md#1033-エラー処理とリトライ)）。

---

**問 10.2 の解答**

`--times 3` と指定すると、`args.times` には**文字列の `"3"`** が入るためです。
`range()` は整数しか受け取れません。
省略したときは `default=1` の**整数の `1`** がそのまま入るので、動いてしまいます。

```text
実行結果:
Traceback (most recent call last):
  File "demo.py", line 7, in <module>
    for i in range(args.times):
             ^^^^^^^^^^^^^^^^^
TypeError: 'str' object cannot be interpreted as an integer
```

直し方は `type=int` を足すだけです。

```python
parser.add_argument("--times", type=int, default=1)
```

**解説**

**コマンドラインから渡される値は、必ず文字列です。**
これは `input` の戻り値が必ず文字列だったのと同じ事情です
（[2.5.3](./02-basics.md#253-入力は必ず文字列である)）。

この問題がやっかいなのは、**省略したときだけ動いてしまう**ことです。
`default=1` は Python のコードの中に書いた値なので整数のまま渡り、
コマンドラインから来た値だけが文字列になります。
「動作確認したときは動いたのに、オプションを付けたら落ちた」という形で表面化します。

`type=int` を書いておけば、変換してくれるうえに、
数値でないものを渡されたときは `argparse` が止めてくれます
（[10.5.1](./10-practice-data-script.md#1051-argparse-の基本)）。

```text
実行結果:
demo.py: error: argument --times: invalid int value: 'おおい'
```

> **よくある間違い**
> `int(args.times)` と、使うたびに変換する書き方もできます。
> ただし、そうすると**使う場所すべてで変換を書く**ことになり、1か所忘れると同じ事故が起きます。
> **受け取る場所で1回だけ変換する**（`type=int`）ほうが確実です。
> これは、CSV を読んだ値を `Sale` を作るときに `int()` しておく
> （[10.2.2](./10-practice-data-script.md#1022-読み込んで表示する)）のと同じ考え方です。

---

**問 10.3 の解答**

```python
data = {
    "daily": {
        "time": ["2024-07-01", "2024-07-02"],
        "temperature_2m_max": [28.2, 31.4],
    }
}

temperatures = {}
for index, day in enumerate(data["daily"]["time"]):
    temperatures[day] = data["daily"]["temperature_2m_max"][index]

print(temperatures)
```

```text
実行結果:
{'2024-07-01': 28.2, '2024-07-02': 31.4}
```

**解説**

**2本のリストが、同じ順番で並んでいる**のがポイントです。
`time` の0番目と `temperature_2m_max` の0番目が同じ日のデータなので、
**同じ番号どうしを組にすれば**ほしい形になります
（[10.3.2](./10-practice-data-script.md#1032-json-を扱う)）。

番号と値を同時に取り出す道具が `enumerate` でした
（[3.2.3](./03-control-flow.md#323-enumerate-で番号付きに回す)）。
`index` が 0, 1, 2, ... と進み、`day` に日付が入ります。

**別解：番号だけを回す**

```python
days = data["daily"]["time"]
values = data["daily"]["temperature_2m_max"]

temperatures = {}
for index in range(len(days)):
    temperatures[days[index]] = values[index]
```

`range(len(...))` でも同じ結果になります。
ただし `days[index]` と2段階で書くことになるので、`enumerate` のほうが読みやすくなります。

**別解：辞書内包表記**

```python
days = data["daily"]["time"]
values = data["daily"]["temperature_2m_max"]

temperatures = {day: values[index] for index, day in enumerate(days)}
```

辞書内包表記（[4.5.3](./04-data-structures.md#453-辞書内包表記)）でも書けます。
1行で書けますが、**この形が読みやすいかどうかは場合によります。**
迷ったら `for` に戻してかまいません（[4.5.4](./04-data-structures.md#454-読みにくくなったら-for-に戻す)）。

> **よくある間違い**
> `data["daily"]["time"]` を毎回書くのが長いからと、
> 先に `daily = data["daily"]` と取り出しておくのは良い書き方です。
> 一方、`data["time"]` のように**入れ子を1段飛ばす**と `KeyError` になります。
> 迷ったら `print(data.keys())` でその階層のキーを確認してください
> （[10.3.2](./10-practice-data-script.md#1032-json-を扱う) のよくある間違い）。

---

**問 10.4 の解答**

`defaultdict` は**存在しないキーを読んでもエラーにならない**ため、
受け取った側でキーを打ち間違えると、`0` が返って静かに間違った結果になるからです。
集計中だけ `defaultdict` を使い、返すときに普通の辞書に戻しておくと、
打ち間違いが `KeyError` としてその場で分かります。

**解説**

`defaultdict(int)` の便利さは、そのまま危うさでもあります。

```python
totals = defaultdict(int)
totals["2024-07-01"] += 100
print(totals["2024-07-99"])   # 0 が返る（存在しない日付なのに）
```

集計している最中は、この動きが**まさに必要なもの**です。
初めて出てきた日付を `0` から数え始めてくれるので、
`if 日付 in totals:` のような確認が要りません。

しかし、集計が終わって値を返したあとは、この動きは**害にしかなりません。**
`date_totals["2024-07-08"]` と書き間違えても `0` が返るので、
「なぜか売上が 0 円の日がある」という形でしか気づけません。

`dict(totals)` は、**中身が同じ普通の辞書**を新しく作ります。
1行で「便利さが必要な区間」を終わらせられます
（[10.2.4](./10-practice-data-script.md#1024-集計する)）。

> **補足**
> `defaultdict` は `dict` を継承したクラスです（[8.3](./08-oop.md#83-継承)）。
> そのため `dict[str, int]` を求める場所にそのまま渡しても、
> mypy には叱られません。**道具が守ってくれない部分**なので、
> 書く側が意識して戻す必要があります。

---

**問 10.5 の解答**

使えるのは **1** です。

日付が `YYYY-MM-DD` の形（**桁数が固定で、大きい単位が左にある**）なら、
文字列として1文字ずつ比べた結果と、日付として比べた結果が一致します。

2 の `2024/7/4` の形では使えません。月や日が1桁のときに桁数が変わるため、
`"2024/7/4"` と `"2024/12/1"` を比べると、4文字目の `7` と `1` の比較になり、
**7月のほうが後ろだと判定されてしまいます。**

**解説**

文字列どうしの `<` は、**先頭の文字から順に比較**します。
最初に違いが出たところで勝負が決まります。

```python
>>> "2024-07-04" < "2024-07-06"
True
>>> "2024-12-31" < "2025-01-01"
True
>>> "2024/7/4" < "2024/12/1"
False
```

3つ目が `False` になるのが、2 が使えない理由です
（[10.2.3](./10-practice-data-script.md#1023-条件で絞り込む)）。

`2024/7/4` の形のデータを扱うときは、
`datetime.strptime` で日付として読み込んでから比べるか、
読み込む時点で `2024-07-04` の形に直します。
**このテキストの範囲では、「そろった形のデータを使う」ことで避けています。**

> **補足**
> `YYYY-MM-DD` の形は **ISO 8601** という国際規格で決まった書き方です。
> データを保存するときにこの形を選んでおくと、
> 並べ替えも比較もそのままできる、という利点があります。
> 自分でデータを作るときは、この形にしておくのが安全です。

---

**問 10.6 の解答**

**そのあとどうするかを、その場で決められるかどうか**で分かれています。
気温が取れないときは「気温なしで集計を続ける」と決められるので捕まえます。
CSV がないときは、代わりに何を読めばよいか決められないので、捕まえずに止めます。

**解説**

これは [7.6.3](./07-files-and-exceptions.md#763-どこで例外を捕まえるべきか) で決めた方針を、
実際のプログラムに当てはめたものです。

| 起きたこと | 決められること | 対応 |
|-----------|--------------|------|
| 気温が取れない | 気温の列を空にして、売上の集計は出す | 捕まえて `None` を返す |
| CSV が見つからない | 何も決められない（読むものがない） | 捕まえずにトレースバックで止める |

**捕まえること自体が良いことではありません。**
「捕まえたけれど、結局どうしていいか分からない」という場合、
それは握りつぶし（[7.5.5](./07-files-and-exceptions.md#755-握りつぶさないexcept-pass-の危険)）に近づきます。
そのまま止まって、**どこで何が起きたかがトレースバックに出る**ほうが親切です。

> **補足**
> 「CSV が見つかりません、パスを確認してください」という
> 親切なメッセージを出したい、と考えたなら、その判断は妥当です。
> その場合、`main` の中で `except FileNotFoundError:` を書き、
> メッセージを表示して `return` します。
> **決められることが増えたなら、捕まえてよい**ということです
> （[10.6.3](./10-practice-data-script.md#1063-動作確認のチェックリスト) の補足）。

---

**問 10.7 の解答**

- `--no-weather` を**付けた**とき … `True`
- **付けなかった**とき … `False`

**解説**

`action="store_true"` は「**付いていれば `True` を入れる**」という指定です。
値を書く必要がなく、`--no-weather` と書くだけで動きます。
省略したときは、自動的に `False` になります
（[10.5.2](./10-practice-data-script.md#1052-引数を受け取る)）。

本文ではこう使っていました。

```python
if args.no_weather:
    temperatures = None
else:
    temperatures = fetch_max_temperatures(args.start, args.end)
```

なお、`--no-weather` のように `-` を含む名前は、
取り出すときに **`-` が `_` に変わって** `args.no_weather` になります。
`args.no-weather` と書くと `SyntaxError` になるので注意してください
（Python は `no - weather` という引き算だと解釈します）。

---

### 演習

### 演習 10.1 の解答

`analyze_sales.py` に追加する関数です。

```python
def total_by_shop(sales: list[Sale]) -> dict[str, int]:
    """店舗ごとの売上金額を返す。"""
    totals: defaultdict[str, int] = defaultdict(int)
    for sale in sales:
        totals[sale.shop] += sale.subtotal()
    return dict(totals)
```

`main` の中、商品別売上の表示のあと（`rows = build_rows(...)` の**前**）に追加します。

```python
    print()
    print(f"【{args.start} 〜 {args.end} の店舗別売上】")
    shop_totals = total_by_shop(target)
    for shop in sorted(shop_totals):
        print(f"{shop}: {shop_totals[shop]:,}円")

    print()
```

```powershell
python analyze_sales.py --no-weather
```

```text
実行結果:
【2024-07-01 〜 2024-07-07 の商品別売上】
ソフトクリーム: 278,600円
かき氷: 134,100円
アイスコーヒー: 56,100円
ホットコーヒー: 41,700円
焼きドーナツ: 14,600円

【2024-07-01 〜 2024-07-07 の店舗別売上】
本店: 333,300円
駅前店: 191,800円

7日ぶんを out\sales_report_20260831.csv に書き出しました
```

**解説**

`total_by_item`（[10.2.4](./10-practice-data-script.md#1024-集計する)）と比べてみてください。
**違うのは `sale.item` が `sale.shop` になった1か所だけ**です。

```python
totals[sale.item] += sale.subtotal()    # 商品ごと
totals[sale.shop] += sale.subtotal()    # 店舗ごと
```

「何をキーにして足し込むか」を変えるだけで、集計の切り口が変わります。
**これが `defaultdict` を使った集計の型です。** 一度覚えると、
「担当者ごと」「月ごと」「地域ごと」など、そのまま応用できます。

**なぜ `sorted` でキーを並べるのか**

```python
for shop in sorted(shop_totals):
```

辞書は、**入れた順に並んでいます。** そのまま回すと、
CSV の並び方が変わったときに表示順も変わってしまいます。
`sorted` を通せば、いつ実行しても同じ順（ここでは店舗名の順）になります。

なお、日本語のキーを `sorted` に渡すと、**文字コードの順**に並びます
（[4.1.5](./04-data-structures.md#415-並べ替えsort--sorted)）。
「本店」「駅前店」という並びは、その結果です。読み順ではありません。

> **よくある間違い**
> `print()` を入れずに続けて表示すると、2つの見出しがくっついて読みにくくなります。
> **空の `print()` は1行の空行を出す**のでした（[2.5.1](./02-basics.md#251-print-の使い方)）。
> 表示を人が読むものとして整えるのも、スクリプトの仕事のうちです。

**別解：関数を1つにまとめる**

`total_by_item` と `total_by_shop` はほぼ同じなので、
「何をキーにするか」を引数で渡す形にもできます。

```python
def total_by(sales: list[Sale], key_name: str) -> dict[str, int]:
    """指定した属性ごとの売上金額を返す。"""
    totals: defaultdict[str, int] = defaultdict(int)
    for sale in sales:
        totals[getattr(sale, key_name)] += sale.subtotal()
    return dict(totals)
```

`getattr(オブジェクト, "名前")` は、属性を文字列で指定して読み取る命令です。
**このテキストでは扱っていない書き方**なので、演習の解答としては上のものを正解とします。
「まとめられそうだ」と感じたなら、その感覚は正しいものです。
ただし、**この場合はまとめないほうが読みやすい**という判断もあります。
`total_by(sales, "shop")` は、打ち間違えても mypy が気づけないためです。

---

### 演習 10.2 の解答

`parse_args` に追加する部分です（`--no-weather` の**前**に置きました）。

```python
    parser.add_argument(
        "--min-total",
        type=int,
        default=0,
        metavar="金額",
        help="この金額以上の日だけ書き出す（既定: 0）",
    )
```

`main` の書き出し部分を、1行足して次のようにします。

```python
    rows = build_rows(date_totals, date_quantities, temperatures)
    rows = [row for row in rows if int(row["total"]) >= args.min_total]
    out_path = make_output_path(Path(args.out_dir))
    save_rows(rows, out_path)
    print(f"{len(rows)}日ぶんを {out_path} に書き出しました")
```

```powershell
python analyze_sales.py --min-total 70000
```

```text
実行結果:
【2024-07-01 〜 2024-07-07 の商品別売上】
ソフトクリーム: 278,600円
かき氷: 134,100円
アイスコーヒー: 56,100円
ホットコーヒー: 41,700円
焼きドーナツ: 14,600円
4日ぶんを out\sales_report_20260831.csv に書き出しました
```

```text
date,quantity,total,max_temperature
2024-07-04,202,72700,33.2
2024-07-05,230,83050,33.7
2024-07-06,318,110300,33.7
2024-07-07,258,93350,32.9
```

**解説**

この演習の要点は2つです。

**1. `type=int` を書く**

書かないと `args.min_total` は文字列の `"70000"` になります。
そのまま比較すると、**文字列どうしの比較**になってしまいます。

```python
>>> "110300" >= "70000"
False
```

先頭の `1` と `7` を比べて `False` になるためです。
金額としては 110,300 のほうが大きいのに、逆の結果になります。
**エラーが出ないので、間違いに気づきにくい**のがこの落とし穴の怖いところです
（[10.5.1](./10-practice-data-script.md#1051-argparse-の基本)）。

**2. `int(row["total"])` で戻す**

`build_rows` は、**すべての値を文字列にして**返していました
（[10.4.1](./10-practice-data-script.md#1041-csv-に書き出す)）。
比較するときは `int()` で数値に戻す必要があります。

「文字列にしたものを、また整数に戻すのは無駄では」と思うかもしれません。
そのとおりで、**絞り込みを `build_rows` より前に置く**ほうが素直です。

**別解：辞書のうちに絞り込む**

```python
    date_totals = {
        day: amount
        for day, amount in date_totals.items()
        if amount >= args.min_total
    }
    rows = build_rows(date_totals, date_quantities, temperatures)
```

`build_rows` に渡す前に、`date_totals` のほうを絞ります。
`date_totals` の値は整数のままなので、`int()` が要りません。
`build_rows` は `sorted(date_totals)` の日付だけを見て組み立てるので、
渡す辞書を減らせば、そのまま行数が減ります。

**どちらでも正解です。** 変換の回数だけを見るならこちらのほうが素直で、
「書き出す直前に絞る」という意図の分かりやすさなら前者です。

> **よくある間違い**
> `--min-total` は、取り出すときに `args.min_total` になります
> （`-` が `_` に変わる）。`args.min-total` と書くと `SyntaxError` です。

---

### 演習 10.3 の解答

追加・変更するのは3つの関数です。

**1. 商品別の行を組み立てる関数を追加する**

```python
def build_item_rows(item_totals: dict[str, int]) -> list[dict[str, str]]:
    """商品別の書き出し用の辞書のリストを、売上の多い順に組み立てて返す。"""
    total = sum(item_totals.values())
    rows: list[dict[str, str]] = []
    for item, amount in sorted(
        item_totals.items(), key=lambda pair: pair[1], reverse=True
    ):
        rows.append(
            {
                "item": item,
                "total": str(amount),
                "share": f"{amount / total * 100:.1f}",
            }
        )
    return rows
```

**2. `save_rows` が列名を受け取るようにする**

```python
def save_rows(rows: list[dict[str, str]], path: Path, fieldnames: list[str]) -> None:
    """辞書のリストを CSV として書き出す。"""
    path.parent.mkdir(parents=True, exist_ok=True)
    with open(path, "w", encoding="utf-8", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()
        for row in rows:
            writer.writerow(row)
```

**3. `make_output_path` がファイル名の先頭を受け取るようにする**

```python
def make_output_path(out_dir: Path, prefix: str) -> Path:
    """今日の日付を入れた書き出し先のパスを返す。"""
    stamp = datetime.now().strftime("%Y%m%d")
    return out_dir / f"{prefix}_{stamp}.csv"
```

`main` の最後を、次のように書き換えます。

```python
    rows = build_rows(date_totals, date_quantities, temperatures)
    out_path = make_output_path(Path(args.out_dir), "sales_report")
    save_rows(rows, out_path, ["date", "quantity", "total", "max_temperature"])
    print(f"{len(rows)}日ぶんを {out_path} に書き出しました")

    item_rows = build_item_rows(item_totals)
    item_path = make_output_path(Path(args.out_dir), "item_report")
    save_rows(item_rows, item_path, ["item", "total", "share"])
    print(f"{len(item_rows)}商品ぶんを {item_path} に書き出しました")
```

```powershell
python analyze_sales.py
```

```text
実行結果:
【2024-07-01 〜 2024-07-07 の商品別売上】
ソフトクリーム: 278,600円
かき氷: 134,100円
アイスコーヒー: 56,100円
ホットコーヒー: 41,700円
焼きドーナツ: 14,600円
7日ぶんを out\sales_report_20260831.csv に書き出しました
5商品ぶんを out\item_report_20260831.csv に書き出しました
```

`out/item_report_20260831.csv` の中身です。

```text
item,total,share
ソフトクリーム,278600,53.1
かき氷,134100,25.5
アイスコーヒー,56100,10.7
ホットコーヒー,41700,7.9
焼きドーナツ,14600,2.8
```

**解説**

**`sum(item_totals.values())` で全体を出す**

`values()` は辞書の値だけを取り出します（[4.3.4](./04-data-structures.md#434-キー値両方を回す)）。
それを `sum()` に渡すと合計になります。
`sum(item_totals)` と書くと**キー（商品名）を足そうとして** `TypeError` になるので注意してください。

**`f"{amount / total * 100:.1f}"`**

`:.1f` は「小数第1位まで表示する」という書式指定です
（[2.4.4](./02-basics.md#244-f-string) の `:.2f` の仲間）。

`278600 / 524600 * 100` は `53.107...` という値になりますが、
`:.1f` を通すと `53.1` という**文字列**になります。
`build_rows` と同じく、書き出す直前に文字列へそろえています
（[10.4.1](./10-practice-data-script.md#1041-csv-に書き出す)）。

**なぜ `save_rows` に `fieldnames` を渡すのか**

もとの `save_rows` は、列名が関数の中に**書き込まれて**いました。

```python
writer = csv.DictWriter(
    f, fieldnames=["date", "quantity", "total", "max_temperature"]
)
```

このままだと、商品別の CSV を書くときに `save_rows` をもう1つ作ることになります。
**「書き出す」という仕事は同じで、違うのは列名だけ**なので、
違う部分を引数にすれば1つの関数で足ります。

これが [5.6.3](./05-functions.md#563-どこで関数に切り出すか)「どこで関数に切り出すか」の逆向きの判断——
**「ほとんど同じ関数が2つできそうなら、違いを引数にする」**——です。

> **よくある間違い**
> `fieldnames` に書いた列名と、`rows` の辞書のキーが1つでも食い違うと、
> `DictWriter` は次のエラーを出します。
>
> ```text
> ValueError: dict contains fields not in fieldnames: 'share'
> ```
>
> 「辞書に `share` があるのに、列名の一覧に書かれていない」という意味です。
> **列名の一覧と、辞書のキーを見比べてください。**

**別解：合計を引数で受け取る**

```python
def build_item_rows(item_totals: dict[str, int], total: int) -> list[dict[str, str]]:
```

合計を関数の外で計算して渡す形もあります。
「全体の売上」を他の場所でも使うなら、こちらのほうが二度手間になりません。
今回は他で使わないので、関数の中で計算しました。

---

### 演習 10.4 の解答

**1. 取得の関数を一般化する**

```python
def fetch_daily_values(start: str, end: str, variable: str) -> dict[str, float] | None:
    """期間中の日ごとの値を返す。取得できなければ None を返す。"""
    params: dict[str, str] = {
        "latitude": str(LATITUDE),
        "longitude": str(LONGITUDE),
        "start_date": start,
        "end_date": end,
        "daily": variable,
        "timezone": "Asia/Tokyo",
    }
    for attempt in range(1, RETRY_COUNT + 1):
        try:
            response = requests.get(API_URL, params=params, timeout=10)
            response.raise_for_status()
            data = response.json()
        except requests.RequestException as e:
            print(f"{variable} の取得に失敗しました（{attempt}回目）: {e}")
            if attempt < RETRY_COUNT:
                time.sleep(RETRY_WAIT_SECONDS)
            continue

        values: dict[str, float] = {}
        for index, day in enumerate(data["daily"]["time"]):
            values[day] = data["daily"][variable][index]
        return values

    return None
```

**2. `build_rows` に降水量を足す**

```python
def build_rows(
    date_totals: dict[str, int],
    date_quantities: dict[str, int],
    temperatures: dict[str, float] | None,
    rainfalls: dict[str, float] | None,
) -> list[dict[str, str]]:
    """書き出し用の辞書のリストを、日付順に組み立てて返す。"""
    rows: list[dict[str, str]] = []
    for day in sorted(date_totals):
        if temperatures is None:
            temperature = ""
        else:
            temperature = str(temperatures.get(day, ""))
        if rainfalls is None:
            rainfall = ""
        else:
            rainfall = str(rainfalls.get(day, ""))
        rows.append(
            {
                "date": day,
                "quantity": str(date_quantities[day]),
                "total": str(date_totals[day]),
                "max_temperature": temperature,
                "rainfall": rainfall,
            }
        )
    return rows
```

**3. `save_rows` の列名に足す**

```python
        writer = csv.DictWriter(
            f,
            fieldnames=["date", "quantity", "total", "max_temperature", "rainfall"],
        )
```

**4. `main` から2回呼ぶ**

```python
    if args.no_weather:
        temperatures = None
        rainfalls = None
    else:
        temperatures = fetch_daily_values(args.start, args.end, "temperature_2m_max")
        rainfalls = fetch_daily_values(args.start, args.end, "precipitation_sum")
        if temperatures is None or rainfalls is None:
            print("気象データなしで集計を続けます")
```

```python
    rows = build_rows(date_totals, date_quantities, temperatures, rainfalls)
```

```powershell
python analyze_sales.py
```

```text
date,quantity,total,max_temperature,rainfall
2024-07-01,149,48600,28.2,6.7
2024-07-02,172,60900,31.4,4.9
2024-07-03,164,56200,31.4,1.1
2024-07-04,202,72700,33.2,0.0
2024-07-05,230,83050,33.7,0.0
2024-07-06,318,110300,33.7,1.1
2024-07-07,258,93350,32.9,0.0
```

**解説**

書き出された表を読むと、面白いことが分かります。
**雨が降った 7月1日・2日は、気温のわりに売上が伸びていません。**
気温だけを見ていたときには気づけなかったことです。

**列を1つ足すだけで、見えるものが変わる。** これがデータ処理の面白いところです。

**「関数の中に固定で書かれていた値」を引数にする**

もとの `fetch_max_temperatures` は、`"temperature_2m_max"` を2か所に書き込んでいました。

```python
"daily": "temperature_2m_max",                              # 送るとき
values[day] = data["daily"]["temperature_2m_max"][index]    # 受け取るとき
```

**この2つは、必ず同じ値でなければなりません。**
Open-Meteo は「送った変数名を、そのままキーにして返す」ためです。
引数 `variable` にまとめると、**片方だけ直す**という間違いが起こらなくなります。

これは演習 10.3 の `fieldnames` と同じ考え方です。
「ほとんど同じ関数が2つできそうなら、違いを引数にする」
（[5.2.1](./05-functions.md#521-位置引数) / [5.6.3](./05-functions.md#563-どこで関数に切り出すか)）。

**`if temperatures is None or rainfalls is None:`**

どちらか一方でも取れなかったら、メッセージを出します。
`build_rows` の中で、取れなかったほうの列だけが空欄になります。

**`0.0` は「雨が降らなかった」**

`rainfall` の列に `0.0` と入っている日は、雨が降らなかった日です。
空欄（取得できなかった）とは意味が違います。
**「値がない」と「値が 0 である」は違う**——という区別は、
`None` と `0` の区別（[2.2.4](./02-basics.md#224-none)）と同じ話です。

> **よくある間違い**
> `build_rows` の引数を1つ増やしたのに、
> `main` の呼び出し側を直し忘れると、次のエラーになります。
>
> ```text
> TypeError: build_rows() missing 1 required positional argument: 'rainfalls'
> ```
>
> **引数を増やしたら、呼んでいる場所をすべて直す**（[5.2.1](./05-functions.md#521-位置引数)）。
> mypy を実行すれば、動かす前にこの間違いを見つけられます。

> **補足**
> 2回 API を呼んでいるので、通信も2回起きています。
> Open-Meteo は `"daily": "temperature_2m_max,precipitation_sum"` のように
> **カンマで区切って一度に複数の値を要求**できます。
> 1回の通信で済ませたい場合は、この形にして、
> 返ってきた `daily` から2つのリストを取り出すことになります。
> **通信の回数は少ないほうがよい**（相手のサーバーにも優しい）ので、
> 実務ではこちらを選ぶ場面が多くなります。

---

## 第11章

### 理解度チェック

**問 11.1 の解答**

- A = **`test_`**（ファイル名も関数名も、この4文字で始める）
- B = **`assert`**
- C = **`AssertionError`**

**解説**

pytest は、設定を書かなくても**名前の規則だけ**でテストを見つけます
（[11.2.1](./11-next-steps.md#1121-テストpytest)）。

- `test_analyze_sales.py` … ファイル名が `test_` で始まる
- `def test_filter_sales_は期間の外を落とす():` … 関数名が `test_` で始まる

この規則から外れると、書いても実行されません。
`pytest` を実行して `collected 0 items` と出たときは、まずここを疑ってください。

`assert` は pytest の機能ではなく、**Python の文法**です。

```python
assert 1 + 1 == 2   # 何も起きない
assert 1 + 1 == 3   # AssertionError で止まる
```

pytest は、この `AssertionError` で止まったテストを「失敗」として数え、
**期待した値と実際の値の両方**を表示してくれます。

---

**問 11.2 の解答**

`analyze_sales.py` の最後が、次のようになっているためです。

```python
if __name__ == "__main__":
    main()
```

`main()` は、この `if` の中でしか呼ばれていません。
`if __name__ == "__main__":` は「このファイルを**直接実行したとき**だけ True になる」条件なので、
`import` されたときは False になり、`main()` は呼ばれません。

**解説**

[6.4.1](./06-modules.md#641-if-__name__--__main__-の意味) で「なぜこの1行を書くのか」を学びましたが、
その効き目が実際に必要になるのが、この場面です。

もしこの `if` がなく、ファイルの末尾に `main()` とだけ書いてあったら、
テストを実行するたびに次のことが起きます。

- `data/sales.csv` を読み込む
- 外部 API に通信しに行く（オフラインだと、そのぶん待たされる）
- `out/` に CSV が書き出される

**テストのつもりが、本番の処理を毎回動かしていた**ことになります。

> **補足**
> 「あとで `import` するかどうか分からないから、とりあえず書いておく」という説明を
> 6.4.1 ではしました。テストを書き始めると、この「あとで」がすぐ来ます。

---

**問 11.3 の解答**

**2・4・5** が、そのままではテストを書きにくい関数です。

| 番号 | 関数 | 書きにくい理由 |
|------|------|--------------|
| 2 | `fetch_max_temperatures` | **通信する。** 相手のサーバーの状態や、ネットワークの有無で結果が変わる |
| 4 | `make_output_path` | **実行した日で結果が変わる。** 期待する値を書いても、翌日には合わなくなる |
| 5 | `save_rows` | **ファイルを書き出す。** テストを動かすたびにファイルが増え、後片付けが必要になる |

1（`total_by_item`）と 3（`build_rows`）は、**値を渡すと値が返るだけ**なので、
期待値をそのまま書けます。

**解説**

書きにくさの正体は、**「同じものを渡しても、同じ結果になるとは限らない」**ことです。
通信・時刻・ファイルは、いずれも**自分のパソコンの外や、時間とともに変わるもの**に触れています。

ここで大事なのは、これが**関数の分け方の結果**だということです
（[10.6.1](./10-practice-data-script.md#1061-関数に分割する)）。
第10章では「1つの関数は1つのことをする」という理由で分けましたが、
その結果として、**外に用事がある処理が端に寄り、真ん中の計算だけを取り出せる形**になりました。

もし `load_sales` の中で読み込みも絞り込みも集計もしていたら、
集計だけを試すことはできませんでした。

> **補足**
> 2・4・5 もテストできないわけではありません。
> 通信を偽物に差し替える、一時的なディレクトリに書き出す、
> 日付を引数で受け取るようにする——といった方法があります。
> ただし、どれも道具や書き換えが要るので、**まずは1と3のような関数から**書いてください。

---

**問 11.4 の解答**

`pandas.read_csv` が、**列ごとに型を推測して変換してくれる**ためです。

**解説**

[7.4.1](./07-files-and-exceptions.md#741-csv-モジュール) で学んだとおり、`csv.DictReader` が返す値は**すべて文字列**です。
そのため第10章では、`Sale` を作るときに `int(row["quantity"])` と変換していました。

`pandas.read_csv` は、読み込むときに列全体を見て
「この列は全部数字だから整数として持とう」と判断します。
だから `df["quantity"] * df["unit_price"]` がそのまま掛け算になります
（[11.2.2](./11-next-steps.md#1122-データ分析pandas)）。

> **注意**
> 便利な反面、**推測が外れることもあります。**
> 数字だけの商品コード（`0123`）が整数と判断され、先頭の `0` が消える——というのが典型例です。
> 「勝手に変換される」ことを知らずに使うと、原因の分からない不一致に悩むことになります。

---

**問 11.5 の解答**

自動実行のときは、**有効化のコマンドを打つ人がいない**ためです。

**解説**

`.venv\Scripts\Activate.ps1` や `source .venv/bin/activate` は、
**あなたがターミナルで打つ前提**のコマンドです（[1.5.3](./01-environment.md#153-有効化する)）。
タスクスケジューラや cron は、ターミナルを開いて有効化してくれるわけではありません。

そもそも「有効化」がやっているのは、
**`python` と打ったときに `.venv` の中の Python が使われるようにする**ことです。
だったら、最初から `.venv` の中の Python を直接指せば、同じ結果になります。

```text
C:\Users\taro\sales-analyzer\.venv\Scripts\python.exe analyze_sales.py
```

**解説（もう1つの理由）**

有効化せずに `python analyze_sales.py` と登録してしまうと、
**OS に最初から入っている Python** や、別の場所の Python が使われることがあります。
その Python には `requests` が入っていないので、次のエラーで止まります。

```text
ModuleNotFoundError: No module named 'requests'
```

**自動実行が「なぜか動かない」原因の、いちばん多いパターン**です
（[11.2.3](./11-next-steps.md#1123-自動化スクリプト)）。

---

**問 11.6 の解答**

ファイルの移動は**取り消せない**ため、実行する前に
「何をするつもりなのか」を確かめる必要があるからです。
このやり方を **DRY RUN**（ドライラン）と呼びます。

**解説**

集計スクリプトなら、間違えても数字が合わないだけで、もう一度実行すれば済みます。
しかし、ファイルを動かすプログラムが間違っていた場合、
**動いてしまったあとに気づいても元に戻せません。**

DRY RUN は、その差を埋めるための習慣です
（[11.2.3](./11-next-steps.md#1123-自動化スクリプト)）。

- まず、`plan_moves` が返した計画を**表示するだけ**にする
- 表示された内容に「消えたら困るもの」が混ざっていないか、目で確認する
- 問題なければ、`APPLY = True`（演習 11.3 のあとは `--apply`）で実行する

**「計画を作る関数」と「実行する関数」を分けてある**ことが、これを可能にしています。
`plan_moves` と `apply_moves` を1つの関数にまとめてしまうと、
「表示だけ」ができなくなります。

---

### 演習

### 演習 11.1 の解答

`sales-analyzer/test_analyze_sales.py`（先頭の `import` と、末尾に追加した3件）

```python
from analyze_sales import Sale, filter_sales, total_by_date, total_by_item
```

```python
def test_total_by_date_は日付ごとに合計する() -> None:
    assert total_by_date(make_sales()) == {
        "2024-07-01": 700,
        "2024-07-02": 2750,
    }


def test_total_by_date_は空のリストなら空の辞書を返す() -> None:
    assert total_by_date([]) == {}


def test_total_by_date_は1件だけでも辞書を返す() -> None:
    sale = Sale(
        date="2024-07-01",
        shop="本店",
        item="かき氷",
        quantity=1,
        unit_price=450,
    )
    assert total_by_date([sale]) == {"2024-07-01": 450}
```

```text
実行結果:
============================= test session starts ==============================
platform win32 -- Python 3.13.1, pytest-9.1.1, pluggy-1.6.0
rootdir: C:\Users\taro\sales-analyzer
collected 8 items

test_analyze_sales.py ........                                           [100%]

============================== 8 passed in 0.11s ===============================
```

**解説**

**期待する金額の出し方**

`make_sales()` の3件は、次のようになっています。

| 日付 | 商品 | 数量 | 単価 | 小計 |
|------|------|------|------|------|
| 2024-07-01 | ソフトクリーム | 2 | 350 | 700 |
| 2024-07-02 | かき氷 | 3 | 450 | 1350 |
| 2024-07-02 | ソフトクリーム | 4 | 350 | 1400 |

`2024-07-02` は2件あるので、`1350 + 1400 = 2750` です。
**テスト用のデータを小さく作ってあるから、期待値を手で計算できます。**
本物の `data/sales.csv`（36件）を読ませていたら、この計算はできません。

**空のリストのテストが重要な理由**

`total_by_date([])` は、`for` が1回も回らないまま `dict(totals)` に進み、`{}` が返ります。

このテストは「いまも壊れていないこと」を確かめるために置きます。
たとえば、あとから「最初の日付を取り出しておこう」と考えて
`first = sales[0].date` のような行を足すと、空のリストで `IndexError` になります。
**そのとき、このテストが赤くなって教えてくれます。**

**わざと壊したときの結果**

`+=` を `=` に変えると、同じ日付の2件目が**足されずに上書き**されます。

```text
実行結果:
E       AssertionError: assert {'2024-07-01'... '2024-07-02': 1400} == {'2024-07-01'... '2024-07-02': 2750}
```

`2024-07-02` が `2750` ではなく `1400`（最後の1件だけ）になっています。

> **よくある間違い**
> `import` に `total_by_date` を足し忘れると、次のエラーで止まります。
>
> ```text
> NameError: name 'total_by_date' is not defined
> ```
>
> テストファイルも普通の Python のファイルなので、
> **使うものは import する**という決まりは同じです（[6.2.2](./06-modules.md#622-from--import-)）。

---

### 演習 11.2 の解答

`sales-analyzer/sales_pandas.py`

```python
"""本店の商品別売上を、金額の大きい順に表示する。"""

import pandas as pd

df = pd.read_csv("data/sales.csv")
df["subtotal"] = df["quantity"] * df["unit_price"]

honten = df[df["shop"] == "本店"]
item_totals = honten.groupby("item")["subtotal"].sum().sort_values(ascending=False)

print(item_totals)
print(f"合計: {item_totals.sum():,}円")
```

```text
実行結果:
item
ソフトクリーム    157500
かき氷        134100
ホットコーヒー     41700
Name: subtotal, dtype: int64
合計: 333,300円
```

**解説**

**1行の中で、4つのことが順番に起きています**

```python
honten.groupby("item")["subtotal"].sum().sort_values(ascending=False)
```

| 書いたもの | やっていること |
|-----------|-------------|
| `honten` | 本店の行だけに絞った表 |
| `.groupby("item")` | `item` が同じ行どうしをまとめる |
| `["subtotal"]` | まとめた中の `subtotal` 列を使う |
| `.sum()` | 合計する |
| `.sort_values(ascending=False)` | 値の大きい順に並べ替える |

左から右へ、**前の結果に次の処理をつなげていく**書き方です。
一度に読みにくければ、途中で変数に入れて分けても構いません。

```python
grouped = honten.groupby("item")["subtotal"].sum()
item_totals = grouped.sort_values(ascending=False)
```

**`ascending=False`**

`sort_values()` は、そのままだと**小さい順**（昇順）に並べます。
`ascending`（アセンディング。昇順）を `False` にすると、大きい順になります。

これは [4.1.5](./04-data-structures.md#415-並べ替えsort--sorted) の
`sorted(..., reverse=True)` にあたるものです。
**同じ「大きい順」でも、道具ごとに引数の名前が違います。**
`sorted` は `reverse=True`、pandas は `ascending=False` です。

**合計の確認**

`item_totals.sum()` は、並べ替えた3つの値をすべて足します。

```text
157500 + 134100 + 41700 = 333300
```

第10章の演習 10.1 で求めた本店の売上と一致しました。
**同じデータを、別の道具で計算して同じ答えになる**ことを確かめられたので、
どちらの書き方も正しかったと分かります。

> **よくある間違い**
> 絞り込みを `honten = df["shop"] == "本店"` と書いてしまうと、
> `True` / `False` が並んだ Series が入ってしまいます。
> **`df[...]` で囲んで初めて、行の絞り込みになります**（[11.2.2](./11-next-steps.md#1122-データ分析pandas)）。

> **別解：金額に桁区切りを付けて1件ずつ表示する**
>
> ```python
> for item, amount in item_totals.items():
>     print(f"{item}: {amount:,}円")
> ```
>
> ```text
> 実行結果:
> ソフトクリーム: 157,500円
> かき氷: 134,100円
> ホットコーヒー: 41,700円
> ```
>
> `items()` で「名前と値」を1組ずつ取り出せるのは、辞書と同じ形です
> （[4.3.4](./04-data-structures.md#434-キー値両方を回す)）。
> 第10章の `main` の表示と、見た目をそろえたいときはこちらを使ってください。

---

### 演習 11.3 の解答

`sales-analyzer/organize_files.py`

```python
"""指定したディレクトリのファイルを、拡張子ごとのディレクトリに仕分ける。"""

import argparse
from pathlib import Path


def plan_moves(target: Path) -> list[tuple[Path, Path]]:
    """(移動元, 移動先) の組を、名前順に並べて返す。"""
    moves: list[tuple[Path, Path]] = []
    for item in sorted(target.iterdir()):
        if not item.is_file():
            continue
        if item.suffix == "":
            continue
        folder = item.suffix.lstrip(".").lower()
        moves.append((item, target / folder / item.name))
    return moves


def apply_moves(moves: list[tuple[Path, Path]]) -> None:
    """組のとおりにファイルを移動する。"""
    for src, dst in moves:
        if dst.exists():
            print(f"移動先にすでにあるので飛ばします: {dst}")
            continue
        dst.parent.mkdir(parents=True, exist_ok=True)
        src.rename(dst)
        print(f"移動しました: {src.name} -> {dst.parent.name}/")


def parse_args() -> argparse.Namespace:
    """コマンドラインの引数を読み取って返す。"""
    parser = argparse.ArgumentParser(
        description="ディレクトリの中のファイルを、拡張子ごとに仕分けます。",
        epilog="例: python organize_files.py --dir demo_downloads --apply",
    )
    parser.add_argument(
        "--dir",
        default=str(Path.home() / "Downloads"),
        metavar="パス",
        help="仕分けるディレクトリ（既定: ホームの Downloads）",
    )
    parser.add_argument(
        "--apply",
        action="store_true",
        help="実際に移動する（付けないと計画の表示だけ）",
    )
    return parser.parse_args()


def main() -> None:
    """仕分けの計画を表示し、--apply が付いたときだけ実際に移動する。"""
    args = parse_args()
    target = Path(args.dir)
    if not target.is_dir():
        print(f"ディレクトリが見つかりません: {target}")
        return

    moves = plan_moves(target)
    for src, dst in moves:
        print(f"{src.name} -> {dst.parent.name}/")
    print(f"対象は {len(moves)} 件です")

    if args.apply:
        apply_moves(moves)
    else:
        print("--apply が付いていないので、まだ何も移動していません")


if __name__ == "__main__":
    main()
```

```text
実行結果（python organize_files.py --dir demo_downloads）:
データ.csv -> csv/
メモ.txt -> txt/
レシピ.pdf -> pdf/
写真1.JPG -> jpg/
写真2.jpg -> jpg/
請求書.pdf -> pdf/
対象は 6 件です
--apply が付いていないので、まだ何も移動していません
```

```text
実行結果（python organize_files.py --help）:
usage: organize_files.py [-h] [--dir パス] [--apply]

ディレクトリの中のファイルを、拡張子ごとに仕分けます。

options:
  -h, --help  show this help message and exit
  --dir パス    仕分けるディレクトリ（既定: ホームの Downloads）
  --apply     実際に移動する（付けないと計画の表示だけ）

例: python organize_files.py --dir demo_downloads --apply
```

**解説**

**`plan_moves` と `apply_moves` は、1行も変えていません**

変えたのは「どこから設定を受け取るか」だけです。
[10.6.1](./10-practice-data-script.md#1061-関数に分割する) で見た
「`main` だけが全体を知っていて、他の関数はお互いを知らない」形になっているので、
入り口だけを差し替えられました。

**`default=str(Path.home() / "Downloads")`**

`Path.home() / "Downloads"` は `Path` ですが、`argparse` に渡す既定値は
**文字列にそろえておく**ほうが安全です。
`--dir` で指定されたときも文字列で届くので、
`main` の中で `Path(args.dir)` と1か所だけ変換すれば、以降は `Path` として扱えます。

**受け取る場所で1回だけ変換する**——第10章の `type=int` と同じ考え方です
（[10.5.1](./10-practice-data-script.md#1051-argparse-の基本)）。

**`if not target.is_dir():`**

指定されたディレクトリがないときに、`iterdir()` を呼ぶと
`FileNotFoundError` でトレースバックが出ます。
ここは「そのあとどうするか」を決められる場面
（[7.6.3](./07-files-and-exceptions.md#763-どこで例外を捕まえるべきか)）なので、
メッセージを出して `return` で終わります。

`is_dir()` は `is_file()` の仲間で、「それがディレクトリか」を調べます
（[7.3.3](./07-files-and-exceptions.md#733-存在確認作成一覧)）。

**`action="store_true"` を選ぶ理由**

`--apply` は、値を取らない「付けるか付けないか」のオプションです。
第10章の `--no-weather` とまったく同じ形になります
（[10.5.2](./10-practice-data-script.md#1052-引数を受け取る)）。

**既定を「安全な側」にしてあることが大事です。**
`--apply` を付けなければ何も動かないので、うっかり実行しても事故になりません。
逆に `--dry-run` というオプションにして、既定を「移動する」にしてしまうと、
**忘れた瞬間にファイルが動きます。**

> **よくある間違い**
> `args.dir` をそのまま `plan_moves(args.dir)` に渡すと、
> mypy が次のように報告します。
>
> ```text
> error: Argument 1 to "plan_moves" has incompatible type "str"; expected "Path"  [arg-type]
> ```
>
> `plan_moves` は `Path` を受け取る関数として書いてあるためです
> （[9.1.2](./09-typing-and-tools.md#912-変数と引数に型を書く)）。
> **動かす前に、この食い違いを見つけられる**のが型チェックの効き目です。

> **補足：もう一歩進めるなら**
> `--dir` を複数回指定できるようにする、
> 拡張子ごとのディレクトリ名を「画像」「書類」のようにまとめる、
> 移動したファイルの一覧をログに書き出す——などが考えられます。
> ただし、**どれもファイルを動かす処理なので、必ず DRY RUN で確かめてから**にしてください。
