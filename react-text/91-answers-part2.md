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

> 🚧 執筆中です。

## 第8章

> 🚧 執筆中です。

## 第9章

> 🚧 執筆中です。

## 第10章

### 理解度チェック

**問 10.1 の解答**

- A = **文字列**
- B = **`JSON.stringify`**
- C = **`JSON.parse`**
- D = **`null`**

**解説**

`localStorage` は「文字列を入れる箱」です。
配列やオブジェクトをそのまま渡しても、エラーにはならず**黙って別物になります**（10.4.2）。

```js
localStorage.setItem('bad', { a: 1 })
console.log(localStorage.getItem('bad'))   // '[object Object]'
```

エラーが出ないぶん、原因に気づきにくいのがこの間違いの怖いところです。

D の `null` も重要です。**「まだ保存していない」と「空を保存した」は違います。**

```js
console.log(localStorage.getItem('nothing'))   // null
```

だから 10.4.3 では `if (saved !== null)` と書きました。
`JSON.parse(null)` は `null` を返してしまうので、確認せずに `setTasks` すると
タスクが `null` になり、`tasks.map is not a function` というエラーになります。

---

**問 10.2 の解答**

**state にするべきでないのは 3 と 5 です。**

| | 判定 | 理由 |
|--|-----|------|
| 1. タスクの配列 | state にする | 他の何からも計算できない元データ |
| 2. 入力中の文字 | state にする | 利用者の入力そのもの |
| 3. 未完了タスクの件数 | **しない** | `tasks` から計算できる |
| 4. 選ばれている絞り込み | state にする | 利用者の選択そのもの |
| 5. 画面に並ぶタスク | **しない** | `tasks` と `filter` と `sort` から計算できる |

**解説**

判断の基準は1つだけです。**「他の state から計算できるか」**（8.1.5、10.2.4）。

計算できるものを state にすると、**同じ事実を2箇所で持つ**ことになります。

```js
// ❌ 件数を state にすると、追加のたびに2箇所を更新することになる
setTasks([...tasks, newTask])
setActiveCount(activeCount + 1)   // ← 完了済みを追加したときに間違える
```

片方を更新し忘れると、「一覧には5件あるのに、件数は4件」というずれが起きます。
**計算で求めれば、ずれようがありません。**

```js
// ✅ 毎回計算する
const activeCount = tasks.filter((task) => !task.isDone).length
```

> **補足**
> 「毎回計算したら遅くないですか」と思うかもしれません。
> タスクが数十件なら、まったく問題ありません。
> 本当に重い計算になったときに `useMemo` を検討します（8.4.3、8.4.5）。

---

**問 10.3 の解答**

**理由**

このコードは、次の2つの点で「React に変化を伝えられていない」状態です。

1. `task.isDone = !task.isDone` で、**元のオブジェクトを直接書き換えている**
2. `setTasks(tasks)` で、**さっきと同じ配列**を渡している

React は「渡された値が前と違うか」を見て再レンダリングするかを決めます（7.2.4）。
中身を書き換えても配列そのものは同じなので、React からは**何も変わっていないように見えます。**

**正しい書き方**

```jsx
function handleToggle(id) {
  setTasks(
    tasks.map((task) =>
      task.id === id ? { ...task, isDone: !task.isDone } : task
    )
  )
}
```

**解説**

`map` は**新しい配列**を返し、`{ ...task, isDone: ... }` は**新しいオブジェクト**を作ります。
両方が新しくなるので、React は違いに気づけます（5.4.3、10.3.4）。

> **よくある間違い**
> `setTasks([...tasks])` のようにコピーだけして渡すと、画面は更新されます。
> しかし**中のオブジェクトは元のまま書き換えられている**ので、
> 「一度戻して、また進める」といった操作をしたときに、思わぬところが変わってしまいます。
>
> **配列も、中身のオブジェクトも、両方新しくする。** これが原則です。

---

**問 10.4 の解答**

**削除したあとに追加すると、id が重複します。**

3件（id は 1・2・3）のうち id が 2 のものを削除すると、残るのは 1 と 3 の2件です。
ここで「件数 + 1 = 3」として新しいタスクを作ると、**id が 3 のタスクが2つ**になります。

このとき、React は次の警告を出します。

```text
Warning: Encountered two children with the same key, `3`.
Keys should be unique so that components maintain their identity across updates.
```

**解説**

`key` は、React が「どの要素がどれか」を見分けるための目印です（7.4.2）。
同じ `key` が2つあると見分けがつかなくなり、次のような不具合が起きます。

