# 第 1 课：浏览器如何找到你的程序

## 一、先建立最重要的一张图

```text
客户端
  │
  │ 发出请求
  ↓
服务器
  │
  │ 处理请求
  ↓
返回结果
```

比如：

```text
Chrome 浏览器
       │
       │ “我要查询用户 1”
       ↓
Spring Boot 后端
       │
       │ 查到用户信息
       ↓
返回用户数据
```

这就是 Web 开发最核心的事情之一。

再复杂的代码，本质上很多事情都是：

**谁在发请求？请求发给谁？服务器收到以后做什么？最后返回什么？**

------



## 二、什么是客户端 Client？

可以把客户端理解成：主动向服务器提出要求的**程序**

常见客户端有：

```text
Chrome
Edge
Firefox
手机 App
Postman
前端网页中的 JavaScript
```

**Postman** 本质上就是一个专门帮程序员：**“向后端发送 HTTP 请求” 的客户端**

------



## 三、什么是服务器 Server？

服务器 Server 负责：

- 等待客户端请求
- 接收请求
- 处理请求
- 返回结果

 Server 既可以**指机器**，也可以**指提供服务的软件**

客户端和服务器完全可以运行在同一台电脑上

------



## 四、127.0.0.1 和 localhost

`127.0.0.1` 是 IPv4 的回环地址，`localhost` 是它的名字

现阶段可以这样理解：

```text
localhost
≈
127.0.0.1
≈
我自己这台电脑
```

所以很多时候：

```text
http://localhost:8080
```

和：

```text
http://127.0.0.1:8080
```

访问的是同一个本机服务

------



## 五、端口 Port

端口就是用来进一步区分：**这台电脑上的哪个网络服务**

```text
IP
↓
找到哪台电脑

端口
↓
找到这台电脑上的哪个服务
```

------

### 端口号

开发中常见的情况：

```text
Spring Boot Web 项目
常见：8080

MySQL
默认：3306

Redis
默认：6379
```

所以假设电脑上同时运行：

```text
Spring Boot → 8080
MySQL → 3306
```

```text
localhost:8080
```

大致意思是：找我自己电脑上，监听 8080 端口的那个服务

```text
localhost:3306
```

大致意思是：我自己电脑上，监听 3306 端口的服务

------



## 六、URL

```
http://localhost:8080/login/users?id=1
```

这整个东西通常叫：**URL**

URL 可以理解成：**告诉客户端 “去哪里访问什么资源” 的地址**

我们拆开来看：

```text
http://localhost:8080/login/users?id=1
│       │         │      │         │
└────────────────────────────────────── 协议：使用 HTTP 这种规则进行通信
        └────────────────────────────── 主机：访问我自己这台电脑
                  └──────────────────── 端口：找这台电脑上使用 8080 端口的服务
                         └───────────── 路径：具体要访问服务器提供的某个资源或功能
                                   └─── 查询参数：在访问某个路径时，额外带给服务器的一些信息
       

```

------



# 第2课：HTTP 到底是什么

```
客户端
  │
  │ HTTP Request
  ↓
服务器
  │
  │ HTTP Response
  ↓
客户端
```

## 一、HTTP 请求 Request

请求就是：**客户端告诉服务器：我想做什么**

浏览器访问：

```
http://localhost:8080/users/1
```

它可能发送：

```
GET /users/1
——— ————————
 └─────│────────────── 表示：我要 进行什么操作（获取数据）
       └────────────── 表示：我要 访问服务器的哪个资源
```

------

### HTTP 请求的内容：

```
请求
 |
 |-- 方法
 |
 |-- 地址
 |
 |-- Header
 |
 |-- Body
```

#### 1. 请求头 Header

请求头：客户端附带的一些说明信息

比如：

```
User-Agent: Chrome
```

意思：“我是 Chrome 浏览器”

再比如：

```
Content-Type: application/json
```

意思：“我发送的数据格式是 JSON”

------

#### 2. 请求体 Body

请求体：真正携带的数据

例如注册账号：

