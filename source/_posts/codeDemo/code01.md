---
title: '代码块演示'
date: '2024-5-9 21:16:00'
top_img: "./img/01.jpg"
cover: "./img/01.jpg"
--- 

```Java
public class SpringMyWebApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringMyWebApplication.class, args);
	}

}
```
# 内聚&耦合

**内聚**：软件中各个功能模块内部的功能联系。

**耦合**：衡量软件中各个层/模块之间的依赖，关联的程度。

**软件设计软则**：**高内聚低耦合**，模块内部的联系越紧密越好。层与层之间依赖关联越低越好，最好能解除耦合【一般只能降低，最多解除内部耦合，通过注解来转移到外部耦合】

# 分层解耦

1. 将new 实例对象部分删除，只留下变量声明。即留下功能接口，不显示创建实现类
2. 使用IOC（控制反转）和DI（依赖注入）注解

```java
@Service
public class EmpServices implements SPServices {
    @Autowired  //运行时，IOC容器会提供bean对象，赋值给变量，依赖注入
    private EmpDao empDao;
	  //...
}
```

## IOC&DI

**IOC控制反转**：inversion Of Control。对象的创建控制权由程序自身转移到外部（容器），这种思想称为控制反转。*【工具箱】*

**DI依赖注入**：Dependency Injection。容器为应用程序提供运行时，所依赖的资源，称之为依赖注入。

*【工具箱自动提供工具的功能】*

**Bean对象**：IOC容器中创建，管理的对象，称之为bean。*【工具箱中的工具】*


*【这里可以理解为一个工人原本完成工作一次只能带一把工具，如果要切换，就得出去重新换一把工具。现在工人选择好（在其他实现类加上@Component注解）需要的工具放入工具箱（IOC容器）然后带去屋子里工作，需要什么工具时，工具箱会自动提供（@Autowired装配）工具（bean），而且工人可以根据需要选择（@primary等）合适的工具。工具箱会按选择自动提供，那么所有准备工作都是在屋子外进行的，工人进来就只用专心完成屋子里的任务】*

### IOC控制反转

将对象的创建控制权交给IOC，由IOC来创建管理这些对象，容器中的对象也成为Bean对象，通过@Component或者衍生其衍生注解来声明bean对象

| 注解 | 说明 | 位置 |
| --- | --- | --- |
| @Conponent | 声明bean的基础注解 | 不属于以下三类时用此注解（比如一些工具类） |
| @Controller | @Conponent的衍生注解 | 标注在控制器上 |
| @Service | @Conponent的衍生注解 | 标注在业务逻辑类上 |
| @Repository | @Conponent的衍生注解 | b标注在数据访问类上（由于与mybatis整合，用得少） |
- bean名字一般默认实现类的首字母小写，也可用value声明
- 以上4个注解都能声明bean，但是在springboot集成web开发中，声明控制器controller的bean只能用@controller

**Bean组件扫描**

- 前面声明bean的四大注解，要想生效，还需要被组件扫描注解@ComponentSacn扫描 *【检查工具是否在工作台】*
- @ComponentSacn注解虽然没有显式配置，但实际上已经包含在启动类声明注解@SpringBootApplication中，默认扫描的范围是启动类所在包及其子包。 显示声明会覆盖默认扫描器，于是需要手动为其他类注解扫描*【工具台自带检查，不需要手动检查，当工具不在工具台时，扫描不成功，就没办法选择放入工具箱，如果工具不在，就需要带上扫描器去找，当然这时候默认的就不生效，工具太还得在留一个扫描器】*

### DI依赖注入 @Autowired

**@Autowired注解：**默认按类型进行，如果存在多个类型的bean，将会报错

- 按照提示，可以选择以下几种方案：
    
    @Primary：加在需要使用的实现类上
    
    @Qualifier(”bean的名称”)+@Autowired
    
    @Resource：不使用Autowired注入，而采用Resource加bean名。与auto不同，他是**默认以名称注入**【这个不是spring boot提供的】
	```java
	@Primary
	@Service
	public calss EmpServiceA implements EmpService{
	}
	// 
	@RestController
	public class EmpController{
		@Autowired
		@Qualifier("empServiceA")
		private EmpService empService;
	}
	//
	public class EmpController{
		@Resource(name = "empServiceA")
		private Empservice empService;
	}
	```