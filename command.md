- ルーティング設定一覧
`php artisan route:list`

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
