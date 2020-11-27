## php 变量

$x = 5;
$y = 6;
$z = $x + $ y;
echo $z;

### php 变量作用域

-   local
-   global
-   static
-   parameter

### 局部和全局作用域

函数外部定义的变量，拥有全局作用域。除函数外，全局变量可以被脚本中的任何部分访问，在函数内部访问一个全局变量，需要使用 global 关键字。  
php 函数内部声明的变量是局部变量，仅能在函数内部访问。

### php global 关键字

global 关键字用于函数内访问全局变量。  
在函数内调用函数外定义的全局变量，需要在函数中的变量前加上 global 关键字。  
**php 将所有全局变量存储在一个名为\$GLOBALS[index]的数组中。index 保存变量的名称，这个数组可以在函数内部访问，也可以直接用来更新全局变量**

### static 作用域

函数完成时，所有变量通常都会被删除，若希望某个局部变量不会被删除，可以在声明时使用 static 关键字。

```
<?php
function myTest()
{
    static $x=0;
    echo $x;
    $x++;
    echo PHP_EOL;    // 换行符
}

myTest();
myTest();
myTest();
?>
```

每次调用函数时，变量将会保留着函数前一次被调用时的值，变量仍为函数的局部变量。

### 参数作用域

参数是通过调用代码将值传递给函数的局部变量。  
参数是在参数列表中声明的，作为函数声明的一部分。

## php5 echo 与 print 语句

区别：

-   echo 可以输出一个或多个字符串。
-   print 只允许输出一个字符，返回值为 1
-   echo 输出速度比 print 快，echo 没有返回值，print 有返回值 1。

### php echo 语句

echo 可以一次输出多个值，值之间使用逗号分隔。echo 是语言结构(language construct)，并不是真正的函数，因此不能作为表达式的一部分使用。  
print 打印成功显示则返回 true，失败返回 false。  
print_r()可以将字符串和数字简单打印出来，数组则以括起来的键和值得列表形式显示，并以 Array 开头，但输出的布尔值与 null 的结果没有意义，都是打印\n。  
var_dump():判断一个变量的类型与长度，并输出变量的数值，如果变量有值输得是变量的值并返回数据类型，此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值，数组将递归展开值，通过缩进显示其解构其结构。

## php EOF(heredoc)使用

是一种在命令行和程序语言中里定义一个字符串的方法。  
使用概述：

1. 必须后接分号。
2. EOF 可以用其他任意字符代替，只需保证结束标识与开始标识一致。
3. 结束标识必须顶格独占一行。
4. 开始表示只可以不带引号或带单双引号，不带引号与带双引号效果一致，解释内嵌的变量与转义符号，带单引号则不解释内嵌的变量和转义符号。
5. 当内容需要内嵌引导时，不需要加转义符，本身对双引号转义。

## php 数据类型

字符串  
整型：

-   必须有一个数字
-   整数不能包含逗号或者空格
-   整数没有小数点
-   整数可以是正数负数
-   整型可以用三种格式：十进制，十六进制(0x)，八进制(0)

浮点型：

-   带小数点或者指数形式

布尔型：  
数组：
php 的数组是一个有序映射，映射是一种将 values 关联到 keys 的类型。

```
array(key => value,...)
//  键key可以是一个整数integer或字符串string。
//  值value可以是任意类型的值。
```

```
//  php5.4后
$array = [
    "foo" => "bar",
    "bar" => "foo",
];
```

```
<?php
    $cars=array("Volvo","BMW","Toyota");
    var_dump($cars);
?>
```

对象：  
对象数据类型也可以用于存储数据。  
在 php 中，对象必须声明。  
首先，你必须使用 class 关键字声明类对象。类是可以包含属性和方法的结构。  
在类中定义数据类型，然后在实例化的类中使用数据类型。

```
<?php
    class Car{
        var $color;
        function __construct($color="green") {
            $this->color = $color;
        }
        function what_color() {
            return $this->color;
        }
    }
?>
```

NULL:  
null 表示没有值。  
null 指明一个变量是否为空值。  
设置变量值为 null 来清空变量数据。

## php 类型比较

-   松散比较：== 只比较值，不比较类型。
-   严格比较： === 既比较值也比较类型。

### php 中比较 0，false，null

