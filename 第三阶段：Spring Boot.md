# 第1课：Spring Boot 项目全景

------

## 一、 Spring Boot 到底是在解决什么问题？

如果不用 Spring Boot，我想用 Java 做一个网站后端，需要干什么？

假设前端发送：

```
GET /users/1
```

后端要做的事情大概是：

```
接收 HTTP 请求
↓
判断请求路径是 /users/1
↓
找到对应 Java 代码
↓
执行查询用户的逻辑
↓
访问数据库
↓
把 User 对象转换成 JSON
↓
构造 HTTP Response
↓
返回给前端
```

真正的 Web 项目还需要处理很多“基础设施”，例如：

```
启动 Web 服务器
解析 HTTP 请求
管理大量 Java 对象
连接数据库
处理 JSON
管理配置
处理异常
记录日志
管理依赖
……
```

如果每做一个 Java Web 项目，都从零搭这些东西，会非常痛苦，于是出现了 **Spring**

------



## 二、Spring 和 Spring Boot 是什么关系？

```
Spring = 一整套帮助你开发 Java 应用的框架体系

Spring Boot = 让你更方便地使用 Spring 的工具和约定
```

假设 Spring 是：一大箱建筑材料和工具，里面有水泥、钢筋、电线、工具

- 但问题是：房子怎么搭？哪些工具要配置？哪些东西应该组合在一起？
- 过去使用 Spring，开发者需要做大量配置

Spring Boot 做的事情可以粗略理解成：**帮你预先搭好很多东西，让你尽快开始真正写业务**，

所以：**Spring Boot ≠ 取代 Spring**，Spring Boot 建立在 Spring 之上，让 Spring 更容易使用

这些很多其实都是 **Spring 的核心概念**

- Bean
- IoC
- 依赖注入
- @Service
- @Component

这些则明显带有 **Spring Boot** 的特色

- @SpringBootApplication
- application.yml
- starter
- 自动配置

**我们表面上是在学 Spring Boot，实际上也会逐渐接触 Spring 的核心能力**

------



## 三、一个 Spring Boot 项目长什么样？

真实项目可能存在很多文件：

```
demo
├── .idea
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.example.demo
│   │   │       ├── controller
│   │   │       ├── service
│   │   │       ├── mapper
│   │   │       ├── entity
│   │   │       ├── config
│   │   │       └── DemoApplication.java
│   │   │
│   │   └── resources
│   │       └── application.yml
│   │
│   └── test
│
├── pom.xml
└── ...
```

------

### 1. `src/main/java`：Java 代码的大本营

```
src
└── main
    └── java
```

这里放的是：**项目主要的 Java 源代码**，**后端程序主体**

------



### 2. 为什么里面还有 `com/example/demo`？

```
src/main/java
└── com
    └── example
        └── demo
```

IDEA 有时候会把它显示成：

```
com.example.demo
```

它就是 **包 package**

```
package com.example.demo;
```

------



### 3.包 package的作用：

#### **1. 提供命名空间，解决命名冲突**

- Java 通过全限定类名（包名 + 类名）唯一标识类
- 不同包下可存在同名类，JVM 据此进行区分，避免类定义被覆盖

#### **2. 实施访问权限控制**

- 未显式指定 `public`、`protected` 或 `private` 的类成员**默认为包可见**（Package-Private）
- 该级别的访问权限仅限同一包内的类，用于隐藏内部实现细节，增强封装性

#### **3. 组织物理结构，支持模块化**

- 包名必须与文件目录结构严格对应
- 该机制便于按功能模块组织源代码，同时支持将相关包打包为 `.jar` 文件进行独立分发，使模块边界清晰可辨

------



### 4. `resources`：放“不是 Java 代码，但程序需要的东西”

```
src/main/resources
```

`resources` ：资源，这里通常不会放主要 Java 代码，常见内容包括：

```
application.yml
application.properties

SQL / Mapper XML
静态资源
配置文件
模板
```

------



### 5. `test`：测试代码

```
src/test
```

它主要用于：**测试项目代码**

Spring Boot 项目经常能看到：

```
src
├── main   → 正式程序
│   └── ...
│
└── test   → 测试程序
    └── ...
```

------



### 6. `pom.xml`：整个项目的“装备清单”

项目根目录通常有：

```
pom.xml
```

`pom.xml` 决定这个 Maven 项目拥有哪些依赖和构建配置

例如你的项目想使用：

```
Spring Web
MySQL 驱动
MyBatis
Redis
```

一个普通 Java 项目本身并不会天然知道这些代码在哪里，Maven 会根据 `pom.xml` 去管理这些依赖

可以把 `pom.xml` 理解成：项目需要哪些外部能力的说明书

------



### 7.`controller`：接待 HTTP 请求

 `Controller` 是 **Web 请求入口**，负责接待 HTTP 请求

```
GET /users/1001
```

访问进入项目后，通常首先来到：`UserController`

```
HTTP Request
↓
Controller
```

------



### 8.`service`：真正处理业务

`Service` 负责：**业务逻辑**

`Controller` 收到：查询用户 1001 请求之后：

```
Controller  → 负责接请求
↓
Service     → 负责办事情
```

------



### 9.`mapper`：和数据库打交道

`Mapper` 负责：**访问数据库**，是Java 代码和数据库之间的桥梁之一

非常粗略地理解：

```
Service
↓
Mapper
↓
MySQL
```

Mapper 会做类似的工作：

```
SELECT * FROM user WHERE id = 1001;
```

------

### 10.`entity`：Java 世界里的“数据对象”

数据库里可能有一张表`user`：

| id   | name  | age  |
| ---- | ----- | ---- |
| 1001 | Zhang | 23   |

Java 程序拿到以后，总要用某种 Java 对象表示它，于是可能有：

```java
class User {
    Long id;
    String name;
    Integer age;
}
```

这个 `User` 很可能就是一个 Entity

`entity` 是存放与数据有关的 Java 对象：

```
数据库中的一条用户数据
↓
Java 里的 User 对象
```

------

### 11.`config`：项目的一些配置代码

`config` 通常用于放：**Java 形式的配置**

这里通常不是具体的业务代码，而是在配置项目某些功能

------

### 12. 把五个目录连起来

假设浏览器发送：

```
GET /users/1001
```

整个项目里可能发生：

```
浏览器 / 前端
      │
      │ GET /users/1001
      ↓
Controller
      │
      │ “我要查询1001号用户”
      ↓
Service
      │
      │ “执行查询用户的业务”
      ↓
Mapper
      │
      │ 帮我从数据库拿数据
      ↓
MySQL数据库
```

然后数据再回来：

```
MySQL
  ↓
Mapper      得到 User Entity
  ↓
Service     处理 User
  ↓
Controller  返回数据
  ↓
HTTP Response
  ↓
浏览器
```

------

### 13.把五个目录分成三类

请求处理，形成主调用链：

```
controller
service
mapper
```

数据，保存/表达数据：

```
entity
```

配置，配置项目行为：

```
config
```

------

### 14. 认识启动类

一个 Spring Boot 项目里通常还能找到类似：

```
DemoApplication.java
```

或者：

```
MyProjectApplication.java
```

内容大概长这样：

```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

**这通常是整个 Spring Boot 项目的启动入口**

------



## 四、目录名不是 Spring 强制规定的

```
controller
service
mapper
entity
config
```

不要理解成：Spring Boot 法律规定，项目必须有这五个文件夹

这些更多是：**工程中的常见分层和命名习惯**

------



## 五、现在脑中应该有的项目地图

第一层：

```
Spring Boot 项目
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
│
└── pom.xml
```

第二层：

```
java
├── controller   接收 Web 请求
├── service      处理业务
├── mapper       访问数据库
├── entity       表示数据
├── config       项目配置
└── Application  项目启动入口
```

第三层：

```
HTTP Request
↓
Controller
↓
Service
↓
Mapper
↓
Database
```

------



# 第2课：Maven 与项目配置

------

## 一、Java 项目为什么需要 Maven？

### 1. Java 自己并不是什么都自带

现在要开发 Spring Boot 项目，想写：

```java
@RestController
```

`RestController` 是 Java 自带的吗？不是，它来自 Spring

再比如使用：`ObjectMapper` 进行 JSON 处理，连接：`MySQL`，使用：`MyBatis`，

这些同样不是 JDK 自带的，于是问题就变成：**这些别人写好的代码，我的项目从哪里获得？**

------

### 2. 如果没有 Maven，会发生什么？

如果没有 Maven 这种依赖管理工具，需要使用某个第三方库，可能要：

```
上网
↓
找到项目官网
↓
下载 jar 文件
↓
复制到自己项目
↓
配置 classpath
↓
让 Java 知道这个 jar 在哪里
```

并且一个库往往还依赖其他库：

假设你想使用 A 库，但 A 库自己又使用了 B 库、C 库，C 又依赖 D 库

```
项目
 ↓
 A
 ├── B
 └── C
     └── D
```

如果全部手动管理：

- 下载 A 之后程序可能报错：缺少 B
- 再去下载 B，然后：缺少 C
- 再下载 C，最后：D 的版本不对

Maven 最重要的作用之一，就是处理这个问题

------



## 二、Maven 到底是什么？

可以把 Maven 理解成：**Java 项目的依赖管理 + 构建工具**

### 1. 什么叫“依赖管理”？

假设 Spring Boot 项目需要：Spring Web、MySQL Driver、MyBatis

不用自己去一个个下载 jar，而是在 Maven 的配置文件中声明项目需要这些东西

Maven 会：找到它们 → 下载它们下载它们依赖的其他东西 → 让项目能够使用这些代码

所以 Maven 很像：**项目的依赖管理员**

------

### 2. 什么叫“构建”？

平时写的是：`.java`，但最终运行的不是单纯这一堆 `.java` 文件

项目还需要经历：编译 → 测试 → 打包 → ……

Maven 也可以统一完成这些工作

所以 Maven 又像：**项目的流水线管理员**

因此我们可以重新理解：

```
Maven
├── 管依赖
└── 管构建
```

------



## 三、`pom.xml` 是什么？

### 1. POM 是 Maven 项目的核心配置文件

```
demo
├── src
└── pom.xml
```

`pom.xml` 是 Maven 项目的核心配置文件，可以粗略理解为：**这个项目的说明书**

里面会告诉 Maven：

```
这个项目叫什么
版本是什么
用了哪些依赖
需要什么插件
怎么构建
```

所以以后拿到一个陌生 Spring Boot 项目，查看 `pom.xml`，往往能快速判断：这个项目用了哪些主要技术

------

### 2. 一个简化的 `pom.xml`

```xml
<project>

    <groupId>com.example</groupId>     |
    <artifactId>demo</artifactId>      | → 项目信息
    <version>0.0.1-SNAPSHOT</version>  |

    <dependencies>                     |
                                       |
        <dependency>                   |
            ...                        |
        </dependency>                  | → 依赖
                                       |
        <dependency>                   |
            ...                        |
        </dependency>                  |
                                       |
    </dependencies>                    |

</project>
```

现在只看结构：

```
project
│
├── 项目信息
│
└── dependencies     → 依赖清单
    ├── dependency   → 具体依赖
    └── dependency   → 具体依赖
```

------

### 3. 一个 dependency 通常长这样

```xml
<dependency>
    <groupId>xxx</groupId>          ≈ 厂商
    <artifactId>yyy</artifactId>    ≈ 产品名
    <version>1.0.0</version>        ≈ 产品版本
</dependency>
```

- `groupId`：这个东西属于谁 / 哪个组织
- `artifactId`：具体是哪一个库
- `version`：用哪个版本

但Spring Boot 项目的 `pom.xml` 经常不写很多 version

因为 Spring Boot 已经通过它自己的依赖管理机制帮你管理了一批常用依赖版本，**处理了常用依赖之间的版本搭配**

------



## 四、为什么写一个 `<dependency>`，项目就有能力了？

假设在：`pom.xml` 写：

```xml
<dependency>
    ...
