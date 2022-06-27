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
    <version>2.9.2</version>
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
* [springboot 2.6.0结合swagger3.0.0不兼容问题](https://blog.csdn.net/dchua123/article/details/121918921)



* 此时就可以测试 Swagger http://127.0.0.1:8080/v2/api-docs

* 可以得到 `json`  但是没有 UI, 不方便调试，可以添加 ui 依赖
```xml
        <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
        <!--
         提供 swagger UI
         -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```


**NOTE**
* 在本文在写的时候 `Swagger 3.0.0` 版本还存在兼容性问题,此处演示使用 `2.x.x ` 的版本

##### Step 3: Take APIs out the root to `/api`


```java
package com.example.swagger;
import org.springframework.web.bind.annotation.*;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ConcurrentHashMap;

@RestController
//===================================
@RequestMapping("/api")
//===================================
public class AddressBookResource {

    // hash map
    ConcurrentHashMap<String, Contact> contacts = new ConcurrentHashMap<>();

    // /id  获取特定 id
    @GetMapping("/{id}")
    public Contact getContact(@PathVariable String id ) {
        return contacts.get(id);
    }

    // 访问 root, 获得所有 Contact
    @GetMapping("/")
    public List<Contact> getAllContacts(){
        return new ArrayList<Contact>(contacts.values());
    }

    @PostMapping("/")
    public Contact addContact(@RequestBody Contact contact) {
        contacts.put(contact.getId(), contact);
        return contact;
    }

}
```

##### Step 4: 访问 `http://127.0.0.1:8080/swagger-ui.html`

[![jExVER.png](https://s1.ax1x.com/2022/06/27/jExVER.png)](https://imgtu.com/i/jExVER)


###  Controller
* click the controller and you can see `GET`  `POST` ect.
* you can test your APIs throug the webpage

###  Model 
* you can see your model object  on the page



## Config `Swagger`
##### Control how API documentated and the model object how `to be showed
   * The way to config Swagger is to create an instence of an Obeject called `Docket` (案卷), it will contains all the custom configuration you intend for Swagger to pick out when it generate documentation
   * the following is an example
```java
package com.example.swagger;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.context.annotation.Bean;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

//@SpringBootApplication
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})


/**
* 启动使用 Swagger2
*/
@EnableSwagger2

public class SwaggerApplication {
    public static void main(String[] args) {
        SpringApplication.run(SwaggerApplication.class, args);
    }

    @Bean
// ==============================================================
    public Docket SwaggerConfiguration() {
        return new Docket (DocumentationType.SWAGGER_2)
                .select()
                .paths(PathSelectors.ant("/api/*")) // api d对外暴露的路径
                .apis(RequestHandlerSelectors.basePackage("com.example.swagger")) // Swagger 查看的包
                .build();

//                .select()
//                    |
//                    |
//                    V
//                .build();


    }
}

// ==============================================================

```
* Then can only see the `address-book-resource`  and part of the `model`, `error-handler` is gone
[![jVHpkT.png](https://s1.ax1x.com/2022/06/27/jVHpkT.png)](https://imgtu.com/i/jVHpkT)

#####  Adding Application metadata(Custom infomation in the Swagger ui page)

* Add `apiInfo()` after the `bulid()`
```java
    @Bean
    public Docket SwaggerConfiguration() {
        return new Docket (DocumentationType.SWAGGER_2)
                .select()
                .paths(PathSelectors.ant("/api/*")) // api d对外暴露的路径
                .apis(RequestHandlerSelectors.basePackage("com.example.swagger")) // Swagger 查看的包
                .build()
                .apiInfo(apiDetails());



//                .select()
//                    |
//                    |
//                    V
//                .build();


    }

    private ApiInfo apiDetails() {
        return  new ApiInfo(
                "API 测试 页面",
                "简单的 API 页面",
                "1.0",
                "免费试用",
                new springfox.documentation.service.Contact("clayliu","http://testing.com", "a@b"),
                "API License",
                "http://testing.com",
                Collections.emptyList());
    }
```

[![jVbsRe.png](https://s1.ax1x.com/2022/06/27/jVbsRe.png)](https://imgtu.com/i/jVbsRe)

### Adding more details to APIs
```java

package com.example.swagger;

import io.swagger.annotations.ApiModelProperty;

public class Contact {
    @ApiModelProperty(notes = "唯一的 id")
    private String id;

    @ApiModelProperty(notes = "姓名")
    private String name;

    @ApiModelProperty(notes = "电话")
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




