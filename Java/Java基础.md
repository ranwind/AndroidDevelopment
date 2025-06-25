## **一、Java基础篇**

### 1. 开发环境搭建
#### 1.1 JDK安装与环境变量配置
**概念**：  
JDK（Java Development Kit）是Java开发工具包，包含编译器（javac）、运行时环境（JRE）和核心类库。

**步骤**：  
1. 从Oracle官网下载JDK（推荐JDK 11或17）  
2. 安装后配置环境变量：  
   - `JAVA_HOME`：指向JDK安装路径（如`C:\Program Files\Java\jdk-17`）  
   - `Path`：添加`%JAVA_HOME%\bin`  

**验证安装**：  
```bash
java -version  # 显示版本号即成功
```

**注意事项**：  
- 确保安装路径无空格或中文  
- 区分JDK（开发）和JRE（仅运行）

---

#### 1.2 IDE使用（IntelliJ IDEA）
**示例**：创建第一个Java程序  
1. 新建项目 → 选择Java → 设置JDK路径  
2. 创建类`HelloWorld.java`：  
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
3. 右键运行 → 控制台输出结果  

**快捷键**：  
- `psvm`：快速生成`main`方法  
- `sout`：快速生成`System.out.println()`

---

### 2. 基础语法
#### 2.1 数据类型与变量
**基本类型**（8种）：  
| 类型    | 大小   | 示例           |
|---------|--------|----------------|
| `int`   | 4字节  | `int age = 25;`|
| `double`| 8字节  | `double pi = 3.14;` |
| `boolean` | 1位  | `boolean isTrue = true;` |

**引用类型**：  
```java
String name = "Alice";  // 字符串
int[] arr = new int[5];  // 数组
```

**注意事项**：  
- 基本类型直接存值，引用类型存地址  
- `String`不可变，修改需新建对象  

---

#### 2.2 运算符与表达式
**算术运算符**：  
```java
int a = 10 / 3;  // 结果为3（整数除法）
double b = 10.0 / 3;  // 结果为3.333...
```

**逻辑运算符**：  
```java
boolean result = (a > 5) && (b < 4);  // 逻辑与
```

**三目运算符**：  
```java
String msg = (age >= 18) ? "成人" : "未成年";
```
以下是Java中所有运算符的详细分类表格，涵盖算术、关系、逻辑、位运算、赋值等类别，并附上示例说明：

---

#### **Java运算符分类表**

| **类别**       | **运算符**       | **符号**          | **描述**                                                                 | **示例**                          |
|----------------|------------------|-------------------|--------------------------------------------------------------------------|-----------------------------------|
| **算术运算符** | 加法             | `+`               | 数值相加或字符串连接                                                      | `int a = 5 + 3;` → `8`            |
|                | 减法             | `-`               | 数值相减                                                                  | `int b = 5 - 3;` → `2`            |
|                | 乘法             | `*`               | 数值相乘                                                                  | `int c = 5 * 3;` → `15`           |
|                | 除法             | `/`               | 数值相除（整数除法会截断小数）                                            | `int d = 5 / 3;` → `1`            |
|                | 取模             | `%`               | 返回除法的余数                                                            | `int e = 5 % 3;` → `2`            |
|                | 自增             | `++`              | 变量值加1（前缀/后缀）                                                   | `int f = 5; f++;` → `6`           |
|                | 自减             | `--`              | 变量值减1（前缀/后缀）                                                   | `int g = 5; g--;` → `4`           |
| **关系运算符** | 等于             | `==`              | 判断两值是否相等                                                          | `5 == 3` → `false`                |
|                | 不等于           | `!=`              | 判断两值是否不等                                                          | `5 != 3` → `true`                 |
|                | 大于             | `>`               | 判断左值是否大于右值                                                      | `5 > 3` → `true`                  |
|                | 小于             | `<`               | 判断左值是否小于右值                                                      | `5 < 3` → `false`                 |
|                | 大于等于         | `>=`              | 判断左值是否大于或等于右值                                                | `5 >= 5` → `true`                 |
|                | 小于等于         | `<=`              | 判断左值是否小于或等于右值                                                | `5 <= 3` → `false`                |
| **逻辑运算符** | 逻辑与           | `&&`              | 两条件均为真时返回真（短路）                                              | `true && false` → `false`         |
|                | 逻辑或           | `\|\|`            | 任一条件为真时返回真（短路）                                              | `true \|\| false` → `true`        |
|                | 逻辑非           | `!`               | 反转条件结果                                                              | `!true` → `false`                 |
| **位运算符**   | 按位与           | `&`               | 对二进制位进行与操作                                                      | `5 & 3` → `1`（`0101 & 0011`）    |
|                | 按位或           | `\|`              | 对二进制位进行或操作                                                      | `5 \| 3` → `7`（`0101 \| 0011`）  |
|                | 按位异或         | `^`               | 对二进制位进行异或操作（相同为0，不同为1）                                 | `5 ^ 3` → `6`（`0101 ^ 0011`）    |
|                | 按位取反         | `~`               | 对二进制位取反（一元运算符）                                              | `~5` → `-6`（补码表示）            |
|                | 左移             | `<<`              | 左移指定位数（低位补0）                                                   | `5 << 1` → `10`（`0101`→`1010`）  |
|                | 右移             | `>>`              | 右移指定位数（高位补符号位）                                              | `-5 >> 1` → `-3`（保留符号）       |
|                | 无符号右移       | `>>>`             | 右移指定位数（高位补0）                                                   | `-5 >>> 1` → `2147483645`         |
| **赋值运算符** | 简单赋值         | `=`               | 将右值赋给左变量                                                          | `int a = 5;`                      |
|                | 复合赋值         | `+=`, `-=`, `*=`  | 先运算再赋值（如 `a += b` 等价于 `a = a + b`）                            | `a += 3;` → `a = a + 3`           |
|                |                  | `/=`, `%=`, `&=`  | 支持所有算术和位运算符                                                     | `a &= 3;` → `a = a & 3`           |
|                |                  | `^=`, `\|=`, `<<=` |                                                                          | `a <<= 1;` → `a = a << 1`         |
| **条件运算符** | 三元运算符       | `? :`             | 根据条件选择返回值                                                        | `int max = (a > b) ? a : b;`      |
| **其他运算符** | 实例检查         | `instanceof`      | 检查对象是否属于某类或其子类                                              | `obj instanceof String`           |
|                | 字符串连接       | `+`               | 连接字符串（若操作数为非字符串，自动调用 `toString()`）                    | `"Hello " + 123` → `"Hello 123"`  |

