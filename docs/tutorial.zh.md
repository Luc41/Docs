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

In this section of the tutorial, we're going to open the file `Sally.yarn`, and look at what it's doing.

1. **在您选择的编辑器中打开`Sally.yarn`。**

Yarn Spinner将其所有的对话分组为**节点**。节点包含：您的对话行，展示给玩家的选择，传递给游戏的命令等所有内容。`Sally.yarn`文件包含上述四项内容：`Sally`，`Sally.Watch`，`Sally.Exit`和`Sally.Sorry`。设置示例游戏后，当您走到Sally旁边并按空格键时，游戏将开始运行`Sally`节点。

Yarn Spinner groups all of its dialogue into **nodes**. Nodes contain everything: your lines of dialogue, the choices you show to the player, and the commands that you send to the game. The `Sally.yarn` file contains four of them:  `Sally`, `Sally.Watch`, `Sally.Exit`, and `Sally.Sorry`. The example game is set up so that when you walk up to Sally and press the spacebar, the game will start running the `Sally` node. 

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

Take a second now to look at this code, and get a feel for its structure.

### 行与逻辑

现在，我们将仔细研究该代码的每个部分，并说明发生了什么。

We'll now take a closer look at each part of this code, and explain what's going on.

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

The first line of code in this node checks to see if Yarn Spinner has already run this node. `visited` is a function that this example game has defined - it isn't built into Yarn Spinner. It returns `true` if the node you specify has been run before. You'll notice that this line is wrapped in `<<` and `>>` symbols. This tells Yarn Spinner that it's control code, and not meant to be shown to the player.

<!-- add a tutorial on defining functions -->

If they haven't run the `Sally` node yet, it means that this is the first time that we've spoken to Sally in this game. As a result, we run lines in which Sally and the player character meet. Otherwise, we instead run some shorter lines.

Each line in Yarn Spinner is just a run of text, which is sent directly to the game. It's up to the game to decide how it wants to display it; in the example game, it's shown at the top of the screen.

At the end of each line, you'll see a `#line:` tag. This tag lets Yarn Spinner identify lines across multiple translations, and is optional if you aren't translating your game into other languages. Yarn Spinner can automatically generate them for you.

### Options

Here's the next part of the code.

```yarn
<<if not visited("Sally.Watch")>>
    [[Anything exciting happen on your watch?|Sally.Watch]] #line:5d7a7c
<<endif>>

<<if $sally_warning and not visited("Sally.Sorry")>>
    [[Sorry about the console.|Sally.Sorry]] #line:0a7e39
<<endif>>
```

In the next part of the code, we do a check, and if it passes, we add an *option*. Options are things that the player can select; in this game, they're things the player can say, but like lines, it's up to the game to decide what to do with them. Options are shown to the player when the end of a node is reached.

The first couple of lines here test to see whether the player has run the node `Sally.Watch`. If they haven't, then the code adds a new option. Options are wrapped with `[[` and `]]`. The text before the `|` is shown to the player, and the text after is the name of the node that will be run if the player chooses the option. Like lines, options can have line tags for localisation.

If the player *has* run the `Sally.Watch` node before, this code won't be run, which means that the option to run it again won't appear.

The rest of this part does a similar thing as the first: it does a check, and adds another option if the check passes. In this case, it checks to see if the variable `$sally_warning` is true, and if the player has not yet run the `Sally.Sorry` node. `$sally_warning` is set in a different node - it's in the node `Ship`, which is stored in the file `Ship.yarn`.

```yarn
[[See you later.|Sally.Exit]] #line:0facf7
```

The very last line of the node adds an option, which takes the player to the `Sally.Exit` line. Because this option isn't inside an `if` statement, it's always added.

When Yarn Spinner hits the end of the node, all of the options that have been accumulated so far will be shown to the player. Yarn Spinner will then wait for the player to make a selection, and then start running the node that they selected.

And that's how the node works!

## Writing Some Dialogue

Let's write some dialogue! We'll add a couple of lines to the Ship.

1. **Open the file `Ship.yarn`.** It contains a single node, called `Ship` - go to it.

This code uses couple of features that we didn't see in `Sally`: *commands*, and *variables*. 

### Commands

Commands are messages that Yarn Spinner sends to your game, but aren't intended to be shown to the player. Commands let you control things in your scene, like moving the camera around, or instructing a character to move to another point. 

Because every game is different, Yarn Spinner leaves the task of defining most commands to you. Yarn Spinner defines two built-in commands: `wait`, which pauses the dialogue for a certain number of seconds, and `stop`, which ends the dialogue immediately.

The example game defines its own command, `setsprite`, which is used to change the sprite that the Ship character's face is displaying. You can see this in action in the file `Ship.yarn`:

```yarn
Player: How's space?
Ship: Oh, man.
<<setsprite ShipFace happy>>
Ship: It's HUGE!
<<setsprite ShipFace neutral>>
```

{{<note>}}
You can learn how to define your own custom commands in {{<xref "/docs/unity/working-with-commands">}}.
{{</note>}}

### Variables

Variables are how you store information about what the player has done in the game. We saw variables in use in the `Sally` node, where the variable `$sally_warning` was used to control whether some content was shown or not. This variable is set in here, in the `Ship` node - it represents whether or not the player has heard Sally's warning about the console from the Ship.

Variables in Yarn Spinner start with a `$`, and can store text, numbers, booleans (true or false values), or `null`. If you try and access a variable that hasn't been set, you'll get the value `null`, which represents "no value".

### Adding Some Content

2. **Add some new dialogue.** Add the following text to the end of the node:

```yarn

Ship: Anything else I can help with?

-> No, thanks.
    Ship: Aw, ok!
-> I'm good.
    Ship: Let me know!

Ship: Bye!
```

### Shortcut Options

The `->` items that we just added are called **shortcut options**. Shortcut options let you put choices in your node without having to create new nodes, which you link to through the `[[Option]]` syntax. They exist in-line with the rest of your node.

To use a shortcut option, you write a `->`, followed by the text that you want to display. Then, on the next lines, indent the code a few spaces (it doesn't matter how many, as long as you're consistent.) The indented lines will run if the option they're attached to is selected. Shortcut options can be nested, which means you can put a group of shortcut options inside another. You can put any kind of code inside a shortcut option's lines.

Because shortcut options don't require you to create new nodes, they're really good for situations where you want to offer the player some kind of choice that doesn't significantly change the flow of the story.

3. **Save the file, and go back to the game.** Play the game again, and talk to the Ship. At the end of the conversation, you'll see new dialogue.

## Where Next

The example game is set up so that when you talk to Sally, the node `Sally` is run, and when you talk to the Ship, the node `Ship` is run. With this in mind, change the story so that after you get told off by Sally, she asks you to go and fix a problem with the Ship.

You can also read the [Syntax Reference]({{< ref "syntax.md" >}}) for Yarn Spinner.


<!-- 
Where to go from here:

* [Customising the UI](ref "customising-ui.md")
* [Creating Yarn Commands](ref "creating-yarn-commands.md")
-->
