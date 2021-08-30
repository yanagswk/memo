## memo
- Implicit Binding  [ドットインストール](https://dotinstall.com/lessons/basic_laravel_db/57413)

- bladeで改行を含むhtmlを埋め込むには、{!! nl2br(e($post->body)) !!} こんな感じにする。[ドットインストール](https://dotinstall.com/lessons/basic_laravel_crud/58311)  

- `users/edit/{{$id}}`などurlを使うときの引数名は、controllerとweb.phpを一致させる。bladeは一致してなくても良い。

- route/api.phpファイルにルーティングを記載すると自動的に /apiというURLが付与される。

- updateOrCreate : 指定したカラムのパラメータが既に存在しており、別のカラムのパラメータを変更したいときは、update文を実行し、それ以外のときにinsert文を実行する

- Laravelではコレクション型としてデータベースから情報を取得します。Object型の一種で、  
Laravel独自の型でデータベースからデータを取得した際等はこの型になっています。  
コレクションで利用可>能なメソッドは全部で100個以上あります。メソッドチェーンで記述が可能です。

- タイムスタンプのないテーブルを更新する方法  
    `Column not found: 1054 Unknown column ‘updated_at’ in ‘field list’`  
    タイムスタンプのないテーブルを更新すると、上記のようなエラーが出る。  
    そのため、以下のようにタイムスタンプを無効にする。  
    ```php
    $status = new Status;
    $status->name = 'キャンセル';
    $status->timestamps = false;
    $status->save();
    ```

- 新たにカラムを追加する場合は、このファイルを編集するのではなく、カラムを追加するためのマイグレーションファイルを作成。  
(そうすることで、テーブルのカラムが増えた、減ったなどの履歴を管理しています。)

- コントローラーの行数が長くなってきたと感じたらサービスなどに切り分けることで、
できるだけ読みやすく、すっきりしたコントローラーファイルを維持する。

- web.phpを使い、Route::group()で共通の処理をグルーピング化。  
`Route::group(['prefix'=>'users', 'middleware'=>'auth'], コールバック関数(Route::get()など))`  
'prefix'=>'users' : Urlの先頭にusersが付与 共通するurlを設定  
'middleware'=>'auth' : 認証済みかどうかを判定  

- ログインするとセッションが記録されるため、リダイレクトされる。

- 結果データ取得の際にget()を使用すると、StdClass オブジェクトのインスタンスを結果として含む Illuminate\Support\Collection オブジェクトとして返ってくる。各データにアクセスするには、foreachなどでループを回したりして、取得する。

## laravel用関数
- getClientOriginalName() : ファイルアップロード時のファイル名取得
- getClientOriginalExtension() : ファイルアップロード時の拡張子取得
- getRealPath() : ファイルアップロード時のファイルのパスを取得


## Routeクラスの指定
- Route::group

- Route::middleware

- Route::name

## Authクラス
- 以下を実行することで、認証機能が取り込まれる。(コントローラ、ビュー、ルーティングが自動的に生成)
  - `php artisan make:auth`(laravel:ver5.8まで?)
  - `php artisan ui bootstrap --auth`(laravel:ver8)
- ログイン後にデフォルトで/homeというパスにリダイレクトされる。変更するには、$redirectToプロパティを変更する。

- 現在認証しているユーザーを取得
`Auth::user();`
- 現在認証しているユーザーのIDを取得
`Auth::id();`
- ログインしているか
`Auth::check();` ログインしていればtrue


## サービスプロバイダー、コンテナ

## 定数はクラスで定義する

## LOGについて

## Event クラスについて
php artisan make:event ChatPushe

## redirect('/')->withメソッド
with()に指定した引数の値をセッションデータに保存したうえでリダイレクト

# リレーション
[公式](https://readouble.com/laravel/8.x/ja/eloquent-relationships.html#one-to-many)
## 1対多
ブログ投稿は、いくつものコメントを持つ場合がある。  
Postモデル(ブログ情報のDB)とCommentモデル(コメント情報のDB)があるとする。
- Postモデル
    ```php
    class Post extends Model
    {
        // ブログポストのコメントを取得
        public function comments()
        {
            return $this->hasMany(Comment::class);
        }
    }
    ```
    EloquentはCommentモデルの外部キーカラムがデフォルトで、comment_idカラムであると想定します。
    <br>
    `hasMany`メソッドに以下のように引数を渡すことで、外部キーを指定できる。
    ```php
    return $this->hasMany(Comment::class, 'foreign_key');

    return $this->hasMany(Comment::class, 'foreign_key', 'local_key');
    ```
    リレーションメソッドを定義したら、commentsプロパティにアクセスして、commentモデルを取得できる。
    ```php
    use App\Models\Post;

    $comments = Post::find(1)->comments;
    foreach ($comments as $comment) {
        //
    }
    ```
    また、commentsメソッドを呼び出し、クエリに条件をチェーンし続けて、リレーションのクエリへさらに制約を追加できる。
    ```php
    $comment = Post::find(1)->comments()
                    ->where('title', 'foo')
                    ->first();
    ```

- Commentモデル
    ```php
    class Comment extends Model
    {
        public function post()
        {
            return $this->belongsTo(Post::class);
        }
    }
    ```


- コントローラでjoinを記入
- マイグレーションファイルに外部キーを設定して、マイグレードする

## ヘルパー関数