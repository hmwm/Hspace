---
title: 依赖注入方式
date: 2024-05-19 00:01:50
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image14.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image14.jpg
---
### Notes

- 思考，向一个类中传递数据有几种方式？
    - 普通方法（set）
    - 构造方法
- 依赖注入描述了在容器中建立bean与bean之间的依赖关系的过程，如果bean运行需要的是数字或字符串呢？【简单说，注入的不是new对象，而是数字之类的】
    - **setter注入**：简单类型（简单数据类型和String），引用类型
    - **构造器注入**：简单类型，引用类型

# setter注入（自开发常用）

### 引用类型

- 在bean中定义引用类型属性并提供可访问的set方法
    
    ```java
    UserDao userDao;
    
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    ```
    
- 配置中使用property标签ref属性注入引用类型对象
    
    ```xml
    <bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" >
         <!--配置service与dao的关系-->
         <property name="userDao" ref="userDao"/>
    </bean>
    ```
    

### 简单类型（基本数据类型String）

- 在bean中自定义引用属性并提供可访问set方法
    
    ```java
    private int connectionNumber;
    private String connectionName;
    private Integer MaxConnectionNum;
    
    		**public void setConnectionNumber(int connectionNumber) {
            this.connectionNumber = connectionNumber;
        }
    
        public void setConnectionName(String connectionName) {
            this.connectionName = connectionName;
        }
    
        public void setMaxConnectionNum(Integer maxConnectionNum) {
            MaxConnectionNum = maxConnectionNum;
        }**
    ```
    
- 配置中使用property标签value属性注入简单数据类型
    
    ```xml
    <bean id="bookDao" name ="bookDao2 bookDao3" class="com.mvnweb.dao.impl.BookDaoImpl">
            <property name="connectionNumber" value="10"/>
            <property name="maxConnectionNum" value="11"/>
            <property name="connectionName" value="mybatis"/>
    </bean>
    ```
    

*对于**`Integer`**等包装类类型属性，尽管它们是引用类型，但在Spring配置文件中可以通过**`value`**属性直接配置其值，Spring会自动完成类型转换。*

# 构造器注入（框架常用）

构造器注入与setter注入大同小异，基本只是更换了标签名以及name绑定的不再是属性，而是构造器的参数名。

标签名更换constructor-arg

name绑定构造器的参数

### 引用类型

- 在bean中定义引用类型属性并提供可访问的**构造**方法
    
    ```java
    UserDao userDao;
    
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    ```
    
- 配置中使用constructor-arg标签ref属性注入引用类型对象
    
    ```xml
    <bean id="bookDao" class="com.mvnweb.dao.impl.BookDaoImpl"/>
    <bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl"/>
    
    <bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" >
            <constructor-arg name="bookDao" ref="bookDao"/>
            <constructor-arg name="userDao" ref="userDao"/>
    </bean>
    ```
    

### 简单类型（基本数据类型String）

- 在bean中自定义引用属性并提供可访问构造方法
    
    ```java
    private int connectionNumber;
    private String connectionName;
    
    public BookDaoImpl(int connectionNumber, String connectionName) {
            this.connectionNumber = connectionNumber;
            this.connectionName = connectionName;
    }
    		
    ```
    
- 配置中使用constructor标签value属性注入简单数据类型
    
    ```xml
    <bean id="bookDao" class="com.mvnweb.dao.impl.BookDaoImpl">
    	  <constructor-arg name="connectionName" value="mysql"/>
    	  <constructor-arg name="connectionNumber" value="11"/>
    </bean>
    ```
    

可以看到，使用构造器注入出现了紧耦合（参数名变化，配置也需要变化），有什么方法可以解耦呢？

### 构造器参数适配（了解）

提供两种方式解决上述耦合情况，但都不是完善的解决方案，灵活了解即可【***图一乐***】

- 配置中使用constructor标签type属性设置形参类型注入
- 配置中使用constructor标签index属性设置形参位置注入

```xml
# 解决了参数名问题，形参名不耦合了，但是类型相同的参数出现就寄咯
<bean id="bookDao" class="com.mvnweb.dao.impl.BookDaoImpl">
		<constructor-arg type="java.lang.String" value="mysql"/>
	  <constructor-arg type="int" value="10"/>
</bean>
# 解决了参数类型重复问题
<bean id="bookDao" class="com.mvnweb.dao.impl.BookDaoImpl">
		<constructor-arg index="0" value="mysql"/>
	  <constructor-arg index="1" value="10"/>
</bean>
```

# 依赖注入方式选择

1. **强制依赖使用构造器**进行，使用setter注入有概率不进行注入导致null对象出现
2. **可选依赖使用setter注入**进行，灵活性强
3. **Spring框架**倡导**使用构造器**,第三方框架内部大多数采用构造器注入的形式进行数据初始化，**相对严谨**
4. 如果**有必要**可以**两者同时使用**,使用构造器注入完成强制依赖的注入，使用setter注入完成可选依赖的注入
5. 实际开发过程中还要**根据实际情况**分析，如果**受控对象没有提供setter方法**就**必须使用构造器注入【别人的bean】**
6. **自己开发的模块推荐使用setter注入 【自己用更方便的，不得已情况再用构造器】**

*构造器一旦指定必须给参数，否则报错，所以是比较严谨的方式【做框架是很严谨的奥】*

*setter因为可选则注入，不注入时会产生null对象*

---
📌 **: 依赖注入感觉这么麻烦，不可以自动注入吗？有办法吗？有！下一节  自动装配**
---