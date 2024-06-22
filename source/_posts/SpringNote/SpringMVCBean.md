---
title: springMVC中Bean加载控制
date: 2024-06-22 00:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image22.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image22.jpg
---
### Notes

实际业务中不止有controler层，还有其他层，SpringMVC加载时需要只加载Controller层的bean

简单来说就是spring和SpringMVC分别加载自己的bean

# Controller加载控制与业务bean加载控制

- SpringMVC相关bean（表现层bean）
- Spring控制的bean
    - 业务bean（Service）
    - 功能bean（DataSource等）

- SpringMVC相关bean加载控制
    - SpringMVC加载的bean对应的包均在com.spring.controller包内
- Spring相关bean加载控制
    - 方式一：Spring加载的bean设定扫描范围为com.spring,排除掉controller包内的bean
    - 方式二：Spring加载的bean设定扫描范围为精准范围，例如service包，dao包
    - 方式三：不区分spring和springMVC的环境，加载到同一个环境中

方式一

```java
@Configuration
//方式一（常用）
@ComponentScan({"com.springweb.service", "com.springweb.dao"})
public class SpringConfig {
}
```

方式二

```java
//方式二 （springboot框架源码底层精细控制粒度时使用）
@ComponentScan(value = "com.springweb",
                excludeFilters = @ComponentScan.Filter(
                        type = FilterType.ANNOTATION,
                        classes = Controller.class
                )
)
public class SpringConfig {
}

//提供两种属性
//excludeFilters:扫描路径中加载的bean，需要指定类别（type）与具体项（class）
//includeFilters：加载指定的bean，需要指定类别（type）与具体项（class）

```

# bean加载格式

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
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(SpringConfig.class);
        return context;
    }
}
```

因为只需要提供配置类，也可以使用AbstractDispatcherServletInitializer的子类AbstractAnnotationConfigDispatcherServletInitializer提供的简化开发方式

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```
