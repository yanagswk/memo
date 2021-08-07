# issetとemptyの違い  
- ### isset ： 引数にNULL以外がセットされていればtrueを返す
```php

// 空文字
$var1 = '';
var_dump(isset($var1));
//        ==>bool(true)

// 文字列
$var2 = 'test';
var_dump(isset($var2));
//        ==>bool(true)

// 数字の0
$var3 = 0;
var_dump(isset($var3));
//        ==>bool(true)

// false
$var4 = false;
var_dump(isset($var4));
//        ==>bool(true)

// NULL
$var5 = NULL;
var_dump(isset($var5));
//        ==>bool(false)

// 宣言なし
var_dump(isset($var6));
//        ==>bool(false)
```  
勘違いしやすい点として、以下はisset()ではtrueになる。  
- 空文字
- 0(数字のゼロ)
- false  

isset()には複数の引数を渡すことができる。  
渡された全ての引数がNULL以外の時、trueを返す。  
つまり、一つでもNULLがあればfalseになる。  
```php
// 複数の値（NULLを含まない）
$var8 = 'sample';
$var9 = 100;
var_dump(isset($var8, $var9)); 
//         ==> bool(true)
 
// 複数の値（NULLを含む）
$var10 = 'sample';
$var11 = NULL;
var_dump(isset($var10, $var11)); 
//         ==> bool(false)
```

<br>

