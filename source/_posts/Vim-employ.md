title:  Git 使用问题
date: 2016-03-18 11:30:00
description: Git 使用过程中碰到的问题
categories:
- Git
tags:
- 知识
- 问题
toc: true
author: Louis Wang
comments:
original:
permalink: 
---

Git 使用和问题总结 
===========
          
## 1.解决github push错误The requested URL returned error: 403 Forbidden while accessing  
[参考链接](http://houzhiqingjava.blog.163.com/blog/static/167399507201472343324562) 
### 使用git 提交的时候提示以下内容：
```
[git@AYZ github01]$ git push -u origin master
error: The requested URL returned error: 403 Forbidden while accessing
https://github.com/jingzing/JingJourney.git
```

### 解决办法：
```
vi .git/config

# 将
[remote "origin"]  
    url = https://github.com/jingzing/JingJourney.git
修改为：
[remote "origin"]
url = https://jingzing@github.com/jingzing/JingJourney.git
```

### 总结：其实还不是很明白为什么这样是可以的，网上找的解决办法，先mark 下。

## 2.git pull push没有指定branch报错的解决方法
[参考链接](https://www.netroby.com/view/3203)
### git 执行git push 和git pull的操作时候，经常看到下面的提示：
```
You asked me to pull without telling me which branch you
want to merge with, and 'branch.dev.merge' in
your configuration file does not tell me, either. Please
specify which branch you want to use on the command line and
try again (e.g. 'git pull <repository> <refspec>').
See git-pull(1) for details.

If you often merge with the same branch, you may want to
use something like the following in your configuration file:

[branch "dev"]
remote = <nickname>
merge = <remote-ref>

[remote "<nickname>"]
url = <url>
fetch = <refspec>

See git-config(1) for details.
```

### 在高版本的 git下面，也许会看见这样的提示：
```
 There is no tracking information for the current branch.
 
 Please specify which branch you want to merge with.
 
 See git-pull(1) for details
 
 git pull <remote> <branch>
 
 If you wish to set tracking information for this branch you can do so with
 
 git branch --set-upstream master origin/<branch>
 
 看到第二个提示，我们现在知道了一种解决方案。也就是指定当前工作目录工作分支，跟远程的仓库，分支之间的链接关系。
 
 比如我们设置master对应远程仓库的master分支
 
 git branch --set-upstream master origin/master
 
 这样在我们每次想push或者pull的时候，只需要 输入git push 或者git pull即可。
 
 在此之前，我们必须要指定想要push或者pull的远程分支。
 
 git push origin master
 
 git pull origin master
```

