---
title: SpringMVC概念
date: 2024-06-21 23:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image21.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image21.jpg
---
### Notes

# SpringMVC概述

- SpringMVC是一种基于Java实现MVC模型的轻量级Web框架
- SpringMVC技术与Servlet技术功能等同，均属于web层开发技术（表现层）
- 优点
    - 使用简单，开发便捷（相对于Servlet）
    - 灵活性强

# 入门案例

springMVC入门程序总结（1+N），通常一个项目配置一次就行，请求层需要多次使用

- 一次性工作-配置
    - 创建工程，设置服务器，加载工程
    - 导入坐标
    - 创建web容器启动类，加载SpringMVC配置，并设置SpringMVC请求拦截路径
    - SpringMVC核心配置类（设置配置类，扫描controller包，加载Controller控制器bean）
- 多次工作（请求控制类，后面会细说，入门主要关注配置）
1. 创建Maven工程，选择web工程目录结构，设置Tomcat服务器用以启动项目。*【后续开发都是使用springboot快速启动开发，这里只要了解一下Tomcat的配置即可】*
2. 需要导入的坐标如下
    
    ```xml
    <dependency>
          <groupId>jakarta.servlet</groupId>
          <artifactId>jakarta.servlet-api</artifactId>
          <version>4.0.4</version>
          <scope>provided</scope>
        </dependency>
    
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>6.1.6</version>
        </dependency>
        
      //下面是tomcat和maven编译的插件，无需特别关注，后续使用springboot集成简化的这些配置
      <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
              <source>1.8</source>
              <target>1.8</target>
            </configuration>
          </plugin>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
    ```
    
3. 创建web容器启动类，加载SpringMVC配置，并设置SpringMVC请求拦截路径
    
    ```java
    //定义一个Servlet容器启动的配置类，在里面加载spring配置
    public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
        //加载springMVC容器配置
        @Override
        protected WebApplicationContext createServletApplicationContext() {
            AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
            context.register(SpringMvcConfig.class);
            return context;
        }
        //设置哪些请求归属springMVC处理
        @Override
        protected String[] getServletMappings() {
            return new String[] {"/"};
        }
        //加载spring容器的配置
    
        @Override
        protected WebApplicationContext createRootApplicationContext() {
            return null;
        }
    }
    ```
    
    这个启动器的作用就是使用servlet容器加载springMVC的配置，并拦截Tomcat的请求路径给到SpringMVC
    
4. SpringMVC核心配置类（设置配置类，扫描controller包，加载Controller控制器的bean）
    
    ```java
    //创建springMVC的配置文件，加载controller对应的bean
    @Configuration
    @ComponentScan("com.springweb.controller")
    public class SpringMvcConfig {
    }
    ```
    
5. Contoller请求控制器的基本写法和bean管理
    
    ```java
    @Controller
    public class UserController {
        //设置当前操作访问路径
        @RequestMapping("/save")
        //设置当前操作的返回值
        @ResponseBody
        public String save() {
            System.out.println("user save ...");
            return "{'modules':'springmvc'}";
        }
    }
    ```
    

### 补充一点

- AbstractDispatcherServletInitializer类是SpringMVC提供的快速初始化web3.0容器的抽象类
- AbstractDispatcherServletInitializer提供三个接口方法供用户实现（参考上文）

# 入门案例流程分析

- 服务器启动初始化过程
    - 服务器启动，执行ServletContainersInitConfig类，初始化web容器
    - 执行createServletApplicationContext方法，创建WebApplicationContext对象（SpringMVC容器）
    - 加载SpringMvcConfig
    - 执行@ComponentScan加载对应的bean
    - 加载UserController,每个@RequestMapping的名词对应一个具体方法（但方法和路径的映射不是在ControllerBean中配置的，spring有统一配置）
    - 执行getServletMappings方法，定义所有请求都通过SpringMVC
    
    ```java
    public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
        //加载springMVC容器配置
        @Override
        protected WebApplicationContext createServletApplicationContext() {
            AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
            context.register(SpringMvcConfig.class);
            return context;
        }
        //设置哪些请求归属springMVC处理
        @Override
        protected String[] getServletMappings() {
            return new String[] {"/"};
        }
        //加载spring容器的配置
    
        @Override
        protected WebApplicationContext createRootApplicationContext() {
            return null;
        }
    }
    ```
    
    ![Untitled](https://raw.githubusercontent.com/hmwm/vblog/main/pic/mvc.png)
    
- 单次请求过程
    - 发送请求localhost/save
    - web容器发现所有的请求都经过SpringMVC，将请求交给SpringMVC处理
    - 解析请求路径/save
    - 由/save匹配执行对应方法save()
    - 执行save()
    - 检测到有@ResponsBody直接将save()方法的返回值作为响应体返回给请求方。