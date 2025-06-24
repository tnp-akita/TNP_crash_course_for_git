---
marp: true
---

# TNP 初年次講義 [Git]

(ppt版が必要かわからんので git でアウトライン管理)

---

# Git をはじめよう

どうしてみんな Git を使うんだ

---

## Git を使った作業の流れ

1. 取り組む作業の方針を決めます
2. 作業用の branch に切り替えます
3. 作業を実行します
4. 作業が終わったら対象のファイルを add して commit します
5. リモートから pull します
6. rebase か merge の適切な手法でローカルを更新します
7. push します

---

## Git を使わない作業の流れ

1. 作業を実行します

---

## うれしいこと

- 「変更」が保存される
  - 変更を分けると複数作業を並列で進められる
  - 変更を取り組むと複数人で同時作業できる
  - 変更を遡ると作業履歴が見える

## うれしくないこと

- 作業が増えます
  - ブランチを作る、作業内容を統合(マージ)する...
- 必要知識が増えます

---

## でもなぜ Git？

- 作業内容が端的に表現できる
  - 作業を保存するとき「概要」(commit message)を書くので
- 誤った作業で環境を破壊しても時を戻せる
  - 変更を取り消して以前の状況を復元できるので
- 複数人で同時でない作業ができる
  - 非同期でコードの同時編集をしないので

---

# Git の基本概念

- [ ] 図を出す、Commit, Merge, Branch が見えればいいかな

---

# Git の操作たち

- ひとりで使うために

---

## add

- commit 対象としてファイルを追加します
- これを「ステージング」といいます

```bash
git add [追加対象のファイルへのパス]
```

- ディレクトリを追加する場合はその階層以下もすべてステージングします
- ステージング後の変更は自動でステージングされないのでcommitできません。``git status`` で確認してね

---

## ステータス (status)

git が認識している変更・作業の状況を確認します

- コミットされていないファイル、コミットされたがまだプッシュされていないファイル、変更されたファイルの一覧が出てきます。

```bash
git status
```

---

## コミット (commit)

- コミット: 「作業を保存」します
  - ステージングされているファイルを履歴に追加します
  - **コミットメッセージ** を指定して、いままでの作業を説明する必要があります

- [ ] 図で言うどこか出す

```bash
git commit
```

---

## チェックアウト

ブランチを切り替えるか作成します

- コミットしていない変更があるときは実行できません
- 移動したいときは `stash` を使います
- 現在のブランチを確認するには `git status` が使えます

```bash
git checkout <切り替え先のブランチ名>
```

- `origin/main` のブランチを見に行くときには

```bash
git checkout origin/main
```

- `-c` をつけて新規作成できます
- `add-title-screen` というブランチを作るには

```bash
git checkout -c add-title-screen
```

> [!warning]
ブランチを切り替えてからはそのブランチにしかコミットができません。もし、間違ったブランチで作業をしてしまった場合は以下のコマンドを使うと、作業内容を残したままブランチを切り替えることができます。```git switch -c <ブランチ名>```

---


## マージ (merge)

いまいるブランチに、別のブランチのコミットをすべて取り込みます

- merge は統合って意味です

```bash
git merge [取り込みたいコミット・ブランチ]
```

---

### マージの競合 (conflict)

別のブランチの変更を取り込むとき、矛盾が生じて取り込めない場合があります

#### 競合の起こる例

- `main` ブランチにこんな文章があるとします

```diff
GitHub は、共同編集や履歴管理に留まらず、コミュニケーションの機会を提供するサービスです。
世界中で9400万人が利用しています。
```

<div class="row">
<div class="col">
Aさんの変更 @ <code>main</code> ブランチ:

世界中で<b><u>9400万人</u></b>が利用しています。
</div>
<div class="col">
Bさんの変更 @ <code>fix-document</code> ブランチ:

世界中で<b><u>1億人以上</u></b>が利用しています。
</div>
</div>

- ここで `main` ブランチに `fix-document` ブランチをマージする場合、Git はどちらを信じればいいかわからないため競合が発生します

---

### マージの解決 (resolve)

競合が起きたら解決する必要があります

- 競合が起きているファイルは `git status` で確認できます。開いてみると、以下のようにファイルが変更されています

<div class="row">
<div class="col">
<pre><code>世界中で
<<<< main
9400万人
====
1億人以上
>>>> fix-document
が利用しています。
</code></pre>
</div>
<div class="col">
いま起きている競合は
<ul>
<li><code>main</code> ブランチには「9400 万人」</li>
<li><code>fix-document</code> ブランチには「1億人以上」</li>
</ul>
</div>
</div>

