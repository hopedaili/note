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



  







































































































































































































































































































































































