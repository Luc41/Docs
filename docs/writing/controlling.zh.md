---
title: "对话控制"
date: 
tags: []
summary: "了解如何控制对话流程。"
draft: false
toc: true
weight: 2
menu: 
    docs:
        parent: "writing"
---

Yarn Spinner旨在使玩家可以更轻松地编写对话的同时控制对话。

在本节中，我们将根据玩家的选择以及游戏的其余部分，探讨控制对话的方法。

## 选项

*选项*是到达节点末端时显示在玩家的行。选项与节点关联； 当您编写选项时，您还将编写在选择该选项时应该运行哪个节点。

当Yarn Spinner遇到一个选项时，它不会立即显示出来。而是将其添加到的一个选项列表中，并保留该列表。

当Yarn Spinner到达节点的末尾时，它会将看到的所有选项发送给游戏，并等待游戏发回玩家的选择。然后，它运行与该选项关联的节点。

例如，请考虑以下来自*Night in the woods*中的代码：

```yarn
Penderson: So you’re back, eh?
Mae: Yes, Mr. Penderson.
Penderson: Didn’t last long, eh?
Mae: No, Mr. Penderson
Penderson: You get a job yet?
[[I’ve only been back for like 24 hours.|BackFor24Hours]]
[[Yes. I’ve been elected mayor.|ElectedMayor]]
```

如果玩家选择了选项“是的，我已经当选市长”，Yarn Spinner将运行节点`ElectedMayor`。 

选项通常位于节点的末端，但不一定必须如此。例如，上面示例中的选项可以放置在代码中的任何位置。这是因为仅在到达节点末尾时才显示选项。 

{{<note>}}
如果到达节点的末尾，并且看不到任何选项，则Yarn Spinner将结束对话。
{{</note>}}

## 快捷选项

快捷选项允许您无需创建其他节点而向玩家显示选项。当您要在节点中间显示一些选项，然后继续运行节点的其余部分时，它们很有用。

快捷选项以`->`（破折号，后跟一个大于号）开头，后跟该选项的文本。 

例如，考虑以下代码：

```yarn
Mae: That was pretty great.
Chazokov: We can look again in two days time.
Chazokov: Will you be back?
-> Yeah if I remember!
-> Probably not.
Chazokov: Oh, you will be.
Chazokov: No one can resist the stars forever.
Mae: That’s spooky, Mr. Chazokov.
```

运行此代码后，选项将出现在“Will you be back”行之后。 然后，Yarn Spinner发送选项“Yeah if I remember!”和“Probably not.”到游戏，然后等待响应。在做出选择后，其余的行开始运行。 

{{<note>}}
快捷选项和常规选项之间的唯一区别是它们的编写方式。从游戏和玩家的角度来看，它们的表现没有什么区别。
{{</note>}}
 
### 快捷选项和行

快捷方式选项可以有自己的行，其在选择该选项时被运行。要编写此代码，请*缩进*属于快捷选项的行。 

在下面的代码中，根据选择的两个快捷选项中的哪一个，将运行不同的行。

```yarn
Mae: What are you doing anyway? 
Chazokov: Hunting dusk stars! 
-> What’s dusk stars? 
    Chazokov: Wandering stars the light of which does not come through at night. 
    Mae: How does that work? 
    Chazokov: It is a trick of the atmosphere and setting sunlight. 
    Chazokov: Only visible for few weeks every year in the spring and fall, so lovely. 
    Mae: Neato. 
-> Dusk Stars is the name of my shoegaze band. 
    Chazokov: Really? 
    Mae: No. 
    Chazokov: It is music of looking at shoes? 
    Mae: With a lot of reverb. 
    Chazokov: Why are we talking about shoes? 
    Mae: I forget. 
```

### 嵌套快捷选项

除了包含行，快捷选项还可以包含*另一个*快捷选项。 

例如，来自*Night in the Woods*的以下代码显示了如何嵌套快捷选项： 

