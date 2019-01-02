title: Maven依赖机制
author: Asia Cui
tags:
  - Maven
categories:
  - Maven依赖机制
date: 2019-01-15 17:31:00
---
## pom.xml解析

### 依赖范围
```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```
scope:该标签指定依赖的作用域。
### 依赖传递

### 依赖冲突
- 依赖路径最短优先
- pom中谁写在前面谁优先


```
graph LR
A-->B
B-->C
C-->common-2.0.jar
```

```
graph LR
A-->B
B-->common-3.0.jar
```
介绍：项目依赖了两个版本的common.jar，在打包时会采用依赖路径最短的common.3.0.jar。

### 项目聚合
把多个模块聚合到一块。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.asia</groupId>
    <artifactId>demo-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <name>demo-parent</name>
    <!--为pom-->
    <packaging>pom</packaging>

<!--demo-parent聚合demo1、demo2、demo3、demo4模块-->
    <modules>
        <module>../demo1</module>
        <module>../demo2</module>
        <module>../demo3</module>
        <module>../demo4</module>
    </modules>
</project>

```
### 依赖继承
把各个模块公用的依赖抽取成父类，供子模块继承使用。
- 父pom

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.asia</groupId>
    <artifactId>demo-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <name>demo-parent</name>
    <packaging>pom</packaging>
    
    <!--版本管理-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <!--mysql驱动依赖-->
        <mysql-connector-java.version>5.1.38</mysql-connector-java.version>
    </properties>

    <!--dependencyManagement:管理父类的依赖，把版本号抽取出来放到properties标签中统一管理-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql-connector-java.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>

```

- 子pom

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--子类的坐标-->
    <groupId>com.asia</groupId>
    <artifactId>demo-children</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <name>demo-children</name>
    <packaging>pom</packaging>

    <!--继承父类的pom依赖-->
    <parent>
        <groupId>com.asia</groupId>
        <artifactId>demo-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
</project>
```


## 其他依赖标签介绍
依赖传递排除
```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>${shiro.version}</version>
        <!--排除依赖传递列表 -->
        <exclusions>
            <exclusion>
                        
            </exclusion>
        </exclusions>
</dependency>
```
构建行为列表，bulid标签配置maven构建项目时的需要执行的各种行为，一般都是maven的插件

```xml
<build>
        <pluginManagement>
            <plugins>
                <plugin> <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                      <source>${java.version}</source>
                        <target>${java.version}</target>
                    </configuration>
                </plugin>
        </pluginManagement>
    </build>
```

# maven的使用


`demo-parent`为父工程，聚合如下四个子工程：

```xml
  //demo-parent下的子工程，同事子工程之间存在相互依赖，demo-admin依赖demo-core
  <modules>
        <module>../demo-admin</module>
        <module>../demo-core</module>
        <module>../demo-rest</module>
        <module>../demo-generator</module>
    </modules>
```

## package
`demo-admin` 依赖 `demo-core`，如果在 `demo-core` 未打包的情况下，单独打包子工程 `demo-admin`，此时会报错，说找不到 `demo-core`，因为打包的时候通过pom去maven仓库寻找 `demo-core`，因 `demo-core` 不存在所以报错。<br>
如果给 `demo-core` 执行package命令后，去给 `demo-core` 打包还会报同样的错，因为package命令仅仅是在 `demo-core` 的target目录下生成jar包，并未放入到maven仓库中，所以还是找不到。<br>
所以最终解决方案是执行完package命令后，在执行insetall命令，此时会把jar安装到maven仓库中。

## maven仓库分类

在maven中，仓库可以分为：<br>
本地仓库、远程仓库。 
- 远程仓库可以分为：中央仓库、私服仓库。 
中央仓库是maven官方指定的仓库，可以理解为“寻找的最后一站”。 
- 私服仓库可以是自己建的，也可以是其它主体建的（比如aliyun的maven仓库，jboss的maven仓库等）。 
私服可以分为：全局应用的私服仓库、应用到项目自身的私服仓库。

maven寻找得顺序大致可以理解为： 
```
graph LR
本地仓库-->私服
私服-->远程仓库
```
1. 在本地仓库中寻找，如果没有则进入下一步。 
2. 在全局应用的私服仓库中寻找，如果没有则进入下一步。
3. 在项目自身的私服仓库中寻找，如果没有则进入下一步。 
4. 在中央仓库中寻找，如果没有则终止寻找。

## maven常用命令
- package命令完成了项目编译、单元测试、打包功能，但没有把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库
- install命令完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库，==但没有布署到远程maven私服仓库==
- deploy命令完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到==本地maven仓库和远程maven私服仓库==　　

> 后一个命令会覆盖前一个命令的操作

```bash
//分别是打包、安装、发布跳过测试命令
mvn package -Dmaven.test.skip=true
mvn install -Dmaven.test.skip=true
mvn deploy -Dmaven.test.skip=true
```