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
> react-text の演習 6.3 の解答を読みましたが、
> なぜ import のパスに ./ を付けなければならないのかが納得できません。
> ```

---

## 第6章

### 理解度チェック

**問 6.1 の解答**

- A = **手順**
- B = **対応**

**解説**

第5章の DOM 操作では、「追加したら一覧を書き換えて、件数も書き換えて……」と、
**画面をどう変えるかの手順**を1つずつ書いていました。これを**命令的**と呼びます（6.1.1）。

React では `<p>件数: {tasks.length}件</p>` のように、
**「このデータなら、こう表示される」という対応**だけを書きます。
データが変われば、その表示は React が作り直します。これを**宣言的**と呼びます（6.1.2）。

命令的なやり方の問題は、**操作の種類 × 表示の種類**の分だけ、
書き換え処理が増えていくことです。1つでも書き忘れると、画面とデータがずれます。

---

**問 6.2 の解答**

**理由：JSX が返せるのは1つの要素だけだから。**

```text
出るエラー:
Adjacent JSX elements must be wrapped in an enclosing tag.
```

**直し方1：`<div>` で囲む**

```jsx
function App() {
  return (
    <div>
      <h1>タイトル</h1>
      <p>本文</p>
    </div>
  )
}
```

**直し方2：フラグメントで囲む**

```jsx
function App() {
  return (
    <>
      <h1>タイトル</h1>
      <p>本文</p>
    </>
  )
}
```

**解説**

JSX は、実行前に**関数の呼び出し**へ変換されます（6.4.1 の補足）。
関数の戻り値は1つだけなので（4.6.2）、返す要素も1つでなければなりません。

2つの直し方の違いは、**実際の HTML に要素が出るかどうか**です（6.4.5）。

| 書き方 | `<div id="root">` の中に出るもの |
|--------|------------------------------|
| `<div>` で囲む | `<div><h1>…</h1><p>…</p></div>` |
| `<>` で囲む | `<h1>…</h1><p>…</p>` |

**CSS を当てたいなら `<div>`、ただ1つにまとめたいだけなら `<>`** を選びます。
迷ったら `<>` にしておくと、余計な要素が増えません。

---

**問 6.3 の解答**

**JSX では `class` ではなく `className` と書く必要があるため**（6.4.2 の違い2）。

**直したコード**

```jsx
<div className="app">
  <h1>見出し</h1>
</div>
```

**解説**

`class` は JavaScript が別の意味で使っている単語なので、JSX の属性名としては使えません。

やっかいなのは、**エラーで止まらない**ことです。
画面は表示されるのに CSS だけが効かないため、CSS ファイルのほうを疑って時間を溶かしがちです。

ブラウザのコンソール（1.6.4）には、次の警告が出ています。

```text
Warning: Invalid DOM property `class`. Did you mean `className`?
```

**「CSS が効かない」と思ったら、まずコンソールを見る**——第3章 3.7 で学んだ調べ方と同じです。

---

**問 6.4 の解答**

**書けないもの：2 と 4**

**解説**

波かっこの中に書けるのは、**最終的に1つの値になるもの（式）**だけです（6.4.3）。

| 選択肢 | 判定 | 理由 |
|--------|------|------|
| 1. `price * count` | ○ | 計算した結果という「値」になる |
| 2. `if (age >= 20) { '成人' }` | **×** | `if` 文は値にならない |
| 3. `age >= 20 ? '成人' : '未成年'` | ○ | 三項演算子は「値」になる（4.4.4） |
| 4. `const name = 'たろう'` | **×** | 変数宣言は値にならない |
| 5. `Math.floor(3.7)` | ○ | 関数の戻り値という「値」になる |

**間違えやすいのは 3 です。** 見た目は条件分岐ですが、三項演算子は**式**なので書けます。
JSX の中で条件によって表示を変えたいときに、`if` の代わりとして使うのはこのためです。

なお、4 のような変数宣言は、**`return` より前**に書けば問題ありません。

```jsx
function App() {
  const name = 'たろう'   // ここならOK

  return <p>{name}</p>
}
```

---

**問 6.5 の解答**

**理由：コンポーネント名が小文字で始まっているため。**
React は小文字で始まるタグを HTML のタグとして扱い、
`<productCard>` という HTML タグは存在しないので、何も表示されずに終わります。

**直したコード**

```jsx
function ProductCard() {
  return <div className="card">商品</div>
}