```json
{
    "username":"zhanghao",
    "password":"123456"
}
```

------

但是注意：**GET 请求通常没有 Body**

查询用户：

```
GET /users/1
```

只需要告诉服务器：我要哪个用户

------



## 二、HTTP 响应 Response

请求发送出去以后，服务器处理

然后返回：HTTP 响应 Response

------

### HTTP 响应的内容

```
响应
 |
 |-- 状态码
 |
 |-- Header
 |
 |-- Body
```

#### 1. 状态码 Status Code

它告诉客户端：“你的请求结果怎么样”

例如：

- 200 表示：成功
- 404 表示：找不到
- 500 表示：服务器自己出问题

------

#### 2. 响应体 Body

响应体：是服务器真正返回的数据

- 浏览器拿到以后，可以显示网页

- 前端程序可以读取数据

- App 可以展示内容

------



# 第3课：HTTP 请求方式

## 一、 HTTP 请求方法

**URL** 是在说：我要操作谁？

**请求方法**是在说：我要对它做什么？

```
请求方法 + URL = 对某个资源执行某种操作
```

比如：

```
GET /users/1
↑      ↑
做什么  操作谁
```

五种请求方法的对应关系：

| 操作     | HTTP 方法 |
| -------- | --------- |
| 查询     | GET       |
| 创建     | POST      |
| 修改     | PUT       |
| 删除     | DELETE    |
| 局部修改 | PATCH     |

------



## 二、GET：查询

GET 最常见的用途就是：**获取数据**

例如：

```
GET /users/1
```

可以理解成：给我 1 号用户的信息

GET 的重点是：**表达的是 “获取” 而不是 “修改”**

------



## 三、POST：创建

POST 最常见的用途是：**创建新的数据**

例如：

```
POST /users
```

请求体：

```json
{
  "name": "Tom",
  "age": 20
}
```

可以理解成：根据这些信息创建一个新用户

这里注意：

```
POST /users
```

通常不是：操作某个已经存在的具体用户

而是：在 users 这类资源里，新建一个

------



## 四、PUT：修改

PUT 常见用途：**修改已有数据**

比如要修改 1 号用户：

```
PUT /users/1
```

请求体 Body：

```json
{
  "name": "Jerry",
  "age": 21
}
```

表达的是：修改 1 号用户的信息

------



## 五、DELETE：删除

DELETE 非常直观：**删除资源**

例如：

```
DELETE /users/1
```

意思：删除 1 号用户

------



## 六、PATCH：简单认识

PATCH 也可以用于：**局部修改数据**

它和 PUT 的区别：

- PUT：倾向于完整更新
- PATCH：倾向于**局部更新**

例如：

```json
{
  "name": "Tom",
  "age": 20,
  "email": "a@test.com"
}
```

如果只是修改：`email`

PATCH 可以表达：

```
PATCH /users/1
```

请求体 Body：

```json
{
  "email": "b@test.com"
}
```

意思：只改 `email` 这一部分

------



## 七、和数据库 CRUD 的关系

`CRUD` 分别是：

- Create 增
- Read 查
- Update 改
- Delete 删

和 `HTTP` 请求方式对应：

- Create   → POST
- Read     → GET
- Update   → PUT / PATCH
- Delete   → DELETE

------

# 第4课：请求参数放在哪里

##  一、核心问题：参数为什么要放不同位置？

**同样都是客户端传给服务器的数据，但它们可以放在 HTTP 请求的不同位置。**

假设客户端要告诉服务器一些信息

```
例如：“我要查询 1 号用户”
这里至少有一个数据：1
```

```
例如：“我要查询第 2 页，每页 10 条用户数据”
这里有两个数据：page = 2，size = 10
```

这些都叫：**请求参数 / 请求数据**，它们不一定全在同一个地方，不同位置适合表达不同类型的信息

```
Path
→ 哪个资源
→ @PathVariable

Query
→ 查询条件 / 附加条件
→ @RequestParam

Body
→ 一整组业务数据
→ @RequestBody

Header
→ 请求附加信息
→ @RequestHeader
```

------