- 片方を削除したのに、もう片方が消える
- チェックを入れたのに、別の行に取り消し線が付く

`Date.now()` は押すたびに違う数値になるので、この問題が起きません（10.2.3）。

> **補足**
> `Date.now()` も万能ではありません。**同じミリ秒の間に2件作ると重複します。**
> 手で押して追加する限り起きませんが、プログラムでまとめて作るときは
> `crypto.randomUUID()` のような、重複しない値を作る仕組みを使います。

---

**問 10.5 の解答**

**ガードを消すと、保存したタスクが毎回消えて、初期値に戻ります。**

動く順番は次のとおりです。

1. 最初の描画。このとき `tasks` は初期値（`sampleTasks`）
2. 描画のあと、**読み込みの `useEffect`** が動く。`localStorage` から読んで `setTasks` を呼ぶ
3. **同じタイミングで、保存の `useEffect` も動く**。
   このときの `tasks` は**まだ初期値のまま**（`setTasks` の結果は次の描画から反映される）
4. その結果、`localStorage` が**初期値で上書き**される

つまり、2 で読み込んだ値は 4 で捨てられ、次に開いたときには初期値しか残っていません。

**解説**

`isLoaded` は「読み込みが終わったか」を表す旗です。

- 1回目の描画では `isLoaded` が `false` なので、保存の `useEffect` は `return` で止まる
- 読み込みが終わり `setIsLoaded(true)` が反映された**次の描画**から、保存が動き始める

これで「読み込む前に保存する」という順番が起きなくなります（10.4.3）。

> **よくある間違い**
> 「2つの `useEffect` の順番を入れ替えれば解決する」と考える人がいますが、**解決しません。**
> どちらの順番でも、保存の `useEffect` が見る `tasks` は初期値のままだからです。
> 直すべきは順番ではなく、**「まだ保存してよい状態ではない」という条件**です。

---

**問 10.6 の解答**

**利用者が「データが消えた」と勘違いするからです。**

たとえば、10件すべてを完了にしたあと「未完了」で絞り込むと、表示は0件になります。
ここで「タスクがありません」と出ると、利用者はこう考えます。

> さっきまで10件あったのに、全部消えた。バグだ。

実際にはデータは残っていて、**いまの条件に合うものがないだけ**です。
「条件に合うタスクがありません」と書けば、
**絞り込みを変えれば戻ってくる**ことが利用者に伝わります（10.5.2）。

**解説**

これは 9.4.3 の「ユーザーに何を見せるか」と同じ話です。
画面には4つの状態がありました。**0件は、エラーでも成功でもない第3の状態**です。

同じ「0件」でも、**原因が違えば、利用者が次に取るべき行動も違います。**

| 状態 | 利用者が次にすべきこと |
|------|---------------------|
| 登録が0件 | 入力欄からタスクを追加する |
| 絞り込みの結果が0件 | 絞り込みを「すべて」に戻す |

**文言は、次に何をすればよいかが分かるように書きます。**

---

### 演習問題

### 演習 10.1 の解答

`src/App.jsx`（計算を1行足し、`<h1>` の下に表示を足す）

```diff
    const visibleTasks = [...filteredTasks].sort((a, b) => {
```

```diff
      return b.createdAt - a.createdAt
    })

+   // state ではなく計算。tasks が変われば自動的に新しい値になる（10.2.4）
+   const activeCount = tasks.filter((task) => !task.isDone).length
+
    return (
      <div className="app">
        <h1>タスク管理アプリ</h1>
+       <p className="task-count">
+         未完了 {activeCount} 件 / 全 {tasks.length} 件
+       </p>
        <TaskForm onAdd={handleAdd} />
```

`src/App.css`（末尾に追記する）

```css
.task-count {
  margin: 0 0 12px;
  color: #666666;
}
```

```text
表示される内容（3件中1件が完了のとき）:
タスク管理アプリ
未完了 2 件 / 全 3 件
```

**解説**

要点は2つです。

1. **`useState` を1つも増やしていない。**
   件数は `tasks` から計算できるので、state にすると更新漏れの原因になります（10.2.4、問 10.2）
2. **`filteredTasks` や `visibleTasks` ではなく `tasks` から数えている。**
   完成条件に「絞り込みを変えても件数は変わらない」とあるのは、この違いを確認するためです

`tasks` から数えているので、次のように動きます。

| 操作 | 未完了 | 全 |
|------|-------|----|
| タスクを1件追加 | +1 | +1 |
| チェックを入れる | −1 | 変わらない |
| 1件削除（未完了のもの） | −1 | −1 |
| 絞り込みを「完了」に変える | **変わらない** | **変わらない** |

