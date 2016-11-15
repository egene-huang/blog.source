---
title: 使用.gitignore文件，取消某些文件版本控制
date: 2016-11-15 22:25:29
tags: git, github
---

--------------

   忽然想把我的博客重新弄一下，然后还想把博客源码备份到github上，于是就开始弄了， 于是一边准备环境一边提交到 *github* 上，结果就导致了 `/public/` 、`/node_modules/`、`db.json`等这些文件/夹也推动到github上了, 一直只是`听说git/github` 没有实际使用过(瞬间感觉自己好悲哀呀，路人甲一定这么认为滴-_-#) ，所以刚学习了git/github使用，听说有一个`.gitignore`文件可以做到忽略指定文件/夹， 让我们`git status` 不会罗列被忽略的文件。

<!-- more -->
所以我就在git仓库根目录下新建了`.gitignore` 结果，没有生效，因为这些文件之前已经被git追踪了，网上一顿找，需要先告诉git取消这些文件的追踪 ， 需要运行这行命令:
```git
git rm --cached public/
```
使用这种方式是无法达到目的的，会提示:
```bash
fatal: not removing 'public/' recursively without -r
```
需要使用参数`-r` 来递归删除目录，否则只能删除文件，所以取消git版本的命令应该是这样:
```git
git rm -r --cached public
```
然后就可以看到很多文件从版本中删除了。 

可笑，我之前还想着从github仓库中删除这些文件，然后本地`git pull` 就能达到目的，我真是凹凸了。。。

慢慢折腾，加油！！！！


参考链接:
[segmentfault#n͛i͛g͛h͛t͛i͛r͛e͛](https://segmentfault.com/q/1010000000430426)