## 二、路径参数 Path Variable

先看一个非常常见的请求：

```
GET /users/1
```

这里：

```
/users/1
       ↑
```

这个 `1` 就可以理解为：**路径参数**，它直接写在 URL 路径里面

表达的是：“我要访问 users 资源里面的 1 号用户”

------

### 1. 为什么把 id 写进路径？

因为这个 `1` 本身就在表示：**你要操作哪个具体资源**

```
GET /users/1
GET /users/2
GET /users/3
```

意思分别是：

> 查询 1 号用户
> 查询 2 号用户
> 查询 3 号用户

所以路径参数很适合表达：**资源的身份**

```
GET /articles/10
```

→ 查询 10 号文章

```
DELETE /orders/1001
```

→ 删除 1001 号订单

```
PUT /users/5
```

→ 修改 5 号用户

------

### 2. Spring Boot 怎么接这个值？

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
}
```

浏览器请求：

```
GET /users/1
```

Spring Boot 看到：

```
/users/{id}
```

这里：`{id}` 是一个位置占位符

现在可以先把：**`@PathVariable`** 理解成：**从 URL 路径里取值**

------



## 三、Query 参数：? 后面的参数

另一种形式：

```
GET /users?page=2
```

这里：`?page=2` 就是 Query 参数

如果有多个：

```
GET /users?page=2&size=10
```

就是：`page = 2，size = 10`

------

### 1. Query 参数一般用来表达什么？

它更适合这种“附加条件”：

- 第几页；
- 每页多少条；
- 按什么字段排序；
- 搜索关键词；
- 是否筛选某个条件。

比如：

```
GET /articles?page=2&size=10
```

可以理解为：给我文章列表，不过我要第 2 页，每页 10 条

再比如：

```
GET /products?keyword=phone
```

意思：查询商品，并且关键词是 phone

所以 Query 参数通常不是在表达：“到底是哪一个资源？”

而更像是在表达：**怎么查？有什么额外条件？**

------

### 2. Path 和 Query 都在 URL 里，有什么区别？

```
/users/1
```

```
/users?id=1
```

都能表达：查询 1 号用户，但常见规范更倾向于：

```
/users/1
```

表示：一个确定的用户资源

而 Query 更适合：

```
/users?name=Tom
```

表示：**根据条件查询**

```
路径参数 → “哪个资源？”
Query 参数 → “以什么条件查询？”
```

------

### 3. Spring Boot 怎么接 Query 参数？

```java
@GetMapping("/users")
public List<User> getUsers(
    @RequestParam Integer page
) {
}
```

客户端请求：

```
GET /users?page=2
```

于是：

```
?page=2
   ↓
@RequestParam page
   ↓
page = 2
```

所以 **`@RequestParam`** 现在可以理解成：**从 URL 的 Query 参数中取值**

------



## 四、Body：真正承载大量数据的地方

现在来看一个完全不同的场景：注册用户

可能需要提交大量数据，如果全写 URL：

```
/users?username=Tom&password=123456&email=...
```

不仅很乱，而且很多情况下根本不合适

所以 HTTP 请求还有一个专门放数据的地方：**Body 请求体**

比如：

```
POST /users
```

Body：

```json
{
  "username": "Tom",
  "password": "123456",
  "age": 20
}
```

------

### 1. Body 和 Query 最大的直觉区别

Query：适合少量、简单的查询条件

Body：适合提交一整组结构化数据

比如：

```
GET /users?page=2&size=10
```

Query 很合适

而：

```
POST /users
```

Body：

```json
{
  "name": "Tom",
  "age": 20,
  "email": "tom@test.com"
}
```

Body 更合适

------

### 2. Spring Boot 怎么接 Body？

```java
@PostMapping("/users")
public void createUser(
    @RequestBody UserDTO dto
) {
}
```

客户端发：

```json
{
  "name": "Tom",
  "age": 20
}
```

Spring Boot 会把 JSON 数据转成 Java 对象：

```
JSON Body
   ↓
@RequestBody
   ↓
