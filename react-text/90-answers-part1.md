---
title: "解答編 その1（第1章〜第5章）"
---

# 解答編 その1（第1章〜第5章）

**先に自分で解いてから読んでください。**

解けなかった問題は、この解答を読んだあと、
**何も見ずにもう一度自分で書き直して**ください。それで定着します。

> **解答を読んでも納得できないとき**
> AI に次のように聞いてください。
>
> ```text
> react-text の演習 3.2 の解答を読みましたが、
> なぜ display: flex を .card ではなく .card-list に書くのかが納得できません。
> ```

---

## 第1章

### 理解度チェック

**問 1.1 の解答**

- A = **リクエスト**
- B = **レスポンス**

**解説**

レストランでたとえると、リクエストが「注文」、レスポンスが「料理」です。
ブラウザ（客席）が注文を出し、サーバー（厨房）が料理を返します。

この2つのやり取りのルールが **HTTP** で、
それを暗号化したものが **HTTPS** です。

---

**問 1.2 の解答**

**2. CSS**

**解説**

| 名前 | 役割 |
|------|------|
| HTML | 何がどこにあるか（構造） |
| **CSS** | **どう見えるか（見た目）** |
| JavaScript | どう動くか（動作） |
| Node.js | 開発の道具を動かす土台（見た目とは無関係） |

Node.js を選んでしまった場合は、1.5.1 を読み返してください。
Node.js は「ブラウザの外で JavaScript を動かすもの」で、
Web ページの見た目には直接関係しません。

---

**問 1.3 の解答**

`pwd`

**解説**

**p**rint **w**orking **d**irectory の略で、「作業中のディレクトリを表示」という意味です。

**これは、この先いちばん使うコマンドになります。**
「ファイルが見つからない」というエラーの原因は、
9割方「現在地が想定と違う」ことです。困ったらまず `pwd` を打ってください。

---

**問 1.4 の解答**

**1つ上のディレクトリに移動する。**

**解説**

`..`（ドット2つ）は「1つ上のディレクトリ」を表す特別な書き方です。

```text
C:\Users\yamada\Desktop\react-lesson   ← いまここ
        cd ..
C:\Users\yamada\Desktop                ← ここに移動する
```

2つ上に行きたいときは `cd ../..` と重ねます。

なお、`.`（ドット1つ）は「いまいるディレクトリ」を意味します。
`code .` が「いまいるディレクトリを VS Code で開く」になるのは、このためです。

---

**問 1.5 の解答**

**ターミナルをいったんすべて閉じて、開き直す。**

**解説**

インストール直後は、**すでに開いているターミナルに新しい PATH の設定が反映されていません。**
これが原因の場合、ターミナルを開き直すだけで解決します。

**この問題で悩む人が非常に多いです。**
まずこれを試し、それでもだめならパソコンを再起動、
それでもだめなら AI に相談してください。

他に考えられる原因：

- Node.js のインストール自体が失敗していた
- PATH に登録されていない
- 管理者権限が必要だった

いずれも**環境の問題であって、あなたの能力とは無関係**です。
AI に丸投げして構いません。

---

**問 1.6 の解答**

`index.html` を作ったつもりが、実際には **`index.html.txt`** のような
別の種類のファイルになってしまい、ブラウザで開いても
HTML として表示されない（文字がそのまま表示される）。

**解説**

Windows は初期設定で拡張子を隠します。
すると、画面上はどちらも `index` としか見えないため、
**間違いに気づけません。**

「HTML を書いたのに、タグがそのまま画面に表示される」という現象が起きたら、
まず拡張子を確認してください。

---

### 演習

### 演習 1.1 の解答

`profile.html`

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>自己紹介</title>
  </head>
  <body>
    <h1>山田 太郎</h1>
    <p>はじめまして。プログラミングの勉強を始めました。</p>
    <p>好きな食べ物はラーメンです。</p>
    <p>将来は Web エンジニアになりたいと思っています。</p>
  </body>
</html>
```

**解説**

チェックポイントは4つです。

1. **`<title>自己紹介</title>`** — ブラウザのタブに表示されます
2. **`<meta charset="UTF-8" />`** — これがないと日本語が文字化けします
3. `<h1>` は1つだけ
4. `<p>` が3つ以上

**もし文字化けした場合**

`<meta charset="UTF-8" />` を書いていても文字化けすることがあります。
その場合、**ファイル自体が UTF-8 で保存されていません。**

VS Code の画面右下に文字コードが表示されています。
`Shift JIS` などになっていたらクリックして、
「エンコード付きで保存」→「UTF-8」を選んでください。

---

### 演習 1.2 の解答

**Windows（PowerShell）/ macOS 共通**

```powershell
mkdir practice
cd practice
pwd
cd ..
ls
```

**解説**

1. `mkdir practice` — `practice` ディレクトリを作る
2. `cd practice` — その中に移動する
3. `pwd` — 現在地を表示（末尾が `practice` になっていれば成功）
4. `cd ..` — 1つ上に戻る
5. `ls` — 一覧を表示（`practice` が見えれば成功）

**打ち間違いを減らすコツ**

`cd prac` まで打って **Tab キー**を押すと、`cd practice` まで自動補完されます。
**必ず使ってください。** 打ち間違いによる無駄なエラーが激減します。

**うまくいかなかった場合**

```text
mkdir : 項目 D:\...\practice は既に存在するため作成できませんでした。
```

すでに同名のディレクトリがあります。別の名前で試すか、
`ls` で確認してから進めてください。

---

### 演習 1.3 の解答

この演習に「正解のコード」はありません。次の3つができていれば達成です。

**1. 大見出しのタグを特定する**

「要素」タブで HTML を上から辿ると、`<h1>` が見つかります。
その行にマウスを乗せると、画面上の対応する部分がハイライトされます。

もっと速い方法もあります。
**調べたい文字の上で右クリック →「検証」**（または「調査」）を選ぶと、
その要素が「要素」タブで直接選択された状態で開きます。**この方法を覚えてください。**

**2. コンソールを確認する**

「コンソール」タブに赤い文字でエラーが出ているサイトは、実は珍しくありません。
大手のサイトでも出ています。**エラーが出ていても、ページは動くことがある**と知っておいてください。

**3. 表示を書き換える**

「要素」タブの文字をダブルクリックすると、その場で編集できます。
書き換えると、画面の表示が即座に変わります。

**この演習の狙い**

> 開発者ツールでの変更は、**あなたのブラウザの表示だけ**が変わっています。
> サーバー上のデータは一切変わりません。再読み込みすれば元に戻ります。
>
> ここで実感してほしいのは、
> **どんなに複雑に見えるサイトも、あなたが 1.6.1 で書いたものと同じ、
> ただの HTML でできている**ということです。
>
> 「自分にはとても作れない」と思うサイトも、中身を開けば
> `<div>` と `<p>` と `<img>` の組み合わせにすぎません。

---

## 第2章

### 理解度チェック

**問 2.1 の解答**

**間違い1：閉じタグの順番が逆**

```html
<!-- 間違い -->
<p><strong>重要</p></strong>

