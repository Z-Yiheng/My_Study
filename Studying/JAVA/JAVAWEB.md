# JavaWeb



## 基础知识



### 1. 技术讲解



#### ASP：

- 微软：国内最早流行的ASP
- 在HTML中嵌入VB脚本，ASP+COM
- 在ASP开发中，基本一个页面都有几千行的业务代码，页面极其混乱
- 维护成本高
- C#



#### PHP

- PHP开发速度很快，功能很强大，而且跨平台，代码简单
- 无法承载大访问量的情况（局限性）



#### JSP/Servlet

B/S：浏览和服务器

C/S：客户端和服务器

- sun公司主推的B/S架构
- 基于java语言的（所有的大公司，或者一些开源的组件，都是用java写的）
- 可以承载三高问题带来的影响 





### 2. Web服务器

服务器是一种被动的操作，用来处理用户的一些请求和用户一些相应信息



**IIS**

微软的；ASP...，Windows自带的

**Tomcat**

Tomcat实际上运行JSP页面和Servlet





### 3. Http

**Http（超文本传输协议）是一个简单的请求-响应协议，它通常运行再TCP之上**

- 文本：html、字符串，...
- 超文本：图片、音乐、视频、定位、...
- 端口：80

Https：安全

- 端口：443



#### Http请求

- 客户端---发送请求（Request）---服务器

```java
Request URL: https://www.baidu.com/
Request Method: GET
Status Code: 200 OK    //状态
Remote Address: 163.177.151.109:443
Referrer Policy: strict-origin-when-cross-origin
```

##### 1、请求行

- 请求行中的请求方式：GET

- 请求方式：**GET**，**POST**，HEAD，DELETE，PUT，... 

- - get：请求能够携带的参数比较少，大小由限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - post：请求携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效

##### 2、消息头

```java
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding:支持那种编码格式  GBK  UTF-8  GB2312   ISO8859-1
Accept-Language:告诉浏览器，它的语言环境
Cache-Control:缓存控制
Connection:告诉浏览器，请求完成时断开还是保持
HOST:主机
```



#### Http响应

- 服务器---响应---客户端

1、响应体

```java
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding:支持那种编码格式  GBK  UTF-8  GB2312   ISO8859-1
Accept-Language:告诉浏览器，它的语言环境
Cache-Control:缓存控制
Connection:告诉浏览器，请求完成时断开还是保持
HOST:主机
Refrush:告诉客户端，多久刷新一次
Location:让网页重新定位
```

2、响应的状态码

200：请求响应成功

3**：请求重定向

- 重定向：重新到给的新位置

4**：找不到资源     404

- 资源不存在

5**：服务器代码错误      500     502(网关错误)

### 4. Maven

Maven == 项目架构管理工具

目前用来方便导入jar包

Maven的核心思想：**约定大于配置**

- 有约束，不要去违反

Maven会规定号你该如何去编写java代码，必须要按照这个规范





## 1、Servlet

#### 1.1 Servlet简介

- Servlet就是sun公司开发动态Web的一门技术
- sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
  - 编写一个类，实现Servlet接口
  - 把开发好的java类部署到Web服务器中

**把实现了Servlet接口的java程序叫做——Servlet**

#### 1.2 