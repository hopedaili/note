# 知识点

### dependencyManagement 和 dependencies 的区别？

> Maven 使用 dependencyManagement 元素提供一种管理依赖版本号的方式。
>
> 通常会在一个组织或者项目的最顶层的父POM中看到 dependencyManagement 元素。
>
> 使用 pom.xml 中的 dependencyManagement 元素能让所有子项目中引用一个依赖儿不用显式的列出版本号。
>
> Maven 会沿着父子层次向上走，直到找到一个拥有 dependencyManagement 元素的项目，然后使用这个元素中指定的版本号。
>

父项目 XML 代码：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.2</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子项目添加 mysql-connector 时可以不指定版本号：

```xml
<dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
    </dependencies>
```

> 好处：如果有多个子项目引用同一个依赖，可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一个版本时，只需要在顶层父容器更新，不需要一个一个子项目更改；另外，如果某个子项目需要另外的版本，只需要声明 version 即可。
>
> 注：dependencyManagement 只是声明依赖，并不引入，子项目需要显式声明需要用到的依赖。
>

### 微服务模块步骤

1. 建 module
2. 改 pom
3. 写 yml
4. 主启动
5. 业务类

### restTemplate

- restTemplate 是什么？

> RestTemplate 提供了多种便捷访问远程 Http 服务的方法。是一种简单便捷的访问 restful 服务模板类，是 Spring 提供的用于访问 Rest 服务的客户端模板工具类。

- restTemplate 怎么用？

> ( url, requestMap, RespinseBean.class ) 三个参数被别代表
>
> REST 请求地址，请求参数，HTTP 响应转换被转换成的对象类型。

### @RequestParam 注解

> **作用：**@RequestParam 注解是作用在形参列表上的，接受的参数来自 HTTP 请求体或请求 url 的 QueryString中。
>
> **描述：**
>
> RequestParam 可以接受简单类型的属性，也可以接受对象类型。
>
> @RequestParam 有三个配置参数：
>
> - requird 表示是否必须，默认为 true。
>
> - defaultValue 可设置请求参数的默认值。
>
> - Value 为接受 url 的参数名（相当于 key 值）。
>
>  **使用时机：**@RequestParam用来处理 `Content-Type` 为 `application/x-www-form-urlencoded` 编码的内容，`Content-Type`默认为该属性；用来处理 `multipart/form-data` (表单上传的)。
>
> **注意：**@RequestParam注解默认（需要自定义 HandlerMethodArgumentResolver 类）无法读取application/json格式数据，这时候就需要使用到@RequestBody。

### @RequestBody 注解

> **作用：**@RequestBody 是作用在形参列表上的，用于将前台发送过来固定格式的数据（XML 或者 json）封装成 **JavaBean** 对象，封装时使用的一个对象时系统默认配置的 HttpMessageConverter 进行解析，然后封装到形参上

> **使用时机：**根据 request header Content-Type 的值来判断
>
> `form-data`、`x-www-form-urlencoded`：不可以用@RequestBody；可以用@RequestParam。
>
> `application/json`：json字符串部分可以用@RequestBody；url中的?后面参数可以用@RequestParam。
>
> GET请求中，因为没有HttpEntity，所以@RequestBody并不适用。

> **注意：**直接通过浏览器输入url时，@RequestBody获取不到json对象，在发送请求时，
>
> 1、需要在 Postman 的 headers 中将 “Content-Type” 设置为 “application/json”。
>
> 2、在 body 中选在 raw 添加json数据。key需要加引号。举例：`{ "id" : 3 }`

### @RequestBody 与 @RequestParam 总结

**@RequestBody**

```java
(@RequestBody Map map)
(@RequestBody Object object)
//application/json时候可用
//form-data、x-www-form-urlencoded时候不可用
//GET请求中不可以使用@RequestBody
```

**@RequestParam**

```java
(@RequestParam Map map)
//application/json时候，json字符串部分不可用，
//url中的?后面添加参数即可用，
//form-data、x-www-form-urlencoded时候可用，但是要将Headers里的Content-Type删掉
```

```java
(@RequestParam String waterEleId,@RequestParam String enterpriseName)
//application/json时候，json字符串部分不可用，url中的?后面添加参数即可用
//form-data、x-www-form-urlencoded时候可用，且参数可以没有顺序（即前端传过来的参数或者url中的参数顺序不必和后台接口中的参数顺序一致，只要字段名相同就可以），但是要将Headers里的Content-Type删掉
```

```java
(@RequestParam Object object)
//不管application/json、form-data、x-www-form-urlencoded都不可用
//GET请求中不可以使用
```

**既不是@RequestBody也不是@RequestParam，没有指定参数哪种接收方式**

```java
(Map map)
(Object object)
//application/json时候：json字符串部分不可用，url中的?后面添加参数不可用。因为没有指定，它也不知道到底是用json字符串部分还是?后面添加参数部分，所以干脆都不可以用
//form-data、x-www-form-urlencoded时都不可用
 
(HttpServletRequest request)
//application/json不可用
//form-data、x-www-form-urlencoded时可用
```



### @ResponseBody 注解