<!-- 正しい -->
<p><strong>重要</strong></p>
```

タグは**開いた順と逆の順で閉じます**。
`<p>` → `<strong>` の順で開いたなら、`</strong>` → `</p>` の順で閉じます。

**間違い2：`<img>` に `alt` 属性がない**

```html
<!-- 間違い -->
<img src="cat.png">

<!-- 正しい -->
<img src="cat.png" alt="窓辺で眠る三毛猫" />
```

`alt` は、画像が読み込めなかったときの代替表示、
読み上げソフトへの情報提供、検索エンジンへの情報提供に使われます。**必須です。**

> なお `<img src="cat.png">` の末尾に ` />` がない点は、**間違いではありません。**
> HTML としてはどちらも有効です。ただしこの本では、
> React で必須になるため `/>` を書く形に統一しています。

---

**問 2.2 の解答**

**3. 何も表示されない**

**解説**

`<head>` は**ページの設定**を書く場所で、**画面には表示されません。**
表示したい内容は、すべて `<body>` の中に書きます。

エラーにもなりません。**ただ静かに表示されないだけ**なので、
原因に気づきにくいトラブルです。

「書いたのに表示されない」と思ったら、**まず `<body>` の中かどうか**を確認してください。

---

**問 2.3 の解答**

```text
こんにちは さようなら
```

**1行で、間に半角スペース1つが入って表示されます。**

**解説**

**HTML は、ソースコード上の改行とスペースを無視します。**
連続する空白（改行、スペース、タブ）は、すべて**半角スペース1つ**に置き換えられます。

表示上も改行したい場合は、次のどちらかにします。

```html
<!-- 方法1：br タグ -->
<p>こんにちは<br />さようなら</p>

<!-- 方法2：段落を分ける（こちらが望ましい） -->
<p>こんにちは</p>
<p>さようなら</p>
```

「1つの段落の中で、どうしても改行したい」場合だけ `<br />` を使います。

---

**問 2.4 の解答**

**`id` はページ内で1つだけしか使えない固有の名前。
`class` は何個の要素にでも付けられるグループ名。**

**解説**

たとえるなら、`id` はマイナンバー（1人に1つ）、
`class` は部活動（同じ部活に何人でも所属できる）です。

```html
<!-- OK -->
<p class="note">A</p>
<p class="note">B</p>

<!-- NG（id が重複している） -->
<p id="note">A</p>
<p id="note">B</p>
```

**使い分け**

| 用途 | 使うもの |
|------|---------|
| CSS で見た目を指定する | **class**（3.2.3 参照） |
| ページ内リンクの飛び先 | id |
| `<label for="...">` の対応先 | id |

**実務では class を圧倒的に多く使います。**

---

**問 2.5 の解答**

```html
<img src="../images/logo.png" alt="ロゴ" />
```

**解説**

```text
site/
├── index.html
├── images/
│   └── logo.png       ← 行き先
└── pages/
    └── about.html     ← 出発点
```

1. `about.html` は `pages/` の中にいる
2. `images/` は `pages/` と同じ階層（`site/` の直下）にある
3. なので、まず `../` で `site/` に上がる
4. そこから `images/logo.png` に入る

結果、`../images/logo.png` となります。

**よくある間違い**

| 書き方 | なぜ間違いか |
|--------|------------|
| `images/logo.png` | `pages/images/` を探しにいってしまう |
| `/images/logo.png` | 先頭の `/` は「サーバーの最上位」の意味。`file://` では意図どおりに動かない |
| `..\images\logo.png` | HTML では `\` ではなく `/` を使う |

**確認方法**

画像が表示されなかったら **F12 →「コンソール」タブ**を見てください。
ブラウザが実際に探しに行ったパスが表示されるので、原因がすぐわかります。

---

**問 2.6 の解答**

**`id` 属性に、`for` と同じ値（`email`）を書く。**

```html
<label for="email">メールアドレス</label>
<input type="email" id="email" name="email" />
```

**解説**

`for` と `id` を対応させると、次の効果が得られます。

1. **ラベルの文字をクリックすると、入力欄にカーソルが入る**
2. 読み上げソフトが「メールアドレスの入力欄です」と伝えられる

**正しく対応しているかの確認方法**

**ラベルの文字をクリックしてみてください。**
入力欄にカーソルが移動すれば成功、何も起きなければ対応が間違っています。

`name` 属性は別の役割（サーバーに送るときの項目名）なので、
`for` の対応先にはなりません。

---

**問 2.7 の解答**

**`type="button"` を指定する。**

指定しないと `type="submit"` として扱われ、
**ボタンを押すたびにフォームが送信され、ページが再読み込みされてしまう。**

**解説**

```html
<!-- 危険：押すとページが再読み込みされる -->
<form>
  <button onclick="...">計算する</button>
