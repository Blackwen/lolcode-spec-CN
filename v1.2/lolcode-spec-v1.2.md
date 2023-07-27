# LOLCODE 中文规范 1.2

---
## 前言
翻译:Github@Blackwen

博客:https://www.lilkon.cn/

`
大部分使用 Google 翻译，调整部分语序，减少阅读障碍（其实就是机翻）
`
---
* 该规范的目标是作为所有后续 LOLCODE 规范的基准。 因此，一些传统上预期的语言功能可能会显得“不完整”。 这很可能是故意的，因为添加到语言中比更改和引入进一步的不兼容性更容易。*

---

## 格式

### 空白

* 空格用于在语言中划分标记，尽管某些关键字结构可能包含空格。

* 多个空格和制表符被视为单个空格，否则是不相关的。

* 缩进并不重要。

* 命令从行的开头开始，换行符表示命令的结束，特殊情况除外。

* 换行符将是回车符 (/13)、换行符 (/10) 或两者 (/13/10)，具体取决于实现的系统。这只涉及 LOLCODE 代码本身，并不表明在执行期间应如何在字符串或文件中处理这些代码。

* 多个命令可以放在一行中，如果它们之间用逗号 (,) 分隔。在这种情况下，逗号充当虚拟换行符或软命令中断。

* 通过在行末尾包含三个句点 (...) 或 unicode 省略号字符 (u2026)，可以将多行组合成单个命令。这会导致下一行的内容被评估，就像它在同一行上一样。

* 带有续行的行可以串在一起，许多行连续，以允许单个命令延伸多于一行或两行。只要每行以三个句点结束，就包含下一行，直到到达没有三个句点的行，此时整个命令就可以被处理。

* 具有续行符的行后面不能跟空行。三个句点本身可以​​在一行上，在这种情况下，空行“包含”在命令中（不执行任何操作），并且也包含下一行。

* 单行注释总是以换行符结束。注释 ( BTW) 后的续行符 (...) 和软命令中断 (,) 将被忽略。

* 带引号的字符串内的行继续和软命令中断将被忽略。未终止的字符串文字（无结束引号）将导致错误

### 注释

*(从 1.1 开始)*

单行注释以`BTW`, 开始，可以出现在一行代码之后、单独的一行上，或者出现在行分隔符 (,) 后面的一行代码之后。

所有这些都是有效的单行注释：

```
I HAS A VAR ITZ 12          BTW VAR = 12
```

```
I HAS A VAR ITZ 12,         BTW VAR = 12
```

```
I HAS A VAR ITZ 12
                BTW VAR = 12
```

多行注释以`OBTW`开始和结束`TLDR`结束，且应该在它们自己的行上开始，或者在行分隔符之后跟随一行代码。

这是有效的多行注释：

```
I HAS A VAR ITZ 12
            OBTW this is a long comment block
                 see, i have more comments here
                 and here
            TLDR
I HAS A FISH ITZ BOB
```

```
I HAS A VAR ITZ 12,  OBTW this is a long comment block
      see, i have more comments here
      and here
TLDR, I HAS A FISH ITZ BOB
```

### 文件创建

*(从 1.1 修改)*

所有 LOLCODE 程序都必须使用命令HAI打开。HAI后应该跟上当前的 LOLCODE 语言版本号（在本例中为 1.2）。不过，当前没有处理版本号的实现标准行为。

LOLCODE 文件由关键字`KTHXBYE`关闭，该关键字关闭`HAI`代码块。

---

## 变量

### 范围

*(有待重新审查和完善)*

从该版本开始，所有变量作用域都是封闭函数或主程序块的本地变量。变量只有在声明后才能访问，并且没有全局作用域。

### 命名

*(从 1.1 开始)*

变量标识符可以全部为大写或小写字母（或两者的混合）。它们必须以字母开头，后面只能跟其他字母、数字和下划线。不允许使用空格、破折号或其他符号。变量标识符区分大小写 - “cheezburger”、“CheezBurger”和“CHEEZBURGER”都是不同的变量。

