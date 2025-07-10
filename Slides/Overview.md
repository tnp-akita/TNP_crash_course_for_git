---
marp: true
---

<style>
@import 'default';
/* Bootstrap */
@import url('https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css');
</style>

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

# TNP 初年次講義 (Git)

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

![Git のコミットツリー。mainブランチから複数のブランチが分岐し、それらは特定の箇所で合流している / center](/images/Multi-fast-forward-merge.svg)

Git では変更履歴が記録され、上のような図で表されます

- 「コミット(円)」はひとつの変更
- それがつながったものを「ブランチ」
- これが合流することを「マージ」

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
  - ステージングされているファイルのみを履歴に追加します
  - **コミットメッセージ** を指定して、いままでの作業を説明する必要があります

![Commit は変更を保存するノード / center](/images/Commit-emphasized.svg)

```bash
git commit -m [コミットメッセージ]
```

- [#ログ](#ログ-log) を参照するとコミットの履歴を参照できます

---

### コミットの詳細を見つつコミットするには

```bash
git commit
```

- `-m` オプションを指定しないで実行すると、テキストエディタが開きます
  - デフォルト設定では Vim が開くことがあります
  - 下に `["COMMIT_EDITMSG" 11L, 231B]` など記載されている場合は [Vimの使い方解説記事(@alpaca-honke氏)](https://qiita.com/alpaca-honke/items/b7a682f6ee4aedada544#%E4%BD%BF%E3%81%84%E6%96%B9%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E5%91%BD) を見て Vim と仲良くなりましょう
  - どうしても Vim が好きになれない場合: `core.editor を変更する`

---

```plaintext

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   hello.txt
```

- コミット対象のブランチ、対象のファイルが表示されます
- 最初の行には概要 (**コミットメッセージ**)、後の行には詳細を書く風習があります
- `#` のない行に何か書き、保存してエディタを閉じるとコミットを実行します

> [!TIP]
> ここで何も書かずにエディタを閉じると、コミットせず元の画面に戻ります。

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

![右端がマージコミット / center](/images/Fast-forward-merge.svg)

---

### マージの戦略

マージを行う際には、以下の戦略 (strategy) があります。戦略によってマージの仕方が変わります

- (単なる) Merge
- Squash and merge
- Rebase (and merge)

---

#### Merge (単にマージ)

マージ元のコミットをマージ先のブランチの終端へすべてコピーします。

マージ先に変更がある場合は、マージしたことを表す**「マージコミット」を加えて**(Non-fast-forward)マージを行います。

![Non Fast-forward merge / center](/images/Fast-forward-merge.svg)

- 😄 マージ元のブランチのコミットがそっくりコピーされるので理解しやすい
- 😣 マージコミットが追加されるので、履歴の参照が大変になる (直線でなくなる)

---

#### Squash and merge (ひとまとめ)

マージ元のコミットを**1つのコミットにまとめ**、それをマージ先のブランチに追加します。

![Squash(まとめて) merge / center](/images/Squash-merge.svg)

- 😄 マージ元のブランチに大量のコミットがあっても、マージ先はコンパクトな履歴を保てる
- 😣 マージ元のブランチを見ないと、特定の変更だけを取り出せない
  - 複数人が同じファイルを編集すると競合が大きくなりやすい (経験談)

---

### Rebase (and merge)

「マージ先のブランチのコミット後に、マージ元のブランチのコミットが行われた」かのように**コミットを改変**(Rebase)したのち、それらをマージ先に追加します

![Rebase (and merge) / center](/images/Rebase-merge.svg)

- 😄 終端にコミットが追加されるので、履歴が直線的になり理解しやすい
- 😣 履歴を書き換えるので force push を行う必要がある

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

# みんなで使うために

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

```bash
git remote set-url origin https://github.com/tnp-akita/TNP_crash_course_for_git
```

- `--push` スイッチを指定するとプッシュ先だけを変更します

```plaintext
origin  https://github.com/tnp-akita/TNP_crash_course_for_git (fetch)
origin  https://github.com/tnp-akita/TNP_crash_course_for_git (push)
```

---

## 作成者(Author)を変更する

何も設定せず `git commit` を行うと以下のメッセージが出ることがあります。

```bash
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

メッセージに従い、メールアドレスとユーザー名を設定しましょう。

---

> [!NOTE]
> 以下のコマンドで入力した内容は、一度でもコミットを行うと、該当のリポジトリにアクセスできる全員に表示されます。
>
> なお、これを非公開にしたい場合は、GitHub でコミット専用のメールアドレスを使うこともできます。[コミットメールアドレスを設定する - GitHub Docs](https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address#about-commit-email-addresses:~:text=%5BKeep%20my%20email%20address%20private%5D(%E3%83%A1%E3%83%BC%E3%83%AB%20%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9%E3%82%92%E9%9D%9E%E5%85%AC%E9%96%8B%E3%81%AB%E3%81%99%E3%82%8B)%20%E3%82%92%E9%81%B8%E6%8A%9E%E3%81%97%E3%81%BE%E3%81%99) を参照してください。

メールアドレスは以下のように設定します。

```bash
git config --global user.email [メールアドレス]
```

ユーザー名は以下のように設定します。

```bash
git config --global user.name [作成者名]
```

---

作成者名は、単に先ほどのコマンドの引数を書かないことで確認できます。

```bash
$ git config --global user.email
nitcelcius@nitcelcius.me

$ git config --global user.name
NitCelcius
```

---

### リポジトリごとに作成者を変える

複数のアカウントを使っているなどの理由で、特定のリポジトリだけの設定や作成者を変更したい場合は `git config` の `--local` オプションを使います。

```bash
git config --local user.email [このリポジトリで使いたいメールアドレス]
git config --local user.name [このリポジトリで使いたい作成者名]
```
