# 配列  

## barray_slice(配列, 位置, 個数): array  
配列の何番目からいくつ取り出すかを指定して、配列で取得する。  
元の配列は変化なし。   
4つ目の引数をtrueにすると、取り出した値の数値キーをそのまま取得する。  
```php
$array = array("a", "b", "c", "d", "e");
$output = array_slice($array, 2, 2);
$output2 = array_slice($array, 2, 2, true);

print_r($array);    // 元の配列
print_r($output);   // 取得した配列
print_r($output2);   // true指定
```
```
Array ( [0] => a [1] => b [2] => c [3] => d [4] => e )
Array ( [0] => c [1] => d )
Array ( [2] => c [3] => d )
```
array_slice() に、数値と文字列のキーが混じった連想配列を渡す例  
```php
$ar = array('a'=>'apple', 'b'=>'banana', '42'=>'pear', 'd'=>'orange');
print_r(array_slice($ar, 0, 3));
print_r(array_slice($ar, 0, 3, true));
```
```
Array ( [a] => apple [b] => banana [0] => pear ) 
Array ( [a] => apple [b] => banana [42] => pear )
```

<br>

## array_merge(配列, ...配列): array  
配列同士を結合。  
キー名が重複したら、上書きされる。
引数には、配列を入れる。空文字やNULLが一つでもある場合は、NULLを返す。
```php
$array1 = array("color" => "red", 2, 4);
$array2 = array("a", "b", "color" => "green", "shape" => "trapezoid", 4);
$result = array_merge($array1, $array2);
print_r($result);
```
```
Array ( [color] => green [0] => 2 [1] => 4 [2] => a [3] => b [shape] => trapezoid [4] => 4 )
```
    
キーの数値は振りなおされる。  
```php
$array1 = array();
$array2 = array(1 => "data");
$result = array_merge($array1, $array2);
```  
```
Array ( [0] => data )
```
二番目の配列の要素を最初の配列に追加したい (最初の配列に存在する要素は上書きせず、添字も変更しない) 場合は、  
配列結合演算子 + を使う。
```php
$array1 = array(0 => 'zero_a', 2 => 'two_a', 3 => 'three_a');
$array2 = array(1 => 'one_b', 3 => 'three_b', 4 => 'four_b');
$result = $array1 + $array2;
```
```
Array ( [0] => zero_a [2] => two_a [3] => three_a [1] => one_b [4] => four_b )
```

<br>

## array_merge_recursive(配列, ...配列): array  
配列同士を結合するが、array_mergeとは違い、キーが重複しても、上書きされない。  
```php
$array1 = array("color" => "red", 2, 4);
$array2 = array("a", "b", "color" => "green", "shape" => "trapezoid", 4);
$result2 = array_merge($array1, $array2);
$result1 = array_merge_recursive($array1, $array2);
print_r($result1);
print_r($result2);
```
```
Array ( [color] => green [0] => 2 [1] => 4 [2] => a [3] => b [shape] => trapezoid [4] => 4 )
Array ( [color] => Array ( [0] => red [1] => green ) [0] => 2 [1] => 4 [2] => a [3] => b [shape] => trapezoid [4] => 4 )  
```
<br>

## in_array(mixed $needle, array $haystack, bool $strict = false): bool    
配列のhaystack 内の needle を検索する。 strict をtrueにすると型の比較を行う。
配列の中に指定された値が含まれているかを返す。  
```php
$os = array("Mac", "NT", "Irix", "Linux");
$result = in_array("Mac", $os) ? "true" : "false";
echo $result;   // true
```
needleが配列の場合  
```php
$a = array(array('p', 'h'), array('p', 'r'), 'o');
if (in_array(array('p', 'h'), $a)) echo "true";     // true
```

<br>

## shuffle(array &$array): bool  
配列をシャッフルする。成功したらtrue。 
元の配列を返る。  
```php
$numbers = range(1, 20);
shuffle($numbers);
print_r($numbers);
```

<br>

## ソート  
  - sort(), rsort()・・・配列をソート
  - asort(), arsort()・・・連想配列をソート(valueでソートされる。)
  - ksort(), krsort()・・・キーをソート

<br>

- usort(), uasort(), uksort()  
    ユーザーが定義した関数に基づいて配列をソート

<br>

##  array_multisort()  

<br>

## array_unique(array $array, int $flags = SORT_STRING): array
配列から重複した値を削除する。  
```php
$input = array("a" => "green", "red", "b" => "green", "blue", "red");
$result = array_unique($input);
print_r($result);
```
```
Array ( [a] => green [0] => red [1] => blue )
```

<br>

## array_walk(array|object &$array, callable $callback, mixed $arg = null): bool  
配列の全ての要素にユーザー定義の関数を適用する
```php
$fruits = array("d" => "lemon", "a" => "orange", "b" => "banana", "c" => "apple");

function test_alter($item1, $key, $prefix)
{
    $item1 = "$prefix: $item1";
}

function test_print($key, $item2)
{
    echo "$key. $item2\n";
}

echo "Before ...:\n";
array_walk($fruits, 'test_print');

array_walk($fruits, 'test_alter', 'fruit');
echo "... and after:\n";

array_walk($fruits, 'test_print');
```
```
Before ...:
d. lemon
a. orange
b. banana
c. apple
... and after:
d. fruit: lemon
a. fruit: orange
b. fruit: banana
c. fruit: apple
```

<br>

## array_search(mixed $needle, array $haystack, bool $strict = false): int|string|false  
haystack 内の needle を検索する。  
指定した値を配列で検索し、見つかった場合に対応する最初のキーを返す。  
見つからない場合は、falseを返す。
```php
$array = array(0 => 'blue', 1 => 'red', 2 => 'green', 3 => 'red');
$key = array_search('green', $array);
echo $key;      // 2
```
