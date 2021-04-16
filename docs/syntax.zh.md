---
title: "语法快速参考"
date: 2019-12-11T16:59:07+11:00
tags: []

toc: true
menu:
  docs:
    weight: 110
    title: "语法参考"
---

本文档是一份您可以在Yarn文件中编写的各种语法的快速参考指南。其并非意在成为一个教程，如需教程，请查阅[此处]({{<ref "tutorial.zh.md">}})！如果您想知道如何构造Yarn代码行才能正常工作，那么本文档将是一个有用的参考。

## 基础用法

Yarn是一种为对话设计的编程语言。这意味着它需要遵照一些规则，使得计算机可以理解它。

Yarn文件以节点（nodes）作为分割。每个节点的组成包含多种不同的语句。
通常来说Yarn Spinner对Yarn文件中的每一新行为视为独立的元素。Yarn Spinner需要理解其可能的含义。

## 节点（nodes）

在Yarn中所有内容都是一个节点，并且您的所有对话包含在一个节点之中。
您的Yarn文件可能包含多个节点或仅有一个节点，但是所有的Yarn都必须是节点的一部分。
所有节点都拥有相同的基础结构：

```yarn
标头
---
正文
===
```

标头与主体之间用`---`分隔（一行上有三个`-`符号）。
节点以`===`（一行上的三个`=`符号）终止。

如果您使用了可视化编辑器，这些节点组成了您在画布上看到的区块。

## 标头（header）

标头包含了节点的元数据（metadata），并且可以根据需要包含任意数量的节点元数据。
元数据通过键-值（key-value）对形式创建。
每个元数据拥有相同的布局：

```yarn
键:值
```

每个元数据必须自己单独占据一行。

这是一个示例：`myKey:myValue`拥有一个`myKey`键和一个对应的`myValue`值。

### 标题（title）

`title`标头定义了节点的名字。所有节点必须存在一个`title`。

此标头看起来将会像是`title:nodename`形式。节点的名字（在这个示例中，节点的名字是`nodename`）在整个项目中必须唯一，并且可以是大小写字母和数字的任意组合。节点标题不能有空格。

这个值是区分大小写的，所以`nodename`与`nodeName`是不同的。

### 标签（tags）

节点可以具有关于它们的其他元数据，您可以在`tags`标头中定义这些元数据。这是一个用空格分隔的单词列表。

不同于元数据`title`，标签的值不要求唯一。

## 正文（body）

正文是节点中`---`和`===`之间的部分，您将会在这个地方花费大量的时间，同时也是所有对话进行的地方。

## 对话（dialogue）

通常情况下您的Yarn文件的主要内容都是对话——游戏中您的角色所说的台词。
对话的每一行都将被Yarn Spinner视为新的对话声明。
对话可以包含几乎任何文本。

大部分对话行的编写方式如下：

```yarn
玩家: 你好，萨丽。
萨丽: 噢！你好。
萨丽: 你偷偷地靠近我。
```
在这个示例中，有角色名在我们想让其说的台词前。
这种形式是可选的，您可以根据需要编写对话：

```yarn
你好，萨丽。
噢！你好。
你偷偷地靠近我。
```

如果您不编写角色名称，那么您的游戏将需要其他方法来确定哪一行对应哪个角色，选择哪种形式将由您自己决定。

### 内联表达式（inline expressions）

