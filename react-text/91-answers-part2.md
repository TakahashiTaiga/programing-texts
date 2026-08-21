---
title: "解答編 その2（第6章〜第10章）"
---

# 解答編 その2（第6章〜第10章）

> 🚧 **執筆中です。**
> 第6章以降の本文の執筆に合わせて追記していきます。

第1章〜第5章の解答は [解答編 その1](./90-answers-part1.md) にあります。

---

## 第6章

> 🚧 執筆中です。

## 第7章

### 理解度チェック

**問 7.1 の解答**

- A = **props**
- B = **state（状態）**
- C = **`set○○`（`useState` が返す更新用の関数。例：`setCount`）**

**解説**

2つの違いは、**誰が持っているか**です。

| | props | state |
|--|-------|-------|
| 持ち主 | **親**コンポーネント | **そのコンポーネント自身** |
| 渡し方 | `<Card name="りんご" />` | `useState` で作る |
| 書き換え | **できない**（7.1.6） | `set○○` を通してのみ変えられる（7.2.3） |
| 変わったとき | 親が渡す値を変えれば、子も作り直される | そのコンポーネントが再レンダリングされる |

「値が変わる場所を1箇所に限る」という考え方が、React の設計の中心にあります。
props を書き換えられないのも、state を `set○○` 経由でしか変えられないのも、同じ理由です。

---

**問 7.2 の解答**

**理由**

`count` が**普通の変数**だからです。理由は2つあります（7.2.1）。

1. `count = count + 1` という代入は React に見えないため、**画面を作り直すきっかけがない**
2. 仮に作り直されたとしても、`let count = 0` の行がもう一度実行され、**0 に戻ってしまう**

**直したコード**

```jsx
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)

  function handleClick() {
    setCount((prev) => prev + 1)
  }

  return (
    <div>
      <p>{count}</p>
      <button onClick={handleClick}>増やす</button>
    </div>
  )
}

export default Counter
```

**解説**

`useState(0)` は「初期値 0 の state を作ってください」という依頼で、
戻ってくるのは `[いまの値, 更新用の関数]` という**2要素の配列**です（7.2.2）。
それを分割代入（5.4.1）で `count` と `setCount` に分けて受け取っています。

`setCount((prev) => prev + 1)` と関数形式にしているのは、**前の値をもとに更新している**からです（7.2.5）。
`setCount(count + 1)` でもこの問題は動きますが、
「2つ増やす」のように連続して呼ぶ場合に間違いのもとになるため、
このテキストでは前の値を使う更新は常に関数形式で書きます。

> **よくある間違い**
> `import { useState } from 'react'` の行を忘れると、次のエラーになります。
>
> ```text
> Uncaught ReferenceError: useState is not defined
> ```

---

**問 7.3 の解答**

**出るエラー**

```text
Uncaught Error: Too many re-renders. React limits the number of renders to prevent an infinite loop.
```

**原因**

`onClick={handleDelete(item.id)}` は、`()` が付いているため
**画面を作っている最中に `handleDelete` が実行されてしまいます**（7.3.2）。

`handleDelete` の中で `set○○` が呼ばれると state が変わり、再レンダリングが起き、
そこでまた `handleDelete` が実行され……という無限ループになります。

**直したコード**

```jsx
<button onClick={() => handleDelete(item.id)}>削除</button>
```

**解説**

`() => handleDelete(item.id)` は「**呼ばれたら** `handleDelete(item.id)` を実行する関数」です。
渡しているのは関数そのものなので、クリックされるまで実行されません（7.3.3）。

| 書き方 | いつ実行されるか |
|--------|----------------|
| `onClick={handleDelete}` | クリック時（ただし引数を渡せない） |
| `onClick={handleDelete(item.id)}` | **画面を作るとき**（間違い） |
| `onClick={() => handleDelete(item.id)}` | クリック時（引数も渡せる） |

**別の症状**：`handleDelete` が state を変えない処理（`console.log` だけなど）だった場合、
エラーは出ませんが、**クリックしていないのに処理が動く**という形で現れます。
「ページを開いただけで実行されている」と感じたら、`()` を疑ってください。

