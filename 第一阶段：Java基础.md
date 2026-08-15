# 第 1 课：Java 程序的组成

------

## 一、Java 在 Spring Boot 项目中的作用

Spring Boot 是一个基于 Java 的开发框架。

这些文件：

```text
UserController.java
UserService.java
User.java
LoginRequest.java
```

本质上都是 **Java 程序文件**。

Spring Boot 帮助我们更方便地开发后端项目，但它并没有取代 Java。必须先能读懂 Java 语法，才能理解 Spring Boot 项目为什么这样写。

可以暂时理解为：

```text
Java
  ↓
提供基础语法

Spring Boot
  ↓
利用 Java 语法，帮助我们开发后端项目
```

------



## 二、Java 程序是怎样运行的

先看一个简化过程：

```text
编写 Java 代码
        ↓
Java 编译器进行编译
        ↓
生成 JVM 能理解的文件
        ↓
JVM 执行程序
        ↓
得到运行结果
```

在这个过程中，经常会看到三个名称：

```text
JDK
JRE
JVM
```

我们只讲目前够用的部分。

------

### 1. JVM

JVM 的全称是：

```text
Java Virtual Machine
Java 虚拟机
```

可以把 JVM 理解成：

> **专门负责执行 Java 程序的运行环境。**

Java 代码不能直接交给 Windows 处理，而是先转换成 JVM 能理解的形式，再由 JVM 执行。

简化来看：

```text
Java代码
   ↓
JVM
   ↓
Windows、Linux、macOS
```

这也是 Java 可以**跨平台运行**的重要原因。

同一份 Java 程序，只要不同操作系统上安装了对应的 JVM，就有机会运行。

> **JVM 负责真正执行 Java 程序。**

------

### 2. JRE

JRE 的全称是：

```text
Java Runtime Environment
Java 运行环境
```

可以把它理解成：

> **运行 Java 程序所需要的一整套环境。**

其中最核心的部分就是 JVM。

简化关系：

```text
JRE
├── JVM
└── 运行Java程序需要的其他内容
```

所以：

- JVM 负责执行
- JRE 提供完整的运行环境

------

### 3. JDK

JDK 的全称是：

```text
Java Development Kit
Java 开发工具包
```

可以把它理解成：

> **Java 开发人员使用的一整套工具。**

它不仅能运行 Java 程序，还能帮助我们编译、开发和调试 Java 程序。

简化关系：

------

### 三者关系

不用死记复杂结构，只需要记住下面这张图：

```text
JDK：用来开发Java程序
 │
 ├── 编译工具
 ├── 调试工具
 └── 运行Java程序所需的环境
         │
         └── JVM：真正执行Java程序
```

一句话总结：

```text
JDK 用于开发
JRE 用于运行
JVM 负责执行
```

在现代 JDK 中，实际安装结构可能和旧教程里的传统划分不完全一样，但你现阶段按照这个功能关系理解就足够了。

------



## 三、Java 程序由哪些部分组成

现在来看完整代码：

```java
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello");
    }

}
```

先不要急着理解每一个单词，我们先看整体结构。

```text
一个类 Main
└── 一个方法 main
    └── 一条输出语句
```

也可以画成：

```text
Main 类 {
    main 方法 {
        输出 "Hello"
    }
}
```

------



## 四、第一行：`public class Main`

```java
public class Main {
```

我们把它拆开。

------

### 1. `class`

Java 中的大部分代码都要放在类里面，不能随意散落在文件中。

> `class` 表示定义一个类，类可以容纳方法和其他代码。

------

### 2. `Main`

```java
Main
```

`Main` 是这个类的名字。

完整地看：

```java
class Main
```

意思就是：

> 定义一个名叫 `Main` 的类。

类名通常采用“大驼峰命名法”，也就是每个单词的首字母大写：

```text
Main
User
UserController
OrderService
LoginRequest
```

------



## 五、第二行：`public static void main(String[] args)`

```java
public static void main(String[] args) {
```

> **这是 Java 程序的主入口。**

当 Java 运行这个程序时，会先寻找：

```java
main()
```

然后从 `main()` 方法内部开始执行。

------

### 1. 什么是方法

> **一组可以被执行的代码。**

例如：

```java
public static void main(String[] args) {
    System.out.println("Hello");
}
```

这里：

```java
main
```

就是方法名。

```java
System.out.println("Hello");
```

就是这个方法需要执行的代码。

可以类比成一个任务：

```text
任务名称：main
任务内容：
输出 Hello
```

------

### 2. `main`

`main` 是方法的名字。

但是它**不是普通的方法名**。对于最基础的 Java 程序来说：

> JVM 会把符合规定格式的 `main()` 方法当作**程序入口**

也就是说，程序开始运行时，JVM 会寻找：

```java
public static void main(String[] args)
```

找到以后，从这里开始一行一行执行

------

### 3. 为什么 `main()` 是入口

假设一个类中有多个方法，但 JVM 不知道应该先执行哪一个。

所以 Java 规定了一个统一入口：

```java
public static void main(String[] args)
```

相当于告诉 JVM：

> 程序从这里开始。



会在 Spring Boot 启动类中看到：

```java
public static void main(String[] args) {
    SpringApplication.run(MyApplication.class, args);
}
```

这里的 `main()` 同样是程序入口。

Spring Boot 项目本质上也是一个 Java 程序，因此也需要从 `main()` 开始运行。

------

### 3. `static`

现阶段先把它理解成：

> JVM 不需要先创建 `Main` 类的对象，就可以直接调用 `main()` 方法。

你现在只需要知道：Java 规定程序入口需要这样写：

```java
public static void main(String[] args)
```

暂时不要尝试自己改变这里的格式。

------

### 4. `void`

```java
void main(...)
```

表示 `main()` 执行结束后，不需要返回一个具体结果。

------

### 5. `String[] args`

这是 `main()` 方法接收的参数。

先极简理解：

```text
String[]：一组字符串
args：这组字符串的名字
```

完整意思可以暂时理解为：

> 程序启动时，可以接收一组文字参数，并把它们保存到 `args` 中。

目前不会主动使用它，但 Java 入口方法要求保留这个结构。

所以先把它当成固定格式：

```java
public static void main(String[] args)
```

**可以改成别的名字吗？**

`args` 只是参数名，所以技术上可以写成：

```java
public static void main(String[] values)
```

但是大家习惯写：

```java
args
```

------



## 六、第三行：`System.out.println("Hello");`

### 1. `System`

`System` 是 Java 已经为我们提供好的一个类。

这个类中包含一些与系统运行有关的功能。

------

### 2. `out`

`out` 表示标准输出对象。

简单理解：

> 它代表程序默认使用的输出通道，通常就是 IDEA 下方的**控制台**。

```java
System.out
```

可以暂时理解为：

> 使用 Java 提供的系统输出功能。

------

### 3. `println`

`println` 可以粗略拆成：

```text
print + line
打印 + 换行
```

它的作用是：

> 输出内容，并在输出后**换行**。

例如：

```java
System.out.println("Hello");
System.out.println("Java");
```

结果是：

```text
Hello
Java
```

------

### 4. 点号 `.`

这段代码中有两个点：

```java
System.out.println
      ↑   ↑
```

```java
System.out
```

表示使用 `System` 中的 `out`。

```java
System.out.println
```

表示使用 `out` 中的 `println` 功能。

------



## 七、和 Spring Boot 启动类的关系

会在 Spring Boot 项目中看到类似代码：

```java
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}
```

它的基本骨架：

```text
定义了一个 MyApplication 类
        ↓
类中有一个 main 方法
        ↓
main 方法是程序入口
        ↓
程序从 main 方法中的代码开始执行
```

Spring Boot 项目则通常在入口中写：

```java
SpringApplication.run(MyApplication.class, args);
```

> **Spring Boot 项目也是从 Java 的 `main()` 方法启动的。**

------





# 第 2 课：变量、数据类型与表达式

------

## 一、变量易错点

### 1. 同一个作用域中不能重复定义同名变量

错误示例：

```java
int age = 23;
int age = 24;
```

这里不是修改变量，而是试图再次定义同名变量，因此会报错。

正确写法：

```java
int age = 23;
age = 24;
```

------



## 二、Java 的 8 种基本数据类型

整数：

```text
byte
short
int
long
```

小数：

```
float
double
```

字符：

```
char
```

真假：

```
boolean
```

------



## 三、整数类型

```
byte：8 位
short：16 位
int：32 位
long：64 位
```

### 1. `int`

Java 中：int **永远是 32 位**

无论程序运行在 Windows、Linux，还是其他平台，Java 的 `int` 都是 32 位。

这是 Java 跨平台设计的一部分。

------

### 2. `long`

当整数可能超过 `int` 范围时，可以使用 `long`：

```java
long population = 8_000_000_000L;
```

注意最后的`L`

在 Java 中，整数常量默认被当成 `int`，所以直接写：

```java
long population = 8000000000;
```

会报错，因为 `8000000000` 已经超过 `int` 范围。

正确写法：

```java
long population = 8000000000L;
```

推荐使用大写 `L`，因为小写 `l` 容易和数字 `1` 混淆。

------

### 数字中的下划线

Java 允许在数字中加入下划线，提高可读性：

```java
long population = 8_000_000_000L;
int money = 1_000_000;
```

下划线**不会改变数值**。

```java
1_000_000
```

