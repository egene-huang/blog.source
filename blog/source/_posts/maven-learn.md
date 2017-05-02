---
title: 学习maven
date: 2017-05-02 14:18:16
updated:
categories: Maven
permalink:
tags:
- maven
---


maven 是一个应用构建工具, maven将公共资源存放在仓库中进行管理, 使用settings.xml配置文件获取方式和地址. 在settings.xml 文件中配置本地仓库和远程仓库, 远程仓库可以是自己搭建的私服也可以是开源的maven仓库, maven仓库中存放的文件可以通过三个属性定位到唯一文件:
- groupId
- artifactId
- version    


特殊情况还有例如type等属性更加精确的控制,例如:
```xml
<dependency>
    <groupId>com.test.cn</groupId>
    <artifactId>config</artifactId>
    <version>1.0</version>
    <type>pom</type>
</dependency>
```
每个项目/模块都会有一个`pom.xml`来映射这个模块的特征,例如该模块依赖关系,模块packaging值[jar/pom/war等],profile等等.

<!-- more -->

##### 排除依赖
有一个模块 `module-x`,依赖如下:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.test.cn</groupId>
    <artifactId>module-x</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.test.cn</groupId>
            <artifactId>module-A</artifactId>
            <version>1.0</version>
        </dependency>
        <dependency>
            <groupId>com.test.cn</groupId>
            <artifactId>module-B</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>

</project>
```
模块 `module-y` 依赖 `module-x`  但是不需要依赖模块 `module-A`,则需要在 `module-y` 依赖模块`module-x`的时候排除 `module-A` , 在无特殊情况下(基于scope), `module-A`因为maven依赖的传递性, `module-y`也将依赖 `module-A`,则 `module-y` 的pom.xml文件如下:    
```xml
<dependency>
  <groupId>com.test.cn</groupId>
  <artifactId>module-x</artifactId>
  <exclusions>
    <exclusion>
      <groupId>com.test.cn</groupId>
      <artifactId>module-A</artifactId>
    </exclusion>
  </exclusions>
  <version>1.0</version>
</dependency>
```
如果模块y需要依赖2.0版本的模块A,则可以在依赖模块x的时候排除模块A然后手动依赖模块A 2.0即可.
<pre>
小结:
maven解析传递性依赖原则:
1. 距离最近为第一原则  例如: A -> B -> C(1.0) ;  A -> C(2.0) 则这时候只C(2.0)会被加入进来.
2. 声明顺序为第二原则 例如 A -> B -> C(1.0) ; A -> D -> C(2.0) 这时候,如果 B优先于D声明 则只C(1.0)会被加入进来,反之加入C(2.0)依赖
</pre>


##### maven模块协作开发
工作中,为了业务拆分经常将整个项目按照功能或业务依赖等其他方式拆分为多个模块, 在功能或者业务依赖中可以独立存在的模块作为基础服务提供给上层业务使用,则在开发中, 服务提供模块和服务消费模块可以分开放在两个项目开发小组中, 但是,提供服务模块开发完成需要将构建完成的jar文件传给服务使用者,之后,如果有版本更新则,服务使用者也需要同步更新依赖配置, 步骤繁琐容易出错,也没有发挥出maven的功能, 这个问题可以使用maven自动部署构建至私服来解决.比如: 其中一个开发小组 X 负责开发模块 `data-x` 为另一个项目web模块 `web-x`服务, 当X小组完成第一个可交付使用的jar的时候,使用 `mvn clean deploy`命令将 `data-x.jar`部署到公司`nexus`私服(或Jarvana/MVNbrowser等)上, 然后web项目开发小组 W 在pom文件中引入 `data-x`模块,然后运行命令 `mvn clean package/install` 获取模块 `data-x`的全部功能.
在开发阶段, 不管是需求调整还是存在逻辑或业务bug,需要修复,然后重新发布`data-x.jar`,这个时候 X小组就会发布一个最新的版本到私服上, 一般情况下, W小组则需要不断的修改pom文件引入 最新版本的`data-x.jar`文件, 这在开发过程中,是非常不合适的, 所以,maven针对这种情况,也有相应的解决方案, 在 W小组引入`data-x.jar`文件的时候,指明引入其快照版本(`1.0-SNAPSHOT`), 同时X小组也发布`data-x.jar`的快照版本到公司私服的快照仓库中,如此, X小组也不用每次都需要修改发布版本号,W小组也不需要修改依赖版本号. 最终,X小组开发的 `data-x.jar`项目的pom文件如下:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.test.cn</groupId>
    <artifactId>data-x</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <name>bd Maven project</name>
    <url>http://maven.apache.org</url>



    <!-- W小组引入 data-x.jar 版本类似引入 data-y依赖-->
    <dependencies>
        <dependency>
            <groupId>com.test.cn</groupId>
            <artifactId>data-y</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

   <!-- 发布到私服配置 -->
    <distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <name>Nexus Releases Repository</name>
        <url>http://192.168.0.1:8081/nexus/content/repositories/release/</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <name>Nexus Snapshots Repository</name>
        <url>http://192.168.0.1:8081/nexus/content/repositories/snapshot/</url>
    </snapshotRepository>
</distributionManagement>

</project>
```

