- Implicit Binding  [ドットインストール](https://dotinstall.com/lessons/basic_laravel_db/57413)

- formを送信する際は、トークンを生成してセッションに設定しなければならないが、Laravelではformタグの中で「@csrf」を書くだけで、自動でトークンが仕込まれて、CSRF対策ができる。  

- bladeで改行を含むhtmlを埋め込むには、{!! nl2br(e($post->body)) !!} こんな感じにする。[ドットインストール](https://dotinstall.com/lessons/basic_laravel_crud/58311)  

- `users/edit/{{$id}}`などurlを使うときの引数名は、controllerとweb.phpを一致させる。bladeは一致してなくても良い。

- route/api.phpファイルにルーティングを記載すると自動的に /apiというURLが付与される。

- Laravelではコレクション型としてデータベースから情報を取得します。Object型の一種で、  
Laravel独自の型でデータベースからデータを取得した際等はこの型になっています。  
コレクションで利用可>能なメソッドは全部で100個以上あります。メソッドチェーンで記述が可能です。

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

- 新たにカラムを追加する場合は、このファイルを編集するのではなく、カラムを追加するためのマイグレーションファイルを作成。  
(そうすることで、テーブルのカラムが増えた、減ったなどの履歴を管理しています。)

- コントローラーの行数が長くなってきたと感じたらサービスなどに切り分けることで、
できるだけ読みやすく、すっきりしたコントローラーファイルを維持する。

- web.phpを使い、Route::group()で共通の処理をグルーピング化。  
`Route::group(['prefix'=>'users', 'middleware'=>'auth'], コールバック関数(Route::get()など))`  
'prefix'=>'users' : Urlの先頭にusersが付与 共通するurlを設定  
'middleware'=>'auth' : 認証済みかどうかを判定  

- ログインするとセッションが記録されるため、リダイレクトされる。

## laravel用関数
- getClientOriginalName() : ファイルアップロード時のファイル名取得
- getClientOriginalExtension() : ファイルアップロード時の拡張子取得
- getRealPath() : ファイルアップロード時のファイルのパスを取得



## ミドルウェア(Middleware)について
- laravelのフロー
```
HTTP Request
 ↓
Route
 ↓
**Before Middleware** (リクエストをチェックする機能)
 ↓
Controller
 ↓
View（Application）
 ↓
**After Middleware** (レスポンスをチェックする機能)
 ↓
Response
```

- App\Http\Kernelクラスに記述
```php
// Middlewareの関数
public function handle(Request $request, Closure $next)
    {
        //処理

        // $nextでrequestがコントローラーに渡される。
        return $next($request);
    }
```

## Routeクラスの指定
- Route::group

- Route::middleware

- Route::name

## Authクラス

## ダミーデータ作成方法 (usersテーブルを例にする)
[参考](https://qiita.com/shin1kt/items/022c2b8d576c203d8cf1)

### factor
- seederに対して、テスト用にランダムデータを与えることができる
- `php artisan make:factory UsersFactory`で`./database/factories`に作成される。
以下のように記述
```php
$factory->define(User::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence(rand(1,4)), // 1〜4つの単語で文章
        'content' => $faker->realText(512), // 512文字の文章
    ];
});
```


### seeder
- テストデータなどをデータベースに登録できるクラス
- `php artisan make:seeder UsersTableSeeder`で`./database/seeds`に作成される。
以下のように記述
```php
use Illuminate\Database\Seeder;

class UsersTableSeeder extends Seeder
{
    // ダミーデータ直接設定することもできる
    public function run()
    {
        DB::table('users')->insert([
            ['name' => 'ジョブズ',
            'email' => 'user1@example.com',
            'sex' => '0',
            'self_introduction' => 'ジョブズです',
            'img_name' => 'sample001.jpg',
            'password' => Hash::make('password123'),
            ],
            ['name' => 'ザッカーバーグ',
            'email' => 'user2@example.com',
            'sex' => '1',
            'self_introduction' => 'ザッカーバーグです',
            'img_name' => 'sample002.jpg',
            'password' => Hash::make('password123'),
            ],
        ]);
    }

    // ダミーデータをランダムを指定したfactoryでの設定もできる
    public function run()
    {
        // テストデータを15個作成
        factory(User::class, 15)->create();
    }
}
```

- ダミーデータは`DatabaseSeeder.php`から呼び出されるから、以下のように編集
```php
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(UsersTableSeeder::class);
    }
}
```

- composerコマンドでクラス設定の再読み込みをする  
`composer dump-autoload`

- ダミーデータをデータベースへ投入する
`php artisan db:seed`

## サービスプロバイダー、コンテナ

## 定数はクラスで定義する

## LOGについて

## Event クラスについて
php artisan make:event ChatPushe
