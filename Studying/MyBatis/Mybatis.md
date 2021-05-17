# Mybatis

## 1. 简介

### 1.1 什么是MyBatis

- MyBatis 是一款优秀的**持久层框架**
- 它支持自定义 SQL、存储过程以及高级映射
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录



> 获取Mybatis
>
> - maven仓库
>
> ```xml
> <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
> <dependency>
>     <groupId>org.mybatis</groupId>
>     <artifactId>mybatis</artifactId>
>     <version>3.5.6</version>
> </dependency>
> 
> ```
>
> 
>
> - GitHub
>
>   https://github.com/mybatis/mybatis-3/releases
>
> - 中文文档
>
>   https://mybatis.org/mybatis-3/zh/index.html

### 1.2 持久化

数据持久化

- 持久化就是将程序的数据在持久状态和顺势状态转化的过程
- 内存：**断电即失**
- 数据库(jdbc)、io文件持久化

**为什么需要持久化**

- 有一些对象不能丢失
- 内存太贵



### 1.3 持久层

Dao层、Service层、Controller层...

- 完成持久化工作的代码块
- 层界限十分明显



### 1.4 为什么需要Mybatis

- 帮助程序猿将数据存入数据库中
- 方便
- 传统的JDBC代码太复杂。简化框架



## 2.第一个Mybatis程序

思路：搭建环境-->导入Mybatis-->测试

### 2.1 搭建环境

搭建数据库

```mysql
CREATE DATABASE `mybatis`;

USE `mybatis`;

CREATE TABLE `user`(
 `id` INT(20) NOT NULL PRIMARY KEY,
 `name` VARCHAR(10) DEFAULT NULL,
 `pwd` VARCHAR(20) DEFAULT NULL
 )ENGINE=INNODB DEFAULT CHARSET=utf8;
 
 INSERT INTO `user`(`id`,`name`,`pwd`) VALUES
 (1,'张三','123456'),
 (2,'李四','567890')
```

**新建项目**

- 新建一个Maven项目
- 删除src
- 导入maven依赖（pom.xml中导入)

```xml
<!--导入依赖-->
    <dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.48</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

### 2.2 创建一个模块

- 编写Mybatis核心配置文件

在resources创建**'mybatis-config.xml'**,核心文件写在这里面

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

</configuration>
```

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册-->
    <mappers>
        <mapper resource="com/Z/dao/UserMapper/xml"/>
    </mappers>
```



- 编写工具类

创建package-->utils-->MybatisUtils编写工具类

```java
//sqlSessionFactory --> sqlSession
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用Mybatis第一步：获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
```



### 2.3 编写代码

- 实体类

创建package-->pojo-->User编写实体类

```java
//实体类
public class User {
    private int id;
    private String name;
    private String pwd;


    public User() {
    }
    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```



- Dao接口

创建package-->dao-->UserDao(Interface)编写接口

```java
//接口
public interface UserDao {
    List<User> getUserList();
}

```



- 接口实现类

dao-->UserMapper.xml编写接口类

由原来的UserDaoImpl转变为一个Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.Z.dao.UserDao">
    <!--查询语句-->
    <select id="getUserList" resultType="com.Z.pojo.User">
        select *from mybatis.user
    </select>
</mapper>
```





### 2.4 测试

在package(test)创建与mian相同的包

![image-20210405111209723](C:\Users\张以恒\AppData\Roaming\Typora\typora-user-images\image-20210405111209723.png)

注意点：

> org.apache.ibatis.binding.BindingException: Type interface com.Z.dao.UserDao is not known to the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册mappers

```xml
<!--在build中配置resources，来防止我们资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```



- junit测试

```java
@Test
    public void test() {

        //第一步：获得SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        //方式一：getMapper
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
```

可能会遇到的问题：

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. Maven导出资源问题

------

**问题**

- 

> Cause: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

在mybatis-config.xml文件中做如下操作

```xml
<property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
```



## 3.  CRUD

### 3.1 namespace

namespace中的包名要和Dao/mapper接口一致！

### 3.2 select

选择，查询语句；

- id：就是对应namespace中的方法名；
- resultType：Sql语句执行的返回值；
- parameterType：参数类型

1. 编写接口