同时,settings.xml文件需要配置能有上传文件权限的用户配置,如下:
```xml
<servers>
  <server>
  <id>nexus-releases</id>
  <username>admin</username>
  <password>test</password>
</server>
<server>
  <id>nexus-snapshots</id>
  <username>admin</username>
  <password>test</password>
</server>
</servers>
```

注意 settings.xml中server节点id值和 pom文件中`distributionManagement`节点id值保持一致,否则无法获取用户名和密码,以至于发布失败.


##### maven-war-plugin插件
在构建war或jar文件的时候, 可能需要调整项目结构或根据环境不同构建适应不同环境的war包, 这个插件可以解决.利用该插件通过配置profile节点然后构建不同环境使用的war文件,在build节点配置不同环境下的资源,如:
```xml
<finalName>web-x</finalName>
     <sourceDirectory>src/main/java</sourceDirectory>
     <outputDirectory>target/classes</outputDirectory>
     <resources>
         <resource>
             <directory>src/main/java</directory>
             <excludes>
                  <exclude>**/*.java</exclude>
                  <exclude>**/.svn/*</exclude>
              </excludes>
          </resource>
         <resource>
             <directory>src/main/resources</directory>
             <includes>
                 <include>**/*.*</include>
             </includes>
         </resource>
         <resource>
             <directory>src/main/config/${package.environment}/resource</directory>
             <includes>
                 <include>**/*.*</include>
             </includes>
         </resource>
     </resources>

     <profiles>
             <profile>
                 <id>dev</id>
                 <properties>
                     <package.environment>dev</package.environment>
                 </properties>
                 <activation>
                     <activeByDefault>true</activeByDefault>
                 </activation>
             </profile>
             <profile>
                 <id>test</id>
                 <properties>
                     <package.environment>test</package.environment>
                 </properties>
                 <activation>
                     <activeByDefault>false</activeByDefault>
                 </activation>
             </profile>
             <profile>
                 <id>product</id>
                 <properties>
                     <package.environment>product</package.environment>
                 </properties>
                 <activation>
                     <activeByDefault>false</activeByDefault>
                 </activation>
             </profile>
         </profiles>
```

在构建项目的时候使用参数 P 可以构建特定环境下的war,如: mavn install -P test

----------------------

有时候需要将多个war 合并为一个,则 该插件通过 `overlays`配置完成这个需求, 首先需要引入被合并的war:
```xml
<dependency>
    <groupId>com.test.cn</groupId>
    <artifactId>web-z</artifactId>
    <version>1.0-SNAPSHOT</version>
    <type>war</type>
</dependency>
```
配置需要合并的文件:
```xml
<plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-war-plugin</artifactId>
      <version>2.2</version>
      <configuration>
          <warName>web-x</warName>
          <overlays>
              <overlay>
                  <groupId>com.test.cn</groupId>
                  <artifactId>web-z</artifactId>
                  <includes>
                      <include>comm/**</include>
                      <include>comm-static/**</include>
                      <include>WEB-INF/comm/**</include>
                      <include>WEB-INF/favicon.ico</include>
                  </includes>
              </overlay>
          </overlays>
      </configuration>
</plugin>
```

这样被包含的文件/目录就会被合并到 `web-x.war`中, 需要注意两个项目的目录结构必须一致,而且合并时以主动合并的war目录为准,如果存在则不会覆盖. 详细说明在[这里](https://maven.apache.org/plugins/maven-war-plugin/overlays.html)

maven比较常用的大概就是这些了, 笔记很粗糙, 具体使用请参考
[maven使用指南](http://maven.apache.org/users/index.html)
以及:
[Maven实战#许晓斌](https://book.douban.com/subject/5345682/)
Maven权威指南 这两本书看完工作中使用完全没有问题的.


 ##### 记录完
