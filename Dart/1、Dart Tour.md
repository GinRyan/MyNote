## 1、Dart 2 一览

### 前言

Flutter是一个Google官方维护的移动开发框架，拥有很多现代框架所拥有的一些有意思的特性，比如hot reload，60fps高性能刷新律的完全self-drawing widgets，现在越来越多的进入开发者的视野，所以，我特意了解一下Flutter开发所用的Dart 语言，现在Dart出了2版本，或许跟第1版有多少不同，但应该不会像Python3和Python2那样差异巨大。

我们按照官网看一下Dart大体的语言语法和库的用法。



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



#### 默认值



#### final和const值



### 内建类型



#### 数字类型



#### 字符串类型



#### 布尔类型



#### 列表类型



#### 映射类型



#### Rune码点类型



#### 符号



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













