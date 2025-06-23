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