### 声明和赋值

*(从 1.1 修改)*

要声明变量，关键字`I HAS A`后跟变量名。要在同一语句中为变量分配值，您可以在变量名称后面加上`ITZ <value>`。

变量的赋值是通过赋值语句完成的，`<variable> R <expression>`

```
I HAS A VAR            BTW VAR is null and untyped
VAR R "THREE"          BTW VAR is now a YARN and equals "THREE"
VAR R 3                BTW VAR is now a NUMBR and equals 3
```

---

## 类型

*(从 1.1 升级)*
LOLCODE 目前识别的变量类型是：字符串 （YARN）、整数 （NUMBR）、浮点数 （NUMBAR） 和布尔值 （TROOF）（数组 （BUKKIT） 保留用于将来扩展。键入是动态处理的。在给变量一个初始值之前，它是非类型化的（NOOB）。~~铸造操作也对TYPE类型进行操作~~

### 非类型

无类型类型 (NOOB) 不能隐式转换为除 TROOF 之外的任何类型。强制转换为 TROOF 会使变量失败。NOOB 上任何采用其他类型（例如数学）的操作都会导致错误。

对于所有其他类型，NOOB（无类型、未初始化）变量的显式转换将为空/零值。

### 布尔值

两个布尔值 (TROOF) 是 WIN (true) 和 FAIL (false)。空字符串 ("")、空数组和数字零都将转换为 FAIL。所有其他值的计算结果均为 WIN。

### 数值类型

NUMBR是主机实现/体系结构中指定的整数。引号YARN之外的任何连续数字序列，如果不包含小数点（.），都被视为NUMBR。NUMBR可能有一个前导连字符（-）来表示负数。

NUMBAR是宿主实现/体系结构中指定的浮点数。它表示为恰好包含一个小数点的连续字符串。将NUMBAR转换为NUMBR会截断浮点数的小数部分。将NUMBAR转换为YARN（例如，通过打印它），会将输出截断为默认的两位小数。NUMBAR可能有一个前导连字符（-）来表示负数。

将字符串转换为数字类型会像不在引号中一样解析字符串。如果有任何非数字、非连字符、非句点字符，则会导致错误。将WIN转换为数字类型会导致“1”或“1.0”；将FAIL转换为数字零。

### 字符串

字符串文本 （YARN） 用双引号 （“） 表示。在带引号的字符串中忽略行继续和软命令中断。未终止的字符串文本（无右引号）将导致错误。

在字符串中，所有字符都表示其文本值，但冒号 （:)除外，冒号 （ 是转义字符。紧跟在冒号后面的字符也具有特殊含义。

* :) represents a newline (\n)
* :> represents a tab (\t)
* :o represents a bell (beep) (\g)
* :" represents a literal double quote (")
* :: represents a single literal colon (:)

冒号也可能引入更冗长的转义，括在某种形式的括号内。

