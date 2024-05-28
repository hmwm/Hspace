---
title: 注解开发（1）
date: 2024-05-27 22:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image11.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image11.jpg
---
### Notes
### Notes

# 注解开发定义Bean

使用@Component注解把Bean交给Ioc容器管理

可按名称@Component(”id”)，可按类型（默认即可）

```java
@Component("bookService")
public class BookServiceImpl implements BookService {
		//
}
@Component
public class BookServiceImpl implements BookService {
		//
}
```

核心配置文件中通过组件扫描加载bean

```xml
//组件扫描可以范围扫描，所以可以不用写精确类地址
<context:component-scan base-package="com.mvnweb">
```

Spring提供**@Component**注解的三个衍生注解（功能相同，只是有助理解）

- @Controller 用于表现层bean定义
- **@Service 用于业务层bean定义【后续开发中常用】**
- @Repository 用于数据层bean定义
- 实际开发中只有@Service常用，其他两个注解有其他替代

*Spring提供了组件扫描注解，所以配置文件中的配置也可以不写，于是配置文件就没有必要存在，Spring同样提供了注解指定一个类为配置类来替换配置文件*

# 纯注解开发模式

换配置类仅仅是把核心配置文件换了一种形式来描述而已

Spring提供**@Configuration**注解来设定当前类为配置类

配合**@ComponentScan**注解将原来的配置文件完全代替，以达到简化开发目的

**@ComponentScan**注解用于设定扫描路径，此注解只能添加一次，多个数据请用数组格式

```java
@Configuration
@ComponentScan
public class SpringConfig {
}

//多数据
@ComponentScan({"com.dao", "com.service"})
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
 //以上配置文件结构转化为注解
 @Configuration
 
 <context:component-scan base-package="com.mvnweb">
 //扫描也转化为
 @ComponentScan(com.mvntest)
```

相应的，启动类中容器获取方式需要修改接口为
`AnnotationConfigApplicationContext()`来获取

```java
ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
```

