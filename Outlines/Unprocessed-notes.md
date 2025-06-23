---
marp: false
---

# 6.「適宜行う」の詳細な説明

※ 個々のコマンドの説明は下記参照

1. ほかの人の変更を取り込みます。 `git pull <どこから> <どのブランチを>` を行います
2. ファイルやフォルダなどの作業を行います。開発作業とか
3. 変更したファイルを `git add` してステージングし、コミット対象に追加します。
4. `git commit` でコミットします。
5. `git push <どこへ> <どのブランチを>` を使ってリモートリポジトリに変更を反映します。このとき誰かがすでに `push` しているとエラーメッセージが出る場合があります。
6. `git commit` でコミットします。
7. `git push <どこへ> <どのブランチを>` を使ってリモートリポジトリに変更を反映します。このとき誰かがすでに `push` しているとエラーメッセージが出る場合があります。

---

# 基本的なアクションの説明

---

## ステージング (add)

作業内容をコミットの対象に追加する
- その時点での作業内容がコピーされます。したがって、ステージング後に変更した内容はコミットされません。

## 文法

- 特定のフォルダやファイルをステージングする

```bash
git add <ファイルやフォルダ名>
```

- 既存のフォルダやファイル**だけ**をステージングする

```bash
git add .
```

---

## コミット (commit)

作業内容を保存することです。`git add <ファイル名>` で「ステージング」したファイルだけが保存されます

## 文法

- コミットメッセージ (コミットの説明) が決まっていないとき (おすすめ、vimが起動する)

```bash
git commit
```

- コミットメッセージが決まっていて、コミットするファイルがわかっているとき

```bash
git commit -m "<コミットメッセージ>"
```

---

# トラブルシューティング

## Changed not staged for commit と赤いファイル名が見える

```bash
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

- `git add` を忘れています。ファイル名を指定して `git add <ファイル名>` を実行してからやり直しましょう。

---

## Your branch is up to date with と出る

```bash
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

- この英語の通り、変更されているファイルがありません。

---

## ステータス (status)

git が認識している変更・作業の状況を確認します。
- たとえばコミットされていないファイル、コミットされたがまだプッシュされていないファイル、変更されたファイルの一覧が出てきます。

## 文法

```bash
git status
```

---

## プッシュ (push)

コミットで保存した作業内容をリモートに反映します。
- push したcommitは(力技を使わない限り)修正できなくなります。

## 文法

```bash
git push <どこに> <どのブランチを>
```

e.g.  `origin` (GitHub) に `main` ブランチのコミットをプッシュして公開するには

```bash
git push origin main
```

---

## フェッチ (fetch)

リモートに存在する変更をダウンロードします(反映は行いません)
- `リモート名/ブランチ名` というブランチが作成されます

```plaintext
git fetch [リモート名]
```

---

## プル (pull)

リモートに保存されているコミットをローカルに持ってきて、さらにリモートの変更をローカルに反映します。つまり、誰かの行った変更をすべて取り込みます。
まだコミットしていないフォルダやファイルがあるときは pull できません。スタッシュするか変更内容をコミットしましょう。

## 文法

```bash
git pull <どこから> <どのブランチを>
```

e.g. `origin` (GitHub) から `main` ブランチにあるコミットを持ってきて、ローカルリポジトリに反映するには

```bash
git pull origin main
```

---

## チェックアウト

ブランチを切り替えるか作成します
- コミットやスタッシュしていない変更があるときは実行できません。
- 現在のブランチを確認するには `git status` を使います。

```bash
git checkout <切り替え先のブランチ名>
```

- `origin/main` のブランチを見に行くときには

```bash
git checkout origin/main
```

- `add-title-screen` というブランチを作成するには

```bash
git checkout -c add-title-screen
```

> [!warning]
ブランチを切り替えてからはそのブランチにしかコミットができません。もし、間違ったブランチで作業をしてしまった場合は以下のコマンドを使うと、作業内容を残したままブランチを切り替えることができます。```bash
git switch -c <ブランチ名>```