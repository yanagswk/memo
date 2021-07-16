# PDOクラス(データベース)  
- [ドットインストール](https://dotinstall.com/lessons/basic_php_db_v2/54404)
- 4章までの内容
```php
try {
  $pdo = new PDO(
    // DSN
    // host:MySQLがセットアップされたマシン名
    // dbname:データベース名
    // charset:文字コード
    'mysql:host=db;dbname=myapp;charset=utf8mb4',
    // user
    'dbuser',
    // password
    'dbpass',
    [
      // SQL構文でエラーが出た場合、PDOExceptionクラス形式で例外を投げる
      PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
      // カラム名がついた結果のみを指定
      PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
    ]
  );

  // クエリ作成
  $stmt = $pdo->query("ELECT 5 + 3");
  // 結果を配列で取得
  $result = $stmt->fetch();
  var_dump($result);

} catch(PDOException $e) {
  echo $e->getMessage().PHP_EOL;
  exit();
}
```