</form>

<!-- 正しい -->
<form>
  <button type="button" onclick="...">計算する</button>
</form>
```

`<button>` の `type` を省略すると、**デフォルトは `submit`** です。

この現象は、「ボタンを押すと一瞬何かが表示されて、すぐ消える」という
非常にわかりにくい形で現れます。**第7章の React でも同じ問題が起きます。**

**フォームの中のボタンには、必ず `type` を明示する**と覚えてください。

---

### 演習

### 演習 2.1 の解答

`shop.html`

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ネコカフェ にゃんこ</title>
  </head>
  <body>
    <h1>ネコカフェ にゃんこ</h1>
    <p>2020年に開店した、のんびり過ごせるネコカフェです。</p>

    <h2>在籍しているネコ</h2>
    <ul>
      <li>たま（三毛猫・3歳）</li>
      <li>くろ（黒猫・5歳）</li>
      <li>しろ（白猫・2歳）</li>
    </ul>

    <h2>ご利用の流れ</h2>
    <ol>
      <li>受付で手を消毒する</li>
      <li>料金を支払う</li>
      <li>ロッカーに荷物を預ける</li>
      <li>ネコの部屋へ入る</li>
    </ol>

    <h2>ご注意</h2>
    <p>ネコが嫌がることはしないでください。</p>
    <p>フラッシュ撮影は禁止です。</p>
  </body>
</html>
```

**解説**

チェックポイント：

- `<h1>` は**1ページに1つだけ**。店名に使う
- `<h2>` は複数あってよい
- `<ul>` は順番に意味がないもの（ネコの一覧）
- `<ol>` は順番に意味があるもの（利用の流れ）

**`<ol>` で番号を手打ちしていませんか**

```html
<!-- 間違い -->
<ol>
  <li>1. 受付で手を消毒する</li>
</ol>
```

これでは「1. 1. 受付で〜」と二重に表示されます。
**番号は自動で振られます。**

---

### 演習 2.2 の解答

```html
<h2>料金表</h2>
<table>
  <thead>
    <tr>
      <th>プラン名</th>
      <th>時間</th>
      <th>料金</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>お試しプラン</td>
      <td>30分</td>
      <td>700円</td>
    </tr>
    <tr>
      <td>通常プラン</td>
      <td>1時間</td>
      <td>1,200円</td>
    </tr>
    <tr>
      <td>フリープラン</td>
      <td>制限なし</td>
      <td>3,000円</td>
    </tr>
  </tbody>
</table>
```

**解説**

構造は次のとおりです。

```text
table
 ├ thead
 │  └ tr
 │     └ th × 3
 └ tbody
    └ tr × 3
       └ td × 3
```

**押さえるべき点**

1. **`<tr>` が行、`<th>` / `<td>` がセル。** 行を作ってからセルを並べる
2. 見出しは `<th>`、データは `<td>`
3. **各行のセル数を揃える**（揃っていないと表が崩れます）
4. 「列」を作るタグは存在しない。各行の同じ位置のセルが自動的に列になる

**`<thead>` と `<tbody>` は必須か**

省略しても表示されます。ただし書いておくと、
「ここが見出し行」という意味が明確になり、CSS でも指定しやすくなります。**書く習慣をつけてください。**

---

### 演習 2.3 の解答

**3ファイルすべてに、次のナビゲーションを入れます。**

```html
<nav>
  <ul>
    <li><a href="shop.html">お店について</a></li>
    <li><a href="menu.html">メニュー</a></li>
    <li><a href="access.html">アクセス</a></li>
  </ul>
</nav>
```

`menu.html`（例）

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>メニュー | ネコカフェ にゃんこ</title>
  </head>
  <body>
    <h1>メニュー</h1>

    <nav>
      <ul>
        <li><a href="shop.html">お店について</a></li>
        <li><a href="menu.html">メニュー</a></li>
        <li><a href="access.html">アクセス</a></li>
      </ul>
    </nav>

    <h2>ドリンク</h2>
    <ul>
      <li>コーヒー 500円</li>
      <li>紅茶 500円</li>
    </ul>
  </body>
