---
title: java基础
date: 2016-11-17 22:21:32
updated:
categories: Java
permalink:
tags:
- java
- javase
---

{% cq %}

   抽空把java基础部分内容通过自己的理解整理如下，心想着等我技艺升级后在回过头来可能会有不一样的收获呢,这也是极好的。同时也想请大家慷慨指出下面错误、遗漏、或不准确的地方，如果大家对某一块有不一样的理解，也请一并留言！
 　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　			--- 谢谢大家!

{% endcq%}

<!-- more -->

## java基础笔记 ##


###  java关键字(也称为保留字) ###
> `notice`> 这里只是罗列出我们工作可能用到的.

| public | class | new | import | package | static | final |
| ---------- | :-----------: | :----------: | :-----------: | :-----------: | :----------: | ----------: |
| synchronized | private| default | protected | return | break | continue | 
| for | while| do | goto[^1] | enum | interface | extends | 
| abstract | implements | int | long | double | float | short | 
| byte | char | boolean | super | if | else | case | 
| switch | finally | instanceof|this| throws|transient|try|
|catch|void| const|throw|  -  | -  |-| 


### java 的CLASSPATH ###
　　java 的classpath 用来设置JVM加载类(class) 的路径,一般设置值为`.` 表示从当前路径开始加载。JVM默认为从当前路径开始加载.但是我们依然需要设置这个值，因为，如果此路径被指定为其他路径则当前路径下class 就不会被加载了。 比如我们设置`CLASSPATH='/home/test/javatest/'`, 则我们将javatest下生成的class文件移动到`home`目录下，执行`java` 命令会提示找不到`xxx class` 。

### java 的path ###
　　`path`在系统里用来帮助系统找到应用程序可执行文件.这里我们是为了方便找到`javac` 和`java`命令.  _包含jdk/bin/和jre/bin/ 目录下的命令。
　　
### 关于类文件名和public修饰的类名一致 ###
　　我们要求类文件名要和`public`修饰的类名一致，这是为什么呢？
　　我们可以尝试将文件名和`public`修饰的类名不一致，编译后我们观察`class`文件．我们会发现`class`文件名称是我们`public`修饰的名子，不会编译成为这个类所在的文件的名．所以会有找不到编写类的可能． 同时也是为了方便找到对应文件所在的类。

### 标识符 ###
　　标识符是我们平常编写代码经常使用到的．我们给类、变量等去名子的时候需要遵守java标识符规范。
**标识符规范我们：只能用数字、字母、下划线(_) 、$组成，且不能以数字开头或使用java关键字.**


### 数据类型 ###
java 数据类型大致分为以下两大类型:
1. 基本数据类型:
	1. 整型数据类型：byte、int、short、long　　　　-> 默认值 : 0
	2.  浮点数据类型(实型)： float、double	　　　　　  -> 默认值：0.0
	3.  字符型: char　　　　　　　　　　　　　　　　  -> 默认值：‘\\u0000’
	4.  布尔型: boolean　　　　　　　　　　　　　　   -> 默认值: false

2. 引用类型:
	数组、类、接口　　　　　　　　　　　　　　　　　-> 默认值: null

{% blockquote %}
这里有一个需要记住的是 `char` 的取值范围是`-128 ~ 127`。基本数据类型中, 数据最大值和最小值是`相邻`的，
比如说：
 int 数据最大值加一则变为最小值. 同理，最小值减一则变为最大值。这个特性很重要 。
 
Byte、Short、Integer、Long、Character、Boolean 这五种包装类默认创建了数值[-128 ~ 127]对应类型的缓存数据，但是超出范围后仍然会创建新的对象。Float 、Double 并没有实现常量池技术。
{% endblockquote %}
 比如：
```java
Integer i1 = 40; //在编译的时候会直接放入常量池中
Integer i2 = new Intege	r(40);//会创建新的对象
Integer i5 = new Integer(0);

System.out.println("i1 == i2 "+ (i1 == i2)); //false
System.out.println("i1 == i2+i5 "+ (i1 == i2+i5)); //true
System.out.println("i1 == i2 "+ (40 == i2)); //true

Integer i3 = 129;
Integer i4 = 129;//new Integer(129);
System.out.println("i3== i4 "+ (i3 == i4));// false
	
```

> 但是有一个特性，普通操作符不使用于包装类，所以包装类做普通运算的时候会自动拆箱进行运算。
例如 Integer 和int 比较  或者Integer 和int  做 + - * /运算都会自动拆箱后运算。所以结果会跟我想的不一样，我们需要注意.