function App() {
  return (
    <div className="app">
      <ProductCard />
    </div>
  )
}
```

**解説**

React は、**JSX のタグ名の1文字目**だけを見て判断しています（6.5.5）。

| タグ | 解釈 |
|------|------|
| `<div>` `<p>` `<productCard>` | HTML のタグ |
| `<ProductCard>` `<Header>` | 自分で作ったコンポーネント |

**関数名とタグ名の両方**を大文字始まりに直す必要があります。
片方だけ直すと、今度は「そんな名前は定義されていない」というエラーになります。

```text
出るエラー（タグだけ直した場合）:
ProductCard is not defined
```

このミスも**エラーで止まりません。** 画面が真っ白になったら、
ブラウザのコンソールに次の警告が出ていないか確認してください。

```text
The tag <productCard> is unrecognized in this browser.
```

---

**問 6.6 の解答**

- **外側の `{ }`**：「ここに JavaScript の値を書きます」という印
- **内側の `{ }`**：オブジェクトそのもの

つまり `style` には**オブジェクトを渡している**ので、波かっこが2重になります。

**解説**

分けて書くと、2重になっている理由がはっきりします（6.4.4）。

```jsx
const myStyle = { color: 'red', fontSize: '20px' }

<p style={myStyle}>赤い文字</p>
```

この `myStyle` を、変数を経由せずその場に書いたものが `style={{ color: 'red' }}` です。

「2重にする」という**丸暗記ではなく、「オブジェクトを渡している」と理解**しておいてください。
そう理解していれば、次のようなミスも自分で直せます。

```jsx
// エラー：波かっこが1つ足りない（オブジェクトになっていない）
<p style={ color: 'red' }>赤い文字</p>
```

なお、プロパティ名が `font-size` ではなく `fontSize` なのは、
JavaScript では `-` が引き算の記号になってしまうためです（6.4.4）。

---

**問 6.7 の解答**

**`<div>` は実際の HTML に要素として出るが、`<>`（フラグメント）は出ない。**

**解説**

どちらも「複数の要素を1つにまとめる」役割は同じです（6.4.5）。
違いは、**まとめるための入れ物が、画面上に残るかどうか**です。

```jsx
// これを書くと
<div>
  <h1>タイトル</h1>
  <p>本文</p>
</div>
```

```html
<!-- 実際の HTML -->
<div>
  <h1>タイトル</h1>
  <p>本文</p>
</div>
```

```jsx
// これを書くと
<>
  <h1>タイトル</h1>
  <p>本文</p>
</>
```

```html
<!-- 実際の HTML（div が無い） -->
<h1>タイトル</h1>
<p>本文</p>
```

**使い分けの基準**

- そのかたまりに CSS を当てたい → `<div className="...">`
- 1つにまとめたいだけ → `<>`

Flexbox（3.5.2）や Grid（3.5.6）では、**親要素の直接の子**が並びの対象になります。
そこに意図しない `<div>` が挟まるとレイアウトが崩れるため、
`<>` が必要になる場面が出てきます。

---

### 演習

### 演習 6.1 の解答

`src/App.jsx`

```jsx
import './App.css'

function App() {
  const name = 'やまだ たろう'
  const age = 20
  const hobby = '読書'

  return (
    <div className="app">
      <h1>{name}</h1>
      <p>年齢: {age}歳</p>
      <p>趣味: {hobby}</p>
      <p>来年は {age + 1} 歳になります。</p>
    </div>
  )
}

export default App
```

```text
表示される内容:
やまだ たろう
年齢: 20歳
趣味: 読書
来年は 21 歳になります。
```

**解説**

**ポイント1：変数は `return` より前に書く**

```jsx
function App() {
  const name = 'やまだ たろう'   // ここ

  return (
    ...
  )
}
```

`App` は**ただの関数**です（6.3.3）。
関数の中の書き方は、第4章で学んだものがそのまま使えます。
波かっこの中には変数宣言を書けない（問 6.4）ので、`return` より前に置きます。

**ポイント2：来年の年齢は「計算して」表示する**

```jsx
<p>来年は {age + 1} 歳になります。</p>
```

波かっこの中には、**計算式**を書けます（6.4.3）。
`age` が `20` なので、`age + 1` は `21` になります。

**よくある間違い：`age` を書き換えてしまう**

```jsx
// これはやらない
const age = 20
const nextAge = age + 1   // これは問題ない

// これは問題（age 自体が変わってしまう）
let age = 20
age = age + 1
```

「年齢: 20歳」と「来年は 21 歳」を**両方**表示する必要があるので、
`age` 自体を書き換えてしまうと、上の表示まで 21 になってしまいます。

**別解：計算結果を変数に入れる**

```jsx
function App() {
  const name = 'やまだ たろう'
  const age = 20
  const hobby = '読書'
  const nextAge = age + 1

  return (
    <div className="app">
      <h1>{name}</h1>
      <p>年齢: {age}歳</p>
      <p>趣味: {hobby}</p>
      <p>来年は {nextAge} 歳になります。</p>
    </div>
  )
}