</html>
```

**解説**

3ファイルとも同じディレクトリにあるので、相対パスは**ファイル名だけ**で済みます。

```html
<a href="menu.html">   <!-- 同じディレクトリ -->
```

**押さえるべき点**

1. **自分自身へのリンクも入れておく**
   （3ページとも同じナビゲーションになり、管理が楽になります）
2. ナビゲーションは `<nav>` で囲む（意味を持たせる。2.5.2 参照）
3. リンクの集まりは `<ul>` と `<li>` で書く
4. `<title>` はページごとに変える

**リンクが動かない場合**

| 症状 | 原因 |
|------|------|
| クリックしても何も起きない | `href` の書き忘れ、または `href=""` |
| 「ファイルが見つかりません」 | ファイル名の打ち間違い、拡張子違い |
| 別のページに飛ぶ | コピーしたときに `href` を直し忘れている |

**3ページすべてで、3つのリンクをすべてクリックして確認してください。**
コピーして作ると、直し忘れが必ず起きます。

---

### 演習 2.4 の解答

`reserve.html`

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>予約フォーム | ネコカフェ にゃんこ</title>
  </head>
  <body>
    <h1>ご予約</h1>

    <form>
      <div>
        <label for="name">お名前</label>
        <input type="text" id="name" name="name" required placeholder="山田太郎" />
      </div>

      <div>
        <label for="email">メールアドレス</label>
        <input type="email" id="email" name="email" required />
      </div>

      <div>
        <label for="visit-date">来店希望日</label>
        <input type="date" id="visit-date" name="visitDate" />
      </div>

      <div>
        <label for="people">人数</label>
        <input type="number" id="people" name="people" min="1" max="10" value="1" />
      </div>

      <div>
        <label for="plan">プラン</label>
        <select id="plan" name="plan">
          <option value="">選択してください</option>
          <option value="trial">お試しプラン（30分）</option>
          <option value="normal">通常プラン（1時間）</option>
          <option value="free">フリープラン</option>
        </select>
      </div>

      <div>
        <label for="message">ご要望</label>
        <textarea id="message" name="message" rows="5"></textarea>
      </div>

      <div>
        <input type="checkbox" id="agree" name="agree" required />
        <label for="agree">利用規約に同意する</label>
      </div>

      <button type="submit">予約する</button>
    </form>
  </body>
</html>
```

**解説**

**1. `for` と `id` の対応**

すべての入力欄に `<label>` があり、`for` と `id` が一致しています。

```html
<label for="name">お名前</label>
<input type="text" id="name" ... />
         ↑ この2つが一致
```

**確認方法：ラベルの文字をクリックして、カーソルが入力欄に移動すれば成功です。**
7つすべてで確認してください。

**2. チェックボックスだけ順番が違う**

```html
<!-- 通常の入力欄：ラベルが先 -->
<label for="name">お名前</label>
<input type="text" id="name" />

<!-- チェックボックス：入力欄が先 -->
<input type="checkbox" id="agree" />
<label for="agree">利用規約に同意する</label>
```

チェックボックスは「□ 利用規約に同意する」と表示したいので、
入力欄を先に書くのが自然です。**どちらの順でも `for` と `id` は機能します。**

**3. `id` と `name` の値が違ってもよい**

```html
<input type="date" id="visit-date" name="visitDate" />
```

`id` はページ内での識別用、`name` はサーバーに送るときの項目名なので、
**別の値にしても構いません。** 同じにしても問題ありません。

**4. `<div>` で囲んでいる理由**

各項目を `<div>` で囲むと、`<div>` はブロック要素なので**改行されます**。
囲まないと全部が1行に並んでしまい、非常に見づらくなります。

第3章の CSS を学べば、`<div>` に余白を付けてさらに整えられます。

**5. `required` の効果**

`required` を付けた項目が未入力のまま送信しようとすると、
**ブラウザが「このフィールドを入力してください」と警告してくれます。**
JavaScript を書かなくても、この程度のチェックは HTML だけでできます。

**送信ボタンを押すと何が起きるか**

いまの段階では、ページが再読み込みされて入力内容が消えるだけです。
**実際にデータを送るには、送信先（サーバー）が必要**です。
これは第3冊目の FastAPI で扱います。

---

### 演習 2.5 の解答

```html
<body>
  <header>
    <h1>ネコカフェ にゃんこ</h1>
    <nav>
      <ul>
        <li><a href="index.html">ホーム</a></li>
        <li><a href="menu.html">メニュー</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <section>
      <h2>お知らせ</h2>
      <p>本日は通常営業です。</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2026 ネコカフェ にゃんこ</p>
  </footer>
</body>
```

**解説**

書き換えの対応表です。

| 元のコード | 書き換え後 | 理由 |
|-----------|-----------|------|
| `<div class="top">` | `<header>` | ページ上部のまとまり |
| `<div class="title">` | `<h1>` | ページの主題（大見出し） |
| `<div class="menu">` | `<nav>` | ナビゲーション |
| `<div><a>...</a></div>` | `<ul><li><a>` | リンクの一覧はリスト |
| `<div class="content">` | `<main>` | ページの主要な内容 |
| `<div class="block">` | `<section>` | 意味のまとまり |
| `<div class="heading">` | `<h2>` | セクションの見出し |
| `<div class="bottom">` | `<footer>` | ページ下部のまとまり |
| `<div>本日は〜</div>` | `<p>` | 段落 |

**押さえるべき考え方**

**`class` の名前が、そのまま答えを示しています。**

- `class="title"` → タイトル → `<h1>`
- `class="heading"` → 見出し → `<h2>`
- `class="menu"` → メニュー → `<nav>`

**`class` で意味を表そうとしていた**ということは、
**その意味を表すタグがあるはず**、ということです。

**書き換えたら `class` は消す**

```html
<!-- 冗長 -->
<header class="top">

<!-- これでよい -->
<header>
```

`<header>` タグ自体が「ページ上部」という意味を持っているので、
`class="top"` は不要です。

> **ただし、CSS を当てるための `class` は残してよい**
> ページに `<section>` が10個ある場合、
> そのうち1つだけ色を変えたいなら `class` が必要です。
> **「意味を表すための class」は不要、「CSS のための class」は必要**、と使い分けてください。

**見た目が変わっていないことを確認する**

`<header>` も `<div>` も、CSS を当てていない状態では見た目がほぼ同じです。
`<h1>` `<h2>` `<ul>` に変えた部分だけは、
ブラウザの初期スタイルによって見た目が変わります。**これは正しい変化です。**

