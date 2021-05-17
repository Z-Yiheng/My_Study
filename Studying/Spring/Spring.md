# Spring



了解

- SSH：Struct2 + Spring + Hibernate
- SSM：SpringMvc + Spring + Mybatis



**优点**

1. ​	Spring是一个开源的免费框架（容器）
2. Spring是一个轻量级、非入侵式的框架
3. 控制反转（IOC），面向切面编程（AOP）
4. 支持事务的处理，对框架整合的支持



**核心：**

​	依赖注入（DI）

​	控制反转（IOC）

​	面向切面编程（AOP）



**总结一句话：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架**



**拓展：**

- Spring Boot
  - 一个快速开发的脚手架
  - 基于SpirngBoot可以快速开发单个微服务
  - 约定大于配置
- Spring Cloud
  - SpringCloud是基于SpringBoot实现的



**学习思路**：

​	Spring->Spring MVC->Spring Boot



## 1. IOC理论推导

1. UserDao接口
2. UserDaoImpl实现类
3. UserService业务接口
4. UserServiceImpl业务实现类



在之前的业务中，用户的需求可能会影响原来的代码，需要根据用户的需求去修改原代码，如果程序代码量庞大，修改一次的成本很昂贵

我门使用一个Set接口实现. 已经发送了革命性的变化

```java
private UserDao userDao;

//利用set进行动态实现值的注入！
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```



- 之前，程序是主动创建对象，控制权在程序员手上
- 使用set注入后，程序不在具有主动性，而是被动的接收对象

这种思想，从本质上解决问题，程序员不用去管理对象的创建。系统耦合性大大降低，可以更加专注的在业务层的实现上





## 2. IOC创建方式

1. 使用无参构造创建对象，默认

2. 有参构造方式

   1. 下标赋值

      ```xml
      <!--第一种，下标赋值-->
      <bean id="user" class="com.kuang.pojo.User">
      	<constructor-arg index="0" value="**"/>
      </bean>
      ```

   2. 类型

      ```xml
      <!--第二种，通过类型创建，不建议使用-->
      <bean id="user" class="com.kuang.pojo.User">
      	<constructor-arg type="java.lang.String" value="**"/>
      </bean>
      ```

   3. 参数名

      ```xml
      <!--第三种，直接通过参数名来设置-->
      <bean id="user" class="com.kuang.pojo.User">
      	<constructor-arg name="name" value="**"/>
      </bean>
      ```

      

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了



## 3. Spring配置



### 3.1 别名

```xml
<!--别名，如果添加了别名，外卖可以使用别名获取到这个对象-->
<alias name="user" alias="userNew"/>
```



### 3.2 Bean的配置

```xml
<!--
	id：baen的唯一标识符，也就是相当于我们学的对象名
	class：bean对象所对应的全限定名：包名 + 类型
	name：也是别名，而且name可以同时取多个别名
-->
<bean id="userT" class="com.kuang.pojo.UserT" name="user2 u2,u3;u4">
	<property name="name" value="***"/>
</bean>

```



### 3.3 import

import 一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

假设，先在项目中多个人开发，这三个人赋值不同的类开发，不同的类需要注册在不同bean中，我们可以利用import将所有人的beans.xml合并成一个总的

applicationContext.xml

```xml
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<impoet resource="beans3.xml"/>
```

使用的时候，直接使用总的配置就可以了



## 4. 依赖注入（DI）



### 4.1 构造器注入



### 4.2 Set方式注入【重点】

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入



【环境搭建】

- 复杂类型

```java
//几种类型
private String name;
private Address address;
private String[] books;
private List<String> hobbys;
private Map<String,String> card;
private Set<String> games;
private String wife;
private Properties info;
```

- 真实测试对象

```xml
<!--普通注入，value-->
<property name="name" value="**"/>

<!--Bean注入，ref-->
<propertr name="address" ref="address"/>

<!--数组-->
<property name="books">
	<array>
    	<value>红楼梦</value>
    	<value>西游记</value>
    	<value>水浒传</value>
    </array>
</property>

<!--List-->
<property name="hobbys">
	<list>
    	<value>听歌</value>
        <value>篮球</value>
        <value>看电影</value>
    </list>
</property>

<!--Map-->
<property name="card">
	<map>
    	<entry key="身份证" value="*********"/>
    </map>
</property>

<!--Set-->
<property name="games">
	<set>
    	<value>LOL</value>
        <value>DNF</value>
    </set>
</property>

<!--Null-->
<property name="wife">
	<null/>
</property>

<!--Properties-->
<property name="info">
	<props>
    	<prop key="学号">2131312</prop>
    </props>
</property>
```



