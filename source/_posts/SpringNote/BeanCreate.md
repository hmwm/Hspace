---
title: Bean实例化的三种方式
date: 2024-05-18 23:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image27.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image27.jpg
---
Notes
# 一.使用构造器实例化对象

bean本质就是对象，可以使用构造方法进行实例化

提供一个可访问的构造方法（不写（默认），或者空参）

无参构造器不存在的情况：抛出**BeanCreationException**

```java
public BookDaoImpl() {
        System.out.println("book dao constructor is running...");
}
# 这里使用私有构造方法依然能实例化，可以想到，实例化过程是使用反射
private BookDaoImpl() {
        System.out.println("book dao constructor is running...");
}
```

![](https://raw.githubusercontent.com/hmwm/vblog/main/pic/Untitled.png)

可以看到bean确实可以使用构造方法实例化，但内部实现过程是通过**反射**，这里就涉及了后续需要学习的原理

### Spring报错如何看？

这里我们使用有参构造模拟报错环境，可以看见非常恐怖

```java
public BookDaoImpl(int i) {
        System.out.println("book dao constructor is running...");
}
```

![](https://raw.githubusercontent.com/hmwm/vblog/main/pic/Untitled2.png)

***但是先别慌，Sping异常从下向上找，Caused by开始***

1. 没有无参构造方法（这里的核心原因，有可能是spring内部异常，所以需要往上找）**NoSuchMethodException: com.mvnweb.dao.impl.BookDaoImpl.<init>()**
    
    ![](https://raw.githubusercontent.com/hmwm/vblog/main/pic/Untitled4.png)
    
2. 没有默认的构造方法被发现（原因是我们写了无参构造，不送参数，导致默认占用）**BeanInstantiationException: Failed to instantiate [com.mvnweb.dao.impl.BookDaoImpl]: No default constructor found**
    
    ![](https://raw.githubusercontent.com/hmwm/vblog/main/pic/Untitled3.png)
    
3. Bean实例化失败，原因是没有默认构造 （可以看出上一条原因，也被带到了这里的报错信息）**BeanCreationException: Error creating bean with name 'bookDao' defined in class path resource [applicationContext.xml]: Failed to instantiate [com.mvnweb.dao.impl.BookDaoImpl]: No default constructor found**

总结：spring的报错都是从下向上逐渐积压的，大部分时候从下第一个都能解决问题，除非遇到spring内部问题，所以需要向上找

# 二.使用静态工厂（了解）

早些年使用工厂来造对象，以进行一定的解耦，这里是简单工厂的一个例子

```java
//OrderDao.class
//OrderDaoImpl.class
//以上两个类默认存在，不写具体实例
public class OrderDaoFactory {
		//静态方法
    public static OrderDao getOrderDao() {
		    //工厂中会做一些额外配置，有时是必要的
		    //System.out.println("factory setup..");
        return new OrderDaoImpl();
    }
}

	//使用工厂静态方法获得对象 OrderDaoFactory.getOrderDao()
	OrderDao orderDao = OrderDaoFactory.getOrderDao();
	orderDao.save();
```

bean相应配置：这里真正造对象的是工厂方法getOrderDao

```xml
# class决定了这里是factory的对象，所以实例化需要使用factory-method
<bean id="orderDao" class="com.mvnweb.factory.OrderDaoFactory" factory-method="getOrderDao"/>
```

配置完后使用容器实例化orderDao对象

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao1 = (OrderDao) context.getBean("orderDao");
        orderDao1.save();
```

# 三.使用实例工厂（了解）

与静态工厂不同，实例化方法不带static，获取对象时就不能直接OrderDaoFactory.getOrderDao()

```java
//使用实例工厂获得对象 userDaoFactory.getUserDao();
UserDaoFactory userDaoFactory = new UserDaoFactory(); 
UserDao userDao = userDaoFactory.getUserDao();
userDao.save();

//实例工厂
public class UserDaoFactory {
    public UserDao getUserDao() {
        return new UserDaoImpl();
    }
}
```

既然是实例工厂，接下来先造工厂factory Bean对象，实例bean对象通过factory Bean提供方法来实例化bean

```xml
<bean id="userDaoFactory" class="com.mvnweb.factory.UserDaoFactory"/>
# 与静态不同的是，这里不再使用class，选择上面上面的工厂bean代替
<bean id="userDao" factory-bean="userDaoFactory" factory-method="getUserDao"/>
```

接下来还是一样使用Ioc容器对象来获取Bean

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = (UserDao) context.getBean("userDao");
        userDao1.save();
```

***现在会发现，这里的工厂Bean只是配合使用来造Bean对象，本身没有实际意义，而且方法不固定的话，每一次都需要配置。***

***于是Spring针对这种模式进行改良：***

***不造无用工厂bean，固定实例化方法***

# 四.使用FactoryBean实例化（务必掌握）

既然是***Spring***提供，那么自然想到他的实现标准，以及对应提供的接口：FactoryBean<?>

```java
//实现FactoryBean接口
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    @Override
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return UserDao.class;
    }
    //可选重写方法，不实现默认单例，重写设置为false非单例，一般不写
    @Override
    public boolean isSingleton() {
        return FactoryBean.super.isSingleton();
    }
}
```

这样做的好处是什么呢? → **简化配置**

可以看到，相比于前两种工厂，这里的配置十分简洁

```xml
<bean id="userDao" class="com.mvnweb.factory.UserDaoFactory"/>
```

***在最后学习框架整合的时候，大部分框架都使用这种方式与spring打交道***