---

**問 7.4 の解答**

**画面が更新されるのは B です。**

**A が更新されない理由**

`items.push('ぶどう')` は、**いまある配列そのものを書き換えています。**
配列を入れている変数が持っているのは中身ではなく**置き場所**なので（5.4.3）、
`push` をしても `items` が指す場所は変わりません。

そのため `setItems(items)` は React から見ると
**「前と同じものが渡ってきた＝変わっていない」**となり、再レンダリングが起きません（7.2.4）。

**解説**

B の `[...items, 'ぶどう']` は、**元の配列の中身をコピーした新しい配列**を作っています（5.4.2）。
別のものが渡ってくるので、React は「変わった」と判断します。

配列やオブジェクトの state は、**必ず新しいものを作って渡す**のが原則です。

| やりたいこと | ✕ | ○ |
|------------|---|---|
| 追加 | `items.push(x)` | `setItems([...items, x])` |
| 削除 | `items.splice(i, 1)` | `setItems(items.filter(...))` |
| 変更 | `items[0].done = true` | `setItems(items.map(...))` |

> **注意**
> A はエラーが出ません。「押しても何も起きない」という症状だけが出ます。
> **画面が更新されないときは、まず `push` / `splice` / 直接代入を探してください。**

---

**問 7.5 の解答**

**画面には `0` という数字が表示されます。**

**理由**

`memos.length` が `0` のとき、`0 && <p>...</p>` は**左側の `0` をそのまま返します**（7.5.1）。
そして `0` は、JSX の波かっこに入れると**表示される値**です（6.4.3 の表）。
表示されないのは `false` / `null` / `undefined` の3つだけです。

**直し方（どちらか1つ）**

```jsx
{/* 直し方1：比較して真偽値にする */}
{memos.length > 0 && <p>メモが {memos.length} 件あります</p>}
```

```jsx
{/* 直し方2：三項演算子にして、0 件のときの表示も書く */}
{memos.length > 0 ? (
  <p>メモが {memos.length} 件あります</p>
) : (
  <p>メモはまだありません。</p>
)}
```

**解説**

`0 > 0` は `false` になるので、直し方1では何も表示されません。

実際のアプリでは**直し方2のほうが親切**です。
一覧が空のときに画面へ何も出ないと、読み込み中なのか、壊れているのか、
本当に0件なのかが、利用者に伝わらないためです。

**覚え方**：`&&` の左側に数値を書いたら、必ず `> 0` を付ける。

---

**問 7.6 の解答**

次のうち2つが書けていれば正解です（7.4.3）。

1. **一覧の途中の要素を削除する場面**
   削除すると、それ以降の要素の index が1つずつ前にずれ、`key` の対応が崩れます
2. **一覧を並べ替える場面**
   位置が変わると index も変わるため、React が「同じもの」と認識できません
3. **先頭に要素を追加する場面**
   すべての要素の index がずれます

**解説**

表示するだけの一覧なら、ずれても見た目の結果は同じになることが多く、問題が表に出ません。
**壊れるのは、各行が自分で状態を持っているとき**です。

たとえば行ごとにチェックボックスがある一覧で先頭を削除すると、
チェックの状態が**別の行に残っている**ように見えます。
チェックの状態は `key` で対応付けられた要素が持っているためです。

エラーも警告も出ないため、原因にたどり着くのが非常に難しい種類の不具合です。
**一覧を作るときは、最初からデータに `id` を持たせておいてください。**

なお、次の条件を**すべて**満たすなら `key={index}` でも構いません。

- 追加・削除・並べ替えをしない
- 各行が入力欄やチェックボックスを持たない

---

**問 7.7 の解答**

**`value` を指定しているのに `onChange` がないため、state が更新されず、表示も変わらないから**です（7.6.2）。

**解説**

`value={name}` と書いた時点で、この入力欄に表示される文字は
**常に state の `name`** になります（制御コンポーネント）。

