前言：因为项目需求将前端的问卷调查表存入数据库中，通过查询了解到 JSON 这个重要格式，所以对后端 JSON进行学习。

## SpringBoot-JSON



### 1. 简述

在前端与后端数据的传输多使用==JSON格式的数据==进行传输交互

在SpringBoot框架中，接口返回  JSON  格式的数据比较简单：在 Controller 类中使用@RestController注解即可返回 JSON 格式的数据

@RestController

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
 
    /**
     * The value may indicate a suggestion for a logical component name,
     * to be turned into a Spring bean in case of an autodetected component.
     * @return the suggested component name, if any (or empty String otherwise)
     * @since 4.0.1
     */
    @AliasFor(annotation = Controller.class)
    String value() default "";
 
}
```

@RestController 注解包含 @Controller 和 @ResponseBody 两个核心注解

- @Controller ：当一个类引入这个注解，表明当前的这个类是一个 controller 控制器类
- @ResponseBody ：表示方法返回值的注解应该绑定到web响应的消息体中。



### 2. SpringBoot 默认对 JSON 的处理

在项目中，常用的数据结构大致有：类对象、List对象、Map对象

> 案例来自于博客https://blog.csdn.net/qq_27706119/article/details/104852054



#### 2.1. 实体类

```java
**
 * 系统用户实体类
 */
public class SysUser{
 
    private String id;
    private String userName;
    private String password;
 
   /**此处省略构造方法、set和get方法**/
}
```



#### 2.2. Controller 控制器

实体类需要控制器吧信息返回到前端

```java
@RestController
@RequestMapping("/jsonFormat1")
public class SystemUser1Controller {
 
    @GetMapping("/user")
    public SystemUser getUser() {
        return new SystemUser("1", "小明", "xiaoming123");
    }
 
    @GetMapping("/list")
    public List<SystemUser> getUserList() {
        List<SystemUser> list= new ArrayList<>();
        SystemUser xiaoming= new SystemUser("1", "小明", "xiaoming123");
        SystemUser xiaoli = new SystemUser("2", "小丽", "xiaoli123");
        list.add(xiaoming);
        list.add(xiaoli);
        return list;
    }
 
    @GetMapping("/map")
    public Map<String, Object> getMap() {
        Map<String, Object> map = new HashMap<>();
        SystemUser user = new SystemUser("1", "xiaoming", "xiaomign123");
        map.put("用户信息", user);
        map.put("在干啥呢", "working");
        map.put("备注说明", "hello world");
        map.put("一串数字",123456789);
        return map;
    }
}
```



#### 2.3. 返回结果

##### 【1】返回实体类

```txt
{"id":"1","userName":"小明","password":"xiaoming123"}
```

##### 【2】返回List集合

```txt
[{"id":"1","userName":"小明","password":"xiaoming123"},{"id":"2","userName":"小丽","password":"xiaoli123"}]
```

##### [3]返回Map集合

```txt
{"用户信息":{"id":"1","userName":"xiaoming","password":"xiaomign123"},"备注说明":"hello world","在干啥呢":"working","一串数字":123456789}
```



#### 2.4. jackson 中对 null 的处理

在项目中，在对数据封装为 JSON 格式时，难免回遇到返回值为 null ，希望 null 在转换为 JSON 格式时变成“”的空字符串

下面对 jackson 做配置类：

```java
/**
 * jackson数据格式处理类：null值处理
 */
@Configuration
public class JacksonConfig {
    @Bean
    @Primary
    @ConditionalOnMissingBean(ObjectMapper.class)
    public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder) {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
        objectMapper.getSerializerProvider().setNullValueSerializer(new JsonSerializer<Object>() {
            @Override
            public void serialize(Object o, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException {
                jsonGenerator.writeString("");
            }
        });
        return objectMapper;
    }
}
```

**测试**：

```java
@GetMapping("/map")
public Map<String, Object> getMap() {
    Map<String, Object> map = new HashMap<>();
    SystemUser user = new SystemUser("1", "xiaoming", "xiaomign123");
    map.put("用户信息", user);
    map.put("在干啥呢", null);
    map.put("备注说明", "hello world");
    map.put("一串数字", null);
    return map;
}
```

**结果：**

```txt
{"用户信息":{"id":"1","userName":"xiaoming","password":"xiaomign123"},"备注说明":"hello world","在干啥呢":"","一串数字":""}
```

### 3. 其他小问题

#### 时间戳

在B站学习后端 JSON 时发先一个有趣的事情，就是用JSON返回时间

在返回时间时，所返回的是一个==时间戳==

下面针对 **时间戳 **的两种解决方法：

- 方法一（推荐）

  在配置中修改 apllication.property/appllication.yml

  ```yml
  #时间戳同意转换
  spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
  spring.jackson.time-zone=GMT+8
  ```

- 方法二

  @JsonFormat(timezone = "GMT+8",pattern = "yyyyMMddHHmmss")

  private Date createTime





