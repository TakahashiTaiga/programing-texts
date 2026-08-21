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