---

## 第3章

### 理解度チェック

**問 3.1 の解答**

**間違い1：セレクタにドットがない**

```css
/* 間違い：note というタグを探してしまう */
note { ... }

/* 正しい */
.note { ... }
```

class を指定するときは、**必ず `.`（ドット）** を先頭に付けます。
`note` と書くと `<note>` というタグを探しますが、そんなタグは存在しないので何も起きません。

**間違い2：セミコロンがない**

```css
/* 間違い */
.note {
  color: red        ← ここに ; がない
  font-size: 14px;
}

/* 正しい */
.note {
  color: red;
  font-size: 14px;
}
```

**セミコロンを忘れると、その行だけでなく次の行も効かなくなります。**
CSS は `;` までを1つの宣言として読むため、
`color: red font-size: 14px` という不正な1行として解釈されてしまうからです。

「なぜか一部だけ効かない」ときは、**まず1つ上の行の `;` を確認**してください。

---

**問 3.2 の解答**

**青（`#intro` のルールが適用される）**

**解説**

詳細度の点数は次のとおりです。

| セレクタ | 種類 | 点数 |
|---------|------|------|
| `p` | 要素 | 1 |
| `.text` | class | 10 |
| `#intro` | **id** | **100** |

**点数が最も高い `#intro` が勝ちます。**

書いた順番は関係ありません。仮に順番を逆にしても、青のままです。

```css
#intro { color: blue; }   /* 100 — これが勝つ */
.text  { color: red; }    /* 10 */
p      { color: black; }  /* 1 */
```

> **だから CSS で id を使わないのです**
> `#intro` に負けた `.text` を勝たせるには、
> `#intro.text` のように書くか `!important` を使うしかなくなります。
> どちらも保守しにくいコードになります。
>
> **CSS では class だけを使う**と決めておけば、点数が揃うので
> 「後に書いたほうが勝つ」という単純なルールで制御できます。

---

**問 3.3 の解答**

**`padding` は枠線（border）の内側の余白、`margin` は枠線の外側の余白。**

**解説**

```text
┌──────── margin ────────┐   ← 外側（背景色が付かない）
│  ┌───── border ─────┐  │
│  │  ┌── padding ──┐ │  │   ← 内側（背景色が付く）
│  │  │  content    │ │  │
│  │  └─────────────┘ │  │
│  └──────────────────┘  │
└────────────────────────┘
```

**見分ける実験**

```css
.box {
  background-color: lightblue;
  padding: 20px;   /* 水色の範囲が広がる */
  margin: 20px;    /* 水色は広がらず、外に隙間ができる */
}
```

**背景色を付けると、どちらか一目でわかります。**
迷ったらこの実験をしてください。

**使い分け**

| 目的 | 使うもの |
|------|---------|
| 箱の中身と枠線の間に余裕を持たせる | `padding` |
| 箱と箱の間に隙間を作る | `margin`（または Flexbox の `gap`） |

---

**問 3.4 の解答**

**`box-sizing` の指定なし：324px**

```text
300px (width = content)
+ 10px + 10px (padding 左右)
+  2px +  2px (border 左右)
= 324px
```

**`box-sizing: border-box` あり：300px**

**解説**

初期設定（`content-box`）では、
**`width` は「中身の幅」であって「箱全体の幅」ではありません。**

これは直感に反するため、
「300px の箱を3つ並べたら、900px の枠からはみ出した」という事故が頻発します。

`box-sizing: border-box` を指定すると、
**`width` が「padding と border を含めた全体の幅」に変わります。**
指定した数値が、そのまま画面上の幅になります。

**実務のお約束**

CSS の先頭に必ず次を書きます。

```css
* {
  box-sizing: border-box;
}
```

**これはほぼすべてのプロジェクトで書かれています。** 忘れずに書いてください。

---

**問 3.5 の解答**

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**解説**

| プロパティ | 揃える方向（`flex-direction: row` のとき） |
|-----------|------------------------------------------|
| `justify-content: center` | **主軸** = 横方向の中央 |
| `align-items: center` | **交差軸** = 縦方向の中央 |

この2つを両方 `center` にすることで、完全な中央配置になります。

**注意：親要素に高さが必要です**

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;      /* これがないと上下中央が意味を持たない */
}
```

親の高さが中身の分しかないと、「上下中央」も中身の位置と同じになるためです。

> **Flexbox 登場以前は、これが CSS の最難関でした。**
> 「CSS 上下中央揃え」で検索すると、
> `position: absolute` と `transform` を組み合わせた複雑な手法が今も出てきます。
> **いまは Flexbox の3行で解決します。**

---

**問 3.6 の解答**

**1. `.container`**

**解説**

`display: flex` は、**並べたい要素そのものではなく、その親（＝コンテナ）に書きます。**

```css
.container {
  display: flex;    /* ここに書く */
}
```

`display: flex` を書いた要素は **Flex コンテナ**になり、
**その直接の子**が **Flex アイテム**として並びます。

**これは初学者が最もよくやる間違いです。**

```css
/* 間違い：何も起きない（あるいは予期しない結果になる） */
.item {
  display: flex;
}
```

`.item` に書くと、`.item` の**中身**が横に並ぶ設定になります。
`.item` 同士は並びません。

**覚え方**：「**並べたいものの親に書く**」

---

**問 3.7 の解答**

次のうち3つ挙げられていれば正解です。

1. **CSS ファイルが読み込まれているか**
   → `body { background-color: pink; }` を書いて、背景が変わるか試す
2. **セレクタが合っているか**
   → F12 →「要素」タブで、自分のルールが表示されているか確認
3. **他のルールに上書きされていないか**
   → 「スタイル」パネルで、取り消し線が引かれていないか確認
4. **プロパティ名や値の書き間違いがないか**
   → 綴り、単位の有無、`;` の有無
5. **ブラウザのキャッシュ**
   → `Ctrl` + `Shift` + `R` で強制再読み込み
6. **ファイルを保存したか**
   → `Ctrl` + `S`

**解説**

**この順番で確認するのが重要です。**

1 → 2 → 3 と進むことで、
「そもそも読み込まれていない」のか「セレクタが違う」のか「負けている」のかを、
**切り分けながら**特定できます。

いきなり CSS をあちこち書き換えると、原因がわからなくなります。
**1つずつ確認してください。**

---

### 演習

### 演習 3.1 の解答

`shop.html`（`<body>` の中を `<div class="container">` で囲む）

```html
<body>
  <div class="container">
    <h1>ネコカフェ にゃんこ</h1>
    <p>2020年に開店した、のんびり過ごせるネコカフェです。</p>
    <!-- 以下、既存の内容 -->
  </div>