</dependency>
```

发生的事情并不是：这一小段 XML 自己变出了 Spring 代码，而是：

```
pom.xml 声明：“我需要 Spring Web”
     ↓
Maven 读取 pom.xml
     ↓
Maven 找到对应依赖
     ↓
下载相关 jar
     ↓
把这些 jar 加入项目的依赖环境
     ↓
Java 代码于是可以使用里面的类
```

所以 <dependency> 是告诉 Maven：请把这个外部功能引入我的项目

------



## 五、Maven下载依赖

### Maven依赖下载在哪里？

一般会下载到电脑上的：Maven 本地仓库，常见位置类似：

```
C:\Users\用户名\.m2\repository
```

（如果 Maven 配置改过，也可能在其他位置）

第一次使用某个依赖时：

```
Maven
  ↓
网上下载
  ↓
放进本地仓库
```

以后另一个项目也需要同一个版本，直接使用本地已经下载好的，通常不用重复下载

------

### Maven 去哪里找这些依赖？

完整机制其实还有远程仓库，例如 Maven Central

整体流程可以粗略理解成：

```
项目 pom.xml
   ↓
需要某个依赖
   ↓
先看本地 Maven 仓库有没有
   ↓
   有 → 直接使用
   
  没有 → 去远程仓库下载
             ↓
        保存到本地仓库
             ↓
          项目使用
```

在 IDEA 里，可能刚打开一个 Maven 项目，右下角疯狂下载东西，这通常就是 Maven 正在：

```
根据 pom.xml
下载项目所需依赖
```

------



## 六、什么是 Starter

### 1. 没有 Starter 会有多麻烦

假设开发一个 Web 项目五个库：

- A
- B
- C
- D
- E

你可能必须在 `pom.xml` 里写：

- dependency A
- dependency B
- dependency C
- dependency D
- dependency E

还得自己考虑：它们版本兼不兼容？是不是少了某一个？

Spring Boot 觉得这太麻烦，于是提供了：`Starter`

------

### 2. Starter 可以理解成“依赖套餐”

```
spring-boot-starter-web
```

它大概表达：**我要开发 Web 项目，请把常用 Web 能力的一整套依赖给我**

因此你不用自己研究：到底应该加入十几个什么库？

而是加入一个 Starter：

```
spring-boot-starter-web
```

------

### 3. Starter 本身不等于“一个超级 jar”

不要把 `starter` 想成：一个什么功能都有的巨大 jar

`Starter` 更合适的理解是：帮你组织好一组常用依赖

------



## 七、Maven 的生命周期

```
compile
test
package
install
```

------

### 1. `compile`：编译

`.java` Java 源代码，编译以后得到 `.class`

```
Java 源码 .java
     ↓
编译 compile
     ↓
JVM 可以使用的字节码 .class
```

------

### 2. `test`：测试

它会执行项目中的测试

------

### 3. `package`：打包

Spring Boot 项目开发完成以后，通常需要打包，不可能把几百个 .java 文件丢给服务器说：你自己想办法运行

Maven 可以把项目打成类似 `demo.jar`，也就是把项目构建成**可交付的软件包**

```
target
└── demo-0.0.1-SNAPSHOT.jar
```

这个 `.jar` 就是：mvn package 的构建操作产生的

------

### 4. `install`：安装到本地 Maven 仓库

假设自己开发了一个 Java 模块 `common-utils`

另外一个项目想把它当依赖使用，可以通过 `install`**把构建出来的东西安装到本地 Maven 仓库**，

这样其他 Maven 项目就有机会像使用普通依赖一样使用它

------



## 八、`mvn` 是什么？

```
mvn package
```

可以拆成：

```
mvn + package
```

- `mvn`：调 Maven
- `package`：告诉 Maven 执行 package 阶段

------

```
mvn test
```

- Maven，帮我执行测试

```
mvn package
```

- Maven，帮我把项目打包

```
mvn install
```

- Maven，把项目构建后安装到本地仓库

------



# 第3课：Spring Boot 是如何启动的

## 一、Spring Boot 项目的启动类

一个典型的 Spring Boot 启动类：

```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

一个 Spring Boot 项目的启动入口，通常就这么短，但背后发生了很多事情

```
项目拥有 Spring Boot / Web 等依赖
↓
运行 DemoApplication
↓
main()
↓
SpringApplication.run()
↓
Spring Boot 读取已有环境
↓
扫描
↓
自动配置
↓
Web 服务器启动
```

------

### `SpringApplication.run()`

```java
SpringApplication.run(DemoApplication.class, args);
```

这里传进去：

```java
DemoApplication.class
```

告诉 Spring Boot：这是我的**主启动类**，从这里开始组织整个应用

------

### `@SpringBootApplication` 

它大致帮助完成几件非常重要的事：

```
@SpringBootApplication
     ↓
告诉 Spring Boot：
1. 这是启动入口
2. 要扫描项目中的相关组件
3. 要进行自动配置
```

#### “扫描”

Spring Boot 启动后，会在一定范围内扫描项目中的类

```
Spring Boot 启动
↓
扫描项目
↓
发现 UserController
↓
发现 UserService
↓
发现其他需要管理的类
↓
把它们纳入 Spring 体系
```

------

扫描范围和启动类位置有关

Spring Boot 默认会从自身位置往下面扫描，所以实际项目中，启动类经常放在**项目主包的较上层位置**

------

#### “自动配置”

假设 `pom.xml` 有 `spring-boot-starter-web`

Spring Boot 启动时，会准备一系列常见 Web 配置，并会根据现有依赖和配置，自动完成很多初始化

------

#### 与 Maven 的区别

Maven：

- 将某个场景所需的全部 Jar 包下载到本地
- 只是保证类路径（Classpath）上有这些代码，至于这些代码该如何初始化、如何组装，Maven 完全不管

SpringApplication.run()：

- 是 Spring Boot 应用的总开关，当 JVM 执行到这个方法时，会启动 Spring 容器
- 会扫描类路径下有哪些 Jar 包，并根据这些 Jar 包的存在与否，动态创建对应的 Bean

------



## 二、Spring Boot 启动时到底发生了什么？

假设项目：

```
demo
├── pom.xml
└── src
    └── main
        ├── java
        │   └── com.example.demo
        │       ├── DemoApplication.java
        │       └── controller
        │           └── UserController.java
        │
        └── resources
            └── application.yml
```

点击运行 `DemoApplication.java`

```
JVM
↓
执行 main()
↓
SpringApplication.run()
↓
Spring Boot 启动
↓
扫描 com.example.demo 及其子包
↓
发现 UserController
↓
执行 Web 自动配置
↓
启动内嵌服务器
↓
监听端口
↓
等待请求
```

------

# 第4课：`application.yml`

## 一、`application.yml` 到底是什么

假设 Spring Boot 项目运行在：8080 端口；现在你想改成：8081 端口

最笨的方式是：找到 Spring Boot 内部启动服务器的 Java 代码，把 8080 改成 8081，但是这显然不合理

```
服务器端口
数据库地址
数据库账号密码
日志级别
当前运行环境
```

这些东西经常需要调整，但我们不希望每次调整都去修改 Java 业务代码

于是就把它们单独放进配置文件

Spring Boot 中最常见的配置文件之一就是：

```
application.yml
```

可以把它理解成 **“项目运行设置”**

```
Java 代码
→ 程序要做什么

application.yml
→ 程序运行时怎么做
```

------



## 二、YAML 这种格式怎么看

### 1. YAML 主要靠缩进表达层级

```yml
server:
  port: 8081
```

它表达的是：

```
server
└── port = 8081
```

也可以理解为：

```
server.port = 8081
```

YAML 对缩进敏感，通常用空格缩进，不要混用 Tab

养成习惯：统一使用**两个空格缩进**

------

### 2. 冒号后通常要有空格

推荐：

```
port: 8081
```

不要写：

```
port:8081
```

------



## 三、真实项目里还会配置什么

### 1. 数据库配置

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo  // 数据库的位置
    username: root
    password: 123456
```

打开项目，如果想找这个项目到底连的是哪个数据库，

第一时间就去 `application.yml` 里找

------

### 2. 日志配置

项目运行时，控制台会不断输出日志

有时候项目会配置：

```yml
logging:       → 日志相关配置
  level:
    root: INFO
```

```
INFO   → 正常运行信息
DEBUG  → 很详细，适合调试
WARN   → 警告
ERROR  → 错误
```

这些代表不同日志级别

------



## 四、多环境配置为什么存在

假设同一个项目有：

```
开发环境
测试环境
生产环境
```

它们的配置通常不一样，比如数据库：

```
开发环境
→ localhost

测试环境
→ test-db.company.com

生产环境
→ prod-db.company.com
```

端口、日志级别也可能不同，如果全都硬写在同一个文件里，会很乱

所以 Spring Boot 支持多环境配置：

```
application.yml
application-dev.yml   → development，开发
application-test.yml  → 测试
application-prod.yml  → production，生产
```

------



## 五、`application.properties` 是什么

以后打开别人的项目，可能会发现没有：`application.yml`，反而有：`application.properties`

它们承担类似角色

比如 YAML：

```
server:
  port: 8081
```

换成 properties：

```
server.port=8081
```

YAML：用缩进表达层级

properties：用点号连接层级

```
spring:
  datasource:
    username: root
```

对应：

```
spring.datasource.username=root
```

------



# 第5课：Controller（一）

## 一、Controller 到负责什么

Controller 负责接收客户端发来的 HTTP 请求，并决定由哪个 Java 方法处理这个请求

```
Spring Boot 启动
↓
扫描 Controller
↓
建立请求映射
↓
服务器等待请求
↓
HTTP Request 到来
↓
找到 Java 方法
↓
执行
```

------



## 二、Spring Boot 怎么知道一个类是 Controller

### 1. 普通 Java 类并不会自动接收请求

```java
public class UserController {
    public String getUsers() {
        return "users";
    }
}
```

名字虽然叫：

```
UserController
```

但 Spring 并不会因为类名里带了 `Controller` 就自动让这个类负责处理 Web 请求

------

### 2. `@RestController`

典型代码：

```java
@RestController
public class UserController {

}
```

这里 `@RestController` 告诉 Spring：这个类是一个专门处理 Web 请求，并把结果直接返回给客户端的 Controller

------



## 三、请求映射

假设客户端发送：

```
GET /users
```

项目里有很多类，Spring 怎么知道：`/users` 应该找 UserController？

UserController 里面可能有十几个方法，Spring 怎么知道：应该执行哪一个？

这就需要 **“请求映射”** 注解：

```java
@RequestMapping // 通用映射，或类级别声明路径前缀
@GetMapping     // 查询数据
@PostMapping    // 新增/提交数据
```

------

### 1. Mapping

`Mapping` 可以理解为：**映射 / 对应关系**

```
某种 HTTP 请求 ←→ 某个 Java 方法
```

------

### 2.`@GetMapping`（仅 GET）

```java
@RestController                // 告诉Spring，这个类是 Controller
public class UserController {

    @GetMapping("/users")      // 告诉 Spring，当收到 GET /users 请求时，执行下面这个方法
    public String getUsers() {
        return "users";
    }
}
```

可以翻译为：如果收到一个 `GET /users` 请求，就调用 `getUsers()`

于是：

```
GET /users
↓
@GetMapping("/users")
↓
getUsers()
```

------

### 3. `@PostMapping`（仅 POST）

假设要新增一个用户，REST 风格接口中可能设计成：

```
POST /users
```

那么 Controller 里可能写：

```java
@PostMapping("/users")
public String addUser() {
    return "新增用户";
}
```

```
POST /users
↓
addUser()
```

而：

```java
@GetMapping("/users")
public String getUsers() {
    return "查询用户";
}
```

```
GET /users
↓
getUsers()
```

于是两个路径完全一样：

```
/users
```

但因为 HTTP 方法不同，所以可以对应两个不同 Java 方法：

```
GET  /users
→ 查询用户