   ```java
    List<User> getUserList();
   ```

2. 编写对应的mapper中的语句

   ```xml
   <select id="getUserList" resultType="com.Z.pojo.User">
           select *from mybatis.user
   </select>
   ```

   

3. 测试

   ```java
   @Test
       public void test() {
   
           //第一步：获得SqlSession对象
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           //方式一：getMapper
           UserDao userDao = sqlSession.getMapper(UserDao.class);
           List<User> userList = userDao.getUserList();
   
           for (User user : userList) {
               System.out.println(user);
           }
   
           //关闭SqlSession
           sqlSession.close();
       }
   ```

   

### 3.3 Insert

1. 编写接口

   ```java
    int addUser(User user);
   ```

2. 编写对应的mapper中的语句

   ```xml
    <!--添加用户-->
       <insert id="addUser" parameterType="com.Z.pojo.User" >
           insert into mybatis.user (id, name, pwd) values (#{id},#{name},#{pwd});
       </insert>
   ```

   

3. 测试

   ```java
   @Test
       public void addUser(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           UserDao mapper = sqlSession.getMapper(UserDao.class);
   
           int res = mapper.addUser(new User(3,"lll","123456"));
           if (res>0){
               System.out.println("插入成功");
           }
   
           //提交事务
           sqlSession.commit();
           sqlSession.close();
       }
   ```

   

### 3.4 update

```xml
<update id="updateUser" parameterType="com.Z.pojo.User">
        update mybatis.user
        set name = #{name },pwd = #{pwd}
        where id = #{id};
</update>
```



### 3.5 Delete

```xml
<!--删除一个用户-->
    <delete id="deleteUser" parameterType="com.Z.pojo.User">
        delete from mybatis.user where id = #{id} ;
    </delete>
```

**注意点：**

- 增删改需要提交事务

```xml
sqlSession.commit()
```

### 3.6 万能Map

```java
int addUSer2(Map<String,Object>map);

```

```xml
<!--对象中的属性，可以直接取出来  传递map的key-->
<insert id="addUser" parameterType="map">
	insert into mybatis.user(id, pwd) values (#{userid}, #{password});
</insert>
```

```java
@Test
public void addUser2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    
    Map<String, Object> map = new HashMap<String, Object>();
    
    map.put("userid",5);
    map.put("password","233333333");
    
    mapper.addUser2(map);
    
    sqlSession.close();
}
```

Map传递参数，直接在sql中取出key即可！  [parameterType="map"]

对象传递参数，直接在sql中取对象的属性即可！  [parameter="Object"]

只有一个基本类型参数的情况下，可以直接在sql中取

多个参数用Map，或者注解！

## 4、 配置解析



### 4.1 核心配置文件

- mybatis-config.xml
- MyBatis的配置文件包含了会深深影响M有B按提示行为的设置和属性信息

```xml
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
```



### 4.2 环境配置（environments）

MyBatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境**

学会使用配置多套运行环境！

Mybatis默认的事务管理器就是ＪＤＢＣ，连接池：ＰＯＯＬＥＤ



### ４.３属性（properties）

我们可以通过Properties属性来实现引用配置文件

这些属性都是可外部配置且可动态替换的，既可以在典型的java属性文件中配置，亦可通过properties元素的子元素来传递

编写一个配置文件

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
username=root
password=123456
```

在核心配置文件中引入

```xml
<!--引入外部配置文件-->
    <properties resource="db.properties"/>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的



### 4.4 类型别名（typeAliases）

- 类型别名是为java类型设置一个短的名字
- 存在的意义仅在于用来减少类完全限定名的冗余

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.Z.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个报名，MyBatis胡在报名下面搜索需要的Java Bean，比如：

扫描实体类的包，它的默认别名就为这个类的类名，首字母小写！

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.Z.pojo">
    </typeAliases>
```

在实体类比较少的时候，使用第一种方式

如果实体类十分多，建议使用第二种

第一种可以DIY

第二种想要DIY可以在实体上添加注解

```java
@Alias("user")
public class User {
    
}
```



### 4.5 设置

这是MyBatis中极为重要的调整设置

| 设置名             |                             描述                             | 有效值                                                       | 默认值 |
| :----------------- | :----------------------------------------------------------: | :----------------------------------------------------------- | :----- |
| cacheEnabled       |   全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。   | true \| false                                                | true   |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false                                                | false  |
| logImpl            |    指定 MyBatis 所用日志的具体实现，未指定时将自动查找。     | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |



### 4.6 映射器 （mappers）

#### 方式一：

【推荐使用】

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="com.Z.dao.UserMapper.xml"/>
</mappers>
```

#### 方式二：

使用class文件绑定注册

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="com.Z.dao.UserMapper"/>
</mappers>
```

注意点：

- 接口和他的Mapper配置文件必需同名
- 接口和他的Mapper配置文件必须在同一个包下

#### 方式三：

使用扫描包进行注入绑定

```xml
<mappers>
  <package name="com.Z.dao"/>
</mappers>
```



## 5、解决属性名和字段名不一致

### 问题

数据库中的字段

![image-20210406214153030](C:\Users\张以恒\AppData\Roaming\Typora\typora-user-images\image-20210406214153030.png)

```java
public class User {
    