UserDTO
```

所以：**`@RequestBody`** 可以先理解成：**从 HTTP 请求体中读取数据**

------



## 五、Header：请求的“附加说明”

Header 中：

```
Authorization: Bearer abcdefg
```

表示：客户端携带身份认证信息

再比如：

```
Content-Type: application/json
```

表示：请求体里的数据是 JSON

所以 Header 是：**这次请求的元信息 / 附加信息**，而不是业务数据本身

------

### Spring Boot 怎么接 Header？

```java
@RequestHeader("Authorization") String token
```

现在只需要理解：

```
HTTP Header
    ↓
@RequestHeader
    ↓
Java 参数
```

------



## 六、把四种位置放在一起比较

| 数据位置  | 常见用途             | 示例            | Spring Boot 常见对应 |
| --------- | -------------------- | --------------- | -------------------- |
| Path 路径 | 指定具体资源         | `/users/1`      | **`@PathVariable`**  |
| Query     | 查询条件、分页、筛选 | `?page=2`       | **`@RequestParam`**  |
| Body      | 提交结构化数据       | JSON            | **`@RequestBody`**   |
| Header    | 附加信息、认证、格式 | `Authorization` | **`@RequestHeader`** |

------



## 七、用一个真实请求完整分析

```json
PUT /users/17?notify=true

Authorization: Bearer abc123
Content-Type: application/json

{
  "name": "Tom",
  "age": 21
}
```

------

### 1. PUT

表示：修改

------

### 2. /users/17

`17` 是路径参数，表示：修改 17 号用户

------

### 3. ?notify=true

这是 Query 参数，表示：额外告诉服务器，修改后是否通知用户

------

### 4. Authorization

这是 Header，表示：身份认证信息

------

### 5. Content-Type

也是 Header，告诉服务器：Body 是 JSON

------

### 6. JSON

```json
{
  "name": "Tom",
  "age": 21
}
```

是 Body，表示：真正要修改的用户数据

------

### 对应到 Spring Boot

```java
@PutMapping("/users/{id}")
public void updateUser(
    @PathVariable Long id,
    @RequestParam Boolean notify,
    @RequestHeader("Authorization") String token,
    @RequestBody UserDTO dto
) {
}
```

```
/users/17
    ↓
@PathVariable id

?notify=true
    ↓
@RequestParam notify

Authorization
    ↓
@RequestHeader token

JSON Body
    ↓
@RequestBody dto
```

**这些注解只是告诉 Spring Boot：“去 HTTP 请求的哪个位置拿数据**

------



# 第5课：JSON

------

## 一、为什么前端不能直接把 Java 对象发给后端？

假设 Spring Boot 里有一个 Java 对象：

```java
User user = new User();
user.setName("Tom");
user.setAge(20);
```

但前端可能不是 Java 写的，它可能是：

```
Vue
React
JavaScript
手机 App
Python 程序
```

这些程序都不认识 Java 里的 `User` 这个类

也就是说：Java 对象只存在于 Java 程序自己的世界里

如果前端要和后端交流，就需要一种：**双方都能认识的数据格式**

JSON 就承担了这个角色

------



## 二、JSON 是什么？

JSON 全称是：**`JavaScript Object Notation`**，是一种**用文本表示结构化数据**的格式

比如一个用户：

```json
{
  "name": "Tom",
  "age": 20
}
```

这个东西不是 Java 对象，也不是 JavaScript 对象本身，

它只是一段符合 JSON 规则的文本，不同语言都能读懂它

```
前端程序
   ↓
把数据转成 JSON
   ↓
通过 HTTP 发送
   ↓
Spring Boot
   ↓
把 JSON 转成 Java 对象
```

反过来也是一样：

```
Spring Boot
   ↓
Java 对象
   ↓
转成 JSON
   ↓
HTTP Response
   ↓
