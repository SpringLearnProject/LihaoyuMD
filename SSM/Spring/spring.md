#### 一 ，spring简单试例的开发流程

1. 写springXML-绑定Bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.lihaoyu.Impl.UserDaoImpl">

    </bean>
</beans>
```



1. 写接口

```java
package com.lihaoyu.dao;

public interface UserDao {
    public void save();
}
```

1. 写实现类

```java
package com.lihaoyu.Impl;
import com.lihaoyu.dao.UserDao;
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("hello Spring");
    }
}
```

1. 写主方法

```java
package com.lihaoyu.demo;
import com.lihaoyu.dao.UserDao;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserDaoDemo {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("ApplictionContext.xml");
        UserDao userDao = (UserDao)app.getBean("userDao");
        userDao.save();
    }
}
```

> 报错：Process finished with exit code -1073741819 (0xC0000005)，查资料说将显卡驱动回滚到376.33可以解决，但我自身电脑无法适配375.33版本驱动。
>
> 在查找国内资料之后，说是和金山词霸的划词有关，就退出了金山词霸，果然问题解决了，妈的玄学。



#### 二，IOC理论推导

1. userDao接口
2. userDaoImpl接口实现类
3. UserService业务接口
4. UserService业务实现类 

#### 三，spring简单实例

1. bean类

```java
package com.li.Bean;

public class book {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "book{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

1. ApplictionContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="book" class="com.li.Bean.book">
        <property name="name" value="LIVE OR DEED"/>
    </bean>
</beans>
```

1. main函数（测试）

```java
import com.li.Bean.book;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyText01 {
    public static void main(String[] args) {
        ApplicationContext app=new ClassPathXmlApplicationContext("ApplictionContext.xml");
        book dk=(book) app.getBean("book");
        System.out.println(dk);
    }
}
```

#### 四，beanIOC构造器方式

1. 在实例类中写无参构造器

```xml
<bean id="user" class="com.li.Bean.User">
        <property name="name" value="lihaoyu"/>
        <property name="age" value="24"/>
</bean>
```

2. 在实例类中不写无参构造器

```xml
<bean id="user" class="com.li.Bean.User">
        <constructor-arg index="0" value="lihaoyu"/>
        <constructor-arg index="1" value="12"/>
        