和：

```java
1000000
```

完全相同。

------



## 四、Java 没有常规无符号整数

C++ 中可以写：

```cpp
unsigned int count;
```

Java 中没有对应的常规写法：

```java
unsigned int count;
```

这是非法的。

Java 的整数类型**默认都是有符号类型**：

```text
byte
short
int
long
```

都可以表示正数和负数。

Java 标准库提供了一些无符号计算工具，但普通 Spring Boot 业务开发中基本不会使用。

所以你可以直接记住：

> Java 业务代码中通常不使用无符号整数。

------



## 五、浮点类型

### 1. `double`

```java
double score = 95.5;
double price = 19.99;
double height = 176.5;
```

Java 中的小数**默认是 `double` 类型**。

`double` 是 64 位浮点数，也是普通小数计算中最常用的类型。

------

### 2. `float`

```java
float score = 95.5F;
或
float score = 95.5f;
```

注意结尾的`F`

因为小数常量 `95.5`，默认是 `double` 类型。

所以这样写会报错：

```java
float score = 95.5;
```

Java 不允许直接把 `double` 放入范围和精度更小的 `float` 中。

------

### 3. 浮点数不能用于精确金额计算

例如：

```java
double result = 0.1 + 0.2;
System.out.println(result);
```

结果可能是：

```text
0.30000000000000004
```

这不是 Java 独有的问题，而是二进制浮点数的通用精度问题。

因此在 Spring Boot 项目中：

```java
double money;
```

通常不适合保存需要精确计算的金额。

金额一般使用 `BigDecimal`，例如：

```java
BigDecimal price;
```

`BigDecimal` 属于后续项目中会接触到的类，本课只需知道：

> `double` 适合一般小数，不适合精确金融计算。

------



## 六、布尔类型 `boolean`

```java
boolean enabled = true;
boolean deleted = false;
```

`boolean` 只有两个值：

```java
true
false
```

不能是 `1` 和 `0`

```text
0 不等于 false
非 0 不等于 true
```

------

### Java 与 C++ 的重要区别

C++ 中，这样是可以的：

```cpp
bool flag = 1;
或
if (1) {
}
```

但是，Java 中不行，错误示例：

```java
boolean flag = 1;
或
if (1) {
}
```

这是 Java 与 C++ 一个非常重要的区别。

------



## 七、字符类型 `char`

```java
char grade = 'A';
char symbol = '中';
```

`char` 使用单引号：

```java
'A'
```

字符串使用双引号：

```java
"A"
```

------

### 单引号和双引号的区别

```java
char letter = 'A';
String text = "A";
```

虽然它们显示出来都像 `A`，但类型不同：

```text
'A'   是 char
"A"   是 String
```

------



## 八、`String` 字符串

```java
String name = "Zhang Hao";
String message = "登录成功";
```

`String` 用来保存一串字符，也就是文本。

注意：

```java
String
```

**首字母必须大写**。

------

### `String` 不是基本数据类型

Java 的 8 种基本类型中**没有 `String`**。

`String` 是一个类。

所以：

```java
String name = "Zhang Hao";
```

从严格意义上说，`name` 保存的是一个 `String` 对象的引用。

------



## 九、Java 使用 `+` 拼接字符串

##### `字符串` +`字符串`：

```java
String firstName = "Zhang";
String lastName = "Hao";

String fullName = firstName + " " + lastName;
```

结果：

```text
Zhang Hao
```



**`字符串` + `数字`：**

```java
int age = 23;
String message = "年龄是：" + age;
```

结果：

```text
年龄是：23
```

------



## 十、变量命名规则

Java 变量名可以包含：

- 英文字母
- 数字
- 下划线 `_`
- 美元符号 `$`

**不能以数字开头**

------

### 变量名推荐使用小驼峰命名法

变量名通常**首字母小写**，**后续单词首字母大写**：

```java
int userAge;
String userName;
double productPrice;
boolean loginSuccess;
```

### 类名推荐使用大驼峰命名法

```java
UserController
UserService
LoginRequest
```

------



## 十一、常量：`final`

如果一个变量**赋值后不允许再次修改**，可以使用：

```java
final int MAX_COUNT = 100;
```

再次赋值会报错：

```java
MAX_COUNT = 200;
```

------

### Java `final` 与 C++ `const`

简单对比：

```cpp
const int maxCount = 100;
```

Java：

```java
final int maxCount = 100;
```

在**基本数据类型上**，两者可以粗略理解为**相近**：

> 赋值后不能再次修改。

但 Java 的 `final` 用在**对象引用上**时，只代表**引用不能改指向**，不代表对象内容一定不可修改。

例如：

```java
final User user = new User();
```

表示：

```java
user = new User();
```

不能再让 `user` 指向另一个对象。

但可能仍然允许：

```java
user.setName("Zhang Hao");
```

这里修改的是对象内部内容，不是修改引用本身。

------

### 常量命名

真正作为常量使用的变量，通常全部大写，单词之间用下划线：

```java
final int MAX_COUNT = 100;
final double PI = 3.14159;
```

------



## 十二、整数除法

```java
int result = 10 / 3;
```

```
结果是：3
而不是：3.333...
```

因为 `10` 和 `3` 都是整数，所以进行整数除法，小数部分直接丢弃。

这与 C++ 一致。

------

### 如何得到小数结果

只要**参与运算的一方是浮点数**：

```java
double result = 10.0 / 3;
```

也可以：

```java
double result = 10 / 3.0;
```

或者强制转换：

```java
double result = (double) 10 / 3;
```

------



## 十三、字符串不能随便用 `==` 比较内容

因为对对象来说，`==` 比较的通常不是字符串内容，而是**两个引用是否指向同一个对象**。

看下面代码：

```java
String name1 = new String("Zhang Hao");
String name2 = new String("Zhang Hao");

System.out.println(name1 == name2);
```

结果：

```text
false
```



**比较字符串内容**应该使用：

```java
name1.equals(name2)
```

结果：

```text
true
```

------



## 十四、自动类型转换

Java 可以在某些类型之间自动转换。

基本原则是：

> 小范围类型可以自动转换为大范围类型。

例如：

```java
int age = 23;
long value = age;
```

`int` 可以自动转成 `long`。

```java
int count = 100;
double value = count;
```

`int` 也可以自动转成 `double`。

------

### 常见自动转换顺序

可以粗略理解为：

```text
byte
  ↓
short
  ↓
int
  ↓
long
  ↓
float
  ↓
double
```

其中 `char` 也可以转换到某些整数类型。

例如：

```java
char letter = 'A';
int code = letter;
```

`code` 得到的是字符对应的数值编码：

```text
65
```

------



## 十五、强制类型转换

大范围类型转成小范围类型时，需要显式强制转换：

```java
double score = 95.8;
int result = (int) score;
```

执行后：

```text
result = 95
```

小数部分**直接被截断**，不会四舍五入。

------

### 大数转小数可能溢出

```java
long value = 3_000_000_000L;
int result = (int) value;
```

`int` 保存不了这么大的数，结果会发生溢出，变成一个错误的负数或其他值。

------



## 十六、算术运算中的类型提升

Java 在进行算术运算时，会对较小的整数类型进行提升。

看下面代码：

```java
byte a = 10;
byte b = 20;

byte result = a + b;
```

这会报错。

原因是：`a + b` 在计算时会被提升成 `int`

所以右边的结果类型是 `int`，不能直接赋给 `byte`

```java
int result = a + b;
```

------



## 十七、一个较特殊的复合赋值规则

```java
byte value = 10;
value += 1;
```

可以编译。

```java
byte value = 10;
value = value + 1;
```

会报错。



原因是`value + 1`会提升为 `int`。

而复合赋值：

```java
value += 1;
```

Java 会隐**式执行类似强制转换**的操作：

```java
value = (byte) (value + 1);
```

这也意味着复合赋值可能发生溢出，只是编译器允许它。

这是 Java 中一个容易忽略的细节。

------



## 十八、`null` 简单认识

**引用类型变量可以**保存 `null`

```java
String name = null;
```

> `name` 当前没有指向任何字符串对象

但**基本数据类型不能**赋值为 `null`



```java
Integer age = null;
Boolean enabled = null;
```

它们可以保存 `null`，因为 `Integer` 和 `Boolean` 是类，不是基本类型。

------





# 第 3 课：判断与逻辑

## 一、与 C++ 的重要区别：条件必须是 `boolean`

C++ 中可能写：

```cpp
int count = 1;

if (count) {
    std::cout << "成立";
}
```

因为 C++ 会把非零整数视为真。

而 Java 必须明确写成：

```java
if (count != 0) {
    System.out.println("成立");
}
```

**Java 中条件必须是真正的 `boolean`**

```text
C++：0 可以表示 false，非 0 可以表示 true
Java：条件必须严格是 boolean
```

------



## 二、判断字符串是否为空

```text
null：没有字符串对象
""：存在字符串对象，但内容长度为0
```

判断非空：

```java
if (username != null && !username.isEmpty()) {
    System.out.println("用户名有效");
}
```

这里利用了短路运算：

- 先确保 `username` 不是 `null`
- 再调用 `isEmpty()`

`isEmpty()` 表示字符串长度是否为 0。

------



## 三、现代 Java 的 `switch` 箭头写法

较新的 Java 版本可以写：

