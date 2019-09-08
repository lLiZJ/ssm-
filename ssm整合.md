## resources/mybaties/mybaties-config.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>

        <!-- 别名 -->
        <!-- 起別名后，不用在mappring resultType 填写全类名 -->
        <typeAliases>
            <package name="com.li.pojo" />
        </typeAliases>

    </configuration>
## resources/spring/spring-dao.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
        xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

        <!-- 配置 读取properties文件 jdbc.properties -->
        <context:property-placeholder location="classpath:db.properties" />
        
        <!-- 配置 数据源 -->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="${jdbc.driver}" />
            <property name="url" value="${jdbc.url}" />
            <property name="username" value="${jdbc.username}" />
            <property name="password" value="${jdbc.password}" />
        </bean>
        
        <!-- 配置SqlSessionFactory -->
        <bean class="org.mybatis.spring.SqlSessionFactoryBean">
            <!-- 设置MyBatis核心配置文件 -->
            <property name="configLocation" value="classpath:mybatis/mybatis-config.xml" />
            <!-- 设置数据源 -->
            <property name="dataSource" ref="dataSource" />
            <!-- 它表示我们的Mapper文件存放的位置，当我们的Mapper文件跟对应的Mapper接口处于同一位置的时候可以不用指定该属性的值。 -->
            <property name="mapperLocations" value="classpath:mappings/*.xml" />
            <!-- 那么在Mapper文件里面就可以直接写对应的类名 而不用写全路径名了  -->
            <!-- 跟mybatis中<typeAliases>作用一样 -->
            <!-- <property name="typeAliasesPackage" value="com.jeenotes.ssm.pojo"/> -->
        </bean>
        
        <!-- 配置Mapper扫描 -->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <!-- 设置Mapper扫描包 -->
            <property name="basePackage" value="com.li.dao" />
        </bean>
        
    </beans>
## resources/spring/spring-mvc.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
            http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        <!-- 配置Controller扫描 -->
        <context:component-scan base-package="com.li.controller" />
        
        <!-- 配置注解驱动 -->
        <mvc:annotation-driven />
        
        <!-- 对静态资源放行  -->
        <mvc:resources location="/css/" mapping="/css/**"/>
        <mvc:resources location="/js/" mapping="/js/**"/>
        <mvc:resources location="/fonts/" mapping="/fonts/**"/>
        <mvc:resources location="/frame/" mapping="/frame/**"/>
        <mvc:resources location="/images/" mapping="/images/**"/>
        <mvc:resources location="/style/" mapping="/style/**"/>
        <!-- 配置视图解析器 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <!-- 前缀 -->
            <property name="prefix" value="/WEB-INF/jsp/" />
            <!-- 后缀 -->
            <property name="suffix" value=".jsp" />
        </bean>
    </beans>
## resources/spring/spring-service.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
        xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

        <!-- 配置Service扫描 -->
        <context:component-scan base-package="com.li.service" />
    </beans>
## resources/db.properties
    jdbc.driver = com.mysql.cj.jdbc.Driver
    jdbc.url = jdbc:mysql:///stu?serverTimezone=GMT
    jdbc.username = root
    jdbc.password = 123456
## resources/log4j.properties
    # Global logging configuration
    log4j.rootLogger=DEBUG, stdout
    # Console output...
    log4j.appender.stdout=org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
## 依赖
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>3.8.1</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.1.1</version>
    </dependency>
    <!-- 实现sl4j接口并整合 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.1.1</version>
        <scope>test</scope>
    </dependency>
    <!-- 数据库驱动依赖 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.13</version>
    </dependency>
    <!-- 数据库依赖连接池 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.9</version>
    </dependency>
    <!-- DAO框架：Mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.2</version>
    </dependency>
    <!-- MyBatis整合Spring的适配包 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.1</version>
    </dependency>
    <!--Servlet web相关依赖 -->
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
    <!-- jstl标签库 -->
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <!-- 返回json字符串的支持 -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.8.8</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.0.1</version>
    </dependency>
    <!-- Spring依赖 -->
    <!--1. Spring核心依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <!--2. SpringDAO层依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <!--3. Spring WEB依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
    <!--4.Spring-test相关依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>4.3.7.RELEASE</version>
    </dependency>
## ssm目录结构
![https://github.com/lLiZJ/ssm-/blob/master/ssm.png]