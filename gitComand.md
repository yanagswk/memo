# [参考動画](https://www.udemy.com/course/unscared_git/)

# gitコマンド一覧

|項目                         |コマンド                                     |備考                                                                                     |
| ---                         | ---                                         | ---                                                                                     |
|git 登録情報                 |git config --list                             | リポジトリのアドレスの後に名前を指定すると、ルートフォルダ名を変更できる。                   |
|リポジトリをクローンする     |git clone https://リポジトリのアドレス                       |                                                              |
|特定のブランチをクローンする     |git clone -b ブランチ名 https://リポジトリのアドレス                      |                                                                                         |
|ステージへ追加               |git add .                                    |.(ピリオド)ですべて追加                                                                  |
|addを取り消す                |git reset HEAD <ファイル名>                  | リモートリポジトリから最新のコミットを取得して上書きする。 HEADは自分が作業しているブランチ名。 |
|                             |git reset HEAD <ディレクトリ名>              |                                                                                         |
|                             |git reset HEAD .                             | addしたものすべて取り消す                                                               |
|コミット                     |git commit                                   | gitのリモートリポジトリに登録する。                     |
|                            |git commit -m "コメント"                     |                                                                                         |
|                            |git commit -v                                | 変更内容も表示                                                                          |
|直前のコミットをやり直す      |git commit --amend                           | 中身もかえるなら、ファイルを直してaddした後に実行する。pushする前に実行する。 |
|変更差分の確認 (add前)       |git diff (<ファイル名>)                       | ステージに追加する前にどのような変更を行ったかを確認。                          |
|変更差分の確認 (add後)       |git diff --staged                            | コミットする前にどのような変更を行ったかを確認。                        |
|コミット間の差分       |git diff コミットID                            | --name-onlyでファイル名のみの差分。git diff HEAD~で直前のコミットとの差分。                        |
|変更したファイルを確認        |git status                                   | addできるファイルとcommitできるファイルを確認できる                                          |
|                           |git status -s                                   | <span style="color: green; ">M(緑)</span>→git add されているけどまだ git commit されていないファイルの一覧    |
|                           |                                                | <span style="color: red; ">M(赤)</span>→編集・変更・削除されているが、まだ git add されていないファイルの一覧    |
|                           |                                                | <span style="color: red; ">??</span>→Git管理されていない、かつ .gitignore で管理除外対象にもされていないものの一覧 |
|コミット履歴を確認           |git log                                      |   --decorateでどのブランチがどのコミットを指しているかを確認できる。                                      |
|                            |git log --oneline                            | 一行でログを表示                                                                        |
|                            |git log -p <ファイル名>                      | ファイルの変更差分を表示。ファイルの中身が開く                                          |
|                            |git log -n <コミット数>                      | 表示するコミット数を引数に入れる。直前のコミットをみたいなら1をいれる。                                 |
|ブランチがどのコミットをさしてるか確認   |git log --oneline --decorate                      |                                  |
|ファイルの削除               |git rm <ファイル名>                          | リモートリポジトリ(コミットしたもの)にファイルの削除をステージ記録する。ワークツリーファイルも削除。そこからコミットしたらリモートリポジトリから削除される。           |
|                             |git rm -r <ディレクトリ名>                      | リモートリポジトリから削除(コミットしたもの)。ディレクトリも削除。                                                        |
|                             |git rm --cached                              | リモートリポジトリのみ削除(コミットしたもの)。ファイルは残る。gitからは消したいけど、自分のワークツリーには残したいとき。
そのあとは、addしてコミットする。 |
|ファイルの移動　　　　　　　　|git mv <ファイル名> <移動先>                   | ワークツリーの移動+ステージにも追加。　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　 |
|ファイル名変更　　　　　　　　|git mv <旧ファイル名> <新ファイル名>            | ワークツリーの変更+ステージにも追加。　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　 |
|githubを登録                 |git remote add origin <githubのurl>          | originというショートカットでurlのリモートリポジトリを登録する。今後はoriginでリモートリポジトリにアップしたり、取得したりできる。リモートリポジトリは複数登録できて、originとは別の名前のショートカットで登録する。        |
|                             |git remote add <リモート名>                  |リモートリポジトリを新規追加                                                            |
|リモートリポジトリ確認           |git remote -v                                  |                            |
|リモートリポジトリ詳細           |git remote show origin                           |                           |
|リモート名変更               |git remote rename <旧リモート> <新リモート>  |                                                                                         |
|リモート名削除               |git remote rm <リモート>                     |                                                                                         |
|リモートリポジトリへ送信     |git push <リモート名><ブランチ名>            | git push origin master                                                                  |
|                             |git push -u origin master                    | -uで次回以降はgit pushでorigin masterにpushする                                         |
|ファイルの変更を取り消す     |git checkout -- <ファイル名>                 | 挙動としては、ワークツリーの状態をステージの状態と一致させる。 "--"はブランチ名とファイル名が被ったときに、どちらを指してるか明確にするためにつける    |
|                             |git checkout -- <ディレクトリ名>             |                                                                                         |
|                             |git checkout -- .                            |                                                                                         |
|ブランチを作成               |git branch <ブランチ名>                      | ブランチを作成するだけで、切り替わらない。                                                            |
|ブランチ一覧                 |git branch                                   |                                                          |
|すべてのブランチを表示        |git branch -a                                   | リモートブランチも表示                                                        |
|リモートリポジトリ(masterブランチ)からmasterブランチを作成 |git branch master origin/master   |                                                         |
|ブランチを切り替える         |git checkout <既存ブランチ名>                  |                                                                                         |
|                             |git checkout -b <新ブランチ名>                 | ブランチを新規作成して切り替える                                                        |
|ブランチ名を変更             |git branch -m <新ブランチ名>                   |  現在いるブランチ名を変更                                                                      |
|ブランチ削除                 |git branch -d <ブランチ名>                   |                                                                                         |
|ブランチ強制削除              |git branch -D <ブランチ名>                | masterにmergeしていないブランチがあるときは、削除できないが、このコマンドだと強制削除ができる。 |
|変更履歴をマージする         |git merge <ブランチ名>                       | コンフリクトが起きた場合は、git statusで確認                                                   |
|                             |git merge <リモート名/ブランチ名>            | git merge origin master                                                              |
|リモートから取得する (fetch)|git fetch <リモート名> (git fetch origin)    | リモートリポジトリからローカルリポジトリに取得。ワークツリーには反映されない。remote/リモート/ブランチに保存される。 反映するには、git merge origin/masterをする。  |
|プルのマージ型               |git pull <リモート名> <ブランチ名>           | git pull origin master  リモートリポジトリからローカルリポジトリに取得して、ワークツリーにも反映する。|
|プルのリベース型             |git pull --rebase <リモート名> <ブランチ名>  | git pull --rebase origin master master                                                  |
|タグ作成(軽量版タグ)         |git tag [タグ名]                             |                                                                                         |
|タグを作成(注釈付きタグ)     |git tag -a [タグ名] -m "[メッセージ]"        |                                                                                         |
|                             |git push [リモート名] [タグ名]               | タグをリモートリポジトリに送信する                                                      |
|タグのデータと関連付けられたコミットを表示　|git show [タグ名]               |                                                       |
|作業を一次非難する           |git stash save                               | saveは省略可                                                                            |
|一時避難                     |git stash list                               | 非難した作業を確認する                                                                  |
|最新の作業を復元する                             |git stash apply                              |                                                                    |
|ステージの状況も復元する                             |git stash apply --index                      |                                                                 |
|特定の作業を復元する       |git stash apply [スタッシュ名]                 |   スタッシュ名はlistで確認する。　stash@{1}みたいな感じ。                                    |
|リモートとローカルの差          |git                                |                                                                   |
|コマンドにエイリアス         |git config --global alias st status          | 省略したい単語、コマンドの順番。--globalでpc全体の設定になる。                          |
|untracked file確認         |git clean -n          | -dnでディレクトリ確認                          |
|untracked file削除         |git clean -f          | -dfでディレクトリ削除                          |


