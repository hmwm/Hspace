---
title: Spring核心思想
date: 2024-05-14 23:58:25
tags: 
top_img: "./img/02.jpg"
cover: "./img/04.png"
---
### Notes

# IoC（Inversion of Control）控制反转

目的：解耦

思想：使用对象时，由主动new对象转换为外部提供对象，**对象创建控制权由程序转移到外部**

Spring实现IOC思想：Spring提供了容器，IOC容器，用来充当思想中的“**外部**”

IOC容器：负责对象的创建，初始化等一系列工作，被创建或管理的对象在IoC容器中统称为“**Bean**”

```java
@Service
public class EmpServices implements SPServices {

    private EmpDao empDao;
	  //...

}
```

*此时程序运行还是会报错，因为只在容器中造了对象，但是service依赖于Dao对象才能运行。既然你需要Dao，那么顺带在把这个对象也给到你，这就是依赖注入*

<div align="center">
  <img src="https://hspacetest.oss-cn-wuhan-lr.aliyuncs.com/%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5.png" alt="DI（Dependency Injection）依赖注入">
</div>

# DI（Dependency Injection）依赖注入

在容器中**建立bean与bean之间的依赖关系**的整个过程，称为依赖注入

```java
@Service
public class EmpServices implements SPServices {
    @Autowired  //运行时，IOC容器会提供bean对象，赋值给变量，依赖注入
    private EmpDao empDao;
	  //...
    
}
```

# 核心思想

目标：充分解耦

1. 使用IoC容器管理bean（IoC）
2. 在IoC容器内将所有依赖关系的bean进行关系绑定（DI）

最终效果：使用对象时不仅可以直接从IoC容器中获取，并且获取到的bean已经绑定了所有依赖关系

---
📌 **SUMMARY:用对象时不要自己new，由IoC容器提供，容器所管理对象即为Bean，如果有关系，就进行绑定。**
---