POST /users
→ 新增用户
```

------

### 4. `@RequestMapping` （全部，GET / POST / PUT / DELETE / PATCH等）

一个更真实的写法：

```java
@RestController                    // 这是一个 Web Controller
@RequestMapping("/users")          // 这个 Controller 的接口路径统一从 `/users` 开始
public class UserController {

    @GetMapping
    public String list() {
        return "查询全部用户";
    }

    @GetMapping("/{id}")
    public String getById() {
        return "查询一个用户";
    }

    @PostMapping
    public String add() {
        return "新增用户";
    }
}
```

类上面多了：@RequestMapping("/users")

而方法上的：@GetMapping、@PostMapping 反而没有写路径

当 @RequestMapping("/users") 放在整个类上，**UserController 里的这些接口，统一加一个 `/users` 前缀**

------

# 第6课：Controller（二）—— 请求参数怎么进入 Java 方法

## 一、三种常见数据位置

@RequestParam （查询参数）

```
GET /users?id=10

查询参数
→ URL ? 后面
→ @RequestParam
```

@PathVariable（路径）

```
GET /users/10

路径参数
→ URL 路径本身
→ @PathVariable
```

@RequestBody（请求体）

```json
POST /users
{
  "name": "Zhang",
  "age": 23
}

请求体
→ HTTP Body
→ @RequestBody
```

------



## 二、`@RequestParam`：拿 URL 查询参数

### 1. 查询参数

```
GET /users?id=10
```

这里真正的路径是：`/users`，而：`?id=10` 是查询参数

------

### 2. Spring Boot 里怎么接

```java
@GetMapping("/users")
public String getUser(@RequestParam Long id) {
    return "用户id：" + id;
}
```

当请求：

```
GET /users?id=10
```

Spring 会做：

```
请求参数 id=10
↓
@RequestParam Long id
↓
Java 参数 id 得到 10
```

------

### 3. 参数名可以明确写出来

```java
@GetMapping("/users")
public String getUser(@RequestParam("id") Long userId) {
    return "用户id：" + userId;
}
```

```
@RequestParam("id")
```

表示：去请求里找名为 `id` 的参数，然后把它交给 Java 变量：

```
Long userId
```

------

### 4. 默认通常要求参数存在

比如：

```java
@GetMapping
public String search(@RequestParam String name) {
    ...
}
```

但请求：

```
GET /users
```

没有 `name`，就可能出现参数缺失错误

如果它是可选的，可以写类似：

```java
@GetMapping
public String search(
        @RequestParam(required = false) String name) {  // 允许这个参数不传
    ...
}
```

------

### 5. 还能设置默认值

```java
@RequestParam(defaultValue = "1") Integer page
```

意思：如果用户没传 `page`，默认使用 1

很适合分页：

```
GET /users?page=2
```

如果没传 `page`：

```
GET /users
```

则：

```
page = 1
```

------



## 三、`@PathVariable`：拿 URL 路径里的值

```
GET /users/10
```

```java
@GetMapping("/users/{id}")
public String getUser(@PathVariable Long id) {
    return "用户id：" + id;
}
```

### 1. `{id}` 是什么意思

`{id}` 是一个占位符

```
/users/10
/users/20
/users/1001
```

都能匹配：

```
/users/{id}
```

------

### 2. 也可以明确指定名字

例如：

```java
@GetMapping("/users/{id}")
public String getUser(@PathVariable("id") Long userId) {
    return "用户id：" + userId;
}
```

对应：

```
/users/{id}
       ↓
      10

@PathVariable("id")
       ↓
Long userId
       ↓
      10
```

------



## 四、`@RequestBody`：拿 HTTP 请求体里的数据

把数据放进 HTTP Body：

```json
{
  "name": "Zhang",
  "age": 23
}
```

然后 Java 里：

```java
@PostMapping("/users")
public String addUser(@RequestBody User user) {
    return "新增用户：" + user.getName();
}
```

@RequestBody 把 HTTP 请求体里的数据拿出来，并转换成 Java 对象

------

### 1. JSON 为什么能变成 Java 对象

请求体：

```json
{
  "name": "Zhang",
  "age": 23
}
```

Java：

```java
public class User {
    private String name;
    private Integer age;
}
```

JSON转换成Java 对象的大致过程：

```json
HTTP Request Body
{
  "name": "Zhang",
  "age": 23
}
↓

@RequestBody
↓

JSON 解析
↓

User 对象
name = "Zhang"
age = 23
```

于是方法里可以直接：

```
user.getName()
user.getAge()
```

不用手工写：

```
取 JSON
解析字符串
找到 name
找到 age
```

------

### 2. `@RequestBody` 一般用对象接，不会把十几个字段拆开

不推荐变成一长串：

```java
public String add(
    String name,
    Integer age,
    String email,
    String phone,
    ...
)
```

更常见是封装成：

```java
@RequestBody User user
```

或者以后更规范地：

```java
@RequestBody UserCreateDTO dto
```

------



## 五、这三种参数可以同时存在

```
PUT /users/10?notify=true
```

Body：

```json
{
  "name": "Zhang",
  "age": 24
}
```

那么可能同时有：

```java
public String update(
    @PathVariable Long id,
    @RequestParam Boolean notify,
    @RequestBody User user
)
```

分别从 HTTP Request 的不同位置取数据

------

# 第7课：Controller（三）——返回值、JSON 与统一响应

## 一、Controller 的 `return` 最后去了哪里

```
Controller 方法 return
↓
Spring 处理
↓
HTTP Response
↓
客户端
```

### 1. 先看最简单的 String

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

当客户端访问：

```
GET /users/hello
```

执行：

```
hello()
```

然后：

```
return "hello";
```

Spring 会继续处理：

```
HTTP Response
↓
Response Body
↓
hello
```

所以前端最终收到：

```
hello
```

------

### 2. `@RestController` 在这里再次发挥作用

```
@RestController
```

通常意味着：Controller 方法的**返回值直接用于响应客户端**

```
return "hello";
```

这里的 `"hello"` 会成为响应内容

Spring 把 Java 返回值转成 HTTP Response

------



## 二、返回 Java 对象时发生什么

```java
public class User {

    private Long id;
    private String name;
    private Integer age;

    // getter / setter
}
```

Controller：

```java
@GetMapping("/{id}")
public User getUser(@PathVariable Long id) {

    User user = new User();
    user.setId(id);
    user.setName("Zhang");
    user.setAge(23);

    return user;  // 注意这里返回的是一个 Java `User` 对象
}
```

但前端并不认识 JVM 里的 User 对象

前端真正能收到的通常是：

```json
{
  "id": 10,
  "name": "Zhang",
  "age": 23
}
```

这说明中间发生了转换：

```
Java User 对象
↓
Spring Boot
↓
JSON
↓
HTTP Response Body
```

这就是：**JSON 自动转换**

序列化：

```
Java 对象
↓
JSON
```

反序列化：

```
JSON
↓
Java 对象
```

------

### List 也可以自动变成 JSON

```java
@GetMapping
public List<User> list() {
    return userList;
}
```

假设 Java 中：

```
userList
├── User(id=1, name="A")
├── User(id=2, name="B")
└── User(id=3, name="C")
```

前端可能收到：

```json
[
  {
    "id": 1,
    "name": "A"
  },
  {
    "id": 2,
    "name": "B"
  },
  {
    "id": 3,
    "name": "C"
  }
]
```

------



## 三、统一返回对象 `Result<T>`

### 1. 如果每个接口都随意返回

接口 A：

```json
{
  "id": 1,
  "name": "Zhang"
}
```

接口 B：

```json
[
  {
    "id": 1
  },
  {
    "id": 2
  }
]
```

接口 C 失败：

```
用户不存在
```

接口 D 又返回：

```json
{
  "error": "数据库异常"
}
```

前端会发现：每个接口返回格式都不一样，它必须针对每个接口单独判断：

```
这个接口返回对象
那个返回数组
失败时有时 String
有时 error
有时 status
```

维护会越来越麻烦，于是实际项目经常引入：**统一返回结构**

------



### 2. 常见的返回格式

```json
{
  "code": 200,          // 业务状态码
  "message": "success", // 提示信息
  "data": {             // 真正的数据
    "id": 10,
    "name": "Zhang"
  }
}
```

那么 Java 可以设计：

```java
public class Result<T> {
    private Integer code;
    private String message;
    private T data;
}
```

这里 `<T>` 就是 **泛型**，表示：`data` 可以装不同类型的数据

------

#### 1. 查询一个用户

返回：`Result<User>`，其中：`T = User`

所以：`private T data;`，实际上相当于：`private User data;`

------

#### 2. 查询用户列表

返回：`Result<List<User>>`，那么：`T = List<User>`

于是最终可能是：

```json
{
  "code": 200,
  "message": "success",
  "data": [
    {
      "id": 1,
      "name": "A"
    },
    {
      "id": 2,
      "name": "B"
    }
  ]
}
```

------

#### 3. 没有数据时也可以统一结构

例如删除成功：

```json
{
  "code": 200,
  "message": "删除成功",
  "data": null
}
```

前端无论调用哪个接口，都可以先统一处理：

```
看 code
↓
看 message
↓
需要时再读取 data
```

这就是统一返回格式最大的好处。

------



### 3.`Result<T>` 为什么使用泛型

如果不用泛型，可能写：

```java
public class Result {
    private Integer code;
    private String message;
    private Object data;
}
```

但是 Object 太宽泛了，

使用：Result<User>，开发者一眼就知道：`data` 是 User

使用：Result<List<User>>，一眼知道：`data` 是用户列表

------



### 4. 提供静态工具方法

为了方便使用，这个类通常还会提供一些静态工具方法，

比如 `Result.success(data)` 和 `Result.error(message)`，用来快速构造成功或失败的结果对象

#### 1.`success` 方法：代表“一切正常”

```java
// 只传数据，状态码和消息使用默认值
public static <T> Result<T> success(T data) 

// 完全自定义（较少用）
public static <T> Result<T> success(Integer code, String message, T data) 
```

#### 2. `error` 方法：代表“出问题了”

```java
// 只传错误消息，使用通用的失败状态码（较少用）
public static <T> Result<T> error(String message) 

// 自定义错误码和消息（更常用，便于前端分类处理）
public static <T> Result<T> error(Integer code, String message) 
```

例子：

```java
public class Result<T> {
    private Integer code;
    private String message;
    private T data;
}
```

```java
@RestController
public class UserController {
    
    // 获取用户信息接口
    @GetMapping("/user/{id}")
    public Result<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        // 手动将数据和成功状态包装起来返回
        return Result.success(user); 
    }
    
    // 一个可能失败的接口
    @PostMapping("/login")
    public Result<String> login(@RequestBody LoginRequest request) {
      
        // 假设登录失败
        if (invalid) {
            // 返回一个错误结果
            return Result.error(401, "用户名或密码错误");
        }
        return Result.success("登录成功");
    }
}
```




## 四、`ResponseEntity` 是什么

```java
return user;
```

Spring 自动生成 HTTP Response

但有时候开发者想更明确地控制：

```
HTTP 状态码
响应头
响应体
```

这时可以使用：**ResponseEntity**

```java
@GetMapping("/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    ...
}
```

这里表达：我返回的不只是业务数据 `User`，而是整个 HTTP Response 的一种封装

可以表达：

```json
HTTP Status: 200