---

##### **关键说明**
1. **短路行为**：  
   - `&&` 和 `||` 会短路（若左操作数可确定结果，右操作数不执行）。  
   - `&` 和 `|` 是非短路的（始终计算两侧操作数）。

2. **运算符优先级**：  
   - 括号 `()` > 自增/自减 `++` `--` > 算术 `*` `/` `%` > 加减 `+` `-` > 移位 `<<` `>>` > 关系 `>` `<` `==` > 位运算 `&` `^` `|` > 逻辑 `&&` `||` > 三元 `? :` > 赋值 `=` `+=` 等。

3. **特殊用例**：  
   - `+` 在字符串上下文自动转为连接操作：  
     ```java
     System.out.println(1 + 2 + "3");   // 输出 "33"（先计算1+2，再连接"3"）
     System.out.println("1" + 2 + 3);   // 输出 "123"（从左到右连接）
     ```



---

#### 2.3 控制结构
**if-else**：  
```java
if (score >= 90) {
    System.out.println("优秀");
} else if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}
```

**for循环**：  
```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);  // 输出0到4
}
```

**注意事项**：  
- `switch`支持`String`（JDK7+）  
- 避免死循环（如`while(true)`需有退出条件）

---

### 3. 面向对象编程（OOP）
#### 3.1 类与对象
**定义类**：  
```java
public class Person {
    // 字段（属性）
    String name;
    int age;

    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 方法
    public void sayHello() {
        System.out.println("Hi, I'm " + name);
    }
}
```

**实例化对象**：  
```java
Person p = new Person("Bob", 30);
p.sayHello();  // 调用方法
```

**注意事项**：  
- 类名首字母大写，遵循驼峰命名法  
- 构造方法无返回值类型  

---

#### 3.2 封装、继承、多态
**封装**（隐藏细节）：  
```java
private String name;  // 私有字段

public String getName() {  // Getter
    return name;
}

public void setName(String name) {  // Setter
    this.name = name;
}
```

**继承**（复用代码）：  
```java
public class Student extends Person {
    private String school;

    public Student(String name, int age, String school) {
        super(name, age);  // 调用父类构造方法
        this.school = school;
    }
}
```

**多态**（同一行为不同表现）：  
```java
Person p = new Student("Alice", 20, "Harvard");
p.sayHello();  // 实际调用Student的方法（若重写）
```

---

#### 3.3 抽象类与接口
**抽象类**：  
```java
public abstract class Animal {
    public abstract void eat();  // 抽象方法
}

public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("Dog eats bones");
    }
}
```

**接口**：  
```java
public interface Flyable {
    void fly();  // 默认public abstract
}

public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird flies");
    }
}
```

**区别**：  
- 抽象类可包含具体方法，接口只能有抽象方法（JDK8前）  
- 类单继承，接口可多实现  

---

### 4. 静态成员与内部类
**静态变量**（类共享）：  
```java
public class Counter {
    static int count = 0;  // 所有实例共享

    public Counter() {
        count++;
    }
}
```

**内部类**：  
```java
public class Outer {
    private String msg = "Hello";

    class Inner {
        void print() {
            System.out.println(msg);  // 访问外部类成员
        }
    }
}
```

**注意事项**：  
- 静态方法不能访问非静态成员  
- 匿名内部类常用于事件监听  

---

### 总结表格
| 主题               | 关键点                              | 示例                          |
|--------------------|-----------------------------------|-------------------------------|
| 数据类型           | 基本类型8种，引用类型存地址          | `int a = 10; String s = "Hi";`|
| 继承               | `extends`单继承，`super`调用父类     | `class Student extends Person`|
| 接口               | 多实现，定义行为规范                 | `class Bird implements Flyable`|