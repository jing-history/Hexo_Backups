title:  Hexo 使用
date: 2016-03-07 13:58:00
description: 改用hexo 来搭建blog
categories:
- Hexo
tags:
- Hexo
toc: true
author: Louis Wang
comments:
original:
permalink: 
---
　　** Hexo 主题：**已经开始准备养成写博客的习惯，发现Hexo 快速、简洁且高效的博客框架，所以开始学习使用，部署在git 上面。
[文章链接](http://luuman.github.io/2015/12/21/GitHub+Hexo/)   

Hexo 的安装 
========
          
网上已经有了很详细的参考文档，可以参考下面：
[如何搭建一个独立博客——简明Github Pages与Hexo教程](http://www.jianshu.com/p/05289a4bc8b2) 
## 安装命令：
```
$ npm install -g hexo-cli 安装应该是使用这个
```

上面的写的是整个比较详细的搭建，但是里面有些写的不完整，
比如使用 hexo d 命令的时候，没有说明要先安装git deploy 插件。
```
$ npm install hexo-deployer-git --save
Edit settings.
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```


## 一、安装准备：
>环境搭建：
1.多个git账号之间的切换
*因为原来使用的git 账号已经不使用了，在更换新的账号时候发现多个账号之前不能识别，不能提交到新的远程git 上去。*
[多个git账号之间的切换](http://memoryboxes.github.io/blog/2014/12/07/duo-ge-gitzhang-hao-zhi-jian-de-qie-huan/) 
[Mac上多个git账号之间的切换](http://blog.lessfun.com/blog/2014/06/11/two-github-account-in-one-client/) 
    参考上面的内容，但是本地配置然后还是提交到之前的账号，不知道是不是里面配置顺序的问题，后来的解决方法是把之前
    的git 上的SSH key 的删除掉就可以了，好像有点暴力 0_0..._
2.Error: Permission to user/repo denied to other-user
    机器甲上曾经使用过一个私人github账号A，并且添加了ssh验证
    后来为了公共开发，又创建了一个github账号B，提交的时候发现出现错误: remote: Permission to user_B/xxx.git denied to user_A
    使用git config, git log 查看用户名都是账号B
    github上的解决方法（https://help.github.com/articles/error-permission-to-user-repo-denied-to-other-user/）不适合，因为该项目是B创建的，并不属于A 
    按照https://help.github.com/articles/generating-ssh-keys/ 方法重新创建，还是一样的错误
    最后尝试将https 换成 git （https://help.github.com/articles/changing-a-remote-s-url/）
    git remote set-url origin git@github/...     


