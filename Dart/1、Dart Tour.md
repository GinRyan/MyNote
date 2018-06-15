## 1、Dart 2 一览

### 前言

Flutter是一个Google官方维护的移动开发框架，拥有很多现代框架所拥有的一些有意思的特性，比如hot reload，60fps高性能刷新律的完全self-drawing widgets，现在越来越多的进入开发者的视野，所以，我特意了解一下Flutter开发所用的Dart 语言，现在Dart出了2版本，或许跟第1版有多少不同，但应该不会像Python3和Python2那样差异巨大。

我们按照官网看一下Dart大体的语言语法和库的用法，本文为官网Tour的笔记和部分翻译，会与原文有大量出入。

原文链接: [Dart 2 Tour](https://www.dartlang.org/guides/language/language-tour)



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

const关键字不仅用于声明常量变量。您也可以使用它来创建常量值，以及声明创建常量值的构造器。任何变量都可以有一个常量值。 

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

由于Dart处处是对象，所以可以使用*构造器*来初始化变量。有的内建类型有自己的构造器，例如使用Map()构造器可以创建一个映射，可以这么用`new Map()`



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

你可以创建一个相同的对象来使用Map的构造器

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

Dart 是一个完全面向对象语言，所以即便是函数，也是对象，并且拥有类型，Function类型。这意味着，函数可以被指派为一个变量并且可以作为其它函数参数进行传递。你也可以调用一个Dart类的实例，就像它是一个函数一样。更多详见：[可调用类](https://www.dartlang.org/guides/language/language-tour#callable-classes)。

实现一个函数的例子: 

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

尽管《Effective Dart》建议对[公开的API使用类型修饰符](https://www.dartlang.org/guides/language/effective-dart/design#prefer-type-annotating-public-fields-and-top-level-variables-if-the-type-isnt-obvious)，然而你假使省略了返回类型函数依然有效。

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

对于只有一行代码的函数，可以使用速记语法:

```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

速记语法`=>表达式`是对`{return 表达式;}`的缩写，`=>`符号有时被称作`大箭头`语法。

> 注意: =>表达式到;之间，只能是表达式，不可以是声明。比如你不能使用if，但是可以用条件表达式。

#### 可选参数

可选参数可以是位置指定或名称指定的，但不能同时包含两者。 

###### 可选命名参数

当调用一个函数，你可以指定命名参数使用`参数名:值`的形式。

```dart
enableFlags(bold: true, hidden: false);
```

而当我们定义这么一个函数时，使用`{param1,param2,...}`来指定命名参数。

```dart
/// 设置 [bold] 和 [hidden] 命名参数标识 ...
void enableFlags({bool bold, bool hidden}) {
  // ...
}
```



###### 可选位置参数

在`[]`中包装一组函数参数将它们标记为可选的位置参数： 

```dart
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

不加可选参数调用这个函数：

```dart
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```

附加可选参数调用这个函数: 

```dart
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

如果是多个可选参数，可以这么包装: 

```dart
[int one,int two,...]
```

###### 默认参数值

参数可以定义默认参数值，对名称和位置参数均可用。默认值必须是编译时常量。

如果没有提供默认值，默认值为`null`。

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold = false, bool hidden = false}) {
  // ...
}

// bold will be true; hidden will be false.
enableFlags(bold: true);
```

下个例子是给位置参数设置默认值:

```dart
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}

assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
```

复杂一点的，你可以传递有默认值的List或者Map参数。下面的例子定义了一个函数`doStuff()`，指定了一个默认列表参数作为list参数，和一个默认的映射作为gifts参数。

```dart
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
    
  print('list:  $list');
  print('gifts: $gifts');
}
```



#### main 函数

每个应用都要有顶级的main()函数，用于提供应用程序入口。二`main()`函数返回`void`，并且有一个可选的`List<String>`参数作为入参数。

这是个简单的WebApp示例:

```dart
void main() {
  querySelector('#sample_text_id')
    ..text = 'Click me!'
    ..onClick.listen(reverseText);
}
```

> 注意: `..`语法称作[级联](https://www.dartlang.org/guides/language/language-tour#cascade-notation-)。通过级联，可以对单个对象的成员执行多个操作。 

对于一个读取参数的控制台应用这里有一个范例:

```dart
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

可以使用[args参数库](https://pub.dartlang.org/packages/args)，来定义和解析控制台参数。

#### 函数作为第一等对象

你可以传递一个函数作为另一个函数的参数。例如:

```dart
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// 会把printElement函数作为参数传进去
list.forEach(printElement);
```

>  补充说明: 不了解foreach和函数对象的人会对上面这段代码的执行一头雾水。由于这行代码省略了lambda式参数写法，省略了代码块花括号和lambda参数表，只保留了方法引用，隐藏了大量信息。
>
>  上面这段代码的执行会输出
>
>  1
>  2
>  3
>
>  由于foreach的参数是一个函数，但是这个函数也是有要求的，它只能有一个参数，在foreach遍历list元素时，会将遍历时遇到的元素逐个执行传入printElement()函数中，成为printElement()执行的参数。因此遍历时会多次执行被传入的函数printElement。详细了解请见下节匿名函数，以及函数式编程。



#### 匿名函数

大多数函数是命名的，比如main()或printElement()。可以创建无名称函数，称之为*匿名函数*，或者有时被称为*lambda*或者*闭包*。你可以为匿名函数指派到一个变量，比如你可以在一个集合中添加或者移除掉。

匿名函数和命名函数很相似，只是没有函数名。

函数体

```dart
([[Type] param1[, …]]) {
  codeBlock;
}; 
```

示例:

```dart
void main() {
  var list = ['apples', 'bananas', 'oranges'];
  list.forEach((item) {
    print('${list.indexOf(item)}: $item');
  });
}
```

输出:

```
0: apples
1: bananas
2: oranges
```

如果该函数只包含一条语句，则可以使用大箭头符号将其缩短。

```dart
list.forEach(
    (item) => print('${list.indexOf(item)}: $item'));
```



####  范围作用域

Dart可以通过紧邻的外层花括号范围来确定变量作用范围。最显著的用处是内嵌函数。

```dart
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```



#### 闭包作用域

```dart
/// Returns a function that adds [addBy] to the
/// function's argument.
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

void main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```





#### 返回值

所有函数都有返回值，没有返回值指定的都返回null。没有显式指定的都会被隐式加入。

```dart
foo() {}

assert(foo() == null);
```



### 操作符

Dart定义了很多操作符，如下表。可以重载大多数操作符。见[可重载操作符。](https://www.dartlang.org/guides/language/language-tour#overridable-operators)



| 描述        | 操作符      |
| ------------------ | ------------------------------------------------------------ |
| unary postfix      | `*expr*++`    `*expr*--`    `()`    `[]`    `.`    `?.`      |
| unary prefix       | `-*expr*`    `!*expr*`    `~*expr*`    `++*expr*`    `--*expr*` |
| multiplicative     | `*`    `/`    `%`    `~/`                                    |
| additive           | `+`    `-`                                                   |
| shift              | `<<`    `>>`                                                 |
| 位运算AND          | `&`                                                          |
| 位运算XOR          | `^`                                                          |
| 位运算OR           | `|`                                                          |
| 关系运算和类型判断 | `>=`    `>`    `<=`    `<`    `as`    `is`    `is!`          |
| 判等               | `==`    `!=`                                                 |
| 逻辑AND            | `&&`                                                         |
| logical OR         | `||`                                                         |
| if null            | `??`                                                         |
| 条件               | `*expr1* ? *expr2* : *expr3*`                                |
| 级联               | `..`                                                         |
| 赋值               | `=`    `*=`    `/=`    `~/=`    `%=`    `+=`    `-=`    `<<=`    `>>=`    `&=`    `^=`    `|=`    `??=` |

>  警告: 对于在两个操作数上工作的操作符，最左边的操作数决定使用哪个版本的操作符。例如，如果您有一个Vector对象和一个Point对象，则aVector + aPoint将使用Vector的Vector版本。 

####　数学操作符

Dart支持通用的算术运算符，如下表所示。 

| 操作符    | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| `+`       | 加法                                                         |
| `–`       | 减法                                                         |
| `-*expr*` | Unary minus, also known as negation (reverse the sign of the expression) |
| `*`       | Multiply                                                     |
| `/`       | Divide                                                       |
| `~/`      | 地板除法, 返回整形结果                                       |
| `%`       | 取余运算(取模)                                               |

#### 类似判定符

运行时类型操作符：`as, is和 is!` 用于运行时检查类型

| 操作符 | 说明                        |
| ------ | --------------------------- |
| `as`   | 类型转换                    |
| `is`   | 如果对象有指定类型返回true  |
| `is!`  | 如果对象有指定类型返回false |

```dart
if (emp is Person) {
  // Type check
  emp.firstName = 'Bob';
}
```

as类型转换:

```dart
(emp as Person).firstName = 'Bob';
```

>  注: emp如果不是Person 类型或者是null，会抛出异常。



#### 赋值操作符

除了类似C和Java的操作符外，增加了`??=`运算符。`??=`仅在赋值变量为空时分配值。



####  逻辑操作符

同C系语法。

#### 位操作符

同C系语法。

#### 条件操作符

同C系语法。

#### 级联符号

级联符号可以允许在同一个对象上，多次调用操作。

例如: 

```dart
querySelector('#confirm') // Get an object.
  ..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```

其等同于:

```dart
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
```

另外可以嵌套级联:

```dart
final addressBook = (new AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (new PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```

要小心级联你的对象，使每次调用都有返回的有效对象，否则会执行失败。

> 注意: 严格的讲，双点号不是操作符，是Dart语法一部分。

> 补充说明: 在我来看，这是个没用的特性。不如用use{} 或者with{}类似的特性代替来的实用。



####  其它操作符

| 操作符 | 名称                      | 说明                                                         |
| ------ | ------------------------- | ------------------------------------------------------------ |
| `()`   | Function application      | Represents a function call                                   |
| `[]`   | List access               | Refers to the value at the specified index in the list       |
| `.`    | Member access             | Refers to a property of an expression; example: `foo.bar` selects property `bar` from expression `foo` |
| `?.`   | Conditional member access | 对象不为null时访问对象成员                                   |

更多关于`.`, `?.`, `..`操作符，见[类](https://www.dartlang.org/guides/language/language-tour#classes)。



### 控制流

#### if 和 else

同C系语言。

#### for 循环

同C系语言。

#### while和do while

同C系语言。

#### break 和 continue

同C系语言。

#### switch 和 case

switch case的条件可以是String类型。然而，不支持省略break语法和下落。

> 补充说明：那还要switch干什么！

#### assert 断言

同Java。

### 异常

#### throw

同Java。

不过throw允许直接抛出字符串说明

#### catch 

可以指定on后面的异常类型，也可以不指定。

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```

#### finally



```dart
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
```

### 类 

Dart 是一个包含基于mixin混入集成特性的面向对象语言。

每个对象都是类的实例，所有的类都继承自Object. Mixin 意味着每个类都只有一个父类，一个类体可以通过多种类关系树，重用。(类似java 的接口机制)

要创建对象，你可以使用`new`关键字配合一个类的*构造器*。比如：

```dart
// Create a Point using Point().
var p1 = new Point(2, 2);

// Create a Point using Point.fromJson().
var p2 = new Point.fromJson({'x': 1, 'y': 2});
```

> Dart 2开始，new可以省略

使用点操作符(`.`)来引用实例中的变量或者方法。

```dart
var p = new Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(new Point(4, 4));
```

使用`?.`而不是`.`可以规避左对象因为空指针导致的null指针异常。

有的类提供常量构造器。可以使用const代替new来构造常构造器。

```dart
var p = const ImmutablePoint(2, 2);
```

> [隐式创建的非正式规范](https://github.com/dart-lang/sdk/blob/master/docs/language/informal/implicit-creation.md)

构建两个相同的编译时常量，会生成一个典型的单例：

```dart
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```

要获取对象的类型，你可以使用`runtimeType`  属性，它会返回一个`Type`对象。

```dart
print('The type of a is ${a.runtimeType}');
```

以下开始讨论如何实现一个类

####  实例变量

定义实体变量

```dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
```

未初始化实体变量值都是null。

所有实例变量都默认生成了隐式getter方法，非final的实例变量生成一个隐式setter。参考:[[Getters 和 setters](https://www.dartlang.org/guides/language/language-tour#getters-and-setters)]

```dart
class Point {
  num x;
  num y;
}

void main() {
  var point = new Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```



#### 构造器

```dart
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```

`this`关键字引用当前实例。

> `this`的用法与Java相同

为实例变量分配构造器参数的模式非常常见，Dart使用语法糖来简化它 :

```dart
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

##### 默认构造器

如果自己不声明构造器，会提供一个默认构造器。默认构造器无参数并且调用父类无参数构造器。

##### 构造器不继承

子类不继承父类构造器。声明没有构造器的子类只有默认的（无参数，无名称）构造器。

##### 命名构造器

使用命名构造器为一个类实现多个构造器或提供额外的清晰度:

 ```dart
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
 ```

构造器不能继承。*如果你想用一个在父类中定义的命名构造器来创建子类，那么你必须在子类中实现该构造器* 。

##### 调用一个非默认父类构造器

默认情况下，子类中的构造器调用父类的无名无参构造器。父类的构造器在构造器体的开头被调用。如果还使用了*初始化器列表*，则会在调用父类之前执行。

1. *初始化器列表*(见下一节)
2. 超类的无参数构造器
3. 主类的无参数构造器

 如果超类没有一个无名的无参构造器，那么你必须手动调用超类中的一个构造器。 在冒号（:)之后指定超类构造器，就在构造器体（如果有）之前。 

```dart
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person 没有构造器;
  //你必须调用 super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // 会打印:
  // in Person
  // in Employee
  if (emp is Person) {
    // 做类型检查
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
```

执行结果:

```dart
in Person
in Employee
```

> 警告：超类构造器不能使用this。可以调用静态方法，但是不能使用实例方法。

##### 初始化列表

除了调用超类构造器之外，还可以在**构造器体运行之前**初始化实例变量。用逗号分隔初始值，在冒号之后。 

```dart
// 初始化器列表会在构造器体执行之前
// 设置好实例变量
Point.fromJson(Map<String, num> json)
    : x = json['x'], y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```

> 警告: 初始化器不能使用this关键字。

> 补充说明：以上两个警告均说明构造器执行前以及初始化器列表执行完毕前类的实例没有准备好，this不可用于指向自身实例。

开发流程中，可以使用`assert`在初始化器列表中验证输入。

```dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

例如:

```dart
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
```

结果:

```
3.605551275463989
```

##### 重定向构造

有时候构造器目的只是重定向，那么构造器可以为空，在(`:`)后调用构造器即可。

```dart
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```

##### 常量构造器

如果你的类产生永不改变的对象，你可以使这些对象编译时常量。为此，定义一个const构造函数并确保所有实例变量都是final的。 

```dart
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
```

##### 工厂构造器

将构造器加上`factory`关键字修饰，便不会每次都产出一个新的实例，比如工厂构造器可以从一个缓存中返回实例或者返回一个子类型实例。

例如: 

```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = new Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
```

> 注意: 工厂构造器不能访问this

调用工厂构造器时，仍然使用`new`关键字。

```dart
var logger = new Logger('UI');
logger.log('Button clicked');
```



#### 方法

方法是提供在对象上的行为的函数。

##### 实例方法

对象上的实例方法可以访问实例变量和`this`。

```dart
import 'dart:math';

class Point {
  num x, y;

  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```



##### Getter和Setter

getter和setter方法可以对对象中的属性进行读写。每个实例变量都是隐式的getter和setter方法。也可以自己实现get和set。

```dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = new Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```



##### 抽象方法

用法同Java



##### 可重载操作符

可以重载表中的操作符

| `<`  | `+`  | `|`  | `[]`  |
| ---- | ---- | ---- | ----- |
| `>`  | `/`  | `^`  | `[]=` |
| `<=` | `~/` | `&`  | `~`   |
| `>=` | `*`  | `<<` | `==`  |
| `–`  | `%`  | `>>` |       |

如何重载`+`和`-`:

```dart
class Vector {
  final int x, y;

  const Vector(this.x, this.y);

  /// Overrides + (a + b).
  Vector operator +(Vector v) {
    return new Vector(x + v.x, y + v.y);
  }

  /// Overrides - (a - b).
  Vector operator -(Vector v) {
    return new Vector(x - v.x, y - v.y);
  }
}

void main() {
  final v = new Vector(2, 3);
  final w = new Vector(2, 2);

  // v == (2, 3)
  assert(v.x == 2 && v.y == 3);

  // v + w == (4, 5)
  assert((v + w).x == 4 && (v + w).y == 5);

  // v - w == (0, 1)
  assert((v - w).x == 0 && (v - w).y == 1);
}
```

如果要重载`==`那么你也要重载`hashCode`的getter方法。详见[实现map键](**https://www.dartlang.org/guides/libraries/library-tour#implementing-map-keys**)。

重载详见[继承类](https://www.dartlang.org/guides/language/language-tour#extending-a-class)。



#### 抽象类

同Java语法。



#### 隐式接口

每个类都隐含地定义了一个接口，该接口包含该类的所有实例成员及其实现的任何接口。如果你想创建一个支持类B的API而不继承B的实现的类A，那么类A应该实现B接口。 

> 补充说明:  这句话我真没看明白。看例子，貌似跟Java一样。

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(new Person('Kathy')));
  print(greetBob(new Impostor()));
}
```

实现多个接口：

```dart
class Point implements Comparable, Location {...}
```



#### 继承扩展一个类

使用`extends`创建一个子类，并且`super`关键字可以引用父类。

同Java语法。

```dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```

##### 重载成员

类似Java，重载的成员函数也需要有``@override`  `修饰:

```dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```

>  要在类型安全的代码中缩小方法参数或实例变量的类型， 你可以使用covariant关键字. 

##### noSuchMethod()

要检测或者响应一些尝试调用不存在的方法或者实例变量，你可以重载`noSuchMethod()`。

```dart
class A {
  // 除非你重载了noSuchMethod, 否则尝试调用一个不存在的方法会
  // 导致抛出 NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
```

详见[noSuchMethod说明](https://github.com/dart-lang/sdk/blob/master/docs/language/informal/nosuchmethod-forwarding.md)。



#### 枚举类型

> 不想看

#### 为类添加特性mixin

mixin是一种在多个类层级树中重用类代码的方式。

使用`with`关键字跟在一个或者多个mixin名称来使用mixin。

```dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

要实现mixin，创建一个类只继承Object，不声明构造函数，没有任何`super`调用。

```dart
abstract class Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

> 注意:  对于1.13，在DartVM上的mixin的两个限制已经被取消。
>
> - mixin 可以继承其它类
> - mixin 可以调用`super()`



#### 类变量和方法



### 泛型

#### 为何用泛型?



#### 集合关键字



#### 使用带构造器的参数化类型



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