```java
int day = 2;

switch (day) {
    case 1 -> System.out.println("星期一");
    case 2 -> System.out.println("星期二");
    case 3 -> System.out.println("星期三");
    default -> System.out.println("无效日期");
}
```

这种写法不需要 `break`，也不会发生传统穿透。

多个值可以合并：

```java
int month = 1;

switch (month) {
    case 12, 1, 2 -> System.out.println("冬季");
    case 3, 4, 5 -> System.out.println("春季");
    case 6, 7, 8 -> System.out.println("夏季");
    case 9, 10, 11 -> System.out.println("秋季");
    default -> System.out.println("月份无效");
}
```

这比传统写法更清晰。

------



## 四、`switch` 也可以返回结果

现代 Java 中，`switch` 可以作为表达式产生结果：

```java
int day = 2;

String dayName = switch (day) {
    case 1 -> "星期一";
    case 2 -> "星期二";
    case 3 -> "星期三";
    default -> "无效日期";
};
```

这里整个：

```java
switch (day) {
    ...
}
```

会产生一个 `String` 结果，然后赋给：

```java
dayName
```

最后注意分号：

```java
};
```

因为整个 `switch` 表达式参与了赋值语句。

------

### 多行分支与 `yield`

如果某个分支需要执行多行代码，可以写：

```java
int score = 85;

String level = switch (score / 10) {
    case 10, 9 -> "优秀";
    case 8 -> {
        System.out.println("成绩处于80分段");
        yield "良好";
    }
    case 7, 6 -> "及格";
    default -> "不及格";
};
```

`yield` 表示：当前 `switch` 分支产生这个结果。

------



## 五、`switch` 能判断哪些类型

常见可用于 `switch` 的类型包括：

```text
byte
short
char
int

对应的包装类型
String
enum
```

不能直接使用普通 `long`、`float`、`double` 作为传统常见 `switch` 条件。

例如：

```java
String role = "ADMIN";

switch (role) {
    case "ADMIN" -> System.out.println("管理员");
    case "USER" -> System.out.println("普通用户");
    default -> System.out.println("未知角色");
}
```

**Java 支持使用 `String` 进行 `switch` 判断**

------



## 六、`switch` 判断字符串时发生什么

```java
String role = "ADMIN";

switch (role) {
    case "ADMIN" -> System.out.println("管理员");
    case "USER" -> System.out.println("普通用户");
    default -> System.out.println("未知角色");
}
```

这里是**按字符串内容匹配**，不需要手动写：

```java
role.equals("ADMIN")
```

但需要注意：

```java
role == null
```

时，直接执行传统字符串 `switch` 可能产生**空指针异常**。

因此业务代码中常常**先处理 `null`**：

```java
if (role == null) {
    System.out.println("角色不能为空");
} else {
    switch (role) {
        case "ADMIN" -> System.out.println("管理员");
        case "USER" -> System.out.println("普通用户");
        default -> System.out.println("未知角色");
    }
}
```

------



# 第 4 课：循环

## 一、增强 for 循环

这是 Java 中非常常见的写法。

```java
for (String name : names) {

}
```

例如：

```java
String[] names = {"Tom", "Jack", "Bob"};

for (String name : names) {
    System.out.println(name);
}
```

输出：

```text
Tom
Jack
Bob
```

------

### 为什么 Spring Boot 经常看到它

例如查询用户列表：

```java
List<User> users = userMapper.selectList();

for (User user : users) {
    System.out.println(user.getName());
}
```

意思：

> 遍历 users 集合中的每一个 User。

------



## 二、break

`break`：

> 立即结束当前循环。

例如：

```java
for (int i = 1; i <= 10; i++) {

    if (i == 5) {
        break;
    }

    System.out.println(i);
}
```

输出：

```text
1
2
3
4
```

当：

```java
i == 5
```

时：

```java
break;
```

跳出循环。

------



## 三、continue

`continue`：

> 跳过当前这一次循环，进入下一次循环。

例如：

```java
for (int i = 1; i <= 5; i++) {

    if (i == 3) {
        continue;
    }

    System.out.println(i);
}
```

输出：

```text
1
2
4
5
```

当：

```text
i = 3
```

跳过输出。

------





# 第 5 课：方法

## 一、Java 方法和 C++ 函数的区别

### 1. Java 方法必须属于类

C++：

```cpp
void sayHello()
{
}
```

可以存在于全局。

Java：

```java
public class Main {
    public static void sayHello() {
    
    }
}
```

**必须放在类里面**，这是 Java 面向对象设计的要求。

------

### 2. Java 没有真正的全局函数

Java 必须：

```java
class Calculator {
    int add(int a,int b){

    }
}
```

所以 Spring Boot 中：

```
UserService
OrderService
UserMapper
```

本质都是：

> 装方法的类。

------



## 二、方法名规范

Java 方法名一般使用小驼峰：

```java
getUser()

findUserById()

createOrder()

checkPassword()
```

------



## 三、方法重载（简单认识）

Java 支持同一个类中**方法名相同**，但是**参数不同**。

例如：

```java
public static int add(int a,int b){
    return a+b;
}

public static double add(double a,double b){
    return a+b;
}
```

调用 `add(1,2)`，使用 `int版本`

调用 `add(1.5,2.5)`，使用 `double版本`

### 方法不能只靠返回值区分重载

Java 不允许：

```java
int add(int a,int b)
double add(int a,int b)
```

判断重载只看：

```
方法名 + 参数列表
```

**不看返回值**

------



## 四、对象参数传递（重点）

现在看：

```java
class User {
    String name;
}

public class Main {
    public static void change(User user){
        user.name = "Tom";
    }
    
    public static void main(String[] args){
        User u = new User();
        u.name = "Jack";
        change(u);
        System.out.println(u.name);
    }
}
```

输出：

```text
Tom
```

### 对象变量保存的是对象的引用

关键：

```java
User u = new User();
```

变量 `u` 保存的不是整个 User 对象，而是 `对象的引用

`change(u)` Java 复制的是 `引用`，不是对象

实际上修改的是 `堆中的User对象`

### 但是重新赋值对象，不会影响外部

```java
public static void change(User user){

    user = new User();

    user.name = "Tom";

}

User u = new User();

u.name = "Jack";

change(u);

System.out.println(u.name);
```

输出：

```text
Jack
```

方法调用前：

```
栈内存               堆内存
u   ─────────────►  User对象1 (name="Jack")
```

方法调用时（参数传递）：

```
栈内存               堆内存
u    ─────────────►  User对象1 (name="Jack")
user ─────────────►  User对象1 (name="Jack")   // user 复制了 u 的地址
```

执行 `user = new User()` 后：

```
栈内存               堆内存
u    ─────────────►  User对象1 (name="Jack")
user ─────────────►  User对象2 (name="Tom")    // user 指向了新对象
```

------



## 五、Java 的引用类型

Java 中变量大致分两类：

### 1. 基本类型

```java
int
double
boolean
char
```

### 2. 引用类型

```java
User
String
List
Map
```

保存：

```text
对象的引用
```

**注意String 的特殊性**：String 是对象，而不是基本类型

数组参数：**数组也是引用类型**

------



# 第 6 课：`static`

`static`表示：这个方法属于类本身，可以不用创建对象，**直接通过类调用**

------

## 一、static 变量

普通变量，属于**对象**

`static`变量，属于**类**

```java
public class Student {
    static String school = "南京信息工程大学";
}
```

```java
Student s1 = new Student();
Student s2 = new Student();
```

虽然对象有两个，但是`school`只有一份，节省了开销

此时可以直接`Student.school`，而不需要`s1.school`，因为`static`的`school`成员属于`Student类`，而不是某个对象。

------



## 二、static 方法

```java
public class User {
    public static void hello(){
        System.out.println("Hello");
    }
}
```

`hello()`属于：`User类`，不是对象

```java
User.hello();  // ✅
```

------

### 普通方法必须依赖对象

```java
public class User {
    public void hello(){
        System.out.println("Hello");
    }
}
```

普通方法属于对象，必须通过**对象调用方法**

```java
User user = new User();
user.hello();  // ✅
```

不能直接用类调用：

```java
User.hello();  // ❌
```

| 方法        | 调用方式      |
| ----------- | ------------- |
| 普通方法    | `对象.方法()` |
| static 方法 | `类.方法()`   |

------

###  `main()` 必须是 static

```java
public class Main {
	public static void main(String[] args)
}
```

程序刚启动的时候，还没有任何对象。

JVM需要先执行`main()`，`main`必须属于类



### static 方法里面不能直接访问普通成员变量

```java
public class User {
    String username;

    public static void test(){   // static方法不能访问static
        System.out.println(username); // 错误❌
    }
}
```

`test()`属于类，执行时可能一个对象都没有，或无法确定是哪个对象的成员变量。

------



### static 方法可以访问

#### ① static 变量

```java
public class User {
    static String school="NUIST";

    public static void getSchool(){
        System.out.println(school); // 正确✅
    }
}
```

------

#### ② static 方法

```java
public class User {
    public static void hello(){
	}

	public static void test(){
    	hello();    // 正确✅
	}
}
```

没有问题，因为都属于类

------



### 类内的普通方法可以访问 static

```java
class User {
    static String school="NUIST";
    String name;

