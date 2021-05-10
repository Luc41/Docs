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

## 节点

Yarn Spinner文件包含**节点**。每个节点至少具有一个标题和一个正文。标题是节点的名称，正文则是包含游戏的对话的Yarn脚本。

节点的标题很重要，因为游戏使用它来向Yarn Spinner指示要开始运行的节点，并在节点之间建立连接，例如[选项]({{< relref "#options" >}})。

节点标题不会展示给玩家，并且不能包含空格。
## 节点内容

节点的正文由三个不同的内容构成：*行*，*命令*，和*选项*。

### 行

当您编写Yarn Spinner对话时，在节点中编写的几乎每一行文本都是**行**。当运行节点时，它每次运行一行，然后将其发送到您的游戏。

例如，考虑以下来自*Night in the Woods*中的代码：

```yarn
Mae: Well, this is great.
Mae: I mean I didn't expect a party or anything
Mae: but I figured *someone* would be here.
Mae: ...
Mae: Welcome home, Mae.
```

当此代码在游戏中运行时，它看起来像这样：

{{< video poster="lines-poster.png" mp4="lines.mp4" webm="lines.webm" loop="true" autoplay="true" controls="true" >}}

Yarn Spinner一次将这些行中的一行发送到游戏中。游戏负责获取文本并将其呈现给玩家； 在*Night in the Woods*的情况中，这意味着绘制气泡，为每个字母添加动画效果，并等待用户按下按键以前进到下一行。

### 命令

**命令**是告诉您的游戏执行某项操作的指令。
命令就像电影剧本中的舞台指导：它们不会像行一样直接显示给玩家，而是可以用来告诉游戏执行其他某种动作。

命令以`<<`（两个小于号）开头，并以`>>`（两个大于号）结尾。它们可以包含您喜欢的任何文本；当Yarn Spinner遇到它们时，它将命令的内容直接传递给游戏。

{{<note>}}
`<<`和`>>`符号也用于Yarn语言的其他地方。如果换行不是以`if`或`set`之类的关键字开头，则用`<<`和`>>`包裹的行将被视为命令。
{{</note>}}

例如，考虑以下来自*Night in the Woods*的代码，该代码使用了大量命令。

```yarn
Mae: When do you think that door's gonna be finished? 

// 在回复前等待一段时间
<<close>>
<<wait 1>>
Janitor: Now. 
<<close>>
<<wait .75>>
Janitor: Goodbye. 

// 令看门人走出场景
<<close>>
<<flip Janitor -1>>
<<walk Janitor ExitLeft>>
<<wait 1>>

// 播放音效
<<playOneShot event:/bus_station/bus_station_door BusStationDoor>>

// 当看门人完成移动时关闭灯光
<<waitForMove Janitor>>
<<hide Janitor>>
<<setSceneMood LightsOff>>
<<closeAll>>

Mae: uh. bye. 
```

除了出现的四行对话之外，此代码还包含许多命令，这些命令控制着诸如移动角色和改变场景中的灯光之类的事情。

{{< video mp4="commands.mp4" webm="commands.webm" autoplay="true" loop="true" controls="true" >}}

命令是由您的游戏定义的，而非Yarn Spinner，因为Yarn Spinner不知道您的游戏需要处理哪种舞台指导。在[Working with Commands]({{< ref "docs/unity/working-with-commands">}})中进一步了解如何自定义命令。

### 选项

当您想让玩家决定说什么时，您可以使用**选项**。选项可让您向玩家显示多条潜在的对话行，并让玩家选择其中一条；一旦他们这样做，Yarn Spinner将根据玩家的选择显示不同的内容。

例如，考虑*Night in the Woods*中的以下三个节点：

{{< img "options.png" "Three nodes: \"excuse\", \"something\" and \"someone\"."  >}}

让我们看一下第一个节点的代码：

```yarn
Mae: Excuse me, but where is everybody? 
Janitor: It's 10:45. It's closed. 
Janitor: Not a lot of folks getting off the last bus to Possum Springs these days. 
Janitor: Just you. 
[[Isn't there supposed to be someone at the desk?|someone]] 
[[So are you the Janitor or something?|something]] 
```

此代码使Yarn Spinner向玩家显示选项。当Yarn Spinner遇到以两个方括号（`[[`）开头的选项行时，会将其添加到要显示的选项列表中。

到达节点的末端时，到此为止所有可见的选项都将发送到游戏，然后将其呈现给玩家并等待选择。

当玩家做出选择后，游戏会将其选择发送回Yarn Spinner，然后由Yarn Spinner开始运行指定的节点。


{{< video mp4="options.mp4" webm="options.webm" autoplay="true" loop="true" controls="true" >}}

例如，在上面的视频中，玩家选择了选项"Isn't there supposed to be someone at the desk?"，该选项链接到节点`someone`。则Yarn Spinner接下来开始运行`someone`节点。

如果节点结束，并且没有看到任何选项，则对话结束。