* :(`<hex>`) resolves the hex number into the corresponding Unicode code point.
* :{`<var>`} interpolates the current value of the enclosed variable, cast as a string.
* :[`<char name>`] resolves the `<char name>` in capital letters to the corresponding Unicode [normative name](http://www.unicode.org/Public/4.1.0/ucd/NamesList.txt).

### 数组

*数组和字典类型当前未指定。人们普遍愿意统一它们，但索引和定义仍在讨论中。*

### 类型

TYPE 类型仅具有 TROOF、NOOB、NUMBR、NUMBAR、YARN 和 TYPE 的值，作为裸词。他们可以合法地投射到TROOF（除了菜鸟之外都是真的）或YARN

*类型正在审查中。当前的观点是延迟定义它们，直到用户定义的类型相关，但这意味着在此期间无法解决类型比较。*

---

## Operators

### Calling Syntax and Precedence

Mathematical operators and functions in general rely on prefix notation. By doing this, it is possible to call and compose operations with a minimum of explicit grouping. When all operators and functions have known arity, no grouping markers are necessary. In cases where operators have variable arity, the operation is closed with `MKAY`. An `MKAY` may be omitted if it coincides with the end of the line/statement, in which case the EOL stands in for as many `MKAYs` as there are open variadic functions.

Calling unary operators then has the following syntax:

```
<operator> <expression1>
```

The `AN` keyword can optionally be used to separate arguments, so a binary operator expression has the following syntax:

```
<operator> <expression1> [AN] <expression2>
```

An expression containing an operator with infinite arity can then be expressed with the following syntax:

```
<operator> <expr1> [[[AN] <expr2>] [AN] <expr3> ...] MKAY
```

### Math

The basic math operators are binary prefix operators.

```
SUM OF <x> AN <y>       BTW +
DIFF OF <x> AN <y>      BTW -
PRODUKT OF <x> AN <y>   BTW *
QUOSHUNT OF <x> AN <y>  BTW /
MOD OF <x> AN <y>       BTW modulo
BIGGR OF <x> AN <y>     BTW max
SMALLR OF <x> AN <y>    BTW min
```

`<x>` and `<y>` may each be expressions in the above, so mathematical operators can be nested and grouped indefinitely.

Math is performed as integer math in the presence of two NUMBRs, but if either of the expressions are NUMBARs, then floating point math takes over.

If one or both arguments are a YARN, they get interpreted as NUMBARs if the YARN has a decimal point, and NUMBRs otherwise, then execution proceeds as above.

If one or another of the arguments cannot be safely cast to a numerical type, then it fails with an error.

### Boolean

Boolean operators working on TROOFs are as follows:

```
BOTH OF <x> [AN] <y>          BTW and: WIN iff x=WIN, y=WIN
EITHER OF <x> [AN] <y>        BTW or: FAIL iff x=FAIL, y=FAIL
WON OF <x> [AN] <y>           BTW xor: FAIL if x=y
NOT <x>                       BTW unary negation: WIN if x=FAIL
ALL OF <x> [AN] <y> ... MKAY  BTW infinite arity AND
ANY OF <x> [AN] <y> ... MKAY  BTW infinite arity OR
```

`<x>` and `<y>` in the expression syntaxes above are automatically cast as TROOF values if they are not already so.

### Comparison

Comparison is (currently) done with two binary equality operators:

```
BOTH SAEM <x> [AN] <y>   BTW WIN iff x == y
DIFFRINT <x> [AN] <y>    BTW WIN iff x != y
```

Comparisons are performed as integer math in the presence of two NUMBRs, but if either of the expressions are NUMBARs, then floating point math takes over. Otherwise, there is no automatic casting in the equality, so `BOTH SAEM "3" AN 3` is FAIL.

There are (currently) no special numerical comparison operators. Greater-than and similar comparisons are done idiomatically using the minimum and maximum operators.

```
BOTH SAEM <x> AN BIGGR OF <x> AN <y>   BTW x >= y
BOTH SAEM <x> AN SMALLR OF <x> AN <y>  BTW x <= y
DIFFRINT <x> AN SMALLR OF <x> AN <y>   BTW x > y
DIFFRINT <x> AN BIGGR OF <x> AN <y>    BTW x < y
```

If `<x>` in the above formulations is too verbose or difficult to compute, don't forget the automatically created IT temporary variable. A further idiom could then be:

```
<expression>, DIFFRINT IT AN SMALLR OF IT AN <y>
```

*Suggestions are being accepted for coherently and convincingly english-like prefix operator names for greater-than and similar operators.*

### Concatenation

An indefinite number of YARNs may be explicitly concatenated with the `SMOOSH...MKAY` operator. Arguments may optionally be separated with `AN`. As the `SMOOSH` expects strings as its input arguments, it will implicitly cast all input values of other types to YARNs. The line ending may safely implicitly close the `SMOOSH` operator without needing an `MKAY`.

### Casting

Operators that work on specific types implicitly cast parameter values of other types. If the value cannot be safely cast, then it results in an error.

An expression's value may be explicitly cast with the binary `MAEK` operator.

```
MAEK <expression> [A] <type>
```

Where `<type>` is one of TROOF, YARN, NUMBR, NUMBAR, or NOOB. This is only for local casting: only the resultant value is cast, not the underlying variable(s), if any.

To explicitly re-cast a variable, you may create a normal assignment statement with the `MAEK` operator, or use a casting assignment statement as follows:

```
<variable> IS NOW A <type>         BTW equivalent to
<variable> R MAEK <variable> [A] <type>
```

---

## Input/Output

### Terminal-Based

The print (to STDOUT or the terminal) operator is `VISIBLE`. It has infinite arity and implicitly concatenates all of its arguments after casting them to YARNs. It is terminated by the statement delimiter (line end or comma). The output is automatically terminated with a carriage return (:)), unless the final token is terminated with an exclamation point (!), in which case the carriage return is suppressed.