```yarn
Mae: Whatcha readin'?
Mom: Book about a guy who grew up secretly living on a fishing ship
Mom: living in a barrel, eating raw fish, crabs,
Mom: octopus, squid, lobster, gulls, albatross,
    ->I get it.
    ->wow!
        Mom: sharks, dolphins, sea cucumbers,
        Mom: seaweed, sand, rocks,
        Mom: kelp, but that's the same as seaweed i think,
            ->I get the picture.
            ->wow!
            Mom: ropes, sails, one of the boats, the rigging,
            Mom: sailor shoes, sailor hats, sailor pants
            Mom: sailor shirts, sailor underwear (clean),
```                

{{<note>}}
使用嵌套的快捷选项创建非常复杂的嵌套结构很容易，但这会使您的对话难以在编辑器中阅读。如果您的对话开始变得过于复杂，请考虑使用常规[选项](#options)来重写。
{{</note>}}

### 条件判断

您可以在快捷选项的末尾添加*条件判断*，以控制该选项是否出现。为此，请在该行的末尾编写[`if`语句](#if-statements)。

例如，考虑以下代码：

```yarn
Gregg: so what's up?
-> Just saying hello.
    Gregg: coooool.
-> You up for smashing some lightbulbs? <<if $light_bulb_smash_done is 0>>
    [[LightBulbSmash]]
-> Diiiid you wanna check out the historical society? <<if $did_gregg_investigation_quest is 0>>
    Gregg: rock on dude
    [[InvestigationQuest]]
```

在本例中：

* 选项“Just saying hello”因为没有条件判断所以总会显示。
* 选项“You up for smashing some lightbulbs?”只有当变量`$light_bulb_smash_done`值设置为0时才会显示。
* 选项“Diiiid you wanna check out the historical society?”只有当变量`did_gregg_investigation_quest`值设置为0时才会显示。

请注意，与常规的`if`语句不同，这里的条件判断没有在行尾使用`else`或`endif`。

{{<note>}}
在{{<xref "/docs/writing/expressions-and-variables">}}中进一步了解可以在条件判断中使用的表达式。
{{</note>}}

## 跳转

**跳转**是一个立即开始运行另一个节点的指令。 

例如，考虑以下代码：

```yarn
Bea: You know what? Yeah. Let's go.
Mae: Great!
Bea: Yep. Great.
Mae: I promise it'll be great.
[[BeaWeSureWereDoingThis]]
```

当Yarn Spinner到达最终行时，它会立即开始运行节点`BeaWeSureWereDoingThis`。

{{<note>}}
因为跳转直接转移到另一个节点，所以在之后的任何内容都不会运行。
{{</note>}}

## `if`语句

`if`语句使您可以根据一个表达式的结果来控制运行对话的哪些部分。 

例如，考虑以下代码：

```yarn
Gregg: i mean you know what Angus would say
<<if $did_angus_investigation_quest is true>>
    Mae: ha ha yeah
    Mae: pattern seeking
<<else>>
    Mae: what?
    Gregg: he goes off about how people see like patterns they want to see
    Gregg: like it’s in our brains. Our brains do that.
<<endif>>
```

如果变量`$did_angus_investigation_quest`的值置为`true`,则显示行“ha ha yeah, pattern seeking”。否则其他行将会显示。

{{<note>}}
在{{<xref "docs/writing/expressions-and-variables" >}}中进一步了解可以在`if`语句中使用的表达式。
{{</note>}}


### `if`语句和选项

如果一个选项在一个`if`语句的内部，并且`if`语句没有运行，则Yarn Spinner将不会把该选项显示给玩家。例如，考虑以下代码：

```yarn
Mae: Ok kids we're gonna go with...
[[Actually, I'm not sure yet...|NotSure]]
<<if $robot_head_0_done is 1>>
    [[Frog Head|PickRobotMascot0]]
<<endif>>
<<if $robot_head_1_done is 1>>
    [[Pig Head|PickRobotMascot1]]
<<endif>>
<<if $robot_head_2_done is 1>>
    [[Rabbit Head|PickRobotMascot2]]
<<endif>>
```

当节点的末尾运行时，显示的选项将取决于`if`语句中使用的变量的值：

* 第一个选项“Actually, I'm not sure yet”总是会显示。
* 选项“Frog Head”仅在变量`$robot_head_0_done`的值置为`1`时显示。
* 选项“Pig Head”仅在变量`$robot_head_1_done`的值置为`1`时显示。
* 选项“Rabbit Head”仅在变量`$robot_head_2_done`的值置为`1`时显示。




