# 概要

Git 操作練習用リポジトリ

## WSL2 に git インストール

```
sudo apt update
sudo apt install git -y
git --version
```

## 初期設定

```
git config --global user.name "username"
git config --global user.email "mailaddress"
git config --global init.defaultBranch main
git config --list
```

## ローカルリポジトリ作成

```
mkdir dirname
cd dirname
git init
```

## リモートリポジトリ設定

http接続
```
git remote add remotename URL
git remote -v → リモートリポジトリ確認

<URL変更する場合>
git remote set-usl remotename URL
```
ssh接続
```
ssh-keygen -t rsa → 秘密鍵、公開鍵生成
cat ~/.ssh/id_rsa.pub | clip.exe → 公開鍵をコピー
https://github.com/settings/sshで公開鍵をペーストして設定
ssh -T git@github.com → 疎通確認
git remote add remotename sshURL → url変更
```

## 基本操作

local repo と worktree の差分チェック

```
git diff
```

ファイルをステージへ追加

```
git add filename ※.で全ファイル
```

local repo と stage の差分確認

```
git diff --staged
```

local repo へコミット

```
git commit -m "commit message"
git commit -v  → コミット内容確認
```

変更ファイル確認

```
git status
```

変更履歴確認

```
git log
 --online：ワンライナー表示
 -p filename：ファイルの変更差分表示
 -n <commit cnt>：表示コミット数制限（直近から遡る）
```

ファイル削除

```
git rm filename
git rm -r dirname
git rm --cached filename → worktreeには残す
```

ファイル移動

```
git mv oldfile newfile
```

リストア

```
git restore filename → worktreeの変更取り消し
git restore --staged filename → stageにあげた変更をworktreeに戻す
```

ブランチ

```
git branch → ブランチ一覧確認
 -a：githubのブランチも表示
git branch branchname → ブランチ作成
git branch -m master main
```

スイッチ

```
git switch branchname → ブランチ切り替え
 -c：ブランチ新規作成
```

マージ

```
git merge branchname → ブランチマージ
```

push

```
git push remotename branchname
```

pull

```
git pull [remotename branchname] → local repoに反映してworktreeに反映（コンフリクト懸念があればfetchしてmerge）
```

fetch

```
git fetch remotename → local repoに取得
```

clone

```
git clone URL
```

## .gitignore（除外ルール）
基本
```
test.txt ：ファイル名
*.txt ： ワイルドカード
node_modules/ ： ディレクトリ
!important-file.txt ： 特定のファイルやフォルダを除外しない
```
サンプル（nodejs pj）
```
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Dependency directories
node_modules/

# Build directories
dist/
build/
```
すでに追跡済みのファイルは影響範囲外のため、キャッシュクリアする必要がある
```
git rm -r --cached
git add .
git commit -m "update .gitignore"
```


