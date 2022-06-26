# Swagger
## What is Swagger ?
- develop APIs
- Interact with APIs
- Document APIs
## Documenting APIs
 - why documenting APIs ? 
   - when you using `Sping boot`  or Other technologies write APIs, Some one need to know how to use it. 
## API Technologies
- `SOAP` ----> has  WSDL for documentation
- `REST` ----> no formal documentation in spec
- `Swagger` 
  - you add `metadata` in your API application, 
  - `swagger` will generate API documentation for you in a nice form
  - if you **upadted** your `APIs` or the `metadata`, `swagger` will **automaticlly update** the API documentation.  
## Adding `Swagger` to `Spring Boot`
1. get the swagger dependency
2. enable swagger in your code
3. configuring swagger
4. adding details as annotations(注释) to APIs


# Deom
```console
.
└── com
    └── example
        └── swagger
            ├── AddressBookResource.java
            ├── Contact.java
            └── SwaggerApplication.java

```

## 主入口程序 `SwaggerApplication.java`
```java
package com.example.swagger;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// @SpringBootApplication
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})   // 使 应用 暂时不连接数据库, 只做演示目的
public class SwaggerApplication {

    public static void main(String[] args) {
        SpringApplication.run(SwaggerApplication.class, args);
    }

}

```

## `AddressBookResource.java`
```java
package com.example.swagger;
public class Contact {
    private String id;
    private String name;
    private String phone;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

}

```

## `Contact.java`
```java
package com.example.swagger;
public class Contact {
    private String id;
    private String name;
    private String phone;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }
}
```

## Run it & test it use postman

## Using `Swagger`
##### Step 1:Adding Swagger dependency 
* `pom.xml`
* https://mvnrepository.com/artifact/io.springfox/springfox-swagger2
```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
```
##### Step 2:Enable swagger2
* go to the main application, which is  `SwaggerApplication.java` in this case and add : `@EnableSwagger2`
```java
package com.example.swagger;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

//@SpringBootApplication
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})       // 使 应用 暂时不连接数据库, 只做演示目的
@EnableSwagger2
public class SwaggerApplication {

    public static void main(String[] args) {
        SpringApplication.run(SwaggerApplication.class, args);
    }

}


```
**NOTE:**
`spring boot 2` 与 `Swagger 3.0` 版本有不兼容问题
解决办法: 在 ` application.properties` 中加入下一行
```
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```
**See more detials :**

* [github issues #3642](https://github.com/springfox/springfox/issues/3462)
*  [Spring Boot整合Swagger报错："this.condition" is null](https://zhuanlan.zhihu.com/p/521178511)
* [] (https://blog.csdn.net/dchua123/article/details/121918921)