export default App
```

計算が複雑になるほど、**`return` の前で変数に入れておくほうが読みやすくなります。**
JSX の中の波かっこは、短い式だけにとどめるのがコツです。

---

### 演習 6.2 の解答

`src/App.jsx`

```jsx
import './App.css'

function App() {
  const score = 85

  return (
    <div className="app">
      <h1>テスト結果</h1>
      <p>点数: {score}点</p>
      <p className={score >= 60 ? 'pass' : 'fail'}>
        {score >= 60 ? '合格' : '不合格'}
      </p>
    </div>
  )
}

export default App
```

`src/App.css`（`.app` に追記する）

```css
.app {
  max-width: 720px;
  margin: 0 auto;
  padding: 24px;
}

.pass {
  color: #2e7d32;
  font-weight: bold;
}

.fail {
  color: #c0392b;
  font-weight: bold;
}
```

```text
表示される内容（score が 85 のとき）:
テスト結果
点数: 85点
合格          ← 緑の太字

表示される内容（score を 40 に書き換えたとき）:
テスト結果
点数: 40点
不合格        ← 赤の太字
```

**解説**

**ポイント1：`if` は書けないので三項演算子を使う**

```jsx
{score >= 60 ? '合格' : '不合格'}
```

波かっこの中に書けるのは**式**だけです（6.4.3、問 6.4）。
`if` 文は値にならないので書けません。三項演算子（4.4.4）が代わりになります。

**ポイント2：`className` にも波かっこが使える**

```jsx
<p className={score >= 60 ? 'pass' : 'fail'}>
```

属性に変数や式を渡すときは、`" "` ではなく `{ }` で囲みます（6.4.4）。
`score` が 85 なら `className="pass"`、40 なら `className="fail"` になったのと同じ状態になります。

**別解：`style` で色を切り替える**

```jsx
import './App.css'

function App() {
  const score = 85
  const isPassed = score >= 60

  return (
    <div className="app">
      <h1>テスト結果</h1>
      <p>点数: {score}点</p>
      <p style={{ color: isPassed ? '#2e7d32' : '#c0392b', fontWeight: 'bold' }}>
        {isPassed ? '合格' : '不合格'}
      </p>
    </div>
  )
}

export default App
```

CSS ファイルを触らずに済む代わりに、**JSX が長くなります。**
`const isPassed = score >= 60` のように判定結果を変数に入れると、
同じ条件を2回書かずに済み、あとから基準点を変えるのも1箇所で済みます。

なお 6.4.4 の補足のとおり、**見た目の指定は原則 `className` 側**に置きます。
`style` を使うのは、値を計算で決めたいときだけにしてください。

**よくある間違い：クォートで囲んでしまう**

```jsx
// 効かない：文字列 "score >= 60 ? 'pass' : 'fail'" というクラス名になる
<p className="score >= 60 ? 'pass' : 'fail'">
```

`" "` で囲むと、**中身がそのまま文字列として扱われます**（6.4.4）。
式を書きたいときは、必ず `{ }` を使ってください。

---

### 演習 6.3 の解答

`src/components/SiteHeader.jsx`

```jsx
function SiteHeader() {
  return (
    <header className="site-header">
      <h1>くだもの通信</h1>
      <p className="tagline">旬のくだもの情報をお届けします</p>
    </header>
  )
}

export default SiteHeader
```

`src/components/NoticeCard.jsx`

```jsx
function NoticeCard() {
  return (
    <article className="card">
      <p className="date">2026年4月1日</p>
      <h2>春の新商品を追加しました</h2>
      <p>いちごとメロンの商品ページを公開しました。ぜひご覧ください。</p>
    </article>
  )
}

export default NoticeCard
```

`src/components/SiteFooter.jsx`

```jsx
function SiteFooter() {
  const year = 2026

  return (
    <footer className="site-footer">
      <p>&copy; {year} くだもの通信</p>
    </footer>
  )
}

export default SiteFooter
```

`src/App.jsx`

```jsx
import SiteHeader from './components/SiteHeader.jsx'
import NoticeCard from './components/NoticeCard.jsx'
import SiteFooter from './components/SiteFooter.jsx'
import './App.css'

function App() {
  return (
    <div className="app">
      <SiteHeader />
      <NoticeCard />
      <NoticeCard />
      <NoticeCard />
      <SiteFooter />
    </div>
  )
}

