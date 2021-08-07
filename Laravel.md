- Implicit Binding  [ドットインストール](https://dotinstall.com/lessons/basic_laravel_db/57413)

- formを送信する際は、トークンを生成してセッションに設定しなければならないが、Laravelではformタグの中で「@csrf」を書くだけで、自動でトークンが仕込まれて、CSRF対策ができる。  

- bladeで改行を含むhtmlを埋め込むには、{!! nl2br(e($post->body)) !!} こんな感じにする。[ドットインストール](https://dotinstall.com/lessons/basic_laravel_crud/58311)  

## タイムスタンプのないテーブルを更新する方法  
`Column not found: 1054 Unknown column ‘updated_at’ in ‘field list’`  
タイムスタンプのないテーブルを更新すると、上記のようなエラーが出る。  
そのため、以下のようにタイムスタンプを無効にする。  
```php
$status = new Status;
$status->name = 'キャンセル';
$status->timestamps = false;
$status->save();
```

