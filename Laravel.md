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
