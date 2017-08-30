---
title: Google Java 代码风格指南(中文版)
date: 2017-03-23 17:40:46
tags: [代码风格]
---
**翻译中，目前进度75%**
[原文链接](https://google.github.io/styleguide/javaguide.html)

*P.S.* 翻译水平有限，希望大家能提供好的建议和指正。
### 目录
* [1 简介](#1-简介)
* * [1.1 术语注意事项](#1-1-术语注意事项)
* * [1.2 指南注意事项](#1-2-指南注意事项)
* [2 源码基础风格](#2-源码基础风格)
* * [2.1 文件名](#2-1-文件名)
* * [2.2 文件编码:UTF-8](#2-2-文件编码:UTF-8)
* * [2.3 特殊字符](#2-3-特殊字符)
* [3 源码文件结构](#3-源码文件结构)
* * [3.1 如果存在，请添加License或copyright信息](#3-1-如果存在，请添加License或copyright信息)
* * [3.2 package语句](#3-2-package语句)
* * [3.3 import语句](#3-3-import语句)
* * [3.4 class声明](#3-4-class声明)
* [4 格式化](#4-格式化)
* * [4.1 括号](#4-1-括号)
* * [4.2 代码块儿缩进:+2空格](#4-2-代码块儿缩进-2空格)
* * [4.3 每行一个语句](#4-3-每行一个语句)
* * [4.4 列宽:100](#4-4-列宽-100)
* * [4.5 换行](#4-5-换行)
* * [4.6 空白符](#4-6-空白符)
* * [4.7 分组括号](#4-7-分组括号)
* * [4.8 特殊结构](#4-8-特殊结构)
* [5 命名](#5-命名)
* * [5.1 通用规则](#5-1-通用规则)
* * [5.2 不同标识符的规则](#5-2-不同标识符的规则)
* * [5.3 驼峰命名](#5-3-驼峰命名)
* [6 编程规范](#6-编程规范)
* * [6.1 必须使用 @Override](#6-1-必须使用-Override)
* * [6.2 不能忽略已捕获的异常](#6-2-不能忽略已捕获的异常)
* * [6.3 使用类来访问静态成员](#6-3-使用类来访问静态成员)
* * [6.4 禁止使用 Finalizers](#6-4-禁止使用-Finalizers)
* [7 Javadoc](#7-Javadoc)
* * [7.1 格式化](#7-1-格式化)
* * [7.2 摘要](#7-2-摘要)
* * [7.3 在什么地方使用 Javadoc](#7-3-在什么地方使用-Javadoc)

## 1 简介
### 1.1 术语
本文档中除非特别说明，否则遵循以下规则：

1. *class* 表示“普通”类、枚举类、接口或注解类(`@interface`)
1. (类中的) *member* 一般表示内部类、成员、方法或构造器，也就是说包含顶级类中除了初始化块和注释全部内容。
1. *comment* 特指直接注释。我们不使用“文档注释”这个词，而是直接使用"Javadoc"。

其他局部注意事项将在本文中不时出现。

### 1.2 指南
所有示例代码都是 **不规范** 的。也就是说，尽管例子是使用的Google风格，但仅仅只是展示代码风格。

## 2 源码基础风格
### 2.1 文件名
所有源码文件的命名与顶级类保持一致（包括大小写），再加 `.java` 扩展名。

### 2.2 文件编码:UTF-8
所有源文件使用 **UTF-8** 编码。

### 2.3 特殊字符
#### 2.3.1 空白字符
Aside from the line terminator sequence, the **ASCII horizontal space character (0x20)** is the only whitespace character that appears anywhere in a source file. This implies that:

1. All other whitespace characters in string and character literals are escaped.
1. Tab characters are **not** used for indentation.

#### 2.3.2 特殊转义字符
对于能用字符表示的特殊转义符（`\b`、`\t`、`\n`、`\f`、`\r`、`\"`、`\'`、`\\`），不要使用8进制转义（比如：`\012`）或Unicode转义（比如：`\u000a`）。

#### 2.3.3 非ASCII字符
对于非ASCII字符，直接用Unicode字符（比如：`∞`）或用Unicode转义字符（比如：`\u221e`）都可以。具体采用哪种方式只取决于代码的可读性。

**提示:** 使用Unicode转义符或不常见的Unicode符号时，强烈建议添加注释予以说明。

**示例:**

| 示例 | 结论 |
| :---- | :---- |
| String unitAbbrev = "μs"; | 最佳：非常清晰，甚至不需要注释说明。 |
| String unitAbbrev = "\u03bcs"; // "μs" | 也可以：但没必要这么做。 |
| String unitAbbrev = "\u03bcs"; // Greek letter mu, "s" | 可接受：但不容易理解。 |
| String unitAbbrev = "\u03bcs"; | 不可接受：作者以外的人完全不知道这是啥？ |
| return '\ufeff' + content; // byte order mark | 正确：使用转义符表示不能显示的字符并添加了说明注释。 |

**提示:** 千万不要因为害怕别的程序不能识别非ASCII字符而牺牲代码的可读性。

## 3 源码文件结构
源码文件按以下 **顺序** 排序：

1. 许可信息和版权信息（如果有的话）
1. `package` 语句
1. `import` 语句
1. 顶级类

各个部分使用 **单行空行** 来分割。

### 3.1 如果存在，请添加License或copyright信息
如果这个文件拥有许可和版权信息，请在文件中添加。

### 3.2 package语句
`package` 语句 **不要换行** 。
列宽的限制([4.4 列宽:100](#4-4-列宽-100))不影响 `package` 语句。

### 3.3 import语句
#### 3.3.1 不要使用通配符import
不管是静态导入还是声明，**绝不使用通配符导入**。

#### 3.3.2 不要换行
`import` 语句 **不要换行** 。
列宽的限制([4.4 列宽:100](#4-4-列宽-100))不影响 `import` 语句。

#### 3.3.3 排序和空行
导入顺序如下：

1. 所有静态导入在一个块儿中。
1. 所有非静态导入在一个块儿中。

如果既存在静态导入，又有非静态导入，则在两个块儿间用一个空行进行分割。

在每个块儿中，按类名ASCII码序排序。（**注意** 这并不意味是按语句的ASCII码序排序，因为`.`的ASCII序在`;`的ASCII序前。）

#### 3.3.4 不要静态导入
不要使用静态导入来导入内部静态类。使用普通导入即可。

### 3.4 class声明
#### 3.4.1 完整的顶级类声明
每个顶级类都存唯一对应的一个源码文件。

#### 3.4.2 类的内容的排序
类中方法和构造器的顺序对于别人理解你的代码有巨大帮助。但是，这个顺序并没有一定之规，而是根据不同的类有不同的顺序。

重要的是作者能类中内容遵循的 **某种逻辑顺序** 一个合理解释。比如，把新添加的方法放在最后，虽然也能解释为按添加顺序排序，但这并不存在任何逻辑关系。

##### 3.4.2.1 重载方法不要分离
当一个类中包含多个构造器或多个同名方法时，把这些方法排列在一起（中间不穿插任何代码，甚至私有成员变量也不行）。

## 4 格式化
*块儿状结构* 指类、方法或构造器的方法体。 注意,在[4.8.3.1 数组可以像块一样初始化](#4-8-3-1-数组可以像块一样初始化)中，这些数组初始化块儿也被认为是块儿状结构。

### 4.1 括号
#### 4.1.1 一律使用大括号
`if`、`else`、`for`、`do`和`while`语句一律使用大括号，哪怕语句块儿只有一行或者为空。

#### 4.1.2 非空代码块使用K&R风格
括号非空代码块使用K&R风格：
* 在`{`前不换行。
* 在`{`后换行。
* 在`}`前换行。
* 在`}`后换行，当且仅当`}`后是控制符或结束符不换行。例如，`}`后是`else`或`;`。

**示例:**
```Java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```
还有一些例外在[4.8.1 枚举类](#4-8-1-枚举类)]中详尽描述。

#### 4.1.3 空代码块可以很简洁
空代码块指`{}`构成的块儿状结构，中间没有任何空格和空行。空代码块儿是允许的，但不允许在多块儿语句（`if`/`else`或`try`/`catch`/`finally`）中使用。

**示例:**
```Java
  // 这种写法没问题
  void doNothing() {}

  // 这种写法也没问题
  void doNothingElse() {
  }
```

```Java
  // 这种写法不允许: 不允许在多块儿语句中使用空代码块儿。
  try {
    doSomething();
  } catch (Exception e) {}
```

### 4.2 代码块儿缩进:+2空格
每当开始新代码块，需要缩进两个空格。当代码块儿结束后，恢复缩进。

### 4.3 每行一个语句
每行只写一条语句。

### 4.4 列宽:100
除了下面罗列的几种情况，任何Java代码的列宽均设置为100个字符。

几个例外：

1. 行宽超过列宽（比如：一个特别长的URL或一个特别长的JSNI方法引用）。
1. `package`和`import`语句（参考[3.2 package语句](#3-2-package语句)和[3.3 import语句](#3-3-import语句)）。
1. 注释中可能被复制粘贴到终端的命令行。

### 4.5 换行
名词解释：当可能合法占用单行的代码被划分成多行时，这个活动被称为换行。

尽管没有一个全面的，确定性的规则规定在各种情况下该如何换行，但通常有几种有效的方式来换行。

注意：尽管换行的典型原因是为了避免溢出列限制，即使实际符合列限制的代码也可能由作者自行决定。

提示：大部分情况下，可以通过提取方法或局部变量来解决问题，而不是换行。

#### 4.5.1 在哪里换行？
**TODO**
The prime directive of line-wrapping is: prefer to break at a **higher syntactic level**. Also:

**TODO**
1. When a line is broken at a non-assignment operator the break comes before the symbol. (Note that this is not the same practice used in Google style for other languages, such as C++ and JavaScript.)
 * This also applies to the following "operator-like" symbols:
    * the dot separator (.)
    * the two colons of a method reference (::)
    * an ampersand in a type bound (<T extends Foo & Bar>)
    * a pipe in a catch block (catch (FooException | BarException e)).
1. When a line is broken at an assignment operator the break typically comes after the symbol, but either way is acceptable.
 * This also applies to the "assignment-operator-like" colon in an enhanced for ("foreach") statement.
1. A method or constructor name stays attached to the open parenthesis (() that follows it.
1. A comma (,) stays attached to the token that precedes it.
1. A line is never broken adjacent to the arrow in a lambda, except that a break may come immediately after the arrow if the body of the lambda consists of a single unbraced expression. Examples:
```Java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```
**注意** 换行最主要的目的是保持代码的清晰，而不是占用最少的行。

#### 4.5.2 缩进连续行:+4空格
当折行时，新的行要比原始行缩进4个空格。

当有多个连续行时，保持后续行相同的缩进。

### 4.6 空白符
#### 4.6.1 垂直空白
空行会在以下几种情况下使用：

1. 类中不同类型的成员和构造器之间: 属性、构造器、方法、内部类、静态初始化块和实例初始化.
 * **例外** 属性之间的空行可以省去，一般在属性之间的空行用来分组。
 * **例外** 枚举常量的空行使用在[4.8.1 枚举类](#4-8-1-枚举类)中讨论。
1. 语句之间用以标识逻辑上的分割。
1. 在第一个成员或初始化块前或最后一个成员或初始化块后也可以增加空行。
1. 在其他章节提到的要求（比如[3 源码文件结构](#3-源码文件结构)和[3.3 import语句](#3-3-import语句)）。

不推荐使用连续多行的空行（但也不禁止）。

#### 4.6.2 水平空白
**TODO**
Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places only.

**TODO**
1. Separating any reserved word, such as else or catch, from a closing curly brace (}) that precedes it on that line
1. Separating any reserved word, such as if, for or catch, from an open parenthesis (() that follows it on that line
1. Before any open curly brace (`{`), with two exceptions:
  * `@SomeAnnotation({a, b})` (no space is used)
  * `String[][] x = {{"foo"}};` (no space is required between `{`, by item 8 below)
1. On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:
  * the ampersand in a conjunctive type bound: `<T extends Foo & Bar>`
  * the pipe for a catch block that handles multiple exceptions: `catch (FooException | BarException e)`
  * the colon (:) in an enhanced for ("foreach") statement
  * the arrow in a lambda expression: `(String str) -> str.length()`

  but not
  * the two colons (`::`) of a method reference, which is written like `Object::toString`
  * the dot separator (.), which is written like object.toString()
1. After `,:;` or the closing parenthesis ()) of a cast
1. On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.
1. Between the type and variable of a declaration: `List<String> list`
1. Optional just inside both braces of an array initializer
  * `new int[] {5, 6}` and `new int[] { 5, 6 }` are both valid

This rule is never interpreted as requiring or forbidding additional space at the start or end of a line; it addresses only interior space.

#### 4.6.3 不要求水平对齐
**TODO**
This practice is permitted, but is never required by Google Style. It is not even required to maintain horizontal alignment in places where it was already used.

**TODO**
Here is an example without alignment, then using alignment:
```Java
private int x; // this is fine
private Color color; // this too

private int   x;      // permitted, but future edits
private Color color;  // may leave it unaligned
```
**TODO**
Tip: Alignment can aid readability, but it creates problems for future maintenance. Consider a future change that needs to touch just one line. This change may leave the formerly-pleasing formatting mangled, and that is allowed. More often it prompts the coder (perhaps you) to adjust whitespace on nearby lines as well, possibly triggering a cascading series of reformattings. That one-line change now has a "blast radius." This can at worst result in pointless busywork, but at best it still corrupts version history information, slows down reviewers and exacerbates merge conflicts.


### 4.7 分组括号
**TODO**
Optional grouping parentheses are omitted only when author and reviewer agree that there is no reasonable chance the code will be misinterpreted without them, nor would they have made the code easier to read. It is not reasonable to assume that every reader has the entire Java operator precedence table memorized.

### 4.8 特殊结构
#### 4.8.1 枚举类
在每个逗号后可以直接跟枚举，也可以添加换行。例如：
```Java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```
如果枚举里不包含任何方法和注释时，可以使用[4.8.3.1 数组可以像块一样初始化](#4-8-3-1-数组可以像块一样初始化)]的风格。
```Java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```
既然枚举类也是类，所有其他格式化风格遵循类的风格。

#### 4.8.2 变量声明
##### 4.8.2.1 一次声明一个变量
每次声明只一个变量。比如，`int a, b;`就不允许。

##### 4.8.2.2 需要时才声明
局部变量不要在代码块的开头定义，而是在马上要用到的地方声明。

#### 4.8.3 数组
##### 4.8.3.1 数组可以像块一样初始化
任何数组的初始化都可以像块儿结构一样。比如：
```Java
new int[] {
  0, 1, 2, 3
};
new int[] {
  0,
  1,
  2,
  3,
};
new int[] {
  0, 1,
  2, 3
};
new int[]
    {0, 1, 2, 3};
```
##### 4.8.3.2 不使用C风格的数组声明方式
方括号跟在类型后，而不是参数。
```Java
String[] args; // 这是正确的
String args[]; // 这是错误的
```

#### 4.8.4 switch语句
##### 4.8.4.1 缩进
任何代码块儿的开始，都需要缩进两个空格。

在每个`case`或`default`语句后都需换行，其中的语句也像语句块儿一样要缩进两个空格。

##### 4.8.4.2 进入下一条件注释
在`switch`语句块中，每种`case`都应有明确的中断（`break`、`continue`、`return`或`throw`）或用注释说明将会进入下一条件。比如：
```Java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // 继续进入下一条件
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```
注意：像`case 1:`后面就没有注释。注释只需要添加在语句之后。

##### 4.8.4.3 必须提供default条件
每个`switch`语句都应包含`default`条件，哪怕`default`中不包含任何代码。

例外：枚举类的`switch`语句的`default`条件就不是必须的。

#### 4.8.5 注解
在类、方法、构造器上的注解紧跟在文档块儿之后，并且每个注解占据一行。比如：
```Java
@Override
@Nullable
public String getNameIfPresent() { ... }
```
例外：当只有一个注解时，可以和声明写在同一行，比如：
```Java
@Override public int hashCode() { ... }
```
在成员上的注解也紧跟在文档块儿之后，但当有多个注解时，可以写在同一行。比如：
```Java
@Partial @Mock DataLoader loader;
```
关于在参数和临时变量上的注解则没有特别的规则。

#### 4.8.6 注释
本章节只讨论注释。Javadoc在[7 Javadoc](#7-Javadoc)中讨论。

**TODO**
Any line break may be preceded by arbitrary whitespace followed by an implementation comment. Such a comment renders the line non-blank.

##### 4.8.6.1 块注释风格
块儿注释的缩进与上下文的代码保持同级。可以使用`/* ... */`或`// ...`风格。对于多行的`/* ... */`风格注释，每行用`*`开头并与上一行的`*`对齐。
```Java
/*
 * 这样
 * 可以。
 */

 // 也可以
 // 这样。

 /* 甚至你可以
  * 这样。 */
 ```
 **TODO**
Comments are not enclosed in boxes drawn with asterisks or other characters.

#### 4.8.7 修饰符
当有类和方法的修饰符时，使用Java语言推荐的顺序：

`public protected private abstract default static final transient volatile synchronized native strictfp`

#### 4.8.8 易混淆的数字和字母
`long`类型数值使用大写`L`作为结束符而不是小写`l`（避免小写字母`l`和数字`1`混淆）。比如，`3000000000L`就比`3000000000l`要清楚的多。

## 5 命名
### 5.1 通用规则
命名只包含字母、数字和`_`。

### 5.2 不同标识符的规则
#### 5.2.1 包命名
包名称全部使用小写并且不包含`_`。例如，`com.example.deepspace`是推荐的命名，而不是`com.example.deepSpace`或`com.example.deep_space`。

#### 5.2.2 类命名
类命名采用首字母大写的驼峰命名规范。

类名称一般用名词或名词短语命名。例如`Character`或`ImmutableList`。接口也用名词或名词短语命名（比如：`List`），但有时也用形容词或形容词短语（比如：`Readable`）。

注解类没有特别的命名规范。

测试类使用被测试的类加`Test`后缀来命名，比如：`HashTest`或`HashIntegrationTest`。

#### 5.2.3 方法命名
方法使用首字母小写的驼峰命名规范。

方法一般用动词或动词短语来命名。比如：`sendMessage`或`stop`。

`_`一般在测试方法中使用，最典型的用法是`test<MethodUnderTest>_<state>`，比如：`testPop_emptyStack`。

#### 5.2.4 常量命名
常量命名使用全大写并用`_`连接单词。但什么才是常量？

常量是`static final`的成员，也就是不可改变的成员。包括基本类型、`String`、不可变类型、使用不可变类型为泛型的不可变容器。例如：
```Java
// 这些是常量
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// 这些不时常量
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```
命名全部使用名词或名词短语。

#### 5.2.5 非常量成员命名
非静态变量（不管是否是`static`）全部采用首字母小写的驼峰命名规范。

命名全部使用名词或名词短语。比如`computedValues`或`index`。

#### 5.2.6 参数命名
参数全部采用首字母小写的驼峰命名规范。

在`public`方法中避免使用单字符的命名。

#### 5.2.7 局部变量命名
局部变量全部采用首字母小写的驼峰命名规范。

哪怕局部变量是`final`和`immutable`也不使用常量风格命名。

#### 5.2.8 类型变量命名
类型变量的命名遵循以下两种风格：

* 单个大写字母或单个大写字母后加数字。（比如：E、T、X、T2）
* 在类名后添加大写字母T。（比如：RequestT、FooBarT）

### 5.3 驼峰命名
**TODO**
Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like "IPv6" or "iOS" are present. To improve predictability, Google Style specifies the following (nearly) deterministic scheme.

**TODO**
Beginning with the prose form of the name:

**TODO**
Convert the phrase to plain ASCII and remove any apostrophes. For example, "Müller's algorithm" might become "Muellers algorithm".
Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
Recommended: if any word already has a conventional camel-case appearance in common usage, split this into its constituent parts (e.g., "AdWords" becomes "ad words"). Note that a word such as "iOS" is not really in camel case per se; it defies any convention, so this recommendation does not apply.
Now lowercase everything (including acronyms), then uppercase only the first character of:
... each word, to yield upper camel case, or
... each word except the first, to yield lower camel case
Finally, join all the words into a single identifier.
Note that the casing of the original words is almost entirely disregarded. Examples:

| Prose form | Correct | Incorrect |
| :---- | :---- | :---- |
| "XML HTTP request" | XmlHttpRequest | XMLHTTPRequest |
| "new customer ID" | newCustomerId | newCustomerID |
| "inner stopwatch" | innerStopwatch | innerStopWatch |
| "supports IPv6 on iOS?" | supportsIpv6OnIos | supportsIPv6OnIOS |
| "YouTube importer" | YouTubeImporter<br>YoutubeImporter* | |

**TODO**
`*`Acceptable, but not recommended.

**TODO**
Note: Some words are ambiguously hyphenated in the English language: for example "nonempty" and "non-empty" are both correct, so the method names checkNonempty and checkNonEmpty are likewise both correct.

## 6 编程规范
### 6.1 必须使用 @Override
当方法可以被标记`@Override`时，必须标记，包括重写父类方法、实现接口方法和在接口中重新指定父接口的方法。

**例外：** 当父类方法或接口方法被标记为`@Deprecated`时，可以不标记`@Override`。

### 6.2 不能忽略已捕获的异常
除了下面描述的情况，在`catch`块儿中不做处理都是错误的。（最典型的处理是用日志记录；或当`catch`被认为 *不可能* 执行到时，重新抛出`AssertionError`）

当真的不需要在`catch`块儿中做任何处理时，请通过注释来说明原因。
```Java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // 这不是一个数字; 这是正常情况, 继续进行即可。
}
return handleTextResponse(response);
```

### 6.3 使用类来访问静态成员
如果需要引用静态类成员，请使用类名，而不是该类的对象引用或该类型的表达式。
```Java
Foo aFoo = ...;
Foo.aStaticMethod(); // 推荐方式
aFoo.aStaticMethod(); // 不推荐
somethingThatYieldsAFoo().aStaticMethod(); // 绝不推荐
```

### 6.4 禁止使用 Finalizers
只有在极其特殊的情况下才需要重写`Object.finalize()`方法。

提示：千万别这么做。如果你真要这么做，请先仔细地阅读《Effective Java》第七章`Avoid Finalizers`,然后放弃该做法。

## 7 Javadoc
### 7.1 格式化
#### 7.1.1 通用格式
Javadoc的基本格式如下：
```Java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```
或者像下面这种单行的格式:
```Java
/** An especially short bit of Javadoc. */
```

#### 7.1.2 段落
**TODO**
One blank line—that is, a line containing only the aligned leading asterisk (`*`)—appears between paragraphs, and before the group of block tags if present. Each paragraph but the first has <p> immediately before the first word, with no space after.

#### 7.1.3 标签块
**TODO**
Any of the standard "block tags" that are used appear in the order @param, @return, @throws, @deprecated, and these four types never appear with an empty description. When a block tag doesn't fit on a single line, continuation lines are indented four (or more) spaces from the position of the @.

### 7.2 摘要
**TODO**
Each Javadoc block begins with a brief summary fragment. This fragment is very important: it is the only part of the text that appears in certain contexts such as class and method indexes.

**TODO**
This is a fragment—a noun phrase or verb phrase, not a complete sentence. It does not begin with A {@code Foo} is a..., or This method returns..., nor does it form a complete imperative sentence like Save the record.. However, the fragment is capitalized and punctuated as if it were a complete sentence.

**TODO**
Tip: A common mistake is to write simple Javadoc in the form `/** @return the customer ID */`. This is incorrect, and should be changed to `/** Returns the customer ID. */`.

### 7.3 在什么地方使用 Javadoc
**TODO**
At the minimum, Javadoc is present for every public class, and every public or protected member of such a class, with a few exceptions noted below.

**TODO**
Additional Javadoc content may also be present, as explained in Section 7.3.4, Non-required Javadoc.

#### 7.3.1 不包括不言自明的方法
**TODO**
Javadoc is optional for "simple, obvious" methods like getFoo, in cases where there really and truly is nothing else worthwhile to say but "Returns the foo".

**TODO**
Important: it is not appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a method named getCanonicalName, don't omit its documentation (with the rationale that it would say only `/** Returns the canonical name. */`) if a typical reader may have no idea what the term "canonical name" means!

#### 7.3.2 不包括重写方法
**TODO**
Javadoc is not always present on a method that overrides a supertype method.

#### 7.3.3 不需要的 Javadoc
**TODO**
Other classes and members have Javadoc as needed or desired.

**TODO**
Whenever an implementation comment would be used to define the overall purpose or behavior of a class or member, that comment is written as Javadoc instead (using `/**`).

**TODO**
Non-required Javadoc is not strictly required to follow the formatting rules of Sections 7.1.2, 7.1.3, and 7.2, though it is of course recommended.
