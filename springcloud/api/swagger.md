# Swagger介绍

swagger是开源的API框架，号称“是世界上最流行的”API框架。它拥有自己的工具生态系统，包括编辑、构建、生成文档和使用restful接口工具等。

## swagger工具集

|  | Description |
| :--- | :--- |
| [Swagger Core](https://github.com/swagger-api/swagger-core) | Java库，用来构建、使用API |
| [Swagger Codegen](https://github.com/swagger-api/swagger-codegen) | A code generation framework for building Client SDKs, servers, and documentation from Swagger Definitions |
| [Swagger UI](https://github.com/swagger-api/swagger-ui) | 基于HTML5的网页，用于浏览swagger生成的API接口，并通过网页与接口交互。 |
| [Swagger Editor](https://github.com/swagger-api/swagger-editor) | Browser based editor for authoring Swagger definitions using YAML |

Other tools created by the Swagger Team include:

| Tool | Description |
| :--- | :--- |
| [Swagger JS](https://github.com/swagger-api/swagger-js) | Javascript library for connecting to Swagger-defined APIs from browser and node.js applications |
| [Swagger Node](https://github.com/swagger-api/swagger-node) | Design-driven server implementation for node.js |
| [Swagger Parser](https://github.com/swagger-api/swagger-parser) | Standalone library for parsing Swagger definitions from Java |
| [Validator-Badge](https://hub.docker.com/r/swaggerapi/swagger-validator/) | Standalone web service which validates swagger definitions dynamically |

# Swagger-UI与Springboot整合

### maven依赖

```java
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.6.1</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.6.1</version>
        </dependency>
```

### springboot中swagger配置

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    /**
     * 文档摘要
     * @return
     */
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example"))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * API文档详细信息
     * @return
     */
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("用户模块API文档")
                .description("用户基础微服务的API接口说明")
                .termsOfServiceUrl("http://blog.csdn.com/")
                .contact(new Contact("long哥", "http://www.cnblogs.com/yinguibing/", "longge@qq.com"))
                .version("1.0")
                .build();
    }
}
```

### 编写controller

```
@Api(value="用户接口",tags="用户CRUD接口",produces="application/json")
@RestController
@RequestMapping("/people")
public class PersonController {

    Logger logger = Logger.getLogger(this.getClass().getName()) ;

    @ApiOperation(value="展示所有的用户信息",notes="接口的详细描述信息",response=Person.class,responseContainer="List")
   @RequestMapping(method = RequestMethod.GET)
   public List<Person> showAll() {

      return null ;
   }
    @ApiOperation(value="根据用户ID查找指定用的信息",notes="用户id不可以为空",response=Person.class)
    @ApiImplicitParams(
            @ApiImplicitParam(dataType="long",name="用户id")
            )
   @RequestMapping(value = "/{person}", method = RequestMethod.GET)
   public Person show(@PathVariable Long id) {

      return null ;
   }

    @ApiOperation(value="更新用户信息",notes="根据用户的id去更新用户的信息，更新成功返回true，更新失败返回false",response=Boolean.class)
    @ApiImplicitParams(
            @ApiImplicitParam(dataType="Person",name="用户信息")
            )
    @PutMapping(
            value="/update"
            )
    public Boolean update(Person person)
    {
        logger.info("接受到更新用户信息请求");
        logger.info("信息内容为:"+person.toString());
        return true;
    }
}
```

### 页面展示效果

#### 接口列表页面

![](/assets/import.png)

接口的详细信息列表

![](/assets/api-info-list.png)

### Swagger注解说明

说明：

* @Api：用在类上，说明该类的作用
* @ApiOperation：用在方法上，说明方法的作用
* @ApiImplicitParams：用在方法上包含一组参数说明
* @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
  * paramType：参数放在哪个地方
    * header--
      &gt;
      请求参数的获取：@RequestHeader
    * query--
      &gt;
      请求参数的获取：@RequestParam
    * path（用于restful接口）--
      &gt;
      请求参数的获取：@PathVariable
    * body（不常用）
    * form（不常用）
  * name：参数名
  * dataType：参数类型
  * required：参数是否必须传
  * value：参数的意思
  * defaultValue：参数的默认值
* @ApiResponses：用于表示一组响应
* @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
  * code：数字，例如400
  * message：信息，例如"请求参数没填好"
  * response：抛出异常的类
* @ApiModel：描述一个Model的信息（这种一般用在post创建的时候，使用@RequestBody这样的场景，请求参数无法使用@ApiImplicitParam注解进行描述的时候）
  * @ApiModelProperty：描述一个model的属性

以上这些就是最常用的几个注解了。

需要注意的是：

* ApiImplicitParam这个注解不只是注解，还会影响运行期的程序，例子如下：

![](http://images2015.cnblogs.com/blog/866881/201604/866881-20160405160238047-1725111637.png)

如果ApiImplicitParam中的phone的paramType是query的话，是无法注入到rest路径中的，而且如果是path的话，是不需要配置ApiImplicitParam的，即使配置了，其中的value="手机号"也不会在swagger-ui展示出来。

**测试：**

启动服务，浏览器输入"http://localhost:8080/swagger-ui.html"

![](http://images2015.cnblogs.com/blog/866881/201604/866881-20160402194015223-1123677403.png)

最上边一个红框：@Api

GET红框：method=RequestMethod.GET

右边红框：@ApiOperation

parameter红框：@ApiImplicitParams系列注解

response messages红框：@ApiResponses系列注解

输入参数后，点击"try it out!"，查看响应内容：

![](http://images2015.cnblogs.com/blog/866881/201604/866881-20160402194351629-832756751.png)