キーを打っても `name` を更新する処理がないので、表示は空のままです。
ブラウザのコンソールにも警告が出ています。

```text
Warning: You provided a `value` prop to a form field without an `onChange` handler.
```

**直したコード**

```jsx
const [name, setName] = useState('')

return (
  <input
    type="text"
    value={name}
    onChange={(event) => setName(event.target.value)}
  />
)
```

**`value` と `onChange` は必ずセット**にしてください。

---

### 演習

### 演習 7.1 の解答

`src/components/ProfileCard.jsx`

```jsx
function ProfileCard({ name, age, hobby = '未設定' }) {
  // age から計算した値。props そのものは書き換えない（7.1.6）
  const nextAge = age + 1

  return (
    <div className="profile-card">
      <h2>{name}</h2>
      <p>年齢: {age}歳（来年は {nextAge} 歳です）</p>
      <p>趣味: {hobby}</p>
    </div>
  )
}

export default ProfileCard
```

`src/App.jsx`

```jsx
import ProfileCard from './components/ProfileCard.jsx'
import './App.css'

function App() {
  return (
    <div className="app">
      <h1>メンバー紹介</h1>
      <ProfileCard name="たろう" age={20} hobby="読書" />
      <ProfileCard name="はなこ" age={25} hobby="登山" />
      <ProfileCard name="じろう" age={32} />
    </div>
  )
}

export default App
```

`src/App.css`（追記）

```css
.profile-card {
  border: 1px solid #dddddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
}

.profile-card h2 {
  margin: 0 0 8px;
  font-size: 18px;
}

.profile-card p {
  margin: 4px 0;
}
```

```text
表示される内容:
メンバー紹介
┌────────────────────────────┐
│ たろう                      │
│ 年齢: 20歳（来年は 21 歳です）│
│ 趣味: 読書                  │
└────────────────────────────┘
┌────────────────────────────┐
│ はなこ                      │
│ 年齢: 25歳（来年は 26 歳です）│
│ 趣味: 登山                  │
└────────────────────────────┘
┌────────────────────────────┐
│ じろう                      │
│ 年齢: 32歳（来年は 33 歳です）│
│ 趣味: 未設定                │  ← hobby を渡していない
└────────────────────────────┘
```

**解説**

ポイントは3つです。

1. **分割代入で受け取る**（7.1.3）
   `function ProfileCard({ name, age, hobby })` と書くことで、
   関数の1行目を見ただけで「このコンポーネントは3つの値を受け取る」とわかります。

2. **デフォルト値**（7.1.3）
   `hobby = '未設定'` と書いておくと、渡されなかったときにこの値が使われます。
   デフォルト値がないと、`hobby` は `undefined` になり、**何も表示されません**（6.4.3 の表）。
   エラーにならないぶん、「なぜか空欄になる」という形で気づきにくくなります。

3. **数値は波かっこで渡す**（7.1.4）
   `age={20}` と書いているので、`age` は数値の 20 です。
   もし `age="20"` と書くと文字列になり、`age + 1` が **`'201'`** になります。

```js
20 + 1     // 21
'20' + 1   // '201'  ← 文字列の連結（4.3.8）
```

> **別解**
> 計算をその場に書いても構いません。
>
> ```jsx
> <p>年齢: {age}歳（来年は {age + 1} 歳です）</p>
> ```
>
> 変数に入れるか、その場で書くかは好みの範囲です。
> 同じ計算を2箇所以上で使うなら、変数に入れるほうが読みやすくなります。

---

### 演習 7.2 の解答

`src/components/StockCounter.jsx`

```jsx
import { useState } from 'react'

function StockCounter() {
  const [count, setCount] = useState(0)

  function handleIncrement() {
    setCount((prev) => prev + 1)
  }

  function handleDecrement() {
    // 0 のときは減らさず、前の値をそのまま返す（7.2.5）
    setCount((prev) => (prev > 0 ? prev - 1 : 0))
  }

  function handleReset() {
    // 前の値を使わないので、値をそのまま渡してよい
    setCount(0)
  }

  return (
    <div className="panel">
      <h2 className="panel-title">在庫カウンター</h2>
      <p>いまの数: {count}</p>

      {count >= 10 && <p className="note">在庫が多めです</p>}

      <button onClick={handleIncrement}>1つ増やす</button>
      <button onClick={handleDecrement}>1つ減らす</button>
      <button onClick={handleReset}>リセット</button>
    </div>
  )
}

export default StockCounter
```

