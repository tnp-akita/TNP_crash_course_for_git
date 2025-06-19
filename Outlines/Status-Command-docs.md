---
marp: true
---

# ふろく: git status の例

git とコミュニケーションしましょう

---

## 復習: git status コマンド

git が認識している変更・作業の状況を確認します

- たとえばコミットされていないファイル、コミットされたがまだプッシュされていないファイル、変更されたファイルの一覧が出てきます

---

### status の例 (通常時)

```plaintext
On branch wip/write-overview
    Your branch is ahead of 'origin/main' by 1 commit.
    (use "git push" to publish your local commits)

Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        new file:   Slides/Getting-started-with-git.md
        modified:   Slides/Word-dictionary.md

Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified:   Outlines/Unprocessed-notes.md

Untracked files:
    (use "git add <file>..." to include in what will be committed)
        .vscode/
```

---

最初の段落は、現在のブランチにおけるコミットの状況が表示されます

- 現在のブランチ名
- リモートリポジトリと比較したコミット数

```plaintext
On branch wip/write-overview
    Your branch is ahead of 'origin/main' by 1 commit.
    (use "git push" to publish your local commits)
```

```plaintext
現在のブランチ wip/write-overview
    このブランチは 'origin/main' にないコミットが1件あります。
    ("git push" でローカルのコミットを公開できます)
```

---

ステージングしたファイルがある場合、以下の段落が表示されます

- ステージングされたがコミットしていないファイルの一覧

```plaintext
Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        new file:   Slides/Getting-started-with-git.md
        modified:   Slides/Word-dictionary.md
```

```plaintext
コミット待ちの変更は以下の通りです:
    ("git restore --staged <ファイル名>..." で取り消します)
        追加:   Slides/Getting-started-with-git.md
        変更:   Slides/Word-dictionary.md
```

---

ステージングしていないファイルがある場合、以下の段落が表示されます

- ステージングしていないが変更されているファイルの一覧

```plaintext
ステージングしていない変更は以下の通りです:
    ("git add <ファイル名>..." でコミット待ちに追加します)
    ("git restore <ファイル名>..." で前回のコミットに戻します)
        変更:   Outlines/Unprocessed-notes.md
```

---

まだコミットしたことがないファイルがある場合、以下の段落が表示されます

- 履歴の取得がなされていないファイルの一覧

```plaintext
Untracked files:
    (use "git add <file>..." to include in what will be committed)
        .vscode/
```

```plaintext
追跡していない変更は以下の通りです:
    ("git add <ファイル名>..." でコミット待ちに追加します)
        .vscode/
```

---

[] TODO: merge 時のstatus出力を載せる