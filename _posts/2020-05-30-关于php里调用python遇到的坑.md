---
layout: post
title: '关于php里调用python遇到的坑'
date: 2020-05-30
categories: 技术
tags: 笔记
---

# 关于php里调用python遇到的坑

###  前言：

问题起因是在进行大创项目、信息安全竞赛展示时，由于功能实现用的是python脚本，而后端想用php去实现，因此就需要用php调用python，在此过程中需要了很多坑，本文记录了这些问题及对应的解决办法。

### 环境：

centos7.0

php5.6.3

python 2.7/3.6

### 预备知识：

修改Centos7默认Python版本

首先要安装两个版本的Python，Centos自带python2.7，可满足python2.x系列的语法，但针对python3.x系列则不行，如果想要在命令行运行python3.x，有两种办法：

一是直接运行`python3.x xxx.py`

二是通过修改软连接：

```cmd
mv /usr/bin/python /usr/bin/python.bak
ln -s /usr/local/bin/python3.X /usr/bin/python
mv /usr/bin/pip /usr/bin/pip.bak
ln -s /usr/local/bin/pip3.X /usr/bin/pip
```

然后就可以直接运行`python xxx.py`

注：这样修改会导致yum等命令无法使用，因为yum命令调用的是python指向的是python2.7，而修改软连接后，python指向了python3.x，因此还需要修改相关配置才能使用这些命令。

### 排错方法：

php调用python出错后进行排错的方法：

**0.首先要确认php.ini相关函数如exec、system等是否被禁用**

php调用python出错通常是两个问题：

1.权限问题

因为php进程的用户是www，而不是root，python脚本的路径如果与php在同一目录下，那不会存在问题；如果不在同一目录，用相对路径去表示，如果相对路径权限不够，将找不到该文件，因此需要设置文件权限使www用户能够访问到python脚本文件；

2.python执行报错

由于在php中调用python时，出错将看不到错误信息。如果需要将错误信息打印出来，则需要在命令行语句后面添加2>&1，2>&1的作用是将出错信息也输出，为调试提供点信息，否则在php调用python时没有提示错误，将很难debug。如exec函数，执行`exec("python xxx.py 2>&1",$output)`，将在output中返回debug信息。然后根据python报错信息进行排错即可。

### 遇到的问题：

Centos 7中python库matplotlib报错，报错信息为

>_tkinter.TclError: no display name and no $DISPLAY environment variable

这是由于matplotlib的backend设置问题造成的。

解决办法：

1.matplotlibrc文件中的backend配置，将backend参数设置为Agg，

```
backend : Agg
```

2.shell脚本中导入MPLBACKEND环境变量

```
export MPLBACKEND=Agg
MPLBACKEND=Agg python xxx.py
```