前端读取
```

这就是 JSON 在 Web 开发里的核心价值

------



## 三、JSON 对象长什么样？

最常见的是：

```json
{
  "name": "Tom",
  "age": 20
}
```

你可以把它理解成：一组 `键 : 值`（键值对）

比如：

```
"name": "Tom"
```

name 是键，Tom 是值

------



## 四、JSON 数组

如果不是一个用户，而是很多用户：

```json
[
  {
    "id": 1,
    "name": "Tom"
  },
  {
    "id": 2,
    "name": "Jerry"
  }
]
```

外面的：`[ ]` 表示数组，里面有多个 JSON 对象

HTTP 请求：

```
GET /users
```

服务器返回：

```json
[
  {
    "id": 1,
    "name": "Tom"
  },
  {
    "id": 2,
    "name": "Jerry"
  }
]
```

这就是：用户列表

------



## 五、嵌套 JSON

JSON 里面还能继续放对象。

比如一个订单：

```json
{
  "id": 1001,
  "user": {
    "id": 1,
    "name": "Tom"
  }
}
```

这里：user 对应的值不是普通字符串，而是另一个 JSON 对象

还可以嵌套数组：

```json
{
  "id": 1001,
  "items": [
    {
      "name": "Keyboard",
      "price": 299
    },
    {
      "name": "Mouse",
      "price": 99
    }
  ]
}
```

所以 JSON 能表达很复杂的数据结构

------



## 六、序列化与反序列化

### Java 对象怎么变成 JSON？

假设 Spring Boot 里有 `User user 对象`，里面的数据是：

```java
id = 1
name = Tom
age = 20
```

Spring Boot 最后可能返回：

```json
{
  "id": 1,
  "name": "Tom",
  "age": 20
}
```

这个过程叫：**序列化 Serialization**

```
Java对象
   ↓
序列化
   ↓
JSON
```

------

### JSON 怎么变成 Java 对象？

客户端发送：

```json
{
  "name": "Tom",
  "age": 20
}
```

Spring Boot 可以把它变成：**`UserDTO dto`**，里面：

```
name = Tom
age = 20
```

这个过程叫：**反序列化 Deserialization**

```
JSON
   ↓
反序列化
   ↓
Java对象
```

------



## 七、和 `@RequestBody` 的关系

客户端发送：

```
POST /users
Content-Type: application/json
```

Body：

```json
{
  "name": "Tom",
  "age": 20
}
```

Spring Boot 里：

```java
@PostMapping("/users")
public void createUser(
    @RequestBody UserDTO dto
) {
}
```

这里发生的事情，可以这样理解：

```
HTTP Body 里的 JSON
        ↓
@RequestBody
        ↓
Spring Boot 读取 Body
        ↓
把 JSON 反序列化
        ↓
UserDTO 对象
```

所以：**`@RequestBody`**，告诉 Spring Boot：“这个参数的数据，请从 HTTP 请求体里读取”

然后 Spring Boot 再把 JSON 转成 Java 对象

以后看到：

```
@RequestBody LoginDTO dto
```

应该立刻想到：请求 Body 里应该有 JSON，Spring Boot 会把 JSON 转成 `LoginDTO`

------



## 八、Spring Boot 负责转换

Spring Boot 已经集成了 JSON 转换能力，通常会借助 JSON 转换库，比如 Jackson

所以写：

```
@RequestBody UserDTO dto
```

它就可以尝试把：

```json
{
  "name": "Tom",
  "age": 20
}
```

转换成：

```
UserDTO
```

------



## 九、字段名要对应

**JSON 字段名和 Java 对象属性名一致时，转换最自然**

Java 类：

```java
class UserDTO {
    String name;
    Integer age;
}
```

JSON：

```json
{
  "name": "Tom",
  "age": 20
}
```

Spring Boot 很容易对应：

```
"name"
   ↓
name

"age"
   ↓