    public void show(){   // 普通方法可以访问static
        System.out.println(name);   // 正确✅
        System.out.println(school); // 正确✅
    }
}
```

------



### 工具方法适合用 static

数学计算：

```java
Math.max()
Math.min()
```

不需要对象，应该用`static`

------

字符串工具：

```java
StringUtils.isEmpty(...)
```

没有状态，适合用`static`

------

配置：

```java
public static final int MAX_SIZE = 100;
```

全局常量，所有地方共享，适合用`static`

------



## 三、static final

```java
public static final int MAX_SIZE = 100;
```

`final` 表示：不能修改

以后整个程序都是100，不能对 `MAX_SIZE` 重新赋值

------







# 第 7 课：构造方法与 `this`

构造方法就是**"对象出生时的初始化"**

------

## 一、构造方法

```java
public class User {
    String username;
    int age;
    
    public User() {
        System.out.println("创建了一个User对象");
    }
}
```

这里的方法叫：

```java
public User()
```

**只要执行 `new User()`，构造方法就会自动调用。**

构造方法：

```java
public User() {

}
```

只要：`new User()`，就执行。

负责：构造对象

------

### 构造方法特征：

- ① 方法名必须**和类名一样**
  - 类：`class User`
  - 方法：`User()`
- ② 没有返回值
  - 不能写：`public void User()`，错误！
  - 构造方法：**没有返回值类型**
- ③ 自动执行
  - 只要：`new User()`，就会执行

------



## 二、默认构造方法

如果类里没有写构造方法，仍然可以使用构造方法

因为 Java 自动生成了**默认构造方法（无参构造）**：

```java
public User() {
}
```

但是，只要自己写了：

```java
public User(String name) {
}
```

Java 就不会再自动生成 `public User()`

此时：

```java
new User();  // 编译错误！❌
```

因为已经没有：`User()` 这个构造方法了。

------



## 三、有参构造方法（重点）

```java
public class User {
    String username;
    int age;

    public User(String username, int age) {
        this.username = username;
        this.age = age;
    }
}
```

创建：

```java
User user = new User("Zhang Hao", 23);
```

对象一出生：

```text
username = Zhang Hao
age = 23
```

已经初始化完成，不需要再对对象进行手动初始化

------



## 四、 `this`

`this` 表示 ` "当前这个对象"`

```java
public class User {
    String username;
    
    public User(String username) {
        this.username = username;
    }
}
```

```java
User user = new User("Tom");
```

当前对象就是`user`

于是 `this.username` 表示：当前对象里面的 `username`

`username` 表示：参数 `username`

所以：

```java
this.username = username;
```

真正意思：

```text
对象.username = 参数.username
```

------



## 五、`this` 调用当前对象的方法

```java
public class User {
    String username;

    public void print() {
        System.out.println(username);
    }

    public void show() {
        this.print();
    }
}
```

`this.print()` 就是：当前对象调用自己的` print()`

------



## 六、`this()`

构造方法还可以**互相调用**。

```java
public class User {
    String username;
    int age;

    public User() {         // 提供了一个无参构造方法
        this("Unknown", 0); // 并为属性设置了默认值
    }

    public User(String username, int age) {
        this.username = username;
        this.age = age;
    }
}
```

执行 `new User()`，先执行：

```java
this("Unknown",0);
```

再进入：

```java
User(String username,int age)
```

可以提供一个**无参构造方法**，并为属性**设置默认值**

所有属性设置逻辑都在有参构造方法里，不用在多个构造方法里重复写赋值语句

------



## 七、Java 没有析构函数

JVM 的垃圾回收器（GC）负责对象回收。

------



# 第 8 课：封装

------

## 一、为什么需要封装

Java 面向对象强调：**对象应该自己管理自己的数据。**

别人不能随便改，这就是：**封装**

封装：把数据隐藏起来，只允许**通过规定的方法访问**。

对象只开放：`setName()、setAge()、login()`

而不是：`username、password、balance`

------



## 二、四大访问修饰符

| 修饰符       | 同类 | 同包 | 子类（不同包） | 任意类 |
| ------------ | :--: | :--: | :------------: | :----: |
| `private`    |  ✅   |  ❌   |       ❌        |   ❌    |
| 默认（不写） |  ✅   |  ✅   |       ❌        |   ❌    |
| `protected`  |  ✅   |  ✅   |       ✅        |   ❌    |
| `public`     |  ✅   |  ✅   |       ✅        |   ✅    |

**private**：只有自己

**默认**：自己 + 同包

**protected**：自己 + 同包 + 子类

**public**：所有人



## 三、外部通过方法访问 private

```java
public class User {
    private String username;

    public void setUsername(String username){
        this.username = username;
    }
}
```

```java
User user = new User();
user.setUsername("Tom"); // ✅
```

------



## 四、Getter 和 Setter，两个固定命名规范

### Setter

负责：修改数据。

```java
public void setUsername(String username){
    this.username = username;
}
```

命名方法：`set + 属性名`

例如修改年龄：`setAge()`

**`Setter`不仅仅可以赋值**，还能校验、过滤、记录日志、触发其他操作

```java
public void setPassword(String password){

    if(password.length() < 6){
        System.out.println("密码太短");
        return;
    }
    
    this.password = password;
}
```

------

### Getter

负责：读取数据。

```java
public String getUsername(){
    return username;
}
```

命名方法：`get + 属性名`

例如读取年龄：`getAge()`

**Getter 不仅仅是返回**，也可以动态计算

```java
public double getPrice(){
    return price * discount;
}
```

------

`Getter / Setter`：不是机械地：读取、赋值

它们也是：**对象对外开放**的接口

------



## 五、初识Lombok

```java
@Data
public class User {
    private String username;
    private Integer age;
}
```

没有：`getUsername()、setUsername()`，还能运行

因为`Lombok`自动帮忙生成了

实际项目中，Getter Setter 很多都是**自动生成**的

------



# 第 9 课：继承

**继承**：子类自动拥有父类的属性和方法。

------

## 一、Java 继承语法

格式：

```java
class 子类 extends 父类 
}
```

父类：

```java
class User {
    String username;

    public void login(){
        System.out.println("登录");
    }
}
```

子类：

```java
class AdminUser extends User {
}
```

这里 `extends` 表示继承， `AdminUser` 自动拥有：`username`、`login()`

Java 类继承有一个限制：一个类只能继承一个父类。

```java
class Dog extends Animal // 正确✅
```

```java
class Dog extends Animal, Pet // 错误❌
```

------



## 二、继承中的访问权限

父类：

```java
class User {
    private String username;
}
```

子类：

```java
class AdminUser extends User {
    public void test(){
        username = "Tom"; // 错误❌
    }
}
```

因为 `private` 只能当前类访问，即使子类也不行

------

### protected

给子类使用，同时限制外部访问。

例如：

```java
class User {
    protected String username;
}
```

子类：

```java
class AdminUser extends User {

    public void test(){
        username = "Tom"; // 正确✅
    }
}
```

------



## 三、方法重写 Override

继承之后：子类可以修改父类的方法。

父类：

```java
class Animal {
    public void sound(){
        System.out.println("动物叫");
    }
}
```

子类：

```java
class Dog extends Animal {
    public void sound(){
        System.out.println("汪汪");
    }
}
```

调用：

```java
Dog dog = new Dog();
dog.sound();
```

输出：

```text
汪汪
```

这叫：方法重写（Override）

------

父类定义规则，子类提供具体实现。

------



## 四、`@Override` 注解

推荐写：

```java
@Override
public void sound(){
    System.out.println("汪汪");
}
```

告诉编译器：我想重写父类方法。

------



## 五、重写的规则

子类重写父类方法：

需要满足：

------

### 1. 方法名相同

父类：

```java
login()
```

子类：

```java
login()
```

------

### 2. 参数列表相同

父类：

```java
login(String username)
```

子类：

```java
login(String username)
```

------

### 3. 返回类型兼容

父类：

```java
Object getData()
```

子类：

```java
String getData()
```

（String 是 Object 的子类）

------

### 4. 子类不能降低访问权限

父类：

```java
public void test()
```

子类：

```java
private void test() // 错误❌
```

父类公开的方法，子类不能把它变私有。

------



## 六、`super`

子类重写父类方法：

```java
class Dog extends Animal {

    @Override
    public void sound(){
        System.out.println("汪汪");
    }
}
```

如果还想调用父类版本，使用：`super`

`super`表示：父类对象部分

```java
class Dog extends Animal {
    @Override
    public void sound(){
        super.sound();  // 表示
        System.out.println("汪汪");
    }
}
```

执行：

```java
Dog dog = new Dog();
dog.sound();
```

输出：

```text
动物叫
汪汪
```

------

### `this` 和 `super` 对比

| 关键字 | 含义     |
| ------ | -------- |
| this   | 当前对象 |
| super  | 父类对象 |

```java
this.name // 当前类变量
```

```java
super.name // 父类变量
```

------



## 七、构造方法与继承

**创建子类对象，必须先创建父类部分**

父类：

```java
class Animal {
    public Animal(){
        System.out.println("Animal构造");
    }
}
```

子类：

```java
class Dog extends Animal {
    public Dog(){
        System.out.println("Dog构造");
    }
}
```

```java
Dog dog = new Dog();
```

输出：

```text
Animal构造
Dog构造
```

执行顺序：

```text
new Dog()
   ↓
调用 Animal()
   ↓