</body>
```

`<head>` に次を追加します。

```html
<link rel="stylesheet" href="style.css" />
```

`style.css`

```css
/* ===== リセット ===== */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* ===== 全体 ===== */
body {
  font-family: sans-serif;
  line-height: 1.7;
  color: #333;
  background-color: #f7f5f2;
}

.container {
  width: 800px;
  margin: 0 auto;
  padding: 40px 20px;
  background-color: #fff;
}

/* ===== 見出し ===== */
h1 {
  color: #e07b39;
  margin-bottom: 20px;
}

h2 {
  margin-top: 32px;
  margin-bottom: 12px;
}

/* ===== 本文 ===== */
p {
  margin-bottom: 12px;
}

ul,
ol {
  margin-bottom: 20px;
  padding-left: 24px;
}
```

**解説**

**1. リセットを最初に書く**

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

`*` は「すべての要素」を意味します。
ブラウザが勝手に付けている初期の余白を打ち消し、
`box-sizing` を全要素に適用しています。

**2. `margin: 0 auto` で中央寄せ**

```css
.container {
  width: 800px;
  margin: 0 auto;
}
```

`margin` の左右を `auto` にすると、**左右の余白が自動的に均等になります。**
結果として、要素が中央に配置されます。

**`width` の指定が必須**です。指定しないと横幅いっぱいに広がり、
中央寄せの効果が見えません。

**3. `line-height` は `body` に書く**

```css
body {
  line-height: 1.7;
}
```

`body` に書けば、中のすべての要素に受け継がれます（**継承**といいます）。
`p` に個別に書く必要はありません。

**4. リストの `padding-left`**

リセットで `padding: 0` にすると、
**`<ul>` の点（`・`）が画面の外に出て見えなくなります。**

```css
ul, ol {
  padding-left: 24px;
}
```

これで戻ります。リセットの副作用として、よく起きる現象です。

**5. 色の選び方**

| 用途 | 値 | 理由 |
|------|-----|------|
| 本文 | `#333` | 真っ黒（`#000`）は目が疲れる |
| 背景 | `#f7f5f2` | 真っ白より柔らかい印象になる |
| 見出し | `#e07b39` | アクセント色 |

**まだレスポンシブ対応していません**

`width: 800px` は固定値なので、スマホで見ると横スクロールが発生します。
これは演習 3.4 で対応します。

---

### 演習 3.2 の解答

```css
.card-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 240px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
}

.card h3 {
  margin-bottom: 8px;
  color: #e07b39;
}
```

**解説**

**1. `display: flex` は親（`.card-list`）に書く**

これが最重要ポイントです。
`.card` に書いてしまうと、カード**同士**は並びません。

**2. `gap: 20px` で隙間を作る**

```css
.card-list {
  gap: 20px;
}
```

`margin` で隙間を作ることもできますが、`gap` のほうが優れています。

| 方法 | 問題 |
|------|------|
| `.card { margin-right: 20px; }` | **最後のカードの右にも余白が余る** |
| `.card-list { gap: 20px; }` | カードとカードの**間だけ**に入る。折り返したときの行間にも効く |

**3. `flex-wrap: wrap` で折り返す**

これがないと、画面を狭めたときに**カードが潰れます**（折り返さずに縮む）。

```css
.card-list {
  flex-wrap: wrap;
}
```

これを付けると、入りきらないカードが次の行に移動します。

**動作確認**：ブラウザの幅を狭めていくと、3枚 → 2枚 → 1枚と変化するはずです。

**4. 幅を揃える**

```css
.card {
  width: 240px;
}
```

`width` を指定することで、中身の量に関係なく幅が揃います。

`box-sizing: border-box` が効いているので、
**`padding: 20px` と `border: 1px` を含めて 240px** になります。
（リセットで `*` に指定してあります）

**別解：幅を自動で伸ばす**

```css
.card {
  flex: 1 1 240px;   /* 伸びる・縮む・基準幅 240px */
}
```

こう書くと、**余ったスペースを分け合ってカードが伸びます。**
デザインの好みで使い分けてください。

---

### 演習 3.3 の解答

`HTML`

```html
<header class="site-header">
  <h1 class="site-title">にゃんこ</h1>
  <nav>
    <ul class="nav-list">
      <li><a href="shop.html">ホーム</a></li>
      <li><a href="menu.html">メニュー</a></li>
      <li><a href="access.html">アクセス</a></li>
    </ul>
  </nav>
</header>
```

`CSS`