age
```

所以最终：

```
dto.name = Tom
dto.age = 20
```

如果 JSON 写成：

```
{
  "username": "Tom",
  "userAge": 20
}
```

而 Java 对象却只有：

```
name
age
```

那就不能简单地一一对应了

------

# 第6课：HTTP 状态码

## 一、状态码到底是干什么的？

如果服务器什么数据都不返回，那是：

- 成功但没有内容？
- 还是服务器出错了？

所以 HTTP 需要用一个数字快速告诉客户端，这次请求结果属于哪一类

这个数字就是：**HTTP Status Code，HTTP 状态码**

所以一个完整的响应，可以简单理解成：

```
HTTP Response
├─ 状态码
├─ Header
└─ Body
```

状态码负责告诉客户端：“这次请求总体上是什么结果”

Body 再负责：“具体返回了什么数据或错误信息”

------

### 状态码分类

```
2xx ：请求成功

4xx ：客户端这边的请求本身存在问题

5xx ：服务器处理过程中出了问题
```

------

### 常见状态码

| 状态码 | 直觉理解             |
| ------ | -------------------- |
| 200    | 成功                 |
| 201    | 创建成功             |
| 204    | 成功，但没有内容返回 |
| 400    | 请求有问题           |
| 401    | 没有通过身份认证     |
| 403    | 已认证，但没有权限   |
| 404    | 找不到               |
| 500    | 服务器内部错误       |

------



## 二、2xx:请求成功

### 200：最普通的成功

```
200 OK
```

意思就是：请求成功，服务器正常处理了

调接口时看到：

```
Status: 200
```

说明这是个好事儿啊

------

### 201：创建成功

```
201 Created
```

它常用于：**创建资源成功**

比如当服务器成功创建了新用户，这时比单纯返回 `200` 更准确的状态码是 `201`

它不仅告诉你：成功了，还进一步说明：成功创建了一个新资源

------

### 204：成功，但没有内容返回

```
204 No Content
```

意思：请求成功，但服务器没有 Response Body 要返回

比如删除用户：

```
DELETE /users/1
```

服务器成功删除，可能返回：

```
204 No Content
```

并且 Body 为空

------



## 三、4xx:客户端存在问题

### 400：你的请求有问题

```
400 Bad Request
```

它通常表示：**客户端发来的请求不符合服务器要求**

比如注册用户时，服务器要求：

```
name 必须填写
age 必须是数字
```

客户端却发：

```
{
  "name": "",
  "age": "abc"
}
```

服务器可能就返回：

```
400 Bad Request
```

这类问题可能包括：

- 参数格式错误；
- 缺少必要参数；
- JSON 格式不对；
- 参数校验失败。

------

### 401：你还没证明你是谁

```
401 Unauthorized
```

可以把它理解成：**没有通过身份认证**

比如访问：

```
GET /profile
```

这个接口要求登录，但你没有携带有效登录信息，服务器就可能返回 `401`

意思：“我还不能确认你是谁，所以不能让你访问”

------

### 403：我知道你是谁，但你没权限

```
403 Forbidden
```

意思：“身份没问题，但权限不够”

和 `401` 的区别：

- `401`：你是谁我都还没确认
- `403`：我知道你是谁，但你没有权限

------

## 404：找不到

```
404 Not Found
```

它表示：**请求的资源或路径没有找到**

例如访问：

```
GET /abcxyz
```

但 Spring Boot 根本没有这个接口，返回 `404`

再比如：

```
GET /users/999
```

但 999 号用户不存在，有些接口设计也会返回 `404`

------



## 四、5xx

### 500：服务器自己存在问题

```
500 Internal Server Error
```

它表示：服务器处理请求时发生了未正常处理的错误

比如代码里出现异常，或者某段程序逻辑崩了，这时服务器可能返回 `500`

------



## 五、状态码和 Body 的关系

```
状态码
↓
告诉你“结果属于哪一类”