    </bean>
```

3. 按照实例类中的数据类型（不推荐。如果相同数据类型有多个实力类的话就会出错）
4. 参数名

```xml
<bean id="user" class">
	<constructor-arg name="name" value="dachui"/>
	<constructor-arg name="age"  value="32"/>
</bean>
```

#### 五，Spring配置文件

##### 1. 别名：

> 代替id

```xml
<alias name="user" alias="dasdwa"/>
```

##### 2. Bean的基本配置

> 后面慢慢补充

| Class                    | [实例化 Beans](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-class) |
| ------------------------ | ------------------------------------------------------------ |
| Name                     | [命名 Beans](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-beanname) |
| Scope                    | [Bean的作用域](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes) |
| Constructor arguments    | [依赖注入](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-collaborators=) |
| Properties               | [依赖注入](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-collaborators) |
| Autowiring mode          | [自动化协作者](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-autowire) |
| Lazy initialization mode | [Lazy-initialized Beans](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-lazy-init) |
| Initialization method    | [初始化回调](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-lifecycle-initializingbean) |
| Destruction method       | [毁灭回调](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-lifecycle-disposablebean) |

###### 1，name

> 别名，可以多个（通过`空格`，`,`来分隔）。

```xml
<bean id="user" class="com.li.Bean.User" name="user2,us,dadw">
        <constructor-arg name="name" value="dachui"/>
        <constructor-arg name="age"  value="32"/>
</bean>
```

###### 2，scope

| 范围                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [singleton](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-custom) | (默认)为每个 Spring IoC 容器的单个 object 实例定义单个 bean 定义。 |
| （prototype）[原型](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-singleton) | 为任意数量的 object 实例定义单个 bean 定义。                 |
| （request）[请求](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-prototype) | 将单个 bean 定义范围限定为单个 HTTP 请求的生命周期。也就是说，每个 HTTP 请求都有自己的 bean 实例，该实例是在单个 bean 定义的后面创建的。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| [session](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-request) | 将单个 bean 定义范围限定为 HTTP `Session`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| （application）[应用](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-session) | 将单个 bean 定义范围限定为`ServletContext`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |
| [WebSocket](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#beans-factory-scopes-application) | 将单个 bean 定义范围限定为`WebSocket`的生命周期。仅在 web-aware Spring `ApplicationContext`的 context 中有效。 |

 ```xml
1,<bean id="userDao" class="com.lihaoyu.Impl.UserDaoImpl" scope="prototype"></bean>
2,<bean id="userDao" class="com.lihaoyu.Impl.UserDaoImpl""></bean>
 ```

```java
ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("ApplictionContext.xml");
        UserDao ud1= (UserDao) app.getBean("userDao");
        UserDao ud2=(UserDao) app.getBean("userDao");
        System.out.println(ud1);
        System.out.println(ud2);
```

```java
1,	com.lihaoyu.Impl.UserDaoImpl@55fe41ea
	com.lihaoyu.Impl.UserDaoImpl@fbd1f6
    
2,  com.lihaoyu.Impl.UserDaoImpl@2a5c8d3f
	com.lihaoyu.Impl.UserDaoImpl@2a5c8d3f
```

scope="singleton" 

bean实例化个数：1个

实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例。

1，对象创建：当对象加载，创建容器时，对象被创建了

2，对象运行：只要容器在，对象一直活着

3，对象销毁：当应用卸载，销毁容器时，对象被销毁了。

scope="prototype"

bean实例化个数：多个

实例化时机：当调用getBean（）方式被调用时。

1，对象创建：当使用对象时，对象被创建了

2，对象运行：只要对象在使用中，就一直活着

3，对象销毁：当对象长时间不用时，被java的垃圾回收器回收。

> request，session，appliction标签只能在web环境中使用。



##### 3. import

> 在有多个beanxml文件时，需要有一个总的xml文件，即便其中有一样的bean，也会选择一个进行创建。
>
> ```xml
> <import resource="bean3.xml"/>
> <import resource="bean45.xml"/>
> ```
>
> 

#### 六，依赖注入

##### 1，构造器注入

前面已经学习过

##### 2，set方式注入（重）

依赖注入：set注入

- 依赖

对象的创建依赖于容器

- 注入

Bean对象中的所有属性，由容器来注入

【环境搭建】

1. 复杂类型

   ```java
   package com.li.pojo;
   
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
       @Override
       public String toString() {
           return "Address{" +
                   "address='" + address + '\'' +
                   '}';
       }
   
       public Address(String address) {
           this.address = address;
       }
   
