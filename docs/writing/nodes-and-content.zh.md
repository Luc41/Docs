---
title: "节点和内容"
date: 
tags: []
summary: "了解Yarn文件中的内容，以及Yarn Spinner如何安排其内容。"
draft: false
toc: true
weight: 1
menu: 
    docs:
        parent: "writing"
        weight: 1
---

在Yarn Spinner中，所有的对话都保存在`.yarn`文件中。您可以使用[Yarn编辑器]({{< ref "yarn-editor" >}})，或者[纯文本编辑器]({{< ref "text-editor" >}})来创建这些文件。

In Yarn Spinner, all of your dialogue is stored in `.yarn` files. You can create these files using the [Yarn Editor]({{< ref "yarn-editor" >}}), or with a [plain text editor]({{< ref "text-editor" >}}).

## 节点

Yarn Spinner文件包含**节点**。每个节点至少具有一个标题和一个正文。标题是节点的名称，正文则是包含游戏的对话的Yarn脚本。

Yarn Spinner files contain **nodes**. Each node has, at the very minimum, a title and a body. The title is the name of the node, and the body contains the Yarn script that contains your game's dialogue. 

节点的标题很重要，因为游戏使用它来向Yarn Spinner指示要开始运行的节点，并在节点之间建立连接，例如[选项]({{< relref "#options" >}})。

The title of a node is important, because it's used by your game to indicate to Yarn Spinner which node to start running, and to make connections between nodes, such as with [options]({{< relref "#options" >}}).

节点标题不会展示给玩家，并且不能包含空格。

Node titles are not shown to the player, and they cannot contain spaces.

## 节点内容

节点的正文由三个不同的内容构成：*行*，*命令*，和*选项*。

The body of a node is made up of three different kinds of content: *lines*, *commands*, and *options*.

### 行

当您编写Yarn Spinner对话时，在节点中编写的几乎每一行文本都是**行**。当运行节点时，它每次运行一行，然后将其发送到您的游戏。

When you write Yarn Spinner dialogue, just about every line of text that you write in a node is a **line**. When a node is run, it runs each line, one at a time, and sends it to your game.

例如，考虑以下来自*Night in the Woods*中的代码：

For example, consider the following Yarn code from *Night in the Woods*:

```yarn
Mae: Well, this is great.
Mae: I mean I didn't expect a party or anything
Mae: but I figured *someone* would be here.
Mae: ...
Mae: Welcome home, Mae.
```

当此代码在游戏中运行时，它看起来像这样：

When this code is run in the game, it looks like this:

{{< video poster="lines-poster.png" mp4="lines.mp4" webm="lines.webm" loop="true" autoplay="true" controls="true" >}}

Yarn Spinner一次将这些行中的一行发送到游戏中。游戏负责获取文本并将其呈现给玩家； 在*Night in the Woods*的情况中，这意味着绘制气泡，为每个字母添加动画效果，并等待用户按下按键以前进到下一行。

Yarn Spinner sends each of these lines, one at a time, to the game. The game is responsible for taking the text, and presenting it to the player; in the case of *Night in the Woods*, this means drawing the speech bubble, animating each letter in, and waiting for the user to press a key to advance to the next line.

### 命令

A **command** is an instruction that tells your game to do something. Commands are like stage directions in a screenplay: they aren't shown directly to the player, like a line would be, but instead can be used to tell the game to perform some other kind of action. 

Commands start with a `<<` (two less-than symbols), and end with a `>>` (two greater-than symbols). They can contain whatever text you like; when Yarn Spinner encounters them, it passes the contents of the command directly to the game.

{{<note>}}
The `<<` and `>>` symbols are also used in other parts of the Yarn language. A line that's wrapped in `<<` and `>>` is considered a command if it doesn't begin with an a keyword like `if` or `set`.
{{</note>}}

For example, consider the following code from *Night in the Woods*, which makes used of a large number of commands:

```yarn
Mae: When do you think that door's gonna be finished? 

// Wait a moment before replying
<<close>>
<<wait 1>>
Janitor: Now. 
<<close>>
<<wait .75>>
Janitor: Goodbye. 

// Walk the Janitor out of the scene
<<close>>
<<flip Janitor -1>>
<<walk Janitor ExitLeft>>
<<wait 1>>

// Play a sound effect
<<playOneShot event:/bus_station/bus_station_door BusStationDoor>>

// Turn the lights off when the Janitor has finished moving
<<waitForMove Janitor>>
<<hide Janitor>>
<<setSceneMood LightsOff>>
<<closeAll>>

Mae: uh. bye. 
```

This code, in addition to the four lines of dialogue that appear, contains a number of commands that control things like moving a character around and changing the lights in the scene.

{{< video mp4="commands.mp4" webm="commands.webm" autoplay="true" loop="true" controls="true" >}}

Commands are defined by your game, not by Yarn Spinner, because Yarn Spinner doesn't know what kinds of stage directions your game needs to handle. To learn how to define your own commands, see [Working with Commands]({{< ref "docs/unity/working-with-commands">}}).

### Options

When you want to let the player decide what to say, you use an **option**. Options let you show multiple potential lines of dialogue to the player, and let the player select one; once they do this, Yarn Spinner will show different content depending on the player's choice.

For example, consider the following three nodes from *Night in the Woods*:

{{< img "options.png" "Three nodes: \"excuse\", \"something\" and \"someone\"."  >}}

Let's look at the code for the first node:

```yarn
Mae: Excuse me, but where is everybody? 
Janitor: It's 10:45. It's closed. 
Janitor: Not a lot of folks getting off the last bus to Possum Springs these days. 
Janitor: Just you. 
[[Isn't there supposed to be someone at the desk?|someone]] 
[[So are you the Janitor or something?|something]] 
```

This code causes Yarn Spinner to show options to the player. When Yarn Spinner encounters an option line, which starts with two square brackets (`[[`), it adds it to a list of options to show. 

When the end of a node is reached, all of the options that have been seen so far are sent to the game, which then presents them to the player and waits for a selection. 

When the player makes a selection, the game sends their choice back to Yarn Spinner, which then starts running the specified node. 


{{< video mp4="options.mp4" webm="options.webm" autoplay="true" loop="true" controls="true" >}}

For example, in the above video, the player chose the option "Isn't there supposed to be someone at the desk?", which links to the node `someone`. Yarn Spinner then started running the `someone` node.

If a node ends, and no options have been seen, the dialogue ends.