Body
↓
告诉你“具体结果是什么”
```

------



## 六、调 Spring Boot 接口时，状态码非常有用

状态码是以后定位接口问题时非常实用的第一信号

假设在 Postman 里请求一个接口，结果没成功，第一时间就可以先看状态码:

如果看到 `400`，优先怀疑：请求参数是不是有问题？JSON 格式对不对？有没有缺字段？

如果看到 `401`，优先怀疑：是否没登录？Token 是否没传或失效？

如果看到 `403`，优先怀疑：当前账号是不是没有权限？

如果看到 `404`，优先怀疑：URL 路径是不是写错了？Controller 有没有这个接口？

如果看到 `500`，优先怀疑：后端代码是不是发生异常了？

------

# 第7课：RESTful 接口设计

## 一、RESTful 最核心的想法：围绕“资源”设计接口

**接口设计时，RESTful 更倾向：**

- URL 描述 “你要操作什么资源”
- 请求方法描述 “你要做什么”

比如：

```
GET /users/1
```

这里：

- `/users/1`：表示“1 号用户”这个资源
- `GET`：表示“查询”

所以整体意思就是：查询 1 号用户

------

### 为什么 `/users` 比 `/getUsers` 更 RESTful？

两种写法：

- /getUsers
- /users

第一种：`/getUsers`，URL 本身就在表达动作：获取用户

第二种：`/users`，URL 只表达：用户资源

至于具体要做什么，交给 HTTP 方法表达

RESTful 更倾向于：

- URL 表示资源
- HTTP 方法表示动作

而不是把动作全塞进 URL

------



## 二、一个用户系统怎么设计？

### 1. 查询全部用户

```
GET /users
```

含义：查询用户列表

------

### 2. 查询某个用户

```
GET /users/1
```

含义：查询 1 号用户

------

### 3. 创建用户

```
POST /users
```

Body：

```json
{
  "name": "Tom",
  "age": 20
}
```

含义：创建一个用户

------

### 4. 修改用户

```
PUT /users/1
```

Body：

```json
{
  "name": "Jerry",
  "age": 21
}
```

含义：修改 1 号用户

------

### 5. 删除用户

```
DELETE /users/1
```

含义：删除 1 号用户

------

把它们放一起：

```
GET    /users
GET    /users/1
POST   /users
PUT    /users/1
DELETE /users/1
```

URL 很稳定，变化主要体现在 HTTP 方法

------



## 三、为什么这种设计对 Spring Boot 很重要？

Controller 很可能长这样：

```java
@GetMapping("/users/{id}")
@PostMapping("/users")
@PutMapping("/users/{id}")
@DeleteMapping("/users/{id}")
```

```
/users/{id}
```

是在描述资源，而：

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
```

是在描述操作

------



## 四、Path 参数和 Query 参数，在 RESTful 里也有不同角色

```
GET /users/1
```

这里 `1` 是 Path 参数，它表示：哪一个具体用户

```
GET /users?page=2&size=10
```

这里 `page=2, size=10` 是 Query 参数，它表示：查询用户列表时，要怎么查？

所以很常见的思路是：

```
Path → 定位资源

Query → 查询条件 / 筛选条件
```

------



## 五、状态码也属于接口设计的一部分

RESTful 并不是只管 URL，一个接口返回什么状态码，也应该尽量符合语义

### 查询成功

```
GET /users/1
```

返回：

```
200 OK
```

------

### 创建成功

```
POST /users
```

返回：

```
201 Created
```

------

### 删除成功，没有内容

```
DELETE /users/1
```

返回：

```
204 No Content
```

------

### 用户不存在

```
GET /users/999
```

返回：

```
404 Not Found
```

请求方法、URL、参数、状态码，其实都在共同表达接口语义

------



## 六、RESTful 不是“法律”，而是一套设计风格

很多真实项目并不完全严格遵守 RESTful

例如：

```
/api/user/login
/api/user/register
```

这些 URL 里明显有：

```
login
register
```

严格按 RESTful 思路来说，不算特别“纯”，但现实项目里非常常见。

RESTful 是：**一种推荐的接口设计风格**，不是说偏离它接口就不能用。

------

### 那 `login`、`register` 为什么经常保留？

因为有些操作很难自然地套进最简单的 CRUD，比如：

```
登录
注册
重置密码
上传文件
发送验证码
```

它们更像业务动作，所以现实项目常写：

```
POST /api/user/login
POST /api/user/register
POST /api/password/reset
```

现阶段不用追求“绝对 RESTful”，更重要的是：**看懂团队采用的接口风格，并保持一致**
