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
> python-text の演習 1.3 の解答を読みましたが、
> なぜ import した名前でパッケージを呼び出せるのかが納得できません。
> ```

---

## 第1章

### 理解度チェック

**問 1.1 の解答**

- Windows: **`python`**
- macOS: **`python3`**

**解説**

macOS には、システムが使う別の `python` が存在したり、まったく無かったりします。
そのため、公式インストーラーで入れた Python 3 は **`python3`** という名前で呼び出します。
Windows はインストーラーが `python` を用意してくれるので、そのまま `python` で動きます。

このテキストの本文は、環境依存の手順をすべて Windows（`python`）/ macOS（`python3`）の
両方で示しています。自分の OS のブロックを読んでください。

---

**問 1.2 の解答**

- 名前：**REPL**
- 抜ける命令：**`exit()`**（`quit()` でも可。ショートカットなら Windows は `Ctrl` + `Z` → Enter、macOS は `Ctrl` + `D`）

**解説**

REPL は、`>>>` のあとに式を打つと、その場で結果が返ってくる対話モードです。
電卓のように使えるので、「この書き方で合っているかな」をすぐ試せます。

ただし REPL に打った内容は**保存されません**。
繰り返し動かすプログラムは、ファイル（`.py`）に書いて保存します（1.3.4）。

---

**問 1.3 の解答**

- （1）エラーの種類：**`NameError`**
- （2）行番号：**3行目**（`line 3`）

**解説**

トレースバックは**下から上**に読みます。

1. いちばん下の `NameError: name 'nummber' is not defined` が、エラーの**種類と説明**です
   （「`nummber` という名前は定義されていません」）
2. `File "app.py", line 3` が、**どのファイルの何行目か**を示します
3. `^^^^^^^` が、特に怪しい場所（`nummber`）を指しています

この場合、3行目の `nummber` が `number` の打ち間違いだと見当がつきます。

---

**問 1.4 の解答**

**プロジェクトごとに必要なライブラリのバージョンが違っても、互いに衝突しないようにできる。**

**解説**

パソコン全体に1か所だけライブラリを入れると、
「プロジェクトAは `requests` の v2.31、プロジェクトBは v2.20 が必要」というときに、
どちらか片方しか置けず、もう片方が動かなくなります（1.5.1）。

仮想環境（venv）を使うと、プロジェクトごとに専用の入れ物（`.venv`）を持てるので、
中に別々のバージョンを入れても干渉しません。

---

**問 1.5 の解答**

**「作る」コマンド。**

**解説**

- 作る：`python -m venv .venv`（`.venv` という入れ物を作る）
- 有効化する：Windows は `.\.venv\Scripts\Activate.ps1`、macOS は `source .venv/bin/activate`

作っただけでは使われません。**有効化して初めて**、その仮想環境の中にライブラリが入るようになります。
有効化できているかは、ターミナルの先頭に `(.venv)` が付いているかで見分けます（1.5.3）。

---

**問 1.6 の解答**

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

**解説**

Windows には、安全のためにスクリプトの実行を止める「実行ポリシー」という設定があり、
これが有効化スクリプト（`Activate.ps1`）をブロックしていました。

上のコマンドは、
- `-Scope CurrentUser`：いまログインしているあなただけに適用
- `RemoteSigned`：自分のパソコンで作ったスクリプトは許可し、ネットから落としたものは署名を求める

という、開発でよく使われる安全側の設定に変えるものです（1.5.5）。
確認を求められたら `Y` を押します。設定後、もう一度有効化を試してください。

---

### 演習問題

### 演習 1.1 の解答

`greet.py`

```python
print("こんにちは、山田です。")
print("Python の学習を始めました。")
```

**Windows（PowerShell）**

```powershell
python greet.py
```

**macOS / Linux**

```bash
python3 greet.py
```

```text
実行結果:
こんにちは、山田です。
Python の学習を始めました。
```

**解説**

`print(...)` は、かっこの中の文字を1行として画面に出す命令です。
`print` を2つ並べれば、2行表示されます。名前の部分は自分のものに変えて構いません。

> **よくある間違い**
> - 文字を囲む引用符（`"`）が全角（`”`）になっていると `SyntaxError` になります。半角で打ってください。
> - `print` を `Print`（大文字始まり）と書くと `NameError` になります。Python は大文字と小文字を区別します。