       public Address() {
       }
   }
   ```

2. 真实测试对象

   ```java
   private String name;
   private Address address;
   private String[] book;
   private List<String> hobby;
   private Map<String,String> card;
   private Set<String> games;
   private Properties info;//配置类
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="student" class="com.li.pojo.student">
           <property name="name" value="lihaoyu"/>
           <property name="address"  ref="address"/>
           <property name="book">
               <array>
                   <value>红楼梦</value>
                   <value>水浒传</value>
                   <value>西游记</value>
                   <value>三国演义</value>
               </array>
           </property>
           <property name="hobby">
               <list>
                   <value>读书</value>
                   <value>看报</value>
                   <value>零食</value>
                   <value>睡觉</value>
               </list>
           </property>
           <property name="games">
               <set>
                   <value>黑魂</value>
               </set>
           </property>
           <property name="info">
               <props>
                   <prop key="学号">1741310427</prop>
                   <prop key="driver">dadwad</prop>
                   <prop key="url">localhost</prop>
                   <prop key="passworld">123456</prop>
               </props>
           </property>
           <property name="card">
               <map>
                   <entry key="身份证" value="312312324214214214214"/>
               </map>
           </property>
       </bean>
       <bean id="address" class="com.li.pojo.Address">
           <property name="address" value="山西忻州"/>
       </bean>
   </beans>
   ```

   

4. 测试类

   ```java
   import com.li.pojo.student;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class mytext {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("ApplictionContext.xml");
           student li = (student) context.getBean("student");
           System.out.println(li.toString());
   /**
    * student{name='lihaoyu', 
    * address=Address{address='山西忻州'}, 
    * book=[红楼梦, 水浒传, 西游记, 三国演义], 
    * hobby=[读书, 看报, 零食, 睡觉], 
    * card={身份证=312312324214214214214}, 
    * games=[黑魂], 
    * info={学号=1741310427, url=localhost, driver=dadwad, passworld=123456}}
    */
       }
   }
   
   ```

   



##### 3，其他方式

###### p命名空间

> 不能直接使用，需要导入
>
> ```xml
>        xmlns:p="http://www.springframework.org/schema/p"
>        xmlns:c="http://www.springframework.org/schema/c"
> ```
>
>  

```xml
<bean id="user" class="com.li.pojo.User" p:age="23" p:nname="lihaoyu"/>
```



```java
public class mytext {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans01.xml");
        User li = (User) context.getBean("user");
        System.out.println(li.toString());
//  User{nname='lihaoyu', age=23}      
```

#### 七，Spring自动装配

- 自动配置是spring满足Bean依赖的一种方法
- spring会上下文自动寻找，并自动给bean装属性

Spring中三种装配方式

1. xml
2. java
3. 注解（隐式自动装配）`重`

##### 7.1 测试

1，环境搭建

> cat类
>
> ```java
> public class Cat {
>     public void nose(){
>         System.out.println("喵 喵");
>     }
> }
> ```
>
> 
>
> Dag类
>
> ```java
> public class Dag {
>     public void nose(){
>         System.out.println("汪 汪");
>     }
> }
> ```
>
> Person类
>
> ```java
> package com.li.pojo;
> 
> public class Person {
>     private Cat cat;
>     private Dag dag;
>     private String name;
> 
>     @Override
>     public String toString() {
>         return "Person{" +
>                 "cat=" + cat +
>                 ", dag=" + dag +
>                 ", name='" + name + '\'' +
>                 '}';
>     }
> 
>     public Cat getCat() {
>         return cat;
>     }
> 
>     public void setCat(Cat cat) {
>         this.cat = cat;
>     }
> 
>     public Dag getDag() {
>         return dag;
>     }
> 
>     public void setDag(Dag dag) {
>         this.dag = dag;
>     }
> 
>     public String getName() {
>         return name;
>     }
> 
>     public void setName(String name) {
>         this.name = name;
>     }
> 
>     public Person() {
>     }
> 
>     public Person(Cat cat, Dag dag, String name) {
>         this.cat = cat;
>         this.dag = dag;
>         this.name = name;
>     }
> }
> ```
>
> xml配置(原始配置)
>
> ```xml
> <bean id="cat" class="com.li.pojo.Cat"/>
> <bean id="dag" class="com.li.pojo.Dag"/>
> <bean id="person" class="com.li.pojo.Person">
> 	<property name="name" value="li"/>
>     <property name="dag" ref="dag"/>
>     <property name="cat" ref="cat"/>
> </bean>
> ```
>
> xml配置（自动装配）`autowire="byName"`
>
> ```xml
> <bean id="cat" class="com.li.pojo.Cat"/>
> <bean id="dag" class="com.li.pojo.Dag"/>
> <bean id="person" class="com.li.pojo.Person" autowire="byName">
>     <property name="name" value="li"/>
> </bean>
> ```
>
> `autowire="byType"`
>
> ==当使用引用对象时，使用byType是极其方便的，因为不会有类型冲突。==
>
> 不管是Type还是name，都是将set方法后面的对象属性按照name或者type来上下文检索，实现自动装配。==byname时 id必须唯一，bytype class必须唯一，==

##### 7.2 注解自动装配

###### 导入依赖xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context
                            http://www.springframework.org/schema/context/spring-context.xsd">
<context:annotation-config/>
    <bean id="cat" class="com.li.pojo.Cat"/>
    <bean id="dag" class="com.li.pojo.Dag"/>
    <bean id="person" class="com.li.pojo.Person">
        <property name="name" value="li"/>
    </bean>
</beans>
```

###### 7.2.1 @Autowired

每一个pojo都需要写一个注解才行，可以删除set方法，但要保留get和构造器。

```java
@Nullable  字段标注这个注解，说明这个字段可以为null 
```

但是当有多个相似的bean实现对一个class实例的装配时，可以使用

```java
<bean id="dag233" class="com.li.pojo.Cat"/>
----------------------------------------------------
@Autowired
@Qualifier(value = "dag233")
    private Cat cat;
```

==当装配环境比较复杂时，可以使用@Qualifier配合@Autowired指定唯一一个bean导入==

`@Resource`

```java
@Resource
private Cat cat;
@Resource
private Dag dag;
```

不需要spring也可以使用，是java自身所带的。适用于简单环境。==先找名字，之后找类型，都找不到才会报错。==

#### 八，使用注解开发

1. bean

   > ==@Component==等价于在xml文件中注册了一个Bean。会默认注册类名小写的id，调用时只需要调用这个id就好。
   >
   > 

   

2. 属性如何注入

