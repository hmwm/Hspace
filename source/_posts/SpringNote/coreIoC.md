---
title: 容器相关
date: 2024-05-26 21:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image12.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image12.jpg
---
### Notes

# 创建容器

1. 加载类路径下的配置文件（**常用**）
    
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    ```
    
2. 从文件系统下加载配置文件（使用绝对路径）
    
    ```java
    ApplicationContext context1 = new FileSystemXmlApplicationContext();
    ```
    
3. 也是可以加载多个配置文件
    
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml", "bean2.xml");
    ```
    

# 获取bean

1. 使用bean名称获取（需强转）
    
    ```java
    UserDao userDao = (UserDao) context.getBean("userDao");
    ```
    
2. 使用bean名称获取并指定类型（和1没啥区别）
    
    ```java
    UserDao userDao1 = context.getBean("userDao", UserDao.class);
    ```
    
3. 使用bean类型获取（保证该类型bean只有一个）
    
    ```java
    UserDao userDao2 = context.getBean(UserDao.class);
    ```
    

# 容器类层次结构

BeanFactory是容器的顶层接口，后续用到的接口是又它一级一级发展而来，知道层次结构就行，也可以领会一下这里功能接口设计的思想

![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/BeanFactory.png)

# BeanFactory

BeanFactory也可以作为容器管理bean，但是先开发中已经不用，了解即可。

还有一个特点是BeanFactory是**延迟加载Bean的（启动时没有立即加载）**，如果ApplicationContext想要实现，可以在bean配置文件加入lazy-init属性为true