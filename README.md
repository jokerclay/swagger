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

## Add `AddressBookResource.java`
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

## Add `Contact.java`
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