    private int id;
    private String name;
    private String password;
}
```

测试出现问题

![image-20210406214826532](C:\Users\张以恒\AppData\Roaming\Typora\typora-user-images\image-20210406214826532.png)

```xml
select *from mybatis.user where id = #{id}
select id,name,pwd from mybatis.user where id = #{id}
```

解决方法

### 5.1起别名

```xml
<select id="getUserById" resultType="com.Z.pojo.User">
	select id,name,pwd as password from mybatis.user where id = #{id}
</select>
```



### 5.2 resultMap

结果集映射

```
id	name	pwd
id	name	password
```

```xml
<!---->
<resultMap id="UserMap" type="User">
	<!---->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="getUserById" resultMap="UserMap">
	select *from mybatis.user where id = #{id}
</select>
```

- resultMap 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对于简单的语句各奔不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行
- ResultMap 最优秀的地方在于，虽然尼已经对它相当了解了，但是根本不需要显式的用到他们

## 6、日志

### 6.1 日志工厂

如果一个数据库操作，出现了异常，我们需要排错。日志就是最好的助手！

曾经：sout、debug

现在：**日志工厂**

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------ | ------ |
|         |                                                       |                                                              |        |

- SLF4J
- LOG4J 【掌握】
- LOG4J2 
-  JDK_LOGGING 
-  COMMONS_LOGGING 
-  STDOUT_LOGGING  【掌握】
-  NO_LOGGING

在Mybatis中具体使用哪个日志实现，在设置中设定



**STDOUT_LOGGING 标准日志输出**

在mybatis核心配置文件中，配置我们的日志！

```xml
<settings>
	<setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```



### 6.2 LOG4J

(程序加载中的的输出，表明某一部分语句成功/失败)

什么是LOG4J

- Log4j是Apache的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI组件
- 我们可以控制每一条日志的输出格式
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成
- 通过一个配置文件来灵活地进行配置，而不需要修改应用的代码



1. 导入log4j的包

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. log4j.properties

   ```properties
   # priority  :debug<info<warn<error
   #you cannot specify every priority with different file for log4j 
   log4j.rootLogger=debug,stdout,info,debug,warn,error 
    
