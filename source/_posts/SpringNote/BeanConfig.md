---
title: Bean的配置
date: 2024-05-15 23:58:25
tags: "Spring"
categories: "Note"
top_img: "./img/02.jpg"
cover: "./img/04.png"
---
### Notes

# Bean基础配置

| m名称 | bean |
| --- | --- |
| 类型 | 标签 |
| 功能 | 定义Spring核心容器管理的对象 |
| 属性列表 | id：bean的id，使用容器可以通过id值获取对相应bean，在一个容器中id为一class：bean的类型，配置bean的全路径类名 |
```xml
<!-- 格式 -->
<beans>
    <bean/>
    <bean></bean>
</beans> 
```

# Bean别名配置以及相关异常

bean对象可以设置别名，id和name是等同的

bean标签的name属性：可定义多个，使用逗号，分号，空格分隔

```xml
<bean id="bookDao" name ="bookDao2 bookDao3" class="com.mvnweb.dao.impl.BookDaoImpl"/>
# 进行绑定时ref可以绑定name，也可以绑定id
```

当我们使用未定义的bean名，会报**NoSuchBeanDefinitionException: No bean named 'bookDao4' available**

```java
BookDao bookDao = (BookDao) ctx.getBean("bookDao4"); 
```

所以遇到**NoSuchBeanDefinitionException:**时，检查getBean和配置文件定义的name

# Bean的非单例配置

控制bean创建实例的数量

| scope属性 | 定义bean的范围，可选范围如下：singleton 单例 (spring默认)prototype 非单例 |
| --- | --- |

```xml
<bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" scope="prototype">
```

### 为什么Spring要bean默认为单例？

非单例的话容器会创建无穷无尽的bean，所以Spring并不会管理非单例的bean，否则会对容器造成压力

适合交给容器进行管理的bean：

- 表现层对象
- 业务层对象
- 数据层对象
- 工具对象

***管理可复用对象***

不适合交给容器进行管理的bean

- 封装实体的域对象【状态的，记录了成员变量属性值】

***不可复用***

