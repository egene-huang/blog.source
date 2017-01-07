---
title: Spring Data 框架整合2
date: 2017-01-07 17:43:07
updated:
categories: spring
permalink:
tags:
- spring mvc
- spring data jpa
- java
---


继续 Spring Data 框架整合: 
将上述依赖jar下载到本地 maven仓库后,配置 `Spring MVC` 容器配置文件, 这里的配置文件名字最好与`web.xml`配置的`spring mvc servlet`名字保持一致, 因为框架很多地方是有约定大于规范的.这个配置文件大致包含了两部分:    


- action 视图控制层类自动注入.
- 视图解析器配置以及视图文件识别规范      

<!-- more -->
spring mvc 配置文件如下:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
 	http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
 	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!--<context:component-scan base-package="com.soccer.bat.controller" />-->

    <!--  spring mvc controller -->
    <context:component-scan base-package="com.soccer.bat" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice"/>
    </context:component-scan>

    <mvc:annotation-driven />
    
    <!-- For static resources -->
    <mvc:resources mapping="/image/**" location="/images/" />
    <mvc:resources mapping="/js/**" location="/js/" />
    <mvc:resources mapping="/css/**" location="/css/" />

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/WEB-INF/views/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>

</beans>
```



然后开始配置 `web.xml`文件,这里是容器启动初始化的依据, 内容如下:

```xml
<web-app id="WebApp_ID" version="2.4"
         xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee

http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

    <display-name>spring mvc</display-name>

    <!--  初始化spring context-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:/spring/applicationContext.xml</param-value>
    </context-param>

    <!-- spring全局监听 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>


    <listener>
        <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
    </listener>



    <filter>
        <filter-name>OpenEntityManagerInViewFilter</filter-name>
        <filter-class>org.springframework.orm.jpa.support.OpenEntityManagerInViewFilter</filter-class>
        <init-param>
            <param-name>entityManagerFactoryBeanName</param-name>
            <param-value>entityManagerFactory</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>OpenEntityManagerInViewFilter</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>

    <filter>
        <filter-name>Spring character encoding filter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>Spring character encoding filter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>



    <servlet>
        <servlet-name>spring-mvc</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath*:spring/spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>spring-mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

</web-app>

```



然后配置spring容器配置文件和 JPA支持. 就是`web.xml` 初始化参数的指定的配置文件 -- applicationContext.xml 内容如下:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:jpa="http://www.springframework.org/schema/data/jpa"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.1.xsd
        http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa-1.3.xsd">

    <!--<context:annotation-config/>-->
    <!--<context:component-scan base-package="com.soccer.bat"/>-->

    <!-- 自动扫描去除controller -->
    <context:component-scan base-package="com.soccer.bat.**">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <context:property-placeholder location="classpath*:/application.properties" />

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <!-- Connection Info -->
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />

        <!-- Connection Pooling Info -->
        <property name="maxActive" value="${dbcp.maxActive}" />
        <property name="maxIdle" value="${dbcp.maxIdle}" />
        <property name="defaultAutoCommit" value="false" />
        <!-- 连接Idle一个小时后超时 -->
        <property name="timeBetweenEvictionRunsMillis" value="60000" />
        <property name="minEvictableIdleTimeMillis" value="60000" />
    </bean>

    <bean id="hibernateJpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
        <property name="showSql" value="true"/>
        <property name="database">
            <value>MYSQL</value>
        </property>
    </bean>

    <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="packagesToScan">
            <list>
                <value>com.soccer.bat.model</value>
            </list>
        </property>
        <property name="jpaVendorAdapter" ref="hibernateJpaVendorAdapter">
        </property>
        <property name="jpaProperties">
            <props>
                <prop key="hibernate.current_session_context_class">thread</prop>
                <prop key="hibernate.hbm2ddl.auto">validate</prop><!-- validate/update/create -->
                <prop key="hibernate.show_sql">true</prop>
                <prop key="hibernate.format_sql">true</prop>
                <!-- 建表的命名规则 -->
                <prop key="hibernate.ejb.naming_strategy">org.hibernate.cfg.ImprovedNamingStrategy</prop>
            </props>
        </property>
    </bean>

    <!-- 自动扫描并注入Spring Data JPA -->
    <jpa:repositories base-package="com.soccer.bat.repository.**"
                      entity-manager-factory-ref="entityManagerFactory"
                      transaction-manager-ref="transactionManager">
    </jpa:repositories>

    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactory" />
    </bean>

    <tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>

    <bean class="org.springframework.orm.jpa.support.PersistenceAnnotationBeanPostProcessor" />

    <!-- 启动对@AspectJ（面向切面）注解的支持-->
    <aop:aspectj-autoproxy />

</beans>
```

这里需要注意配置包扫描路径,需要使用`**`来匹配某一个包下的所有包. 在这里我折腾了一天,以为他懂我意思呢,结果...  说出来都是泪 ~~~

接着配置数据源参数,例如mysql驱动/数据库访问用户名和密码等,内容如下:
```xml
#jdbc配置
jdbc.driver=com.mysql.jdbc.Driver
#jdbc.url=jdbc:mysql://172.17.70.244:3306/soccer?characterEncoding=UTF-8
jdbc.url=jdbc:mysql://127.0.0.1:3306/soccer
jdbc.username=root
jdbc.password=root
dbcp.maxIdle=5
dbcp.maxActive=40
```


然后是`logback`日志配置,方便查看启动信息,如果启动异常,随时可以在控制台查看启动日志便于排查. 我这里配置的很简单, 内容如下:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <Target>System.out</Target>
        <encoder>
            <pattern>%d{yy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>DEBUG</level>
        </filter>
    </appender>
    <logger name="com.soccer.bat" level="DEBUG" />
    <logger name="org.springframework" level="DEBUG" />
    <root level="DEBUG">
        <appender-ref ref="Console" />
    </root>
</configuration>
```

然后可以启动项目,如果没有问题可以尝试编写一个测试类,测试是否可以从数据库中查询到需要的数据. 我这里的数据持久层借口直接继承的是`Repository`标记接口,因为我暂时用不上JPA提供的方便方法. 
至此,我的第一次整合框架之旅就结束了,总体上来看还算是顺利的,主要在以下几个地方耗时严重:
- spring mvc 和 spring applicationContext配置文件的命名空间上折腾好了好久, tomcat启动总是提示不能解析这些文件头,最后在网上找一个一个改了改才好.
- spring包扫描路径的配置, 不知道需要配置 `**`才生效.





