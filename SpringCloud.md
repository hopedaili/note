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

### IDEA 生成 Eureka Server 端服务注册中心

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

### Eureka Client 端 cloud-prover-payment8001 将注册进 Eureka Server 成为服务提供者 provider

### Eureka Client 端 cloud-consumer-order80 将注册进 Eureka Server 成为消费者 consumer

## 集群 Eureka 构建步骤

### Eureka 集群原理说明

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

### EurekaServer 集群环境构建步骤

1. 参考 cloud-eureka-server7001

2. 新建 cloud-eureka-server7002

3. 改 pom

4. 修改映射配置

   修改映射配置文件添加进 hosts 文件

   127.0.0.1 eureka7001.com

   127.0.0.1 eureka7002.com

5. 写 YML（以前是单机）

6. 主启动



### 将支付服务8001微服务发布到上面两台 Eureka 集群配置中

修改 YML 文件，defaultZone 修改为集群版

### 将订单服务80微服务发不到上面两台 Eureka 集群配置中

同上

### 测试01

### 支付服务提供者8001集群环境构建

1. 参考 cloud-provider-payment8001
2. 新建 cloud-provider-payment8002
3. 改 pom
4. 写 YML
5. 主启动
6. 业务类
7. 修改 8001/8002 的 Controller

### 负载均衡

1. 订单服务访问地址不能写死，换成服务名称
2. 使用@LoadBalanced注解赋予RestTemplate负载均衡的能力
3. ApplicationContextBean：Ribbon的负载均衡功能，默认轮询机制

### 测试02



## actuator 微服务信息完善

### 主机名称：服务名称修改

yml 文件增加 instance-id

### 访问信息有 IP 信息显示

yml 文件增加 prefer-ip-address



## 服务发现 Discovery

### 对于注册进 Eureka 里面的微服务，可以通过服务发现来获得该服务的信息



### 修改 cloud-provider-payment8001 的 Controller



### 8001 主启动类

@EnableDiscoveryClient

### 自测



## Eureka 自我保护

### 故障现象

概述：

保护模式主要用于一组客户端和 Eureka Server 之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server 将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务。

### 导致原因

某时刻一个微服务不可用了，Eureka 不会立即清理，依旧会对该微服务的信息进行保存。

属于 CAP 里面的 AP 分支。

#### **为什么会产生 Eureka 自我保护机制？**

为了防止 Eureka Client 可以正常运行，但是与 Eureka Server 网络不通情况下，Eureka Server 不会立即将 Eureka Client 服务剔除。

#### **什么是自我保护模式？**

默认情况下，如果 Eureka Server 在一定时间内没有接收到某个微服务实例的心跳，Eureka Server 将会注销该实例（默认90s）。但是当网络分区故障发生（延时、卡顿、拥挤）时，微服务与 Eureka Server 之间无法正常通信，以上行为可能变得非常危险了--因为微服务本身其实是健康的，此时本不该注销这个微服务。Eureka通过“自我保护模式来解决这个问题”--当Eureka Server 节点在短时间内丢失过多客户端时（可能放生了网络分区故障），那么这个节点就会进入自我保护模式。

### 怎么禁止自我保护

#### 注册中心 Eureka Server 端 7001

1. 出场默认，自我保护机制是开启的，eureka.server.enable-self-preservation=true
2. 使用 eureka.server.enable-self-preservation=false 可以禁用自我保护
3. 关闭效果
4. 在 Eureka Server 端7001 处设置关闭自我保护机制

#### 生产者客户端 Eureka Client 端 8001

## Eureka 停更说明

# zookeeper

## 注册中心 Zookeeper

zookeeper 是一个分布式协调工具，可以实现注册中心功能。

关闭 Linux 服务器放火前后启动 zookeeper 服务器。

zookeeper 服务器取代 Eureka 服务器，zk 作为服务注册中心。

## 服务提供者

步骤：

1. 新建 cloud-provider-payment8004

2. POM

3. YML

4. 主启动类

5. Controller

6. 启动 8004 注册进 zookeeper

   启动后 jar 包版本冲突问题

   修改 pom 排除自带的 zookeeper3.5.3

7. 验证测试

8. 验证测试2

9. 思考

   服务节点是临时节点还是持久节点：临时节点

## 服务消费者

1. 新建 cloud-consumerzk-roder80
2. POM
3. YML
4. 主启动
5. 业务类
6. 验证测试
7. 访问测试地址

# Consul服务注册与发现

## Consul 简介

### 是什么？

Consul 是一套开源的分布式服务发现和配置管理系统，由 HashiCorp 公司用 Go 语言开发。

提供了微服务系统中的服务治理、配置中信、控制总线等功能。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建安全方位的服务网络，总之 Consul 提供了一种完整的服务网络解决方案。

它具有很多优点。包括：基于 raft 协议，比较简洁；支持健康检查，同时支持HTTP 和 DNS 协议，支持跨数据中心的 WAN 集群，提供图形界面，跨平台，支持 Linux，Mac，Windows。

### 能干嘛？

服务发现：提供 HTTP 和 DNS 两种发现方式

健康监测：支持多种方式，HTTP、TCP、Docker、Shell 脚本定制化

KV 存储：Key、Value 的存储方式

多数据中心：Consul 支持多数据中心

可视化 Web 界面：可视化

### 去哪下？

consul.io

### 怎么玩？

中文：https://www.springcloud.cc/spring-cloud-consul.html

## 安装并运行Consul

使用开发者模式启动：`consul agent -dev`

通过以下地址访问Consul的首页：http://localhost:8500

## 服务提供者

1. 新建Module支付服务 provider8006

   cloud-providerconsul-payment8006

2. POM
3. YML
4. 主启动
5. 业务类
6. 验证测试

## 服务消费者

