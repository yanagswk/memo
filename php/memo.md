## self this違い  
- self::は自クラスを表し、$thisはインスタンス化した自身のオブジェクトを表す。  
- self::はクラス定数やstatic変数など静的なプロパティにアクセスできるのに対し、$thisは静的なプロパティにはアクセスできない。  
- self::を使うときは、呼び出すメソッドやプロパティは、staticやconstをつけて静的なものとして呼び出すようにする。
- 静的なプロパティにアクセスする際は、self::を使用し、動的なプロパティやメソッドにアクセスする際は$thisを使用するなどの使い分けをするといい。  
- 静的メソッドのなかで、thisキーワードは使えない。  
- 静的プロパティはインスタンスごとの値ではなくクラスごとの値であるため、基本的に変更すべき値ではない。よって、静的プロパティは読み取り専用の用途で使う。  
- 静的プロパティ/静的メソッドとは、「インスタンスを生成しなくてもオブジェクトから直接呼び出せるプロパティ/メソッド」のこと  

```php
<?php
class ParentClass {
    function test() {
        self::who(); // output 'parent'
        $this->who(); // output 'child'
    }

    function static who() {
        echo 'parent ';
        }
}


class ChildClass extends ParentClass {
    function who() {
        echo 'child ';
    }
}

$obj = new ChildClass();
$obj->test();
```