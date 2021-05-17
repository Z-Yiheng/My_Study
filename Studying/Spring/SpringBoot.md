# SpringBoot

## 1. 配置文件

### 1.1 .yaml

##### 什么是yaml?

yaml是一种配置语言

可以注入到配置类中

##### yaml语法

```yaml
# 普通的key-value
name: zyh

# 对象
student:
	name: zyh
	age: 13
	
#行内写法
student: {name: zyh,age:13}

#数组
pets:
	-cat
	-dog
	-pig

pets: [cat,dog,pig]
```

类似与properties(只能保存键值对)

```properties
name=zyh

student.name = zyh
student.age = 13
```

yaml比properties更加简便

##### .yaml实体类赋值

```java
public clas Person{
    
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
}
```

```yaml
person:
  name: zyh
  age: 13
  happy: false
  birth: 2001/9/6
```

实体类前添加注解@ConfigurationProperties(prefix = "person")

@ConfigurationProperties作用：

将配置文件中配置的每一个属性的值，映射到这个组件中；告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定

参数 prefix = "person" ：将配置文件中的person下面的所有属性一一对应

只有这个组件时一个容器中的组件，才能使用容器提供@ConfigurationProperties功能

### 1.2 SpringBoot的多环境配置

可选择激活哪一个配置文件

##### properties方式

例：application.properties

​		application-test.properties

​		application-dev.properties

```properties
spring.profiles.active=dev
```

##### yaml方式

（推荐）

```yaml
server:
  port: 8080
#激活的配置
spring:
  profiles:
    active: dev
  
---
server:
  port: 8081
spring:
  profiles: dev
  
---
server:
  port:8082
spring:
  profiles: test
```

### 1.3 自动装配

这就是自动装配的原理!

精髓：

1)、SpringBoot启动会加载大量的自动配置类

2)、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中;

3)、我们再来看这个自动配置类中到底配置了哪些组件; (只要我们要用的组件存在在其中，我们就不需要再手动配置了)

4)、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可;

xxxxAutoConfigurartion:自动配置类;给容器中添加组件

xxxxProperties:封装配置文件中相关属性;

****



## 2. SpringBoot Web开发

jar：webapp！ceng

要解决的问题：

- 导入静态资源
- 首页
- jsp，模板引擎Thymeleaf
- 装配拓展SpringMVC
- 增删改查
- 拦截器

### 2.1 模板引擎Thymeleaf

创建project是添加thymeleaf依赖

或者xml下添加依赖

```xml
<dependency>
	<groupId>org.thymeleaf</groupId>
	<artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
	<groupId>org.thymeleaf.extras</groupId>
	<artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
```

将html文件放在templates目录下即可

```java
public static fina1 String DEFAULT_ _PREFIX = "classpath:/templates/";
public static fina1 String DEFAULT_ SUFFIX = " .html";
```

所以的html元素都可以被thymeleaf替换接管：th:元素名

##### Thymeleaf语法

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all" 
          href="../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>
  
    <p th:text="#{home.welcome}">Welcome to our grocery store!</p>
  
  </body>

</html>
```

- Simple expressions:
  - Variable Expressions: `${...}`
  - Selection Variable Expressions: `*{...}`
  - Message Expressions: `#{...}`
  - Link URL Expressions: `@{...}`
  - Fragment Expressions: `~{...}`
- Literals
  - Text literals: `'one text'`, `'Another one!'`,…
  - Number literals: `0`, `34`, `3.0`, `12.3`,…
  - Boolean literals: `true`, `false`
  - Null literal: `null`
  - Literal tokens: `one`, `sometext`, `main`,…
- Text operations:
  - String concatenation: `+`
  - Literal substitutions: `|The name is ${name}|`
- Arithmetic operations:
  - Binary operators: `+`, `-`, `*`, `/`, `%`
  - Minus sign (unary operator): `-`
- Boolean operations:
  - Binary operators: `and`, `or`
  - Boolean negation (unary operator): `!`, `not`
