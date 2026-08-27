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

## 一、先看 Spring Boot 项目的启动类

### 1. 它通常长这样

一个典型的 Spring Boot 启动类：

```
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

如果你第一次看到这段代码，最容易产生的感觉是：

> 就这几行？

对。

一个 Spring Boot 项目的启动入口，通常真的就是这么短。

这几行代码看起来简单，但背后发生了很多事情。

------

### 2. 先不要把它当 Spring Boot，看成普通 Java

你已经学过 Java 基础，所以先只看：

```
public static void main(String[] args) {
}
```

这个东西你应该认识。

它就是：

> **Java 程序的入口。**

也就是说，当你运行：

```
DemoApplication
```

Java 首先执行的是：

```
main()
```

这一点和你以前写过的：

```
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

没有本质区别。

所以第一步非常简单：

```
点击运行 DemoApplication
↓
JVM 找到 main()
↓
开始执行 main()
```

------

## 二、真正关键的是 `SpringApplication.run()`

### 1. `main()` 自己并没有把网站启动起来

看：

```
public static void main(String[] args) {
    SpringApplication.run(DemoApplication.class, args);
}
```

真正特别的是这一句：

```
SpringApplication.run(...)
```

可以先把它翻译成人话：

> **Spring Boot，开始启动这个应用。**

因此启动过程可以粗略理解成：

```
运行 DemoApplication
↓
进入 main()
↓
执行 SpringApplication.run(...)
↓
Spring Boot 开始初始化整个应用
```

所以：

> `main()` 是 Java 的入口，`SpringApplication.run()` 才真正触发 Spring Boot 的启动过程。

这是这节课第一个重要区别。

------

### 2. 为什么要传 `DemoApplication.class`？

你看到：

```
SpringApplication.run(DemoApplication.class, args);
```

这里传进去：

```
DemoApplication.class
```

你现在不用研究反射、Class 对象这些底层细节。

先理解成：

> **告诉 Spring Boot：这是我的主启动类，从这里开始组织整个应用。**

所以可以粗略读成：

```
SpringApplication.run(
    当前这个 Spring Boot 主类,
    Java 启动参数
)
```

至于：

```
args
```

就是：

```
main(String[] args)
```

收到的启动参数。

现阶段基本不用管。

------

## 三、`@SpringBootApplication` 是干什么的？

### 1. 这不是普通注释

你看到：

```
@SpringBootApplication
```

前面有一个：

```
@
```

说明它是 Java 注解。

你在 Java 基础里已经知道：

> 注解本身不是普通注释，而是给程序提供额外信息。

这里可以先把：

```
@SpringBootApplication
```

理解成：

> **告诉 Spring Boot：这个类是整个 Spring Boot 应用的主启动类。**

所以：

```
@SpringBootApplication
public class DemoApplication {
}
```

可以翻译成：

> 这是一个 Spring Boot 应用的核心启动类，请按照 Spring Boot 的方式处理它。

------

### 2. `@SpringBootApplication` 背后其实做了很多事

这里先不拆它底层组合了哪些注解。

第三阶段目前不需要你背源码。

只需要知道它大致帮助完成几件非常重要的事：

```
@SpringBootApplication
↓
告诉 Spring Boot：
1. 这是启动入口
2. 要扫描项目中的相关组件
3. 要进行自动配置
```

其中：

```
扫描
自动配置
```

是今天真正要理解的两个概念。

------

## 四、什么叫“扫描”？

### 1. 先回忆第一课的项目结构

假设项目是：

```
com.example.demo
├── DemoApplication.java
├── controller
│   └── UserController.java
├── service
│   └── UserService.java
├── mapper
│   └── UserMapper.java
└── config
    └── WebConfig.java
```

现在问题来了。

Spring Boot 启动的时候怎么知道：

```
UserController
UserService
WebConfig
```

这些类存在？

它必须去找。

这就是：

> **扫描。**

------

### 2. 扫描可以理解成“Spring 在项目里找它需要管理的类”

Spring Boot 启动后，会在一定范围内扫描项目中的类。

例如以后你会看到：

```
@RestController
public class UserController {
}
```

或者：

```
@Service
public class UserService {
}
```

Spring 看到这些类之后，会识别：

> 这个类不是一个普通的无关 Java 类，它需要交给 Spring 管理。

所以可以粗略理解：

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

至于“纳入 Spring 体系”具体是什么意思，会在后面的：

```
Bean
IoC
依赖注入
```

再讲。

现在先建立这个直觉：

> **Spring Boot 启动时会主动“认识”项目中的一些类。**

------

## 五、扫描范围为什么和启动类位置有关？

这是一个很实用的点。

假设：

```
com.example.demo
├── DemoApplication.java
├── controller
├── service
└── mapper
```

启动类：

```
DemoApplication
```

放在：

```
com.example.demo
```

这个比较上层的位置。

Spring Boot 默认会从这个位置往下面扫描。

于是：

```
com.example.demo
├── controller
├── service
└── mapper
```