```
<?php
echo '0 == false: ';
var_dump(0 == false);
echo '0 === false: ';
var_dump(0 === false);
echo PHP_EOL;
echo '0 == null: ';
var_dump(0 == null);
echo '0 === null: ';
var_dump(0 === null);
echo PHP_EOL;
echo 'false == null: ';
var_dump(false == null);
echo 'false === null: ';
var_dump(false === null);
echo PHP_EOL;
echo '"0" == false: ';
var_dump("0" == false);
echo '"0" === false: ';
var_dump("0" === false);
echo PHP_EOL;
echo '"0" == null: ';
var_dump("0" == null);
echo '"0" === null: ';
var_dump("0" === null);
echo PHP_EOL;
echo '"" == false: ';
var_dump("" == false);
echo '"" === false: ';
var_dump("" === false);
echo PHP_EOL;
echo '"" == null: ';
var_dump("" == null);
echo '"" === null: ';
var_dump("" === null);
0 == false: bool(true)
0 === false: bool(false)

0 == null: bool(true)
0 === null: bool(false)

false == null: bool(true)
false === null: bool(false)

"0" == false: bool(true)
"0" === false: bool(false)

"0" == null: bool(false)
"0" === null: bool(false)

"" == false: bool(true)
"" === false: bool(false)

"" == null: bool(true)
"" === null: bool(false)
```

## 常量

### 设置常量

使用 define()函数：

```
define( string $name, mixed $value, case_insensitive);
- name: 常量名称，即标志符。
- value: 常量的值。
- case_insensitive: 可选参数，true对大小写不敏感，默认敏感。
// 合法的常量名
define("FOO",     "something");
define("FOO2",    "something else");
define("FOO_BAR", "something more");
```

define 函数不允许在 class 中使用。  
使用 define 函数创建的常量默认为全局常量。

### php 魔术常量

## 字符串变量

### 并置运算符

并置运算符用于将两个字符串值连接起来。

```
<?php
    $txt1="Hello world!";
    $txt2="What a nice day!";
    echo $txt1 . " " . $txt2;
?>
```

### 字符串函数

1. strlen()  
   返回字符串的长度(字节数)
2. strpos()  
   用于在字符串内查找一个字符或一段指定的文本。  
   找到匹配，则会返回第一个匹配的字符位置，若未找到则返回 false。

```
<?php
    echo strpos("Hello world!","world");
?>
输出：6
```

......

## 运算符

### 算数运算符

### 赋值运算符

### 递增递减运算符

### 比较运算符

### 逻辑运算符

| 运算符  | 描述                                 |
| ------- | ------------------------------------ |
| x and y | xy 都为 true 则返回 true             |
| x or y  | xy 中一个为 true 则返回 true         |
| x xor   | xy 中有且只有一个为 true 则返回 true |
| x && y  | xy 都为 true 则返回 true             |
| x       |                                      | y | xy 有一个为 true 则返回 true |
| !x      | x 不为 true 则返回 true              |

### 数组运算符

| 运算符 | 名称   | 描述                                                 |
| ------ | ------ | ---------------------------------------------------- |
| x+y    | 集合   | x 和 y 的集合                                        |
| x==y   | 相等   | x 与 y 具有相同的键值对，则返回 true                 |
| x===y  | 恒等   | x 与 y 具有相同的键值对，且顺序类型相同，则返回 true |
| x!=y   | 不相等 | x 不等于 y，返回 true                                |
| x<>y   | 不相等 | x 不等于 y，返回 true                                |
| x!==y  | 不恒等 | x 不等于 y，返回 true                                |

### 三元运算符

`(expr1) ? (expr2) : (expr3)`

```
<?php
$test = '菜鸟教程';
// 普通写法
$username = isset($test) ? $test : 'nobody';
echo $username, PHP_EOL;

// PHP 5.3+ 版本写法
$username = $test ?: 'nobody';
echo $username, PHP_EOL;
?>
```

PHP7+增加 null 合并运算符？？

```
<?php
// 如果 $_GET['user'] 不存在返回 'nobody'，否则返回 $_GET['user'] 的值
$username = $_GET['user'] ?? 'nobody';
// 类似的三元运算符
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';
?>
```

### 组合比较符

也被称为太空船操作。<=>  
`$c = $a <=> $b`;  
若 a > b,c 的值为 1;  
若 a == b,c 的值为 0;  
若 a < b,c 的值为-1;

## 函数参考

### 数组函数

```
is_array()  //检测变量是否是数组。
explode()   //使用一个字符串分割另一个字符串。
使用： explode(string $delimiter, string $string[]) : array
返回值： 函数返回由字符串组成的array，每个元素都是string的一个字串，它们被字符串delimiter作为边界点分割出来。
若分割字符串delimiter为空字符串""，则explode()将返回false。如果delimiter所包含的值string中找不到，并且使用了负数的limit，则会返回空的array，否则返回string单个元素的数组。
isset()     //检测变量是否已经设置并且非NULL
```

## switch

```
<?php
switch(condition){
    case 1:
        ...;
        break;
    case 2:
        ...;
        break;
}
>
```

## php 数组

php 中的数组是一个有序映射，映射是一种把 values 关联到 keys 的类型。  
可以用 array()来新建一个数组。  
5.4 起可以使用[]代替 array  
\$array = [
"foo" => "bar"
]  
key 可以是 integer 或者 string，value 可以是任何类型。

