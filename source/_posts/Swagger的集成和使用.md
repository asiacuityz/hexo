title: Swagger的集成和使用
author: Asia Cui
tags:
  - Spring Boot
categories:
  - Swagger
date: 2019-01-24 11:39:00
---
##  手写文档存在的问题

*   文档需要更新的时候，需要再次发送一份给前端，也就是文档更新交流不及时。
*   接口返回结果不明确
*   不能直接在线测试接口，通常需要使用工具，比如：`Postman`
*   接口文档太多，不好管理

##  使用 Swagger 解决问题

Swagger 也就是为了解决这个问题，当然也不能说 Swagger 就一定是完美的，当然也有缺点，最明显的就是代码植入性比较强。

###  Maven

增加 Swagger2 所需依赖，`pom.xml` 配置如下：

```xml
<!-- Swagger2 Begin -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.8.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.8.0</version>
</dependency>
<!-- Swagger2 End -->

```

###  配置 Swagger2

注意：`RequestHandlerSelectors.basePackage("com.funtl.itoken.service.admin.controller")` 为 Controller 包路径，不然生成的文档扫描不到接口

创建一个名为 `Swagger2Config` 的 Java 配置类，代码如下：

```java
package com.funtl.itoken.service.admin.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class Swagger2Config {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.funtl.itoken.service.admin.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("iToken API 文档")
                .description("iToken API 网关接口，http://www.baidu.com")
                .termsOfServiceUrl("http://www.baidu.com")
                .version("1.0.0")
                .build();
    }
}

```
### 启用 Swagger2

Application 中加上注解 `@EnableSwagger2` 表示开启 Swagger

```java
package com.funtl.itoken.service.admin;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import springfox.documentation.swagger2.annotations.EnableSwagger2;
import tk.mybatis.spring.annotation.MapperScan;

@SpringBootApplication(scanBasePackages = "com.funtl.itoken")
@EnableEurekaClient
@EnableSwagger2
@MapperScan(basePackages = {"com.funtl.itoken.common.mapper", "com.funtl.itoken.service.admin.mapper"})
public class ServiceAdminApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServiceAdminApplication.class, args);
    }
}

```

### 使用 Swagger2

在 Controller 中增加 Swagger2 相关注解，代码如下：

```java
/**
 * 分页查询
 *
 * @param pageNum
 * @param pageSize
 * @param tbSysUserJson
 * @return
 */
@ApiOperation(value = "管理员分页查询")
@ApiImplicitParams({
        @ApiImplicitParam(name = "pageNum", value = "页码", required = true, dataType = "int", paramType = "path"),
        @ApiImplicitParam(name = "pageSize", value = "笔数", required = true, dataType = "int", paramType = "path"),
        @ApiImplicitParam(name = "tbSysUserJson", value = "管理员对象 JSON 字符串", required = false, dataTypeClass = String.class, paramType = "json")
})
@RequestMapping(value = "page/{pageNum}/{pageSize}", method = RequestMethod.GET)
public BaseResult page(
        @PathVariable(required = true) int pageNum,
        @PathVariable(required = true) int pageSize,
        @RequestParam(required = false) String tbSysUserJson
) throws Exception {

    TbSysUser bSysUser = null;
    if (bSysUserJson != null) {
        bSysUser = MapperUtils.json2pojo(bSysUserJson, TbSysUser.class);
    }
    PageInfo pageInf = adminService.page(pageNum, pageSize, bSysUser);

    // 分页后的结果集
    List<TbSysUser> list = pageInf.getList();

    // 封装 Cursor 对象
    BaseResult.Cursor cursor = new BaseResult.Cursor();
    cursor.setTotal(new Long(pageInf.getTotal()).intValue());
    cursor.setOffset(pageInf.getPageNum());
    cursor.setLimit(pageInf.getPageSize());

    return BaseResult.ok(list, cursor);
}

```
###  Swagger 注解说明

Swagger 通过注解表明该接口会生成文档，包括接口名、请求方法、参数、返回信息的等等。

*   `@Api`：修饰整个类，描述 Controller 的作用
*   `@ApiOperation`：描述一个类的一个方法，或者说一个接口
*   `@ApiParam`：单个参数描述
*   `@ApiModel`：用对象来接收参数
*   `@ApiProperty`：用对象接收参数时，描述对象的一个字段
*   `@ApiResponse`：HTTP 响应其中 1 个描述
*   `@ApiResponses`：HTTP 响应整体描述
*   `@ApiIgnore`：使用该注解忽略这个 API
*   `@ApiError`：发生错误返回的信息
*   `@ApiImplicitParam`：一个请求参数
*   `@ApiImplicitParams`：多个请求参数

###  访问 Swagger2

访问地址：http://ip:port/{contentpath}/swagger-ui.html

注意：

- 如果项目配置了contentpath需要加上contentpath
- 如果有权限控制拦截此请求，也会访问失败


## 注解的使用

- @ApiIgnore 

  例如想隐藏接口接收参数`Moedl`在swagger文档中不显示，需要在`Model`参数前添加`@ApiIgnore `属性，接口如下：

  ```java
  @ApiOperation(value = "测试接口")
  @GetMapping("test")
  @ResponseBody
  public Object test(String name, User user, @ApiIgnore Model model) {
     return null;
  }
  ```

- @ApiOperation

  接口http请求的描述

  ```java
  @ApiOperation(value = "获取图书信息", notes = "获取图书信息", response = Book.class, responseContainer = "Item", produces = "application/json")
  ```

- @ApiResponses

  用于方法，描述操作的可能响应。

- @ApiImplicitParams

  用于方法，参数，字段说明，表示对参数的添加元数据（说明或是否必填等）

  ```java
  @ApiImplicitParams
  @ApiImplicitParams({
          @ApiImplicitParam(name = "name", value = "姓名", required = true, dataType = "String", paramType = "query"),
          @ApiImplicitParam(name = "age", value = "年龄", required = true, dataType = "String", paramType = "query")
  })
  ```
作用在方法上，表示单独的请求参数 
参数： 
1. name ：参数名。 

2. value ： 参数的具体意义，作用。 

3. required ： 参数是否必填。 

4. dataType ：参数的数据类型。 

5. paramType ：查询参数类型，这里有几种形式：

   | 类型   | 作用                                     |
   | ------ | ---------------------------------------- |
   | path   | 以地址的形式提交数据(@PathVariable)      |
   | query  | 直接跟参数完成自动映射赋值(url使用&连接) |
   | body   | 以流的形式提交 仅支持POST                |
   | header | 参数在request headers 里边提交           |
   | form   | 以form表单的形式提交 仅支持POST          |

   **踩坑**：当我发POST请求的时候，当时接受的整个参数，不论我用body还是query，后台都会报Body Missing错误。这个参数和SpringMvc中的@RequestBody冲突，索性我就去掉了paramType，对接口测试并没有影响。


## GET请求 

如果参数比较多的况下，Controller接收GET请求参数时，后台代码为了方便会把所有的参数封装到一个实体类中，此时显示实体类的所有参数到swagger中，需要使用`@ModelAttribute`注解。接口注解使用如下：
```java
  @ApiOperation(value = "测试接口")
  @GetMapping("test")
  @ResponseBody
  public Object test( @ModelAttribute User user) {
     return null;
  }
```
实体类注解如下：

```java
@ApiModel(value = "用户实体类")
 public class User  {
     
    @ApiModelProperty(value = "姓名",required = true)
    private String name;
    
    @ApiModelProperty(value = "年龄")
    private String age;
  }
  
```