# PDOクラス(データベース)  
- 参考動画 : [ドットインストール](https://dotinstall.com/lessons/basic_php_db_v2/54404)
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


# プリペアードステートメント  

- SQL文を最初に用意しておいて、その後はクエリ内のパラメータの値だけを変更してクエリを実行できる機能のこと。  
- SQLインジェクション対策に必要なパラメータのエスケープ処理をする。安全かつ効率が良い。  
- 参考動画 : [Udemy](https://www.udemy.com/course/backend-tutorial/learn/lecture/25392400#questions)

```php
<form action="<?php $_SERVER['REQUEST_URI']; ?>" method="POST">
    Shop ID: <input type="text" name="shop_id">
    <input type="submit" value="検索">
</form>

<?php
if(isset($_POST['shop_id'])) {

    try {
        $shop_id = $_POST['shop_id'];

        $user = 'test_user';
        $pwd = 'pwd';
        $host = 'localhost';
        $dbName = 'test_phpdb';
        $dsn = "mysql:host={$host};port=8889;dbname={$dbName};";
        $conn = new PDO($dsn, $user, $pwd);
        $conn->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
        $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        // プリペアードステートメントの機能をつかう
        $conn->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);

        // プリペアードステートメント
        $pst = $conn->prepare("select * from test_phpdb.mst_shops where id = :id;");

        // bindValueで渡したい変数を指定して、型指定ができる。
        // $pst->bindValue(':id', $shop_id, PDO::PARAM_INT);

        // クエリ実行
        // bindValueを使わずに、executeで渡したい変数を配列で指定できる
        $pst->execute([
            ':id' => $shop_id
        ]);

        // 見つからない場合はfalseが返る。
        $result = $pst->fetch();

        // warningが出ないように、!emptyで調べてあげる。
        if (!empty($result) && count($result) > 0) {
            echo "店舗名は[{$result['name']}]です。";
        } else {
            echo "店舗が見つかりません。";
        }

    } catch(PDOException $e) {
        echo 'エラーが発生しました。';
    }
}
```  


# トランザクション  
・途中のSQLでエラーが発生して、データの不整合をなくすように、トランザクションをする。  
・SQLを使うときは、必ずトランザクションをする。
```php  

try {

    $user = 'test_user';
    $pwd = 'pwd';
    $host = 'localhost';
    $dbName = 'test_phpdb';
    $dsn = "mysql:host={$host};port=8889;dbname={$dbName};";
    $conn = new PDO($dsn, $user, $pwd);
    $conn->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $conn->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);

    $product_id = 3;    // 商品ID
    $from_shop_id = 1;  // 移動元
    $to_shop_id = 3;    // 移動先
    $amount = 10;       // 量

    $pst = $conn->prepare('
        update txn_stocks
        set amount = amount + :amount
        where shop_id = :shop_id
        and product_id = :product_id
    ');

    //トランザクション
    $conn->beginTransaction();

    $pst->execute([
        ':amount' => $amount,
        ':shop_id' => $to_shop_id,
        ':product_id' => $product_id
    ]);
    $pst->execute([
        ':amount' => -1 * $amount,
        ':shop_id' => $from_shop_id,
        ':product_id' => $product_id
    ]);
    // コミットする。
    $conn->commit();

} catch (PDOException $e) {
    echo 'エラーが発生しました。<br>';
    echo $e->getMessage();

    // エラーが発生した場合は、rollBackする。
    $conn->rollBack();
}
```


# データベースクラス作成  

```php
// datasource.php


namespace db;
use PDO;

class DataSource {
    private $conn;
    private $sqlResult;
    public function __construct($host='localhost', $port='8889',
                         $dbName='test_phpdb', $username='test_user',
                         $password='pwd') {
        $dsn = "mysql:host={$host};port={$port};dbname={$dbName};";
        $this->conn = new PDO($dsn, $username, $password);
        $this->conn->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
        $this->conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        $this->conn->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
    }

    // select用(複数レコード用)
    public function select($sql="", $params=[]) {
        $stmt = $this->executeSql($sql, $params);
        return $stmt->fetchAll();
    }

    // insert、apdate用
    public function execute($sql = "", $params = []) {
        $this->executeSql($sql, $params);
        return  $this->sqlResult;
    }

    // 単一レコード用
    public function selectOne($sql, $params) {
        $result = $this->select($sql, $params);
        // 値が取得できなかった場合は、空で帰ってくる。
        // そうなるとエラーになってしまうので、countで判定してあげる。
        return count($result) > 0 ? $result[0] : false;
    }

    public function begin() {
        $this->conn->beginTransaction();
    }

    public function commit() {
        $this->conn->commit();
    }

    public function rollback() {
        $this->conn->rollback();
    }

    private function executeSql($sql, $params) {
        $stmt = $this->conn->prepare($sql);
        $this->sqlResult = $stmt->execute($params);
        return $stmt;
    }
}
```  

```php
// index.php


require_once 'datasource.php';

use db\DataSource;

try {
    $db = new DataSource;

    $product_id = 2;
    $from_shop_id = 1;
    $to_shop_id = 3;
    $amount = 10;

    $sql = '
        update txn_stocks
        set amount = amount + :amount
        where shop_id = :shop_id
        and product_id = :product_id
    ';

    $db->begin();

    $db->execute($sql, [
        ':amount' => $amount,
        ':shop_id' => $to_shop_id,
        ':product_id' => $product_id
    ]);

    $db->execute($sql, [
        ':amount' => -1 * $amount,
        ':shop_id' => $from_shop_id,
        ':product_id' => $product_id
    ]);

    $db->commit();
} catch (PDOException $e) {

    echo 'エラーが発生しました。<br>';
    echo $e->getMessage();
    $db->rollBack();
}
```  


# SQL実行結果がクラスで返却  
```php
// index.php


<form action="<?php $_SERVER['REQUEST_URI']; ?>" method="POST">
    商品ID：<input type="text" name="product_id">
    <input type="submit">
</form>


require_once 'product.model.php';
require_once 'datasource.php';

use db\DataSource;
use model\ProductModel;

if(isset($_POST['product_id'])) {
    try {

        $product_id = $_POST['product_id'];

        $db = new DataSource();

        $result = $db->selectOne('
            select * from mst_products where id = :id and delete_flg <> 1;
        ', [':id' => $product_id], DataSource::CLS, ProductModel::class);

        if(!empty($result)) {
            echo "商品名は[{$result->name}]です。";
        } else {
            echo '一致する商品が見つかりません。';
        }

    } catch(PDOException $e) {
        echo '時間をおいて再度お試しください。';
    }
}
```

```php
// datasource.php


namespace db;

use PDO;

class DataSource {

    private $conn;
    private $sqlResult;
    public const CLS = 'cls';

    public function __construct($host = 'localhost', $port = '8889', $dbName = 'test_phpdb', $username = 'test_user', $password = 'pwd') {
        
        $dsn = "mysql:host={$host};port={$port};dbname={$dbName};";
        $this->conn = new PDO($dsn, $username, $password);

        $this->conn->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
        $this->conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        $this->conn->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);

    }

    public function select($sql="", $params=[], $type="", $cls= "") {
        $stmt = $this->executeSql($sql, $params);
        if ($type === static::CLS) {
            // クラスでデータを取得する。
            return $stmt->fetchAll(PDO::FETCH_CLASS, $cls);
        } else {
            // 配列でデータを取得する。
            return $stmt->fetchAll();
        }
    }

    public function execute($sql = "", $params = []) {
        $this->executeSql($sql, $params);
        return  $this->sqlResult;
    }

    public function selectOne($sql = "", $params = [], $type="", $cls= "") {
        $result = $this->select($sql, $params, $type, $cls);
        return count($result) > 0 ? $result[0] : false;
    }

    public function begin() {
        $this->conn->beginTransaction();
    }

    public function commit() {
        $this->conn->commit();
    }

    public function rollback() {
        $this->conn->rollback();
    }

    private function executeSql($sql, $params) {
        $stmt = $this->conn->prepare($sql);
        $this->sqlResult = $stmt->execute($params);
        return $stmt;
    }
}
```  


```php
// product.model.php

namespace model;

class ProductModel {
    public int $id;
    public string $name;
    public int $delete_flg;
}
```