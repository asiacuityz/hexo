title: Spring Boot默认配置源码
author: Asia Cui
tags:
  - Spring Boot
categories:
  - Spring Boot默认配置源码
date: 2018-12-30 14:51:00
---
### 源码位置
`Spring Boot` 默认自动配置绑定在 `ServerProperties` 类上，源码中所在位置如下：

```
|-- spring-boot-project
    |-- spring-boot-autoconfigure
        |-- src/main/java
            |-- org.springframework.boot.autoconfigure
                |-- web
                    |-- ServerProperties  
```