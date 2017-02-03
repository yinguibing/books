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

编写controller

