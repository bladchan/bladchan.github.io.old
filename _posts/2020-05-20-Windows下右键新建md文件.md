---
layout: post
title: 'Windows下右键新建.md文件'
date: 2020-05-20
categories: 技术
tags: 小知识
---

## Windows下右键新建.md文件

每次需要写.md文件时，都需要打开Typora文件的新建选项，或是新建一个文本文档，然后将后缀改为.md。对于一个有强迫症的程序员来说，这种操作是非常麻烦的。为此本页介绍了在Windows操作系统下如何添加右键新建.md文件的方法。

### 环境

Windows 10 操作系统

Typora编辑器

### 步骤

下以Typora编辑器创建为例，其他编辑器修改方法差不多

在磁盘任意处新建txt文档，将下面内容拷贝到txt文档中

```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\.md]
@="Typora.exe"
[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""
[HKEY_CLASSES_ROOT\Typora.exe]
@="Markdown"
```

`@="Typora.exe" `代表的是指定.md文件的运行程序；
`@="Markdown" `代表的是右键时默认的文件名字，这样写新建的文件名为`新建Markdown.md`，同时右键菜单中显示`MarkDown`。

**注意：需要将新建的txt文档另存为.reg后缀的文件，同时需要将编码设置为Unicode，如果不存在Unicode，可选择UTF-16 LE编码方式，如下图：**

![](https://www.bladchan.ml/assets/img/mdsavesetting.png)

### 效果图

![](https://www.bladchan.ml/assets/img/mdresult.png)