- 採用するほうを残して(Accept)、もう片方を削除(Discard)しましょう
- `<<<<` `====` `>>>>` (実際には7文字)の行を削除し、必要ない変更も削除します
- これが終わったら、最後にステージングとコミットをして変更を保存します

---

たとえば「1億人以上」が正しいときは、

```plaintext
世界中で1億人以上が利用しています。
```

と書き直して、`add` & `commit` しなおすと競合が解決し、マージが完了します。

- これを「マージコミット」と呼びます

---

## スタッシュ (stash)

- 現在の変更をしまっておきます
- ファイルの中身が、最後にコミットした時点に戻ります
- コミットしていない変更をしまうには

```bash
git stash
```

- 取り出すには

```bash
git stash pop
```

---

## ログ (Log)

- 今いるブランチにおけるコミットの履歴を見ます

```bash
git log
```

- ブランチを横断した履歴は `git reflog` で確認できます

---

## みんなで使うために

---

## Local vs. Remote

- Git を使うときには(たいてい)2箇所のリポジトリを同時に扱います
  - 「ローカルリポジトリ」: パソコン上のリポジトリ
  - 「リモートリポジトリ」: パソコン上にないリポジトリ

- 「リモートリポジトリ」は...
  - GitHub などいろんなサービスが提供しています
  - `origin` と呼ばれたりします

- `fetch` `pull` `push` コマンドはローカルとリモートを同期するために使います

---

## Remote の使い道

他人との共同作業はコミットを公開することで行います

- `fetch`: リモートの状況をのぞき見します
- `pull`: リモートからローカルにコミットを持ってきます
- `push`: リモートにコミットを公開します

---

## フェッチ (fetch)

リモートに存在するブランチやコミットをダウンロードして見られるようにします
(作業環境に変更を反映することはありません)

- `リモート名/ブランチ名` というブランチが作成されます

```bash
git fetch [リモート名]
```

---

## プル (pull)

リモートに保存されているコミットをローカルに持ってきて、さらにリモートの変更をローカルに反映します。つまり、誰かの行った変更をすべて取り込みます。
まだコミットしていないディレクトリやファイルがあるときは pull できません。スタッシュするか変更内容をコミットしましょう。

```bash
git pull <どこから> <どのブランチを>
```

e.g. `origin` (GitHub) から `main` ブランチにあるコミットを持ってきて、ローカルリポジトリに反映するには

```bash
git pull origin main
```

---

### プルの出力

```plaintext
From https://github.com/tnp-akita/TNP-GitHub-Intro
 * branch            main       -> FETCH_HEAD
Already up to date.
```

---

### プル の動作

> [!TIP]
> `pull` は `fetch` + `merge` と表現されます

```bash
git pull origin main
```

- リモート `origin` にある `main` をローカルに取り込む

これは、以下のように書き換えられます

```bash
git fetch origin
git merge origin/main main
```

- `fetch` でリモートの状況を `origin/` 以下に反映して、その後 `merge` で `origin/main` を `main` にマージする

---

## プッシュ (push)

- コミットをリモートリポジトリに反映します
- push したcommitは(力技を使わない限り)修正できません

```bash
git push <どこに> <どのブランチを>
```

e.g.  `origin` (GitHub) に `main` ブランチのコミットをプッシュして公開するには

```bash
git push origin main
```

---

## Remote の確認

リモートリポジトリの設定を見たい場合、以下のコマンドで確認できます

```bash
git remote -v
```

```plaintext
origin  https://github.com/NitCelcius/TNP-GitHub-Intro.git (fetch)
origin  https://github.com/NitCelcius/TNP-GitHub-Intro.git (push)
```

- `origin` というリモートには `https://github.com/NitCelcius/TNP-GitHub-Intro.git` が設定されているようです

---

## Remote の修正

もしリモートリポジトリのURLを変更する必要がある場合は、[Remote の確認](#remote-の確認) で現在の設定を確認しましょう
修正は以下のコマンドで行います

```bash
git remote set-url [リモート名] [URL]
```

> [!TIP]
> `git remote add` (リモートの追加) や `git remote remove` (リモートの削除)、`git remote rename` (リモートの名前変更) も同様に使えます。
> `git remote -?` で用法を確認してね

---

### (メモ) Remote の修正例

```bash
git remote set-url origin https://github.com/tnp-akita/TNP_crash_course_for_git
```

- `--push` スイッチを指定するとプッシュ先だけを変更します

```plaintext
origin  https://github.com/tnp-akita/TNP_crash_course_for_git (fetch)
origin  https://github.com/tnp-akita/TNP_crash_course_for_git (push)
```
