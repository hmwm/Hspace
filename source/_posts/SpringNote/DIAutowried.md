---
title: 自动装配
date: 2024-05-22 23:58:25
tags: "Spring"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image24.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image24.jpg
---
### Notes

# 依赖自动装配

- IoC容器根据bean所依赖的资源在容器中自动查找并注入到bean中的过程为自动装配
- 自动装配方式
    - **按类型（常用）**
    - 按名称
    - 按构造方法（不推荐）
    - 不启用自动装配（手动配置，参考上节）

# Set方式自动装配

```java
public class BookServiceImpl implements BookService {

    BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
	    //
    }

}
```

**bean标签属性：autowire**=" "

```xml
<bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl"/>
// 或<bean class="com.mvnweb.dao.impl.UserDaoImpl"/>
<bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" autowire="byType"/>
```

注意被装配的bean必须存在

当被装配的bean标签有多个时，一个实体类对应多个bean实例会报不唯一异常

即按**类型装配（autowire**=" byType"）时不能出现如下情况：

```xml
//一个实体类有多个bean实例对象
<bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl"/>
<bean id="userDao1" class="com.mvnweb.dao.impl.UserDaoImpl"/>

<bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" autowire="byType"/>
```

通常我们都是默认都是唯一，如果真要写两（多个）可以按**名称装配（autowire**=" byName"**）**

按名匹配：属性名对应类标id*【实际上用的名是set方法名取属性的首字母小写setBookDao→bookDao】*

```xml
<bean id="userDao1" class="com.mvnweb.dao.impl.UserDaoImpl"/>
<bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl"/>
<bean id="bookService" class="com.mvnweb.service.impl.BookServiceImpl" autowire="byName"/>
```

如果名不匹配：返回**Null Pointer Exception异常**（注入了没找到，返回一个空对象）

**总结：**通常还是**按类型**，避免改名出现问题，尽量降低耦合。

# 依赖自动装配特征

- 自动装配用于引用类型的依赖注入，不能对简单类型进行操作【1，2这种数值配置时还是比较抽象的】
- 使用按类型装配时（byType）必须保障容器中相同的bean唯一，推荐使用
- 使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因为变量名与配置耦合，不推荐使用
- 自动装配优先级低于setter注入与构造器注入，同时出现时，自动装配配置失效

# 几种集合注入方式

集合注入通常都是以简单数据类型为主，很少有引用类型，后续有时间再补充，这里主要是简单数据类型的注入

xml配置格式：其他大致相同，注意map和properties即可

map <entry key="ik" value="mo"/>

properties

引用类型 <ref bean="beanId">

```xml
<bean id="userDao" class="com.mvnweb.dao.impl.UserDaoImpl">
        <property name="arrayDemo">
            <array>
                <value>0</value>
                <value>1</value>
                <value>2</value>
               //引用类型 <ref bean="">
            </array>
        </property>
        <property name="list">
            <list>
                <value>"0"</value>
                <value>"1"</value>
                <value>"2"</value>
            </list>
        </property>
        <property name="set">
            <set>
                <value>it</value>
                <value>pc</value>
                <value>kit</value>
            </set>
        </property>
        <property name="map">
            <map>
                <entry key="ik" value="mo"/>
                <entry key="lo" value="pp"/>
                <entry key="ke" value="vl"/>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="ok">0</prop>
                <prop key="la">1</prop>
                <prop key="po">2</prop>
            </props>
        </property>
 </bean>
```

配置也是动态的，加入标签，对应添加元素

这里了解格式即可，集合注入通常框架使用得比较多，开发不常用