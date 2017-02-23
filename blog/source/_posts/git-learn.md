---
title: git-学习
date: 2017-01-06 23:48:59
updated:
categories: git
permalink:
tags: git github
---

在java开发过程中,不出意外大家都会使用各种IDE(集成开发环境)搭建项目然后进行开发, 各种IDE为了更好的管理当前项目,让开发人员更高效的开发,都会针对当前项目生成一些类似索引或者项目配置的文件或者目录. 例如 [IntelliJ IDEA](http://www.jetbrains.com/idea/) 会生成 `.idea` 目录用来管理整个`project`,和`.iml` 文件用来管理当前 `Module`.

<!-- more -->

所以 我们在使用`git`来协作开发的时候并不希望这些文件跟随这版本一起出现在大家的视野中, 不仅仅这些文件或目录开发的web应用没有作用,更重要的是会搞乱大家的工作环境,因为一个项目组完全可能出现两种以上的`IDE`.
所以需要在`git`版本中过滤这些目录或文件.

一般,采用`git`提供方式: 将需要忽略的文件或目录添加到 `.gitignore`文件中,这时候,使用`git`命令查看 `change list`时,被忽略文件则不会出现在待提交列表里. `git status`

`.gitignore`文件内容类似这样的
<pre>
*.iml
.idea   
db.json   
/target/       
*.log    
blog/node_modules/    
blog/public/   
blog/themes/    
.deploy*/
</pre>

如果需要将版本控制内文件或目录添加到该文件内,进而去除该文件或目录的版本跟踪,则需要先将被忽略文件或目录添加到`.gitignore`后还需要以下步骤:
删除cache

```git
git rm -r --cached  /ignorepath/

```
提交`.gitignore`文件
<code>
git add .gitignore
git commit -m "message"
</code>
