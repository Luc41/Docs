---
title: "使用 Yarn Editor"
date: 
tags: []
summary: "了解如何使用Yarn Editor（一个在Yarn Spinner中用于编写对话的可视化工具）进行工作。-=-"
draft: false
toc: true
dont_show_in_lists: true
weight: 5
menu: 
    docs:
        parent: "editor"        
---

[Yarn Editor](https://github.com/YarnSpinnerTool/YarnEditor)是一个用于为Yarn Spinner编写对话的可视化工具。它是免费和开源的，并且可以在Windows和macOS上运行。您也可以在浏览器中使用Yarn Editor。

The [Yarn Editor](https://github.com/YarnSpinnerTool/YarnEditor) is a visual tool for writing dialogue for Yarn Spinner. It's free and open source, and works on Windows and macOS. You can also use the Yarn Editor in the browser.

## 获取 Yarn Editor

要下载Yarn编辑器，请转到项目的[发布页面](https://github.com/YarnSpinnerTool/YarnEditor/releases/latest)，然后根据您的操作系统下载该应用程序的版本。

To download the Yarn Editor, go to the project's [releases page](https://github.com/YarnSpinnerTool/YarnEditor/releases/latest), and download the version of the app based on your OS.

* 对于Windows用户，请下载.exe。
* 对于macOS用户，请下载.dmg。

要在浏览器中使用Yarn Editor，请访问[yarnspinnertool.github.io/YarnEditor](https://yarnspinnertool.github.io/YarnEditor/)。Yarn Editor支持谷歌Chorme，火狐以及Safari浏览器。

To use the Yarn Editor in the browser, go to [yarnspinnertool.github.io/YarnEditor](https://yarnspinnertool.github.io/YarnEditor/). The Yarn Editor supports Google Chrome, Firefox, and Safari.

启动Yarn Editor后，它将以一个包含一个节点的新文档开始。

Once you've launched the Yarn Editor, it will start with a new document, which contains one node.

{{< img "yarn-editor.png" "The Yarn Editor" >}}

## 打开和保存文档

要将您的工作保存为`.yarn`文件，请将鼠标光标移至`FILE`菜单上方，然后选择`SAVE AS YARN`。

To save your work as a `.yarn` file, move your mouse cursor over the `FILE` menu, and choose `SAVE AS YARN`. 

{{< note >}}
虽然Yarn Editor允许以其他格式保存，但Yarn Spinner仅适用于`.yarn`格式。

While the Yarn Editor allows saving in other formats, Yarn Spinner will only work with `.yarn` format. 
{{< /note >}}


要打开`.yarn`文件，请将鼠标光标移到`FILE`菜单上，然后选择`OPEN`。

To open a `.yarn` file, move your mouse cursor over the `FILE` menu, and choose `OPEN`. 

## 创建节点

要创建一个节点，单击`+ NODE`按钮。新节点将添加到文档中。

To create a node, click the `+ NODE` button. A new node will be added to the document.

## 移除节点

要从文档中删除节点，请将鼠标移到该节点上，然后将出现删除按钮。单击它，该节点将被删除。

To remove a node from the document, move your mouse over it, and the Delete button will appear. Click on it, and the node will be deleted.

## 在节点视图中移动

要更改您正在查看文档的哪一部分，可以四处移动视图，以及放大和缩小。

To change which part of your document you're looking at, you can move the view around, and zoom in and out.

* 要放大和缩小，请滚动鼠标的滚轮。
* 要移动视图，请按住Option键（在Windows中为Alt键），然后单击并用鼠标左键拖动。您也可以使用鼠标中键单击并拖动。

## 排列节点

要四处移动节点，请用鼠标左键单击并拖动节点。节点将四处移动。

To move nodes around, click and drag on a node with the left mouse button. The node will move around.

要同时移动多个节点，请在节点视图中的任何空白点上单击鼠标左键并拖动，然后拖动以创建一个矩形。释放鼠标按钮时，将选择矩形中的所有节点；当您单击并拖动这些节点中的任何一个时，它们都将一起移动。在任何节点外部单击以取消选择所有节点。

To move multiple nodes around at the same time, click and drag with the left mouse button on any empty point in the node view, and drag to create a rectangle. When you release the mouse button, any node in the rectangle will be selected; when you click and drag any of these nodes, all of them will move together. Click outside any node to de-select all nodes.

## 编辑节点

要编辑节点的内容，请双击该节点。将会出现编辑视图。

To edit the contents of a node, double-click on it. The editing view will appear.

{{< img  "yarn-editor-editing-node.png" "Editing a node in the Yarn Editor" >}}

在编辑视图中，您可以修改节点的*title*，*tags*和*body*。完成后，在编辑视图外部单击，它将关闭。

In the editing view, you can modify the *title*, *tags* and *body* of a node. When you're done, click outside the editing view, and it will close.