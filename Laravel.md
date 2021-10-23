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

- web.phpを使い、Route::group()で共通の処理をグルーピング化。  
`Route::group(['prefix'=>'users', 'middleware'=>'auth'], コールバック関数(Route::get()など))`  
'prefix'=>'users' : Urlの先頭にusersが付与 共通するurlを設定  
'middleware'=>'auth' : 認証済みかどうかを判定  

- ログインするとセッションが記録されるため、リダイレクトされる。  

- 直接urlを入力したら遷移してしまうのを防ぐために、middleware('auth');をcontrollerに定義して、ログインしていないとアクセスできないようにする。

- 結果データ取得の際にget()を使用すると、StdClass オブジェクトのインスタンスを結果として含む Illuminate\Support\Collection オブジェクトとして返ってくる。各データにアクセスするには、foreachなどでループを回したりして、取得する。  

- コントローラーにはあまり多くの処理を持たせないようにすることが望ましい。コントローラーが肥大化している状態はファットコントローラーとよばれ、アンチパターンとされています。 サービスファイルとして切り分けるようにする。

- $article->likesとすると、記事モデルからlikesテーブル経由で紐付いているユーザーモデルのコレクションが返りました。
今回は、$article->likes()としており、多対多のリレーション(BelongsToManyクラスのインスタンス)が返ります。  

- Laravelでは、コントローラーのアクションメソッドで配列や連想配列を返すと、JSON形式に変換されてレスポンスされます。  

- 各テーブルモデルクラスは、`this->カラム名`みたいにプロパティとして扱える。  

- phpUnitでのテスト時にphpDogで、@dataProvide だけが、実行時に影響がある。 @param の行は省略しても問題ない。  
`PHPUnit ではデータプロバイダという仕組みを使って、テストパターンを配列で定義し、テストメソッドに順に渡してテストを行うことができます。以下のように @dataProvider メソッド名 の形式で、テストメソッドの DocComment 内に書いてください。`  
以下のように、テストができる。
```php
/**
 * @param int $remainingCount
 * @param string $expectedMark
 * @dataProvider dataMark
 */
public function testMark(int $remainingCount, string $expectedMark)
{
    $level = new VacancyLevel($remainingCount);
    $this->assertSame($expectedMark, $level->mark());
}


public function dataMark()
{
    return [
        '空きなし' => [
            'remainingCount' => 0,
            'expectedMark' => '×',
        ],
        '残りわずか' => [
            'remainingCount' => 4,
            'expectedMark' => '△',
        ],
        '空き十分' => [
            'remainingCount' => 5,
            'expectedMark' => '◎',
        ],
    ];
}
```

- Fakerはテスト時など、データベースになんでも良い値を入れたい時に使う。factoryやseederで使う。  

- テスト作成時の注意点
`RefreshDatabase や DatabaseTransactions といったトレイトが用意されており、各テストケースを実行後、データベースを実行前の状態に戻します。
基本的には前者を使えばいいと思いますが、前者はテスト開始時に artisan migrate:fresh --seed を実行するのと同等なので、テスト用のデータベースを必ず用意してください（開発用のデータベースを共用していると、テスト実行時にデータが消えてしまいます）。`  

- リレーション時
`$this->reservations->count()`  
-> こちらの方法では reservations は Collection です。 `SELECT * FROM reservations WHERE lesson_id = ?` のような SQL を発行して Lesson にひもづく reservations のレコードをいったんぜんぶ取得し、その件数を数えます。  
`$this->reservations()->count();`  
-> HasMany::count()を使って、`SELECT COUNT(*) FROM reservations WHERE lesson_id = ?` のような SQL を発行して件数を取得します


## Authクラス
- 以下を実行することで、認証機能が取り込まれる。(コントローラ、ビュー、ルーティングが自動的に生成)
  - `php artisan ui bootstrap --auth`
- ログイン後にデフォルトで/homeというパスにリダイレクトされる。変更するには、$redirectToプロパティを変更する。

- 現在認証しているユーザーを取得
`Auth::user();`
- 現在認証しているユーザーのIDを取得
`Auth::id();`
- ログインしているか
`Auth::check();` ログインしていればtrue


## redirect('/')->withメソッド
with()に指定した引数の値をセッションデータに保存したうえでリダイレクト