- Comparisons and equality:
  - Comparators: `>`, `<`, `>=`, `<=` (`gt`, `lt`, `ge`, `le`)
  - Equality operators: `==`, `!=` (`eq`, `ne`)
- Conditional operators:
  - If-then: `(if) ? (then)`
  - If-then-else: `(if) ? (then) : (else)`
  - Default: `(value) ?: (defaultvalue)`
- Special tokens:
  - No-Operation: `_`

### 2.2 SpringBoot---WebMvcConfigurer



#### 2.21 简介

WebMvcConfigurer配置类其实是`Spring`内部的一种配置方式，采用`JavaBean`的形式来代替传统的`xml`配置文件形式进行针对框架个性化定制，==可以自定义一些Handler，Interceptor，ViewResolver，MessageConverter==。基于java-based方式的spring mvc配置，需要创建一个**配置**类并实现**`WebMvcConfigurer`** 接口



#### 2.22 WebMvcConfigurer接口

```java
public interface WebMvcConfigurer {
    void configurePathMatch(PathMatchConfigurer var1);
 
    void configureContentNegotiation(ContentNegotiationConfigurer var1);
 
    void configureAsyncSupport(AsyncSupportConfigurer var1);
 
    void configureDefaultServletHandling(DefaultServletHandlerConfigurer var1);
 
    void addFormatters(FormatterRegistry var1);
 
    void addInterceptors(InterceptorRegistry var1);
 
    void addResourceHandlers(ResourceHandlerRegistry var1);
 
    void addCorsMappings(CorsRegistry var1);
 
    void addViewControllers(ViewControllerRegistry var1);
 
    void configureViewResolvers(ViewResolverRegistry var1);
 
    void addArgumentResolvers(List<HandlerMethodArgumentResolver> var1);
 
    void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> var1);
 
    void configureMessageConverters(List<HttpMessageConverter<?>> var1);
 
    void extendMessageConverters(List<HttpMessageConverter<?>> var1);
 
    void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> var1);
 
    void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> var1);
 
    Validator getValidator();
 
    MessageCodesResolver getMessageCodesResolver();
}
```



常用方法：

```java
 /* 拦截器配置 */
void addInterceptors(InterceptorRegistry var1);
/* 视图跳转控制器 */
void addViewControllers(ViewControllerRegistry registry);
/**
     *静态资源处理
**/
void addResourceHandlers(ResourceHandlerRegistry registry);
/* 默认静态资源处理器 */
void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer);
/**
     * 这里配置视图解析器
 **/
void configureViewResolvers(ViewResolverRegistry registry);
/* 配置内容裁决的一些选项*/
void configureContentNegotiation(ContentNegotiationConfigurer configurer);
/** 解决跨域问题 **/
public void addCorsMappings(CorsRegistry registry) ;
```

##### 1) addInterceptors: 拦截器

