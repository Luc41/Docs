---
title: "使用文本编辑器"
date: 
tags: []
summary: "了解如何使用诸如Visual Studio Code或Sublime Text等文本编辑器来编写Yarn Spinner内容。"
draft: false
toc: true
dont_show_in_lists: true
weight: 7
menu: 
    docs:
        parent: "editor"
---

Yarn文件是纯文本文件，这意味着您可以使用任何您喜欢的文本编辑器来编写它们。如果您要使用与游戏其他部分相同的编辑器或集成开发环境，这可能会很有用。

{{< note >}}
如果您使用[Visual Studio Code](https://code.visualstudio.com)，我们提供一个[扩展](https://marketplace.visualstudio.com/items?itemName=SecretLab.yarn-spinner)来添加对Yarn Spinner的支持。
{{< /note >}}

## 创建一个新的 `.yarn` 文件

要在文本编辑器中创建一个新的`.yarn`文件，请创建一个新的空文件，并向其中添加以下文本：

```yarn
title: Start
---
Hello, world!
===
```

该示例文件包含一个名为`Start`的节点，该节点本身仅包含一行（"Hello，world!"）。

将该文件另存为（例如）`Demo.yarn`。该文件现在可以在您的游戏中使用了。

## Yarn Spinner 文件结构

在`.yarn`文件中，节点分为两部分：*标头*和*正文*。标头包含有关节点的信息，而主体包含该节点的实际文本。

至少需要一个标头：`title`标头，它定义节点的名称。

如果您的节点具有标签，它们也将存储在标头中。例如，以下节点具有两个标签，即`tag1`和`tag2`：

```yarn
title: Start
tags: tag1 tag2
```

标头与正文以`---`（三个破折号）分隔。 

正文可以包含任意多的行，直到包含`===`（三个等于号）的行；这表示节点的末尾。在这之后可以有另一个节点，同样始于标头。

## 使用其他编辑器进行工作

`.yarn`文件可以被任意编辑器打开和修改。如果您在一个文本编辑器中创建了文件，您也可以使用[Yarn Editor]({{< ref "yarn-editor" >}})来打开它，反之同理。

某些编辑器可能会向节点添加其他标题。例如，使用Yarn Editor会包含用于定义节点位置的其他标头。