> **よくある間違い**
> `visibleTasks.length` を使うと、絞り込みを変えるたびに「全 ◯ 件」が変わってしまいます。
> **どの配列から数えるか**は、意味がまったく違います。

> **別解**
> 完了の件数も出したいなら、同じ形でもう1行足します。
>
> ```js
> const doneCount = tasks.length - activeCount
> ```
>
> `tasks.filter((task) => task.isDone).length` でも同じ結果になりますが、
> **すでに計算した値から引ける**なら、そのほうが読みやすくなります。

---

### 演習 10.2 の解答

`src/App.jsx`（削除の関数と件数の計算を足し、ボタンを並べる）

```diff
    function handleDelete(id) {
      setTasks(tasks.filter((task) => task.id !== id))
    }

+   function handleDeleteDone() {
+     // 「完了していないものだけを残す」＝完了したものが全部消える
+     setTasks(tasks.filter((task) => !task.isDone))
+   }
+
```

```diff
    const activeCount = tasks.filter((task) => !task.isDone).length
+   const doneCount = tasks.length - activeCount
```

演習 10.1 をやっていない場合は、`activeCount` を使わずに直接数えても構いません。

```js
const doneCount = tasks.filter((task) => task.isDone).length
```

```diff
-       <TaskFilter filter={filter} onChangeFilter={setFilter} />
+       <div className="task-toolbar">
+         <TaskFilter filter={filter} onChangeFilter={setFilter} />
+         <button
+           type="button"
+           onClick={handleDeleteDone}
+           disabled={doneCount === 0}
+         >
+           完了したタスクを削除
+         </button>
+       </div>
```

`src/App.css`（末尾に追記する）

```css
.task-toolbar {
  display: flex;
  align-items: center;
  gap: 8px;
}
```

`.task-filter` にはもともと `margin: 12px 0` が付いているので、
上下の余白はそのまま保たれます。

```text
表示される内容（完了が1件あるとき）:
[すべて][未完了][完了] [完了したタスクを削除]

表示される内容（完了が0件のとき）:
[すべて][未完了][完了] [完了したタスクを削除]  ← 灰色で押せない
```

**解説**

**1件の削除と、まとめての削除は、同じ道具で書けます**（10.3.5）。
違うのは `filter` に渡す条件だけです。

```js
// 1件だけ削除：この id 以外を残す
tasks.filter((task) => task.id !== id)

// 完了を全部削除：完了していないものだけを残す
tasks.filter((task) => !task.isDone)
```

`filter` は「**残すもの**の条件」を書くメソッドです（5.3.2）。
「消すもの」を書こうとすると、条件が逆になって混乱します。
**「何を残すか」で考えてください。**

ボタンを押せなくしているのは `disabled` です（10.5.3）。

```jsx
disabled={doneCount === 0}
```

`doneCount` も計算で求めているので、チェックを入れた瞬間にボタンが押せるようになります。

再読み込みしても削除された状態が保たれるのは、`useLocalStorage` が
`tasks` の変化を検知して自動的に保存しているからです（10.4.4）。
**保存のためのコードを1行も書いていない**ことを確認してください。

> **よくある間違い**
> 次のように書くと、**完了したタスクだけが残ります。**
>
> ```js
> setTasks(tasks.filter((task) => task.isDone))   // ❌ 逆
> ```
>
> `!` の付け忘れです。動かしてすぐ気づけるので、必ず一度押して確認してください。

---

### 演習 10.3 の解答

**1. 対応表を作る**

`src/data/priorities.js`（新規作成）

```js
// 内部で使う値 → 画面に出す文字
export const PRIORITY_LABELS = {
  high: '高',
  normal: '中',
  low: '低',
}

// 内部で使う値 → 並べ替え用の数値（小さいほど先に並ぶ）
export const PRIORITY_ORDER = {
  high: 1,
  normal: 2,
  low: 3,
}
```

**2. サンプルデータに `priority` を足す**

`src/data/sampleTasks.js`（全文を次の内容にする）

```js
export const sampleTasks = [
  {
    id: 1,
    title: '牛乳を買う',
    isDone: false,
    createdAt: 1740000000000,
    priority: 'normal',
  },
  {
    id: 2,
    title: '第9章の演習をやる',
    isDone: true,
    createdAt: 1740000100000,
    priority: 'high',
  },
  {
    id: 3,
    title: '部屋を片づける',
    isDone: false,
    createdAt: 1740000200000,
    priority: 'low',
  },
]
```

**3. フォームで選べるようにする**

`src/components/TaskForm.jsx`（4箇所を変更する）