`src/App.jsx`

```jsx
import StockCounter from './components/StockCounter.jsx'
import './App.css'

function App() {
  return (
    <div className="app">
      <StockCounter />
    </div>
  )
}

export default App
```

```text
表示される内容（最初）:
在庫カウンター
いまの数: 0
[1つ増やす] [1つ減らす] [リセット]

0 のときに [1つ減らす] を押しても → いまの数: 0

10 回押したあと:
いまの数: 10
在庫が多めです
```

**解説**

**1. 0 より下げない書き方**

```jsx
setCount((prev) => (prev > 0 ? prev - 1 : 0))
```

`set○○` に渡した関数が**返した値**が、次の state になります（7.2.5）。
`prev` が 0 以下なら 0 を返す、つまり「変えない」という書き方です。

**2. なぜ関数形式にするのか**

次のように書いても、ボタンを1回ずつ押すぶんには動きます。

```jsx
setCount(count > 0 ? count - 1 : 0)
```

ただし `count` は**その関数が呼ばれた時点の値**なので（7.2.3）、
連続して更新する処理を書いたときに、意図しない結果になります。
**前の値を使う更新は、常に関数形式**にしておくのが安全です。

**3. 条件付きの表示**

```jsx
{count >= 10 && <p className="note">在庫が多めです</p>}
```

`count >= 10` は**比較の結果（真偽値）**なので、7.5.4 の「0 が表示される罠」は起きません。

一方、次のように書くと 0 のときに `0` が表示されてしまいます。

```jsx
{/* ✕ count が 0 のとき、画面に 0 と出る */}
{count && <p>在庫があります</p>}
```

> **よくある間違い**
> `<button onClick={handleDecrement()}>` と `()` を付けると、
> ページを開いた瞬間に処理が走り、`Too many re-renders` になります（7.3.2）。

---

### 演習 7.3 の解答

`src/components/ShoppingApp.jsx`

```jsx
import { useState } from 'react'

function ShoppingApp() {
  const [items, setItems] = useState([])
  const [text, setText] = useState('')

  function handleChange(event) {
    setText(event.target.value)
  }

  function handleSubmit(event) {
    // これが無いとページが再読み込みされ、入力も一覧も消える（7.6.5）
    event.preventDefault()

    // 空欄やスペースだけのときは追加しない
    if (text.trim() === '') {
      return
    }

    const newItem = { id: Date.now(), name: text }
    setItems((prev) => [...prev, newItem])

    // 入力欄を空に戻す（7.6.2）
    setText('')
  }

  function handleDelete(id) {
    setItems((prev) => prev.filter((item) => item.id !== id))
  }

  return (
    <div className="panel">
      <h2 className="panel-title">買い物リスト</h2>

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={text}
          onChange={handleChange}
          placeholder="品物を入力"
        />
        <button type="submit">追加</button>
      </form>

      {items.length > 0 ? (
        <ul className="memo-list">
          {items.map((item) => (
            <li key={item.id} className="memo-item">
              <span>{item.name}</span>
              <button onClick={() => handleDelete(item.id)}>削除</button>
            </li>
          ))}
        </ul>
      ) : (
        <p>まだ何もありません。</p>
      )}

      <p>{items.length}件</p>
    </div>
  )
}

export default ShoppingApp
```

`src/App.css`（追記。7.4.4 で追記済みなら不要です）

```css
.memo-list {
  list-style: none;
  padding: 0;
}

.memo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #eeeeee;
  padding: 8px 0;
}
```

