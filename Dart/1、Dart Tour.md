## 1、Dart 2 一览

### 前言

Flutter是一个Google官方维护的移动开发框架，拥有很多现代框架所拥有的一些有意思的特性，比如hot reload，60fps高性能刷新律的完全self-drawing widgets，现在越来越多的进入开发者的视野，所以，我特意了解一下Flutter开发所用的Dart 语言，现在Dart出了2版本，或许跟第1版有多少不同，但应该不会像Python3和Python2那样差异巨大。

我们按照官网看一下Dart大体的语言语法和库的用法，本文为官网Tour的笔记和部分翻译，会与原文有大量出入。



### 一个基本的Dart例子

```dart
// 定义一个函数.
printInteger(int aNumber) {
  print('The number is $aNumber.'); // Print to console.
}

//程序执行入口
main() {
  var number = 42; // 声明和初始化变量
  printInteger(number); // 调用一个函数
}
```
main函数的简单说明
```dart
//这是单行注释

/*
 * 这是多行注释
 */

///
/// 这是文档注释
///

int 整形类型
String  字符串类型
List   列表类型
bool 布尔类型

print()   打印函数
'单引号字符串'  
"双引号字符串"

$变量插值为字符串
${变量、表达式插值为字符串}

main()  main函数，是一个特殊的函数，它是必须存在的，用于指定应用执行开始的函数。void main()是无返回类型的入口函数，可以存在List<String>类型的参数，例如:
    void main(List<String> arguments){
        ....
    }


   
```

### 重要概念

使用Dart语言时，要牢记这些事实和概念：

- 所有变量引用的都是对象，每个对象都是类的实例。甚至数字、函数和null都是对象。所有的对象有继承自Object。
- 尽管Dart是`强`类型的，类型指定是可有可无的，Dart可以推断类型。上面的代码中，数字会被推断成int类型。当你想要明确说明却没有想要使用的类型，那就使用一个dynamic类型。
- Dart支持泛型。
- 如List <int>（整数列表）或List <dynamic>（任何类型的对象列表） 
- Dart支持顶层函数、对象成员函数(static和实例方法)。支持嵌套函数和局部函数。
- Dart支持顶级变量，也支持绑定在类或者对象的成员变量。实例变量也就是我们熟知的域或属性。
- 不像Java，Dart没有public、protected和private。只有以下划线“_” 开始的才是库中私有的。
- 可以用字母和下划线开头，然后是字符和数字的组合。
- 要明确区分表达式和声明
- Dart会报告警告和错误。警告不会中断代码的运行，只是说明隐患，错误会在编译时和运行时出错。



### 关键字

| abstract (1)  | do             | import (1)    | super       |
| ------------- | -------------- | ------------- | ----------- |
| as (1)        | dynamic (1)    | in            | switch      |
| assert        | else           | interface (1) | sync* (2)     |
| async (2)       | enum           | is            | this        |
| async* (2)      | export (1)     | library (1)   | throw       |
| await (2)       | external (1)   | mixin (1)     | true        |
| break         | extends        | new           | try         |
| case          | factory (1)    | null          | typedef (1) |
| catch         | false          | operator (1)  | var         |
| class         | final          | part (1)      | void        |
| const         | finally        | rethrow       | while       |
| continue      | for            | return        | with        |
| covariant (1) | get (1)        | set (1)       | yield (2)     |
| default       | if             | static (1)    | yield* (2)    |
| deferred (1)  | implements (1) |               |             |

1、标明为1的是内建标识符，防止使用这些内建标识符作为用户标识符。一个编译时期错误发生时，当你尝试使用内建关键字尝试定义类名或者类型名时会报编译时错误。

2、标明为2时更新的，实在Dart  1.0以后的用于异步的保留字。你不能使用async、await或yield。

关键字表中的所有其他字都是保留字。 您不能使用保留字作为标识符。 



### 变量

一个变量声明并初始化：

```dart
var name = 'Bob';
```

变量存储的是引用，名称为`name`包含一个值为`Bob`的引用。

`name` 变量会被推断为`String`, 你可以通过直接指定类型名称更改它的类型。如果对象没有被严格限制为某种类型，那么应该指定为`Object`或者`dynamic`类型:

```dart
dynamic name = 'Bob';
```

或者可以显式指定类型名:

```dart
String name = 'Bob';
```