Body:
{
  "id": 10,
  "name": "Zhang"
}
```

或者：

```json
HTTP Status: 404
```

------

### 1. `ResponseEntity` 和 `Result<T>` 不是一回事

`ResponseEntity` 更偏：**HTTP 协议层**，它关心：

- HTTP 状态码
- Headers
- Body

`Result<T>` 更偏：项目自己规定的**业务响应格式**

例如：

```json
{
  "code": 200,  // 不是http状态码，是自定义的业务码
  "message": "success",
  "data": {}
}
```

这个 `code = 200` 甚至不一定就是 HTTP Status Code，它可能只是项目自己定义的业务码。

所以可以先区分：

```
ResponseEntity
→ HTTP 响应封装
```

```
Result<T>
→ 项目业务响应格式
```

------



## 五、把 Controller 三课串起来

画出完整结构：

```
客户端发送 HTTP Request
↓

@RequestMapping / @GetMapping / @PostMapping
↓

找到 Controller 方法
↓

@RequestParam / @PathVariable / @RequestBody
↓

把请求数据放进 Java 参数
↓

执行 Java 方法
↓

return String / User / List / Result<T>
↓

Spring 自动处理返回值
↓

Java 对象 → JSON
↓

HTTP Response
↓

客户端
```

------



# 第8课：Service

## 一、什么叫“业务逻辑”

这个系统为了完成实际业务，需要遵守哪些规则、执行哪些步骤

例如银行转账，用户请求：

```
把 500 元从 A 转给 B
```

真正的业务逻辑可能是：

```
A 是否存在？
↓
B 是否存在？
↓
A 账户是否被冻结？
↓
转账金额是否大于 0？
↓
A 的余额够不够？
↓
是否超过每日转账额度？
↓
A 扣 500
↓
B 加 500
↓
生成交易记录
```

这些就是：**转账业务规则**

Controller 不应该自己承担这些规则

------



## 二、Controller 和 Service 应该怎么分工

### 1. Controller 负责什么

```
接收 HTTP 请求
↓
获取请求参数
↓
调用 Service
↓
把结果返回给客户端
```

比如：

```java
@PostMapping
public User add(@RequestBody User user) {
    return userService.add(user);
}
```

主要工作：前端要新增用户，那我把用户数据交给 `userService.add()`

------

### 2. Service 负责什么

真正的业务逻辑放到：

```java
public class UserService {
}
```

例如：

```java
public User add(User user) {

    // 检查数据
    // 判断用户是否已存在
    // 新增用户
    // 返回结果

}
```

于是结构变成：

```
HTTP Request
↓
Controller
↓
Service
↓
执行真正业务
```

------



## 三、把 CRUD 拆成 Controller + Service

### 1. 原来的坏结构

```
UserController
├── 接收 GET
├── 查询用户
├── 接收 POST
├── 新增用户
├── 接收 PUT
├── 修改用户
└── 删除用户
```

Web 代码和业务代码混在一起

------

### 2. 拆分以后

```
UserController
├── 接收 GET
├── 接收 POST
├── 接收 PUT
└── 接收 DELETE

        ↓ 调用

UserService
├── 查询用户
├── 新增用户
├── 修改用户
└── 删除用户
```

```java
@RestController
@RequestMapping("/users")
public class UserController {

    private UserService userService;

    @GetMapping
    public List<User> list() {
        return userService.list();
    }

    @PostMapping
    public User add(@RequestBody User user) {
        return userService.add(user);
    }

    @PutMapping("/{id}")
    public User update(
            @PathVariable Long id,
            @RequestBody User user) {

        return userService.update(id, user);
    }

    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

调用关系：

```
Controller
↓
userService.xxx()
↓
Service
```

------

### 3. Service 里面才真正操作数据

```java
@Service
public class UserService {

    private final List<User> users = new ArrayList<>();

    public List<User> list() {
        return users;
    }

    public User add(User user) {
        users.add(user);
        return user;
    }

    public User update(Long id, User newUser) {
        for (User user : users) {
            if (user.getId().equals(id)) {
                user.setName(newUser.getName());
                user.setAge(newUser.getAge());
                return user;
            }
        }
        return null;
    }

    public void delete(Long id) {
        users.removeIf(user -> user.getId().equals(id));
    }
}
```

**把 Web 层和业务层拆开**

------



## 四、分层的好处

### 1. Controller 会保持简单

理想情况下，一个 Controller 方法可能只有：

```java
@GetMapping("/{id}")
public User getById(@PathVariable Long id) {
    return userService.getById(id);
}
```

```
HTTP 请求进来
↓
拿 id
↓
交给 Service
↓
返回结果
```

------

### 2. 业务逻辑集中在一个地方

假设：网页端、手机 App、后台管理系统，都需要：查询用户详细信息

使用 Service：

```
Web Controller ──┐
                 │
App Controller ──┼→ UserService.getUser()
                 │
Admin Controller ┘
```

业务规则只写一次，大家都可以复用：

```
userService.getUser(...)
```

测试、修改业务规则也更容易

------



# 第9课：Entity、DTO、VO

| 类型   | 最简单的理解                   |
| ------ | ------------------------------ |
| Entity | 数据库中的数据在 Java 里的表示 |
| DTO    | 用于接口/层之间传递的数据      |
| VO     | 返回给前端展示的数据           |

## 一、先看问题：一个 `User` 能不能从头用到尾？

前端提交：

```json
{
  "name": "Zhang",
  "age": 23
}
```

Controller：

```java
@PostMapping
public User add(@RequestBody User user) {
    return userService.add(user);
}
```

Service 继续用这个 `User`，数据库也映射成 `User`，最后又直接：

```java
return user;
```

于是整个流程：

```
前端 JSON
↓
User
↓
Service
↓
数据库
↓
User
↓
前端 JSON
```

问题是：**前端传进来的数据、数据库里的数据、返回给前端的数据，通常不是完全一样的**

------

### 1. 新增用户时，前端可能只传

```
name
password
email
```

但数据库里的 User 可能还有：

```
id
createTime
updateTime
status
```

这些并不是前端新增用户时应该负责提供的

------

### 2. 返回用户时，也可能不能把所有字段都给前端

数据库 Entity：

```java
class User {
    Long id;
    String username;
    String password;
    String email;
}
```

如果直接：

```java
return user;
```

可能把：`password` 也返回出去

因此真实项目常把“不同阶段的数据形态”拆开。

------



## 二、Entity：数据库里的数据在 Java 中的表示

Entity ：**Java 程序内部，用来表示数据库数据的对象**

### 1. Entity 主要对应持久化数据

例如数据库里有：

```
user 表

id
username
password
email
status
create_time
```

Java 可能有：

```java
public class User {

    private Long id;
    private String username;
    private String password;
    private String email;
    private Integer status;
    private LocalDateTime createTime;
}
```

这个 `User` 就可以作为 Entity

```
MySQL 中的一行 user 数据
↓
User Entity
```

------



## 三、DTO：用来传递数据

DTO 全称：Data Transfer Object，数据传输对象

是：**为了某一次数据传输，专门定义的数据结构**

------

### 1. 新增用户 DTO

前端新增用户时只需要：

```json
{
  "username": "zhang",
  "password": "123456",
  "email": "a@example.com"
}
```

可以专门定义：

```java
public class UserCreateDTO {
    private String username;
    private String password;
    private String email;
}
```

Controller：

```java
@PostMapping
public void add(@RequestBody UserCreateDTO dto) {
    userService.add(dto);
}
```

这个对象专门负责接收“创建用户”所需要的数据，它不需要：

```
id
createTime
status
```

------

### 2. 修改用户可能是另一个 DTO

比如修改资料只允许：

```
nickname
email
```

于是：

```java
public class UserUpdateDTO {
    private String nickname;
    private String email;
}
```

这样比把整个 `User` 交给前端修改安全很多

还有类似的：

```
UserCreateDTO
UserUpdateDTO
LoginDTO
PasswordChangeDTO
```

DTO 不一定“一张数据库表只对应一个”，**同一个 Entity 可能对应很多 DTO**

不同接口需要的数据不同

------



## 四、VO：专门给前端看的数据

VO：View Object，视图对象，**最终准备返回给前端的数据结构**

例如数据库里的 `User`：

```java
public class User {

    private Long id;
    private String username;
    private String password;
    private Integer status;
}
```

但前端页面只需要：

```
id
username
statusName
```

于是：

```java
public class UserVO {
    private Long id;
    private String username;
    private String statusName;
}
```

最终 Controller：

```java
@GetMapping("/{id}")
public Result<UserVO> getUser(@PathVariable Long id) { // 返回类型是：Result<UserVO>
    return Result.success(userService.getUser(id));
}
```

前端收到：

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": 10,
    "username": "zhang",
    "statusName": "正常"
  }
}
```

------



## 五、把三者放在一条数据流里

假设前端创建用户：

```
前端 JSON
↓
DTO
↓
Service
↓
Entity
↓
Mapper
↓
数据库
```

查询时再反过来：

```
数据库
↓
Mapper
↓
Entity
↓
Service
↓
VO
↓
Controller
↓
JSON
↓
前端
```

合起来就是教学计划里的核心数据流：

```
JSON
↓
DTO
↓
Entity
↓
VO
↓
JSON
```

------



## 六、为什么不能一直直接用 Entity

### 1. 防止前端修改不应该修改的字段

假设 Entity：

```java
class User {
    Long id;
    String username;
    Integer role;
    Integer status;
}
```

如果新增接口直接：

```java
@RequestBody User user
```

那么前端理论上可能提交：

```java
{
  "username": "zhang",
  "role": 999,
  "status": 1
}
```

但你的业务可能根本不允许普通用户自己决定：

```
role
status
```

用 DTO 后：

```java
class UserCreateDTO {
    String username;
}
```

这些字段压根就不存在

------

### 2. 防止敏感字段返回

典型就是：

```
password
```

数据库 Entity 可以有密码字段，VO 不放

这样边界很清楚：

```
Entity → 内部使用
VO     → 对外输出
```

------

### 3. 数据库变化不容易直接影响 API

比如数据库后来增加：

```
internal_flag
deleted
last_login_ip
```

如果 Controller 直接返回 Entity，这些字段有可能影响返回结构

使用 VO：

```
数据库结构
和
前端 API 结构
```

就可以相对独立。

------



# 第10课：Mapper——Service 为什么不直接操作数据库

这一课进入“数据访问”模块。按照教学计划，我们先只讲思想：**Mapper 是什么、为什么需要 Mapper、它和 SQL、数据库是什么关系，以及完整调用流程。真正的 SQL 留到下一阶段。**

## 一、Mapper 在整个项目里处于什么位置

前面我们已经有：

```
Controller
↓
Service
```

现在再往下补一层：

```
Controller
↓
Service
↓
Mapper
↓
MySQL
```

可以先用一句话理解：

> **Mapper 负责数据访问，也就是让 Java 业务代码和数据库发生联系。**

### 1. 为什么 Service 不直接写 SQL

假设查询用户：

```
public User getUser(Long id) {
    // 业务判断
    // 写 SQL
    // 连数据库
    // 执行查询
    // 解析结果
}
```

技术上当然能这么做，但职责就混乱了。

`Service` 本来应该关心：

```
这个用户能不能查？
查到之后还要做什么业务处理？
```

而不是关心：

```
SQL 怎么写？
数据库连接怎么拿？
结果集怎么转换？
```

所以把数据库访问单独交给 Mapper：

```
Service
→ 负责“我要什么数据”

Mapper
→ 负责“怎么从数据库拿到这些数据”
```

这和上一课 Controller/Service 的分层逻辑完全一样。

------

## 二、Mapper 到底长什么样

在 MyBatis 项目里，你以后经常看到：

```
public interface UserMapper {

