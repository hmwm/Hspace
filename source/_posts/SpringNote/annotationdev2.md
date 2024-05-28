---
title: 注解开发（2）
date: 2024-05-28 22:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image34.png
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image34.png
---
### Notes

# Bean管理

### bean作用范围

使用**@Scope**定义bean作用范围

```java
@Component
@Scope("singleton")
public class BookServiceImpl implements BookService {
}
```

### bean生命周期

使用@PostConstruct，@PreDestory定义bean生命周期

(该注解已经在java11以上被删除，了解即可)

```java
@Component
public class BookServiceImpl implements BookService {
		@PostConstruct
    public void init() {

    }
    @preDestroy
    public void destroy() {

    }
}
```

# 依赖注入

### 引用类型

使用**@Autowired**注解开启自动装配模式（按类型）

```java
public class BookServiceImpl implements BookService {
    @Autowired
    BookDao bookDao;
    
    public void save() {
	    bookDao.save();
    }
}

```

- 自动装配基于**反射设计创建对象**并**暴力反射**对应属性**为私有属性初始化数据**，因此无需提供setter方法
- 自动装配**建议使用无参构造方法创建对象**（默认），如果不提供对应构造方法，请提供唯一的构造方法
- 如果想使用名称装配(*bean不唯一情况*)可以使用@Qualifiler注解（必须配合@Autowired）
    
    ```java
    public class BookServiceImpl implements BookService {
        @Autowired
        @Qualifier("bookDao")
        BookDao bookDao;
        
        public void save() {
    	    bookDao.save();
        }
    }
    ```
    
    ### 简单类型
    
    使用**@Value**实现简单类型注入
    
    ```java
    public class BookServiceImpl implements BookService {
    		@Value("100")
    		private String connectionNum;
    }
    ```
    
    这里的**值**通常会来自于外部的property文件，避免值写死
    
    在配置类中使用**@PropertySource**加载property文件
    
    ```java
    @Configuration
    @ComponentScan("com.mvn")
    @PropertySource("classpath:jdbc.properties") //多个配置文件时也使用数组 @PropertySource({"jdbc.properties"},{"jdbc.properties"})
    public class SpringConfig {
    }
    
    //注入修改
    //name为配置文件中的配置项
    public class BookServiceImpl implements BookService {
    		@Value("${name}")
    		private String connectionNum;
    }
    ```
    
    注意：路径仅支持单一文件配置，多文件请使用数组格式配置，且不允许使用通配符*
    
    # 第三方bean管理 @Bean
    
    整合框架时都需要@Bean来管理别人写的对象
    
    使用**@Bean**配置第三方bean
    
    ```java
    public class JdbcConfig {
        //定义一个方法获取要管理的bean
        //添加@Bean，表示当前方法的返回值是一个bean
        @Bean
        public DataSource dataSource() {
            ComboPooledDataSource dataSources = new ComboPooledDataSource();
            dataSources.setDriverClass("com.jdbc.mysql.Driver");
            dataSources.setJdbcUrl("jdbc:mysql://localhost:3306/springWeb");
            dataSources.setUser("root");
            dataSources.setPassword("root");
            return  dataSources;
        }
    }
    ```
    
    这里本应该是写在核心配置类中的，但是不建议这样做，管理大量第三方bean时不方便，所以单独创建了JdbcConfig类来管理
    
    然后使用**@Import**将它导入到核心配置类中（导入式）
    
    同样@Import只允许写一次，多个导入使用数组
    
    ```java
    @Configuration
    @ComponentScan
    @Import({JdbcConfig.class})
    public class SpringConfig {
    
    }
    ```
    
    还有一种扫描式，即为JdbcConfig加上@Configuration注解，在核心文件使用组件扫描该类。但是不推荐（第三方bean信息不透明），这不具体展开。
    
    # 第三方bean依赖注入
    
    ### 简单类型注入：成员变量
    
    上一段中的简单类型仍然是写死到代码中的，这里做一下改写
    
    ```java
    public class JdbcConfig {
        @Value("com.jdbc.mysql.Driver")
        private String driver;
        @Value("jdbc:mysql://localhost:3306/springWeb")
        private String url;
        @Value("root")
        private String userName;
        @Value("root")
        private String password;
        //定义一个方法获取要管理的bean
        //添加@Bean，表示当前方法的返回值是一个bean
        @Bean
        public DataSource dataSource() throws PropertyVetoException {
            ComboPooledDataSource dataSources = new ComboPooledDataSource();
            dataSources.setDriverClass(driver);
            dataSources.setJdbcUrl(url);
            dataSources.setUser(userName);
            dataSources.setPassword(password);
            return  dataSources;
        }
    }
    ```
    
    ### 引用类型依赖注入：方法形参
    
    只需要为bean定义方法设置形参即可，容器会根据类型自动装配对象
    
    ```java
     @Bean
        public DataSource dataSource(BookService bookService) {
            bookService.save();
            ComboPooledDataSource dataSources = new ComboPooledDataSource();
         
            return  dataSources;
        }
    ```
    

