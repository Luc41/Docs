---
title: "标记"
date: 
tags: []
summary: "了解如何使用属性修改文本 "
draft: false
toc: true
weight: 5
menu: 
    docs:
        parent: "writing"
---

标记允许您在文本中添加属性，例如`[a]hello[/a]`。您的游戏可以使用这些属性来执行诸如更改文本格式，添加动画等操作。

解析文本后，标记将从文本中删除，您会收到有关属性适用的纯文本范围的信息。

## 属性

属性适用于一个范围内的文本：

```yarn
Oh, [wave]hello[/wave] there!
```

`Dialogue.ParseMarkup`方法将获取此文本，并产生两个元素：_纯文本_ 和 _属性_ 的集合。纯文本是没有任何标记的文本；在此示例中，它将是：

```yarn
Oh, hello there!
```

属性表示包含其他内容信息的纯文本范围。它们包含一个 _位置_，一个 _长度_ 和它们的 _名称_ 以及 _特性_。 

在本示例中，将生成一个带有位置为4，长度为5，名称为“wave”的单个属性。 

属性以`[这样]`的形式开头，并以`[/这样]`的形式结束。

### 重叠属性

属性可以重叠：

您可以将多个属性相互放置。 例如： 

```yarn
Oh, [wave]hello [bounce]there![/bounce][/wave]
```

您也可以按任意顺序关闭属性。例如，该示例与上一个示例的含义相同： 

```yarn
Oh, [wave]hello [bounce]there![/wave][/bounce]
```

### 自闭合属性

属性可以自我闭合：

```yarn
[wave/]
```
自闭合属性的长度为零。 

### 关闭全部标记

标记`[/]`是 _关闭全部标记_。它闭合所有当前未闭合的属性。例如：

```yarn
[wave][bounce]Hello![/]
```

### 特性（properties）

属性可以具有特性：

```yarn
[wave size=2]Wavy![/wave]
```

该属性“wave”具有一个名为“size”的特性，该特性具有一个整数值为5。 

### 速记特性（Short-hand Properties）

属性可以具有速记特性，如下所示：

```yarn
[wave=2]Wavy![/wave]
```

该属性“wave”具有一个称为“wave”的特性，该特性具有一个整数值2。该属性的名称取自第一个特性。

### 特性数据类型

特性可以是以下任何一种数据类型：

- 整数
- 浮点数
- ‘真’或‘假’
- 字符串

不带引号的单个单词将被解析为字符串。例如，以下两行是相同的：

```yarn
[mood=angry]Grr![/mood]
[mood="angry"]Grr![/mood]
```

### 空格对齐（Whitespace Trimming）

如果自闭合属性前面有空格，或者位于
行的开头，则需要在其后对齐一个空格。这表示以下文本将产生纯文本“A B”：

```yarn
A [wave/] B
```

如果您不想对齐空格，请添加特性`trimwhitespace`，并设置为`false`： 

```yarn
A [wave trimwhitespace=false/] B (产生 "A  B")
```

## `nomarkup`属性

`nomarkup`属性使解析器忽略其中的任何标记字符。 

如果您想包含`[`和`]`之类的字符，请将其包裹在
`nomarkup`属性中： 

```yarn
A [nomarkup];][/nomarkup] B (产生 "A ;] B")
```

## `character`属性

`character`属性用于标记行中正在说话的角色的部分。 

Yarn Spinner将通过在行中像这样寻找角色的名字尝试为您添加该角色：

```yarn
CharacterA: Hello!
CharacterB: Oh, hi!
```

标记解析器将把从行开始到行的第一个`：`之间的所有内容（及其后的任何空白）标记为`character`属性。 该属性具有一个名称特性，该特性包含从行首到`:`的文本。如果不存在`:`，或者在标记中添加了`character`属性，则该特性不会被添加。 

这意味着上面的示例与此示例相同： 

```yarn
[character name="CharacterA"]CharacterA: [/character]Hello!
[character name="CharacterB"]CharacterB: [/character]Oh hi!
```

您可以使用它来对齐游戏中各行的角色名称。

## 标记和格式化函数

{{<note>}}
本部分仅适用于从2.0版本之前的Yarn Spinner迁移过来的用户。 
{{</note>}}

标记取代了Yarn Spinner v1.1的格式化函数。现在，格式化函数已实现为自闭合标记；行解析器类的`RegisterMarkerProcessor`方法允许您将某些标记声明为“替换”标记，行解析器将调用这些标记到您的代码中，以获取要插入到最终组成的行中的文本。`nomarkup`，`select`，`plural`和`ordinal`标记就是通过这种方式实现的。 

这意味着您的格式化函数需要稍作更改以匹配新格式。

旧的格式：

```yarn
[select {$gender} m="he" f="she" nb="they"]
[plural {$pies} one="pie" other="pies"]
[plural {$position} one="%st" two="%nd" few="%rd" other="%th"]
```

现在的格式：

```yarn
[select value={$gender} m="he" f="she" nb="they" /]
[plural value={$pies} one="pie" other="pies" /]
[plural value={$position} one="%st" two="%nd" few="%rd" other="%th" /]
```

另外，您需要将`Dialogue.LocaleCode`属性设置为语言环境代码，例如`en`。
