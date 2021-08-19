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