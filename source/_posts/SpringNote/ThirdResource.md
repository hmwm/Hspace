---
title: 第三方资源管理
date: 2024-05-24 21:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image7.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image7.jpg
---
### Notes

# 第三方资源管理

即第三方bean管理，拿到一个第三方资源的时候如何了解需要配置的bean

这里我们用C3p0举例（数据库连接池）

1. 安装C3p0依赖
2. 获取class时并不清楚到底需要导入一个类，这时候除了搜索外，可以尝试敲DataSource再根据提示找到c3p0的包。
    
    ![](https://raw.githubusercontent.com/hmwm/vblog/main/pic/importBean.png)
    
3. 但是这里并不是C3P0的包，需要注意，如果有多个，且不了解，就需要搜索
4. 配置项时，先进入包中查找构造方法和setter方法CTRL+F12，也可以尝试敲出常规标准的字段（比如url）再通过提示补全
    
    ![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/selectBean.png)
    

```xml
<bean class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value=""/>
        <property name="jdbcUrl" value=""/>
        <property name="user" value=""/>
        <property name="password" value=""/>
</bean>
```

其他第三方资源都可以按照这个套路进行bean管理

对于这些配置型的属性，通常都不是写死在xml文件中，未来方便管理，我们需要加载peoperties配置文件中的值到xml文件中

# 加载properties文件

1. 开启context命名空间，需要增加一些context属性，复制原本属性，替换beans为context

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
```

![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/Contextxml.png)

1. 使用context命名空间，加载指定的properties文件
    
    ![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/context.png)
    
2. 使用${}读取加载的属性值

![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/properties.png)

以上都是加载的固定套路

### 一些规范写法

读取当前配置文件的标准格式,如果读取其他jar包的配置文件，需要在类路径再加通配符

```xml
//当前
<context:property-placeholder location="classpath:*.properties" system-properties-mode="NEVER">
//外部
<context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER">
```