1. 新建 Module 消费者服务 order80 

   cloud-consumerconsul-order80

2. POM

3. YML

4. 主启动

5. 配置 Bean

6. Controller

7. 验证测试

8. 访问测试地址

## 三个注册中心异同

CAP

C：Consistency（强一致性）

A：Availability（可用性）

P：Partition tolerance（分区容错性）

CAP 理论关注粒度是数据，而不是整体系统设计。

CAP 理论的核心：一个分布式系统不可能同时很好的满足一致性、可用性和分区容错性这三个需求，因此，根据 CAP原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则、和满足 AP 原则三大类。

CA-单点集群，满足一致性，可用性的系统，通常可扩展性上不太强大。

CP-满足一致性，分区容忍行的系统，通常性能不是特别高。

AP-满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。

AP（Eureka）

CP（ZK/Consul）

# Ribbon 负载均衡服务调用

## 概述

Spring Cloud Ribbon 是基于 Nextflix Ribbon 实现的一套客户端负载均衡的工具。

Ribbon 是Netflix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon 客户端组件提供一系列完善的配置项入连接超时，重试等。简单的说，就是在配置文件中列出 Load Balancer （简称LB） 后面所有的机器，Ribbon 会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用 Ribbon 实现自定义的负载均衡算法。

一句话：负载均衡 + RestTemplate 调用

官网：https://github.com/Nextflix/ribbon/wiki/Getting-Started

Ribbon 目前也进入维护模式。

### 负载均衡（LB）

1. 集中式 LB 

   即在服务的消费方和提供方之间独立的 LB 设施（可以是硬件，如 F5；也可以是软件，如 Nginx），由该设施负载把访问请求通过某种策略转发至服务的提供方。

2. 进程内 LB 

   将 LB 逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选在处一个合适的服务器。Ribbon 就属于进程内 LB，他只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。

#### LB负载均衡（Load Balance）是什么？

简答的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的 HA（高可用）。

常见的负载均衡软件有 Nginx，LVS，硬件 F5 等。

#### Ribbon 本地负载均衡客户端 VS Nginx 服务端负载均衡区别？

Nginx 是服务端负载均衡，客户端所有请求都会交给 nginx，然后由 nginx 实现转发请求。即负载均衡是由服务端实现的。

Ribbon 本地负载均衡，在调用微服务接口的时候，会在注册中心上获取注册信息服务列表之后缓存到 JVM 本地，从而在本地实现 RPC 远程服务调用技术。

## Ribbon 负载均衡演示

### 架构说明

总结：Ribbon 其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和 Eureka 结合只是其中的一个实例。

**Ribbon 在工作时分成两步**

第一步先选择 EurekaServer，它优先选择在同一个区域内负载较少的Server。

第二步再根据用户指定的策略，再从 server 取到的服务注册列表中选择一个地址。

其中 Ribbon 提供了多种策略：比如轮询、随机和根据响应时间加权。

### POM

spring-cloud-start-netflix-rureka-client 自带了 spring-cloud-starter-ribbon。

自己引入写法：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

### RestTemplate 使用

#### getForObject 方法，getForEntity 方法

getForObject 方法返回对象为响应体中数据转化成的对象，基本上可以理解为 Json。

getForEntity 方法返回对象为 ResponseEntity 对象，包含了响应中的一些重要信息，比如响应头、响应状态码，响应体等。

#### postForObject 方法，postForEntity 方法

#### GET 请求方法

#### POST 请求方法

## Ribbon 核心组件 IRule

**IRule：根据特定算法中从服务列表中选取一个要访问的服务。**

**自带 7 种：**

1. com.netflix.loadbalancer.RoundRobinRule 轮询
2. com.netflix.loadbalancer.RandomRule 随机
3. com.netflix.loadbalancer.RetryRule 先按照 RoundRobinRule  的策略获取服务，如果获取失败则在指定时间内会进行重试，获取可用的服务。
4. WeightedResponseTimeRule 对 RoundRobinRule 的扩展，响应速度越快的实例选择权重越大，越容易被选择。
5. RestAvailableRule 会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务。
6. AvilabilityFilteringRule 先过滤掉故障实例，再选择并发较小的实例。
7. ZoneAvoidanceRule 默认规则，符合判断 server 所在区域的性能和 server 的可用性选择服务器

**如何替换？**

1. 修改 cloud-consumer-order80

2. 注意配置细节

   官方文档明确给出了警告：

   这个自定义配置类不能放在@ComponentScan 所扫描的当前包下及子包下，否则我们自定义的这个配置类就会被所有的 Ribbon 客户端所共享，达不到特殊化定制的目的了。

3. 新建packge：com.atguigu.myrule

4. 上面包下新建 MySelfRule 规则类

5. 主启动添加@RibbonClient

6. 测试

## Ribbon 负载均衡算法

### 原理

负载均衡算法：rest 接口第几次请求数 % 服务器集群总数量=实际调用服务器位置下标，每次服务重启后 rest 接口计数从 1 开始。

```java
List<ServiceInstance> instances = discoveryClient.getInstances("cloud-payment-service");
```

例如：

```java
List[0] instances = 127.0.0.1:8002
List[1] instances = 127.0.0.1:8001
```

请求数为 1 时：1 % 2 = 1 对应下标为 1，获取服务器地址为 127.0.0.1:8001

请求数为 2 时：2 % 2 = 0 对应下标为 0，获取服务器地址为 127.0.0.1:8002

···

### 源码



### 手写

7001/7002 集群启动

8001/8002 微服务改造-controller

80 订单为服务改造

1. ApplicationContextBean 去掉注解@LoadBalanced
2. LoadBalancer 接口
3. MyLB
4. OrderController
5. 测试 http://localhost.consumer.payment/lb











































































































































































































































































































