    User selectById(Long id);

}
```

注意它经常是：

```
interface
```

而不是普通 `class`。

你现在可能会立刻产生一个问题：

> 接口不是没有方法实现吗？那 `selectById()` 到底是谁执行的？

这个问题先记下来。

真正的答案涉及 MyBatis 如何根据 Mapper 和 SQL 创建实现，属于下一阶段数据库/MyBatis 的重点。

这一课只需要知道：

> **Service 调用 Mapper 方法，Mapper 再负责完成对应的数据访问。**

例如：

```
public User getUser(Long id) {
    return userMapper.selectById(id);
}
```

调用链就是：

```
UserService.getUser(10)
↓
UserMapper.selectById(10)
↓
数据库查询
↓
得到 User
↓
返回给 Service
```

------

## 三、Mapper 和 SQL 是什么关系

### 1. Mapper 方法背后通常对应数据库操作

比如：

```
User selectById(Long id);
```

背后逻辑上可能对应：

```
SELECT *
FROM user
WHERE id = ?
```

再比如：

```
void insert(User user);
```

背后可能对应：

```
INSERT INTO user ...
```

再比如：

```
void deleteById(Long id);
```

背后对应删除操作。

所以可以这样理解：

```
Mapper 方法
↓
对应某种 SQL 操作
↓
操作数据库
```

但是以后你会发现 SQL 的具体写法可能有多种形式，例如：

```
Mapper XML
注解
MyBatis-Plus 自动生成
```

这些目前都先不展开。

------

### 2. Mapper 不是数据库本身

这个区别要明确：

```
Mapper
≠ MySQL
```

Mapper 是 Java 项目中的：

> **数据访问层。**

真正保存数据的是：

```
MySQL
```

关系是：

```
Java 世界                 数据库世界

Service
   ↓
Mapper
   ↓
SQL
   ↓
MySQL
```

Mapper 可以理解成两边之间的桥梁。

------

## 四、把查询用户完整走一遍

假设前端请求：

```
GET /users/10
```

Controller：

```
@GetMapping("/{id}")
public Result<UserVO> getUser(@PathVariable Long id) {
    return Result.success(userService.getUser(id));
}
```

Service：

```
public UserVO getUser(Long id) {

    User user = userMapper.selectById(id);

    // 后续业务处理
    // Entity → VO

    return userVO;
}
```

Mapper：

```
public interface UserMapper {

    User selectById(Long id);

}
```

完整流程：

```
GET /users/10
↓
Controller
↓
id = 10
↓
userService.getUser(10)
↓
Service
↓
userMapper.selectById(10)
↓
Mapper
↓
执行对应数据库查询
↓
MySQL
↓
查询到用户数据
↓
转换为 User Entity
↓
Mapper 返回 User
↓
Service 处理
↓
转换成 UserVO
↓
Controller
↓
JSON
↓
前端
```

到这里，你第一课看到的总图已经基本落地了。

------

## 五、为什么要有 Mapper 这一层

### 1. 职责分离

现在三层职责已经非常清楚：

```
Controller
→ HTTP 请求与响应

Service
→ 业务逻辑

Mapper
→ 数据库访问
```

例如：

> 查询 10 号用户，并判断他是否允许登录。

Service 负责：

```
查询用户
↓
用户是否存在
↓
账号是否被冻结
↓
账号是否被删除
```

Mapper 只需要负责：

```
帮我把 10 号用户从数据库查出来
```

它不应该关心：

> 这个用户能不能登录。

因为那是业务规则。

------

### 2. 数据访问方式变化时，业务层受影响更小

比如今天项目用：

```
MySQL
```

Service 调：

```
userMapper.selectById(id);
```

以后底层查询方式调整了，只要 Mapper 层的接口和行为尽量保持稳定，Service 不需要知道底层发生了多少变化。

所以分层的意义仍然是：

> **让不同职责尽量彼此独立。**

------

## 六、Mapper、Entity、Service 串起来

上一课我们讲：

```
Entity
→ 数据库数据在 Java 中的表示
```

现在就能看到它真正出现的位置。

数据库：

```
user 表

id = 10
username = Zhang
age = 23
```

Mapper 查询后得到：

```
User user;
```

这个 `User` 就是 Entity。

所以：

```
MySQL 一行数据
↓
Mapper
↓
User Entity
↓
Service
```

如果要返回前端：

```
User Entity
↓
Service 转换
↓
UserVO
↓
Controller
↓
JSON
```

现在第10课的 Entity/VO 和第11课的 Mapper 也连起来了。

------

## 七、以后读项目时怎么顺着 Mapper 看

假设你看到：

```
@GetMapping("/{id}")
public Result<UserVO> get(@PathVariable Long id) {
    return Result.success(userService.getById(id));
}
```

进入 Service：

```
public UserVO getById(Long id) {
    User user = userMapper.selectById(id);
    ...
}
```

这时候你就应该知道：

> 真正的数据查询继续往 `userMapper.selectById()` 追。

于是阅读路线：

```
Controller
↓
Service
↓
Mapper
↓
SQL / 数据库
```

以后 Codex 修改了一个“查询用户”功能，你也可以沿着这条链检查：

```
接口有没有改？
↓
业务逻辑有没有改？
↓
Mapper 查询有没有改？
↓
SQL 有没有改？
```

这就是很典型的 Spring Boot 项目阅读方式。

------

## 八、一个容易混淆的地方：Mapper 和 Service 都可能有 CRUD 方法

例如：

```
UserService.getById()
```

里面调用：

```
UserMapper.selectById()
```

看起来两个方法都在“查用户”。

但它们的含义不一样。

Service：

```
getById()
→ 完成“查询用户”这个业务
```

Mapper：

```
selectById()
→ 从数据库按 id 查询数据
```

Service 以后可能变成：

```
查数据库
↓
检查用户状态
↓
处理权限
↓
转换 VO
↓
返回
```

而 Mapper 仍然只是：

```
查数据库
```

所以不要把：

```
Service = Mapper 的转发器
```

作为最终理解。

简单 CRUD 项目里有时候看起来像，但复杂业务一来，区别会非常明显。

------

## 九、本课只需要掌握这张图

到现在，一个典型请求已经可以完整画成：

```
前端
↓
Controller
   HTTP
↓
Service
   业务逻辑
↓
Mapper
   数据访问
↓
MySQL
```

回来时：

```
MySQL
↓
Mapper
↓
Entity
↓
Service
↓
VO
↓
Controller
↓
JSON
↓
前端
```

这一课不要求你会写 SQL，也不要求你理解 MyBatis 为什么能给接口生成实现。

教学计划明确把真正的 SQL 放在下一阶段；这里的目标只是先理解 Mapper 在项目架构中的职责。第三阶段：SpringBoot教学计划.mdMD

下一课 **第12课：Bean、IoC** 会开始解决一个我们故意拖到现在的问题：

```
private UserService userService;
private UserMapper userMapper;
```

我们一直在写：

```
Controller 调 Service
Service 调 Mapper
```

但始终没有解释：

> **这些对象到底是谁创建的？为什么代码里经常根本看不到 `new UserService()`、`new UserMapper()`？**

这就是 Spring 真正的核心开始出现的地方。



# 第11课：Bean、IoC——对象到底是谁创建的

前面我们一直在写这种结构：

```
Controller
↓
Service
↓
Mapper
```

也一直默认：

```
userController 里有 userService
userService 里有 userMapper
```

但我们始终没回答一个关键问题：

> **这些对象到底是谁创建的？为什么真实 Spring 项目里经常看不到 `new UserService()`？**

这正是这一课的核心。教学计划也明确要求从“Controller 为什么不用 new”自然引出 Bean、IoC 和 Spring 容器。第三阶段：SpringBoot教学计划.mdMD

## 一、先看传统 Java 是怎么做的

假设没有 Spring。

你想让 Controller 使用 Service，最直接的写法是：

```
public class UserController {

    private UserService userService = new UserService();

}
```

Service 又要用 Mapper：

```
public class UserService {

    private UserMapper userMapper = new UserMapper();

}
```

于是：

```
UserController
自己 new UserService

UserService
自己 new UserMapper
```

这种写法很符合普通 Java 直觉：

> 我要一个对象，我就自己 `new`。

但 Spring 的核心思想恰恰是：

> **很多重要对象，不再由业务代码自己 new，而是交给 Spring 统一创建和管理。**

------

## 二、Bean 是什么

先给一个现阶段最实用的定义：

> **Bean，就是被 Spring 容器创建和管理的对象。**

比如：

```
@Service
public class UserService {
}
```

如果 Spring 扫描到这个类，并把它创建出来、放进容器管理，那么这个 `UserService` 对象就是一个 Bean。

同理：

```
@RestController
public class UserController {
}
```

这个 Controller 对象通常也是 Bean。

所以：

```
UserController 对象
UserService 对象
某些 Mapper 对象
某些 Config 对象
```

都可能是 Spring Bean。

### 1. Bean 不是一种特殊 Java 类型

这一点很重要。

Bean 本质上仍然只是普通 Java 对象。

比如：

```
UserService userService = new UserService();
```

这是普通 Java 对象。

而如果这个 `UserService` 对象由 Spring 创建并管理：

> 它就被称为 Spring Bean。

所以区别不在“这个类长得不一样”，而在于：

> **谁创建它、谁管理它。**

------

## 三、Spring 容器是什么

既然 Bean 要由 Spring 管理，那总要有个地方放这些对象。

你可以先把：

```
Spring 容器
```

理解成一个大型“对象管理中心”。

启动时大致发生：

```
Spring Boot 启动
↓
扫描项目
↓
发现 @RestController
发现 @Service
发现其他组件
↓
创建对应对象
↓
把这些对象放进 Spring 容器
↓
以后统一管理
```

比如容器里可能有：

```
Spring 容器

├── UserController Bean
├── UserService Bean
├── OrderController Bean
├── OrderService Bean
└── ...
```

所以你可以把第3课的“扫描”继续补全：

```
扫描
↓
找到需要 Spring 管理的类
↓
创建 Bean
↓
放进 Spring 容器
```

------

## 四、什么是 IoC

IoC 全称：

> Inversion of Control，控制反转。

这个名字非常容易把初学者吓到，但思想其实没有那么复杂。

### 1. 什么“控制”被反转了

传统 Java：

```
public class UserController {

    private UserService userService = new UserService();

}
```

这里：

> `UserController` 自己决定什么时候创建 `UserService`。

也就是说：

```
对象创建权
在自己的代码手里
```

而 Spring：

```
UserController 不自己 new
↓
Spring 创建 UserService
↓
Spring 管理 UserService
↓
需要时再把它提供给 UserController
```

也就是说：

> 对象的创建和管理控制权，从业务代码转移给了 Spring。

这就是所谓：

```
控制反转
```

可以记成：

```
以前：
我需要对象
→ 我自己 new

Spring：
我需要对象
→ Spring 给我
```

这就是 IoC 最核心的直觉。

------

## 五、为什么要把对象交给 Spring 管

你可能会问：

> 我自己 `new` 明明也能用，为什么搞这么复杂？

小项目里确实能。

但项目一复杂，Spring 统一管理对象会有明显价值。

### 1. 减少对象之间的强绑定

如果：

```
UserController
```

里面写死：

```
new UserService()
```

那么 Controller 和这个具体实现绑得很紧。

以后如果创建方式发生变化，就得改 Controller。

而交给 Spring：

```
Controller 只表达：
“我需要一个 UserService”

