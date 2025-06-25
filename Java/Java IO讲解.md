# **Java 输入输出（I/O）全面详解**

Java 的输入输出（I/O）系统是 Java 编程中非常重要的部分，涉及文件操作、流处理、序列化、网络通信等。本文将全面讲解 Java I/O 的核心概念、类库、使用方法和最佳实践。

---

## **1. Java I/O 概述**
Java I/O 主要分为：
- **字节流（Byte Streams）**：处理二进制数据（`InputStream` / `OutputStream`）
- **字符流（Character Streams）**：处理文本数据（`Reader` / `Writer`）
- **缓冲流（Buffered Streams）**：提高 I/O 性能
- **文件操作（File I/O）**：`File` 和 `Path` 类
- **对象序列化（Object Serialization）**：`ObjectInputStream` / `ObjectOutputStream`
- **NIO（New I/O）**：非阻塞 I/O（`java.nio` 包）

---

## **2. 字节流（Byte Streams）**
字节流用于处理二进制数据（如图片、音频、视频等），核心类是 `InputStream` 和 `OutputStream`。

### **2.1 `InputStream`（输入流）**
用于从数据源（文件、网络、内存等）读取字节数据。

#### **常用子类**
| 类名 | 描述 |
|------|------|
| `FileInputStream` | 从文件读取字节 |
| `ByteArrayInputStream` | 从字节数组读取 |
| `BufferedInputStream` | 带缓冲的输入流 |
| `ObjectInputStream` | 反序列化对象 |

#### **示例：读取文件**
```java
try (InputStream input = new FileInputStream("test.txt")) {
    int data;
    while ((data = input.read()) != -1) {  // 逐字节读取
        System.out.print((char) data);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### **2.2 `OutputStream`（输出流）**
用于向目标（文件、网络、内存等）写入字节数据。

#### **常用子类**
| 类名 | 描述 |
|------|------|
| `FileOutputStream` | 写入字节到文件 |
| `ByteArrayOutputStream` | 写入字节到数组 |
| `BufferedOutputStream` | 带缓冲的输出流 |
| `ObjectOutputStream` | 序列化对象 |

#### **示例：写入文件**
```java
try (OutputStream output = new FileOutputStream("output.txt")) {
    String text = "Hello, Java I/O!";
    byte[] bytes = text.getBytes();
    output.write(bytes);  // 写入字节数组
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## **3. 字符流（Character Streams）**
字符流用于处理文本数据（如 `.txt`、`.csv` 文件），核心类是 `Reader` 和 `Writer`。

### **3.1 `Reader`（字符输入流）**
用于读取字符数据。

#### **常用子类**
| 类名 | 描述 |
|------|------|
| `FileReader` | 从文件读取字符 |
| `BufferedReader` | 带缓冲的字符输入流 |
| `InputStreamReader` | 字节流 → 字符流转换 |

#### **示例：逐行读取文件**
```java
try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {  // 逐行读取
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### **3.2 `Writer`（字符输出流）**
用于写入字符数据。

#### **常用子类**
| 类名 | 描述 |
|------|------|
| `FileWriter` | 写入字符到文件 |
| `BufferedWriter` | 带缓冲的字符输出流 |
| `OutputStreamWriter` | 字符流 → 字节流转换 |

#### **示例：写入文件**
```java
try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
    writer.write("Hello, Java I/O!");  // 写入字符串
    writer.newLine();  // 换行
    writer.write("第二行");
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## **4. 缓冲流（Buffered Streams）**
缓冲流（`BufferedInputStream` / `BufferedOutputStream` / `BufferedReader` / `BufferedWriter`）通过减少底层 I/O 操作次数提高性能。

### **示例：使用缓冲流**
```java
// 字节缓冲流
try (
    BufferedInputStream bis = new BufferedInputStream(new FileInputStream("input.jpg"));
    BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.jpg"))
) {
    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = bis.read(buffer)) != -1) {
        bos.write(buffer, 0, bytesRead);  // 批量读写
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## **5. 文件操作（File & Path）**
Java 提供了 `File` 和 `Path` 类来操作文件系统。

### **5.1 `File` 类（传统方式）**
```java
File file = new File("test.txt");
System.out.println("文件是否存在？ " + file.exists());
System.out.println("文件大小：" + file.length() + " bytes");
System.out.println("绝对路径：" + file.getAbsolutePath());
```

### **5.2 `Path` 类（NIO 方式，推荐）**
```java
Path path = Paths.get("test.txt");
System.out.println("文件名：" + path.getFileName());
System.out.println("父目录：" + path.getParent());
Files.copy(path, Paths.get("copy.txt"));  // 复制文件
```

---

## **6. 对象序列化（Object Serialization）**
Java 对象可以序列化为字节流，以便存储或传输。

### **6.1 序列化对象**
```java
class Person implements Serializable {
    String name;
    int age;
}

try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
    Person person = new Person();
    person.name = "Alice";
    person.age = 25;
    oos.writeObject(person);  // 序列化
} catch (IOException e) {
    e.printStackTrace();
}
```

### **6.2 反序列化对象**
```java
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
    Person person = (Person) ois.readObject();  // 反序列化
    System.out.println(person.name + ", " + person.age);
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

---

## **7. NIO（New I/O）**
Java NIO 提供了非阻塞 I/O 和更高效的文件操作。

### **7.1 `Files` 类**
```java
Path path = Paths.get("test.txt");
byte[] data = Files.readAllBytes(path);  // 读取所有字节
List<String> lines = Files.readAllLines(path);  // 读取所有行
Files.write(path, "New content".getBytes());  // 写入文件
```

### **7.2 `Channels` 和 `Buffers`**
```java
try (RandomAccessFile file = new RandomAccessFile("test.txt", "rw");
     FileChannel channel = file.getChannel()) {
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    int bytesRead = channel.read(buffer);  // 读取到缓冲区
    buffer.flip();  // 切换为读模式
    while (buffer.hasRemaining()) {
        System.out.print((char) buffer.get());
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## **8. 总结**
| **类别** | **核心类** | **用途** |
|---------|-----------|---------|
| **字节流** | `InputStream` / `OutputStream` | 处理二进制数据 |
| **字符流** | `Reader` / `Writer` | 处理文本数据 |
| **缓冲流** | `BufferedInputStream` / `BufferedReader` | 提高 I/O 性能 |
| **文件操作** | `File` / `Path` | 文件系统操作 |
| **对象序列化** | `ObjectInputStream` / `ObjectOutputStream` | 对象存储与传输 |
| **NIO** | `Files` / `Path` / `Channel` / `Buffer` | 高效 I/O 操作 |

---

### **最佳实践**
1. **使用 `try-with-resources`** 自动关闭流（避免资源泄漏）。
2. **优先使用 `Buffered Streams`** 提高性能。
3. **文本数据用 `Reader/Writer`**，二进制数据用 `InputStream/OutputStream`。
4. **新项目推荐 `NIO`**（`Path` 和 `Files` 比 `File` 更强大）。

希望这份指南能帮助你全面掌握 Java I/O！如果有具体问题，欢迎继续讨论。 🚀