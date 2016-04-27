title:  VIM使用教程
date: 2016-03-21 17:30:00
description: VIM快捷键和操作的使用
categories:
- VIM
tags:
- tutorial
- tool
toc: true
author: Louis Wang
comments:
original:
permalink: 
---

VIM 使用教程
========

[文章转载自：](http://beiyuu.com/git-vim-tutorial/) 
[发现一个很不错的VIM使用教程可以参考：](http://coolshell.cn/articles/5426.html)

** 第一次使用VIM **，会觉得无所适从，他并不像记事本，你敲什么键就显示什么，理解VIM的需要明白他的两种模式： - 命令模式 (Command Mode) - 编辑模式 (Insert Mode)

命令模式下，可以做移动、编辑操作；编辑模式则用来输入。键入i,o,s,a等即可进入编辑模式，后面解释原因。

模式的设计是VIM和其他编辑器最不同的地方，优势和劣势也全基于此而生。

## 基本操作(以下介绍的键盘操作，都是大小写敏感的，并且要在命令模式下完成，需注意)：

### 以字为单位的移动
> - h 向左移动一个字
> - j 向下移动一行
> - k 向上
> - l 向右

这四个键在右手最容易碰到几个位置，最为常用。

### 以词为单位的移动：
> - w 下一個word w(ord)
> - W 下一個word(跳过标点)
> - b 前一個word b(ackward)
> - B 前一个word(跳过标点)
> - e 跳到当前word的尾端 e(nd)

### 行移动：
> - 0 跳到当前行的开头
> - ^ 跳到当前行第一个非空字符
> - $ 跳到行尾
助记：0(第0个字符),^和$含义同正则表达式

### 段落移动：
> - { 上一段(以空白行分隔)
> - } 下一段(以空白行分隔)
> - % 跳到当前对应的括号上(适用各种配对符号)

### 跳跃移动：
> - /xxxx 搜索xxxx，然后可以用n下一个，N上一个移动
> - # 向前搜索光标当前所在的字
> - * 向后搜索光标当前所在的字
> - fx 在当前行移动到光标之后第一个字符x的位置 f(ind)x
> - gd 跳到光标所在位置词(word)的定义位置 g(o)d(efine)
> - gg 到文档顶部
> - G 到文档底部
> - :x 跳到第x行(x是行号)
> - ctrl+d 向下翻页 d(down)
> - ctrl+u 向上翻页 u(p)

## 基本编辑

### 修改
> - i 在光标当前位置向前插入 i(nsert)
> - I 在本行第一个字符前插入
> - a 在光标当前位置向后插入 a(fter)
> - A 在本行末尾插入
> - o 向下插入一行
> - O 向上插入一行
> - :w 保存
> - :q 退出
> - :wq 保存并退出

### 删除
> - x 删除当前字符
> - dd 删除当前行 d(elete)
> - dw 删除当前光标下的词 d(elete)w(ord)

### 复制粘贴
> - yy 复制当前行 y(ank)
> - yw 复制当前光标下的词 y(ank)w(ord)
> - p 粘贴 p(aste)
> - P 粘贴在当前位置之前

## 进阶操作
限于篇幅，在这里我仅介绍下我非常常用的几个操作。
### 重复操作
因为VIM所有的操作都是原子化的，所以把这些操作程序化就非常简单了：

> - 5w 相当于按五次w键；
> - 6j 下移6行，相当于按六次j；
> - 3J 大写J,本来是将下一行与当前行合并，加上数量，就是重复操作3次；
> - 6dw和d6w 结果是一样，就是删除6个word；
> - 剩下的无数情况，自己类推吧。

### 高效编辑
> - di" 光标在""之间，则删除""之间的内容
> - yi( 光标在()之间，则复制()之间的内容
> - vi[ 光标在[]之间，则选中[]之间的内容
> - 以上三种可以自由组合搭配，效率奇高，i(nner)
> - dtx 删除字符直到遇见光标之后的第一个x字符
> - ytx 复制字符直到遇见光标之后的第一个x字符

### 标记和宏(macro)
> - ma 将当前位置标记为a，26个字母均可做标记，mb、mc等等；
> - 'a 跳转到a标记的位置；
> - 这是一组很好的文档内标记方法，在文档中跳跃编辑时很有用；
> - qa 将之后的所有键盘操作录制下来，直到再次在命令模式按下q，并存储在a中；
> - @a 执行刚刚记录在a里面的键盘操作；
> - @@ 执行上一次的macro操作；
宏操作是VIM最为神奇的操作之一，需要慢慢体会其强大之处；
VIM的基本操作，可以挖掘的东西非常多，不仅仅需要记忆，更需要自己去探索总结，熟练之后，效率会大幅度提升。后面会给出一些参考链接。

## Mac下面使用一些技巧记录：
### 如果想在终端使用macvim,可以如下面配置:
```
sudo port install MacVim
alias vi='open -a MacVim'
alias vim='/Applications/MacPorts/MacVim.app/Contents/MacOS/Vim'

这样在命令行默认敲 vi 就会使用 gui 版本，敲 vim 就使用终端版

其中第一个 alias 也可以改为
alias vi='/Applications/MacPorts/MacVim.app/Contents/MacOS/Vim -g'
```

## VIM 中文显示乱码解决  
### 这里所说的都是全局设定，打开vimrc文件后，只需要在文件最后添加以下代码就可以了：
```
set fileencodings=utf-8,gb2312,gbk,gb18030
set termencoding=utf-8
set fileformats=unix
set encoding=prc
```