都比较容易被扫到。

所以实际项目中，启动类经常放在：

> **项目主包的较上层位置。**

这是为什么你经常看到：

```
com.xxx.project
├── ProjectApplication.java
├── controller
├── service
├── mapper
└── entity
```

而不是把：

```
ProjectApplication.java
```

藏到特别深的某个子包里。

------

## 六、如果启动类放错位置，会发生什么？

假设项目变成：

```
com.example.demo
├── controller
│   └── UserController.java
│
└── app
    └── DemoApplication.java
```

如果 Spring Boot 默认从：

```
com.example.demo.app
```

向下扫描，那么：

```
com.example.demo.controller
```

不在它下面。

这就可能导致：

```
UserController
```

没有被扫描到。

最终你可能发现：

```
项目明明启动成功
但访问 /users
却找不到接口
```

这类问题以后真实开发里可能会遇到。

所以现阶段先记：

> **启动类的位置会影响默认扫描范围。**

而最稳妥的常见做法就是：

> 把启动类放在项目主包的根部或较上层。

------

## 七、什么叫“自动配置”？

这个词是 Spring Boot 最核心的思想之一。

但非常容易被讲得过于抽象。

我们先从没有自动配置的世界开始。

### 1. 假设你要开发 Web 项目

你希望：

```
程序启动
↓
拥有 Web 服务器
↓
能够监听 8080 端口
↓
能够接收 HTTP 请求
↓
能够处理 JSON
```

如果全部手动配置，你可能需要自己告诉框架：

```
用什么 Web 服务器？
服务器怎么启动？
监听什么端口？
JSON 用什么工具？
请求怎么映射？
```

Spring Boot 的目标就是：

> **很多常见场景，我根据你的项目情况，先帮你配置一个合理的默认方案。**

这就是自动配置的直觉。

------

### 2. Spring Boot 会根据“你有什么”来推断“你需要什么”

假设你的 `pom.xml` 有：

```
spring-boot-starter-web
```

Spring Boot 启动时发现：

> 这个项目明显是要做 Web 开发。

于是它会帮你准备一系列常见 Web 配置。

你不用自己从零告诉它：

```
请启动 Web 环境
请准备处理 HTTP 请求的能力
请准备 JSON 转换
……
```

Spring Boot 会根据现有依赖和配置，自动完成很多初始化。

所以：

```
依赖
+
配置
+
Spring Boot 的规则
↓
自动推断和配置
↓
应用具备对应能力
```

------

## 八、自动配置不等于“Spring Boot 什么都猜”

这里很重要。

“自动配置”不是：

> Spring Boot 会读心术。

也不是：

> 什么都不需要你写。

更准确的是：

> **Spring Boot 根据已有依赖、已有配置和默认规则，尽量帮你把常见东西配置好。**

例如你已经声明：

```
我要 Web 能力
```

Spring Boot 就帮你准备常见 Web 配置。

但如果你想改变：

```
默认端口 8080
```

你仍然可以在：

```
application.yml
```

写：

```
server:
  port: 8081
```

然后 Spring Boot 就会采用你的配置。

这正好衔接下一课的 `application.yml`。

------

## 九、为什么 Spring Boot 项目能自己启动 Web 服务器？

这是一个非常重要的直觉。

传统思路里，你可能会觉得：

> 网站不是应该先安装一个 Tomcat，然后把项目丢进去吗？

而 Spring Boot 常见的 Web 项目里，Web 服务器通常已经作为项目的一部分被带进来了。

比如常见的：

```
Tomcat
```

可以作为嵌入式服务器跟着应用一起启动。

于是启动过程变成：

```
运行 main()
↓
Spring Boot 启动
↓
初始化 Web 环境
↓
启动内嵌 Web 服务器
↓
服务器监听端口
↓
等待 HTTP 请求
```

所以你不需要：

```
先手动启动 Tomcat
再部署项目
```

而是：

```
运行 Spring Boot 项目
↓
服务器和应用一起起来
```

这正是 Spring Boot 使用体验特别方便的地方之一。

------

## 十、现在重新看这三行代码

回到：

```
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

现在逐行翻译。

### 1. `@SpringBootApplication`

```
这是一个 Spring Boot 应用的主启动类
↓
Spring Boot 会以它为重要起点
↓
进行组件扫描
↓
进行自动配置
```

### 2. `main()`

```
Java 程序入口
```

### 3. `SpringApplication.run()`

```
正式启动 Spring Boot
↓
创建并初始化 Spring 运行环境
↓
扫描组件
↓
执行自动配置
↓
启动 Web 环境
↓
项目开始运行
```

到这里，这段代码就已经不再是“神秘三行”。

------

## 十一、把 Maven 和启动过程连起来

第二课我们学过：

```
pom.xml
↓
Maven
↓
准备项目依赖
```

现在第三课补上：

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

所以：

> Maven 解决“项目拥有什么能力”。

Spring Boot 启动过程解决：

> “这些能力怎么真正运行起来”。

这是第二课和第三课之间非常重要的衔接。

------

## 十二、再把第一课的 Controller 接进来

第一课我们有：

```
浏览器
↓
Controller
↓
Service
↓
Mapper
↓
数据库
```

但以前少了一件事：

> Controller 怎么有机会接请求？

现在可以补全。

```
运行 Application
↓
Spring Boot 启动
↓
扫描到 Controller
↓
启动 Web 服务器
↓
监听端口
↓
浏览器发送 HTTP 请求
↓
请求进入 Controller
↓
Service
↓
Mapper
```

这样三课知识开始真正连起来了。

------

## 十三、Spring Boot 启动时到底发生了什么？

如果以后有人问你：

> Spring Boot 启动流程是什么？

现阶段千万不要回答一大堆底层源码类名。

你现在只需要这个版本：

```
1. Java 执行 main()