```text
表示される内容（最初）:
買い物リスト
[品物を入力          ] [追加]
まだ何もありません。
0件

「牛乳」と入力して [追加] を押すと:
[                    ] [追加]   ← 入力欄が空に戻る
牛乳                  [削除]
1件
```

**解説**

この演習は、7.6.5 の「メモ帳」と同じ形です。**確認すべき点を1つずつ見ていきます。**

| 完成条件 | 対応する書き方 | 本文 |
|---------|--------------|------|
| 制御コンポーネント | `value={text}` と `onChange={handleChange}` | 7.6.2 |
| 再読み込みされない | `event.preventDefault()` | 7.6.5 |
| 入力欄が空に戻る | `setText('')` | 7.6.2 |
| 空欄では追加しない | `if (text.trim() === '') return` | 7.6.5 |
| `key` に id を使う | `id: Date.now()` と `key={item.id}` | 7.4.4、7.4.2 |
| その行だけ消える | `filter((item) => item.id !== id)` | 7.4.4、5.3.2 |
| 件数が変わる | `{items.length}件` | 7.4.4 |
| 0 件のときの案内 | `items.length > 0 ? ... : ...` | 7.5.4 |

**なぜ `text.trim()` を使うのか**

`text === ''` だけの判定では、**スペースだけを入力された場合を防げません。**
`trim()` は前後の空白を取り除いた文字列を返すので、
スペースだけなら `''` になり、追加を止められます（7.6.5）。

> **別解：`<form>` を使わず、ボタンの `onClick` で追加する**
>
> ```jsx
> <input type="text" value={text} onChange={handleChange} />
> <button onClick={handleAdd}>追加</button>
> ```
>
> この形でも動きますが、**Enter キーで追加できなくなります。**
> `<form>` を使うと Enter が送信として扱われるため、このテキストでは `<form>` を使います。

> **よくある間違い**
> `id` に `map` の index を使うと、削除したときに `key` がずれます（7.4.3）。
> 一覧を削除できるアプリでは、**必ずデータ側に id を持たせてください。**

---

### 演習 7.4 の解答

`src/components/ShopItem.jsx`

```jsx
function ShopItem({ name, price, inStock }) {
  return (
    <li className={inStock ? 'shop-item' : 'shop-item sold-out'}>
      <span>{name}</span>
      <span>{price}円</span>
      {inStock ? <span>在庫あり</span> : <span>在庫切れ</span>}
    </li>
  )
}

export default ShopItem
```

`src/components/ShopPage.jsx`

```jsx
import { useState } from 'react'
import ShopItem from './ShopItem.jsx'

// コンポーネントの外に置く（再レンダリングのたびに作り直さないため）
const products = [
  { id: 1, name: 'りんごジュース', price: 200, inStock: true },
  { id: 2, name: 'みかんジュース', price: 180, inStock: true },
  { id: 3, name: 'ぶどうジュース', price: 260, inStock: false },
  { id: 4, name: 'ももジュース', price: 300, inStock: true },
  { id: 5, name: 'りんごゼリー', price: 150, inStock: false },
]

function ShopPage() {
  const [keyword, setKeyword] = useState('')
  const [onlyInStock, setOnlyInStock] = useState(false)

  // 表示するものは state にせず、そのつど計算する（7.6.2 の補足）
  const shown = products.filter(
    (product) =>
      product.name.includes(keyword) &&
      (onlyInStock === false || product.inStock)
  )

  return (
    <div className="panel">
      <h2 className="panel-title">商品一覧</h2>

      <p>
        <input
          type="text"
          value={keyword}
          onChange={(event) => setKeyword(event.target.value)}
          placeholder="商品名で検索"
        />
      </p>

      <p>
        <label>
          <input
            type="checkbox"
            checked={onlyInStock}
            onChange={(event) => setOnlyInStock(event.target.checked)}
          />
          在庫ありのみ表示
        </label>
      </p>

      <p>{shown.length}件表示中</p>

      {shown.length > 0 ? (
        <ul className="shop-list">
          {shown.map((product) => (
            <ShopItem
              key={product.id}
              name={product.name}
              price={product.price}
              inStock={product.inStock}
            />
          ))}
        </ul>
      ) : (
        <p>該当する商品がありません。</p>
      )}
    </div>
  )
}

export default ShopPage
```

