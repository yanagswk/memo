- モデル作成
`php artisan make:model Test` `php artisan make:model Test -m`でマイグレーションファイルも作成

- 新規マイグレーションファイル作成(「tests」テーブル作成--createオプション)
`php artisan make:migration create_tests_table`

- マイグレーションファイル実行(upメソッド)
`php artisan migrate`

- 既存のテーブル構造変更(「tests」テーブル構造変更--tableオプション)
`php artisan make:migration add_column_to_tests_table`

- マイグレーションやり直し
`php artisan migrate:rollback`

- 対話シェル起動
`php artisan tinker`

- キャッシュ削除
`php artisan config:clear`
`php artisan cache:clear`
`php artisan config:cache`

- 認証機能
`php artisan make:auth`(laravel:ver5.8まで)
`php artisan ui bootstrap --auth`(laravel:ver8)

- リクエストフォーム作成(バリデーション)
`php artisan make:request CartItemRequest`

- 新しいクラス作ったり、ファイル名を変更した場合 [参考](https://prograshi.com/framework/laravel/laravel-error-class-not-found/)
`composer dump-autoload`  

- ルーティングの一覧  
`php artisan route:list`  

- ポリシー作成  
`php artisan make:policy ArticlePolicy(ポリシー名) --model=Article(対応するモデル)`  

- メールクラス作成  
`php artisan make:mail BareMail`  

- 通知クラス作成  
`php artisan make:notification PasswordResetNotification`  

- Laravel Debugbarインストール  
`composer require barryvdh/laravel-debugbar --dev`  

- シンボリックリンク作成  
`php artisan storage:link`  
画像は初期設定で`storage/app/public`に保存されるが、公開するページは`/public`なのでシンボリックリンクを作成  

- イベント作成  
`php artisan make:event UserRegistered`  

- リスナー作成  
`php artisan make:listener SendWelcomeEmail --event=UserRegistered`  

- イベントとリスナー同時に作成    
EventServiceProviderに事前にイベントとリスナーを記述
`php artisan event:generate`  

- イベントとリスナ一覧表示  
`php artisan event:list`

- テスト作成  
`php artisan make:test テスト名`

- テスト実行  
`./vendor/bin/phpunit --testdox`