1. 浮点型会被转化为整型。
2. 布尔值会被转化为整型
3. null 会被转化为空字符串，""
4. 数组和对象不能被用作键名

### php 关联数组

`$arr = array("foo1" => "111")`
`$arr["foo1"] = "111"`
遍历关联数组，可以使用 foreach

```
<?php
    $arr = array("peter" => "35", "ben" => "11");
    foreach($arr as $x => $x_value){
        echo "Key=" . $x . ",Value" . $x_value;
        echo "<br>";
    }
>
```

note: foreach 方法进能够应用于数组和对象

### 数组方法

1. 获取数组长度 -- count(\$array)
2. 比较数组 -- array_diff()

### 数组排序

-   sort -- 对数组升序排列
-   rsort -- 对数组降序排列
-   asort -- 根据关联数组的值，对数组升序排列
-   ksort -- 根据关联数组的键，对数组升序排列
-   arsort -- 根据关联数组的值，对数组降序排列
-   krsort -- 根据关联数组的值，对数组降序排列

### 数组解引用

```
$arr = array["1", "2", "3"];
//  1
$tmp = $arr[1];
//  2
list(,$tmp) = $arr;
```

### 新建/修改

```
<?php
    $arr = arr(5 => 1,12 => 2);
    $arr[] = 56     //  13 => 56
    $arr["x"] = 42      //  "x" => 42
    unset($arr[5])
>
```

unset 函数允许删除数组中的键，但不会重建索引，删除后重建索引可以用 array_values()函数。

```
<?php
    $a = array(1 => 1, 2 => 2);
    unset($a[2]);
    $b = array_values($a);
>
```

数组的赋值，引用运算符通过引用来拷贝数组。

```
引用赋值&
<?php
    $arr1 = array(2,3);
    $arr2 = $arr1;
    $arr2[] = 4;    //  $arr2 changed, $arr1(2,3)
    $arr3 = &$arr1;
    $arr3[] = 4     // $arr1 and $arr3 are the same
>
```

## php 函数

### 可变参数

自定义函数中支持可变的参数列表，由...语法实现。  
php5.6 后的版本，包含。。。的参数，会转换为指定参数变量的一个数组。

```
function(...$args){
    $num = count($args);    //统计参数个数
    foreach($args as $arg){
        echo $arg . " ";
    };  遍历数组打印参数
}
```

### 自定义函数

函数中函数的调用，需要调用外部函数执行，使得内部函数变为已定义的函数。  
php 不支持函数重载，也不能取消定义或者重定义已声明的函数。

### 递归函数

note: 避免递归函数调用超过 100-200，可能会使堆栈崩溃使当前的脚本终止，无限递归被视为编译错误。

```
<?php
    function recursion($a){
        if($a < 20){
            echo "$a\n";
            recursion($a + 1);
        }
    }
>
```

### 函数参数

默认情况函数参数通过值传递，即在内部改变参数的值不会影响到函数外部的值。若通过引用传递，则会修改外部的值。

### 默认参数

```
<?php
    function make($type = 'a'){
        echo "make $type\n"
    }
    echo make();    //make a
    echo make(null);    //make
    echo make(b);   //make b
>
```

### 函数引用

从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用引用运算符&。

```
<?php
    function &returns_reference(){
        return $someref;
    }
    $newref = &returns_reference;
?>
```

### 类型声明

### 匿名函数

也称为闭包函数，允许临时创建一个没有指定名称的函数，经常作为回调函数 callable 参数的值。  
匿名函数是通过 Closure 类实现的。

-   匿名函数

```
<?php
    echo preg_replace_callback('~-([a-z])~', function ($match) {
        return strtoupper($match[1]);
    }, 'hello-world');
    // 输出 helloWorld?>
```

-   从父作用域继承变量

```
<?php
    $message = 'hello';
    // 没有 "use"
    $example = function () {
        var_dump($message);
    };
    echo $example();

    // 继承 $message
    $example = function () use ($message) {
        var_dump($message);
    };
    echo $example();

    // Inherited variable's value is from when the function
    // is defined, not when called
    $message = 'world';
    echo $example();

    // Reset message
    $message = 'hello';

    // Inherit by-reference
    $example = function () use (&$message) {
        var_dump($message);
    };
    echo $example();

    // The changed value in the parent scope
    // is reflected inside the function call
    $message = 'world';
    echo $example();

    // Closures can also accept regular arguments
    $example = function ($arg) use ($message) {
        var_dump($arg . ' ' . $message);
    };
    $example("hello");
    //  result
    Notice: Undefined variable: message in /example.php on line 6
    NULL
    string(5) "hello"
    string(5) "hello"
    string(5) "hello"
    string(5) "world"
    string(11) "hello world"
?>
```  