```css
.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  background-color: #e07b39;
}

.site-title {
  color: #fff;
  font-size: 24px;
}

.nav-list {
  display: flex;
  gap: 24px;
  list-style: none;
}

.nav-list a {
  color: #fff;
  text-decoration: none;
}

.nav-list a:hover {
  color: #ffe0c0;
}
```

**解説**

**1. Flexbox を2箇所で使う**

これがこの演習の要点です。

| 場所 | 目的 |
|------|------|
| `.site-header` | サイト名とナビを**左右に分ける** |
| `.nav-list` | リンクを**横に並べる** |

`.site-header` にだけ書いても、`<ul>` の中のリンクは縦に並んだままです。
**`<ul>` にも `display: flex` が必要**です。

**2. `justify-content: space-between`**

```text
┌──────────────────────────────────────────┐
│  にゃんこ           ホーム メニュー アクセス  │
└──────────────────────────────────────────┘
   ↑ 左端                          右端 ↑
```

**両端に寄せて、間のスペースを空ける**という指定です。
「左にロゴ、右にメニュー」というヘッダーの定番パターンです。

**3. `align-items: center`**

サイト名（24px）とリンク（16px）は文字サイズが違うため、
指定しないと**上端で揃ってしまい**、見た目がずれます。

`center` にすると、**高さの中央で揃います。**

**4. `list-style: none`**

`<ul>` に付く点（`・`）を消します。
**ナビゲーションでは必ず消す**ので、覚えておいてください。

**5. リンクの下線**

```css
.nav-list a {
  text-decoration: none;    /* 下線を消す */
}

.nav-list a:hover {
  color: #ffe0c0;           /* マウスを乗せたら色を変える */
}
```

`:hover` は**擬似クラス**で、「マウスが乗っている状態」を指します。

> **下線を消すなら、代わりの手がかりを残す**
> 下線を消すと「これはリンクだ」という手がかりが減ります。
> `:hover` で色を変えるなどして、**押せることが伝わるように**してください。

**6. なぜ `.site-header` と `.nav-list` に class を付けたのか**

```css
/* これでも動くが... */
header { display: flex; }
nav ul { display: flex; }
```

動きますが、**ページ内に別の `<header>` や `<ul>` が増えたときに巻き込まれます。**
`class` を付けておけば、影響範囲を限定できます。

---

### 演習 3.4 の解答

**方法1：メディアクエリで縦並びにする**

```css
.card-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 240px;
}

@media (max-width: 768px) {
  .card-list {
    flex-direction: column;
  }

  .card {
    width: 100%;
  }
}
```

**方法2：カードの幅だけを変える**

```css
@media (max-width: 768px) {
  .card {
    width: 100%;
  }
}
```

`flex-wrap: wrap` があるので、幅を 100% にすれば自動的に1列になります。
**方法2のほうが簡潔です。**

**あわせて必要な修正**

演習 3.1 の `.container` が `width: 800px` の固定値のままだと、
**スマホで横スクロールが発生します。** 次のように直してください。

```css
.container {
  width: 100%;
  max-width: 800px;    /* 800px までは広がるが、それ以上は広がらない */
  margin: 0 auto;
  padding: 40px 20px;
}
```

| プロパティ | 意味 |
|-----------|------|
| `width: 100%` | 親の幅いっぱいに広がる |
| `max-width: 800px` | ただし 800px を超えない |

**この `width: 100%` + `max-width` の組み合わせは、レスポンシブの基本形です。**
非常によく使うので覚えてください。

**画像のはみ出し対策**

画像を使っている場合は、次も必ず書いてください。

```css
img {
  max-width: 100%;
  height: auto;
}
```

**解説：`<meta name="viewport">` の確認**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

**これがないと、メディアクエリは一切効きません。**

スマホは「幅 980px の画面」として描画してから全体を縮小するため、
`max-width: 768px` の条件に**永遠に当てはまらない**からです。

**スマホ表示モードにしても何も変わらない**という場合は、
まずこの1行を確認してください。

**確認手順**

1. `F12` で開発者ツールを開く
2. `Ctrl` + `Shift` + `M`（macOS: `command` + `shift` + `M`）でスマホ表示モードに切り替え
3. 上部のドロップダウンから「iPhone SE」など幅の狭い機種を選ぶ
4. **横スクロールバーが出ていないこと**を確認する
5. カードが縦1列に並んでいることを確認する

---

### 演習 3.5 の解答

この演習は自由度が高いため、**解答例**を示します。
完成条件を満たしていれば、細部が違っていても問題ありません。

`index.html`

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ネコカフェ にゃんこ</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <header class="site-header">
      <h1 class="site-title">にゃんこ</h1>
      <nav>
        <ul class="nav-list">
          <li><a href="#menu">メニュー</a></li>
          <li><a href="#access">アクセス</a></li>
        </ul>
      </nav>
    </header>

    <main>
      <section class="hero">
        <h2 class="hero-title">ネコと過ごす、しずかな時間</h2>
        <p class="hero-text">12匹のネコがお待ちしています。</p>
      </section>

      <section id="menu" class="section">
        <div class="container">
          <h2 class="section-title">メニュー</h2>
          <div class="card-list">
            <div class="card">
              <h3>お試しプラン</h3>
              <p class="price">700円</p>
              <p>30分。はじめての方に。</p>
            </div>
            <div class="card">
              <h3>通常プラン</h3>
              <p class="price">1,200円</p>
              <p>1時間。ドリンク1杯付き。</p>
            </div>
            <div class="card">
              <h3>フリープラン</h3>
              <p class="price">3,000円</p>
              <p>時間制限なし。ドリンク飲み放題。</p>
            </div>
          </div>
        </div>
      </section>

      <section id="access" class="section">
        <div class="container">
          <h2 class="section-title">アクセス</h2>
          <table class="info-table">
            <tbody>
              <tr>
                <th>住所</th>
                <td>東京都千代田区1-1-1 にゃんこビル 2F</td>
              </tr>
              <tr>
                <th>営業時間</th>
                <td>10:00 - 20:00</td>
              </tr>
              <tr>
                <th>定休日</th>
                <td>毎週水曜日</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <p>&copy; 2026 ネコカフェ にゃんこ</p>
    </footer>
  </body>