export default App
```

`src/App.css`

```css
.app {
  max-width: 720px;
  margin: 0 auto;
  padding: 24px;
}

.site-header {
  border-bottom: 2px solid #333333;
  margin-bottom: 20px;
}

.tagline {
  color: #666666;
  font-size: 14px;
}

.card {
  border: 1px solid #dddddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 12px;
}

.card h2 {
  margin: 0 0 8px;
  font-size: 18px;
}

.date {
  margin: 0;
  color: #888888;
  font-size: 13px;
}

.site-footer {
  border-top: 1px solid #dddddd;
  margin-top: 20px;
  padding-top: 12px;
  color: #888888;
  font-size: 14px;
  text-align: center;
}
```

```text
表示される内容:
くだもの通信
旬のくだもの情報をお届けします
────────────────────
┌────────────────────────────┐
│ 2026年4月1日                │
│ 春の新商品を追加しました      │
│ いちごとメロンの商品ページを… │
└────────────────────────────┘
（同じお知らせが3つ）
────────────────────
© 2026 くだもの通信
```

**解説**

**ポイント1：1コンポーネント1ファイル + `export default`**

```jsx
function SiteHeader() { ... }

export default SiteHeader
```

`export default` は第5章で学んだデフォルトエクスポートです（5.6.3）。
1ファイルにつき1つだけ書けるもので、**そのファイルの主役**を指定します。

読み込む側では、名前を自分で決められます（`{ }` を付けません）。

```jsx
import SiteHeader from './components/SiteHeader.jsx'
```

**ポイント2：`import` のパスは `./` で始める**

```jsx
// ○
import SiteHeader from './components/SiteHeader.jsx'

// × Failed to resolve import になる
import SiteHeader from 'components/SiteHeader.jsx'
```

`./` が付いていないものは `node_modules` から探されます（6.3.2、6.5.3）。
自分で書いたファイルは、必ず `./` か `../` で始めてください。

**ポイント3：1箇所直すと3つとも変わる**

`NoticeCard.jsx` の `<h2>` を書き換えて保存してください。
**3つのお知らせが同時に変わります。** これがコンポーネントに分ける最大の効果です（6.5.1）。

**ポイント4：年は変数に入れる**

```jsx
const year = 2026

<p>&copy; {year} くだもの通信</p>
```

`<p>&copy; 2026 くだもの通信</p>` と直接書いても、いまは同じ表示になります。
それでも変数にしておくのは、**あとから「今年の年を自動で入れる」に変えるのが1行で済む**からです。

**よくある間違い1：ファイル名と大文字小文字が違う**

```text
作ったファイル: src/components/siteheader.jsx
書いた import : './components/SiteHeader.jsx'
```

macOS では動いてしまうことがありますが、**Windows や公開用サーバーでは動きません。**
ファイル名は、コンポーネント名とまったく同じにしてください（6.5.5 の慣習2）。

**よくある間違い2：`export default` を書き忘れる**

```text
出るエラー:
The requested module '/src/components/SiteHeader.jsx' does not provide an export named 'default'
```

「デフォルトの export が無い」という意味です。ファイルの末尾に `export default 名前` を追加してください。

**別解：3つのお知らせを `<>` でまとめる**

```jsx
function App() {
  return (
    <div className="app">
      <SiteHeader />
      <>
        <NoticeCard />
        <NoticeCard />
        <NoticeCard />
      </>
      <SiteFooter />
    </div>
  )
}
```

このように書くこともできますが、**ここでは意味がありません。**
フラグメントは「1つにまとめる必要があるとき」に使うもので（6.4.5）、
すでに `<div className="app">` の中にいるこの場面では不要です。

---

### 演習 6.4 の解答

**解答例（`App` + 2つのコンポーネント）**

`src/components/ShopHeader.jsx`

```jsx
function ShopHeader() {
  return (
    <header className="shop-header">
      <h1>くだものジュース店</h1>
      <p className="tagline">しぼりたてをお届けします</p>
    </header>
  )
}

export default ShopHeader
```

`src/components/ShopInfo.jsx`

```jsx
function ShopInfo() {
  return (
    <section className="shop-info">
      <h2>店舗情報</h2>
      <p>営業時間: 10:00 〜 19:00</p>
      <p>定休日: 水曜日</p>
    </section>
  )
}

export default ShopInfo
```

`src/App.jsx`

```jsx
// ヘッダーと店舗情報は、営業状況の値を使わない固定の表示なので分けた
import ShopHeader from './components/ShopHeader.jsx'
import ShopInfo from './components/ShopInfo.jsx'
import './App.css'