对话行中支持嵌入[表达式](#expression)。这些表达式将会在对话运行到此处时展开。在`{`和`}`之内的任何表达式都会被解析并与其他文本一同按序渲染

例如，假设您有一个名为`$gold`的变量。您可以像这样向玩家显示此变量的内容：

```yarn
您有 {$gold} 个金币.
```

使用更复杂的表达式也是支持的。例如，您可以使用自己提供的值进行数学运算：

```yarn
如果您有两倍的金币, 您可以获得 {$gold * 2} 个金块!
```

{{<note>}}
内联表达式可以同[format functions](#format-functions)一同使用，尤其是当您需要对您的文本进行[本地化]({{<ref "localisation.zh.md#localising-inline-expressions">}})的时候。
{{</note>}}

## 选项（options）

选项允许您在不同节点之间移动。

```yarn
[[展示给用户的文本|节点名字]]
```

选项分为两部分，用竖线符号（`|`）分隔。
第一部分是要显示给用户的任何文本，在竖线后面的第二部分是节点的名称。
节点名称必须是Yarn文件中其他位置的另一个节点的名称，否则程序将无法运行。
在玩家选择一个选项后，将加载链接的节点（在本例中为`节点名字`），并且故事将从此处继续。

内联表达式可以在选项中使用。

{{<note>}}
在{{< xref "/docs/writing/controlling.zh" "options" >}}中进一步了解选项。
{{</note>}}



## 跳转（jumps）

跳转告诉Yarn Spinner开始运行某个其他节点。它们不会显示给玩家。

```yarn
[[节点名字]]
```

当Yarn Spinner遇到这一行时，它将立即开始运行`节点名字`节点的内容。

{{<note>}}
在{{< xref "/docs/writing/controlling" "jumps">}}中进一步了解跳转。
{{</note>}}



## 快捷选项（shortcut options）

快捷选项使您可以无需创建新节点来创建简单的分支对话框。
每个快捷选项均以`->`箭头开头，然后是应显示给玩家的一行文本。

```yarn
我们接下来应该去哪？
-> 北
-> 南
-> 东
-> 西
```

这将显示`我们接下来应该去哪？`行，然后显示`北`，`南`，`东`，`西`的选项。

{{<note>}}
在{{< xref "/docs/writing/controlling" "shortcut-options" >}}中进一步了解快捷选项。
{{</note>}}



### 快捷范围（shortcut scope）

快捷选项允许您在其选项下方添加其他语句以运行其他行，并且在许多情况下，这些语句用于设置或更新变量。
新语句必须位于快捷选项的下方，它们是快捷选项的一部分，并且需要相对于该快捷选项缩进。

```yarn
-> 北
    北方很冷。
-> 南
    您已经听说南方的天气晴朗。
-> 东
    您不确定东方强盗的传言是否属实。
-> 西

你继续你的旅程。
```

根据玩家选择的选项将会显示不同的行：

* 在上面的示例中，如果玩家选择`北`选项，对话行`北方很冷`将会再接下来显示，紧接着是`你继续你的旅程`。
* 如果选择`南`或`东`，玩家将会在`你继续你的旅程`之前看到不同的内容。
* 如果选择`西`,玩家将仅能看到`你继续你的旅程`。

缩进必须保持一致，否则Yarn Spinner将无法解析您的意图。

### 快捷条件（shortcut conditionals）

快捷选项也可以限制为仅在需要时显示。
您可以通过在快捷选项的末尾添加`if`语句来完成此操作。

```yarn
-> 上锁的活板门 <<if $hasKey is true>>
-> 北
-> 南
-> 东
-> 西
```
在上面的示例中选项`上锁的活板门`只有在变量`$hasKey`是`true`的情况下才会显示为一个选项并被玩家选择。
如果`$hasKey`为`false`则只有四个方向出现。

## 变量

您可以在变量中存储信息。

变量名是字母和数字的任意组合，以`$`符号开始。
变量名对大小写敏感，因此就Yarn Spinner而言，`$varName`和`$varname`是两个不同的变量。

变量的作用域是全局的，因此他们存在于所有文件的所有节点中。
这意味着你可以在一个文件中声明一个变量，然后另一个文件中使用该变量。

### 值

变量可以包含数字，文本，`true`和`false`或`null`。

#### 赋值

您可以使用`set`操作对变量来设置和更新变量内部的值：

```yarn
<<set $var to 1>>
```

`set`操作的命令的格式为起始符号`<<`，后紧接关键字`set`，之后是变量，在本例中是`$var`，接着是关键字`to`，最终是您期望的值。

如果愿意，也可以使用`=`来替换关键字`to`。

#### 数值

数值不论赋予什么值，其总是带有小数点的（技术上称为浮点数）。
这意味着如果您对一个变量赋值`1`,当您从Yarn Spinner中返回它时将会是`1.0`。

#### 文本

变量内的文本可以是任何您想要的内容，但必须包含引号。 
这意味着`<<set $var to "hello">>`是有效的，`<<set $var to hello>>`不是。

#### `真` 和 `假`

变量可以通过使用关键字`true`、`TRUE`和`false`、`FALSE`来赋值为真或假。
关键字首字母大小写敏感，因此`<<set $var to true>>`是合法的，`<<set $var to True>>`不合法。

## 条件

Conditionals are how you can create different branching dialogue and events based on logical statements.
All conditionals take the same basic structure, an `if` statement, then zero or more `elseif` statements, then zero or one `else` statements, and finally the `endif`.

Yarn Spinner will work its way down the conditional until it meets a piece of it it can run and presents the statements for that piece.
```yarn
<<if $money > 1>>
	I would like a horse please.
	<<set $money to $money - 2>>
	<<set $hasHorse to true>>
<<elseif $money eq 1 >>
	Just a drink thanks.
	<<set $money to $money - 1>>
<<else>>
	Drat, I can't afford anything.
<<endif>>
```

Take the above example, if `$money` happens to be `1` then the line `Just a drink thanks.` will be shown, but if `$money` was `5` then the line `I would like a horse please.` would be shown.

It can be more readable to indent lines inside the `if`, but it's not required.


{{<note>}}
Learn more about conditionals in {{< xref "/docs/writing/controlling" >}}.
{{</note>}}

### `if`, `elseif`, `else` and `endif` 

The `if` statement opens a conditional and is comprised of the `<<` command opening keyword followed immediately by the `if` keyword, then an expression that controls the `if` and finally the command close keyword `>>`.

Any lines that go between the `if` and the next part of the conditional, so either an `elseif`, an `else`, or an `endif`, is shown if the expression is ultimately true.

#### `elseif`

An `elseif` is an optional part of the conditional and works similar to the `if` but instead uses the `elseif` keyword.
Each `elseif` is identical to its `if` counterpart in structure and you can have as many `elseif`'s as you want, including none.

```yarn
<<elseif $money eq 1>>
```

#### `else`

The `else` is the fallback of the conditional and is run only if the `if` and all its `elseif`'s evaluate to false.
It is optional.

```yarn
<<else>>
```

#### `endif`

The `<<endif>>` finishes the conditional statement and is required.
It is needed so Yarn Spinner knows when the conditional has concluded.


### Expression

An expression is a mathematical or logical operation and work and looks like a line of maths.
For the expression to be useful in the conditional it needs to eventually evaluate to true or false.
If the expression results in true it will be the part of the conditional that gets run.

All expressions follow the same pattern of a subexpression followed by an operator and then another subexpression.
The subexpressions can further broken up into other expression if needed:

```yarn
<<if ($counter + 1) >= ($max - 2)>>
```

#### Logical operators

Yarn Spinner supports the following logical operators and most have multiple ways being written:

- Equality: `eq` or `is` or `==`
- Inequality: `neq` or `!`
- Greater than: `gt` or `>`
- Less than: `lt` or `<`
- Less than or equal to: `lte` or `<=`
- Greater than or equal to: `gte` or `>=`
- Boolean OR: `or` or `||`
- Boolean XOR: `xor` or `^`
- Boolean Negation: `!`
- Boolean AND: `and` or `&&`

#### Maths operators

- Addition: `+`
- Subtraction: `-`
- Multiplication: `*`
- Division: `/`
- Truncating Remainder Division: `%`
- Brackets: `(` to open the brackets and `)` to close them.

#### Order of operations

Yarn Spinner follows a fairly standard order of operations, and falling back to using left to right when operators are of equivalent priority.
The order of operations is as follows:

1. Brackets
2. Boolean Negation
3. Multiplication, Division, and Truncating Remainder Division
4. Addition, Subtraction
5. Less than or equals, Greater than or equals, Less than, Greater than
6. Equality, Inequality
7. Boolean AND, Boolean OR, Boolean XOR

## Commands

Commands are a way of Yarn Spinner communicating back to the game that events have happened that need to be handled.
These are often used to trigger achievements and to move characters and cameras around to where they need to be.

Commands start by having the command opening symbol `<<` then any text you want sent over to the game, and finish with the command close symbol `>>`.
As an example:

```yarn
<<move camera left>>
<<unlockAchievement beganAdventure>>
```

Commands by themselves do nothing, you need to handle these messages yourself.

Inline expressions can be used in options.

{{<note>}}
To learn more about how to define your own commands for your game, see {{<xref "/docs/unity/working-with-commands.md">}}.
{{</note>}}

## Localisation Tags

Localisation tags are a way of marking lines of dialogue to the localisation system.
If you aren't localising your game you don't need them and will not encounter them.
Each tag starts with a `#` symbol and then have a `line` keyword and an autogenerated value.

```yarn
#line:a8e70c
```

The tags always go on the end of the line and should never be edited or created manually.
You will find localisation tags at the end of dialogue lines, shortcuts, and options.

```yarn
Player: Hey. #line:a8e70c
Sally: Oh! Hi. #line:2dc39b

[[See you later.|Sally.Exit]] #line:0facf7
```

## Format functions

Format functions allow you to select content based on a value. Format functions are useful for adapting a line to be grammatically valid based on the value of a variable. 

For example, if you want to say the sentence "I have 1 apple", the word "apple" in English needs to change depending on whether you have 1 apple or 2 apples. Format functions allow you to switch the word out.

Format functions are extremely useful for [localisation]({{<ref "localisation.md">}}), because they can be different in each of the string tables that you're working with.

Format functions start and end with `[` and `]`. Inside these braces, you specify which specific format function you want to use, and then a list of categories and replacements to use.

{{<note>}}
The format function syntax is based on similar implementations in [Unreal](https://docs.unrealengine.com/en-US/Gameplay/Localization/Formatting/#argumentmodifiers), [Unity](https://docs.unity3d.com/Packages/com.unity.localization@0.2/manual/index.html#plural-support), and Unicode's [MessageFormat](https://messageformat.github.io/messageformat/).
{{</note>}}

There are three format functions available: `select`, `plural` and `ordinal`.

### `select`

The `select` format function allows you to use a variable's value to select between a fixed set of options.   

```yarn
[select {$value} option1="replacement1" option2="replacement2"]
```

Based on the value of the variable you provide, different replacement will be selected. In the above example, if the value of `$value` is `"option1"`, then the text `"replacement1"` will be inserted; if `$value` is `"option2"`, then `"replacement2"` will be inserted. There's no limit on the number of options. If `$value` is neither, an error value will be inserted.

{{<note>}}
The `select` format function does not look for specific values; rather, it's a generic text replacement tool.
{{</note>}}

This is most useful for when you want to use different pronouns based on a variable storing the gender of a character. 

For example, imagine that the variable `$gender` contains the gender of a character, and may contain the values "`m`" for male, "`f`" for female, or "`nb`" for nonbinary.

```yarn
I haven't seen [select {$gender} m="him" f="her" nb="them"].
```

### `plural` and `ordinal`

The `plural` and `ordinal` format functions maps a number value to its *plural class*, based on the current language.

Different languages use different rules for pluralising numbers. In English (and many other languages), there are two plural classes numbers: if an integer is 1, it is *singular*, and otherwise, it's *plural*; other rules apply to different languages.

These format functions let you pass in a variable, and select the appropriate text to display based on the plural class:

* The `plural` format function selects a number's *cardinal* plural class, used for counting values: "1 apple", "2 apples", and so on.
* The `ordinal` format function selects a number's *ordinal* plural class, used for ranking values: "1st", "2nd", and so on.

The available plural classes for use in `plural` and `ordinal` are: *zero*, *one*, *two*, *few*, *many*, and *other*.

{{<note>}}
Different plural classes exist in different languages; when localising a line that contains the `plural` or `ordinal` format functions, you will need to use the plural classes that apply to your target language.
{{</note>}}

The rules used to select the plural class for the number are derived from [Unicode's CLDR](https://www.unicode.org/cldr/charts/36.1/supplemental/language_plural_rules.html). The specific rules used at run-time are determined by the text language property on the {{<xref "/docs/unity/components/dialogue-runner">}}.


#### `plural`

The `plural` format function selects text based on the input value's cardinal plural class.

For example, imagine you have a variable called `$apples`, which represents how many apples the player has, and you want to show this to the player. You can use the `plural` format function for this, like so:

```yarn
You have {$apples} [plural {$apples} one="apple" other="apples"]
```

When the language text of the [dialogue runner]({{<ref "dialogue-runner.md">}}) is set to English, this will render in the following ways:

* `$apple == 1`: "You have 1 apple"
* `$apple == 2`: "You have 2 apples"
* `$apple == 0`: "You have 0 apples"

#### `ordinal`

The `plural` format function selects text based on the input value's ordinal plural class.

For example, imagine you have a variable called `$race_position`, which contains the position that the player came in in a race. You can use the `ordinal` format function for this, like so:

```yarn
I came in {$race_position}[ordinal {$race_position} one="st" two="nd" few="rd" other="th"] place!
```

When the language text of the [dialogue runner]({{<ref "dialogue-runner.md">}}) is set to English, this will render in the following ways:

* `$race_position == 1`: "I came in 1st place!"
* `$race_position == 2`: "I came in 2nd place!"
* `$race_position == 3`: "I came in 3rd place!"
* `$race_position == 6`: "I came in 6th place!"

### Value insertion

In a format function's replacement value, the `%` character is replaced with the original value that was used to select the appropriate text to use. For example, going back to our "number of apples" example, you can write the line in a way that only causes numbers to appear in the plural case:

```yarn
You have [plural {$apples} one="an apple" other="% apples"]
```

In English, this will be rendered as:

* `$apple == 1`: "You have an apple"
* `$apple == 2`: "You have 2 apples"
