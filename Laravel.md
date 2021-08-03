- Implicit Binding  [ドットインストール](https://dotinstall.com/lessons/basic_laravel_db/57413)

- formを送信する際は、トークンを生成してセッションに設定しなければならないが、Laravelではformタグの中で「@csrf」を書くだけで、自動でトークンが仕込まれて、CSRF対策ができる。  

- bladeで改行を含むhtmlを埋め込むには、{!! nl2br(e($post->body)) !!} こんな感じにする。[ドットインストール](https://dotinstall.com/lessons/basic_laravel_crud/58311)