# gitignore書き方
* 指定したファイルを除外
  * index.html
* ルートディレクトリを指定
  * /root.html
* ディレクトリ以下を除外
  * dir/
* /以下の文字列にマッチ「＊」
  * /\*/\*.css


# プルリクエストの流れ
* 変更をGitHubへプッシュ(自分が開発していたブランチで)したら、GitHubでプルリクエストを作る。
* 作成したら、チームメンバーにコードレビューをお願いする。
* レビューが通ったら、GitHubでマージボタンを押す。
* マージをしたら、それまで開発していたブランチを削除する。

# リベース
* git branch rebase <ブランチ名>
* リベースで履歴を整えた形で変更を統合する (他のブランチのコミット履歴を自分に取り込めることが出来る。)(マージとリベースの違いは履歴が一直線なのか、枝分かれしてるかの違い。)

# git pull の注意点
- 現在自分がいるブランチにmergeされる。例えば、hogeブランチを取得したくて、`git pull origin hoge`とした時、  
現在hogeブランチにいたら大丈夫だが、masterブランチにいた場合は、masterブランチにマージされる。
- そのためpullするときは、pullするブランチに移動してからpullする。  
- 最初のうちは、git fetchをしてからgit merge origin/masterをした方が良い。  

# コンフリクトした場合  
- コンフリクトを直した後に、git add してから git commit すれば良い。  
- コンフリクしているときは、git statusをすると以下のようになる。  
```sh
$ git status  
On branch work-branch
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   "ファイル名"
        both modified:   "ファイル名"

```

# ブランチを利用した開発の流れ  
masterブランチをリリース用のブランチに、開発はトピックブランチを作成して進めるのが基本

# memo
- origin : ショートカットでurlのリモートリポジトリ名
- HEAD : 自分が作業しているブランチ名  

# Todo  
- コミットしたものを後から複数のコミットに分割する方法(rebaseを使う？)  