调用 Dog()
```

------



## 八、super()

`super()` 是**用来调用父类构造方法**的专用语法

子类构造方法默认会调用：`super()`

------

`super()` 的两种写法：

| 写法          | 含义                       | 何时使用                             |
| :------------ | :------------------------- | :----------------------------------- |
| `super()`     | 调用父类的**无参构造方法** | 父类有无参构造时，编译器**自动插入** |
| `super(参数)` | 调用父类的**有参构造方法** | 父类没有无参构造，或想传参初始化     |

```java
class Parent {
    private String name;
    
    // 无参构造
    public Parent() {
        this.name = "Unknown";
    }
    
    // 有参构造
    public Parent(String name) {
        this.name = name;
    }
}
```

```java
class Child extends Parent {
    public Child() {
        //  super();  编译器自动写入
    }
    
    public Child(String name) {
        super(name);  // 必须手动调用父类有参构造
    }
}
```

------



# 第 10 课：多态

## 一、多态的核心概念

同一个父类引用，在运行时表现出不同子类的行为

`Animal animal` 它可以指向：`Dog` 和 `Cat`

```text
        Animal
          |
    --------------
    |            |
   Dog          Cat
```

```java
Animal a1 = new Dog();
Animal a2 = new Cat();
```

实际对象不同：

```text
a1 → Dog对象

a2 → Cat对象
```

------



## 二、多态成立的三个条件

### 条件1：存在继承关系

```java
class Dog extends Animal
```

必须有：父子关系

------

### 条件2：子类重写父类方法

父类：

```java
class Animal {
    public void eat(){
        System.out.println("吃东西");
    }
}
```

子类：

```java
class Dog extends Animal {
    @Override
    public void eat(){
        System.out.println("吃骨头");
    }
}
```

------

### 条件3：父类引用指向子类对象

```java
Animal animal = new Dog();  // 这就是多态
```

**三个条件缺一不可**

------



## 三、变量类型和对象类型

```java
class Animal {
    public void eat(){
        System.out.println("动物吃东西");
    }
}

class Dog extends Animal {
    @Override
    public void eat(){
        System.out.println("狗吃骨头");
    }
}

public class Main {
    public static void main(String[] args){
        Animal animal = new Dog();
        animal.eat();
    }
}
```

```java
Animal animal = new Dog();
```

------

左边：`Animal animal` 叫：编译类型

决定：能调用什么

------

右边：`new Dog()` 叫：运行类型

决定：实际执行哪个方法

------

执行 `animal.eat()`过程：

第一步：编译器看到`Animal`，检查`Animal类` 有没有`eat()`，如果有 则进入下一步，否则报错

第二步：运行时发现实际对象是`Dog`，于是执行：`Dog.eat()`，这是动态绑定

------



## 四、动态绑定

```java
Animal animal = new Dog();
animal.eat();
```

运行时寻找：实际对象的方法

```text
animal
  ↓
实际指向
  ↓
Dog对象
  ↓
调用Dog.eat()
```

------



## 五、父类引用的限制

**父类引用只能使用父类已有方法**

```java
Animal animal = new Dog();
```

`animal` 只能调用 `Animal类` 中定义的方法。

```java
class Animal {
    public void eat(){
        System.out.println("动物吃东西");
    }
}

class Dog extends Animal {
    public void bark(){
        System.out.println("汪汪");
    }
}
```

```java
animal.bark(); // 错误❌
```

因为左边类型 `Animal类` 里面没有 `bark()`

变量看不到子类新增内容。

------



## 六、向上转型

```java
Animal animal = new Dog();
```

`Dog类`向上变成`Animal类`

```text
Animal
  ↑
  |
 Dog
```

自动完成，不用强制转换

------



## 七、向下转型

父类引用想**调用子类特殊方法**，需要：向下转型

```java
class Animal {
    public void eat(){
        System.out.println("动物吃东西");
    }
}

class Dog extends Animal {
    public void bark(){
        System.out.println("汪汪");
    }
}
```

```java
Dog dog = (Dog) animal; // 这里(Dog)表示：强制转换
dog.bark();
```

------



## 八、instanceof

判断对象是不是某种类型。

```java
if(animal instanceof Dog){
    Dog dog = (Dog) animal;
    dog.bark();
}
```

如果 `animal` 是 `Dog类`，才转换

------



## 九、多态和方法参数

```java
public class AnimalService {
    public void feed(Animal animal){
        animal.eat();
    }
}
```

方法的参数是父类`Animal类`的对象，调用方法时，实参**可以用用子类对象**：

```java
service.feed(new Dog());
service.feed(new Cat());
```

------



## 十、IoC / 依赖注入的基础

```java
@Autowired
private UserService userService;
```

这里的变量类型 `UserService`，但是实际：Spring 注入 `UserServiceImpl` 对象

------

本质是：

```java
UserService userService = new UserServiceImpl();
```

左边（接口）：`UserService` 定义了"有什么功能"（规范）

右边（实现类）：`UserServiceImpl` 具体实现这些功能（细节）

`@Autowired` 的本质，就是把原本由手写的 `new UserServiceImpl()` 这行代码，交给了 Spring 在启动时去执行，并自动赋值。只需要声明 "我需要一个 `UserService` 类型的对象" 即可

------



## 十一、多态与接口关系

接口就是：更抽象的父类。

```java
interface Animal {
    void eat();
}
```

实现：

```java
class Dog implements Animal{
    public void eat(){
    }
}
```

然后：

```java
Animal animal = new Dog();
```

仍然是多态。

------



# 第 11 课：接口

------

## 一、什么是接口？

接口是一份**规则**，规定某个类必须具备哪些能力。

------

定义一个接口：

```java
interface Flyable {
    void fly();
}
```

任何实现 Flyable 的东西，必须拥有：`fly()` 方法

------

### 如果不用接口（紧耦合）

```java
// 写死了具体实现
class Duck {
    public void fly() { 
        System.out.println("鸭子飞"); 
    }
}

class Bird {
    public void fly() { 
        System.out.println("鸟飞"); 
    }
}
```

```java
// 方法直接依赖具体类
public void letFly(Duck duck) { // 这里写死了 Duck
    duck.fly();
}

public void letFly(Bird bird) { // 如果还要让鸟飞，得重载一个方法！
    bird.fly();
}
```

以后增加新的会飞动物，需要不断重载方法，不方便

------

### 如果使用接口（松耦合）

```java
// 接口：定义功能 “会飞的”
interface Flyable {
    void fly();
}
```

```java
// 实现类：实现 “会飞的” 接口功能
class Duck implements Flyable {
    public void fly() { 
        System.out.println("鸭子飞"); 
    }
}

class Bird implements Flyable {
    public void fly() { 
        System.out.println("鸟飞"); 
    }
}

class Superman implements Flyable {
    public void fly() { 
        System.out.println("超人飞"); 
    }
}
```

```java
public class Main {
    // 方法只依赖接口
	public void letFly(Flyable f) { // 接口引用作为参数
    	f.fly();
	}
    
	public void static main(String[] args) {
    	letFly(new Superman());
	}
}
```

这就是：面向接口编程。

------



## 二、interface 语法

```java
interface 接口名 {
    方法;
}
```

```java
public interface Flyable {
    void fly();
}
```

因为接口只规定，不实现

------



## 三、implements

类实现接口， 使用：`implements`

```java
interface Flyable {
    void fly();
}

class Bird implements Flyable {
    public void fly(){
        System.out.println("鸟飞");
    }
}

class Plane implements Flyable {
    public void fly(){
        System.out.println("飞机飞");
    }
}
```

```java
public class Main {
    public static void main(String[] args){
        Flyable f1 = new Bird();
        Flyable f2 = new Plane();
        f1.fly();
        f2.fly();
    }
}
```

------



## 四、接口本质上也是多态

接口：`Flyable`

实现类：`Bird`

```java
Flyable f = new Bird();
```

接口提供一种更加灵活的多态

------

引用接口进行实例化 ，目的是：解耦，让调用者只关心“能不能实现”，而不是“谁实现”

```java
接口名 变量名 = new 实现类()
```

------



## 五、接口和继承的区别

|        | 继承         | 接口         |
| ------ | ------------ | ------------ |
| 关键词 | extends      | implements   |
| 含义   | 是什么       | 会什么       |
| 关系   | is-a         | can-do       |
| 数量   | 只能一个父类 | 可以多个接口 |

------



## 六、Java 一个类可以实现多个接口

Java 类只能单继承，但可以**实现多个接口**，弥补了单继承的不足

```java
public class UserServiceImpl implements UserService, Serializable, Comparable<User> {
    // 可以实现多个接口，获得多种能力
}
```

------

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}
```

```java
class Duck implements Flyable, Swimmable {
    public void fly(){
    }

    public void swim(){
    }
}
```

鸭子同时会飞、会游泳

------



## 七、接口中的变量

接口可以定义变量：

```java
interface Config {
    int MAX_SIZE = 100;
}
```

默认为 `public static final`

其中 `MAX_SIZE` 是：

- 公共的
- 静态的
- 不可修改的

等价于：

```java
public static final int MAX_SIZE = 100;
```

------



## 八、接口中的方法

```java
interface UserService {
    void login();
}
```

默认 `public abstract` 抽象方法，没有实现

------

实现类 必须实现该方法

```java
class UserServiceImpl implements UserService {
    public void login(){
        System.out.println("登录");
    }
}
```

------



