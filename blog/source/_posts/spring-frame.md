---
title: Spring Data 框架整合
date: 2017-01-07 16:44:07
updated:
categories: spring
permalink:
tags:
- spring mvc
- spring data jpa
- java
---

工作三年, 一直在写java代码,一直也没有自己一个人从无到有的搭建一套java web框架, 因为工作中,也都是别人准备好项目开发环境, 然后我再在上面写代码,平时我也没有正经花时间在上面,java web框架很多, jar相互之间的依赖和冲突也比较厉害, 这些问题纠缠在一起, 让我望而却步. 一直想尝试自己一个人整合一套java web框架然后在上面开发,一次偶然的机会迫使我不得不自己动手搭建一套可以快速开发的java web框架, 借这次机会我准备使用 `Spring MVC + Spring Data JPA` 开发web应用.  不出意外,其过程虽然不是很顺利,不过最终总算是不负众望,已经可以投入使用, 其实这里面需要配置的不是很多,更多的时候jar版本的选择比较麻烦.

<!-- more -->

项目整体的结构如下:
```bash
soccer/
├── parent
│   ├── parent.iml
│   └── pom.xml
├── pom.xml
├── soccer-bat
│   ├── pom.xml
│   ├── soccer-bat.iml
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── com
│       │   │       └── soccer
│       │   │           └── bat
│       │   │               ├── controller
│       │   │               │   └── TestController.java
│       │   │               ├── domain
│       │   │               │   └── UserService.java
│       │   │               ├── model
│       │   │               │   └── User.java
│       │   │               └── repository
│       │   │                   └── UserRepository.java
│       │   ├── resources
│       │   │   ├── application.properties
│       │   │   ├── css
│       │   │   ├── images
│       │   │   ├── js
│       │   │   ├── logback.xml
│       │   │   └── spring
│       │   │       ├── applicationContext.xml
│       │   │       └── spring-mvc.xml
│       │   └── webapp
│       │       ├── index.jsp
│       │       ├── META-INF
│       │       │   └── jpa-named-queries.properties
│       │       └── WEB-INF
│       │           ├── views
│       │           │   └── hello.jsp
│       │           └── web.xml
│       └── test
│           ├── java
│           └── resources
└── soccer.iml


```

整合这两个框架大概分为两部分:
- 首先整合 `Spring MVC`
- 在这基础之上再添加数据持久层框架`Spring Data JPA`     


新建一个maven项目,使用idea的话,可以选择自带maven webapp模板将自动生成maven webapp项目目录结构.
然后在项目`pom`文件中添加以上框架jar, 我使用了一个专门的模块来管理其他模块依赖的公共jar. [parent]
在其他模块只要将[parent]引入进来就可以使用这些公共jar了. parent模块 `pom`文件如下:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

    <groupId>com.soccer</groupId>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>soccer :: parent</name>
    <url>http://maven.apache.org</url>



    <dependencies>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.1.2.Release</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>4.1.2.Release</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.1.2.release</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>4.1.2.Release</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.7.0</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.7.0</version>
        </dependency>

        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>4.1.6.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>4.1.0.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate.javax.persistence</groupId>
            <artifactId>hibernate-jpa-2.1-api</artifactId>
            <version>1.0.0.Draft-16</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.1.3.Release</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.1.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>4.1.0.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.1.0.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>4.3.6.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>4.3.6.Final</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>1.7.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.32</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.1.41</version>
        </dependency>
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit-dep</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>javax.servlet.jsp.jstl-api</artifactId>
            <version>1.2.1</version>
        </dependency>
        <dependency>
            <groupId>org.glassfish.web</groupId>
            <artifactId>jstl-impl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.3.2</version>
        </dependency>


        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.12</version>
        </dependency>

        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>20030825.184428</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.32</version>
        </dependency>

        <dependency>
            <groupId>commons-pool</groupId>
            <artifactId>commons-pool</artifactId>
            <version>20030825.183949</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>3.0-alpha-1</version>
        </dependency>

        <dependency>
            <groupId>commons-collections</groupId>
            <artifactId>commons-collections</artifactId>
            <version>3.2</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-ehcache</artifactId>
            <version>4.3.10.Final</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.22</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.22</version>
        </dependency>

        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.1.8</version>
        </dependency>

        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.1.8</version>
        </dependency>

        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-access</artifactId>
            <version>1.1.8</version>
        </dependency>

<!--        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback</artifactId>
            <version>0.5</version>
        </dependency>-->

        <dependency>
            <groupId>org.logback-extensions</groupId>
            <artifactId>logback-ext-spring</artifactId>
            <version>0.1.4</version>
        </dependency>
    </dependencies>

</project>

```

然后`soccer-bat` 模块需要依赖`parent`模块, `pom`文件如下:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <parent>
        <groupId>com.soccer</groupId>
        <artifactId>parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>soccer-bat</artifactId>
    <packaging>war</packaging>
    <name>soccer-bat</name>
    <url>http://maven.apache.org</url>
<!--    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>-->
    <build>
        <finalName>soccer-bat</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>tomcat-maven-plugin</artifactId>
                <configuration>
                    <uriEncoding>UTF-8</uriEncoding>
                    <path>/soccer-bat</path>
                </configuration>
            </plugin>

            <!-- war打包插件, 设定war包名称不带版本号 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <warName>soccer-bat</warName>
                    <webappDirectory>${basedir}/src/main/webapp</webappDirectory>
                    <warSourceDirectory>${basedir}/src/main/webapp</warSourceDirectory>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```


剩下部分在Spring Data 框架整合2.
