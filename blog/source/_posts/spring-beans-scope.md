---
title: spring beans 作用域
date: 2017-02-21 10:50:13
updated:
categories: java web
permalink:
tags:
- spring
- spring beans
- java web
---

一直以为,在spring IOC容器中管理的bean实例的作用域范围都是`singleton`,今天专门翻看了[spring docs](http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)才知道,其实发展到`spring 3.0`以后,在IOC容器中存活的bean支持五种作用域配置:

1. singleton
2. prototype
3. request
4. session
5. global session

后面三种,只能存用于web应用的`ApplicationContext`.

<!-- more -->

这是从网站抓下来的 Table of Bean scopes:

| Scope	                                  | Description |
| :------: |:-----------------:|
|singleton|Scopes a single bean definition to a single object instance per Spring IoC container.|
|prototype|Scopes a single bean definition to any number of object instances.|
|request|Scopes a single bean definition to the lifecycle of a single HTTP request; that is each and every HTTP request will have its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring ApplicationContext.|
|session|Scopes a single bean definition to the lifecycle of a HTTP Session. Only valid in the context of a web-aware Spring ApplicationContext.|
|global session|Scopes a single bean definition to the lifecycle of a global HTTP Session. Typically only valid when used in a portlet context. Only valid in the context of a web-aware Spring ApplicationContext.|       


####  singleton scope
在spring #IOC容器中管理的bean 作用域默认为 `singleton`的, 所以我们不需要特别配置,则应用中所有这个对象的引用共享一个对应的实例,一般就是在spring初始化的时候创建的那个实例. 这个实例是在整个spring容器中有且仅有一个,和我们说的单例设计模式是不同的, 在设计模式中,保证在一个classloader中只能存在一个实例.所以,我们有一种利用classloader类加载互斥的特性来实现单例.
我觉得[spring docs](http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)上描述`singleton`的图片非常的简洁明了,简单易懂:

{% asset_img singleton.png singleton scope %}

在上面三个bean中都引用了`accountDao` 这个bean定义的收作用域是 `singleton`的,那么在左边三个bean中,`accountDao`引用共享一个实例.

`singleton` bean 在`applicationContext.xml`中,配置类似这样:
```xml
<bean id="accountService" class="com.foo.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default); using spring-beans-2.0.dtd -->
<bean id="accountService" class="com.foo.DefaultAccountService" scope="singleton"/>

<!-- the following is equivalent and preserved for backward compatibility in spring-beans.dtd -->
<bean id="accountService" class="com.foo.DefaultAccountService" singleton="true"/>
```

上面三种方式配置效果是等同的, 第一种为我们常见的配置,因为默认为`singleton`作用域.    


#### prototype scope
`prototype scope`和`singleton scope`的区别就是在产生多个实例上,在这个作用域范围,每个实例引用都拥有自己的实例,不和其他引用共享实例, 在每次请求这个bean在容器内注入或者使用`getBean()`方法得到这个bean的时候都会得到一个新的实例. 如下这种情况推荐使用`prototype scope` :
> 容器内对象拥有自己的状态,各个实例引用持有者的对象状态不同,例如,报价服务对象,可以给不同产品报价,然后内部需要维护报价产品的部分信息.    

在`xml`中定义 `prototype scope`bean类似这样:
```xml
<!-- using spring-beans-2.0.dtd -->
<bean id="accountService" class="com.foo.DefaultAccountService" scope="prototype"/>

<!-- the following is equivalent and preserved for backward compatibility in spring-beans.dtd -->
<bean id="accountService" class="com.foo.DefaultAccountService" singleton="false"/>
```
上面两种定义方式效果是等同的

[spring docs](http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)描述图片:
{% asset_img prototype.png prototype scope %}

图片中左边定义的三个bean都引用了 `accountDao`,则这三个bean都会拥有自己的实例,这些实例的生命周期需要注意,docs说明如下:
> There is one quite important thing to be aware of when deploying a bean in the prototype scope, in that the lifecycle of the bean changes slightly. Spring does not manage the complete lifecycle of a prototype bean: the container instantiates, configures, decorates and otherwise assembles a prototype object, hands it to the client and then has no further knowledge of that prototype instance. This means that while initialization lifecycle callback methods will be called on all objects regardless of scope, in the case of prototypes, any configured destruction lifecycle callbacks will not be called. It is the responsibility of the client code to clean up prototype scoped objects and release any expensive resources that the prototype bean(s) are holding onto. (One possible way to get the Spring container to release resources used by prototype-scoped beans is through the use of a custom bean post-processor which would hold a reference to the beans that need to be cleaned up.)

> In some respects, you can think of the Spring containers role when talking about a prototype-scoped bean as somewhat of a replacement for the Java 'new' operator. All lifecycle aspects past that point have to be handled by the client. (The lifecycle of a bean in the Spring container is further described in the section entitled Section 4.5.1, “Lifecycle callbacks”.)     


#### other scopes
##### request
在每次Http请求中,都会重新生成一个实例, 在action中最为常见,因为返回的实例中大多都包含了本次请求的信息, 随着这次请求的结束这些信息也就没有用了,并且不能影响下次的请求,所以这个对象的作用范围只能是本次请求.
定义:
``` xml
  <bean id="loginAction" class="com.foo.LoginAction" scope="request"/>
```      

##### session    
和request类似,如果定义一个bean的作用域为 session则不同的HttpSession中实例不同.
定义:
`` <bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
``
在HttpSession中,每个`userPreferences`引用持有者都修改自己的`userPreferences`实例,而不会相互影响.


##### global session
global session作用域bean定义类似这样:
`` <bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/> ``
global session有点类似 singleton不过其共享范围是在基于portlet context的各个应用之间.
> Please note that if you are writing a standard Servlet-based web application and you define one or more beans as having global session scope, the standard HTTP Session scope will be used, and no error will be raised.    


参考资料:
4.4Bean scopes: http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)
stackoverflow :http://stackoverflow.com/questions/17599216/spring-bean-scopes
cnblog: http://www.cnblogs.com/cxyj/p/3906429.html
