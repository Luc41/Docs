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

Yarn Spinner is designed to make it easier to write conversations where the player has control over the conversation. 

在本节中，我们将根据玩家的选择以及游戏的其余部分，探讨控制对话的方法。

In this section, we'll look at the ways you can control what your dialogue does, based on player choices, and on the rest of the game.

## 选项

*选项*是到达节点末端时显示在玩家的行。选项与节点关联； 当您编写选项时，您还将编写在选择该选项时应该运行哪个节点。

An *option* is a line that's shown to the player when the end of a node is reached. Options are associated with nodes; when you write an option, you also write which node should be run when that option is selected.

当Yarn Spinner遇到一个选项时，它不会立即显示出来。而是将其添加到的一个选项列表中，并保留该列表。

When Yarn Spinner encounters an option, it doesn't show it right away. Instead, it adds it to a list of options that it's seen, and keeps that list around. 

当Yarn Spinner到达节点的末尾时，它会将看到的所有选项发送给游戏，并等待游戏发回玩家的选择。然后，它运行与该选项关联的节点。

When Yarn Spinner reaches the end of the node, it sends all of the options it's seen to the game, and waits for the game to send back the player's selection. It then runs the node that that option was associated with. 

例如，请考虑以下来自*Night in the woods*中的代码：

For example, consider the following code from *Night in the Woods*:

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

If the player selected the option "Yes. I've been elected mayor", Yarn Spinner would run the node `ElectedMayor`.

选项通常位于节点的末端，但不一定必须如此。例如，上面示例中的选项可以放置在代码中的任何位置。这是因为仅在到达节点末尾时才显示选项。 

Options are usually at the end of the node, but they don't have to be. For example, the options in the example above could have been placed anywhere in the code. This is because options are only displayed when the end of a node is reached.

{{<note>}}
如果到达节点的末尾，并且看不到任何选项，则Yarn Spinner将结束对话。

If the end of a node is reached, and no options have been seen, Yarn Spinner will end the dialogue.
{{</note>}}

## 快捷选项

快捷选项允许您无需创建其他节点而向玩家显示选项。当您要在节点中间显示一些选项，然后继续运行节点的其余部分时，它们很有用。

Shortcut options let you present options to the player without having to create additional nodes. They're useful for when you want to show some options in the middle of a node, and then resume running the rest of the node.

快捷选项以`->`（破折号，后跟一个大于号）开头，后跟该选项的文本。 

Shortcut options start with a `->` (a dash, followed by a greater-than sign), followed by the text of the option.

例如，考虑以下代码：

For example, consider the following code:

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

When this code is run, options will appear after the line "Will you be back?". Yarn Spinner then sends the options "Yeah if I remember!" and "Probably not" to the game, and waits for a response. After a choice is made, the remaining lines start being run.

{{<note>}}
快捷选项和常规选项之间的唯一区别是它们的编写方式。从游戏和玩家的角度来看，它们的表现没有什么区别。 

The only difference between shortcut options and regular options is the way they're written. From the perspective of the game, and of the player, there's no difference in how they appear.
{{</note>}}
 
### 快捷选项和行

Shortcut options can have their own lines, which are run when the option is selected. To write this, *indent* the lines that belong to a shortcut option.

In the following code, different lines will run based on which of the two shortcut options are selected.

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

### Nested shortcut options

In addition to containing lines, shortcut options can also contain *other* shortcut options. 

For example, this code from *Night in the Woods* shows how shortcut options can be nested:

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
It's easy to create very complicated nested structures with nested shortcut options, and this can make your dialogue difficult to read in the editor. If your dialogue starts looking too complex, consider rewriting it to use regular [options](#options) instead.
{{</note>}}

### Conditionals

You can add a *conditional* to the end of a shortcut option, which controls whether the option will appear or not. To do this, you write an [`if` statement](#if-statements) at the end of the line.

For example, consider the following code:

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

In this example:

* The option "Just saying hello" will always appear, because it has no conditional. 
* The option "You up for smashing some lightbulbs?" will appear only if the variable `$light_bulb_smash_done` is set to `0`.
* The option "Diiiid you wanna check out the historical society?" will appear only if the variable `$did_gregg_investigation_quest` is set to `0`.

Note that unlike a regular `if` statement, there's no `else` or `endif` used at the end of the line.

{{<note>}}
To learn more about the expressions you can use in a conditional, see {{<xref "/docs/writing/expressions-and-variables">}}.
{{</note>}}

## Jumps

A **jump** is an instruction to immediately start running another node. 

For example, consider the following code:

```yarn
Bea: You know what? Yeah. Let's go.
Mae: Great!
Bea: Yep. Great.
Mae: I promise it'll be great.
[[BeaWeSureWereDoingThis]]
```

When Yarn Spinner reaches the final line, it will immediately start running the node `BeaWeSureWereDoingThis`.

{{<note>}}
Because a jump moves right to another node, any content after it will not be run.
{{</note>}}

## `if` statements

An `if` statement lets you control which parts of your dialogue are run, based on the result of an expression.

For example, consider the following code:

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

If the variable `$did_angus_investigation_quest` is set to `true`, the lines "ha ha yeah, pattern seeking" will appear. Otherwise, the other lines would appear.

{{<note>}}
To learn more about the expressions you can use inside `if` statements, see {{<xref "docs/writing/expressions-and-variables" >}}.
{{</note>}}


### `if` statements and Options

If an option is inside an `if` statement, and that `if` statement is not run, Yarn Spinner won't display that option to the player. For example, consider the following code:

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

When the end of the node is run, the options that are displayed will depend upon the value of variables used in the `if` statements:

* The first option "Actually, I'm not sure yet" will always appear.
* The option "Frog Head" will only appear if the variable `$robot_head_0_done` is set to the value `1`.
* The option "Pig Head" will only appear if the variable `$robot_head_1_done` is set to the value `1`.
* The option "Rabbit Head" will only appear if the variable `$robot_head_2_done` is set to the value `1`.




