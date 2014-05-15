###php 静态绑定

下面是 ZendFramework2 中的一段代码

```php
/**
 * Constructor.
 *
 * Data is read-only unless $allowModifications is set to true
 * on construction.
 *
 * @param  array   $array
 * @param  bool $allowModifications
 */
public function __construct(array $array, $allowModifications = false)
{
    $this->allowModifications = (bool) $allowModifications;

    foreach ($array as $key => $value) {
        if (is_array($value)) {
        
            // 看这部分 new static
            $this->data[$key] = new static($value, $this->allowModifications);
        } else {
            $this->data[$key] = $value;
        }

        $this->count++;
    }
}
```
下面一段是 php 官方的文档解释
```
自 PHP 5.3.0 起，PHP 增加了一个叫做后期静态绑定的功能，用于在继承范围内引用静态调用的类。
准确说，后期静态绑定工作原理是存储了在上一个“非转发调用”（non-forwarding call）的类名。当进行静态方法调用时，该类名即为明确指定的那个（通常在 :: 运算符左侧部分）；当进行非静态方法调用时，即为该对象所属的类。所谓的“转发调用”（forwarding call）指的是通过以下几种方式进行的静态调用：self::，parent::，static:: 以及 forward_static_call()。可用 get_called_class() 函数来得到被调用的方法所在的类名，static:: 则指出了其范围。

该功能从语言内部角度考虑被命名为“后期静态绑定”。“后期绑定”的意思是说，static:: 不再被解析为定义当前方法所在的类，而是在实际运行时计算的。也可以称之为“静态绑定”，因为它可以用于（但不限于）静态方法的调用。
```

具体看几个例子，来加深对这部分内容的理解，例子来自 [stackoverflow][1]

```php
class A {
    public static function get_self() {
        return new self();
    }
    public static function get_static() {
        return new static();
    }
}

class B extends A {}

echo get_class(B::get_self());  // A
echo get_class(B::get_static()); // B
echo get_class(A::get_static()); // A
```

下面的例子是来自 [php][2] 手册
```php
<?php
class A {
    public static function who() {
        echo __CLASS__;
    }
    public static function test() {
        self::who();
    }
}

class B extends A {
    public static function who() {
        echo __CLASS__;
    }
}

B::test(); // A
?>

<?php
class A {
    public static function who() {
        echo __CLASS__;
    }
    public static function test() {
        static::who(); // 后期静态绑定从这里开始
    }
}

class B extends A {
    public static function who() {
        echo __CLASS__;
    }
}

B::test(); // B
?>
```
  [1]: http://stackoverflow.com/questions/5197300/new-self-vs-new-static "stackoverflow"
  [2]: http://php.net/manual/zh/language.oop5.late-static-bindings.php