`src/App.css`（追記）

```css
.shop-list {
  list-style: none;
  padding: 0;
}

.shop-item {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #eeeeee;
  padding: 8px 0;
}

.sold-out {
  color: #999999;
}
```

`src/App.jsx`

```jsx
import ShopPage from './components/ShopPage.jsx'
import './App.css'

function App() {
  return (
    <div className="app">
      <ShopPage />
    </div>
  )
}

export default App
```

```text
表示される内容（最初）:
商品一覧
[商品名で検索        ]
[ ] 在庫ありのみ表示
5件表示中
りんごジュース  200円  在庫あり
みかんジュース  180円  在庫あり
ぶどうジュース  260円  在庫切れ   ← グレー表示
ももジュース    300円  在庫あり
りんごゼリー    150円  在庫切れ   ← グレー表示

「りんご」と入力すると:
2件表示中
りんごジュース  200円  在庫あり
りんごゼリー    150円  在庫切れ

さらに「在庫ありのみ表示」にチェックを入れると:
1件表示中
りんごジュース  200円  在庫あり

「メロン」と入力すると:
0件表示中
該当する商品がありません。
```

**解説**

**1. state に持つのは2つだけ**

この画面で「利用者の操作によって変わるもの」は、**検索文字**と**チェックの状態**の2つだけです。

```jsx
const [keyword, setKeyword] = useState('')
const [onlyInStock, setOnlyInStock] = useState(false)
```

表示される商品リスト（`shown`）は、この2つと元データから**計算で出せます。**
計算で出せるものを state にすると、次の問題が起きます。

- 検索文字を変えたときに、`shown` も自分で更新する処理が必要になる（**更新の呼び忘れ**が復活する）
- 2つの state がずれた状態（検索文字は「りんご」なのに一覧は全件）が起こり得る

**「計算で出せるものは state にしない」**——これは第8章で扱う状態設計の中心にある考え方です。

**2. 2つの条件のつなぎ方**

```jsx
product.name.includes(keyword) && (onlyInStock === false || product.inStock)
```

| 部分 | 意味 |
|------|------|
| `product.name.includes(keyword)` | 商品名に検索文字が含まれている（空文字のときは全件が `true`） |
| `onlyInStock === false \|\| product.inStock` | チェックが入っていない、**または**在庫がある |

チェックが入っていないときは全商品を通したいので、`||`（または。4.3.7）でつないでいます。

**3. `products` をコンポーネントの外に置く理由**

`products` は変化しないデータなので、**関数の外**に書きます。
中に書くと、再レンダリングのたびに配列が作り直されます（7.2.3）。
このくらいの規模では体感できる差は出ませんが、
**「変わらないものは外」**と覚えておくと、第8章以降で役に立ちます。

> **別解：`filter` を2回に分ける**
>
> ```jsx
> const found = products.filter((product) => product.name.includes(keyword))
> const shown = onlyInStock ? found.filter((product) => product.inStock) : found
> ```
>
> 条件が増えて1行が読みにくくなったら、こちらのほうが読みやすくなります。
> どちらでも結果は同じです。

> **よくある間違い**
> `<ShopItem>` に `key` を付け忘れると、次の警告が出ます（7.4.2）。
>
> ```text
> Warning: Each child in a list should have a unique "key" prop.
> ```
>
> `key` は、`map` が返す**一番外側の要素**（ここでは `<ShopItem>`）に付けます。
> `ShopItem` の中の `<li>` に付けても、警告は消えません。

> **よくある間違い**
> チェックボックスに `value={onlyInStock}` と書くと、チェックが動きません（7.6.4）。
> チェックボックスは **`checked`** と `event.target.checked` を使います。

## 第8章

> 🚧 執筆中です。

## 第9章

> 🚧 執筆中です。

## 第10章

> 🚧 執筆中です。