> **作用：**ResponseBody 是作用在方法上的，表示该方法的返回结果直接写入 HTTP response body 中。一般在异步获取数据时使用（也就是AJAX）。
>
> 当方法上面没有写ResponseBody,底层会将方法的返回值封装为ModelAndView对象。
>
> 在使用 @RequestMapping 后，返回值通常解析为跳转路径，但加上 @responseBody 后返回结果不会被解析为跳转路径，而是直接写入 HTTP response body 中。比如异步获取 json 数据，加上 @ResponseBody 后，会直接返回 json 数据。

# 父工程步骤

1. New Project

2. 聚合总父工程名字

3. Maven 选版本

4. 工程名字

5. 字符编码

   Settings-->Editor-->File Encodings

   全部修改为 utf-8

6. 注解生效激活

   Settings-->Build-->Complier-->Annotation Processors

   Default-->Enable annotation processing

7. java版本编译选择

   Settings-->Build-->Complier-->Java Complier

8. File Type 过滤

   Settings-->Editor-->File types-->ActionScript





# Eureka 服务注册与发现

## Eureka 基础知识

### 什么是服务治理？

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务治理。

在传统的 rpc 远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务与服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

###  什么是服务注册与发现？

Eureka 采用了 CS 的设计架构，Eureka Server 作为服务这侧功能的服务器，他是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server 并位置心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息，比如，服务地址、通讯地址等以别名的方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到时机的服务通讯地址，然后在实现本地 RPC 远程调用框架核心设计思想：在于这侧中心，因为使用注册中心管理每个服务与服务器之间的一个依赖关系（服务治理概念）。在任何 rpc 远程框架中，都会有一个注册中心（存放服务地址相关信息（接口地址））。

### Eureka 包含两个组件：Eureka Server 和 Eureka Client

#### Eureka Server 提供服务注册服务

各个微服务节点通过配置启动后，会在 Eureka Server 中进行注册，这样 Eureka Server 中的服务注册中将会有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

#### Eureka Client 通过注册中心进行访问

是一个 Java 客户端，用于简化 Eureka Server 的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向 Eureka Server 发送心跳（默认周期为30s）。如果Eureka Server 在多个心跳周期内没有接收到某个节点的心跳，Eureka Server 将会从服务注册表中吧这个服务节点移除（默认90s）。

## 单机 Eureka 构建步骤

#### IDEA 生成 Eureka Server 端服务注册中心

1. 建 Module

   cloud-eureka-server7001

2. 改 POM

   1.x 和 2.x 对比

   ```xml
   <!--2018版本 eureka server-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-eureka</artifactId>
   </dependency>
   
   <!--2020版本 eureka server-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
   </dependency>
   ```

   

3. 写 YML

4. 主启动

5. 测试

#### Eureka Client 端 cloud-prover-payment8001 将注册进 Eureka Server 成为服务提供者 provider

#### Eureka Client 端 cloud-consumer-order80 将注册进 Eureka Server 成为消费者 consumer

## 集群 Eureka 构建步骤

#### Eureka 集群原理说明

**原理：互相注册，互相守望**

服务注册：将服务信息注册进注册中心

服务发现：从这侧中心上获取服务信息

实质：存 key 服务名，取 value 调用地址

步骤：

1. 先启动 eureka 注册中心
2. 启动服务提供者 payment 支付中心
3. 支付服务启动后会把自身信息（比如服务地址以别名的方式）注册进 eureka
4. 消费者 order 服务在需要调用接口时，使用服务别名去注册中心获取实际的 RPC 远程调用地址
5. 消费者获得调用地址后，底层实际是利用 HttpClient 技术实现远程调用
6. 消费者获得服务地址后会缓存本地 JVM 内存中，默认每间隔 30s 更新一次服务调用地址

#### EurekaServer 集群环境构建步骤

1. 参考 cloud-eureka-server7001

2. 新建 cloud-eureka-server7002

3. 改 pom

4. 修改映射配置

   修改映射配置文件添加进 hosts 文件

   127.0.0.1 eureka7001.com

   127.0.0.1 eureka7002.com

5. 写 YML（以前是单机）

6. 主启动



#### 将支付服务8001微服务发布到上面两台 Eureka 集群配置中

修改 YML 文件，defaultZone 修改为集群版

#### 将订单服务80微服务发不到上面两台 Eureka 集群配置中

同上

#### 测试01

#### 支付服务提供者8001集群环境构建

1. 参考 cloud-provider-payment8001
2. 新建 cloud-provider-payment8002
3. 改 pom
4. 写 YML
5. 主启动
6. 业务类
7. 修改 8001/8002 的 Controller

#### 负载均衡

1. 订单服务访问地址不能写死，换成服务名称
2. 使用@LoadBalanced注解赋予RestTemplate负载均衡的能力
3. ApplicationContextBean：Ribbon的负载均衡功能，默认轮询机制

#### 测试02



## actuator 微服务信息完善

#### 主机名称：服务名称修改

yml 文件增加 instance-id

#### 访问信息有 IP 信息显示

yml 文件增加 prefer-ip-address



## 服务发现 Discovery







## Eureka 自我保护































































































































































































































































































































