## 九、接口不能 new

```java
Flyable fly = new Flyable(); // 错误❌
```

因为接口只是**规则**，不是具体对象

------



## 十、接口与抽象类（基础区别）

### 抽象类：

```java
abstract class Animal {
    abstract void eat();
}
```

特点：

- 有变量
- 有普通方法
- 有抽象方法

------

### 接口：

```java
interface Flyable {
    void fly();
}
```

特点：

- 主要**描述能力**

|                  | 抽象类         | 接口         |
| ---------------- | -------------- | ------------ |
| 关键词           | abstract class | interface    |
| 继承             | extends        | implements   |
| 数量             | 一个           | 多个         |
| 是否能有普通方法 | 可以           | 现代Java可以 |
| 是否有状态       | 可以           | 通常没有     |

------



## 十一、Spring Boot 为什么大量使用接口？

Service 层，接口：

```java
public interface UserService {
    User findById(Long id);
}
```

Service 层，实现：

```java
@Service
public class UserServiceImpl implements UserService {
    public User findById(Long id){
        return user;
    }
}
```

------

Controller 层：

```java
@RestController
public class UserController {
    private UserService userService; // 这里不是 UserServiceImpl userServiceImpl
}
```

Controller 只需要知道 “我要用户服务”，不关心具体是谁实现

如果未来Service 层的实现发生更改，Controller 层的代码不需要改。

这就是：**解耦**

------



## 十二、MyBatis 中接口更常见

```java
@Mapper
public interface UserMapper {
    User selectById(Long id);
}
```

接口里面没有实现，但 MyBatis 会帮你生成实现类。

------



## 十三、Spring IoC 和接口的关系

```java
@Autowired
private UserService userService;
```

这里类型 `UserService ` 是接口

但是Spring 实际注入：`UserServiceImpl对象` （多态）

类似于：

```java
UserService userService = new UserServiceImpl();
```

------

Spring 三大核心：**`IoC`、`DI`、`AOP`**

底层大量依赖：接口 + 多态

------

# 第 12 课：异常处理（Exception）

## 一、为什么需要异常处理？

如果一个网站，用户登录：

```text
输入账号密码
    ↓
查询数据库
    ↓
数据库异常
    ↓
程序直接崩溃
```

用户看到 `500错误` ，体验非常差。

所以我们需要：捕获错误，并进行处理。

------



## 二、异常和错误的区别

Java 中主要分：

```java
Throwable
	├── Exception // 异常, 程序逻辑或环境问题, 通常可以处理
	│
	└── Error     // 错误, 系统级严重问题, 通常无法处理
```

------

### Exception  异常

程序运行过程中出现的不正常情况，通常可以处理：

#### 1. 数学异常

```java
10 / 0
```

#### 2. 空对象

```java
User user = null;
user.getName();
```

#### 3. 数组越界

```java
int[] arr = new int[3];
arr[10];
```

#### 4. 文件不存在

```text
读取xxx.txt
但是文件不存在
```

#### 5. 业务逻辑问题

```
用户名不存在
密码错误
参数为空
```

这些情况，编译可能通过，但是运行时出错

------

### Error 错误

严重问题，程序通常无法处理：

#### 1. 内存耗尽

```text
OutOfMemoryError
```

#### 2. 栈溢出

```text
StackOverflowError
```

通常不是业务代码解决

------



## 三、 try-catch

当 `try 块` 里抛出异常时，程序不会直接崩溃，而是跳转到 `catch 块`

```java
try {
    可能异常的代码
} catch(Exception e) {
    处理异常
}
```

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
            System.out.println(result);
        } catch(Exception e) {
            System.out.println("发生错误");
        }
    }
}
```

输出：

```
发生错误
```

------

### catch里的 e 是什么？

```java
catch(Exception e)
```

`Exception`表示：捕获哪种异常

`e`是：异常对象，里面保存：错误信息

```java
catch(Exception e){
    System.out.println(e.getMessage()); // 输出：报错信息
    e.printStackTrace(); // 打印完整错误堆栈
}
```

------



## 四、捕获具体异常

`catch(Exception e)` 比较宽泛

捕获具体异常：

```java
try {
    int a = 10 / 0;
}
catch(ArithmeticException e){
    System.out.println("不能除0");
}
```

------

空指针：

```java
try {
    user.getName();
}
catch(NullPointerException e){
    System.out.println("用户为空");
}
```

不同错误的处理方式不同

------



## 五、多个 catch

```java
try {
    //代码1
}
catch(ArithmeticException e){
	//代码2
}
catch(NullPointerException e){
	//代码3
}
```

执行时：匹配对应异常

顺序从具体到一般。

------

错误例子：

```java
catch(Exception e){ // Exception 太宽泛，已经捕获所有异常
	//代码1
}
catch(NullPointerException e){ // 前面捕获了所有异常，代码2永远执行不到
	//代码2
}
```

------



## 六、finally

`finally` 表示：无论是否发生异常，**都会执行**

```java
try {
    System.out.println("打开文件");
}
catch(Exception e){
    System.out.println("异常");
}
finally {
    System.out.println("关闭文件");
}
```

正常：

```
打开文件
关闭文件
```

异常：

```
异常
关闭文件
```

------

### 为什么需要 finally？

数据库连接：

```
打开连接
   |
执行SQL
   |
关闭连接
```

无论SQL成功失败：

都应该关闭连接。

------



## 七、throw：主动抛异常

之前：异常是系统产生

现在：我们可以自己制造异常

```java
public void setAge(int age){
    if(age < 0){
        throw new RuntimeException("年龄不能小于0");
    }
    // 代码
}
```

`throw` 表示：主动抛出异常

**执行流程**：

1. `new RuntimeException(...)` 在堆内存中创建了一个异常对象，里面存着错误信息。
2. `throw` 把这个异常对象扔出去
3. JVM 会**向上寻找**（调用者）有没有`try-catch`接住：
    • 找到了 → 交给 `catch` 处理
       • 找不到 → 程序崩溃，JVM 打印异常堆栈

------



## 八、throws：声明异常

```java
public void test() throws Exception { // 声明这个方法可能抛异常
	// 代码
}
```

表示：这个方法**可能抛异常**，**提醒**调用者注意处理。

------

定义 `readFile()` 方法：

```java
public void readFile() throws IOException {

}
```

调用 `readFile()` 时需要：

```java
try {
    readFile();
}
catch(IOException e){ // 以防异常的出现

}
```

------



## 九、checked 和 unchecked 异常

Java 异常分两类：

### 1. 编译时异常（Checked Exception）

编译器**强制要求处理**

例如：`IOException` 、`FileReader`

必须 `try-catch`，或者 `throws`

------

### 2. 运行时异常（RuntimeException）

例如：`NullPointerException`、`ArithmeticException`、`IndexOutOfBoundsException`

编译器**不强制处理**

Spring Boot 开发中，大量业务异常通常继承 `RuntimeException`

------



## 十、自定义异常

```java
public class BusinessException 
        extends RuntimeException { // BusinessException 继承了 RuntimeException
    public BusinessException(String message){
        super(message);  // 把提示信息传给父类（RuntimeException）
    }
}
```

```java
public class OrderService {
    public void createOrder(int userId, int amount) {
        User user = findUser(userId);
        if (user.getBalance() < amount) {
            // 余额不足，抛出业务异常
            throw new BusinessException("余额不足，当前余额：" + user.getBalance());
            // 没有try-catch，createOrder()方法自己不处理，把异常抛给调用它的方法（上一层）
        }
        // 余额充足，继续创建订单...
        System.out.println("订单创建成功");
    }
}
```

------

项目中的：

```text
BusinessException

LoginException

PermissionException
```

都是自定义异常

------



# 第 13 课：泛型基础

## 一、没有泛型的问题

假设我们设计一个盒子：

```java
public class Box {
    private Object data; // Object是所有类的根父类
    
    public void setData(Object data) {
        this.data = data;
    }
    public Object getData() {
        return data;
    }
}
```

因为 `Object` 是所有类的根父类，所以这个 `data` 字段可以存储**任何类型的对象**：

String、User、Student、Integer，最终都属于 `Object`

因此这个盒子什么都能装：

```java
Box box = new Box();

box.setData("Hello");
box.setData(new User());
box.setData(123);
```

但是，Object 最大的问题：取出来不知道是什么，Java 只知道 Object（类型丢失）

```java
Box box = new Box();
box.setData("Hello");  // 存入 String

Object data = box.getData();  // getData()返回的是 Object类型，取出来的是 Object
// Java 只知道这是个 Object，不知道它原本是 String
```

如果用String去接收返回值：

```java
String text = box.getData(); // 错误❌
// String 是 Object 的子类，编译器不允许向下转型（需要强制转换）
```

于是强制转换：

```java
String text = (String) box.getData(); // 正确✅
```

但是强制转换有风险：

```java
Box box = new Box();
box.setData(123);  // 存的是 Integer

// 强行转换为 String
String text = (String) box.getData();  // 编译通过✅
                                       // 但运行时类型不匹配，报错❌，抛出 ClassCastException
```

------

所以，我们希望创建盒子的时候就规定：

- 这个盒子**只能装 String**，比如 `Box<String>`
- 另一个盒子**只能装 User**，比如`Box<User>`

这样编译器就知道里面到底是什么类型，这就是：**泛型**

泛型就是**把 “类型” 也变成一种参数**

------



## 二、第一次看 `<T>`

```java
public class Box<T> {
    private T data;