</html>
```

`style.css`

```css
/* ===== リセット ===== */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

img {
  max-width: 100%;
  height: auto;
}

/* ===== 全体 ===== */
body {
  font-family: sans-serif;
  line-height: 1.7;
  color: #333;
}

.container {
  width: 100%;
  max-width: 960px;
  margin: 0 auto;
  padding: 0 20px;
}

/* ===== ヘッダー ===== */
.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  background-color: #e07b39;
}

.site-title {
  color: #fff;
  font-size: 24px;
}

.nav-list {
  display: flex;
  gap: 24px;
  list-style: none;
}

.nav-list a {
  color: #fff;
  text-decoration: none;
}

.nav-list a:hover {
  color: #ffe0c0;
}

/* ===== メインビジュアル ===== */
.hero {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 360px;
  background-color: #fdf1e6;
  text-align: center;
  padding: 0 20px;
}

.hero-title {
  font-size: 36px;
  margin-bottom: 12px;
  color: #b35c1e;
}

/* ===== セクション ===== */
.section {
  padding: 60px 0;
}

.section-title {
  text-align: center;
  margin-bottom: 32px;
}

/* ===== カード ===== */
.card-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.card {
  width: 280px;
  padding: 24px;
  border: 1px solid #eee;
  border-radius: 8px;
  background-color: #fff;
}

.card h3 {
  margin-bottom: 8px;
}

.price {
  font-size: 24px;
  font-weight: bold;
  color: #e07b39;
  margin-bottom: 8px;
}

/* ===== 表 ===== */
.info-table {
  width: 100%;
  border-collapse: collapse;
}

.info-table th,
.info-table td {
  padding: 12px;
  border-bottom: 1px solid #eee;
  text-align: left;
}

.info-table th {
  width: 140px;
  background-color: #faf8f6;
}

/* ===== フッター ===== */
.site-footer {
  padding: 24px;
  background-color: #333;
  color: #fff;
  text-align: center;
}

/* ===== スマホ対応 ===== */
@media (max-width: 768px) {
  .site-header {
    flex-direction: column;
    gap: 12px;
  }

  .hero {
    height: 280px;
  }

  .hero-title {
    font-size: 26px;
  }

  .card {
    width: 100%;
  }

  .info-table th {
    width: 100px;
  }
}
```

**解説**

**1. `.hero` で `flex-direction: column` を使っている理由**

```css
.hero {
  display: flex;
  flex-direction: column;    /* 縦に並べる */
  justify-content: center;   /* 主軸（=縦）の中央 */
  align-items: center;       /* 交差軸（=横）の中央 */
  height: 360px;
}
```

見出しとキャッチコピーを**縦に並べたまま、全体を中央に置きたい**ので、
`flex-direction: column` にしています。

このとき、**`justify-content` と `align-items` の意味が入れ替わります。**

| `flex-direction` | `justify-content` | `align-items` |
|------------------|-------------------|---------------|
| `row`（初期値） | 横 | 縦 |
| `column` | **縦** | **横** |

「主軸か交差軸か」で覚えていれば混乱しません。

**2. `border-collapse: collapse`**

```css
.info-table {
  border-collapse: collapse;
}
```

これを書かないと、**セルごとの枠線が二重になって隙間ができます。**
表を作るときは、ほぼ必ず書きます。

**3. `.container` を `<section>` の中に入れている**

```html
<section id="access" class="section">
  <div class="container">
    ...
  </div>
</section>
```

こうすると、**背景色はセクション全体に広がるが、中身は 960px で中央に収まる**
というレイアウトが作れます。

背景色を画面いっぱいに広げたいときの定番パターンです。

**4. `id="menu"` とページ内リンク**

```html
<a href="#menu">メニュー</a>
...
<section id="menu">
```

`#` の後ろに `id` の値を書くと、その位置までスクロールします（2.3.2 参照）。

滑らかにスクロールさせたい場合は、次を追加します。

```css
html {
  scroll-behavior: smooth;
}
```

**5. 作る順番が最重要**

この演習で本当に大事なのは、コードの中身より**作り方**です。

```text
1. HTML の骨組みだけ作る（中身は仮の文字でよい）→ 確認
2. リセット CSS と .container → 確認
3. ヘッダー → 確認
4. メインビジュアル → 確認
5. カード → 確認
6. 表 → 確認
7. フッター → 確認
8. メディアクエリ → 確認
```

**1つ作るたびに保存してブラウザで確認**してください。

一気に全部書いてから確認すると、崩れたときに
**どこが原因か特定できなくなります。**
これは第4章以降の JavaScript でも、実務でもまったく同じです。

**6. 完成後のチェック**

- `F12` →「コンソール」タブにエラーが出ていないか
- `Ctrl` + `Shift` + `M` でスマホ表示にして、**横スクロールが出ないか**
- ナビゲーションのリンクをクリックして、正しくスクロールするか

---

## 第4章

> 🚧 執筆中です。

---

## 第5章

> 🚧 執筆中です。