```diff
  import { useState } from 'react'
+ import { PRIORITY_LABELS } from '../data/priorities.js'
```

```diff
    const [title, setTitle] = useState('')
+   // 選択中の優先度。この部品の中だけで使う（10.2.4）
+   const [priority, setPriority] = useState('normal')
    const [error, setError] = useState('')
```

```diff
-     onAdd(trimmed)
+     onAdd(trimmed, priority)
      setTitle('')
+     setPriority('normal')
      setError('')
```

```diff
          placeholder="やることを入力"
        />
+       <select
+         value={priority}
+         onChange={(event) => setPriority(event.target.value)}
+       >
+         <option value="high">{PRIORITY_LABELS.high}</option>
+         <option value="normal">{PRIORITY_LABELS.normal}</option>
+         <option value="low">{PRIORITY_LABELS.low}</option>
+       </select>
        <button type="submit" disabled={title.trim() === ''}>
```

**4. 受け取って保存する**

`src/App.jsx`（`handleAdd` を変更する）

```diff
- function handleAdd(title) {
+ function handleAdd(title, priority) {
    const newTask = {
      id: Date.now(),
      title: title,
      isDone: false,
      createdAt: Date.now(),
+     priority: priority,
    }
    setTasks([...tasks, newTask])
  }
```

**5. 一覧に表示する**

`src/components/TaskItem.jsx`（`import` と表示を足す）

```diff
+ import { PRIORITY_LABELS } from '../data/priorities.js'
+
  function TaskItem({ task, onToggle, onDelete }) {
```

```diff
          </span>
        </label>
+       <span className="task-priority">{PRIORITY_LABELS[task.priority]}</span>
        <button type="button" onClick={() => onDelete(task.id)}>
```

**6. 並べ替えの選択肢を足す**

`src/components/TaskSort.jsx`（`option` を1つ足す）

```diff
        <option value="title">名前順</option>
+       <option value="priority">優先度順</option>
      </select>
```

`src/App.jsx`（`import` と並べ替えの条件を足す）

```diff
+ import { PRIORITY_ORDER } from './data/priorities.js'
```

```diff
      if (sort === 'title') {
        return a.title.localeCompare(b.title, 'ja')
      }
+     if (sort === 'priority') {
+       // 文字列のままでは比べられないので、対応表で数値に置き換える
+       return PRIORITY_ORDER[a.priority] - PRIORITY_ORDER[b.priority]
+     }
      return b.createdAt - a.createdAt
```

```text
表示される内容（「優先度順」を選んだとき）:
☑ 第9章の演習をやる   高  [削除]
□ 牛乳を買う         中  [削除]
□ 部屋を片づける      低  [削除]
```

**解説**

この演習の山場は、**「文字列は大小を比べられない」**という点です。

```js
console.log('high' < 'normal')   // true（辞書順で h が n より前なので、たまたま）
console.log('normal' < 'low')    // false（n は l より後ろ。意図と逆）
```

文字列をそのまま比べると、**辞書順**になります。
「高 → 中 → 低」という意味の順番とは無関係です。

そこで `PRIORITY_ORDER` という対応表で数値に置き換えます（10.3.7 の参考）。

```js
PRIORITY_ORDER['high']    // 1
PRIORITY_ORDER['normal']  // 2
```

数値になれば、`createdAt` の並べ替えとまったく同じ引き算の形で書けます。

**対応表を2つに分けたのも意味があります。**

| 対応表 | 用途 | 変えたくなる理由 |
|-------|------|---------------|
| `PRIORITY_LABELS` | 画面に出す文字 | 「高」を「急ぎ」に変えたい |
| `PRIORITY_ORDER` | 並べ替えの順番 | 低い順に並べたい |

表示と順番は**別の関心事**なので、混ぜないほうがあとで直しやすくなります。

> **よくある間違い**
> `PRIORITY_LABELS.task.priority` や `PRIORITY_LABELS.priority` と書くと `undefined` になります。
> **変数の中身をキーとして使うときは、角かっこです**（10.3.7 の参考、5.2.2）。
>
> ```jsx
> {PRIORITY_LABELS[task.priority]}   {/* ✅ '高' などが表示される */}
> {PRIORITY_LABELS.task.priority}    {/* ❌ エラーになる */}
> ```

> **よくある間違い**
> 保存済みの古いタスクには `priority` がないため、`PRIORITY_LABELS[undefined]` となり、
> 優先度の欄が**空**になります。エラーは出ません。
>
> `localStorage.removeItem('task-app.tasks')` を実行してから
> ページを再読み込みして、サンプルデータの状態に戻してください（10.4.3）。