   #console
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
   log4j.appender.stdout.layout.ConversionPattern= [%d{yyyy-MM-dd HH:mm:ss a}]:%p %l%m%n
   #info log
   log4j.logger.info=info
   log4j.appender.info=org.apache.log4j.DailyRollingFileAppender 
   log4j.appender.info.DatePattern='_'yyyy-MM-dd'.log'
   log4j.appender.info.File=./src/com/hp/log/info.log
   log4j.appender.info.Append=true
   log4j.appender.info.Threshold=INFO
   log4j.appender.info.layout=org.apache.log4j.PatternLayout 
   log4j.appender.info.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
   #debug log
   log4j.logger.debug=debug
   log4j.appender.debug=org.apache.log4j.DailyRollingFileAppender 
   log4j.appender.debug.DatePattern='_'yyyy-MM-dd'.log'
   log4j.appender.debug.File=./src/com/hp/log/debug.log
   log4j.appender.debug.Append=true
   log4j.appender.debug.Threshold=DEBUG
   log4j.appender.debug.layout=org.apache.log4j.PatternLayout 
   log4j.appender.debug.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
   #warn log
   log4j.logger.warn=warn
   log4j.appender.warn=org.apache.log4j.DailyRollingFileAppender 
   log4j.appender.warn.DatePattern='_'yyyy-MM-dd'.log'
   log4j.appender.warn.File=./src/com/hp/log/warn.log
   log4j.appender.warn.Append=true
   log4j.appender.warn.Threshold=WARN
   log4j.appender.warn.layout=org.apache.log4j.PatternLayout 
   log4j.appender.warn.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
   #error
   log4j.logger.error=error
   log4j.appender.error = org.apache.log4j.DailyRollingFileAppender
   log4j.appender.error.DatePattern='_'yyyy-MM-dd'.log'
   log4j.appender.error.File = ./src/com/hp/log/error.log 
   log4j.appender.error.Append = true
   log4j.appender.error.Threshold = ERROR 
   log4j.appender.error.layout = org.apache.log4j.PatternLayout
   log4j.appender.error.layout.ConversionPattern = %d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
   ```

   

3. 配置log4j为日志的实现

   ```xml
   <settings>
   	<setting name="logImpl" value="LOG4J"/>
   </settings>
   ```

4. Log4j的使用

   简单使用

   1. 在要使用Log4j的类中，导入包

      ```java
       import org.apache.log4j.Logger;
      ```

   2. 日志对象，参数为当前类的class

      ```java
      static Logger logger = Logger.getLogger(UserDaoTest.class);
      ```

   3. 日志级别

      ```java
      logger.info("info:进入了testLog4j");
      logger.debug("debug:进入了testLog4j");
      logger.error("reeor:进入了testLog4j");
      ```

      

## 7、分页





------

## 8、使用注解开发

### 8.1

1. 注解在接口上使用

   ```java
   @Select("select *from user")
   List<User> getUsers();
   ```

2. 需要在核心配置文件中绑定接口

   ```xml
   <!--绑定接口-->
   <mappers>
   	<mapper class="com.Z.dao.UserMapper"/>
   </mappers>
   ```

   

### 8.2 CRUD

我们可以在工具类创建的时候实现自动提交事务！

```java
public static SqlSession getSqlSession(){
    return sqlSessionFactory.openSession(true);
}
```



编写接口，增加注解

```java
public interface UserMapper{
    
    @Select("select *from user")
    List<User>getUsers();
    
    @Select("select *from user where id = #{id}")
    User getUserById(@param("id") int id);
    
    @Insert("insert into user(id,name,pwd) values(#{id},#{name},#{password})")
    int addUser(User user)
        
    @Update("update user set name = #{name},pwd = #{password} where id = #{id}")
    int updateUser(User user);
    
    @Delete("delete from user where id = #{id}")
    int deleteUser(@param("id") int id);
}
```

【注意：我们必须要将接口注册绑定到我们的核心配置文件中！】



**关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但建议加上
- 我们在SQL中引用的就是我们这里的@Param中设定的属性名

## 9、Lombk





------

## 10、多对一处理







------

## 11、一对多处理











------

## 12、动态SQL

什么是动态SQL：指根据不同的条件生成不同的SQL语句













------

## 13、Mybatis整合SpringBoot

### 依赖

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>

```