---

### 演習 1.2 の解答

ターミナルでの操作（Windows / macOS 共通の流れ。コマンドの `python` は macOS では `python3`）。

**1. 新しいディレクトリを作って移動する**

```powershell
cd ~/Desktop
mkdir venv-practice
cd venv-practice
```

**2. 仮想環境を作る**

**Windows（PowerShell）**

```powershell
python -m venv .venv
```

**macOS / Linux**

```bash
python3 -m venv .venv
```

**3. 有効化する**

**Windows（PowerShell）**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

先頭に `(.venv)` が付いたことを確認します。

```text
(.venv) PS C:\Users\yamada\Desktop\venv-practice>
```

**4. 一覧を確認する**

```powershell
pip list
```

```text
実行結果の例:
Package    Version
---------- -------
pip        24.3.1
```

**5. 無効化する**

```text
deactivate
```

先頭の `(.venv)` が消えれば成功です。

**解説**

`python-lesson` とは別に `venv-practice` を作らせたのは、
**仮想環境がプロジェクトごとに独立している**ことを体験してもらうためです。
作ったばかりの仮想環境には、まだ `pip` くらいしか入っていません（4 の `pip list` で確認できます）。

> **よくある間違い**
> Windows で 3 のときに「スクリプトの実行が無効になっている」というエラーが出たら、
> 実行ポリシーの設定が必要です。解答の問 1.6 のコマンドを実行してから、もう一度有効化してください（1.5.5）。

---

### 演習 1.3 の解答

**1. 仮想環境を有効化する**（演習 1.2 の `venv-practice` の中で）

**Windows（PowerShell）**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

**2. パッケージをインストールする**

```powershell
pip install cowsay==6.1
```

**3. パッケージを使うファイルを書く**

`use_package.py`

```python
import cowsay

cowsay.cow("パッケージを使えました")
```

**4. 実行する**

**Windows（PowerShell）**

```powershell
python use_package.py
```

**macOS / Linux**

```bash
python3 use_package.py
```

```text
実行結果:
  _______________________
| パッケージを使えました |
  =======================
                \
                 \
                   ^__^
                   (oo)\_______
                   (__)\       )\/\
                       ||----w |
                       ||     ||
```

**5. 環境を書き出す**

```powershell
pip freeze > requirements.txt
```

`requirements.txt` の中身:

```text
cowsay==6.1
```

**解説**

この演習は、**「有効化 → インストール → import して使う → 書き出す」**という
1.6 の一連の流れを、自分のディレクトリで通しで再現するものです。

`import cowsay` で取り込み、`cowsay.cow(...)` で呼び出しています。
本文の `moo.py`（1.6.2）と同じ形です。

> **よくある間違い**
> - `ModuleNotFoundError: No module named 'cowsay'` が出たら、
>   **有効化していない環境で実行している**のが原因のことがほとんどです。
>   ターミナルの先頭に `(.venv)` が付いているかを確認してください（1.6.2）。
> - `pip freeze` の結果が空になるのは、有効化した環境に何もインストールできていないためです。
>   2 のインストールが成功したか（`Successfully installed cowsay-6.1` が出たか）を見直してください。

> **別解：コマンドラインから直接使う**
> `cowsay` は、ファイルを書かずにコマンドから使うこともできます。
>
> ```powershell
> python -m cowsay -t "コマンドから使えました"
> ```
>
> ただし演習の完成条件は「ファイルから `import` して使う」なので、
> 提出する解答は `use_package.py` のほうです。