> 注意，对于[设计指导](https://www.dartlang.org/guides/language/effective-dart/design#types)的建议，对于**局部变量**更推荐使用`var`关键字，而不是直接指定类型名。



#### 默认值

未初始化变量存在一个初始值为`null`。即使是数字类型，未初始化变量的初始值也为`null`，因为在Dart里，所有的一切皆为对象(不同于Java, 基本类型不是对象)。

```dart
int lineCount;
assert(lineCount == null)
```

> 注意：assert断言会在生产代码中被**忽略**。开发进程中，assert(条件) 会在条件不成立时抛出异常。
>
> 细节部分见**[断言](https://www.dartlang.org/guides/language/language-tour#assert)**



#### final和const值

不变量值(final)和静态常量值(const)，如果要定义不可变量值，使用`final`或者`const`而不是使用`var`更合适。一个final值，只能被赋值一次；一个const值是再编译时期就计算完成而不再变化的常量。(const量也是隐式的final量值)。一个顶级的final量或者类变量会在第一次被使用时初始化。

> 注意：实例变量可以赋值给final变量，但是不可以是const。

创建和设置final 变量

```dart
final name = 'Bob';
name = 'Alice'; //本行代码会报错
```

使用`const`可以将编译期值复制给变量。如果const值在类级别，会标识为`static const`。在声明该变量的位置，可以将该值设置为编译时常量，例如数字或字符串文字，常量变量或常数运算结果: 

```dart
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere
```

const关键字不仅用于声明常量变量。您也可以使用它来创建常量值，以及声明创建常量值的构造函数。任何变量都可以有一个常量值。 

```dart
// Note: [] creates an empty list.
// const [] creates an empty, immutable list (EIL).
var foo = const []; // foo is currently an EIL.
final bar = const []; // bar will always be an EIL.
const baz = const []; // baz is a compile-time constant EIL.

// You can change the value of a non-final, non-const variable,
// even if it used to have a const value.
foo = [];

// You can't change the value of a final or const variable.
// bar = []; // Unhandled exception.
// baz = []; // Unhandled exception.
```



### 内建类型

Dart有以下内建类型：

```dart
numbers
strings
booleans
lists (可以认知为arrays)
maps
runes (String当中的Unicode字符码点)
symbols
```

由于Dart处处是对象，所以可以使用*构造函数*来初始化变量。有的内建类型有自己的构造函数，例如使用Map()构造函数可以创建一个映射，可以这么用`new Map()`



#### 数字类型

包含`int`和`double`两种变体。

int 类型不大于64位。double类型是64位浮点数。他们都是`num`类型的子类型。

可以使用基本运算符: + - \* / 以及`abs()`，`ceil()`和`floor()`方法，位运算符`>>`,`<<`, `|`,`&`等，都在int类中有定义。如果int内建的基本运算不足以使用,  `dart:math`库中可能会有。

int类型的定义方法：

```dar
int x = 1;
int hex = 0xED2A;
```

double类型定义方法：

```dart
double y = 1.1;
double exp = 1.4e5;
```

字符<=>数字  互转：

```dart
//String -> int
var one = int.parse('1');
assert(one == 1);

//String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1)
    
//int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1')

//double -> String
String piAsString = 3.1415926.toStringAsFixed(2)
assert(piAsString == '3.14')
```

int类型同样可以使用传统位运算：

```dart
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 >> 1) == 1); // 0011 >> 1 == 0001
assert((3 | 4) == 7); // 0011 | 0100 == 0111
```

编译期常量，数学表达式会在编译期间计算并赋值。

```dart
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```



#### 字符串类型

一个Dart字符串是UTF-16标准的字符单位序列。你可以使用单引号或者双引号创建字符串。

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

可以使用变量表达式在字符串中获取表达式的值，格式是`${值表达式}`，如果表达式一个量值，那么可以不写`{}`。要把object对象对应的string获取出来，可以使用Dart中每个对象的`toString()`方法。

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
    'Dart has string interpolation, ' +
        'which is very handy.');
assert('That deserves all caps. ' +
        '${s.toUpperCase()} is very handy!' ==
    'That deserves all caps. ' +
        'STRING INTERPOLATION is very handy!');
```

> == 操作符用于测试两个对象是否相等。当两个字符串拥有相同的码值序列，那么则相等。

###### 连接字符串

可以通过 + 连接字符串，或者只要让字符串相邻，也可以连接字符串。

```dart
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
    'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

###### 多行字符串

使用三连引号可以定义多行字符串。

```dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

###### 原生字符串

```dart
var s = r"In a raw string, even \n isn't special.";
```

字符串中的码值[Rune](https://www.dartlang.org/guides/language/language-tour#runes)

字符串是编译时常量。

```dart
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = const [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```



#### 布尔类型

bool 类型传统，仍然只有两个值可以取，true和false。这两个值都是编译时常量。

Dart类型安全意味着if和assert中的条件必须是bool类型结果。

比如：

```dart
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```

#### 列表类型

每个编程语言用到的最多的想必就是列表类型，用于替代数组类型，或者用有序列表。在Dart中，数组就是List对象，所以很多人称它为列表对象。

```dart
var list = [1, 2, 3];
```

> 注意：泛类型引用推断：当分析器推断一个list对象为List<int>，如果你尝试在list中存放非int类型变量，会在运行时报错。参考：[类型引用](https://www.dartlang.org/guides/language/sound-dart#type-inference)

列表仍然使用0基索引，`0`为首项索引并且`list.length -1`是尾项索引。

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

要创建一个编译时常量，在List字面量前加`const`即可：

```dart
var constantList = const [1, 2, 3];
// constantList[1] = 1; // 此行解除注释会报错
```

更多参考见：[泛型](https://www.dartlang.org/guides/language/language-tour#generics)和[集合](https://www.dartlang.org/guides/libraries/library-tour#collections)。

*注:像不像Python?*



#### 映射类型

Map类型

多数情况下，map对象类型关联了key和value键值对，键值都是对象类型。键有唯一性，不可重复。值无此限制。Dart支持映射字面量语法，类似JSON。

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

> 注意: 分析器推断gifts是`Map<String,String>`类型，nobleGases是`Map<int, String>`.  
> 尝试存放不同的类型，会报错。

你可以创建一个相同的对象来使用Map的构造函数

```dart
var gifts = new Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = new Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

添加新键值对:

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // Add a key-value pair
```

读取特定键值:

```dart
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```

如果要读取的key不在map中，返回null

```dart
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```

使用.length读取键值长度:

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```

要创建一个编译时常量值映射，在map字面值前加上const:

```dart
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // Uncommenting this causes an error.
```



#### Rune码点类型

在Dart中，一个Rune是UTF-32字符码点。在字符串中，要表示32位Unicode值需要特定语法。

表示Unicode代码点的常用方法是\ uXXXX，其中XXXX是一个4位十六进制值。例如，心形字符（♥）是`\ u2665`。要指定多于或少于4个十六进制数字，请将该值放在大括号中。例如，笑的表情符号（😆）是`\ u {1f600}`。 

字符串中有多个属性可以解码rune信息，`codeUnitAt` 和 `codeUnit`  属性返回16位码值，使用runes属性获取字符串的所有编码值。

```dart
main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}
```

执行结果:

```
👏
[55357, 56399]
[128079]
♥  😅  😎  👻  🖖  👍
```



#### 符号

在Dart中，符号可以代表操作符和标识符。你可能从来不需要使用符号，但是对于按名称引用标识符的API非常有用。

获取标识符的符号，可以用符号字面量，有#然后跟上标识符:

```dart
#radix
#bar
```

符号是编译期常量。



### 函数



#### 可选参数



#### main 函数



#### 函数作为第一等对象



#### 匿名函数



####  词汇性范围



#### 词汇性闭包



####测试函数的等同性



#### 返回值



### 操作符

####　数学操作符



####  逻辑操作符



#### 赋值操作符



#### 位操作符



#### 条件操作符



#### 级联符号



####  其它操作符



### 控制流

#### if 和 else



#### for 循环



#### while和do while



#### break 和 continue



#### switch 和 case



#### assert 断言



### 异常



#### throw



#### catch 



#### finally



### 类 



####  实例变量



#### 构造函数



#### 方法



#### 抽象类



#### 隐式接口



#### 继承扩展一个类



#### 枚举类型



#### 为类添加特性mixin



#### 类变量和方法



### 泛型

#### 为何用泛型?



#### 集合关键字



#### 使用带构造函数的参数化类型



### 库和可见性

#### 使用库



#### 实现库



### 异步支持

#### 处理Future



#### 声明异步函数



#### 处理Stream



### Generator

####可调用类



#### 独立化



#### 定义类



#### 元数据支持



### 注释



#### 单行注释



#### 多行注释



#### 文档注释