至于这个对象怎么创建
→ Spring 负责
```

业务类就少管很多“对象组装”的事情。

### 2. Spring 可以统一管理对象生命周期

例如：

```
什么时候创建
是否只创建一个
什么时候销毁
创建前后做什么
```

都可以由容器统一控制。

这些细节现阶段不用深入。

### 3. 为后面的依赖注入打基础

真正最实用的是：

> Spring 有了这些 Bean 以后，就可以自动把一个 Bean 提供给另一个 Bean 使用。

例如：

```
UserController Bean
需要
UserService Bean
```

Spring 可以把两者组装起来。

这就是下一课的：

> **依赖注入 DI。**

------

## 六、Bean、IoC、容器三者怎么联系

这三个词很容易混，所以直接放在一起。

### 1. Bean

```
被 Spring 管理的对象
```

例如：

```
UserService 对象
```

### 2. Spring 容器

```
负责保存、创建、管理这些 Bean 的系统
```

### 3. IoC

```
一种思想：
对象创建和管理权交给 Spring 容器
```

可以压缩成：

```
IoC
↓
让 Spring 接管对象创建与管理
↓
Spring 容器
↓
创建并管理各种 Bean
```

这三个概念其实是一条链。

------

## 七、把前面的项目重新看一遍

假设有：

```
@RestController
public class UserController {
}
@Service
public class UserService {
}
```

Spring Boot 启动后，可以粗略理解为：

```
扫描 UserController
↓
创建 UserController Bean

扫描 UserService
↓
创建 UserService Bean

↓
都放进 Spring 容器
```

于是容器里已经有：

```
UserController
UserService
```

接下来真正的问题只剩：

> **怎么把 UserService 交给 UserController？**

这就是下一课的“依赖注入”。

------

## 八、一个容易误解的地方

不要把 IoC 理解成：

> “以后 Java 就完全不能 `new` 了。”

不是。

普通对象当然仍然可以：

```
User user = new User();
```

尤其像：

```
DTO
VO
Entity
临时业务对象
```

很多时候仍然会正常 `new`。

Spring 主要管理的是那些：

> **长期参与应用结构、彼此协作的组件对象。**

例如：

```
Controller
Service
Repository / Mapper
Configuration
```

所以更准确的理解是：

> 不是“Spring 项目禁止 new”，而是“核心组件的创建和组装通常交给 Spring”。

------

## 九、以后看到代码时应该怎么读

假设你看到：

```
@Service
public class UserService {
}
```

第一反应：

> 这是一个 Service 组件，通常会成为 Spring Bean。

看到：

```
@RestController
public class UserController {
}
```

第一反应：

> 这是 Controller，也通常由 Spring 管理。

然后你看到 Controller 里没有：

```
new UserService()
```

不要觉得奇怪。

应该想到：

```
UserService
已经由 Spring 容器创建
↓
之后通过依赖注入交给 Controller
```

------

## 十、本课最重要的四句话

第一：

> **Bean 就是被 Spring 创建和管理的对象。**

第二：

> **Spring 容器负责创建、保存和管理这些 Bean。**

第三：

> **IoC 的核心，就是把对象创建和管理的控制权从业务代码交给 Spring。**

第四：

传统方式：

```
Controller
↓
自己 new Service
```

Spring 方式：

```
Spring 容器创建 Service Bean
↓
再提供给 Controller
```

到这里，第12课的目标就完成了。第三阶段：SpringBoot教学计划.mdMD

下一课是 **第13课：依赖注入**，会把最后一块拼上：

> Spring 容器里明明已经有 `UserService` Bean 了，它到底怎么自动进入 `UserController`？

届时会讲：

```
@Autowired
构造器注入
为什么更推荐构造器注入
Bean 是怎么匹配的
```



# 第12课：依赖注入——Spring 怎么把 Bean 交给另一个 Bean

上一课我们解决了：

```
Spring Boot 启动
↓
扫描组件
↓
创建 Bean
↓
放进 Spring 容器
```

这一课继续回答真正落地的问题：

> **容器里已经有 `UserService` 了，`UserController` 到底怎么拿到它？**

这就是“依赖注入”，简称 **DI（Dependency Injection）**。教学计划这一课要求掌握 `@Autowired`、构造器注入、为什么更推荐构造器注入，以及 Spring 如何寻找 Bean。第三阶段：SpringBoot教学计划.mdMD

## 一、什么叫“依赖”

先看：

```
@RestController
public class UserController {

    private UserService userService;

}
```

这里：

```
UserController
需要
UserService
```

也就是说：

> `UserController` **依赖** `UserService`。

同理：

```
@Service
public class UserService {

    private UserMapper userMapper;

}
```

就是：

```
UserService
依赖
UserMapper
```

所以“依赖”这个词并不神秘，就是：

> **一个对象工作时需要另一个对象。**

------

## 二、什么叫“注入”

上一课的普通 Java 写法是：

```
private UserService userService = new UserService();
```

这是：

> Controller 自己创建依赖。

而 Spring 的思路是：

```
Spring 容器里已经有 UserService Bean
↓
发现 UserController 需要 UserService
↓
把这个 Bean 交给 UserController
```

这个“把依赖提供进去”的过程，就是：

> **依赖注入。**

因此：

```
IoC
→ 对象交给 Spring 管

DI
→ Spring 再把这些对象组装起来
```

这两个概念关系非常紧密。

------

## 三、`@Autowired` 是什么

你以后可能看到：

```
@RestController
public class UserController {

    @Autowired
    private UserService userService;

}
```

这里：

```
@Autowired
```

可以暂时理解成：

> **Spring，请帮我找一个合适的 `UserService` Bean，放到这里。**

于是：

```
Spring 容器

UserService Bean
      │
      ↓
@Autowired
      │
      ↓
UserController.userService
```

这样 Controller 就不用：

```
new UserService()
```

了。

### 1. Spring 大致怎么找

假设容器里有：

```
UserService Bean
OrderService Bean
ProductService Bean
```

Spring 看到：

```
@Autowired
private UserService userService;
```

会根据所需要的类型去寻找：

```
我要 UserService 类型
↓
容器里查找
↓
找到 UserService Bean
↓
注入
```

现阶段先理解为：

> **主要根据类型匹配 Bean。**

如果同一个类型出现多个候选 Bean，就会产生新的匹配问题；这属于以后更深入的内容。

------

## 四、为什么现在更推荐构造器注入

虽然：

```
@Autowired
private UserService userService;
```

很好理解，但真实项目中更常推荐：

```
@RestController
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

这就是：

> **构造器注入。**

Spring 创建 `UserController` 时发现：

```
public UserController(UserService userService)
```

也就是：

> 想创建 UserController，必须先给我一个 UserService。

Spring 就会去容器中寻找：

```
UserService Bean
```

然后相当于完成：

```
new UserController(userServiceBean);
```

当然真正创建过程由 Spring 控制，你不用自己写这句。

------

### 1. 为什么推荐构造器注入

最重要的原因是：**依赖关系更明确。**

看到：

```
public UserController(UserService userService) {
    this.userService = userService;
}
```

你马上知道：

> UserController 没有 UserService 就无法正常创建。

而且可以写：

```
private final UserService userService;
```

表示：

> 这个依赖创建后就固定下来。

还有一个很直观的好处：

普通 Java 测试时也可以直接：

```
UserService service = ...;
UserController controller = new UserController(service);
```

不需要依赖 Spring 帮你偷偷给字段赋值。

所以可以先记：

```
字段注入
→ 简单直观

构造器注入
→ 依赖更明确，通常更推荐
```

------

## 五、为什么构造器上有时没有 `@Autowired`

你以后很可能看到：

```
@RestController
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

然后疑惑：

> 不是说要 `@Autowired` 吗？这里怎么没有？

在现代 Spring 中，如果这个 Bean **只有一个构造器**，Spring 通常可以直接判断：

> 创建这个对象必须使用这个构造器。

所以不需要额外写：

```
@Autowired
```

也就是说：

```
public UserController(UserService userService)
```

已经足够表达依赖关系。

因此以后你看到没有 `@Autowired` 的构造器注入，不要以为：

> 这不是依赖注入。

它依然是。

------

## 六、把 Controller → Service → Mapper 真正组装起来

假设：

```
@RestController
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

然后：

```
@Service
public class UserService {

    private final UserMapper userMapper;

    public UserService(UserMapper userMapper) {
        this.userMapper = userMapper;
    }
}
```

Spring 启动时可以粗略理解为：

```
创建 / 准备 UserMapper Bean
↓
创建 UserService
需要 UserMapper
↓
把 UserMapper 注入 UserService
↓
得到 UserService Bean
↓
创建 UserController
需要 UserService
↓
把 UserService 注入 UserController
↓
得到 UserController Bean
```

于是整个对象关系真正变成：

```
UserController Bean
       ↓
UserService Bean
       ↓
UserMapper Bean
```

这也就是前面一直在画的：

```
Controller
↓
Service
↓
Mapper
```

现在不仅是“代码调用关系”，还是 Spring 帮你建立出来的**对象依赖关系**。

------

## 七、IoC 和 DI 到底怎么区分

这两个概念经常一起出现。

最简单的区分：

### 1. IoC

关注：

> **对象归谁管？**

答案：

```
以前自己 new
↓
现在交给 Spring 容器
```

### 2. DI

关注：

> **一个 Bean 需要另一个 Bean 时怎么办？**

答案：

```
Spring 把需要的 Bean 提供进去
```

所以：

```
IoC
→ Spring 接管对象

DI
→ Spring 负责对象之间的组装
```

可以说：

> **依赖注入是实现 IoC 思想的重要方式。**

------

## 八、读真实代码时应该怎么判断

以后看到：

```
@Service
public class OrderService {

    private final UserMapper userMapper;
    private final ProductMapper productMapper;
    private final OrderMapper orderMapper;

    public OrderService(
            UserMapper userMapper,
            ProductMapper productMapper,
            OrderMapper orderMapper) {

        this.userMapper = userMapper;
        this.productMapper = productMapper;
        this.orderMapper = orderMapper;
    }
}
```

不要被三个参数吓到。

直接读成：

```
OrderService 依赖三个 Bean：

UserMapper
ProductMapper
OrderMapper

↓
Spring 创建 OrderService 时
把这三个依赖都注入进去
```

这就是典型真实项目代码。

------

## 九、本课最重要的结论

第一：

> **依赖，就是一个对象工作时需要另一个对象。**

第二：

> **依赖注入，就是 Spring 把所需要的 Bean 提供给另一个 Bean。**

第三：

```
@Autowired
```

可以表达：

> Spring，请帮我注入这里需要的 Bean。

第四，更推荐的常见写法是构造器注入：

```
private final UserService userService;

public UserController(UserService userService) {
    this.userService = userService;
}
```

第五，如果只有一个构造器，现代 Spring 通常不要求显式写 `@Autowired`。

最后把第12、13课合起来：

```
@SpringBootApplication
↓
扫描组件
↓
Spring 创建 Bean
↓
放进容器
        ↑
       IoC

Controller 需要 Service
Service 需要 Mapper
↓
Spring 自动组装
        ↑
        DI
```

这样，你已经真正理解了为什么真实 Spring Boot 项目里经常没有：

```
new UserService()
```

却照样可以：

```
userService.getUser(...)
```

因为这个 `UserService` 并不是凭空出现的，而是 **Spring 创建 Bean，再通过依赖注入交给 Controller**。这就是第13课的核心目标。第三阶段：SpringBoot教学计划.mdMD

下一课是 **第14课：常见注解**。会把之前零散见过的：

```
@Service
@Component
@Repository
@Configuration
@Bean
@ComponentScan
```

统一整理一遍，重点不是背定义，而是让你以后看到它们时，能立即判断：**这个类为什么会成为 Bean、它大概承担什么职责。**



# 第13课：常见注解——看到它们时该想到什么

这一课不要求背注解定义，而是把前面已经零散见过的几个常见注解统一整理起来。教学计划明确要求认识：

```
@Service
@Component
@Repository
@Configuration
@Bean
@ComponentScan
```

核心目标不是“会默写”，而是以后看到它们时，知道这个类或方法大概在 Spring 里扮演什么角色。

## 一、先建立总分类

你可以先把这些注解分成两组。

### 1. 标在类上的

```
@Component
@Service
@Repository
@Configuration
```

它们共同的一个核心作用是：

