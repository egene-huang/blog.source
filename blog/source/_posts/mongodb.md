---
title: 初次学习mongodb
date: 2016-11-21 14:22:04
updated:
categories: db
permalink:
tags:
- mongodb
- mongodb install
---

## MongoDB安装

学习node块结束了, 想使用express+ejs+mongoDB 完成一个小栗子, 算是我学习node的一个结业考试.  不过,之前从来没有接触过mongodb,对我这小小白来说, 安装-启动mongodb都异常的艰难,  想想真是 ...  铁窗呀 ~~  铁门 ~  铁锁链 ~~~  路过的你们请尽情嘲笑吧 ,  我不怕!!!!

<!-- more -->

在mongodb官网的[安装指南](https://docs.mongodb.com/manual/administration/install-on-linux/) 上没有找到 `ArchLinux`版本的安装包, 所以通过下载安装包解压使用是不太可能了, 只能自己下载源码编译安装或者看看`Arch` 官方仓库或[`AUR`](https://wiki.archlinux.org/index.php/Arch_User_Repository)仓库有没有了, 不过丰富的软件支持在`Arch`里从来不是问题,! `Arch`官方仓库有, `AUR`仓库也有.  如:

** pacman** 
```bash
[palm@arch]: ~/myapps/node-web-learn>$ sudo pacman -Ss MongoDB
[sudo] password for palm: 
community/libmongoc 1.4.2-1
    A client library written in C for MongoDB
community/mongodb 3.2.10-2
    A high-performance, open source, schema-free document-oriented database
community/mongodb-tools 3.2.5-1
    The MongoDB tools provide import, export, and diagnostic capabilities.
community/php-mongodb 1.1.9-1
    MongoDB driver for PHP
community/python-pymongo 3.3.1-1
    Python module for using MongoDB
community/python2-pymongo 3.3.1-1
    Python module for using MongoDB

```
** AUR ** 
```bash
[palm@arch]: ~/myapps/node-web-learn>$ yaourt -Ss MongoDB
community/libmongoc 1.4.2-1
    A client library written in C for MongoDB
community/mongodb 3.2.10-2
    A high-performance, open source, schema-free document-oriented database
community/mongodb-tools 3.2.5-1
    The MongoDB tools provide import, export, and diagnostic capabilities.
community/php-mongodb 1.1.9-1
    MongoDB driver for PHP
community/python-pymongo 3.3.1-1
    Python module for using MongoDB
community/python2-pymongo 3.3.1-1
    Python module for using MongoDB
aur/adminer-git 4.2.5.r0.g53dfafd-1 (2) (0.01)
    a web based SQL management tool supporting MySQL, PostgreSQL, SQLite, MS SQL, Oracle, Firebird, SimpleDB, Elasticsearch and MongoDB. Formerly phpMinAdmin.
aur/cube-git 20140730-1 (0) (0.00)
    A system for analyzing time series data using MongoDB and Node
aur/fluent-plugin-mongo 0.7.7-1 (0) (0.00)
    MongoDB plugin for Fluent event collector
aur/graylog 2.1.2-2 (3) (0.00)
    Graylog is an open source syslog implementation that stores your logs in ElasticSearch and MongoDB
aur/libmongo-client 0.1.8-1 (0) (0.00)
    Alternative C driver for MongoDB (obsolete)
aur/mongo-cxx-driver r3.0.0-1 (Out of Date) (5) (1.24)
    The official MongoDB C++ driver library
aur/mongo-cxx-driver-legacy 1.1.2-1 (6) (0.00)
    Official MongoDB C++ driver (legacy).
aur/mongo-cxx-driver-legacy-0.0-26compat 2.6.12-1 (6) (0.00)
    Official MongoDB C++ driver (26compat).
aur/mongo_fdw 4.0.0-2 (1) (0.00)
    PostgreSQL foreign data wrapper for MongoDB
aur/mongobooster 3.0.3-1 (Out of Date) (3) (0.87)
    The Smartest MongoDB Admin GUI
aur/mongoclient 1.3.0-1 (1) (0.09)
    MongoDB administration client
aur/perl-bson 0.11-1 (1) (0.00)
    Pure Perl implementation of MongoDB's BSON serialization
aur/perl-mojolicious-plugin-mongodb 1.16-1 (0) (0.00)
    Use MongoDB in Mojolicious
aur/perl-mongodb 1.2.3-1 (3) (0.00)
    Official MongoDB Driver for Perl
aur/perl-mongodbx-autoderef 1.110560-1 (0) (0.00)
    Automagically dereference MongoDB DBRefs lazily
aur/php53-mongo 1.5.8-1 (1) (0.00)
    Officially supported PHP 5.3 driver for MongoDB
aur/python-flask-pymongo 0.4.0-1 (1) (0.00)
    Flask-PyMongo bridges Flask and PyMongo, so that you can use Flask’s normal mechanisms to configure and connect to MongoDB.
aur/python-monary-hg 0.3.0.r69.efc4072b9b7f-1 (1) (0.00)
    Perform high-performance column queries from MongoDB for Python 3. 10x speedup over pymongo alone.
aur/python-mongoengine 0.10.6-2 (0) (0.00)
    An object-document mapper for MongoDB.
aur/python2-dex 0.5.5-1 (0) (0.00)
    A MongoDB performance tuning tool that compares queries to the available indexes in the queried collection(s) and generates index suggestions based on simple heuristics.
aur/python2-flask-pymongo 0.4.0-1 (1) (0.00)
    Flask-PyMongo bridges Flask and PyMongo, so that you can use Flask’s normal mechanisms to configure and connect to MongoDB.
aur/python2-monary-hg 0.3.0.r69.efc4072b9b7f-1 (1) (0.00)
    Perform high-performance column queries from MongoDB for Python 2. 10x speedup over pymongo alone.
aur/python2-pymongo-2.9 2.9-1 (0) (0.00)
    Python driver for MongoDB
aur/robomongo 0.9.0-1 (70) (4.08)
    Shell-centric cross-platform open source MongoDB management tool
aur/robomongo-bin 0.9.0-1 (20) (1.54)
    Shell-centric cross-platform open source MongoDB management tool
aur/ros-indigo-moveit-ros-warehouse 0.6.5-2 (0) (0.00)
    ROS - Components of MoveIt connecting to MongoDB.
aur/ros-indigo-warehouse-ros 0.8.8-4 (0) (0.00)
    ROS - Persistent storage of ROS data using MongoDB.
aur/umongo 1.6.2-1 (40) (1.18)
    This package provides a GUI app that can browse and administer a MongoDB cluster
[palm@arch]: ~/myapps/node-web-learn>$ sudo pacman -Ss MongoDB
[sudo] password for palm: 
community/libmongoc 1.4.2-1
    A client library written in C for MongoDB
community/mongodb 3.2.10-2
    A high-performance, open source, schema-free document-oriented database
community/mongodb-tools 3.2.5-1
    The MongoDB tools provide import, export, and diagnostic capabilities.
community/php-mongodb 1.1.9-1
    MongoDB driver for PHP
community/python-pymongo 3.3.1-1
    Python module for using MongoDB
community/python2-pymongo 3.3.1-1
    Python module for using MongoDB
```

随便挑选一个安装就可以了.




## 使用MongoDB

软件找到了以后, 然后找到`mongodb` node.js的[支持指南](http://mongodb.github.io/node-mongodb-native/2.2/installation-guide/) 按照指南安装好node.js的mongodb驱动包,然后编写一个小demo测试一下, [Quick Start](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/):


```node
var MongoClient = require('mongodb').MongoClient,assert = require('assert');
// Connection URL
var url = 'mongodb://localhost:27017/myproject';

// Use connect method to connect to the server
MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log("Connected successfully to server");

  db.close();
});
```

接着运行 `node app.js` ,  不是我想象的那般顺利, 错误如下:

```bash
[palm@arch]: ~/myapps/mongodb-test>$ node app.js 

/home/palm/myapps/mongodb-test/node_modules/mongodb/lib/mongo_client.js:225
          throw err
          ^
AssertionError: null == { MongoError: failed to connect to server [localhost:27017] on first connect
    at Pool.<anonymous> (/home/palm/myapps/mongodb-
    at /home/palm/myapps/mongodb-test/app.js:9:10
    at connectCallback (/home/palm/myapps/mongodb-test/node_modules/mongodb/lib/mongo_client.js:315:5)
    at /home/palm/myapps/mongodb-test/node_modules/mongodb/lib/mongo_client.js:222:11
    at _combinedTickCallback (internal/process/next_tick.js:67:7)
    at process._tickCallback (internal/process/next_tick.js:98:9)
```


网络上一顿狂搜, 好像安装好以后需要指定mongodb存放数据的目录, 默认在`/data/db`  于是:

```bash

[palm@arch]: ~/myapps/mongodb-test/data>$ mongod --dbpath=/home/palm/myapps/mongodb-test/data
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] MongoDB starting : pid=13694 port=27017 dbpath=/home/palm/myapps/mongodb-test/data 64-bit host=arch
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] db version v3.2.10
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] git version: 79d9b3ab5ce20f51c272b4411202710a082d0317
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2j  26 Sep 2016
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] modules: none
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] build environment:
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten]     distarch: x86_64
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2016-11-21T10:45:55.066+0800 I CONTROL  [initandlisten] options: { storage: { dbPath: "/home/palm/myapps/mongodb-test/data" } }
2016-11-21T10:45:55.086+0800 I STORAGE  [initandlisten] exception in initAndListen: 98 Unable to create/open lock file: /home/palm/myapps/mongodb-test/data/mongod.lock errno:13 Permission denied Is a mongod instance already running?, terminating
2016-11-21T10:45:55.086+0800 I CONTROL  [initandlisten] dbexit:  rc: 100

```

大概意思是, 已经有mongodb服务在运行中,现在没有权限再创建一个mongodb实例.  然后我使用`ps` 确是查询到了很多个mongodb进程.  找到后一一都干死, 然后从`Archlinux Wiki  ` MongoDB篇又了解到在Archlinux中[MongoDB安装指南](https://wiki.archlinux.org/index.php/MongoDB#Installation) 需要使用`systemctl` 启动服务:

```bash
systemctl start mongodb.service

```


下面也有一些常用问题解决方案,例如mongodb服务异常关闭导致被锁,和mongodb使用指南, 很方便,  之前老听人说`Archlinux Wiki` 是Linux发行版最完整最详细的Wiki,  现在看来,确是如此, 平时archlinx 闹脾气也都是翻阅Wiki解决的. 

这一通折腾之后, 使用`--repair`修复之前的`database` 设置, 如:

```bash
mongod --dbpath /home/palm/myapps/mongodb-test/data --repair

```

然后运行`mongo` 进入mongodb命令行(MongoDB shell)并初始化数据库:

```bash
[palm@arch]: ~/myapps/mongodb-test>$ mongo
MongoDB shell version: 3.2.10
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2016-11-21T14:19:39.625+0800 I CONTROL  [initandlisten] 
2016-11-21T14:19:39.625+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-11-21T14:19:39.625+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-11-21T14:19:39.625+0800 I CONTROL  [initandlisten] 
>

```



这代表mongodb服务已经成功启动了, 可以使用了, 到这里,我总算是半只脚迈进了mongodb的大门, 接下来可以在项目实践中一边学些一边使用mongodb了.



## 关于设置mongodb数据存储目录

在linux中,默认情况下手动安装或解压mongodb应用包后,缺少数据存储目录: 
> 默认为`/data/db`


这个目录最开始是不存在的,而且也启动服务也不会帮我们新建. 所以需要提前将这个目录新建完成. 当然,这个目录是可以修改的, 使用`--dbpath` 自定义mongodb数据存储目录.  使用`--port arg` 可以自定义服务端口, 还有主机地址 host , 更多参数帮助可以使用:


```bash
mongo --help
```

查看

```bash
[palm@arch]: ~/myapps/mongodb-test>$ mongo --help
MongoDB shell version: 3.2.10
usage: mongo [options] [db address] [file names (ending in .js)]
db address can be:
  foo                   foo database on local machine
  192.169.0.5/foo       foo database on 192.168.0.5 machine
  192.169.0.5:9999/foo  foo database on 192.168.0.5 machine on port 9999
Options:
  --shell                             run the shell after executing files
  --nodb                              don't connect to mongod on startup - no 
                                      'db address' arg expected
  --norc                              will not run the ".mongorc.js" file on 
                                      start up
  --quiet                             be less chatty
  --port arg                          port to connect to
  --host arg                          server to connect to
  --eval arg                          evaluate javascript
  -h [ --help ]                       show this usage information
  --version                           show version information
  --verbose                           increase verbosity
  --ipv6                              enable IPv6 support (disabled by default)
  --disableJavaScriptJIT              disable the Javascript Just In Time 
                                      compiler
  --enableJavaScriptProtection        disable automatic JavaScript function 
                                      marshalling
  --ssl                               use SSL for all connections
  --sslCAFile arg                     Certificate Authority file for SSL
  --sslPEMKeyFile arg                 PEM certificate/key file for SSL
  --sslPEMKeyPassword arg             password for key in PEM file for SSL
  --sslCRLFile arg                    Certificate Revocation List file for SSL
  --sslAllowInvalidHostnames          allow connections to servers with 
                                      non-matching hostnames
  --sslAllowInvalidCertificates       allow connections to servers with invalid
                                      certificates
  --sslFIPSMode                       activate FIPS 140-2 mode at startup

Authentication Options:
  -u [ --username ] arg               username for authentication
  -p [ --password ] arg               password for authentication
  --authenticationDatabase arg        user source (defaults to dbname)
  --authenticationMechanism arg       authentication mechanism
  --gssapiServiceName arg (=mongodb)  Service name to use when authenticating 
                                      using GSSAPI/Kerberos
  --gssapiHostName arg                Remote host name to use for purpose of 
                                      GSSAPI/Kerberos authentication

file names: a list of files to run. files have to end in .js and will exit after unless --shell is specified
```



如果你将mongodb数据存储目录设置在非`/home/` 下,则需要注意权限,  需要让自定义目录拥有`-r 可读 / -w 可写 / -x 可执行 ` 的权限.


## 参考链接

- [Install MongoDB Community Edition on Linux](https://docs.mongodb.com/manual/administration/install-on-linux/)
- [mongoDB# Quick Start](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/)
- [MongoDB in Archlinux](https://wiki.archlinux.org/index.php/MongoDB#Installation)
- [stackoverflow #Unable to create/open lock file](http://stackoverflow.com/questions/15229412/unable-to-create-open-lock-file-data-mongod-lock-errno13-permission-denied)
- [stackoverflow  #mongodb Mongod complains that there is no /data/db folder](http://stackoverflow.com/questions/7948789/mongodb-mongod-complains-that-there-is-no-data-db-folder)

