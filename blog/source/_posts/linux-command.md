---
title: Linux命令整理
date: 2016-12-27 10:51:37
updated:
categories: linux
permalink:
tags:
- linux 
- arch
---

 整理工作中常用的linux命令, 方便查询, 也算是总结, 利人利己, 虽然不是专业运维,工作中也时常需要临时充当一下运维的角色, 所以记录一下,  这里面可能会包含 `arch` 的一些特殊命令, 不过会有特殊标记.

<!-- more -->


### Linux 常用命令整理

#### 新建目录
```bash
mkdir dir
```
#### 新建多级目录
```bash
mkdir -p dir/xxx
```

#### 新建多个目录
```bash
mkdir {d1,d2}
```

#### 删除文件
```bash
rm f1
```

#### 强制删除文件[无提示]
```bash
rm -rf f1
```

#### 删除目录
```bash
rm -r dir/
```

#### 删除多个目录
```bash
rm -r {dir1, dir2}
```

#### ssh登录远程主机
```bash
ssh user@host
```

#### ssh登录远程主机[*指定端口号*]
```bash
ssh user@host -p port
```

#### 本地复制文件
```bash
cp cpfile /targetpath/xx/
```

#### 本地复制文件夹
```bash
cp -r cpdir /targetpath/xxx
```

#### 本地上传文件到远程主机
```bash
scp localfile user@host:/targetpath/
```

#### 本地上传文件到远程主机[*指定端口访问*]
```bash
scp -P port localfile user@host:/targetpath/
```

#### 本地上传文件夹到远程主机[*指定端口访问*]
```bash
scp -P port -r localdir user@host:/targetpath/
```



---- END 以后用到了再补充
