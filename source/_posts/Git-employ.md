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

## Hexo部署时提示Fatal： Could not read from remote repository的问题处理
[原文出处](http://idealife.github.io/2015/10/02/Hexo%E9%83%A8%E7%BD%B2%E6%97%B6%E6%8F%90%E7%A4%BAfatal-Could-not-read-from-remote-repository%E7%9A%84%E9%97%AE%E9%A2%98%E5%A4%84%E7%90%86/) 
第一次在Mac中配置好了hexo,发布的时候却一直提示：
Error: Permission denied (publickey). fatal: Could not read from remote repository.
分析这个错误提示大致可以推断出是公钥配置的问题引起的。

但是github中我已经配置好了当前的公钥信息了，再次检查公钥。
> - 0、github中公钥重新配置了一遍
> - 1、git clone可以正确执行。
> - 2、ssh -T idealife@github.com
也提示：Hi defnngj You’ve successfully authenticated, but GitHub does not provide shell access.
说明配置的公钥没有问题。
就是在执行sudo hexo deploy的时候报公钥问题。

为什么要加sudo呢？
在执行hexo命令时，如果不提权，就会一直提示db.json文件的写权限的错误：
```
Unhandled rejection Error: EACCES: permission denied, open '/Users/idealife/Developer/blog/db.json' at Error (native)

```
所以进行了权限提升。
以前一直是在window环境中配置，对于权限的认识比较浅。突然意识到，就是sudo引起的问题。

因为我生成公钥的命令是ssh-keygen -t rsa -C “idealife@github.com”
没有加sudo，生成的公钥是当前用户的，路径是 /Users/idealife/.ssh。而sudo hexo deploy命令执行的时候应该会去读取的root用户的公钥，很显然root下还没有对应的公钥信息生成。

随即立即验证这个推测。
> - 1、为root生成publickey:sudo ssh-keygen -t rsa -C “idealife@github.com”，对应路径为/var/root/.ssh
> - 2、提取公钥信息并配置到github中。问题又来了，在UI界面中是无法访问到/var下的root文件夹的。我们可以通过sudo cat /var/root/.ssh/id_rsa.pub可以绕开这个问题。
> - 3、执行sudo hexo deploy，部署成功！

总结：在Linux或者UNIX环境中一定要注意权限问题！