> **別解**
> `<option>` も `map` で作れます（10.3.6 の `TaskFilter` と同じ形）。
>
> ```jsx
> {Object.keys(PRIORITY_LABELS).map((key) => (
>   <option key={key} value={key}>
>     {PRIORITY_LABELS[key]}
>   </option>
> ))}
> ```
>
> `Object.keys` は、オブジェクトのキーを配列にして返す関数です。
> 選択肢が3つなら手で書いても構いません。**増えたときに書き換えます。**

---

### 演習 10.4 の解答

`src/App.jsx`（state を1つ足し、削除の関数を書き換え、表示を足す）

```diff
    const [sort, setSort] = useState('newest')
+   // 直前に削除したタスク。何も削除していないときは null
+   const [deletedTask, setDeletedTask] = useState(null)
```

```diff
    function handleDelete(id) {
+     // 消す前に、対象のタスクを取っておく（5.3.3）
+     const target = tasks.find((task) => task.id === id)
      setTasks(tasks.filter((task) => task.id !== id))
+     setDeletedTask(target)
    }

+   function handleUndo() {
+     // 取っておいたものを、そのまま戻す
+     setTasks([...tasks, deletedTask])
+     setDeletedTask(null)
+   }
+
```

```diff
        <TaskList
          tasks={visibleTasks}
          totalCount={tasks.length}
          onToggle={handleToggle}
          onDelete={handleDelete}
        />
+       {deletedTask !== null && (
+         <p className="undo-message">
+           「{deletedTask.title}」を削除しました
+           <button type="button" onClick={handleUndo}>
+             元に戻す
+           </button>
+         </p>
+       )}
      </div>
```

`src/App.css`（末尾に追記する）

```css
.undo-message {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 16px;
  color: #666666;
}
```

```text
表示される内容（「牛乳を買う」を削除した直後）:
□ 部屋を片づける         [削除]
「牛乳を買う」を削除しました [元に戻す]

「元に戻す」を押したあと:
□ 牛乳を買う            [削除]
□ 部屋を片づける         [削除]
（「削除しました」の表示は消える）
```

**解説**

**考え方の中心は、「消す前に取っておく」**です。

削除してしまってからでは、もう `tasks` の中に元のタスクはありません。
だから `filter` で消す**前**に、`find` で対象を取り出しておきます（5.3.3）。

```js
const target = tasks.find((task) => task.id === id)   // 先に取っておく
setTasks(tasks.filter((task) => task.id !== id))      // それから消す
```

**「何も削除していない」を表す値を `null` にしたのも要点です。**
`null` は「値が入っていないことを、はっきり示すための値」でした（4.3.5）。

```jsx
{deletedTask !== null && ( ... )}
```

これは 7.5.1 の条件付きレンダリングです。
`deletedTask` が `null` のあいだ、この表示はまるごと存在しません。

**位置が元どおりに戻る理由**

戻すときは `[...tasks, deletedTask]` で**末尾**に足しているのに、
「新しい順」で見ると元の位置に戻ります。

並び順を決めているのが `createdAt` であり、**取っておいたタスクの `createdAt` は
削除前と同じ値のまま**だからです（10.3.7）。
新しく作り直していたら、`Date.now()` の値が変わって一番上に来てしまいます。

**これが「取っておいたものを戻す」と「作り直す」の違い**です。

> **よくある間違い**
> `handleUndo` の中で `setDeletedTask(null)` を書き忘れると、
> 戻したあとも「削除しました」の表示が残り、**もう一度押すと同じタスクが2つ**になります。
>
> `id` が同じタスクが2つできるので、問 10.4 と同じ `key` の重複警告が出ます。

> **よくある間違い**
> 「元に戻す」の表示を `TaskList` の中に書くと、0件のときに表示されません。
> `TaskList` は0件のとき早期 return するからです（10.5.2）。
>
> **最後の1件を削除したときこそ、取り消したくなります。**
> `TaskList` の外（`App` の中）に置いてください。

> **別解：複数回さかのぼれるようにする**
> `deletedTask` を配列にすれば、何回でも取り消せるようになります。
>
> ```js
> const [deletedTasks, setDeletedTasks] = useState([])
> // 削除時： setDeletedTasks([...deletedTasks, target])
> // 取り消し時： 最後の1件を取り出し、残りを setDeletedTasks に戻す
> ```
>
> ただし「何件までさかのぼれるか」を決めないと、削除したデータが増え続けます。
> **どこまでやるかを決めるのも設計です**（10.1.3）。
