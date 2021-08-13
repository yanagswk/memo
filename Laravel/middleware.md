# ミドルウェア(Middleware)について
[公式](https://readouble.com/laravel/8.x/ja/middleware.html#global-middleware)
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

- ミドルウェア作成  
`php artisan make:middleware EnsureTokenIsValid`  
=>app/Http/Middlewareディレクトリ内に配置

- ミドルウェアはコントローラーの前後で呼ばれる。
    ```php
    // コントローラーに渡る前に、何かしらの処理を実行
    public function handle(Request $request, Closure $next)
        {
            //処理

            // $nextでrequestがコントローラーに渡される。
            return $next($request);
        }
    ```
    ```php
    // コントローラーに渡った後に、何かしらの処理を実行
    public function handle(Request $request, Closure $next)
        {
            $response = $next($request);

            // 処理

            return $response;
        }    
    ```

- ミドルウェアの登録  
  - グローバルミドルウェア  
    アプリケーションへのすべてのHTTPリクエスト中であるミドルウェアを実行する場合は、`app/Http/Kernel.php`クラスの`$middleware`プロパティにミドルウェアクラスをリストする。  
  - ルートに対するミドルウェアの指定
    `app/Http/Kernel.php`ファイルで`$routeMiddleware`プロパティのにキーをミドルウェアを割り当てる必要がある。
    ```php
    protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    etc...
    ];
    ```
    HTTPカーネルで定義されたら、middlewareメソッドを使用して、`routes/web.php`のルートに割り当てることができる。
    ```php
    Route::get('/profile', function () {
        //
    })->middleware('auth');
    ```
    コントローラにミドルウェアを割り当てることもできる。
    ```php
    class UserController extends Controller
    {
    
        public function __construct()
        {   
            // コントローラ全体に、適用される。
            $this->middleware('auth');
                // onlyメソッドで指定したコントローラメソッドだけ設定できる。
                ->only(['create', 'store', 'edit', 'update', 'destroy']);
                // exceptメソッドで一部だけ指定しない。
                ->except(['index', 'show']);
        }
    }
    ```
    配列でキーを指定すると、複数のミドルウェアを割り当てることができる。
    ```php
    Route::get('/profile', function () {
        //
    })->middleware(['first', 'second']);
    ```
    ミドルウェアを割り当てるときに、完全修飾クラス名を渡すこともできる。
    ```php
    use App\Http\Middleware\EnsureTokenIsValid;

    Route::get('/profile', function () {
        //
    })->middleware(EnsureTokenIsValid::class);
    ```
    ミドルウェアをルートのグループ内に割り当てる場合、あるミドルウェアをグループ内の個々のルートに適用しないようにする必要があることもある。`withoutMiddleware`メソッドを使用して適用しないようにする。
    ```php
    Route::middleware([EnsureTokenIsValid::class])->group(function () {
        Route::get('/', function () {
            //
        });
        Route::get('/profile', function () {
            //
        })->withoutMiddleware([EnsureTokenIsValid::class]);
    });
    ```
    withoutMiddlewareメソッドはルートミドルウェアのみを削除でき、グローバルミドルウェアには適用されない。  
  - ミドルウェアグループ  
    複数のミドルウェアを１つのキーにグループ化して、ルートへの割り当てを容易にしたい時もある。その場合は、HTTPカーネルの`$middlewareGroupsプロパティ`を使用する。  
    最初からLaravelは、一般的にWebおよびAPIルートへ適用する可能性のあるミドルウェアを含んだwebおよびapiミドルウェアグループを用意しています。これらのミドルウェアグループは、アプリケーションの`App\Providers\RouteServiceProvider`サービスプロバイダによって、対応するwebおよびapiルートファイル内のルートに自動的に適用されることに注意してください。  
    → routes/web.phpおよびroutes/api.phpファイルへ自動的に適用される。
    ```php
    protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            'throttle:api',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
    ];
    ```
    ミドルウェアグループは、個々のミドルウェアと同じ構文を使用して、ルートとコントローラアクションに割り当てることができる。ミドルウェアグループを使用すると、より便利に一度に多くのミドルウェアをルートに割り当てられる。
    ```php
    Route::get('/', function () {
    //
    })->middleware('web');

    Route::middleware(['web'])->group(function () {
        //
    });
    ```
  - ミドルウェアの優先度
    ルートに割り当てられた場合は、ミドルウェアの順序を制御できない。その場合は、`app/Http/Kernel.php`ファイルの`$middlewarePriority`プロパティを使用してミドルウェアの優先度を指定できます。デフォルトでは存在していないため、定義しなければならない。
    - 特定の順番でミドルウェアの実行をする必要がある場合に、記載順でミドルウェアが優先して実行される
    - グローバルミドルウェアではないものを記載する。
    - `$middlewareGroups`や`$routeMiddleware`に登録したもので最優先にしたいものをこちらにも書いておく

- ミドルウェアのパラメータ  

- 終了処理ミドルウェア