> **让这个类进入 Spring 的管理体系，通常成为 Bean。**

只是它们表达的“职责语义”不同。

### 2. 标在方法上的

```
@Bean
```

它表示：

> **这个方法返回的对象，也交给 Spring 管理。**

另外还有一个：

```
@ComponentScan
```

它和“Spring 去哪里扫描这些组件”有关。

------

## 二、`@Component`：最通用的 Spring 组件

例如：

```
@Component
public class SmsClient {
}
```

可以先理解为：

> 这是一个普通 Spring 组件，请把它交给 Spring 容器管理。

于是：

```
Spring 扫描到 @Component
↓
创建 SmsClient 对象
↓
成为 Bean
↓
放进 Spring 容器
```

`@Component` 是比较通用的。

如果一个类：

- 需要被 Spring 管理
- 但又不明显属于 Controller、Service、Repository 等特定层次

就可能看到 `@Component`。

所以：

```
@Component
≈ “这是一个 Spring 管理的通用组件”
```

------

## 三、`@Service` 和 `@Repository` 是更有语义的组件

### 1. `@Service`

我们已经见过很多次：

```
@Service
public class UserService {
}
```

你现在应该直接想到：

```
这是业务层组件
↓
Spring 会管理它
↓
它通常会被 Controller 注入
```

本质上，它也是一种 Spring 组件。

但相比：

```
@Component
```

写：

```
@Service
```

更能告诉阅读代码的人：

> **这个类负责业务逻辑。**

所以：

```
@Component
→ 泛化标签

@Service
→ 业务层语义更明确
```

------

### 2. `@Repository`

例如：

```
@Repository
public class UserRepository {
}
```

可以理解成：

> **这是数据访问相关组件。**

传统 Spring 项目里常用它标记 DAO / Repository 层。

这里要注意：

你当前课程使用的是：

```
Mapper
```

而 MyBatis 的 Mapper 很多时候并不一定直接写 `@Repository`。

所以不要机械理解成：

> “所有 Mapper 都必须有 `@Repository`。”

不一定。

你只需要先认识：

```
@Repository
→ 数据访问层语义
```

至于 MyBatis Mapper 具体怎么被 Spring 注册，我们放到后面的 MyBatis 阶段。

------

## 四、`@Configuration`：这是一个配置类

例如：

```
@Configuration
public class AppConfig {
}
```

看到它应该想到：

> 这个类主要不是处理用户、订单这些业务，而是在配置 Spring 应用。

它其实和第一课认识的：

```
config
```

目录非常契合。

你可能看到：

```
config
└── AppConfig.java
```

里面：

```
@Configuration
public class AppConfig {
}
```

这就很自然。

------

### 1. 为什么配置还要写 Java 类

之前我们学过：

```
application.yml
```

也是配置。

但两者侧重点不同。

例如：

```
server:
  port: 8081
```

这种：

> 参数型配置

很适合放 YAML。

而有时候你需要告诉 Spring：

> 请按这种方式创建一个对象。

这时候 Java 配置类更合适。

于是就会用：

```
@Configuration
```

配合下面的：

```
@Bean
```

------

## 五、`@Bean`：手动告诉 Spring“这个对象也归你管”

这是这一课最值得理解的一个注解。

前面我们创建 Bean，主要靠：

```
@Service
@Component
@RestController
```

也就是：

```
Spring 扫描类
↓
自动创建对象
```

但有时某个类不是你自己写的，或者你想自己控制它怎么创建。

例如概念上：

```
@Configuration
public class AppConfig {

    @Bean
    public SomeClient someClient() {
        return new SomeClient();
    }
}
```

Spring 看到：

```
@Bean
```

就知道：

> 这个方法返回的对象，也请放进 Spring 容器。

所以：

```
@Bean 方法
↓
执行方法
↓
获得对象
↓
对象成为 Spring Bean
```

### 1. 和 `@Component` 的区别

可以先这样理解：

```
@Component / @Service
→ 在类本身上做标记
→ Spring 扫描这个类

@Bean
→ 我自己写方法创建对象
→ 把返回值交给 Spring
```

例如：

```
@Component
public class A {
}
```

是：

> Spring，你帮我创建 A。

而：

```
@Bean
public A a() {
    return new A(...);
}
```

更像：

> Spring，我告诉你 A 应该怎么创建，创建完以后你来管理。

------

## 六、`@ComponentScan`：Spring 到哪里找这些组件

第3课我们讲过：

```
Spring Boot 启动
↓
扫描组件
```

这一课把对应注解名字补上：

```
@ComponentScan
```

它大致负责：

> **指定 Spring 应该扫描哪些包。**

例如概念上：

```
@ComponentScan("com.example.demo")
```

意思就是：

> 从这个包开始寻找 Spring 组件。

比如：

```
com.example.demo
├── controller
├── service
├── config
└── component
```

Spring 就可以扫描这些下面的类。

不过你可能马上会问：

> 我之前写 Spring Boot 项目明明没看到 `@ComponentScan`？

因为：

```
@SpringBootApplication
```

已经包含了组件扫描相关能力。

所以普通 Spring Boot 项目里，你通常不需要自己再写一个：

```
@ComponentScan
```

这也是为什么我们在第3课说：

> 启动类所在位置会影响默认扫描范围。

------

## 七、把这些注解放回真实项目

假设项目：

```
com.example.demo
├── DemoApplication.java
├── service
│   └── UserService.java
└── config
    └── AppConfig.java
```

启动类：

```
@SpringBootApplication
public class DemoApplication {
}
```

Service：

```
@Service
public class UserService {
}
```

配置：

```
@Configuration
public class AppConfig {

    @Bean
    public SomeClient someClient() {
        return new SomeClient();
    }
}
```

Spring 启动以后可以粗略理解成：

```
@SpringBootApplication
↓
启动组件扫描

发现 @Service
↓
创建 UserService Bean

发现 @Configuration
↓
创建配置类

发现 @Bean 方法
↓
执行 someClient()
↓
把返回对象注册为 Bean
```

最终容器里可能有：

```
UserService Bean
SomeClient Bean
AppConfig Bean
...
```

然后这些 Bean 又可以被依赖注入到其他 Bean 里。

------

## 八、现在把最常见的注解放在一张表里

| 注解                     | 看到它时先想到                             |
| ------------------------ | ------------------------------------------ |
| `@RestController`        | Web Controller，处理 HTTP 请求             |
| `@Service`               | 业务层 Bean                                |
| `@Repository`            | 数据访问层 Bean                            |
| `@Component`             | 通用 Spring Bean                           |
| `@Configuration`         | Spring 配置类                              |
| `@Bean`                  | 把方法返回的对象注册给 Spring              |
| `@ComponentScan`         | 指定组件扫描范围                           |
| `@SpringBootApplication` | Spring Boot 主启动类，并包含扫描等核心能力 |

你不需要去背它们继承了哪些底层注解。

现阶段最重要的是：

> **看到注解，立刻知道这个类为什么被 Spring 管、它大概承担什么职责。**

------

## 九、本课最重要的判断方式

以后打开真实项目，不要看到：

```
@Service
@Component
@Configuration
```

就停下来研究源码。

先按“职责”读：

```
@Service
→ 业务

@Repository
→ 数据访问

@Configuration
→ 配置

@Component
→ 通用组件

@Bean
→ 手动注册一个对象
```

然后再把它和前两课连接起来：

```
这些注解
↓
让对象成为 Bean
↓
Spring 容器管理 Bean
↓
通过依赖注入把 Bean 组装起来
```

至此，第12～14课实际上形成了一整套：

```
扫描
↓
发现组件
↓
创建 Bean
↓
Spring 容器管理
↓
依赖注入
↓
Controller → Service → Mapper 等对象真正连接起来
```

这也就是教学计划“Bean、IoC、依赖注入、常见注解”这几课真正要建立的整体理解。第三阶段：SpringBoot教学计划.mdMD

下一课进入项目规范模块：**第15课：统一返回结果 `Result<T>`**。这会把第7课只是“认识过”的 `Result<T>` 正式提出来，重点解释为什么真实项目通常不希望不同 Controller 各自随意返回不同格式。



# 第14课：统一返回结果 `Result<T>`

前面第7课我们已经见过 `Result<T>`，这一课不再把它当“一个返回值例子”，而是正式理解：

> **为什么真实项目常常要统一接口返回格式，而不是每个 Controller 想返回什么就返回什么。**

这正是教学计划第15课的目标。第三阶段：SpringBoot教学计划.mdMD

## 一、为什么需要统一返回格式

### 1. 不统一会发生什么

假设一个项目有三个接口。

查询用户：

```
@GetMapping("/{id}")
public User getUser(...) {
    return user;
}
```

删除用户：

```
@DeleteMapping("/{id}")
public String delete(...) {
    return "删除成功";
}
```

查询用户列表：

```
@GetMapping
public List<User> list() {
    return users;
}
```

前端收到的东西分别可能是：

```
{
  "id": 1,
  "name": "Zhang"
}
删除成功
[
  {
    "id": 1,
    "name": "Zhang"
  }
]
```

每个接口格式都不一样。

更麻烦的是失败时：

```
{
  "error": "用户不存在"
}
```

另一个接口可能又返回：

```
权限不足
```

这样前端每调用一个接口，都得重新判断：

```
这个接口的数据在哪里？
成功怎么判断？
错误信息在哪里？
```

所以很多项目会约定：

> **所有接口都使用同一种“外壳”。**

------

## 二、`Result<T>` 是什么

例如统一规定：

```
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

Java 可以定义：

```
public class Result<T> {

    private Integer code;
    private String message;
    private T data;

}
```

三个字段可以先这样理解：

```
code
→ 业务处理结果

message
→ 给调用方的说明

data
→ 真正返回的数据
```

其中：

```
<T>
```

就是泛型。

所以一个 `Result` 外壳可以装不同的数据。

### 1. 查询一个用户

```
Result<UserVO>
```

表示：

```
data 是 UserVO
```

最终：

```
{
  "code": 200,
  "message": "success",
  "data": {
    "id": 10,
    "name": "Zhang"
  }
}
```

### 2. 查询用户列表

```
Result<List<UserVO>>
```

最终：

```
{
  "code": 200,
  "message": "success",
  "data": [
    {
      "id": 1,
      "name": "A"
    },
    {
      "id": 2,
      "name": "B"
    }
  ]
}
```

外壳始终没变：

```
code
message
data
```

只有 `data` 的内容变化。

------

## 三、实际项目通常怎么使用

真实代码经常不会每次都手动：

```
new Result<>(200, "success", user)
```

而是在 `Result` 中提供方便的方法。

例如概念上：

```
public static <T> Result<T> success(T data) {
    return new Result<>(200, "success", data);
}
```

以及：

```
public static <T> Result<T> error(
        Integer code,
        String message) {

    return new Result<>(code, message, null);
}
```

于是 Controller 可以写得非常整齐：

```
@GetMapping("/{id}")
public Result<UserVO> getUser(
        @PathVariable Long id) {

    UserVO user = userService.getUser(id);

    return Result.success(user);
}
```

前端收到：

```
{
  "code": 200,
  "message": "success",
  "data": {
    ...
  }
}
```

失败时则可能：

```
return Result.error(10001, "用户不存在");
```

得到：

```
{
  "code": 10001,
  "message": "用户不存在",
  "data": null
}
```

这里具体使用什么 `code` 完全取决于项目规范，不存在所有 Spring Boot 项目统一的业务码。

------

## 四、统一返回有什么实际好处

### 1. 前端处理更统一

前端可以形成固定逻辑：

```
收到响应
↓
看 code
↓
成功
→ 读取 data

