---
title: java回调函数
date: 2016-11-17 13:42:37
updated:
categories: Java
permalink:
tags: 
- java
- callback
---


在js中,因为js是单线程,所以在js中操作中,经常使用回调函数实现程序的异步执行, 从而解决代码串行导致线程阻塞的问题,或者类似[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)/[filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)等函数 - 将一个函数作用到数组的每个元素上. 

<!-- more --> 

如下这样:

```javascript
function dosomth(arg,callback) {

	var exec = (typeof callback === 'function') , 
              ret = [], isArr = arg instanceof Array;
	
	if (!exec) {
		throw new Error(callback + ' is not a function') ;
	}

	if (isArr) {
		for(var i=0, len = arg.length; i < len ; i++) {
			ret.push(callback.call(null,arg[i])) ;
		}
	}else {
		ret = arg ;
	}

	return ret ;
}
```
调用如:
```javascript
var arr = dosomth([1,2,3,4,5],function(e) {
	return e * e ;
}) ;

console.log(arr) ;
```

```log
//print
[ 1, 4, 9, 16, 25 ]
```
这段代码简单的模仿了javascript高阶函数[`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), 循环处理每一个`Entry` ,  在js中能这么使用得益于javaScript中函数有多样化的表达方式, 函数可以是一个对象,也可以是一个函数表达式, 在上面我们可以将一个函数当做一个形参传递给函数`dosomth` . 在java中是没办法做到这一点的, 因为在java中,一切皆对象, 所以成员都需要依附在某个对象上, 所以,如果想实现类似上述回调的机制, 可以从java的代理模式上找到灵感. 

先定义一个`回调函数`接口: 
```java
public interface Callback {

    List<String> callback(List<String> list) ;
}
```
然后,定义使用`这个回调函数的抽象业务类` , 如:
```java
public class C {

    public List<String> doSomthing(List<String> list,Callback callback) {
        //

        return callback.callback(list) ;
    }
}
```

然后定义具体的业务类,如:
```java
public class Test {

    public static void main(String[] args) {
        C c = new C() ;


        List<String> list = new ArrayList<String>() ;
        list.add("1");
        list.add("2");
        list.add("3");
        list.add("4");
        list.add("5");
        list.add("6");
        list.add("7");
        list.add("8");

        List<String> result = c.doSomthing(list, new Callback() {
            @Override
            public List<String> callback(List<String> list) {
                List<String> list1 = new ArrayList<String>();
                for (String str : list) {
                    list1.add(str + "_" + str);
                }

                return list1;
            }
            //
        }) ;

        System.out.println(" 处理后的字符数组 : " + result);
    }
}
```

运行`test.main` 可以看到输出:
```log
 处理后的字符数组 : [1_1, 2_2, 3_3, 4_4, 5_5, 6_6, 7_7, 8_8]
```

在这段代码中,其实就是将js中的函数表达式引用变为java对象引用持有,然后调用指定方法或函数实现业务功能. 这其实和代理模式一样一样的. 都是将具体的业务处理逻辑委托给另一个对象来完成.

以上就是我对代理和java版`回调`的理解,请指正!  thx ~~
