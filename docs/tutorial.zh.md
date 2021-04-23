---
title: "教程"
date: 2019-12-11T16:59:07+11:00
tags: []
summary: "开始使用Yarn Spinner"
toc: true
menu:
  docs:
    weight: 100   
---

**欢迎来到 Yarn Spinner！**在本教程中，您将会学习如何在Unity工程中使用Yarn Spinner来创建交互式对话。

我们将会从下载和安装Yarn Spinner开始。接下来，我们将对驱动Yarn Spinner的核心概念进行概览，并撰写一些对话。在这之后，我们将会探索一些Yarn Spinner的高级功能。

## Yarn Spinner 介绍

Yarn Spinner是一个为在游戏中撰写交互式对话，也就是说，玩家可以与游戏中的角色对话而设计的工具。Yarn Spinner通过允许您使用称为*Yarn*的编程语言编写对话来实现此目的。

Yarn被设计的尽可能的小巧。举个例子，下面展示的便是合法的Yarn代码：

```yarn
Gregg: I think I might be sick.
Mae: True friendship: Letting your friend make you sick.
Gregg: True bros.
Mae: True bros.
```

Yarn Spinner每次处理一行，并将它们传递到游戏中。如何处理这些行则完全取决于游戏；举个例子，这些来自[Night in the Woods](http://nightinthewoods.com)的台词以气泡显示，一次显示一个字符，并等待用户按下按钮再显示下一个。

## 快速开始

我们将会从创建一个空Unity工程开始，安装Yarn Spinner，并且运一个示例游戏。接下来我们会对这个示例进行一些修改。

* **Unity 2018.4的用户**：我们建议您在阅读本教程时下载`.unitypackage`，而非使用Unity Package Manager，否则您将无法获得示例项目。如果您使用的是最新版本的Unity，您可以通过Package Manager或通过`.unitypackage`来安装。

### 运行示例游戏

我们将从运行Yarn Spinner附带的示例游戏开始。 它很短，只有大约2分钟。

1. **创建一个空Unity工程。** 选择2D模版。
2. **下载并安装Yarn Spinner。** 前往[安装]({{< ref "docs/unity/installing.md" >}})指南，并遵循指示。
3. **打开示例场景。**
    * 如果您通过`.unitypackage`安装Yarn Spinner，您将会在`Yarn Spinner/Examples/Space`文件夹中找到它。
    * 如果您通过Package Manage安装Yarn Spinner，导入Space sample，然后在`Samples/Yarn Spinner/<version>/Space`文件夹中找到场景。
4. **运行游戏。** 使用左、右箭头键移动，空格键与角色对话。

现在，我们可以开始进行深入研究了，看看Yarn Spinner如何驱动这款游戏。

### Yarn 编辑器

Yarn Spinner将其对话保存在`.yarn`文件中。这些文件是纯文本，这意味着您可以在任何纯文本编辑器中编辑它们（[Visual Studio Code](https://code.visualstudio.com)是一个很好的选择，并且我们提供了一个[语法高亮扩展](https://marketplace.visualstudio.com/items?itemName=SecretLab.yarn-spinner)来使其更好用！）

<!-- Link to VS Code Extension -->

您也可以使用[Yarn Editor](https://yarnspinnertool.github.io/YarnEditor)，这是一个基于web的Yarn编辑工具。Yarn Editor很有用，因为它使您可以以非常直观的方式查看对话的结构。

<!-- Write a Yarn Editor tutorial and link it here probably  -->

## 读懂 Yarn

在本节教程中，我们将打开文件`Sally.yarn`，看看它在做什么。

1. **在您选择的编辑器中打开`Sally.yarn`。**

Yarn Spinner将其所有的对话分组为**节点**。节点包含：您的对话行，展示给玩家的选择，传递给游戏的命令等所有内容。`Sally.yarn`文件包含上述四项内容：`Sally`，`Sally.Watch`，`Sally.Exit`和`Sally.Sorry`。设置示例游戏后，当您走到Sally旁边并按空格键时，游戏将开始运行`Sally`节点。

2. **转到`Sally`节点**

让我们看下节点都包含什么内容。这是它的全文：

Let's take a look at what that node contains. Here's the entire text of it:

```yarn
<<if visited("Sally") is false>>
    Player: Hey, Sally. #line:794945
    Sally: Oh! Hi. #line:2dc39b
    Sally: You snuck up on me. #line:34de2f
    Sally: Don't do that. #line:dcc2bc
<<else>>
    Player: Hey. #line:a8e70c
    Sally: Hi. #line:305cde
<<endif>>

<<if not visited("Sally.Watch")>>
    [[Anything exciting happen on your watch?|Sally.Watch]] #line:5d7a7c
<<endif>>

<<if $sally_warning and not visited("Sally.Sorry")>>
    [[Sorry about the console.|Sally.Sorry]] #line:0a7e39
<<endif>>
[[See you later.|Sally.Exit]] #line:0facf7
```

花一点时间读一下代码，并理解它的结构。

### 行与逻辑

现在，我们将仔细研究该代码的每个部分，并说明发生了什么。

```yarn
<<if visited("Sally") is false>>
    Player: Hey, Sally. #line:794945
    Sally: Oh! Hi. #line:2dc39b
    Sally: You snuck up on me. #line:34de2f
    Sally: Don't do that. #line:dcc2bc
<<else>>
    Player: Hey. #line:a8e70c
    Sally: Hi. #line:305cde
<<endif>>
```

此节点中的第一行代码检查Yarn Spinner是否已运行此节点。`visited`是此示例游戏已定义的函数——它并未内置在Yarn Spinner中。如果您指定的节点之前已经运行过，它将返回`true`。您会注意到，该行用`<<`和`>>`符号包裹。这告诉Yarn Spinner它是控制代码，而不是要向玩家显示。

<!-- add a tutorial on defining functions -->

如果他们还没有运行`Sally`节点，则意味着这是我们在游戏中第一次与Sally对话。作为结果，我们将运行Sally和玩家角色相遇中的行。否则，我们改为运行一些较短的行。

Yarn Spinner中的每一行只是一小段文本，这些文本将直接发送到游戏中。如何显示文本完全取决于游戏。在示例游戏中，它显示在屏幕顶部。

在每行的行尾，您将会看到`#line`标签。使用此标记，Yarn Spinner可以识别行到多个翻译，如果您不打算将游戏翻译成其他语言，则该标记是可选的。

### 选项

这是代码的下一部分。

```yarn
<<if not visited("Sally.Watch")>>
    [[Anything exciting happen on your watch?|Sally.Watch]] #line:5d7a7c
<<endif>>

<<if $sally_warning and not visited("Sally.Sorry")>>
    [[Sorry about the console.|Sally.Sorry]] #line:0a7e39
<<endif>>
```

在代码的下一部分，我们进行一个检查，如果通过，则添加一个*选项*。选项是玩家可以选择的东西。在本游戏中，它们是玩家可以说的话，但就像行一样，如何处理它们取决于游戏。到达节点末端时，选项会显示给玩家。

这里的前两行测试玩家是否已经运行了节点`Sally.Watch`。如果还没有，则代码会添加一个新选项。选项由`[[`和`]]`包裹。`|`之前的文本向将会展示给玩家，而后面的文本是如果玩家选择该选项将运行的节点的名称。像行一样，选项可以具有用于本地化的行标签。

如果玩家之前*已经*运行过`Sally.Watch`节点，则该代码将不会运行，这意味着不会再次运行该选项。

其余部分与第一部分完成类似的事情：它会进行检查，如果检查通过则添加另一个选项。在本示例中，它将检查变量`$sally_warning`是否为真，以及玩家是否尚未运行`Sally.Sorry`节点。`$ sally_warning`是在另一个节点`Ship`中设置的，该节点存储在文件`Ship.yarn`中。

```yarn
[[See you later.|Sally.Exit]] #line:0facf7
```

节点的最后一行添加了一个选项，该选项将玩家带到`Sally.Exit`行。因为这个选项不在`if`语句中，所以它总是会被添加。

当Yarn Spinner到达节点的末端时，到目前为止已累积的所有选项都将显示给玩家。接着，Yarn Spinner将等待玩家做出选择，然后开始运行他们选择的节点。

这就是节点的工作原理！

## 撰写对话

让我们写一些对话吧！我们将在Ship上添加几行。

1. **打开文件`Ship.yarn`。** 它包含一个名为`Ship`的节点，请转到该节点。

这段代码使用了一些我们在`Sally`中没有看到的功能：*命令*和*变量*。

### 命令

命令是Yarn Spinner发送到您的游戏但并不希望显示给玩家的消息。命令使您可以控制场景中的事件，例如移动摄像机或指示角色移动到另一点。

由于每个游戏都不相同，因此Yarn Spinner把定义大部分命令的权力留给您。Yarn Spinner定义了两个内置命令：`wait`（使对话暂停一定秒数）和`stop`（使对话立即结束）。

该示例游戏定义了自定义的命令`setsprite`，该命令用于更改Ship角色正在显示的脸部精灵。您可以在文件`Ship.yarn`中看到这一点：

```yarn
Player: How's space?
Ship: Oh, man.
<<setsprite ShipFace happy>>
Ship: It's HUGE!
<<setsprite ShipFace neutral>>
```

{{<note>}}
你可以在{{<xref "/docs/unity/working-with-commands">}}中进一步了解如何定义自定义命令。
{{</note>}}

### 变量

变量是您存储有关玩家在游戏中所完成操作的信息的方式。我们在`Sally`节点中看到了变量的用法，使用变量`sally_warning`来控制是否显示某些内容。此变量在此处的`Ship`节点中定义，表示玩家是否已从Ship处听到Sally关于主机的警告。

Yarn Spinner中的变量以`$`开头，其可以存储文本，数字，布尔值（真或假值）或`null`。如果尝试访问未定义的变量，则会得到值`null`，表示“无值”。

### 添加内容

2. **添加一些新对话。** 将以下文本添加到节点的末尾：

```yarn

Ship: Anything else I can help with?

-> No, thanks.
    Ship: Aw, ok!
-> I'm good.
    Ship: Let me know!

Ship: Bye!
```

### 快捷选项

我们刚刚添加的`->`被称为**快捷选项**。快捷选项使您可以在节点中放置选项，而不必创建通过`[[Option]]`语法链接的新节点。它们与节点的其余部分共存.

要使用快捷方式选项，创建一个`->`，然后是要显示的文本。然后，在接下来的几行中，将代码缩进几个空格（保持一致即可）。如果选择了缩进的行所对应的选项，则缩进的行将运行。快捷选项可以嵌套，这意味着您可以将一组快捷选项放在另一个快捷选项中。您可以在快捷选项的行中放入任何类型的代码。

因为快捷选项不需要您创建新节点，所以它们很适用于您希望为玩家提供某种不会显着改变故事流程的选择的情况。

3. **保存文件，并返回游戏。** 再次运行游戏，然后与Ship交谈。在交谈的最后，您将看到新的对话。

## 接下来做什么

示例游戏已经完成设置，当您与Sally对话时，将运行节点`Sally`，而当您与Ship对话时，将运行节点`Ship`。了解这些后，请更改故事，以便在您被Sally告知后，她要求您去解决Ship的问题。

您也可以阅读Yarn Spinner的[语法参考]({{< ref "syntax.md" >}})。


<!-- 
Where to go from here:

* [Customising the UI](ref "customising-ui.md")
* [Creating Yarn Commands](ref "creating-yarn-commands.md")
-->
