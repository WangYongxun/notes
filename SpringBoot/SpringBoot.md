SpringBoot
===
[toc]

---
## 1. 自动配置原理

### 1.1 pom.xml简介：

+ 用于配置maven项目相关信息：项目名、版本号、项目描述、项目依赖等。
+ GroupID和ArtifactID统称为坐标，可以唯一标识一个项目，通过坐标可以找到一个项目。GroupID格式一般为“域.公司名”（eg：cn.Alibaba），ArtifactID一般为项目名（eg：Zhifubao）
+ 按住ctrl 点击项目名/版本号，可以查看到链接的工程

### 1.2 pom标签分析:

#### 1.2.1 parent：父工程
```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>
```
+ 点击本工程的父工程，即spring-boot-starter-parent，可以看到其已经配置好资源过滤（resources）和相关插件（plugins）
+ 点击spring-boot-starter-parent的父工程，即spring-boot-dependencies，可以看到包含了各种jar包各种版本，所以在使用时可以直接引入，不需要指定版本


#### 1.2.2 properties：属性
```xml
<properties>
    <java.version>1.8</java.version>
</properties>
```
+ 一般用于定义版本信息，定义好的属性可以在其他地方引用，方便一处改处处变
+ 如上文定义了java版本，其余地方可以使用${java.version}直接表示1.8

#### 1.2.3 dependencies：依赖（启动器）
```xml
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ...
    
</dependencies>
```
+ SpringBoot中的dependencies内一般为各种启动器，即starter
+ 启动器一般都有starter字样，spring官方的启动器开头为spring-boot-starter-XXX..
+ SpringBoot中的各种依赖说白了就是为了针对各种使用场景使用不同启动器，如编写web应用就是spring-boot-starter-web，用于测试就是spring-boot-starter-test

#### 1.2.4 build：构建相关
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
+ build标签描述了如何来编译及打包项目，一般都是各种打包插件

## 2. JDBC

jdbc : [数据库名] :// [ip] : [端口] / [路径] ? [参数]=[值]

相当于 http：// xxx：8080