```
VISIBLE <expression> [<expression> ...][!]
```

There is currently no defined standard for printing to a file.

To accept input from the user, the keyword is

```
GIMMEH <variable>
```

which takes YARN for input and stores the value in the given variable.

*`GIMMEH` is defined minimally here as a holdover from 1.0 and because there has not been any detailed discussion of this feature. We count on the liberal casting capabilities of the language and programmer inventiveness to handle input restriction. `GIMMEH` may change in a future version.*

---

## Statements

### Expression Statements

A bare expression (e.g. a function call or math operation), without any assignment, is a legal statement in LOLCODE. Aside from any side-effects from the expression when evaluated, the final value is placed in the temporary variable `IT`. `IT`'s value remains in local scope and exists until the next time it is replaced with a bare expression.

### Assignment Statements

Assignment statements have no side effects with `IT`. They are generally of the form:

```
<variable> <assignment operator> <expression>
```

The variable being assigned may be used in the expression.

### Flow Control Statements

Flow control statements cover multiple lines and are described in the following section.

---

## Flow Control

### Conditionals

#### If-Then

The traditional if/then construct is a very simple construct operating on the implicit `IT` variable. In the base form, there are four keywords: `O RLY?`, `YA RLY`, `NO WAI`, and `OIC`.

`O RLY?` branches to the block begun with `YA RLY` if `IT` can be cast to WIN, and branches to the `NO WAI` block if `IT` is FAIL. The code block introduced with `YA RLY` is implicitly closed when `NO WAI` is reached. The `NO WAI` block is closed with `OIC`. The general form is then as follows:

```
<expression>
O RLY?
  YA RLY
    <code block>
  NO WAI
    <code block>
OIC
```

while an example showing the ability to put multiple statements on a line separated by a comma would be:

```
BOTH SAEM ANIMAL AN "CAT", O RLY?
  YA RLY, VISIBLE "J00 HAV A CAT"
  NO WAI, VISIBLE "J00 SUX"
OIC
```

The elseif construction adds a little bit of complexity. Optional `MEBBE <expression>` blocks may appear between the YA RLY and NO WAI blocks. If the `<expression>` following `MEBBE` is WIN, then that block is performed; if not, the block is skipped until the following `MEBBE`, `NO WAI`, or `OIC`. The full expression syntax is then as follows:

```
<expression>
O RLY?
  YA RLY
    <code block>
 [MEBBE <expression>
    <code block>
 [MEBBE <expression>
    <code block>
  ...]]
 [NO WAI
    <code block>]
OIC
```

An example of this conditional is then:

```
BOTH SAEM ANIMAL AN "CAT"
O RLY?
  YA RLY, VISIBLE "J00 HAV A CAT"
  MEBBE BOTH SAEM ANIMAL AN "MAUS"
    VISIBLE "NOM NOM NOM. I EATED IT."
OIC
```