- ### empty - 要素が空であればtrueを返す。   
```php
// 空文字
$var1 = '';
var_dump(empty($var1));
//         ==> bool(true)

// 文字列
$var2 = 'test';
var_dump(empty($var2)); 
//         ==> bool(false)

// 文字列の0
$var3 = '0';
var_dump(empty($var3)); 
//         ==> bool(true)

// 数字
$var4 = 2019;
var_dump(empty($var4)); 
//         ==> bool(false)
 
// 数字の0
$var5 = 0;
var_dump(empty($var5)); 
//         ==> bool(true)
 
// 真偽値(true)
$var6 = true;
var_dump(empty($var6)); 
//         ==> bool(false)
 
// 真偽値(false)
$var7 = false;
var_dump(empty($var7)); 
//         ==> bool(true)
 
// NULL
$var7 = NULL;
var_dump(empty($var7)); 
//         ==> bool(true)
 
// 空の配列
$var8 = array();
var_dump(empty($var8)); 
//         ==> bool(true)

```  
勘違いしやすい点として、以下はisset()ではtrueになる。  
- 0(数字のゼロ')
- 0'(文字列のゼロ)・・・<span style="color: red;">文字列の 0 はtrue(空判定)になる</span>
- NULL
- 空文字('')
- 空の配列

<br>
<br>


# null型演算子
- 三項演算子と isset() を組み合わせる。 よくありがちなパターンを、より簡単に書くためのもの。
```php
// $_GET['user'] を取得します。もし存在しない場合は
// 'nobody' を用います
$username = $_GET['user'] ?? 'nobody';
// 上のコードは、次のコードと同じ意味です。
$username = isset($_GET['user']) ? $GET['user'] : 'nobody';

// 合体演算子を連結することもできる。
// $_GET['user']、$_POST['user'] そして 'nobody'
// の順に調べて、非 &null; が定義されている最初の値を返す。
$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';

```
<br>

# 0 はfalse、1 はtrue

<br>




# constとdefineの違い  
### 変数や関数の戻り値が、使えるか使えないか  
- defineは変数や、関数の戻り値が<span style="color: red;">使える。</span>
```php
// define
$val = '適当な変数';
define('MY_CONST', $val);
echo MY_CONST;      // 適当な変数

define('NOW', microtime(true));
echo NOW;           // 1626407918.5442
```  
- constは変数や、関数の戻り値が<span style="color: red;">使えない。</span>
```php
$val = '適当な変数';
const MY_CONST = $val; 
// PHP Fatal error:  Constant expression contains invalid operations

const NOW = microtime(true);
// PHP Fatal error:  Constant expression contains invalid operations
```  
<br>

### トップレベル以外で、使えるか使えないか  
- defineはifやfor、functionの中で<span style="color: red;">使える。</span>  
```php
if (true) {
    define('MY_CONST', 'defineはifの中でも使える');
}
```
- constはifやfor、functionの中で<span style="color: red;">使えない。</span>  
```php
if (true) {
    const MY_CONST = 'constは構文エラーになる';
    // PHP Parse error:  syntax error, unexpected 'const' (T_CONST) in ...
}
```
<br>

### クラス定数  
- defineはクラス定数を定義するときに<span style="color: red;">使えない。</span> 
```php
class Example {
    public define('MY_CONST', 'defineは関数だから構文エラーになる');
}
// PHP Parse error:  syntax error, unexpected 'define' (T_STRING), ・・・
```

- constはクラス定数を定義するときに<span style="color: red;">使える。</span>  
```php
class Example {
    public const MY_CONST = 'クラス定数を定義するときに使う';
}
echo Example::MY_CONST;
// クラス定数を定義するときに使う
```  
<br>


### 名前空間  
- defineは名前空間上に登録される。   
- constはグローバルに登録される。 
```php
namespace My {
    define('MY_CONST', 'defineで定義した定数');
    const MY_CONST = 'constで定義した定数';
}

namespace Example {
    echo MY_CONST;      // defineで定義した定数
    echo \My\MY_CONST;   // constで定義した定数。
}
```

<br>


# 外部ファイルを読み込む際の使い分け  
- require : ファイル読み込みに失敗した場合に、致命的なエラーが発生する。つまりスクリプトの処理が止まる。  
- require_once : requireとほぼ同じ動きをするが、ファイルがすでに読み込まれているかをチェックする。すでに読み込まれている場合は、ファイルを読み込まない。

```php
//require.php
$test_require_str = 'hoge';
```
```php
//require_once.php
$test_require_once_str = 'hoge';
```
```php
require 'require.php';
echo $test_require_str.PHP_EOL; //出力結果はhoge

require_once 'require_once.php';
echo $test_require_once_str.PHP_EOL; //出力結果はhoge

//変数の値をそれぞれ変更してみる
$test_require_str = 'hoge_change';
$test_require_once_str = 'hoge_change';

//requireでの読み込みなので、通常通り読み込まれ変数の値が再度変更される
require 'require.php';
echo $test_require_str.PHP_EOL; //出力結果はhoge

//require_onceでの読み込みなので、2回目は読み込まれず変数の値は変更されない
require_once 'require_once.php';
echo $test_require_once_str.PHP_EOL; //出力結果はhoge_change
```

- include、include_once : ファイルが見つからない場合は warning が発生する。スクリプトは終了せずに処理は続行する。  
```php
// requireでファイル名不備
require ''; //この時点でエラーになり、処理が停止される
include 'include.php';

//処理が停止されているので、当然出力されない
echo 'ファイルの読み込みをしました。';
```
```php
require 'require.php';
//includeでファイル名不備
include '';     // includeなので処理は停止されない

//Warningにはなりますが、処理は停止されないので出力される
echo 'ファイルの読み込みをしました。';
```
<br>

# staticメソッド、staticプロパティ  
staticメソッドにはクラス共通になるためthisは使えない。(thisが特定できないため)  
オブジェクトごとに値が変えたくないものに使う。  
どのオブジェクトからも共通の値が取得できる。

<br>

# abstract(抽象クラス)の使い方  
- 子クラスの方で定義を強制したいというルールを作りたい場合に便利  
- 参考動画 : [ドットインストール](https://dotinstall.com/lessons/basic_php_objects/54017)
```php

// 継承を前提としたクラス
// BasePostという抽象クラスを作る
abstract class BasePost {
    protected $text;

    public function __construct($text){
        $this->text = $text;
    }
  
    // 子クラスで必ず定義させる(抽象メソッド)
    abstract public function show();
}


class Post extends BasePost {
    public function show() {
        printf('%s' . PHP_EOL, $this->text);
    }

    // parent::__construct を使わずに $this->text が参照できる理由は、親のコンストラクタをそのまま使っているから。
}


class SponsoredPost extends BasePost {
    private $sponsor;
    
    // 第二引数を渡しているので、独自のコンストラクタを使いたいから parent::__constructを呼び出している。
    public function __construct($text, $sponsor) {
        parent::__construct($text);
        $this->sponsor = $sponsor;
    }

  // 例えばこのshowメソッドをコメントアウトするとエラーになる。
  public function show() {
    printf('%s by %s' . PHP_EOL, $this->text, $this->sponsor);
  }
}

$posts = [];
$posts[0] = new Post('hello');
$posts[1] = new Post('hello again');
$posts[2] = new SponsoredPost('hello hello', 'dotinstall');

function processPost(BasePost $post) {
    $post->show();
}

foreach ($posts as $post) {
    processPost($post);
}
```

<br>

# interface(抽象クラス)の実装のやり方  
- 抽象メソッドだけを持つことができて、そのクラスを好きな定義に強制させることができる。  
- 1つのクラスでいくつでも、interfaceを実装できる。  
- 継承関係のないクラスにもインターフェイスを実装できる。  
- 参考動画 : [ドットインストール](https://dotinstall.com/lessons/basic_php_objects/54020)
```php
// インターフェイス実装
// likeメソッドを implements を使って好きなクラスに強制させる
interface LikeInterface {
    public function like();
}

abstract class BasePost{
    protected $text;

    public function __construct($text) {
        $this->text = $text;
    }

    abstract public function show();
}


// implementsでインターフェイスを実装する
class Post extends BasePost implements LikeInterface　{
    private $likes = 0;
    
    public function like()　{
        $this->likes++;
    }

    public function show()　{
        printf('%s (%d)' . PHP_EOL, $this->text, $this->likes);
    }
}


class SponsoredPost extends BasePost　{
    private $sponsor;

    public function __construct($text, $sponsor)　{
        parent::__construct($text);
        $this->sponsor = $sponsor;
    }

    public function show()　{
        printf('%s by %s' . PHP_EOL, $this->text, $this->sponsor);
    }
}


// implementsでインターフェイスを実装する
class PremiumPost extends BasePost implements LikeInterface　{
    private $price;
    private $likes = 0;
    
    public function like()　{
        $this->likes++;
    }
    
    public function __construct($text, $price)　{
        parent::__construct($text);
        $this->price = $price;
    }

    public function show()　{
        printf('%s (%d) [%d JPY]' . PHP_EOL, $this->text, $this->likes, $this->price);
    }
}

$posts = [];
$posts[0] = new Post('hello');
$posts[1] = new Post('hello again');
$posts[2] = new SponsoredPost('hello hello', 'dotinstall');
$posts[3] = new PremiumPost('hello there', 300);

$posts[0]->like();
$posts[3]->like();

function processPost(BasePost $post) 
{
  $post->show();
}

foreach ($posts as $post) {
  processPost($post);
}
```

<br>

# トレイトとは

<br>
<br>
<br>


# setcookieの使い方
デベロッパーツールのapplicationタグのHttpOnlyとSecureも調べとく。
HttpOnlyはJavascriptでcookieをせっとできないようにする。
Secureはhttpsのときのみcookieがセットできるようにする。


<br>

# Todo
- filter_input、filter_input_arrayについて　送信されたものをそのまま利用するのではなく、filterをかけるのが重要  

<br>
<br>

# 配列の削除方法  
- unset  
- array_shift  

# URL  
- parse_url  
- parse_str  

# self static parentの違い


<br>
<br>
<br>
<br>
<br>


- namesupaceで設定すると、PHPが標準で用意しているクラスにもnamespaceがついてしまう。それを会費するには、PHPが標準で用意しているクラスに「\」をつけてあげる。   

- strposの使い方: https://techacademy.jp/magazine/11704  
    - mb_strpos マルチバイトに対応した関数(日本語の場合)

- substrの使い方: https://blog.codecamp.jp/php-substr  

- mb_strlen: マルチバイトに対応した関数(日本語の場合)

- substr  

- substr_replace  

- number_format  

- preg_match  

- preg_match_all  

- preg_replace  

- implode

- explode

- array_slice: 配列を切り出す。元の配列はそのままで、新しい配列として作成される。  

- array_splice: 配列の要素を削除する。array_sliceとは違い、元の要素の配列を削除する。
