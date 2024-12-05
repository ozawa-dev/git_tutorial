## WSL2 に git インストール

```
sudo apt update
sudo apt install git -y
git --version
```

## 初期設定
設定
```
git config --global user.name "username"
git config --global user.email "mailaddress"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
```
確認
```
git config user.name
git config user.email
git config core.editor
git config --list → 全て確認できる（cat ~/.gitconfigと同様）
```
※globalでないなら、./.git/configに保存される

## ローカルリポジトリ作成
```
mkdir dirname
cd dirname
git init → .gitディレクトリが作成される（圧縮ファイル、コミットファイル、ツリーファイル、インデックスファイル、設定ファイルなどが含まれる）
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

### クローン
```
git clone リポジトリ名：リモートリポジトリのファイルと.gitがローカルにコピーされる
```

## ステージへの追加
```
git add ファイル名 or ディレクトリ名 or .（すべてを対象）
```

## コミット
```
git commit → コミットエディタが立ち上がる
git commit -m "メッセージ"
git commit -v →　変更内容を確認する
```
コミットメッセージはわかりやすく書くこと
- 1行目：変更内容の要約
- 2行目：空行
- 3行目：変更の理由

## 現在の変更状況を確認
```
git status → worktreeとstage間で変更されたファイルを確認
             stageとlocal repo間で変更されたファイルを確認
```

## 何を変更したか確認
```
git diff [ファイル名] → worktreeとstageの差分
git diff --staged → stagとlocal repoの差分
```

## 変更履歴を確認
```
git log
git log --oneline → 1行で表示
git log -p ファイル名 → 特定ファイルの変更差分を表示
git log -n コミット数 → 表示するコミット数を制限する（直近だけに絞れる）
```

## ファイルの削除を記録
リポジトリおよびworktreeからファイルを削除&コミットされたgitの記録も消える
```
git rm ファイル名
git rm -r ディレクトリ名
git rm --cached ファイル名 → worktreeにファイルは残す
```
※元に戻す
```
git reset HEAD index.html
git checkout index.html   --cachedの場合は実施不要
```

## ファイルの移動を記録 リネーム
```
git mv 旧ファイル 新ファイル
```

## githubにpushする（ローカルリポジトリの内容をリモートにアップ）
```
git remote add prigin URL
git push リモート名 ブランチ名
```
## エイリアス
```
git config --global alias.ci commit → commitにciというエイリアスをつける
git config --global alias.st status
git config --global alias.br branch
git config --global alias.co checkout
```

## ファイルへの変更を取り消す
```
git checkout -- ファイル名 or ディレクトリ名 or . → worktreeの状態をstageの状態を同じにする
```

## ステージした変更を取り消す
```
git reset HEAD ファイル名 or ディレクトリ名 or . → 指定した変更をstageから消す（worktreeのファイルには影響を与えない→worktreeも消したいならgit checkout）
```                                               
※HEADは最新のコミットのこと

## 直前のコミットをやり直す
```
git commit --amend → 今のstageの状態をもとに直前のcommitをやり直す（事前にファイルに想定していた正しい変更内容に修正してgit addしておくこと）
```
※リモートリポジトリにpushしたコミットはやり直さず。新規でコミットすること（すでにそのコミットをほかのメンバが反映してそれをもとに開発している可能性があるから）

## リモートの情報を確認
```
git remote 設定しているリモートの情報を表示
git remote -v URLも表示する
```

## リモートから情報を取得する（fetch）
```
git fetch リモート名 → リモートからローカルリポジトリに情報をとってくる（worktreeには反映しない） ※remotes/リモート/ブランチに保存される
git merge リモート名/ブランチ → fetchしたものをworktreeに反映させる

git branch -a ：ブランチを表示
git checkout remotes/origin/main → worktreeの内容をremotes/origin/mainのものに切り替える
git checkout main：元に戻す

git merge
```

## リモートから情報を取得（pull）
```
git pull [リモート名 ブランチ名] → git fetchとgit mergeと同じことをしている
```

## fetchとpullの使い分け
pullだと今自分がいるブランチにマージされてしまうので注意が必要

基本はfetchとmergeを使ったほうが安全

## リモートの情報を詳しく知る
```
git remote show リモート名 → git remoteより詳しい情報を表示
```

## リモート名を変更・削除
```
git remote rename 旧リモート名 新リモート名
git remote rm リモート名
```

## ブランチ
並行して別々の機能を開発できる機能（双方の変更の影響を受けない）

- 実体はコミットを指したポインタ
- HEADは現在作業中のブランチへのポインタ（.git/HEAD、.git/refs）
- コミット ← ブランチ ← HEAD
- コミットするとブランチが指すコミットファイルが変わる（最新のものを指す）

```
git branch → 現在のブランチを確認
git branch ブランチ名 → ブランチ新規作成（切り替えは行わない）
git checkout ブランチ名 → ブランチを切り替える
git checkout -b ブランチ名 → ブランチを新規作成して切り替える
git branch -a → リモートも含めすべてのブランチを確認
```

## マージ
```
git merge ブランチ名
git merge リモート名/ブランチ名
```
マージの種類
- fast foward：早送りになるマージ（ブランチが枝分かれしていなかったときはブランチのポインタを前に進めるだけ）
- auto merge：基本的なマージ（マージしたコミットファイルは二つの親コミットへの参照を持つ）
- コンフリクト：複数人が同じ個所を変更している状態（ファイルの内容を書き換える → << == >>の記述を削除することで解決）

## ブランチの変更・削除
```
git branch -m ブランチ名 → ブランチ名の変更
git branch -d ブランチ名 → ブランチの削除（mainにマージされていない変更がある場合削除しない）
git branch -D ブランチ名 → 強制削除する
git branch --remote -d ブランチ名 → リモートブランチの削除
git push リモート名 --delete ブランチ名 → リモートへブランチ削除命令
```

## リベース
内容だけでなく、履歴を整えた形で変更を統合する

githubにpushしたコミットはリベースしないこと（矛盾が生じる）
```
git rebase ブランチ名 → ブランチの起点となるコミットを別のコミットに移動

git checkout feature
git rebase main
git checkout main
git merge feature → fast fowardが起こる
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


