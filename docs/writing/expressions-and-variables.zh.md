---
title: "表达式和变量"
date: 
tags: []
summary: "了解如何使用变量，以及如何在代码中使用它们。"
draft: false
toc: true
weight: 4
menu: 
    docs:
        parent: "writing"
---

Yarn语言是一种完整的编程语言，这意味着它支持编写复杂的表达式，使您可以控制游戏中对话的工作方式。 在本文档中，您将学习如何编写表达式以及可以在何处使用它们。 

## 表达式和`if`语句

`if`语句用于控制是否显示特定的内容。当您编写`if`语句时，需提供一个可计算的*表达式*，如果该表达式的计算结果为`真`，则运行`if`语句的内容。

例如，考虑以下代码：

```yarn
<<if $gregg_day_intro is 0>>
    Gregg: Sup duder. 
    Mae: hey.
<<endif>>
```

在这里被使用的表达式为`$gregg_day_intro is 0`。在该表达式中，变量将会使用`is`[操作符](#operators)将`$gregg_day_intro`的值与数字`0`作比较。如果两者相等，则表达式判断为`真`并且运行其中的行。

## 变量

您可以将有关游戏状态信息存储在`变量`中。变量存储信息；更具体地说，它们可以存储文本，数字，真值或假值以及`null`（代表“无值”） 

要设置变量的值，可以使用关键字`set`。例如： 

```yarn
Mae: Oh god. Steve Scriggins.
<<set $suspect_steve to 1>>
Lori: Yeah. Him.
```

这会将变量`$suspect_steve`设置为数字1。 

如果未设置变量，则其值为`null`。 

### 变量和您的游戏

Yarn Spinner并不管理保存在变量本身中的信息。相反，您的游戏在开始运行对话之前需要为Yarn Spinner提供一个*变量存储*对象。

当Yarn Spinner需要知道变量的值时，它将询问您提供给它的变量存储对象。当Yarn Spinner想要设置变量的值时，它将提供变量的值和名称。这样，您的游戏就可以控制数据的存储方式。

{{<note>}}
有关变量存储以及游戏如何与Yarn Spinner集成的更多信息，请参见{{<xref "/docs/unity/components">}}。
{{</note>}}

## 值

除变量外，表达式还可以使用*值*。值可以是数字，字符串以及`true`和`false`之类的东西。 

除了在表达式中使用之外，任何值都可以存储在变量中。 

Yarn Spinner也有一个特殊值，称为*空*。该值表示“无值”；任何尚未分配值的变量将为`null`。

## 操作符

要使用变量和值，请将它们与*操作符*结合使用。
操作符包括算术运算符`+`（加法）和`-`（减法），比较运算符如`<`（小于）和`==`（等于），以及逻辑运算符如`and`和`or`等的运算符。 

要查看Yarn Spinner中操作符的完整列表，请参阅 {{<xref "/docs/syntax" "expression">}}。

## 内联表达式

表达式可以嵌入到[行]({{<ref "nodes-and-content.md#lines">}})中。当您这么做时，该表达式的结果将嵌入到该行的文本中，并显示给用户。 

例如，如果您将玩家与某角色说话的次数存储在变量`$times_spoken`中，则可以这样显示：

```yarn
Player: Hi!
Character: You've spoken to me {$times_spoken} times today!
```

{{<note>}}
为了使您的行具有正确的语法，可以使用[格式化函数]({{<ref "syntax.md#format-functions">}})来根据表达式选择行中的内容。 
{{</note>}}