function App() {
  const isOpen = true
  const recommend = 'ぶどうジュース'

  return (
    <div className="app">
      {/* ヘッダーは他のページでも使い回せるので分けた */}
      <ShopHeader />

      <p className={isOpen ? 'open' : 'closed'}>
        {isOpen ? '営業中' : '本日休業'}
      </p>

      <p>本日のおすすめは {recommend} です。</p>

      <ShopInfo />
    </div>
  )
}

export default App
```

`src/App.css`

```css
.app {
  max-width: 720px;
  margin: 0 auto;
  padding: 24px;
}

.shop-header {
  border-bottom: 2px solid #333333;
  margin-bottom: 20px;
}

.tagline {
  color: #666666;
  font-size: 14px;
}

.open {
  color: #2e7d32;
  font-weight: bold;
}

.closed {
  color: #c0392b;
  font-weight: bold;
}

.shop-info {
  border-top: 1px solid #dddddd;
  margin-top: 20px;
  padding-top: 12px;
}
```

```text
表示される内容（isOpen が true のとき）:
くだものジュース店
しぼりたてをお届けします
────────────────────
営業中                       ← 緑の太字
本日のおすすめは ぶどうジュース です。
────────────────────
店舗情報
営業時間: 10:00 〜 19:00
定休日: 水曜日

表示される内容（isOpen を false にしたとき）:
（2行目が「本日休業」になり、赤の太字に変わる）
```

**解説**

**ポイント1：値を使う表示は `App` に残す**

`isOpen` と `recommend` は `App` の中で宣言した変数です。
**関数の中の変数は、その関数の中でしか使えません**（スコープ。4.6.4）。

```jsx
function App() {
  const isOpen = true    // App の中だけで使える
  ...
}

function ShopStatus() {
  // ここから isOpen は見えない
  return <p>{isOpen ? '営業中' : '本日休業'}</p>   // エラーになる
}
```

```text
出るエラー:
isOpen is not defined
```

**「営業状況」を別コンポーネントに切り出したい**と思ったなら、その感覚は正しいです。
それを可能にするのが**第7章の props** です。この章ではまだできないので、
**値を使わない部分（ヘッダー・店舗情報）を分ける**のが正解になります。

**ポイント2：分ける対象は 6.5.4 の基準で選ぶ**

| 部分 | 分けた？ | 理由 |
|------|---------|------|
| ヘッダー | ○ | 「ヘッダー」という名前が付けられる（基準2）。他のページでも使い回せる |
| 店舗情報 | ○ | 「店舗情報」という名前が付けられる（基準2）。4行のまとまり |
| 営業状況の1行 | × | `isOpen` を使うので、この章では分けられない |
| おすすめの1行 | × | 同上。また、1行を包むだけのコンポーネントは分けすぎ（6.5.4） |

**ポイント3：同じ条件を2回書くのが気になったら**

```jsx
<p className={isOpen ? 'open' : 'closed'}>
  {isOpen ? '営業中' : '本日休業'}
</p>
```

`isOpen ? ... : ...` が2回出てきます。
気になる場合は、`return` より前で変数にまとめられます。

```jsx
function App() {
  const isOpen = true
  const recommend = 'ぶどうジュース'
  const statusClass = isOpen ? 'open' : 'closed'
  const statusText = isOpen ? '営業中' : '本日休業'

  return (
    <div className="app">
      <ShopHeader />
      <p className={statusClass}>{statusText}</p>
      <p>本日のおすすめは {recommend} です。</p>
      <ShopInfo />
    </div>
  )
}
```

**JSX の中に条件式が増えてきたら、`return` より前に逃がす**——
これは実務でもよく使う整理の仕方です。どちらの書き方でも正解です。

**よくある間違い：コメントを JSX の中で `//` で書く**

```jsx
// これは画面に「// ヘッダーを分けた」と表示されてしまう
<div className="app">
  { // ヘッダーを分けた
  }
  <ShopHeader />
</div>
```

JSX の中では `{/* コメント */}` を使ってください（6.4.2 の違い5）。
`import` 文の並びなど、**JSX の外側**であれば `//` が使えます。

**別解：店舗情報の中にフッターも作る**

コンポーネントを3つ以上に分けても構いません。
ただし 6.5.4 のとおり、**1〜2行を包むだけのコンポーネントは作らない**でください。
`<h2>店舗情報</h2>` だけの `InfoTitle` のようなものを作っても、読みにくくなるだけです。

---

## 第7章

> 🚧 執筆中です。

## 第8章

> 🚧 執筆中です。

## 第9章

> 🚧 執筆中です。

## 第10章

> 🚧 執筆中です。