失败
→ 显示 message
```

不用每个接口重新猜响应结构。

### 2. 后端接口更容易阅读

比如：

```
public Result<UserVO> getUser(...)
```

你一眼就知道：

```
接口使用统一返回结构
真正的数据类型是 UserVO
```

看到：

```
Result<List<OrderVO>>
```

也能立刻判断：

> 返回的是订单列表，只是外面包了一层统一结果。

### 3. 错误处理容易统一

现在我们可能写：

```
Result.error(...)
```

下一课学习全局异常处理以后，可以进一步变成：

```
Service 抛异常
↓
统一异常处理器捕获
↓
自动生成 Result.error(...)
```

这样 Controller 连错误返回都不需要到处手写。

第15课和第16课其实是紧密相连的。

------

## 五、`Result<T>` 和 HTTP 状态码不要混淆

第7课已经提过，这里再确认一次。

假设：

```
{
  "code": 10001,
  "message": "用户不存在",
  "data": null
}
```

这里：

```
10001
```

通常是：

> 项目自己定义的业务状态码。

它和：

```
HTTP 404
HTTP 400
HTTP 500
```

不是天然一回事。

因此可能存在：

```
HTTP Status: 200

Body:
{
  "code": 10001,
  "message": "用户不存在"
}
```

也可能有项目同时正确使用：

```
HTTP Status: 404
```

并搭配统一业务响应。

不同项目规范会不同。

你现在只要牢牢记住：

```
HTTP 状态码
→ HTTP 协议层

Result.code
→ 项目自己的业务协议
```

不要混为一谈。

------

## 六、为什么“直接 return user”不是错

教学计划这里的重点是“为什么不能直接 `return user`”。第三阶段：SpringBoot教学计划.mdMD

这里依然要准确区分：

```
return user;
```

在 Spring Boot 中**完全可以工作**。

问题不是：

> Spring 不允许。

而是：

> **大型项目通常希望所有 API 的结构统一。**

所以：

```
return user;
```

更像：

```
{
  "id": 10,
  "name": "Zhang"
}
```

而：

```
return Result.success(user);
```

得到统一外壳：

```
{
  "code": 200,
  "message": "success",
  "data": {
    "id": 10,
    "name": "Zhang"
  }
}
```

区别主要是**工程规范**。

------

## 七、把现在的 Controller 再升级一次

前面可能是：

```
@GetMapping("/{id}")
public UserVO getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

现在：

```
@GetMapping("/{id}")
public Result<UserVO> getUser(
        @PathVariable Long id) {

    return Result.success(
        userService.getUser(id)
    );
}
```

于是完整调用链：

```
GET /users/10
↓
Controller
↓
Service
↓
Mapper
↓
数据库
↓
Entity
↓
Service 转 VO
↓
Result<UserVO>
↓
JSON
↓
前端
```

现在前面学过的几乎所有东西已经连起来了。

------

## 八、本课最重要的结论

你只需要真正记住三点。

第一：

> `Result<T>` 是项目自己定义的统一响应外壳，不是 Spring Boot 强制要求的标准类。

第二：

统一返回一般解决：

```
成功怎么判断
错误怎么表达
提示信息放哪里
真正的数据放哪里
```

让所有接口保持一致。

第三：

```
Result<UserVO>
```

不要读成一个很复杂的类型。

直接读成：

> **统一响应，里面真正装的是 UserVO。**

整个结构可以压缩为：

```
Controller
↓
Result<T>
├── code
├── message
└── data: T
↓
JSON
↓
前端
```

这就是第15课的核心。第三阶段：SpringBoot教学计划.mdMD

下一课就是第三阶段最后一课：**第16课：全局异常处理**。

我们会解决最后一个很现实的问题：

> 如果 Service 中几十个地方都会发生“用户不存在”“参数错误”“权限不足”，难道每个 Controller 都要写一堆 `try...catch` 和 `Result.error()` 吗？

答案就是：

```
@ControllerAdvice
@ExceptionHandler
```

把异常统一收口处理。



# 第15课：全局异常处理

这是第三阶段最后一课。教学计划这一课要解决的问题很直接：

> 为什么不能在整个项目里到处写 `try...catch`，以及如何用 `@ControllerAdvice`、`@ExceptionHandler` 做统一异常处理。

## 一、为什么不能每个 Controller 都自己处理异常

假设查询用户：

```
@GetMapping("/{id}")
public Result<UserVO> getUser(@PathVariable Long id) {
    try {
        UserVO user = userService.getUser(id);
        return Result.success(user);
    } catch (Exception e) {
        return Result.error(500, "查询失败");
    }
}
```

新增用户又写：

```
@PostMapping
public Result<UserVO> add(@RequestBody UserCreateDTO dto) {
    try {
        return Result.success(userService.add(dto));
    } catch (Exception e) {
        return Result.error(500, "新增失败");
    }
}
```

删除、修改、订单、商品……全部这么写，项目很快会充满：

```
try
catch
try
catch
try
catch
```

这会带来两个明显问题：

1. Controller 被大量异常处理代码淹没。
2. 同一种异常可能在几十个地方重复处理。

更合理的思路是：

```
业务代码
↓
发生异常
↓
直接抛出去
↓
统一的异常处理器负责处理
```

这样 Controller 可以继续专心负责请求和响应。

------

## 二、异常应该怎么“抛出去”

假设 Service 查询不到用户。

不要简单：

```
return null;
```

而是可以抛出一个异常：

```
throw new RuntimeException("用户不存在");
```

于是调用过程：

```
Controller
↓
Service
↓
发现用户不存在
↓
throw 异常
```

异常会沿着调用链向上传递：

```
Service
↑
Controller
↑
Spring Web
```

如果没人处理，最终通常就会变成一个不够友好的服务器错误响应。

所以我们需要一个统一的地方：

> **专门接住这些异常。**

------

## 三、`@ControllerAdvice` + `@ExceptionHandler`

### 1. `@ControllerAdvice`

可以先理解成：

> **这是一个专门给所有 Controller 做统一处理的类。**

例如：

```
@ControllerAdvice
public class GlobalExceptionHandler {
}
```

看到这种类名：

```
GlobalExceptionHandler
```

以后应该马上想到：

> 全局异常处理。

------

### 2. `@ExceptionHandler`

假设：

```
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public Result<Void> handleRuntimeException(
            RuntimeException e) {

        return Result.error(500, e.getMessage());
    }
}
```

这里：

```
@ExceptionHandler(RuntimeException.class)
```

表示：

> 如果 Controller 调用过程中出现 `RuntimeException`，由这个方法处理。

于是：

```
Service 抛 RuntimeException
↓
Spring 捕获
↓
找到对应 @ExceptionHandler
↓
执行 handleRuntimeException()
↓
返回统一 Result
```

Controller 不需要自己写 `try...catch`。

------

## 四、完整走一遍“用户不存在”

假设请求：

```
GET /users/10
```

Controller：

```
@GetMapping("/{id}")
public Result<UserVO> getUser(@PathVariable Long id) {
    return Result.success(userService.getUser(id));
}
```

Service：

```
public UserVO getUser(Long id) {

    User user = userMapper.selectById(id);

    if (user == null) {
        throw new RuntimeException("用户不存在");
    }

    return convertToVO(user);
}
```

异常处理器：

```
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public Result<Void> handle(RuntimeException e) {
        return Result.error(500, e.getMessage());
    }
}
```

整个过程：

```
GET /users/10
↓
Controller
↓
Service
↓
Mapper
↓
数据库没有这个用户
↓
Service 发现 user == null
↓
throw RuntimeException
↓
异常向上传递
↓
Spring 找到全局异常处理器
↓
@ExceptionHandler
↓
Result.error(...)
↓
JSON
↓
前端
```

最终前端可能收到：

```
{
  "code": 500,
  "message": "用户不存在",
  "data": null
}
```

这里的 `500` 只是示例业务码，真实项目通常会设计更明确的异常和业务码。

------

## 五、真实项目通常不会所有异常都用 `RuntimeException`

更规范的项目可能自己定义：

```
public class BusinessException extends RuntimeException {

    private Integer code;

    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
    }
}
```

业务代码：

```
if (user == null) {
    throw new BusinessException(10001, "用户不存在");
}
```

异常处理器：

```
@ExceptionHandler(BusinessException.class)
public Result<Void> handleBusinessException(
        BusinessException e) {

    return Result.error(
        e.getCode(),
        e.getMessage()
    );
}
```

于是职责变得很清楚：

```
Service
→ 只负责发现“用户不存在”
→ 抛 BusinessException

全局异常处理器
→ 决定异常最终怎么返回给前端
```

这比每个 Service、Controller 自己拼 `Result.error()` 更统一。

------

## 六、`@ControllerAdvice` 和 `Result<T>` 正好是一套

第15课：

```
Result<T>
→ 统一成功和失败响应的格式
```

第16课：

```
@ControllerAdvice
@ExceptionHandler
→ 统一异常处理的位置
```

两者合起来：

```
正常情况
↓
Controller
↓
Result.success(...)

异常情况
↓
throw Exception
↓
GlobalExceptionHandler
↓
Result.error(...)
```

最后无论成功还是失败，前端都可能收到类似：

```
{
  "code": "...",
  "message": "...",
  "data": "..."
}
```

所以第15、16课其实是在一起解决：

> **接口响应如何规范化。**

------

## 七、什么时候仍然可以自己 `try...catch`

“全局异常处理”不代表：

> Spring 项目禁止写 `try...catch`。

如果你能在当前业务中真正处理异常，就可以捕获。

例如：

```
调用某个服务失败
↓
可以自动重试
```

或者：

```
某个非关键操作失败
↓
记录日志
↓
继续主流程
```

这种情况下 `try...catch` 是合理的。

不推荐的是：

> 每个 Controller 都机械地 `try...catch`，只是为了把异常转换成响应。

这种重复工作更适合全局处理。

------

## 八、第三阶段的完整调用链终于完成了

教学计划要求最后画出完整调用链。

现在我们可以把这16课全部压缩到一张图里：

```
前端 / 浏览器
↓
HTTP Request
↓
Controller
  ├─ @GetMapping / @PostMapping ...
  ├─ @PathVariable
  ├─ @RequestParam
  └─ @RequestBody
↓
Service
  └─ 业务逻辑
↓
Mapper
  └─ 数据访问
↓
MySQL
↓
Mapper
↓
Entity
↓
Service
  ├─ 业务处理
  └─ Entity → VO
↓
Controller
↓
Result<T>
↓
JSON
↓
HTTP Response
↓
前端
```

如果过程中发生异常：

```
Controller / Service / Mapper
↓
抛异常
↓
@ControllerAdvice
↓
@ExceptionHandler
↓
Result.error(...)
↓
前端
```

------

## 九、第三阶段你现在应该具备的能力

根据这份教学计划，第三阶段结束目标包括：能阅读小型 Spring Boot 项目、理解目录职责、追踪请求调用链、编写简单 CRUD，并看懂 Codex 修改的大部分基础 Spring Boot 代码。第三阶段：SpringBoot教学计划.mdMD

现在你至少已经建立了这套阅读方式：

```
先找 Controller
↓
看接口路径和参数
↓
追 Service
↓
看业务逻辑
↓
追 Mapper
↓
判断数据库操作
↓
看 Entity / DTO / VO 的数据流
↓
看 Result 返回
↓
遇到异常再看全局异常处理
```

以及 Spring 本身的另一条主线：

```
@SpringBootApplication
↓
扫描
↓
创建 Bean
↓
Spring 容器
↓
IoC
↓
依赖注入
↓
Controller / Service / Mapper 被组装起来
```

这两条线——**请求调用链**和**Spring 对象管理链**——就是这一阶段最值得带走的核心。

第三阶段到这里正式结束。下一阶段进入数据库后，我们主要不会推翻这套结构，而是把现在还比较抽象的：

```
Mapper
↓
MySQL
```

真正展开成数据库设计、SQL、MyBatis，以及 Java 对象如何和数据库数据完成映射。