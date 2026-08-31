# 執筆タスク キュー

**このファイルは、クラウドルーティンが毎回読み込む「作業指示書」です。**
実行手順と完成条件は [`RUNBOOK.md`](./RUNBOOK.md) にあります。

---

## 使い方（ルーティン向け）

1. 状態が **`進行中`** のタスクがあれば、それを継続する
2. なければ、上から見て最初の **`未着手`** のタスクを**1件だけ**実行する
3. 終わったら、そのタスクの状態を **`完了`** に更新してコミットする

**1回の実行で1タスクだけ。** 次のタスクに進まないでください。

各タスクが書くべき節・項の一覧は、**対象の本の `README.md`** にあります。
このファイルには重複して書きません。

---

## 進捗

| 本 | 完了 / 全体 |
|----|-----------|
| react-text | **13 / 13（完成）** |
| python-text | 12 / 13 |
| fastapi-text | 0 / 12 |
| docker-text | 0 / 10 |
| mysql-text | 0 / 11 |
| **合計** | **25 / 59** |

---

## 1. react-text

章立て: [`react-text/README.md`](../../react-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| R-00 | 完了 | 第0章 はじめに | `react-text/00-introduction.md` | — | 中 | |
| R-01 | 完了 | 第1章 Web の仕組みと開発環境 | `react-text/01-web-and-environment.md` | part1 | 大 | |
| R-02 | 完了 | 第2章 HTML | `react-text/02-html.md` | part1 | 大 | |
| R-03 | 完了 | 第3章 CSS | `react-text/03-css.md` | part1 | 大 | |
| R-04 | 完了 | 第4章 JavaScript の基礎（前半） | `react-text/04-javascript-basics.md` | part1 | 大 | ★このテキストの山場。下の注記を必ず読む |
| R-05 | 完了 | 第5章 JavaScript の基礎（後半） | `react-text/05-javascript-advanced.md` | part1 | 大 | ★山場。5.7.7 が第6章への橋渡し |
| R-06 | 完了 | 第6章 React をはじめる | `react-text/06-react-start.md` | part2 | 中 | |
| R-07 | 完了 | 第7章 props と state | `react-text/07-props-and-state.md` | part2 | 大 | ★React の中核 |
| R-08 | 完了 | 第8章 状態設計と副作用 | `react-text/08-state-design-and-effects.md` | part2 | 大 | |
| R-09 | 完了 | 第9章 ルーティングと全体設計 | `react-text/09-routing-and-architecture.md` | part2 | 中 | |
| R-10 | 完了 | 第10章 実践：タスク管理アプリ | `react-text/10-practice-task-app.md` | part2 | 大 | |
| R-11 | 完了 | 第11章 次のステップ | `react-text/11-next-steps.md` | — | 小 | |
| R-FIN | 完了 | 通し確認 | — | — | 小 | 章立てとの一致・リンク・解答の対応を機械的に確認。用語のブレを修正し、glossary に未登録の 24 語を追記。残りの申し送りは `review-notes.md` へ |

### R-04 の注記

- 学習者が**生まれて初めてプログラムを書く章**です。ここでの離脱が最も多くなります
- 1つの項につき、**必ず「動かして結果を見る」ところまで**書いてください
- `console.log` は 4.1.3 で導入し、以降すべての例で結果を示すこと
- **`==` と `===`、`let` と `const`、スコープ**は、初学者が確実に詰まる箇所です。
  それぞれに「よくある間違い」の囲み記事を置いてください
- 4.6 の関数は次章以降のすべての土台です。分量を惜しまないでください
- **`map` / `filter` / 配列メソッド / 非同期は第5章の内容なので使わないこと**

### R-05 の注記

- **5.4.3「イミュータブルな更新の考え方」は第7章の state 更新の伏線**です。必ず入れてください
- 5.7.7「なぜこれが大変なのか」で、DOM 直接操作のつらさを体験させてから第6章に渡します。
  ここが React を学ぶ動機になるので、具体的なコード量の比較を見せてください

---

## 2. python-text

章立て: [`python-text/README.md`](../../python-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| P-00 | 完了 | 第0章 はじめに | `python-text/00-introduction.md` | — | 中 | 0.3 の JS 対応表が肝 |
| P-01 | 完了 | 第1章 環境構築 | `python-text/01-environment.md` | part1 | 大 | ★要検証（下の注記）。PATH 復旧・実行ポリシー・pip 例は review-notes.md に検証依頼あり |
| P-02 | 完了 | 第2章 基本文法 | `python-text/02-basics.md` | part1 | 大 | 2.6 でインデント説明のため `if` を書き方だけ先取り（詳細は第3章）。第3章のスタブを新規作成 |
| P-03 | 完了 | 第3章 制御構文 | `python-text/03-control-flow.md` | part1 | 中 | 3.1.4 で `in` / `for` のためリストを「作る・含むか調べる・回す」だけ先取り（詳細は第4章）。第4章のスタブを新規作成 |
| P-04 | 完了 | 第4章 データ構造 | `python-text/04-data-structures.md` | part1 | 大 | 4.4.2 のベン図を SVG→PNG で追加。第5章のスタブを新規作成 |
| P-05 | 完了 | 第5章 関数 | `python-text/05-functions.md` | part1 | 中 | 4章で先送りした `sorted()` の `key` を 5.5.3 で回収。第6章のスタブを新規作成 |
| P-06 | 完了 | 第6章 モジュールとパッケージ | `python-text/06-modules.md` | part2 | 中 | 解答編 その2（`91-answers-part2.md`）を新規作成。6.3.5 で `str.join` を追加説明。第7章のスタブを新規作成 |
| P-07 | 完了 | 第7章 ファイル操作と例外 | `python-text/07-files-and-exceptions.md` | part2 | 中 | 7.6.2 で `class ...(Exception):` のみ「例外を増やす決まった書き方」として先取り（詳細は第8章）。第8章のスタブを新規作成 |
| P-08 | 完了 | 第8章 オブジェクト指向 | `python-text/08-oop.md` | part2 | 大 | 8.5.2 で `@dataclass` のために型の注記を先取り（詳細は第9章）。第9章のスタブを新規作成 |
| P-09 | 完了 | 第9章 型ヒントとモダン Python | `python-text/09-typing-and-tools.md` | part2 | 中 | 9.4 の ruff / mypy の出力は実機で確認済み（バージョンは review-notes.md に検証依頼あり）。第10章のスタブを新規作成 |
| P-10 | 完了 | 第10章 実践：データ処理スクリプト | `python-text/10-practice-data-script.md` | part2 | 中 | 新規プロジェクト `sales-analyzer` を作る章。`requests`（Open-Meteo API）と `argparse` を導入。10.1.2 / 10.3.1 / 10.3.2 / 10.3.3 / 10.6.1 に Mermaid 図。第11章のスタブを新規作成 |
| P-11 | 完了 | 第11章 次のステップ | `python-text/11-next-steps.md` | part2 | 小 | 到達度チェックリスト・JS 対応表の答え合わせに加え、pytest / pandas / 自動化を入口だけ紹介。解答編 その2 を「第6章〜第11章」に改題し、第11章を追記 |
| P-FIN | 未着手 | 通し確認 | — | — | 小 | |

### P-00 / P-01 の注記

- **P-00 では、react-text を読んだ人向けの JavaScript 対応表（0.3.2）が価値の中心**です。
  `let/const` → 代入のみ、`===` → `==`、`{}` → インデント、配列 → リスト、
  オブジェクト → 辞書、`map` → 内包表記、`null/undefined` → `None` などを表にしてください
- **P-01 は必ず人間が実機で検証してください。** とくに次の2点：
  - Windows のインストーラで「Add Python to PATH」を入れ忘れたときの復旧手順
  - PowerShell の実行ポリシーで venv の有効化が失敗するときの対処

---

## 3. fastapi-text

章立て: [`fastapi-text/README.md`](../../fastapi-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| F-00 | 未着手 | 第0章 はじめに | `fastapi-text/00-introduction.md` | — | 小 | |
| F-01 | 未着手 | 第1章 Web API とは | `fastapi-text/01-web-api.md` | part1 | 中 | react-text 1.2 を前提に深掘り |
| F-02 | 未着手 | 第2章 FastAPI をはじめる | `fastapi-text/02-getting-started.md` | part1 | 中 | ★要検証 |
| F-03 | 未着手 | 第3章 パラメータを受け取る | `fastapi-text/03-parameters.md` | part1 | 中 | |
| F-04 | 未着手 | 第4章 Pydantic | `fastapi-text/04-pydantic.md` | part1 | 中 | v2 系で書くこと |
| F-05 | 未着手 | 第5章 プロジェクト構成 | `fastapi-text/05-project-structure.md` | part1 | 中 | |
| F-06 | 未着手 | 第6章 データベース連携 | `fastapi-text/06-database.md` | part2 | 大 | SQLite で始める |
| F-07 | 未着手 | 第7章 認証 | `fastapi-text/07-authentication.md` | part2 | 大 | ★下の注記 |
| F-08 | 未着手 | 第8章 テスト | `fastapi-text/08-testing.md` | part2 | 中 | |
| F-09 | 未着手 | 第9章 実践：React と繋ぐ | `fastapi-text/09-practice-connect-react.md` | part2 | 大 | react-text 第10章の成果物を使う |
| F-10 | 未着手 | 第10章 次のステップ | `fastapi-text/10-next-steps.md` | — | 小 | |
| F-FIN | 未着手 | 通し確認 | — | — | 小 | |

### F-07 の注記

認証はセキュリティに直結します。次を必ず守ってください。

- パスワードのハッシュ化は**必ず専用ライブラリ**を使う。自作しない
- 秘密鍵は環境変数から読む。**コード中にベタ書きした例を載せない**
- 「学習用なので簡略化」する場合は、**本番で何が足りないかを明記**する
- 7.6「やってはいけないこと」を省略しない

---

## 4. docker-text

章立て: [`docker-text/README.md`](../../docker-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| D-00 | 未着手 | 第0章 はじめに | `docker-text/00-introduction.md` | — | 小 | |
| D-01 | 未着手 | 第1章 Docker が解決する問題 | `docker-text/01-why-docker.md` | あり | 中 | 前3冊の苦労を具体的に回収する |
| D-02 | 未着手 | 第2章 インストールと基本操作 | `docker-text/02-install-and-basics.md` | あり | 大 | ★要検証。2.2 が生命線 |
| D-03 | 未着手 | 第3章 Dockerfile | `docker-text/03-dockerfile.md` | あり | 大 | |
| D-04 | 未着手 | 第4章 ボリュームとネットワーク | `docker-text/04-volumes-and-networks.md` | あり | 大 | |
| D-05 | 未着手 | 第5章 Docker Compose | `docker-text/05-compose.md` | あり | 大 | |
| D-06 | 未着手 | 第6章 実践：React + FastAPI + MySQL | `docker-text/06-practice-full-stack.md` | あり | 大 | ★3冊分の成果物を統合 |
| D-07 | 未着手 | 第7章 イメージの最適化と本番運用 | `docker-text/07-optimization.md` | あり | 中 | |
| D-08 | 未着手 | 第8章 次のステップ | `docker-text/08-next-steps.md` | — | 小 | |
| D-FIN | 未着手 | 通し確認 | — | — | 小 | |

> docker-text の解答編は分割せず `docker-text/90-answers.md` の1ファイルにまとめます。

### D-02 の注記

**この章で環境が立ち上がらないと、以降の章がすべて読めなくなります。**
2.2「よくあるトラブル」を厚く書いてください。最低限これらを網羅すること：

- WSL2 が未インストール／未更新（Windows）
- BIOS/UEFI で仮想化が無効
- Apple Silicon での `platform` 指定
- Docker Desktop 未起動での `Cannot connect to the Docker daemon`
- 企業プロキシ環境での `docker pull` 失敗

---

## 5. mysql-text

章立て: [`mysql-text/README.md`](../../mysql-text/README.md)

| ID | 状態 | 章 | 出力ファイル | 解答編 | 規模 | 備考 |
|----|------|----|------------|--------|------|------|
| M-00 | 未着手 | 第0章 はじめに | `mysql-text/00-introduction.md` | — | 小 | |
| M-01 | 未着手 | 第1章 データベースとは | `mysql-text/01-what-is-database.md` | part1 | 中 | |
| M-02 | 未着手 | 第2章 環境構築 | `mysql-text/02-environment.md` | part1 | 中 | ★練習用データを確定させる |
| M-03 | 未着手 | 第3章 データを取り出す（SELECT） | `mysql-text/03-select.md` | part1 | 大 | |
| M-04 | 未着手 | 第4章 データを変更する | `mysql-text/04-modify-data.md` | part1 | 中 | |
| M-05 | 未着手 | 第5章 テーブル設計 | `mysql-text/05-table-design.md` | part1 | 大 | |
| M-06 | 未着手 | 第6章 結合と集計 | `mysql-text/06-join-and-aggregate.md` | part2 | 大 | ★この本の山場 |
| M-07 | 未着手 | 第7章 インデックスと実行計画 | `mysql-text/07-index-and-explain.md` | part2 | 中 | |
| M-08 | 未着手 | 第8章 アプリから使う | `mysql-text/08-use-from-app.md` | part2 | 中 | |
| M-09 | 未着手 | 第9章 次のステップ | `mysql-text/09-next-steps.md` | — | 小 | フェーズ1完走の締め |
| M-FIN | 未着手 | 通し確認 | — | — | 小 | |

### M-02 の注記

**2.5 で決めた練習用テーブルを、第3章以降すべてで使い回します。**
ここで決めた構成は後から変えられないので、次を満たすものにしてください。

- 最低3テーブル（1対多と多対多の両方が作れること）
- 日本語データを含む（文字コードの問題を実感させるため）
- 集計の練習ができる数値列と日付列がある
- 投入用の SQL をコードブロックで全文提示する（学習者がコピペで再現できること）

第3章以降のタスクは、**この章で定義したテーブルだけを使って**例題を作ってください。

---

## `-FIN` タスクの内容

`R-FIN` / `P-FIN` / `F-FIN` / `D-FIN` / `M-FIN` は、1冊を書き終えたあとの仕上げです。
本文の新規執筆はせず、次を行います。

1. 全章を通しで読み、**用語のブレを統一する**（`docs/glossary.md` に照らす）
2. 同じ説明の重複を削る／前の章への参照に置き換える
3. 章間リンク・解答編へのリンクがすべて有効か確認する
4. `docs/style-guide.md` の「章を書き終えたらチェックリスト」を全章分で確認する
5. `config.yaml` の `chapters` と実ファイルが一致しているか確認する
6. `ai/curriculum-map.md` の該当セクションを最終版に更新する
7. その本の `README.md` の「執筆状況」を更新する
8. **人間向けの申し送りを `docs/writing-tasks/review-notes.md` に書く**
   （検証が必要な箇所、スクリーンショットが必要な箇所、自信のない記述）

---

## 状態の凡例

| 状態 | 意味 |
|------|------|
| `未着手` | まだ手をつけていない |
| `進行中` | 前回の実行で途中まで書いた。備考に到達点を記載 |
| `完了` | 本文・解答編・curriculum-map まで終わっている |
| `保留` | 意図的に飛ばす。備考に理由を記載 |

---

## 実行順序について

上から順に実行するのが基本ですが、**依存関係は次のとおり**です。

```
react-text  R-04 → R-05 → R-06 → R-07 → R-08 → R-09 → R-10 → R-11
                                                          ↓
python-text P-00 → ... → P-11                     （F-09 が R-10 の成果物を使う）
                    ↓
fastapi-text F-00 → ... → F-09 ←──────────────────────────┘
                    ↓
docker-text  D-00 → ... → D-06 ← fastapi-text と react-text の成果物を使う
                    ↓
mysql-text   M-00 → ... → M-08 ← docker-text 2章、fastapi-text 6章を前提
```

**冊をまたいで順番を入れ替えないでください。**
後の本は、前の本で作ったアプリを題材にします。