- addInterceptor：需要一个实现HandlerInterceptor接口的拦截器实例
- addPathPatterns：用于设置拦截器的过滤路径规则；`addPathPatterns("/**")`对所有请求都拦截
- excludePathPatterns：用于设置不需要拦截的过滤规则
- 拦截器主要用途：进行用户登录状态的拦截，日志的拦截等

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    super.addInterceptors(registry);
    registry.addInterceptor(new TestInterceptor()).addPathPatterns("/**").excludePathPatterns("/emp/toLogin","/emp/login","/js/**","/css/**","/images/**");
}
```



##### 2) addViewControllers:页面跳转

以前写SpringMVC的时候，如果需要访问一个页面，必须要写Controller类，然后再写一个方法跳转到页面，感觉好麻烦，其实重写WebMvcConfigurer中的addViewControllers方法即可达到效果了

```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
     registry.addViewController("/toLogin").setViewName("login");
}
```

重写addViewControllers方法，并不会覆盖**WebMvcAutoConfiguration**（Springboot自动配置）中的addViewControllers（在此方法中，Spring Boot将“/”映射至index.html），这也就意味着自己的配置和Spring Boot的自动配置同时有效





****

## IOC原理

IoC 全称为 `Inversion of Control`，翻译为 **“控制反转”**，它还有一个别名为 DI（`Dependency Injection`）,即**依赖注入**

> DI—Dependency Injection，即“依赖注入：组件之间依赖关系由容器在运行期决定，形象的说，即**由容器动态的将某个依赖关系注入到组件之中**。
>
> 　　依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。
>
> 　　通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。

### “控制反转”理解

1. 谁控制谁？

   在传统的开发模式下，我们都是采用直接 new 一个对象的方式来创建对象，也就是说你依赖的对象直接由你自己控制，但是有了 IOC 容器后，则直接由 IoC 容器来控制。所以“谁控制谁”，**当然是 IOC 容器控制对象**

2. 控制什么？

   **控制对象**

3. 为何事反转？

   没有 IOC 的时候我们都是在自己对象中主动去创建被依赖的对象，这是正转。但是有了 IoC 后，**所依赖的对象直接由 IoC 容器创建后注入到被注入的对象中，依赖的对象由原来的主动获取变成被动接受，所以是反转**

4. 哪些方面反转了

   **所依赖对象的获取被反转了**

简单的说：**IOC的理念就是让别人为你服务**

可以理解为： IOC主动把妹子注入给想使用它的小伙子。 （调用的时候使用Autowied ，这个对象就是前文说的bean， 通过注册进入IOC容器，被实例化之后再进入IOC容器的bean缓存池，就可以供程序调用了，这就和bean的生命周期连了起来。）



------



### @Configuration 与 @Component的区别

两者同属于配置文件的注解

- @Configuration中所有带@Bean注解的方法都会被动态代理，调用该方法返回的都是同一个实例
- @Component 注解并没有通过cglib来代理@Bean方法的调用，因此调用带@Bean注解的方法态返回的都是新的实例



------



### @Component

相当于将对象存放到spring容器中

其中@Component有三个子注解：

- @Controller用于标注控制层组件，web层
- @Service用于标注业务层，service层
- @Repository用于标注数据访问组件，dao层

而@Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注



------



### @Resource：

可以根据类型注入，可以根据名称注入

```java
//	@Resource //根据类型进行注入
	@Resource(name =  "testDaoImpl1") //根据类型进行注入
	private TestDao testDao;
	
	public void add() {
		System.out.println("service add....");
		testDao.add();
	}

```

------



### 关于IOC的使用

例子

业务类

```java

import org.springframework.stereotype.Service;
@Service  //声明一个Bean文件
public class FunctionService {
	 public String sayHello(String name) {
		 return "Hello "+name+" !";
	 }
}
```

调用类

```java

@Component
public class UseFunctionService {
	 @Autowired
	 private FunctionService functionService;
	 
	 public void show(String name) {
		  System.out.println(this.functionService.sayHello(name));
	 }
}
```

完成装配

```java

@Configuration    //声明配置类
@ComponentScan("bw.swn.boot.service") //将目录下的所有Bean注册到容器中
public class IocConfig {
}
```

启动类

```java

@SpringBootApplication
public class SpringIocApplication {
 
	public static void main(String[] args) {
		//创建容器 并加载配置文件IocConfig
		AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(IocConfig.class);
        //Ioc自动生成
		UseFunctionService use=ctx.getBean(UseFunctionService.class);
		//调用
		use.show("spring boot");
        //关闭容器
		ctx.close();
	}
}
```



个人理解:

> IOC可分为底层、中层、过渡层、上层。
>
> 底层：
>
> 创建类，存放各种方法，可有接口接入，用@Service(不可声明在接口上)声明为一个Bean文件
>
> 中层：
>
> 创建类，用@Compoent再次声明为Bean文件。类中调用底层，创建底层对象，并用@Autowired自动注入对象内容，可定义方法
>
> 过渡层：
>
> 用@Configuration声明为配置类文件，在用@ComponentScan(“地址”)实现将Bean装入容器
>
> 上层：
>
> 启动层，创建容器 并加载配置文件(如：AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(IocConfig.class);)，创建中间层对象