## 5. Bean的自动装配

- 自动装配Spring满足Bean依赖的方式
- Spring会在上下文中自动寻找，并自动给bean装配属性



Spring中有三种装配的方式

1. 在xml中显示配置
2. 在java中显示配置
3. 隐式的自动装配Bean【重要】



### 5.1 测试

环境搭建：一个人有两个宠物！



### 5.2 ByName自动装配

```xml
<bean id="people" class="com.kuang.pojo.People" autowire="byName">
	<property name="name" value="***"/>
</bean>
```



### 5.3 ByType自动装配

```xml
<bean id="people" class="com.kuang.pojo.People" aotowire="byType">
	<property name="name" value="***"/>
</bean>
```

小结：

- byName的时候，需要保证所有bean的id唯一，并且这个bean需要喝自动注入的属性的set方法一致
- byType的时候，需要保证所有bean的class唯一，并且这个bean需要喝自动注入的属性的类型一致



### 5.4 使用注解实现自动装配



**@Autowired**

直接在属性上使用即可，也可以在set方式上使用！

使用Autowired我们可以不用编写Set方法，前提是这个自动装配的属性在IOC容器中存在，且符合名字byname

**@Qualifier**

如果@Autowired自动装配的环境比较浮躁，自动装配无法通过一个 @Autowired 完成的时候，我们可以使用      **@Qualifier(value="XXX")**  去配置@Autowired的使用，指定一个唯一的bean对象注入

------

**@Resource**

**小结：**

@Resource 和 @Autowired 【常用】的区别：

- 都是用来自动装配的。都可以放在属性字段上
- @ Autowired 通过 byname 的方式实现，并且必须要求这个对象存在
- @ Resource 默认通过 byname 的方式实现，如果找不到名字，则通过byType实现，如果两个都找不到的情况下，就报错



## 6. 使用注解开发

1、bean

2、属性如何注入

```java
@Component
public class User {
    public String name;
    
    @Value("***")
    public void setName(String Name) {
        this.name = name;
    }
}
```



3、衍生的注解

@Component 有几个衍生注解，我们在web开发中，会按照MVC三层架构分层

- dao【@Repository】
- service 【@Service】
- controller 【@Controller】
- 这四个注解功能都是一样的，都是代表将某个类注入到Spring中，装配Bean



4、自动装配

5、作用域

6、小结

xml 与 注解：

- xml 更加万能，适用于任何场合！维护简单方便
- 注解 不是自己类使用不了，维护相对复杂！



## 7. 代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！  【SpringAOP 和 SpringMVC】

代理模式的分类：

- 静态分类
- 动态分类

### 7.1 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色。代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人



代码步骤：

1. 接口

   ```java
   public interface Rent{
       public void rent();
   }
   ```

   

2. 真实角色

   ```java
   public class Host implements Rent {
       public void rent() {
           System.out.println("房东要出租房子！")
       }
   }
   ```

   

3. 代理角色

   ```java
   public class Proxy implements Rent {
       
       private Host host;
       
       public Proxy(){
       }
       
       public Proxy(Host host) {
           this.host = host;
       }
       
       public void rent() {
           seeHouse();
           host.rent();
           hetong();
           fare();
       }
       
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
       
       public void hetong(){
           System.out.println("签租赁合同")；
       }
       
       public void fare(){
           System.out.println("收中介费");
       }
   }
   ```

   

4. 客户端访问代理角色

```java
public class Client {
    public static void main(String[] args) {
        Host host = new Host();
        
        Proxy proxy = new Proxy(host);
        
        proxy.rent();
    }
}
```





代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用取关注一些公共的业务
- 公共也就代理角色，实现了业务的分工
- 公共业务发生拓展的时候，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率变低



## 8.动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分为两类：基于接口的动态代理，基于类的动态代理
  - 基于接口 --- JDK动态代理
  - 基于类：cglib
  - java字节码的实现：javasist

需要了解两个类：

​	Proxy：代理		InvocationHandler：调用处理程序



动态代理的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共也就交割代理角色，实现了业务的分工
- 公共业务发生拓展的时候，方便集中管理
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可