    public void setData(T data) {
        this.data = data;
    }
    public T getData() {
        return data;
    }
}
```

把 `T` 理解成：**这里的具体类型以后再告诉我**，相当于**类型参数**，创建时再告诉具体类型：

```java
Box<String>
```

于是：`T = String`

```java
Box<User>
```

于是：`T = User`

------

就像普通方法：

```java
public void test(int x)
```

这里 `x` 是数据参数，调用时才告诉具体值

------

### 真正使用时：

创建：

```java
Box<String> box = new Box<>(); // box 是一个Box，其中的数据类型是 String
```

现在：`T = String`

```java
box.setData("Hello");    // 正确✅
```

```java
box.setData(new User()); // 错误❌
```

编译器发现：这个盒子只能放 String

------

### `<>` 里面不一定叫 T：

```java
class Box<T>
```

这里的：`T` 只是一个名字，理论上完全可以随意命名

但是 Java 有惯例：

| 字母 | 一般表示      |
| ---- | ------------- |
| `T`  | Type，类型    |
| `E`  | Element，元素 |
| `K`  | Key，键       |
| `V`  | Value，值     |

------

### `new Box<>()` 为什么右边没写 String？

你可能已经注意到了：

```java
Box<String> box = new Box<>();
```

为什么不是：

```java
Box<String> box = new Box<String>();
```

其实两个都可以。

以前 Java 常写：

```java
Box<String> box = new Box<String>();
```

现代 Java：

编译器已经知道左边：

```java
Box<String>
```

所以右边：

```java
new Box<>()
```

可以自动推断：

```text
这里也是 String
```

------



## 三、泛型参数

泛型参数可以有多个

```java
class Pair<K, V> {
    private K key;
    private V value;
}
```

```java
Pair<String, Integer>
```

于是：K = String，V = Integer

------



## 四、泛型不能直接使用基本数据类型

Java 泛型只能使用：**引用类型**

```java
Box<int>     // 错误❌
```

```java
Box<Integer> // 正确✅
```

八种基本类型要使用对应的**包装类**

| 基本类型  | 包装类      |
| --------- | ----------- |
| `int`     | `Integer`   |
| `long`    | `Long`      |
| `double`  | `Double`    |
| `boolean` | `Boolean`   |
| `char`    | `Character` |

------



## 五、泛型方法

除了泛型类：`class Box<T>`

方法本身也可以有泛型：

```java
public static <T> void print(T data) {  // <T>放在返回值 void 之前，表示：是这个方法自己定义的泛型参数
    System.out.println(data);
}
```

调用：`print("Hello")`

此时：`T = String`

------

调用：`print(100)`

此时：`T = Integer`

------

注意语法：

```java
public static <T> void print(T data)
```

------



## 六、Spring Boot 中一个非常典型的泛型：`Result<T>`

统一返回结果，很可能会看到这种设计：

```java
public class Result<T> {
    private Integer code;
    private String message;
    private T data;
}
```

data 使用 T，是因为接口**返回的数据不固定**

一个 `Result<T>` 可以适应所有接口

------

查询一个用户：`Result<User>`

那么：`T = User`

最终：data 的数据类型为 User

------

查询用户列表：`Result<List<User>>`

那么：`T = List<User>`

最终：data 的数据类型为 一组User

```text
Result
│
├── code
├── message
│
└── data
     │
     ├── User
     ├── User
     └── User
```

------

登录接口可能：`Result<LoginVO>`

那么：`T = LoginVO`

------



# 第14课：集合（一）：`List` 与 `ArrayList`

## 一、什么是 `List`

```java
List<User> users;
```

这里要拆成两个知识点

`List`：一种“有顺序、允许重复”的集合规范

`<User>`：这个 List 只能存放 `User` 类型元素

所以：`List<User>` 可以理解成：User 列表

```text
下标0 → Tom
下标1 → Jack
下标2 → Alice
```

------

### `List` 本身是接口

`List` 不是普通类，而是：

```java
interface List<E>
```

`List` 规定“列表应该有什么能力”，但它本身不是具体实现

所以不能直接：

```java
new List<User>(); // 错误❌
```

------

### 为什么用 `List` 而非数组？

- `List` 是“**动态数组**”，它解决了数组“长度固定”这个最大的痛点
- 添加元素时，`add()` 自动追加到末尾，无需指定位置
- 删除元素时，`remove()` 自动处理，无需手动移动后续元素
- 插入元素时，`add(index, element)` 自动处理，无需手动移动后续元素

------



## 二、`ArrayList` 是 `List` 的实现类

真正可以创建对象的是：`ArrayList`

```java
ArrayList<User> users = new ArrayList<>();
```

但通常推荐：（**面向接口编程**，减少代码对具体实现类的依赖）

```java
List<User> users = new ArrayList<>(); // 接口类型引用  指向  实现类对象
```

左边：

```java
List<User> // 规定只依赖 List 这套接口
```

右边：

```java
new ArrayList<>() // 决定实际采用 ArrayList 实现
```

------

### 一个 ArrayList 实例

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        
        // 增
        names.add("Tom");  // 添加到列表末尾
        names.add("Jack");
        names.add("Alice");
        System.out.println(names); // 自动输出所有元素，格式是 [元素1, 元素2, 元素3, ...]
        
        // 删
        names.remove(1); // 删除指定下标元素，删除后，后面的元素会前移；返回被删除的元素（"Jack"）
        names.remove("Jack"); // 找到第一个匹配的元素删除；返回 true（删除成功）/ false（没找到）
        
        // 改
        names.set(1, "Bob"); // 修改已有位置元素
        
        // 查
        String name = names.get(0); // 读取元素，得到 Tom
        int index = names.indexOf("Alice"); // 查找 Alice 的元素下标
        int count = names.size(); // 获取元素数量
        boolean result = names.contains("Tom"); // 判断列表中有没有 Tom
        
        // 判空
        boolean result = names.isEmpty()); // 如果列表为空，返回 true
        
        // 遍历 List：普通 for
        for (int i = 0; i < names.size(); i++) {
    		System.out.println(names.get(i));
		}
        
        // 遍历 List：增强 for （最常用）
        for (String name : names) {
            System.out.println(name);
        }
        
    }
}
```

------

### List 可以存重复元素

```java
List<String> names = new ArrayList<>();

names.add("Tom");
names.add("Tom");
names.add("Jack");
```

```java
[Tom, Tom, Jack] // List：允许重复
```

------

### List 有顺序

```java
names.add("Tom");
names.add("Jack");
names.add("Alice");
```

顺序就是：

```text
0 Tom
1 Jack
2 Alice
```

------



## 三、为什么接口返回类型经常写 List，而不是 ArrayList

不推荐：

```java
public ArrayList<User> getUsers() {
    ...
}
```

更常见：

```java
public List<User> getUsers() {
    ...
}
```

原因仍然是：**对外暴露接口**，而不是具体实现。

调用者只关心：

```text
“你给我一组User”
```

并不应该关心内部究竟用：

```text
ArrayList
LinkedList
其他List实现
```

这就是面向接口编程。

------



## 四、`null` 和空 List 不是一回事

```java
List<User> users = null; // 表示：根本没有 List 对象
users.size(); // NullPointerException
```

------

```java
List<User> users = new ArrayList<>();
```

表示：有一个 List，只是里面暂时没有数据

```java
users.size(); // 0
users.isEmpty(); // true
```

业务代码中通常更喜欢返回：`[]`，也就是空 List，而不是：`null`

调用方更容易处理

------



## 五、常见错误：遍历时直接删除

**增强 for 遍历**时，**不要随便直接 remove** 当前集合元素

```java
for (User user : users) {
    if (user.isDeleted()) {
        users.remove(user);
    }
}
```

这种写法可能产生：

```text
ConcurrentModificationException
```

因为：在遍历集合的同时，又直接修改集合结构

------



# 第15课：集合（二）：`Map`、`HashMap` 与 `Set`

## 一、为什么需要 Map？

假设有三个用户：

```java
User user1 = new User(1001L, "Tom");
User user2 = new User(1002L, "Jack");
User user3 = new User(1003L, "Alice");
```

用 `List` 保存：

```java
List<User> users = new ArrayList<>();

users.add(user1);
users.add(user2);
users.add(user3);
```

如果我要找：

```text
id = 1002
```

对应的用户，最直观的办法是遍历：

```java
for (User user : users) {
    if (user.getId() == 1002L) {
        System.out.println(user.getName());
    }
}
```

我们更希望直接：

```java
userMap.get(1002L);
```

立即拿到对应用户，于是有了 `Map`。

------



## 二、什么是 Map？

### Map 是用 Key 找到 Value 的集合

```text
Key              Value

1001       →      Tom
1002       →      Jack
1003       →      Alice
```

所以 Map 不是普通的一排元素，而是一组：

```text
键 → 值
```

------

### Map 也是接口

```java
Map<K, V>
```

不能：

```java
new Map<>(); // 错误❌
```

真正创建对象通常使用：`HashMap`

```java
Map<Long, User> userMap = new HashMap<>();
```

------



## 三、Map 特点

### Key 不能重复

```java
scores.put("Tom", 90);
scores.put("Tom", 100);
```

最终不是：

```text
Tom → 90
Tom → 100
```