2. main() 调用 SpringApplication.run()

3. Spring Boot 初始化应用

4. 扫描需要 Spring 管理的类

5. 根据依赖和配置执行自动配置

6. 如果是 Web 项目，启动内嵌 Web 服务器

7. 项目进入运行状态，等待请求
```

这对当前阶段已经非常够用。

------

## 十四、启动日志其实也可以读

以后你运行 Spring Boot 项目，控制台可能刷很多日志。

你现在不用逐行看。

只关注几个信号。

比如看到类似：

```
Started DemoApplication
```

通常说明：

> Spring Boot 已经成功启动。

如果还看到类似：

```
Tomcat started on port 8080
```

则表示：

> Web 服务器已经启动，并监听 8080 端口。

那么此时浏览器访问：

```
http://localhost:8080
```

才有可能真正进入你的后端程序。

所以以后判断：

> “项目到底启动成功没有？”

不要只看 IDEA 的运行按钮亮没亮。

要看启动日志有没有成功信息，以及有没有异常。

------

## 十五、一个很容易混淆的问题：启动成功 ≠ 接口一定能访问

比如：

```
Started DemoApplication
```

说明：

> Spring Boot 应用启动了。

但访问：

```
/users
```

还是可能 404。

为什么？

可能因为：

```
没有对应 Controller
Controller 没被扫描
接口路径写错
端口访问错
```

所以要分清：

```
项目启动成功
```

和：

```
某个具体接口工作正常
```

是两回事。

这在以后调试时非常重要。

------

## 十六、用一个完整例子串起来

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

你点击运行：

```
DemoApplication.java
```

发生：

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

然后浏览器发送：

```
GET /users
```

才进入：

```
UserController
```

这就是：

> **从“点击运行”到“Controller 能接请求”的完整逻辑。**

------

## 十七、这一课暂时不要深入的东西

你以后可能会看到别人讲 Spring Boot 启动流程时提到：

```
ApplicationContext
Environment
BeanFactory
BeanDefinition
refresh()
事件监听
各种初始化器
各种后置处理器
```

这些不是错。

但现在都不要管。

你的目标是读懂项目，而不是分析 Spring Boot 源码。

本课只需要把握：

```
main
↓
SpringApplication.run
↓
扫描
↓
自动配置
↓
服务器启动
↓
项目运行
```

如果以后真的有必要，我们再把其中某一部分放大。

------

## 十八、本课最重要的六句话

第一：

> `main()` 是 Java 程序入口。

第二：

> `SpringApplication.run()` 会正式触发 Spring Boot 应用的启动过程。

第三：

> `@SpringBootApplication` 表明这是 Spring Boot 主启动类，并参与扫描和自动配置。

第四：

> Spring Boot 会扫描项目中的一些类，并把需要管理的组件识别出来。

第五：

> 自动配置就是 Spring Boot 根据依赖、配置和默认规则，帮你准备常见运行环境。

第六：

> Web 项目启动时，Spring Boot 通常还会启动内嵌 Web 服务器，所以运行一个 `Application.java`，整个后端网站就能够进入可接收请求的状态。

这也就是教学计划要求这一课最终回答的问题：为什么运行 `Application.java`，整个网站就启动了。第三阶段：SpringBoot教学计划.mdMD

## 十九、最后做一个小理解检查

假设：

```
@SpringBootApplication
public class ShopApplication {

    public static void main(String[] args) {
        SpringApplication.run(ShopApplication.class, args);
    }
}
```

你现在应该能把它理解成：

```
运行 ShopApplication
↓
执行 Java 的 main()
↓
调用 SpringApplication.run()
↓
Spring Boot 开始初始化
↓
扫描项目里的 Controller / Service 等组件
↓
根据依赖和配置进行自动配置
↓
如果是 Web 项目，启动 Web 服务器
↓
项目正式进入运行状态
```

如果这条链已经能顺着讲出来，第3课就已经达标了。

下一课是 **第4课：`application.yml`**。

那一课会解决：

> **为什么我不改 Java 代码，只把 `server.port` 从 8080 改成 8081，Spring Boot 的运行方式就真的发生了变化？**