   > ==@Value("lihaoyu")==,不需要写public String name="lihaoyu";
   >
   > 

3. 衍生注解

   > @Component有几个衍生注解，会按照MVC三层架构分层。
   >
   > 1. dao层
   >
   >    @Repository
   >
   > 2. service层
   >
   >    @Service
   >
   > 3. controller层
   >
   >    @Controller
   >
   > 这几个注解实际上都是一样的，都是将特定的类注入Spring的Bean中。

4. 自动装配

   @Autowired
   @Qualifier(value = "dag233")

   

   @Resource

5. 作用域

   ```java
   @Component
   @Scope("singleton")
   public class User {
   
       @Value("lihaoyu")
       public String name;
   }
   ```

6. 小结

   - XML

     万能方便，可以应对任何项目，维护简单。

     维护复杂

   - XML与注解最佳配置

     xml用来管理Bean，注解来注入属性。

     

#### 九，基于java配置Spring（完全不需要xml配置）

pojo类：

```java
package com.li.pojo;

import org.springframework.beans.factory.annotation.Value;

public class User {
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }
    @Value("lihaoyu")
    public void setName(String name) {
        this.name = name;
    }

    public User() {
    }

    public User(String name) {
        this.name = name;
    }
}
```

config类

```java
package com.li.config;

import com.li.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@Import(AppConfig2.class)//引用另一个配置类
@ComponentScan("com.li.pojo") //扫描pojo包
public class AppConfig {
    @Bean
    public User getUser(){
        return new User();//返回pojo类，使用@bean注解
    }
}
```

text类

```java
import com.li.config.AppConfig;
import com.li.pojo.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class mytext {
    @Test
    public void Text(){
        ApplicationContext appConfig = new AnnotationConfigApplicationContext(AppConfig.class);
        User user = (User) appConfig.getBean("getUser");
        System.out.println(user.getName());
    }
}
```



#### 十，代理模式

springAOP的底层就是代理模式（springaop和springMVC）

- 静态代理：抽象的中间件来满足双方的需求
- 动态代理：

#### 十一：AOP

面向切面编程就是在一个纵向的业务中，你的每一个模块都需要某一项功能，例如安全，缓存，事务或者日志。就像用刀把多层的面包切开，在前面或者后面涂上番茄酱，所以叫切面编程。

![image-20200818104632702](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200818104632702.png)

1. 可以使用代理来实现：但当类多时就要写无数的代理方法，不符合开发原则，而且会导致继承体系脆弱。
2. 使用spring自带的方式。

##### aop的执行过程

![image-20200818105617476](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200818105617476.png)

###### 通知：

通知定义了切面是什么以及何时使用。除了描述切面要完成的工作，通
知还解决了何时执行这个工作的问题。它应该应用在某个方法被调用之
前？之后？之前和之后都调用？还是只在方法抛出异常时调用？

![image-20200818105716295](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200818105716295.png)

###### 连接点

连接点是在应用执行过程中能够插入切面的一个点。这个点可
以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利
用这些点插入到应用的正常流程之中，并添加新的行为。

###### 切点：

定义了那里的问题

###### 织入：

织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指定
的连接点被织入到目标对象中。在目标对象的生命周期里有多个点可以
进行织入：

1. 编译期：切面在目标类编译时被织入。这种方式需要特殊的编译
   器。AspectJ的织入编译器就是以这种方式织入切面的
2. 类加载期：切面在目标类加载到JVM时被织入。这种方式需要特殊
   的类加载器（ClassLoader），它可以在目标类被引入应用之前增
   强该目标类的字节码。AspectJ 5的加载时织入（load-time
   weaving，LTW）就支持以这种方式织入切面。
3. 运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入
   切面时，AOP容器会为目标对象动态地创建一个代理对象。Spring
   AOP就是以这种方式织入切面的。

![image-20200818161509022](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200818161509022.png)

##### 方式一：

>spring Xml配置：
>
>```xml
><?xml version="1.0" encoding="UTF-8"?>
><beans xmlns="http://www.springframework.org/schema/beans"
>       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>       xmlns:aop="http://www.springframework.org/schema/aop"
>       xsi:schemaLocation="http://www.springframework.org/schema/beans
>       http://www.springframework.org/schema/beans/spring-beans.xsd
>       http://www.springframework.org/schema/aop
>       https://www.springframework.org/schema/aop/spring-aop.xsd">
><!-- 注册   -->
>    <bean id="log" class="com.li.log.log"/>
>    <bean id="userService" class="com.li.server.UserServiceImpl"/>
>    <bean id="afterLog" class="com.li.log.afterlog"/>
><!--  方式一： 使用原生的spring Api接口 -->
>    <aop:config>
><!--        切入点  expression：表达式 * 所要切入的类下面的所有方法（使用*代替）以及方法中的参数，使用(..)表示-->
>        <aop:pointcut id="pointcut" expression="execution(* com.li.server.UserServiceImpl.*(..))"/>
><!--    执行环绕增加！-->
>        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
>        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
>    </aop:config>
>
></beans>
>```
>
>service 接口类：
>
>```java
>package com.li.server;
>
>public interface userservice {
>    public void add();
>    public void delete();
>    public void update();
>    public void select();
>}
>
>```
>
>service实现类：
>
>```java
>package com.li.server;
>
>public class UserServiceImpl implements userservice {
>
>    public void add() {
>        System.out.println("增加了一个用户");
>    }
>
>    public void delete() {
>        System.out.println("删除了一个用户");
>    }
>
>    public void update() {
>        System.out.println("更新了一个用户");
>    }
>
>    public void select() {
>        System.out.println("查询了一个用户");
>    }
>}
>```
>
>通知类：
>
>```java
>package com.li.log;
>
>import org.springframework.aop.MethodBeforeAdvice;
>
>import java.lang.reflect.Method;
>
>public class log implements MethodBeforeAdvice {
>
>    public void before(Method method, Object[] objects, Object o) throws Throwable {
>        System.out.println(getClass().getName()+"的"+method.getName()+"被执行");
>        System.out.println("helloworld!");
>    }
>
>}
>
>```
>
>
>
>测试类：
>
>```java
>import com.li.server.UserServiceImpl;
>import com.li.server.userservice;
>import org.junit.Test;
>import org.springframework.context.support.ClassPathXmlApplicationContext;
>
>import java.io.IOException;
>
>public class mytext {
>    @Test
>    public void text01()throws IOException{
>        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("ApplictionContext.xml");
>//        动态代理代理的是接口，所以类型只能是userserveice而不能是具体的实现类
>        userservice userServiceImpl = context.getBean("userService", userservice.class);
>        userServiceImpl.add();
>
>    }
>}
>```
>
>测试结果：
>
>![image-20200825172112442](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200825172112442.png)
>
>

##### 方式二：

> 使用自定义类实现AOP
>
> ==实现类==
>
> ```java
> package com.li.diy;
> 
> public class diyPointCut {
>     public void after(){
>         System.out.println("方法执行之后");
>     }
>     public void before(){
>         System.out.println("方法执行之前");
>     }
> }
> ```
>
> ==配置类==
>
> ```xml
> <!--    方法二-->
>     <bean id="diy" class="com.li.diy.diyPointCut"/>
>     <aop:config>
>         <aop:aspect ref="diy">
> <!--            切入点-->
>             <aop:pointcut id="point" expression="execution(* com.li.server.UserServiceImpl.*(..))"/>
> 
> <!--            通知-->
> 
>             <aop:after method="after" pointcut-ref="point"/>
>             <aop:before method="before" pointcut-ref="point"/>
>         </aop:aspect>
>     </aop:config>
> ```
>
> 

##### 方式三：

> 使用java注解
>
> ==实现类==
>
>  ```java
> package com.li.diy;
> 
> import org.aspectj.lang.annotation.After;
> import org.aspectj.lang.annotation.Aspect;
> import org.aspectj.lang.annotation.Before;
> 
> @Aspect
> public class AnnotionConotext {
>     @Before("execution(* com.li.server.UserServiceImpl.*(..))")
>     public void before(){
>         System.out.println("之前");
>     }
>     @After("execution(* com.li.server.UserServiceImpl.*(..))")
>     public void after(){
>         System.out.println("之后");
>     }
> }  
>  ```
>
> ==配置类==
>
> ```xml
>   <!--  第三种  -->
>     <bean id="annotiobcontext" class="com.li.diy.AnnotionConotext"/>
> <!--    开启注解支持-->
>     <aop:aspectj-autoproxy/>
> ```

#### 十二：mybatis整合spring

##### 依赖配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringLearn</artifactId>
        <groupId>SpringLearn</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>Spring-10-Spring-Mybatis</artifactId>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.5</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.5.4</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.5</version>
        </dependency>
    </dependencies>

</project>
```