而是：

```text
Tom → 100
```

因为：**一个 Key 最多对应一个 Value**，后面的 `put()` 会覆盖之前的 Value

------

### Value 可以重复

```java
scores.put("Tom", 90);
scores.put("Jack", 90);
```

完全没问题：

```text
Tom  → 90
Jack → 90
```

------



## 四、HashMap

`HashMap` 底层利用：Hash（哈希）来根据 Key 快速找到对应 Value

```java
userMap.get(1002L);
```

------

### 一个 HashMap实例

```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();

        scores.put("Tom", 90);
        scores.put("Jack", 80);
        scores.put("Alice", 95);
        System.out.println(scores); // 可能输出：{Tom=90, Alice=95, Jack=80}
    }
}
```

`HashMap` **不保证按原先 put 的顺序排列**

------

### `put()`：放入键值对

List 用：`add()`

Map 用：`put()`

```java
scores.put("Tom", 90);
```

表示：

```text
"Tom" → 90
```

```java
scores.put("Jack", 80);
```

得到：

```text
Tom  → 90
Jack → 80
```

------

### `get()`：通过 Key 获取 Value

```java
Integer score = scores.get("Tom");  // 得到 90
```

这就是 Map 最核心的使用方式：

```java
User user = userMap.get(1001L);
```

意思：根据用户 ID `1001` 找对应的 User

如果 `get()` 找不到 Key 会怎样？

```java
Integer score = scores.get("Bob");
```

如果 `Bob` 不存在，返回：`null`，不是异常。

因此加入判断：

```java
Integer score = scores.get("Bob");

if (score != null) {
    System.out.println(score);
}
```

------

### `containsKey()`：判断有没有 key

有时候我们的目的不是马上取数据，而是判断：有没有这个 Key？

如果有，返回 true

```java
if (scores.containsKey("Tom")) {
    System.out.println("存在Tom");
}
```

------

### `containsValue()`：判断有没有 value

也可以判断 Value：

```java
scores.containsValue(90);
```

但实际项目里：`containsKey()` 通常比 `containsValue()` 常见得多

------

### `remove()`：删除一个键值对

```java
scores.remove("Tom");
```

------

### `size()` 和 `isEmpty()`

获取键值对数量：

```java
scores.size(); // 得到键值对数量
```

判断是否为空：

```java
scores.isEmpty();
```

------



## 五、Map 怎么遍历？

List 可以：

```java
for (User user : users)
```

但 Map 中，每个元素其实有两个东西：

```text
Key + Value
```

因此有几种遍历方式。

我们只掌握项目中够用的。

------

### 只遍历 Key：`keySet()`

```java
for (String name : scores.keySet()) {
    System.out.println(name);
}
```

`scores.keySet()` 会得到：所有 Key 组成的 Set

```text
Tom
Jack
Alice
```

然后：

```java
scores.get(name)
```

可以取得 Value：

```java
for (String name : scores.keySet()) {
    Integer score = scores.get(name);
    System.out.println(name + ":" + score);
}
```

------

### 只遍历 Value：`values()`

```java
for (Integer score : scores.values()) {
    System.out.println(score);
}
```

如果：

```text
Tom   → 90
Jack  → 80
Alice → 95
```

就依次处理这些 Value。

------

### 同时获取 Key 和 Value：`entrySet()`

这是非常常见的 Map 遍历方式：

```java
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    String name = entry.getKey();
    Integer score = entry.getValue();

    System.out.println(name + ":" + score);
}
```

`Map.Entry<String, Integer>`：一个 `Entry` 就代表 Map 里的一组键值对

例如：`Tom → 90` 就是一个 `Entry`，其中：

```java
entry.getKey() // 得到：Tom
```

```java
entry.getValue() // 得到：90
```

------



## 六、简单认识 Set

### `Set` 最大特点：不允许重复元素

```java
Set<String> names = new HashSet<>();

names.add("Tom");
names.add("Jack");
names.add("Tom");
```

最终只有：

```text
Tom
Jack
```

第二个 Tom 不会重复保存

------

### Set 与 List 最大区别

```text
List
├── 有顺序
├── 有下标
└── 允许重复

Set
├── 通常不靠下标
└── 不允许重复
```

------

### `HashSet`

`Set` 是接口：

```java
Set<String>
```

`HashSet`的具体实现：

```java
HashSet<String>
```

标准写法：

```java
Set<String> roles = new HashSet<>();
```

------



## 七、List、Set、Map 对比

| 集合       | 核心特点     | 常见用途                 |
| ---------- | ------------ | ------------------------ |
| `List<T>`  | 有序、可重复 | 查询结果、对象列表       |
| `Set<T>`   | 不重复       | ID集合、权限集合、去重   |
| `Map<K,V>` | Key→Value    | ID查对象、统计、映射关系 |

------

# 第16课：Lambda 表达式

## 一、什么是 Lambda 表达式

Lambda 一句话理解：一段**没有名字**、**临时传**给其他方法使用的**小函数**

遍历 `List`：

```java
List<User> users = new ArrayList<>();
for (User user : users) {
    System.out.println(user);  // 从 `users` 里依次拿出每一个 `User`，然后打印
}
```

Java 还有另一种写法：

```java
users.forEach(user -> System.out.println(user));
```

这就是 Lambda 表达式

------

```
user -> System.out.println(user)
```

大致表达：给我一个 `user`，我就执行 `System.out.println(user)`

左边：`user` 是参数

中间：`->` 可以读成：“ 然后执行 ”

右边：`System.out.println(user)` 是要执行的代码

所以这句代码可以理解为：对于传进来的每一个 `user`，把它输出

------

格式：

```
(参数列表) -> { 方法体 }
```

------



## 二、五种常见写法

### 写法1：完整版（参数带类型 + 大括号）

```java
(String s) -> {
    System.out.println("Hello " + s);
}
```

### 写法2：省略参数类型（编译器自动推断）

```java
(s) -> {
    System.out.println("Hello " + s);
}
```

### 写法3：只有一个参数时，括号可以省略

```java
s -> {
    System.out.println("Hello " + s);
}
```

### 写法4：只有一条语句时，大括号可以省略

```java
s -> System.out.println("Hello " + s)
```

### 写法5：只有一条 return 语句时，可以更简

```java
// 完整写法
(a, b) -> {
    return a + b;
}

// 最简写法（去掉大括号和 return）
(a, b) -> a + b
```

------



## 二、`forEach()` 接收 Lambda

```java
users.forEach(
    user -> System.out.println(user)
);
```

`forEach()` 的意思就是：对集合里的每一个元素，执行提供的操作

------



## 三、Lambda 不能凭空存在

Java 不能随便写：

```java
user -> System.out.println(user);
```

然后什么都不接。

Lambda 必须被放在一个：**需要某种函数式接口的位置**，例如：

```java
users.forEach(...)
```

`forEach()` 本身要求：“你给我一个处理元素的方法”，于是：

```java
users.forEach(
    user -> System.out.println(user)
);
```

正好满足要求。

------



## 四、函数式接口

因为 Lambda 表达式**只能用在函数式接口上**

------

**函数式接口** = 只有一个抽象方法的接口

- 首先它**必须是接口**（`interface`）
- 其次这个接口里**只能有一个**没有实现的方法（抽象方法）
- 可以有很多默认方法（`default`）或静态方法，但**抽象方法只能有 1 个**

✅ **是函数式接口**

```java
@FunctionalInterface  // 这个注解是可选的，但加上可以检查
interface MyRunnable {
    void run();  // 只有一个抽象方法
}
```

❌ **不是函数式接口**（抽象方法多于1个）

```java
interface WrongInterface {
    void method1();
    void method2();  // 两个抽象方法 → 不是函数式接口
}
```

✅ **是函数式接口**（虽然有多个方法，但只有1个抽象方法）

```java
interface MyInterface {
    void run();  // 唯一的抽象方法
    
    default void log() {   // 默认方法不算
        System.out.println("log");
    }
    
    static void help() {   // 静态方法不算
        System.out.println("help");
    }
}
```

------

### Java定义好的常见函数式接口

| 接口名            | 抽象方法            | 参数     | 返回值   | 用途                 | Lambda 写法示例              |
| :---------------- | :------------------ | -------- | -------- | :------------------- | :--------------------------- |
| **Consumer<T>**   | `void accept(T t)`  | 1个（T） | 无       | 消费一个数据，无返回 | `s -> System.out.println(s)` |
| **Supplier<T>**   | `T get()`           | 无       | 1个（T） | 提供一个数据，无参数 | `() -> "Hello"`              |
| **Function<T,R>** | `R apply(T t)`      | 1个（T） | 1个（R） | 传入 T，返回 R       | `s -> s.length()`            |
| **Predicate<T>**  | `boolean test(T t)` | 1个（T） | boolean  | 判断真假             | `s -> s.isEmpty()`           |

------



## 五、看 Lambda 代码的 “三步读法”

当看到一行 Lambda 代码时，按这三步读：

1. **看左边**：参数有几个？分别是什么类型？
2. **看右边**：方法体做了什么？
3. **看上下文**：这个方法属于哪个接口？

例子：

```Java
names.forEach(name -> System.out.println(name));
```

1. **左边**：`name` 是 1 个参数
2. **右边**：打印这个参数
3. **上下文**：`forEach` 接收 `Consumer` 接口