#### Case

*(modified from 1.1)*

The LOLCODE keyword for switches is `WTF?`. The `WTF?` operates on `IT` as being the expression value for comparison. A comparison block is opened by `OMG` and must be a literal, not an expression. (A literal, in this case, excludes any YARN containing variable interpolation (`:{var}`).) Each literal must be unique. The `OMG` block can be followed by any number of statements and may be terminated by a `GTFO`, which breaks to the end of the the `WTF` statement. If an `OMG` block is not terminated by a `GTFO`, then the next `OMG` block is executed as is the next until a `GTFO` or the end of the `WTF` block is reached. The optional default case, if none of the literals evaluate as true, is signified by `OMGWTF`.

```
WTF?
  OMG <value literal>
    <code block>
 [OMG <value literal>
    <code block> ...]
 [OMGWTF
    <code block>]
OIC
```

```
COLOR, WTF?
  OMG "R"
    VISIBLE "RED FISH"
    GTFO
  OMG "Y"
    VISIBLE "YELLOW FISH"
  OMG "G"
  OMG "B"
    VISIBLE "FISH HAS A FLAVOR"
    GTFO
  OMGWTF
    VISIBLE "FISH IS TRANSPARENT"
OIC
```

In this example, the output results of evaluating the variable `COLOR` would be:

"R":

```
RED FISH
```

"Y":

```
YELLOW FISH
FISH HAS A FLAVOR
```

"G":

```
FISH HAS A FLAVOR
```

"B":

```
FISH HAS A FLAVOR
```

none of the above:

```
FISH IS TRANSPARENT
```

#### Loops

*Loops are currently defined more or less as they were in the original examples. Further looping constructs will be added to the language soon.*

Simple loops are demarcated with `IM IN YR <label>` and `IM OUTTA YR <label>`. Loops defined this way are infinite loops that must be explicitly exited with a GTFO break. Currently, the `<label>` is required, but is unused, except for marking the start and end of the loop.

*Immature spec – **subject to change**:*

Iteration loops have the form:

```
IM IN YR <label> <operation> YR <variable> [TIL|WILE <expression>]
  <code block>
IM OUTTA YR <label>
```

Where `<operation>` may be UPPIN (increment by one), NERFIN (decrement by one), or any unary function. That operation/function is applied to the `<variable>`, which is temporary, and local to the loop. The `TIL <expression>` evaluates the expression as a `TROOF`: if it evaluates as `FAIL`, the loop continues once more, if not, then loop execution stops, and continues after the matching `IM OUTTA YR <label>`. The `WILE <expression>` is the converse: if the expression is `WIN`, execution continues, otherwise the loop exits.

---

## Functions

### Definition

A function is demarked with the opening keyword `HOW IZ I` and the closing keyword `IF U SAY SO`. The syntax is as follows:

```
HOW IZ I <function name> [YR <argument1> [AN YR <argument2> …]]
  <code block>
IF U SAY SO
```

Currently, the number of arguments in a function can only be defined as a fixed number. The `<argument>`s are single-word identifiers that act as variables within the scope of the function's code. The calling parameters' values are then the initial values for the variables within the function's code block when the function is called.

*Currently, functions do not have access to the outer/calling code block's variables.*

### Returning

Return from the function is accomplished in one of the following ways:

* `FOUND YR <expression>` returns the value of the expression.
* `GTFO` returns with no value (NOOB).
* in the absence of any explicit break, when the end of the code block is reached (`IF U SAY SO`), the value in `IT` is returned.

### Calling

A function of given arity is called with:

```
I IZ <function name> [YR <expression1> [AN YR <expression2> [AN YR <expression3> ...]]] MKAY
```

That is, an expression is formed by the function name followed by any arguments. Those arguments may themselves be expressions. The expressions' values are obtained before the function is called. The arity of the functions is determined in the definition.
