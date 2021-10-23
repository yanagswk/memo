- [ ] 直前のコミットをやり直して、pushをやり直す(push済み)。その際、やり直す前のコミット履歴を消したい。
- [ ] 2,3つ前のコミットをやり直して、やり直す前の履歴を消す。
- [ ] 2つ前のコミットをやり直す(pushしてしまった場合はどうすれば良いか)
- [X] masterブランチではなく、別のブランチで作業をしていて、`git push origin master`をしてしまった時の対処
->エラーが出るためできない
- [X] 一つコミットで変更したファイル名や内容を知りたい。一つ前のコミットとの差分の確認。  
->git diff コミットID (--name-only)
- [X] `is_null()`　について
- [X] 既存のテーブル構造を変更したときのマイグレーションファイル作成方法について
- [X] dockerfileやdocker-compose.yamlに記述されている`${}:`について
->環境変数
- [ ] sshについて
- [ ] よく使うPHPの関数調べとく
- [X] interfaceついて
- [ ] abstractついて
- [X] laravelのlogについて
- [X] ...について  
    ->可変調引数のことで、引数の数が決まっていなくて、状況に応じて複数の引数を指定できる。
- [ ] php 正規表現について (regex:正規表現 - Laravel)[https://readouble.com/laravel/6.x/ja/validation.html#rule-regex](【5分でまるっと理解】PHP正規表現の使い方まとめ)[https://eng-entrance.com/php-regularex](preg_match - PHP公式マニュアル)[https://www.php.net/manual/ja/function.preg-match.php]  
/^(?!.*\s).+$/uは、PHPにおいて半角スペースが無いこと、/^(?!.*\/).*$/uは/が無いことをチェックする正規表現  
- [ ] クロージャーについて。ロージャの外側で定義されている変数を通常使用できません。対策としては、その場合はuseを使う。  
- [X] アクセサについて get...Attribute
- [ ] (async/await)[https://www.codegrid.net/articles/2017-async-await-1]  
- [ ] N+1問題
- [X] laravel : アクセサ、ミューテタ  
->モデルのプロパティ(テーブルのカラム)と同じ感覚で呼び出せる  
- [ ] get...Attribute __toStringのアクセサの使い方(laravelのバージョン6)
- [X] laravel : ポリシー
- [ ] laravel : ゲート
- [X] laravel : サービス
- [ ] laravel : サービスコンテナ
- [ ] laravel : サービスプロバイダ
- [ ] インターフェース->サービスコンテナ->サービスプロバイダ
- [ ] サービスプロバイダいつ実行される？
- [ ] laravel : エラーハンドリング
- [X] laravel : イベント,リスナー
- [ ] laravel : command
- [ ] 多対多 中間テーブル
- [ ] (APIリソース)[https://readouble.com/laravel/8.x/ja/eloquent-resources.html]
- [ ] (Eloquent)[https://readouble.com/laravel/8.x/ja/eloquent.html]
- [X] トレイト
- [ ] キュー
- [ ] APIリソース
- [ ] laravelのヘルパー関数
- [X] self parent thisの違い
- [ ] array_multisort
- [X] laravel : スコープ
- [ ] Builder
- [ ] Session::
- [X] クラスの先頭にバックスラッシュ  
-> 名前空間が定義されたクラスから、名前空間を指定していない（いずれの名前空間にも属さない）クラスを使用する場合、使用するクラス名の先頭に\（バックスラッシュ）を記述する。  
-> 名前空間が定義されたクラスから、その外にあるクラスを呼び出すときは先頭にバックスラッシュをつける。  
-> https://prograshi.com/language/php/php-back-slash-on-top/
- [X] where()->value()
- [ ] $request->input()
- [ ] view()->render()
- [ ] new Carbon
- [ ] exec 接続のやり方 -it
- [ ] php.ini設定　php.ini-devなど　display_errorsとdisplay_startup_errorsの違い　画面にエラー
- [ ] vscode シングルクオーテーションを禁止にしたい
- [X] docker-compose : トップレベルのvolumes(名前付きボリューム)とは？  
    ```
    単体のサービス内だけでなく、複数のサービス（例えばwebとdb）内のvolumesから参照できる。  
    ボリュームを削除するとホスト側のディレクトリごと削除されてしまうので、dbに保存したデータが消えてしまう。
    ```  
    ```
    「マウント」とは「取り付ける」くらいの意味合いで解釈していただき、ローカルのdb-volumeをコンテナの/var/lib/mysqlに取り付ける、と考えると理解しやすいのではないでしょうか？
    ローカルのdb-volumeとコンテナの/var/lib/mysqlで中身を共有しているイメージです。

    コンテナは終了すると中身が全て消えてしまうので、消えては困るようなものはコンテナから取り出してローカルに保存しておきます。そのための設定がvolumesの部分にあたります。
    再度起動した時にコンテナの/var/lib/mysqlの情報がローカルのdb-volumeに残っており、それがコンテナの/var/lib/mysqlにマウントされるので、振る舞いとしては前回のコンテナを引き継いだように見えます、これによって見掛け上永続化できているというものです。
    ``` 
- [ ] httpについて説明してください。
- [ ] コードの記述不十分で返ってくるステータスコードはなんですか？
- [ ] http1.0と2.0の違いはなんですか？
- [ ] httpとhttpsの違いはなんですか？


