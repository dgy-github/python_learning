# 第七课：异常处理与文件I/O

**课时：2小时 | 难度：★★★☆☆**

------

## 🎯 本课学习目标

- ✅ 理解Python和Java的异常处理机制差异
- ✅ 掌握异常捕获、抛出和自定义异常
- ✅ 熟练使用文件读写操作
- ✅ 理解资源管理的最佳实践
- ✅ 掌握序列化与反序列化技术

------

## 📖 课程内容

### ⚠️ 7.1 异常处理机制（40分钟）

#### 7.1.1 异常体系对比（10分钟）

**Python异常层次结构**

```php
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── ValueError
    ├── TypeError
    ├── IOError/OSError
    └── ...
```

**Java异常层次结构**

```php
Throwable
├── Error (不应捕获)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── ...
└── Exception
    ├── RuntimeException (未检查异常)
    │   ├── NullPointerException
    │   ├── IndexOutOfBoundsException
    │   ├── ArithmeticException
    │   └── IllegalArgumentException
    └── IOException (检查异常)
        ├── FileNotFoundException
        └── ...
```

**关键差异：**

- Python：所有异常都是未检查异常
- Java：分为检查异常（Checked）和未检查异常（Unchecked）
- Java的检查异常必须显式处理或声明

------

#### 7.1.2 异常捕获与处理（15分钟）

**基本语法对比**

**Python:**

```python
# 基本异常处理
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"错误: {e}")
except ValueError as e:
    print(f"值错误: {e}")
except Exception as e:
    print(f"未知错误: {e}")
else:
    print("没有异常发生")
finally:
    print("清理资源")

# 捕获多个异常
try:
    # 可能抛出多种异常的代码
    value = int(input("输入数字: "))
    result = 100 / value
except (ValueError, ZeroDivisionError) as e:
    print(f"输入错误或除零: {e}")

# 获取异常信息
import traceback
try:
    # 危险操作
    risky_operation()
except Exception as e:
    print(f"异常类型: {type(e).__name__}")
    print(f"异常信息: {str(e)}")
    print(f"堆栈跟踪:\n{traceback.format_exc()}")
```

**Java:**

```java
// 基本异常处理
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("错误: " + e.getMessage());
} catch (Exception e) {
    System.out.println("未知错误: " + e.getMessage());
} finally {
    System.out.println("清理资源");
}

// 捕获多个异常 (Java 7+)
try {
    String input = scanner.nextLine();
    int value = Integer.parseInt(input);
    int result = 100 / value;
} catch (NumberFormatException | ArithmeticException e) {
    System.out.println("输入错误或除零: " + e.getMessage());
}

// 获取异常信息
try {
    riskyOperation();
} catch (Exception e) {
    System.out.println("异常类型: " + e.getClass().getName());
    System.out.println("异常信息: " + e.getMessage());
    System.out.println("堆栈跟踪:");
    e.printStackTrace();
}
```

**对比要点：**

| 特性         | Python                    | Java                |
| ------------ | ------------------------- | ------------------- |
| 语法关键字   | `try-except-else-finally` | `try-catch-finally` |
| 多异常捕获   | `except (E1, E2)`         | `catch (E1 | E2)`   |
| 异常对象     | `as e`                    | 直接在括号内        |
| else子句     | ✅ 支持                    | ❌ 不支持            |
| 异常必须处理 | ❌ 否                      | ✅ 检查异常必须      |

------

#### 7.1.3 抛出异常（8分钟）

**Python:**

```python
# 抛出异常
def divide(a, b):
    if b == 0:
        raise ValueError("除数不能为零")
    return a / b

# 重新抛出异常
def process_data(data):
    try:
        result = risky_operation(data)
    except ValueError as e:
        print(f"处理失败: {e}")
        raise  # 重新抛出原异常
    except Exception as e:
        # 抛出新异常，保留原异常信息
        raise RuntimeError("处理数据时出错") from e

# 异常链
try:
    process_data(None)
except RuntimeError as e:
    print(f"当前异常: {e}")
    print(f"原始异常: {e.__cause__}")
```

**Java:**

```java
// 抛出异常
public double divide(int a, int b) throws ArithmeticException {
    if (b == 0) {
        throw new ArithmeticException("除数不能为零");
    }
    return (double) a / b;
}

// 检查异常必须声明
public void readFile(String path) throws IOException {
    FileReader reader = new FileReader(path);
    // ...
}

// 重新抛出异常
public void processData(String data) throws Exception {
    try {
        riskyOperation(data);
    } catch (IllegalArgumentException e) {
        System.out.println("处理失败: " + e.getMessage());
        throw e;  // 重新抛出
    } catch (Exception e) {
        // 异常链
        throw new RuntimeException("处理数据时出错", e);
    }
}

// 获取异常链
try {
    processData(null);
} catch (RuntimeException e) {
    System.out.println("当前异常: " + e.getMessage());
    System.out.println("原始异常: " + e.getCause());
}
```

**对比要点：**

- Python: `raise` 关键字，不需要声明
- Java: `throw` 关键字，检查异常需要 `throws` 声明
- 异常链：Python用 `from`，Java用构造函数参数

------

#### 7.1.4 自定义异常（7分钟）

**Python:**

```python
# 基本自定义异常
class CustomError(Exception):
    """自定义异常基类"""
    pass

class ValidationError(CustomError):
    """验证错误"""
    def __init__(self, field, message):
        self.field = field
        self.message = message
        super().__init__(f"{field}: {message}")

class DatabaseError(CustomError):
    """数据库错误"""
    def __init__(self, operation, details):
        self.operation = operation
        self.details = details
        super().__init__(f"数据库{operation}失败: {details}")

# 使用自定义异常
def validate_age(age):
    if not isinstance(age, int):
        raise ValidationError("age", "必须是整数")
    if age < 0 or age > 150:
        raise ValidationError("age", "年龄必须在0-150之间")
    return True

# 捕获自定义异常
try:
    validate_age(-5)
except ValidationError as e:
    print(f"验证失败 - 字段: {e.field}, 原因: {e.message}")
except CustomError as e:
    print(f"自定义错误: {e}")
```

**Java:**

```java
// 基本自定义异常
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
    
    public CustomException(String message, Throwable cause) {
        super(message, cause);
    }
}

// 验证异常（检查异常）
public class ValidationException extends Exception {
    private String field;
    
    public ValidationException(String field, String message) {
        super(field + ": " + message);
        this.field = field;
    }
    
    public String getField() {
        return field;
    }
}

// 数据库异常（运行时异常）
public class DatabaseException extends RuntimeException {
    private String operation;
    
    public DatabaseException(String operation, String details) {
        super("数据库" + operation + "失败: " + details);
        this.operation = operation;
    }
    
    public String getOperation() {
        return operation;
    }
}

// 使用自定义异常
public boolean validateAge(int age) throws ValidationException {
    if (age < 0 || age > 150) {
        throw new ValidationException("age", "年龄必须在0-150之间");
    }
    return true;
}

// 捕获自定义异常
try {
    validateAge(-5);
} catch (ValidationException e) {
    System.out.println("验证失败 - 字段: " + e.getField() + 
                      ", 原因: " + e.getMessage());
}
```

**设计建议：**

- ✅ 继承合适的基类（Exception vs RuntimeException）
- ✅ 提供有意义的错误信息
- ✅ 添加必要的上下文信息（字段、操作类型等）
- ✅ 支持异常链
- ❌ 不要过度使用自定义异常

------

### 📁 7.2 文件操作基础（35分钟）

#### 7.2.1 文本文件读写（12分钟）

**Python:**

```python
# 读取文件 - 方式1: 自动关闭
with open('example.txt', 'r', encoding='utf-8') as f:
    content = f.read()  # 读取全部内容
    print(content)

# 读取文件 - 方式2: 逐行读取
with open('example.txt', 'r', encoding='utf-8') as f:
    for line in f:
        print(line.strip())  # strip()去除换行符

# 读取文件 - 方式3: 读取所有行到列表
with open('example.txt', 'r', encoding='utf-8') as f:
    lines = f.readlines()
    print(lines)

# 写入文件 - 覆盖模式
with open('output.txt', 'w', encoding='utf-8') as f:
    f.write("第一行\n")
    f.write("第二行\n")

# 写入文件 - 追加模式
with open('output.txt', 'a', encoding='utf-8') as f:
    f.write("追加的内容\n")

# 写入多行
lines = ["行1\n", "行2\n", "行3\n"]
with open('output.txt', 'w', encoding='utf-8') as f:
    f.writelines(lines)

# 传统方式（不推荐）
f = open('example.txt', 'r')
try:
    content = f.read()
finally:
    f.close()
```

**Java:**

```java
import java.io.*;
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.util.List;

// 读取文件 - 方式1: 读取全部内容 (Java 11+)
try {
    String content = Files.readString(Path.of("example.txt"), 
                                     StandardCharsets.UTF_8);
    System.out.println(content);
} catch (IOException e) {
    e.printStackTrace();
}

// 读取文件 - 方式2: 读取所有行
try {
    List<String> lines = Files.readAllLines(Path.of("example.txt"),
                                           StandardCharsets.UTF_8);
    for (String line : lines) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 读取文件 - 方式3: 使用BufferedReader
try (BufferedReader reader = new BufferedReader(
        new FileReader("example.txt", StandardCharsets.UTF_8))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 写入文件 - 覆盖模式 (Java 11+)
try {
    Files.writeString(Path.of("output.txt"), 
                     "第一行\n第二行\n",
                     StandardCharsets.UTF_8);
} catch (IOException e) {
    e.printStackTrace();
}

// 写入文件 - 追加模式
try {
    Files.writeString(Path.of("output.txt"),
                     "追加的内容\n",
                     StandardCharsets.UTF_8,
                     StandardOpenOption.APPEND);
} catch (IOException e) {
    e.printStackTrace();
}

// 写入多行
List<String> lines = List.of("行1", "行2", "行3");
try {
    Files.write(Path.of("output.txt"), lines, StandardCharsets.UTF_8);
} catch (IOException e) {
    e.printStackTrace();
}

// 使用BufferedWriter
try (BufferedWriter writer = new BufferedWriter(
        new FileWriter("output.txt", StandardCharsets.UTF_8))) {
    writer.write("第一行");
    writer.newLine();
    writer.write("第二行");
    writer.newLine();
} catch (IOException e) {
    e.printStackTrace();
}
```

**文件模式对比：**

| 模式         | Python | Java                             | 说明   |
| ------------ | ------ | -------------------------------- | ------ |
| 读取         | `'r'`  | `StandardOpenOption.READ`        | 只读   |
| 写入（覆盖） | `'w'`  | `StandardOpenOption.WRITE`       | 覆盖   |
| 追加         | `'a'`  | `StandardOpenOption.APPEND`      | 追加   |
| 读写         | `'r+'` | `StandardOpenOption.READ, WRITE` | 读写   |
| 二进制读     | `'rb'` | 使用InputStream                  | 二进制 |
| 二进制写     | `'wb'` | 使用OutputStream                 | 二进制 |

------

#### 7.2.2 二进制文件操作（8分钟）

**Python:**

```python
# 读取二进制文件
with open('image.png', 'rb') as f:
    data = f.read()
    print(f"文件大小: {len(data)} 字节")

# 写入二进制文件
with open('copy.png', 'wb') as f:
    f.write(data)

# 分块读取大文件
def copy_large_file(src, dst, chunk_size=8192):
    with open(src, 'rb') as f_in:
        with open(dst, 'wb') as f_out:
            while True:
                chunk = f_in.read(chunk_size)
                if not chunk:
                    break
                f_out.write(chunk)

# 使用struct处理二进制数据
import struct

# 写入二进制数据
with open('data.bin', 'wb') as f:
    # 写入一个整数和一个浮点数
    data = struct.pack('if', 42, 3.14)
    f.write(data)

# 读取二进制数据
with open('data.bin', 'rb') as f:
    data = f.read()
    integer, float_num = struct.unpack('if', data)
    print(f"整数: {integer}, 浮点数: {float_num}")
```

**Java:**

```java
import java.io.*;
import java.nio.file.*;

// 读取二进制文件
try {
    byte[] data = Files.readAllBytes(Path.of("image.png"));
    System.out.println("文件大小: " + data.length + " 字节");
} catch (IOException e) {
    e.printStackTrace();
}

// 写入二进制文件
try {
    Files.write(Path.of("copy.png"), data);
} catch (IOException e) {
    e.printStackTrace();
}

// 分块读取大文件
public static void copyLargeFile(String src, String dst) {
    try (InputStream in = new FileInputStream(src);
         OutputStream out = new FileOutputStream(dst)) {
        
        byte[] buffer = new byte[8192];
        int bytesRead;
        while ((bytesRead = in.read(buffer)) != -1) {
            out.write(buffer, 0, bytesRead);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// 使用DataOutputStream/DataInputStream处理二进制数据
// 写入二进制数据
try (DataOutputStream dos = new DataOutputStream(
        new FileOutputStream("data.bin"))) {
    dos.writeInt(42);
    dos.writeFloat(3.14f);
} catch (IOException e) {
    e.printStackTrace();
}

// 读取二进制数据
try (DataInputStream dis = new DataInputStream(
        new FileInputStream("data.bin"))) {
    int integer = dis.readInt();
    float floatNum = dis.readFloat();
    System.out.println("整数: " + integer + ", 浮点数: " + floatNum);
} catch (IOException e) {
    e.printStackTrace();
}

// 使用ByteBuffer (NIO)
import java.nio.ByteBuffer;

ByteBuffer buffer = ByteBuffer.allocate(8);
buffer.putInt(42);
buffer.putFloat(3.14f);
byte[] data = buffer.array();
```

------

#### 7.2.3 资源管理最佳实践（8分钟）

**Python - 上下文管理器（Context Manager）**

```python
# with语句自动管理资源
with open('file.txt', 'r') as f:
    content = f.read()
# 文件自动关闭，即使发生异常

# 同时打开多个文件
with open('input.txt', 'r') as f_in, \
     open('output.txt', 'w') as f_out:
    for line in f_in:
        f_out.write(line.upper())

# 自定义上下文管理器
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        # 返回False表示不抑制异常
        return False

# 使用自定义上下文管理器
with FileManager('test.txt', 'w') as f:
    f.write("测试内容")

# 使用contextlib简化
from contextlib import contextmanager

@contextmanager
def managed_file(filename, mode):
    try:
        f = open(filename, mode)
        yield f
    finally:
        f.close()

with managed_file('test.txt', 'r') as f:
    print(f.read())
```

**Java - try-with-resources**

```java
// try-with-resources 自动管理资源 (Java 7+)
try (BufferedReader reader = new BufferedReader(
        new FileReader("file.txt"))) {
    String content = reader.readLine();
} // reader自动关闭，即使发生异常

// 同时管理多个资源
try (BufferedReader reader = new BufferedReader(
        new FileReader("input.txt"));
     BufferedWriter writer = new BufferedWriter(
        new FileWriter("output.txt"))) {
    
    String line;
    while ((line = reader.readLine()) != null) {
        writer.write(line.toUpperCase());
        writer.newLine();
    }
}

// 自定义可关闭资源
public class ManagedResource implements AutoCloseable {
    private String name;
    
    public ManagedResource(String name) {
        this.name = name;
        System.out.println("打开资源: " + name);
    }
    
    public void doSomething() {
        System.out.println("使用资源: " + name);
    }
    
    @Override
    public void close() {
        System.out.println("关闭资源: " + name);
    }
}

// 使用自定义资源
try (ManagedResource resource = new ManagedResource("MyResource")) {
    resource.doSomething();
} // 自动调用close()

// Java 9+ 改进：可以使用final或effectively final变量
BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
try (reader) {
    // 使用reader
}
```

**对比总结：**

| 特性      | Python                   | Java                 |
| --------- | ------------------------ | -------------------- |
| 语法      | `with` 语句              | `try-with-resources` |
| 协议/接口 | `__enter__` / `__exit__` | `AutoCloseable`      |
| 多资源    | 逗号分隔                 | 分号分隔             |
| 异常处理  | 在`__exit__`中           | 自动处理             |
| 引入版本  | Python 2.5               | Java 7               |

------

#### 7.2.4 CSV和JSON文件处理（7分钟）

**Python:**

```python
import csv
import json

# ========== CSV操作 ==========
# 写入CSV
data = [
    ['姓名', '年龄', '城市'],
    ['张三', 25, '北京'],
    ['李四', 30, '上海']
]

with open('data.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(data)

# 读取CSV
with open('data.csv', 'r', encoding='utf-8') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# 使用字典读写CSV
with open('data.csv', 'w', newline='', encoding='utf-8') as f:
    fieldnames = ['姓名', '年龄', '城市']
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({'姓名': '张三', '年龄': 25, '城市': '北京'})

with open('data.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row['姓名'], row['年龄'])

# ========== JSON操作 ==========
# 写入JSON
data = {
    'name': '张三',
    'age': 25,
    'skills': ['Python', 'Java'],
    'address': {'city': '北京', 'district': '朝阳'}
}

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# 读取JSON
with open('data.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
    print(data)

# JSON字符串转换
json_str = json.dumps(data, ensure_ascii=False, indent=2)
data = json.loads(json_str)
```

**Java:**

```java
import java.io.*;
import java.nio.file.*;
import com.opencsv.*;  // 需要添加依赖
import com.google.gson.*;  // 需要添加依赖

// ========== CSV操作 (使用OpenCSV) ==========
// 写入CSV
try (CSVWriter writer = new CSVWriter(
        new FileWriter("data.csv"))) {
    String[] header = {"姓名", "年龄", "城市"};
    writer.writeNext(header);
    
    String[] row1 = {"张三", "25", "北京"};
    String[] row2 = {"李四", "30", "上海"};
    writer.writeNext(row1);
    writer.writeNext(row2);
} catch (IOException e) {
    e.printStackTrace();
}

// 读取CSV
try (CSVReader reader = new CSVReader(
        new FileReader("data.csv"))) {
    String[] line;
    while ((line = reader.readNext()) != null) {
        System.out.println(String.join(", ", line));
    }
} catch (IOException | CsvValidationException e) {
    e.printStackTrace();
}

// ========== JSON操作 (使用Gson) ==========
// 定义数据类
class Person {
    String name;
    int age;
    List<String> skills;
    Address address;
    
    static class Address {
        String city;
        String district;
    }
}

// 写入JSON
Gson gson = new GsonBuilder()
    .setPrettyPrinting()
    .create();

Person person = new Person();
person.name = "张三";
person.age = 25;
person.skills = List.of("Python", "Java");
person.address = new Person.Address();
person.address.city = "北京";
person.address.district = "朝阳";

try (Writer writer = new FileWriter("data.json")) {
    gson.toJson(person, writer);
} catch (IOException e) {
    e.printStackTrace();
}

// 读取JSON
try (Reader reader = new FileReader("data.json")) {
    Person person = gson.fromJson(reader, Person.class);
    System.out.println(person.name);
} catch (IOException e) {
    e.printStackTrace();
}

// JSON字符串转换
String jsonStr = gson.toJson(person);
Person person = gson.fromJson(jsonStr, Person.class);
```

------

### 📂 7.3 路径与目录操作（25分钟）

#### 7.3.1 路径操作（10分钟）

**Python - pathlib模块（推荐）**

```python
from pathlib import Path
import os

# 创建路径对象
path = Path('data/files/example.txt')
print(path)  # data/files/example.txt

# 路径拼接
base_dir = Path('data')
file_path = base_dir / 'files' / 'example.txt'
print(file_path)  # data/files/example.txt

# 获取路径信息
print(file_path.name)        # example.txt
print(file_path.stem)        # example
print(file_path.suffix)      # .txt
print(file_path.parent)      # data/files
print(file_path.parts)       # ('data', 'files', 'example.txt')

# 绝对路径和相对路径
print(file_path.absolute())  # 绝对路径
print(file_path.resolve())   # 解析符号链接后的绝对路径

# 路径存在性检查
print(file_path.exists())    # 是否存在
print(file_path.is_file())   # 是否是文件
print(file_path.is_dir())    # 是否是目录

# 获取当前工作目录
cwd = Path.cwd()
print(cwd)

# 获取用户主目录
home = Path.home()
print(home)

# 路径匹配
path = Path('data/files/test.txt')
print(path.match('*.txt'))        # True
print(path.match('**/test.txt'))  # True

# 修改路径
new_path = path.with_name('new.txt')      # 修改文件名
new_path = path.with_suffix('.csv')       # 修改扩展名
new_path = path.with_stem('newfile')      # 修改主文件名

# 传统os.path方式（不推荐）
import os
path = os.path.join('data', 'files', 'example.txt')
dirname = os.path.dirname(path)
basename = os.path.basename(path)
```

**Java - Path和Paths类（续）**

```java
import java.nio.file.*;
import java.io.File;
import java.io.IOException;

// 获取路径信息（续）
System.out.println(path.getFileName());    // example.txt
System.out.println(path.getParent());      // data/files
System.out.println(path.getRoot());        // null (相对路径)

// 获取文件名和扩展名
String fileName = path.getFileName().toString();
String extension = "";
int dotIndex = fileName.lastIndexOf('.');
if (dotIndex > 0) {
    extension = fileName.substring(dotIndex);  // .txt
    String nameWithoutExt = fileName.substring(0, dotIndex);  // example
}

// 绝对路径和相对路径
Path absolutePath = path.toAbsolutePath();
Path realPath = path.toRealPath();  // 解析符号链接，抛出IOException

// 路径存在性检查
boolean exists = Files.exists(path);
boolean isFile = Files.isRegularFile(path);
boolean isDir = Files.isDirectory(path);

// 获取当前工作目录
Path cwd = Paths.get("").toAbsolutePath();
// 或
Path cwd = Paths.get(System.getProperty("user.dir"));

// 获取用户主目录
Path home = Paths.get(System.getProperty("user.home"));

// 路径匹配
PathMatcher matcher = FileSystems.getDefault()
    .getPathMatcher("glob:*.txt");
boolean matches = matcher.matches(path.getFileName());

// 路径解析和规范化
Path base = Paths.get("/home/user");
Path relative = Paths.get("data/file.txt");
Path resolved = base.resolve(relative);  // /home/user/data/file.txt

Path normalized = Paths.get("/home/user/../data/./file.txt").normalize();
// 结果: /home/data/file.txt

// 相对路径计算
Path path1 = Paths.get("/home/user/data");
Path path2 = Paths.get("/home/user/docs/file.txt");
Path relativePath = path1.relativize(path2);  // ../docs/file.txt

// 传统File类方式（不推荐）
File file = new File("data/files/example.txt");
String parent = file.getParent();
String name = file.getName();
```

**路径操作对比总结：**

| 操作       | Python (pathlib)  | Java (NIO.2)            |
| ---------- | ----------------- | ----------------------- |
| 创建路径   | `Path('a/b')`     | `Paths.get("a", "b")`   |
| 路径拼接   | `path / 'file'`   | `path.resolve("file")`  |
| 获取文件名 | `path.name`       | `path.getFileName()`    |
| 获取父目录 | `path.parent`     | `path.getParent()`      |
| 绝对路径   | `path.absolute()` | `path.toAbsolutePath()` |
| 检查存在   | `path.exists()`   | `Files.exists(path)`    |
| 规范化     | `path.resolve()`  | `path.normalize()`      |

------

#### 7.3.2 目录操作（8分钟）

**Python:**

```python
from pathlib import Path
import shutil
import os

# 创建目录
Path('new_dir').mkdir()                    # 创建单个目录
Path('parent/child').mkdir(parents=True)   # 创建多级目录
Path('dir').mkdir(exist_ok=True)           # 如果存在不报错

# 删除目录
Path('empty_dir').rmdir()                  # 删除空目录
shutil.rmtree('dir_with_files')            # 删除目录及其内容

# 列出目录内容
path = Path('data')

# 方式1: iterdir() - 返回迭代器
for item in path.iterdir():
    print(item)

# 方式2: glob() - 模式匹配
for txt_file in path.glob('*.txt'):
    print(txt_file)

# 递归搜索
for py_file in path.rglob('*.py'):
    print(py_file)

# 列出所有文件和目录
all_items = list(path.iterdir())
files = [f for f in path.iterdir() if f.is_file()]
dirs = [d for d in path.iterdir() if d.is_dir()]

# 重命名和移动
old_path = Path('old_name.txt')
new_path = Path('new_name.txt')
old_path.rename(new_path)

# 移动到其他目录
shutil.move('file.txt', 'target_dir/')

# 复制文件和目录
shutil.copy('source.txt', 'dest.txt')              # 复制文件
shutil.copy2('source.txt', 'dest.txt')             # 复制文件+元数据
shutil.copytree('source_dir', 'dest_dir')          # 复制目录树

# 获取目录大小
def get_dir_size(path):
    total = 0
    for item in Path(path).rglob('*'):
        if item.is_file():
            total += item.stat().st_size
    return total

size = get_dir_size('data')
print(f"目录大小: {size / 1024 / 1024:.2f} MB")

# 遍历目录树
for root, dirs, files in os.walk('data'):
    print(f"目录: {root}")
    for dir_name in dirs:
        print(f"  子目录: {dir_name}")
    for file_name in files:
        print(f"  文件: {file_name}")
```

**Java:**

```java
import java.nio.file.*;
import java.io.IOException;
import java.util.stream.Stream;

// 创建目录
try {
    Files.createDirectory(Paths.get("new_dir"));           // 创建单个目录
    Files.createDirectories(Paths.get("parent/child"));    // 创建多级目录
} catch (IOException e) {
    e.printStackTrace();
}

// 删除目录
try {
    Files.delete(Paths.get("empty_dir"));                  // 删除空目录
    
    // 删除目录及其内容
    Path dirToDelete = Paths.get("dir_with_files");
    Files.walk(dirToDelete)
        .sorted((a, b) -> b.compareTo(a))  // 反向排序，先删除文件
        .forEach(path -> {
            try {
                Files.delete(path);
            } catch (IOException e) {
                e.printStackTrace();
            }
        });
} catch (IOException e) {
    e.printStackTrace();
}

// 列出目录内容
Path dir = Paths.get("data");

// 方式1: DirectoryStream
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) {
    for (Path entry : stream) {
        System.out.println(entry);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 方式2: Files.list() - 使用Stream API
try (Stream<Path> stream = Files.list(dir)) {
    stream.forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}

// 模式匹配
try (DirectoryStream<Path> stream = 
        Files.newDirectoryStream(dir, "*.txt")) {
    for (Path entry : stream) {
        System.out.println(entry);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 递归搜索
try (Stream<Path> stream = Files.walk(dir)) {
    stream.filter(path -> path.toString().endsWith(".java"))
          .forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}

// 列出所有文件和目录
try (Stream<Path> stream = Files.list(dir)) {
    List<Path> files = stream
        .filter(Files::isRegularFile)
        .collect(Collectors.toList());
    
    List<Path> dirs = stream
        .filter(Files::isDirectory)
        .collect(Collectors.toList());
} catch (IOException e) {
    e.printStackTrace();
}

// 重命名和移动
try {
    Path source = Paths.get("old_name.txt");
    Path target = Paths.get("new_name.txt");
    Files.move(source, target, StandardCopyOption.REPLACE_EXISTING);
    
    // 移动到其他目录
    Files.move(Paths.get("file.txt"), 
               Paths.get("target_dir/file.txt"));
} catch (IOException e) {
    e.printStackTrace();
}

// 复制文件和目录
try {
    // 复制文件
    Files.copy(Paths.get("source.txt"), 
               Paths.get("dest.txt"),
               StandardCopyOption.REPLACE_EXISTING);
    
    // 复制目录树
    Path sourceDir = Paths.get("source_dir");
    Path targetDir = Paths.get("dest_dir");
    Files.walk(sourceDir).forEach(source -> {
        try {
            Path target = targetDir.resolve(sourceDir.relativize(source));
            if (Files.isDirectory(source)) {
                Files.createDirectories(target);
            } else {
                Files.copy(source, target, 
                          StandardCopyOption.REPLACE_EXISTING);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    });
} catch (IOException e) {
    e.printStackTrace();
}

// 获取目录大小
public static long getDirSize(Path path) throws IOException {
    return Files.walk(path)
        .filter(Files::isRegularFile)
        .mapToLong(p -> {
            try {
                return Files.size(p);
            } catch (IOException e) {
                return 0L;
            }
        })
        .sum();
}

long size = getDirSize(Paths.get("data"));
System.out.printf("目录大小: %.2f MB%n", size / 1024.0 / 1024.0);

// 遍历目录树
Files.walkFileTree(Paths.get("data"), new SimpleFileVisitor<Path>() {
    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) {
        System.out.println("文件: " + file);
        return FileVisitResult.CONTINUE;
    }
    
    @Override
    public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) {
        System.out.println("目录: " + dir);
        return FileVisitResult.CONTINUE;
    }
});
```

------

#### 7.3.3 文件属性和元数据（7分钟）

**Python:**

```python
from pathlib import Path
import os
import time
from datetime import datetime

path = Path('example.txt')

# 获取文件属性
stat = path.stat()

# 文件大小
size = stat.st_size
print(f"文件大小: {size} 字节")
print(f"文件大小: {size / 1024:.2f} KB")

# 时间戳
created_time = stat.st_ctime      # 创建时间（Windows）/ 元数据修改时间（Unix）
modified_time = stat.st_mtime     # 修改时间
accessed_time = stat.st_atime     # 访问时间

# 转换为可读格式
print(f"修改时间: {datetime.fromtimestamp(modified_time)}")
print(f"访问时间: {datetime.fromtimestamp(accessed_time)}")

# 文件权限（Unix/Linux）
mode = stat.st_mode
print(f"权限: {oct(mode)}")

# 检查权限
is_readable = os.access(path, os.R_OK)
is_writable = os.access(path, os.W_OK)
is_executable = os.access(path, os.X_OK)

# 修改文件时间
os.utime(path, (accessed_time, modified_time))

# 修改文件权限（Unix/Linux）
os.chmod(path, 0o644)  # rw-r--r--

# 获取文件所有者（Unix/Linux）
import pwd
owner_uid = stat.st_uid
owner_info = pwd.getpwuid(owner_uid)
print(f"所有者: {owner_info.pw_name}")

# 文件类型检查
print(f"是文件: {path.is_file()}")
print(f"是目录: {path.is_dir()}")
print(f"是符号链接: {path.is_symlink()}")
print(f"是绝对路径: {path.is_absolute()}")

# 获取磁盘使用情况
import shutil
usage = shutil.disk_usage(path.parent)
print(f"总空间: {usage.total / 1024**3:.2f} GB")
print(f"已使用: {usage.used / 1024**3:.2f} GB")
print(f"可用空间: {usage.free / 1024**3:.2f} GB")
```

**Java:**

```java
import java.nio.file.*;
import java.nio.file.attribute.*;
import java.io.IOException;
import java.time.Instant;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

Path path = Paths.get("example.txt");

try {
    // 获取基本文件属性
    BasicFileAttributes attrs = Files.readAttributes(
        path, BasicFileAttributes.class);
    
    // 文件大小
    long size = attrs.size();
    System.out.println("文件大小: " + size + " 字节");
    System.out.printf("文件大小: %.2f KB%n", size / 1024.0);
    
    // 时间戳
    FileTime creationTime = attrs.creationTime();
    FileTime modifiedTime = attrs.lastModifiedTime();
    FileTime accessTime = attrs.lastAccessTime();
    
    // 转换为可读格式
    DateTimeFormatter formatter = DateTimeFormatter
        .ofPattern("yyyy-MM-dd HH:mm:ss")
        .withZone(ZoneId.systemDefault());
    
    System.out.println("创建时间: " + 
        formatter.format(creationTime.toInstant()));
    System.out.println("修改时间: " + 
        formatter.format(modifiedTime.toInstant()));
    System.out.println("访问时间: " + 
        formatter.format(accessTime.toInstant()));
    
    // 文件类型检查
    System.out.println("是文件: " + attrs.isRegularFile());
    System.out.println("是目录: " + attrs.isDirectory());
    System.out.println("是符号链接: " + attrs.isSymbolicLink());
    
    // 修改文件时间
    Files.setLastModifiedTime(path, 
        FileTime.from(Instant.now()));
    
    // POSIX文件属性（Unix/Linux）
    if (FileSystems.getDefault()
            .supportedFileAttributeViews().contains("posix")) {
        
        PosixFileAttributes posixAttrs = Files.readAttributes(
            path, PosixFileAttributes.class);
        
        // 获取权限
        Set<PosixFilePermission> permissions = posixAttrs.permissions();
        System.out.println("权限: " + 
            PosixFilePermissions.toString(permissions));
        
        // 获取所有者
        UserPrincipal owner = posixAttrs.owner();
        System.out.println("所有者: " + owner.getName());
        
        // 获取组
        GroupPrincipal group = posixAttrs.group();
        System.out.println("组: " + group.getName());
        
        // 修改权限
        Set<PosixFilePermission> newPerms = PosixFilePermissions
            .fromString("rw-r--r--");
        Files.setPosixFilePermissions(path, newPerms);
        
        // 修改所有者
        UserPrincipalLookupService lookupService = 
            FileSystems.getDefault().getUserPrincipalLookupService();
        UserPrincipal newOwner = lookupService.lookupPrincipalByName("username");
        Files.setOwner(path, newOwner);
    }
    
    // Windows文件属性
    if (System.getProperty("os.name").toLowerCase().contains("windows")) {
        DosFileAttributes dosAttrs = Files.readAttributes(
            path, DosFileAttributes.class);
        
        System.out.println("只读: " + dosAttrs.isReadOnly());
        System.out.println("隐藏: " + dosAttrs.isHidden());
        System.out.println("系统文件: " + dosAttrs.isSystem());
        System.out.println("存档: " + dosAttrs.isArchive());
        
        // 设置隐藏属性
        Files.setAttribute(path, "dos:hidden", true);
    }
    
    // 获取文件存储信息
    FileStore store = Files.getFileStore(path);
    System.out.println("文件系统类型: " + store.type());
    System.out.printf("总空间: %.2f GB%n", 
        store.getTotalSpace() / 1024.0 / 1024.0 / 1024.0);
    System.out.printf("可用空间: %.2f GB%n", 
        store.getUsableSpace() / 1024.0 / 1024.0 / 1024.0);
    
} catch (IOException e) {
    e.printStackTrace();
}
```

------

### 💾 7.4 序列化与反序列化（20分钟）

#### 7.4.1 Python序列化（pickle）（8分钟）

**Python - pickle模块**

```python
import pickle
from dataclasses import dataclass
from typing import List

# 定义数据类
@dataclass
class Person:
    name: str
    age: int
    skills: List[str]

# 创建对象
person = Person("张三", 25, ["Python", "Java"])

# ========== 序列化到文件 ==========
# 写入
with open('person.pkl', 'wb') as f:
    pickle.dump(person, f)

# 读取
with open('person.pkl', 'rb') as f:
    loaded_person = pickle.load(f)
    print(loaded_person)

# ========== 序列化到字节串 ==========
# 序列化
data = pickle.dumps(person)
print(f"序列化后大小: {len(data)} 字节")

# 反序列化
loaded_person = pickle.loads(data)

# ========== 序列化多个对象 ==========
people = [
    Person("张三", 25, ["Python"]),
    Person("李四", 30, ["Java"]),
    Person("王五", 28, ["Go"])
]

with open('people.pkl', 'wb') as f:
    for person in people:
        pickle.dump(person, f)

# 读取多个对象
people = []
with open('people.pkl', 'rb') as f:
    try:
        while True:
            person = pickle.load(f)
            people.append(person)
    except EOFError:
        pass

# ========== 自定义序列化行为 ==========
class CustomClass:
    def __init__(self, data):
        self.data = data
        self.temp = "临时数据"  # 不需要序列化
    
    def __getstate__(self):
        # 返回要序列化的状态
        state = self.__dict__.copy()
        del state['temp']  # 排除临时数据
        return state
    
    def __setstate__(self, state):
        # 恢复对象状态
        self.__dict__.update(state)
        self.temp = "重新初始化"

obj = CustomClass("重要数据")
data = pickle.dumps(obj)
loaded_obj = pickle.loads(data)
print(loaded_obj.data)  # "重要数据"
print(loaded_obj.temp)  # "重新初始化"

# ========== 协议版本 ==========
# pickle支持多个协议版本（0-5）
# 默认使用最新版本，可以指定版本以兼容旧版本Python

# 使用最高协议（最快，最小）
data = pickle.dumps(person, protocol=pickle.HIGHEST_PROTOCOL)

# 使用协议4（Python 3.4+）
data = pickle.dumps(person, protocol=4)

# ========== 安全注意事项 ==========
# ⚠️ 警告：不要反序列化不可信的数据！
# pickle可以执行任意代码，存在安全风险

# 安全的替代方案：使用JSON（对于简单数据）
import json

person_dict = {
    'name': person.name,
    'age': person.age,
    'skills': person.skills
}

json_str = json.dumps(person_dict)
loaded_dict = json.loads(json_str)
```

------

#### 7.4.2 Java序列化（Serializable）（8分钟）

**Java - 原生序列化**

```java
import java.io.*;
import java.util.*;

// 定义可序列化类
class Person implements Serializable {
    // 序列化版本号（重要！）
    private static final long serialVersionUID = 1L;
    
    private String name;
    private int age;
    private List<String> skills;
    
    // transient关键字：标记不需要序列化的字段
    private transient String tempData;
    
    public Person(String name, int age, List<String> skills) {
        this.name = name;
        this.age = age;
        this.skills = skills;
        this.tempData = "临时数据";
    }
    
    // Getters and setters...
    
    @Override
    public String toString() {
        return String.format("Person{name='%s', age=%d, skills=%s}", 
                           name, age, skills);
    }
}

// ========== 序列化到文件 ==========
Person person = new Person("张三", 25, List.of("Python", "Java"));

// 写入
try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("person.ser"))) {
    oos.writeObject(person);
} catch (IOException e) {
    e.printStackTrace();
}

// 读取
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("person.ser"))) {
    Person loadedPerson = (Person) ois.readObject();
    System.out.println(loadedPerson);
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}

// ========== 序列化到字节数组 ==========
// 序列化
byte[] data;
try (ByteArrayOutputStream baos = new ByteArrayOutputStream();
     ObjectOutputStream oos = new ObjectOutputStream(baos)) {
    oos.writeObject(person);
    data = baos.toByteArray();
    System.out.println("序列化后大小: " + data.length + " 字节");
} catch (IOException e) {
    e.printStackTrace();
}

// 反序列化
try (ByteArrayInputStream bais = new ByteArrayInputStream(data);
     ObjectInputStream ois = new ObjectInputStream(bais)) {
    Person loadedPerson = (Person) ois.readObject();
    System.out.println(loadedPerson);
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}

// ========== 序列化多个对象 ==========
List<Person> people = List.of(
    new Person("张三", 25, List.of("Python")),
    new Person("李四", 30, List.of("Java")),
    new Person("王五", 28, List.of("Go"))
);

try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("people.ser"))) {
    for (Person p : people) {
        oos.writeObject(p);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 读取多个对象
List<Person> loadedPeople = new ArrayList<>();
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("people.ser"))) {
    while (true) {
        try {
            Person p = (Person) ois.readObject();
            loadedPeople.add(p);
        } catch (EOFException e) {
            break;  // 文件结束
        }
    }
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}

// ========== 自定义序列化行为 ==========
class CustomClass implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String data;
    private transient String temp;
    
    public CustomClass(String data) {
        this.data = data;
        this.temp = "临时数据";
    }
    
    // 自定义序列化方法
    private void writeObject(ObjectOutputStream oos) throws IOException {
        // 写入默认字段
        oos.defaultWriteObject();
        // 可以写入额外数据
        oos.writeUTF("额外信息");
    }
    
    // 自定义反序列化方法
    private void readObject(ObjectInputStream ois) 
            throws IOException, ClassNotFoundException {
        // 读取默认字段
        ois.defaultReadObject();
        // 读取额外数据
        String extra = ois.readUTF();
        // 重新初始化transient字段
        this.temp = "重新初始化";
    }
}

// ========== Externalizable接口（完全控制） ==========
class FullControlClass implements Externalizable {
    private String name;
    private int value;
    
    // 必须有无参构造函数
    public FullControlClass() {}
    
    public FullControlClass(String name, int value) {
        this.name = name;
        this.value = value;
    }
    
    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        // 完全控制序列化过程
        out.writeUTF(name);
        out.writeInt(value);
    }
    
    @Override
    public void readExternal(ObjectInput in) 
            throws IOException, ClassNotFoundException {
        // 完全控制反序列化过程
        this.name = in.readUTF();
        this.value = in.readInt();
    }
}

// ========== 序列化版本控制 ==========
// serialVersionUID用于版本控制
// 如果类结构改变但serialVersionUID相同，反序列化时会尝试兼容
// 如果不同，会抛出InvalidClassException

class VersionedClass implements Serializable {
    private static final long serialVersionUID = 1L;  // 显式指定
    
    private String field1;
    // 如果添加新字段，旧版本反序列化时会使用默认值
    private String field2 = "默认值";
}
```

------

#### 7.4.3 JSON序列化对比（4分钟）

**Python - JSON**

```python
import json
from dataclasses import dataclass, asdict
from typing import List
from datetime import datetime

@dataclass
class Person:
    name: str
    age: int
    skills: List[str]
    created_at: datetime = None

# 基本序列化
person = Person("张三", 25, ["Python", "Java"])
json_str = json.dumps(asdict(person), ensure_ascii=False, indent=2)
print(json_str)

# 自定义JSON编码器
class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

person.created_at = datetime.now()
json_str = json.dumps(asdict(person), cls=DateTimeEncoder, 
                     ensure_ascii=False, indent=2)

# 自定义JSON解码器
def datetime_decoder(dct):
    if 'created_at' in dct and dct['created_at']:
        dct['created_at'] = datetime.fromisoformat(dct['created_at'])
    return dct

data = json.loads(json_str, object_hook=datetime_decoder)
```

**Java - JSON (使用Gson)**

```java
import com.google.gson.*;
import com.google.gson.annotations.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.List;

class Person {
    private String name;
    private int age;
    private List<String> skills;
    
    @SerializedName("created_at")  // 自定义JSON字段名
    private LocalDateTime createdAt;
    
    @Expose(serialize = false)  // 不序列化此字段
    private String password;
    
    // 构造函数、getters、setters...
}

// 基本序列化
Gson gson = new GsonBuilder()
    .setPrettyPrinting()
    .create();

Person person = new Person("张三", 25, List.of("Python", "Java"));
String json = gson.toJson(person);
Person loadedPerson = gson.fromJson(json, Person.class);

// 自定义日期格式（续）
Gson gson = new GsonBuilder()
    .setDateFormat("yyyy-MM-dd HH:mm:ss")
    .registerTypeAdapter(LocalDateTime.class, 
        new JsonSerializer<LocalDateTime>() {
            @Override
            public JsonElement serialize(LocalDateTime src, Type typeOfSrc, 
                                       JsonSerializationContext context) {
                return new JsonPrimitive(src.format(
                    DateTimeFormatter.ISO_LOCAL_DATE_TIME));
            }
        })
    .registerTypeAdapter(LocalDateTime.class, 
        new JsonDeserializer<LocalDateTime>() {
            @Override
            public LocalDateTime deserialize(JsonElement json, Type typeOfT,
                                           JsonDeserializationContext context) 
                    throws JsonParseException {
                return LocalDateTime.parse(json.getAsString(),
                    DateTimeFormatter.ISO_LOCAL_DATE_TIME);
            }
        })
    .setPrettyPrinting()
    .create();

person.setCreatedAt(LocalDateTime.now());
String json = gson.toJson(person);
Person loadedPerson = gson.fromJson(json, Person.class);

```

**序列化方案对比：**

| 特性     | pickle/Serializable | JSON          |
| -------- | ------------------- | ------------- |
| 可读性   | ❌ 二进制格式        | ✅ 文本格式    |
| 跨语言   | ❌ 语言特定          | ✅ 通用格式    |
| 安全性   | ⚠️ 不安全            | ✅ 相对安全    |
| 性能     | ✅ 较快              | ⚠️ 较慢        |
| 类型保留 | ✅ 完整保留          | ❌ 部分丢失    |
| 适用场景 | 内部存储、缓存      | API、配置文件 |

------

## 🎯 实战练习

### 练习1：日志文件分析器（难度：★★★☆☆）

**需求：** 编写一个日志文件分析工具，实现以下功能：

1. 读取日志文件
2. 统计不同级别（INFO、WARNING、ERROR）的日志数量
3. 提取所有ERROR级别的日志并保存到单独文件
4. 生成统计报告（JSON格式）

**Python实现：**

```python
from pathlib import Path
from collections import Counter
import json
import re
from datetime import datetime

class LogAnalyzer:
    def __init__(self, log_file):
        self.log_file = Path(log_file)
        self.log_pattern = re.compile(
            r'(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) - (\w+) - (.+)'
        )
        self.level_counts = Counter()
        self.error_logs = []
    
    def analyze(self):
        """分析日志文件"""
        try:
            with open(self.log_file, 'r', encoding='utf-8') as f:
                for line in f:
                    match = self.log_pattern.match(line.strip())
                    if match:
                        timestamp, level, message = match.groups()
                        self.level_counts[level] += 1
                        
                        if level == 'ERROR':
                            self.error_logs.append({
                                'timestamp': timestamp,
                                'message': message
                            })
        except FileNotFoundError:
            print(f"错误：文件 {self.log_file} 不存在")
            return False
        except Exception as e:
            print(f"分析失败: {e}")
            return False
        
        return True
    
    def save_errors(self, output_file='errors.log'):
        """保存ERROR日志到单独文件"""
        try:
            with open(output_file, 'w', encoding='utf-8') as f:
                for log in self.error_logs:
                    f.write(f"{log['timestamp']} - ERROR - {log['message']}\n")
            print(f"ERROR日志已保存到: {output_file}")
        except Exception as e:
            print(f"保存ERROR日志失败: {e}")
    
    def generate_report(self, output_file='report.json'):
        """生成统计报告"""
        report = {
            'analysis_time': datetime.now().isoformat(),
            'log_file': str(self.log_file),
            'total_logs': sum(self.level_counts.values()),
            'level_statistics': dict(self.level_counts),
            'error_count': len(self.error_logs)
        }
        
        try:
            with open(output_file, 'w', encoding='utf-8') as f:
                json.dump(report, f, ensure_ascii=False, indent=2)
            print(f"统计报告已生成: {output_file}")
        except Exception as e:
            print(f"生成报告失败: {e}")
    
    def print_summary(self):
        """打印摘要信息"""
        print("\n" + "="*50)
        print("日志分析摘要")
        print("="*50)
        print(f"日志文件: {self.log_file}")
        print(f"总日志数: {sum(self.level_counts.values())}")
        print("\n各级别统计:")
        for level, count in sorted(self.level_counts.items()):
            print(f"  {level:10s}: {count:5d}")
        print("="*50 + "\n")

# 使用示例
if __name__ == '__main__':
    # 创建测试日志文件
    test_logs = [
        "2025-10-16 10:00:00 - INFO - 应用启动",
        "2025-10-16 10:01:00 - INFO - 用户登录成功",
        "2025-10-16 10:02:00 - WARNING - 内存使用率达到80%",
        "2025-10-16 10:03:00 - ERROR - 数据库连接失败",
        "2025-10-16 10:04:00 - INFO - 重试连接",
        "2025-10-16 10:05:00 - ERROR - 连接超时",
    ]
    
    with open('app.log', 'w', encoding='utf-8') as f:
        f.write('\n'.join(test_logs))
    
    # 分析日志
    analyzer = LogAnalyzer('app.log')
    if analyzer.analyze():
        analyzer.print_summary()
        analyzer.save_errors()
        analyzer.generate_report()
```

**Java实现：**

```java
import java.io.*;
import java.nio.file.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.util.regex.*;
import com.google.gson.*;

class LogEntry {
    private String timestamp;
    private String level;
    private String message;
    
    public LogEntry(String timestamp, String level, String message) {
        this.timestamp = timestamp;
        this.level = level;
        this.message = message;
    }
    
    // Getters...
    public String getLevel() { return level; }
    public String getTimestamp() { return timestamp; }
    public String getMessage() { return message; }
}

class LogAnalyzer {
    private Path logFile;
    private Pattern logPattern;
    private Map<String, Integer> levelCounts;
    private List<LogEntry> errorLogs;
    
    public LogAnalyzer(String logFile) {
        this.logFile = Paths.get(logFile);
        this.logPattern = Pattern.compile(
            "(\\d{4}-\\d{2}-\\d{2} \\d{2}:\\d{2}:\\d{2}) - (\\w+) - (.+)"
        );
        this.levelCounts = new HashMap<>();
        this.errorLogs = new ArrayList<>();
    }
    
    public boolean analyze() {
        try (BufferedReader reader = Files.newBufferedReader(logFile)) {
            String line;
            while ((line = reader.readLine()) != null) {
                Matcher matcher = logPattern.matcher(line.trim());
                if (matcher.matches()) {
                    String timestamp = matcher.group(1);
                    String level = matcher.group(2);
                    String message = matcher.group(3);
                    
                    levelCounts.merge(level, 1, Integer::sum);
                    
                    if ("ERROR".equals(level)) {
                        errorLogs.add(new LogEntry(timestamp, level, message));
                    }
                }
            }
            return true;
        } catch (IOException e) {
            System.err.println("分析失败: " + e.getMessage());
            return false;
        }
    }
    
    public void saveErrors(String outputFile) {
        try (BufferedWriter writer = Files.newBufferedWriter(
                Paths.get(outputFile))) {
            for (LogEntry log : errorLogs) {
                writer.write(String.format("%s - ERROR - %s%n",
                    log.getTimestamp(), log.getMessage()));
            }
            System.out.println("ERROR日志已保存到: " + outputFile);
        } catch (IOException e) {
            System.err.println("保存ERROR日志失败: " + e.getMessage());
        }
    }
    
    public void generateReport(String outputFile) {
        Map<String, Object> report = new HashMap<>();
        report.put("analysis_time", LocalDateTime.now().toString());
        report.put("log_file", logFile.toString());
        report.put("total_logs", 
            levelCounts.values().stream().mapToInt(Integer::intValue).sum());
        report.put("level_statistics", levelCounts);
        report.put("error_count", errorLogs.size());
        
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        
        try (Writer writer = Files.newBufferedWriter(Paths.get(outputFile))) {
            gson.toJson(report, writer);
            System.out.println("统计报告已生成: " + outputFile);
        } catch (IOException e) {
            System.err.println("生成报告失败: " + e.getMessage());
        }
    }
    
    public void printSummary() {
        System.out.println("\n" + "=".repeat(50));
        System.out.println("日志分析摘要");
        System.out.println("=".repeat(50));
        System.out.println("日志文件: " + logFile);
        System.out.println("总日志数: " + 
            levelCounts.values().stream().mapToInt(Integer::intValue).sum());
        System.out.println("\n各级别统计:");
        
        levelCounts.entrySet().stream()
            .sorted(Map.Entry.comparingByKey())
            .forEach(entry -> 
                System.out.printf("  %-10s: %5d%n", entry.getKey(), entry.getValue())
            );
        
        System.out.println("=".repeat(50) + "\n");
    }
    
    public static void main(String[] args) {
        // 创建测试日志文件
        List<String> testLogs = List.of(
            "2025-10-16 10:00:00 - INFO - 应用启动",
            "2025-10-16 10:01:00 - INFO - 用户登录成功",
            "2025-10-16 10:02:00 - WARNING - 内存使用率达到80%",
            "2025-10-16 10:03:00 - ERROR - 数据库连接失败",
            "2025-10-16 10:04:00 - INFO - 重试连接",
            "2025-10-16 10:05:00 - ERROR - 连接超时"
        );
        
        try {
            Files.write(Paths.get("app.log"), testLogs);
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // 分析日志
        LogAnalyzer analyzer = new LogAnalyzer("app.log");
        if (analyzer.analyze()) {
            analyzer.printSummary();
            analyzer.saveErrors("errors.log");
            analyzer.generateReport("report.json");
        }
    }
}
```

------

### 练习2：配置文件管理器（难度：★★★☆☆）

**需求：** 实现一个配置文件管理器，支持：

1. 读取和写入JSON配置文件
2. 配置验证
3. 默认值处理
4. 配置备份和恢复

**Python实现：**

```python
import json
from pathlib import Path
from datetime import datetime
import shutil
from typing import Any, Dict, Optional

class ConfigManager:
    def __init__(self, config_file: str):
        self.config_file = Path(config_file)
        self.backup_dir = Path('backups')
        self.config: Dict[str, Any] = {}
        self.defaults = {
            'app_name': 'MyApp',
            'version': '1.0.0',
            'debug': False,
            'max_connections': 100,
            'timeout': 30
        }
    
    def load(self) -> bool:
        """加载配置文件"""
        try:
            if self.config_file.exists():
                with open(self.config_file, 'r', encoding='utf-8') as f:
                    self.config = json.load(f)
                print(f"配置已加载: {self.config_file}")
                return True
            else:
                print("配置文件不存在，使用默认配置")
                self.config = self.defaults.copy()
                return self.save()
        except json.JSONDecodeError as e:
            print(f"配置文件格式错误: {e}")
            return False
        except Exception as e:
            print(f"加载配置失败: {e}")
            return False
    
    def save(self) -> bool:
        """保存配置文件"""
        try:
            # 确保目录存在
            self.config_file.parent.mkdir(parents=True, exist_ok=True)
            
            with open(self.config_file, 'w', encoding='utf-8') as f:
                json.dump(self.config, f, ensure_ascii=False, indent=2)
            print(f"配置已保存: {self.config_file}")
            return True
        except Exception as e:
            print(f"保存配置失败: {e}")
            return False
    
    def get(self, key: str, default: Any = None) -> Any:
        """获取配置项"""
        return self.config.get(key, default)
    
    def set(self, key: str, value: Any) -> None:
        """设置配置项"""
        self.config[key] = value
    
    def validate(self) -> bool:
        """验证配置"""
        required_keys = ['app_name', 'version']
        
        for key in required_keys:
            if key not in self.config:
                print(f"缺少必需配置项: {key}")
                return False
        
        # 类型验证
        if not isinstance(self.config.get('debug', False), bool):
            print("debug必须是布尔值")
            return False
        
        if not isinstance(self.config.get('max_connections', 0), int):
            print("max_connections必须是整数")
            return False
        
        # 范围验证
        max_conn = self.config.get('max_connections', 0)
        if max_conn < 1 or max_conn > 1000:
            print("max_connections必须在1-1000之间")
            return False
        
        print("配置验证通过")
        return True
    
    def backup(self) -> Optional[Path]:
        """备份配置文件"""
        try:
            if not self.config_file.exists():
                print("配置文件不存在，无需备份")
                return None
            
            self.backup_dir.mkdir(exist_ok=True)
            
            timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
            backup_file = self.backup_dir / f"{self.config_file.stem}_{timestamp}.json"
            
            shutil.copy2(self.config_file, backup_file)
            print(f"配置已备份到: {backup_file}")
            return backup_file
        except Exception as e:
            print(f"备份失败: {e}")
            return None
    
    def restore(self, backup_file: str) -> bool:
        """从备份恢复配置"""
        try:
            backup_path = Path(backup_file)
            if not backup_path.exists():
                print(f"备份文件不存在: {backup_file}")
                return False
            
            shutil.copy2(backup_path, self.config_file)
            print(f"配置已从备份恢复: {backup_file}")
            return self.load()
        except Exception as e:
            print(f"恢复失败: {e}")
            return False
    
    def list_backups(self) -> list:
        """列出所有备份"""
        if not self.backup_dir.exists():
            return []
        
        backups = sorted(self.backup_dir.glob('*.json'), reverse=True)
        return [str(b) for b in backups]
    
    def print_config(self) -> None:
        """打印当前配置"""
        print("\n当前配置:")
        print(json.dumps(self.config, ensure_ascii=False, indent=2))

# 使用示例
if __name__ == '__main__':
    config = ConfigManager('config.json')
    
    # 加载配置
    config.load()
    
    # 修改配置
    config.set('debug', True)
    config.set('max_connections', 200)
    
    # 验证配置
    if config.validate():
        # 备份当前配置
        config.backup()
        
        # 保存配置
        config.save()
    
    # 打印配置
    config.print_config()
    
    # 列出备份
    print("\n可用备份:")
    for backup in config.list_backups():
        print(f"  {backup}")
```

**Java实现：**

```java
import com.google.gson.*;
import java.io.*;
import java.nio.file.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.util.stream.Collectors;

class ConfigManager {
    private Path configFile;
    private Path backupDir;
    private Map<String, Object> config;
    private Map<String, Object> defaults;
    private Gson gson;
    
    public ConfigManager(String configFile) {
        this.configFile = Paths.get(configFile);
        this.backupDir = Paths.get("backups");
        this.config = new HashMap<>();
        this.gson = new GsonBuilder().setPrettyPrinting().create();
        
        // 设置默认值
        this.defaults = new HashMap<>();
        defaults.put("app_name", "MyApp");
        defaults.put("version", "1.0.0");
        defaults.put("debug", false);
        defaults.put("max_connections", 100);
        defaults.put("timeout", 30);
    }
    
    @SuppressWarnings("unchecked")
    public boolean load() {
        try {
            if (Files.exists(configFile)) {
                try (Reader reader = Files.newBufferedReader(configFile)) {
                    config = gson.fromJson(reader, Map.class);
                }
                System.out.println("配置已加载: " + configFile);
                return true;
            } else {
                System.out.println("配置文件不存在，使用默认配置");
                config = new HashMap<>(defaults);
                return save();
            }
        } catch (JsonSyntaxException e) {
            System.err.println("配置文件格式错误: " + e.getMessage());
            return false;
        } catch (IOException e) {
            System.err.println("加载配置失败: " + e.getMessage());
            return false;
        }
    }
    
    public boolean save() {
        try {
            // 确保目录存在
            Files.createDirectories(configFile.getParent());
            
            try (Writer writer = Files.newBufferedWriter(configFile)) {
                gson.toJson(config, writer);
            }
            System.out.println("配置已保存: " + configFile);
            return true;
        } catch (IOException e) {
            System.err.println("保存配置失败: " + e.getMessage());
            return false;
        }
    }
    
    public Object get(String key, Object defaultValue) {
        return config.getOrDefault(key, defaultValue);
    }
    
    public void set(String key, Object value) {
        config.put(key, value);
    }
    
    public boolean validate() {
        List<String> requiredKeys = List.of("app_name", "version");
        
        // 检查必需字段
        for (String key : requiredKeys) {
            if (!config.containsKey(key)) {
                System.err.println("缺少必需配置项: " + key);
                return false;
            }
        }
        
        // 类型验证
        Object debug = config.get("debug");
        if (debug != null && !(debug instanceof Boolean)) {
            System.err.println("debug必须是布尔值");
            return false;
        }
        
        Object maxConn = config.get("max_connections");
        if (maxConn != null) {
            // Gson将数字解析为Double
            if (!(maxConn instanceof Number)) {
                System.err.println("max_connections必须是数字");
                return false;
            }
            
            int value = ((Number) maxConn).intValue();
            if (value < 1 || value > 1000) {
                System.err.println("max_connections必须在1-1000之间");
                return false;
            }
        }
        
        System.out.println("配置验证通过");
        return true;
    }
    
    public Path backup() {
        try {
            if (!Files.exists(configFile)) {
                System.out.println("配置文件不存在，无需备份");
                return null;
            }
            
            Files.createDirectories(backupDir);
            
            String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            String fileName = configFile.getFileName().toString();
            String baseName = fileName.substring(0, fileName.lastIndexOf('.'));
            Path backupFile = backupDir.resolve(baseName + "_" + timestamp + ".json");
            
            Files.copy(configFile, backupFile, 
                      StandardCopyOption.REPLACE_EXISTING);
            System.out.println("配置已备份到: " + backupFile);
            return backupFile;
        } catch (IOException e) {
            System.err.println("备份失败: " + e.getMessage());
            return null;
        }
    }
    
    public boolean restore(String backupFile) {
        try {
            Path backupPath = Paths.get(backupFile);
            if (!Files.exists(backupPath)) {
                System.err.println("备份文件不存在: " + backupFile);
                return false;
            }
            
            Files.copy(backupPath, configFile, 
                      StandardCopyOption.REPLACE_EXISTING);
            System.out.println("配置已从备份恢复: " + backupFile);
            return load();
        } catch (IOException e) {
            System.err.println("恢复失败: " + e.getMessage());
            return false;
        }
    }
    
    public List<String> listBackups() {
        try {
            if (!Files.exists(backupDir)) {
                return Collections.emptyList();
            }
            
            return Files.list(backupDir)
                .filter(p -> p.toString().endsWith(".json"))
                .sorted(Comparator.reverseOrder())
                .map(Path::toString)
                .collect(Collectors.toList());
        } catch (IOException e) {
            return Collections.emptyList();
        }
    }
    
    public void printConfig() {
        System.out.println("\n当前配置:");
        System.out.println(gson.toJson(config));
    }
    
    public static void main(String[] args) {
        ConfigManager config = new ConfigManager("config.json");
        
        // 加载配置
        config.load();
        
        // 修改配置
        config.set("debug", true);
        config.set("max_connections", 200);
        
        // 验证配置
        if (config.validate()) {
            // 备份当前配置
            config.backup();
            
            // 保存配置
            config.save();
        }
        
        // 打印配置
        config.printConfig();
        
        // 列出备份
        System.out.println("\n可用备份:");
        for (String backup : config.listBackups()) {
            System.out.println("  " + backup);
        }
    }
}
```

------

## 📝 课程总结

### 核心知识点回顾

#### 1. 异常处理对比

| 特性     | Python                  | Java              |
| -------- | ----------------------- | ----------------- |
| 异常类型 | 全部未检查              | 检查+未检查       |
| 语法     | try-except-else-finally | try-catch-finally |
| 抛出异常 | raise                   | throw/throws      |
| 异常链   | from关键字              | 构造函数参数      |

#### 2. 文件操作对比

| 操作     | Python         | Java               |
| -------- | -------------- | ------------------ |
| 资源管理 | with语句       | try-with-resources |
| 路径操作 | pathlib.Path   | java.nio.file.Path |
| 文件读写 | open()         | Files类            |
| 目录遍历 | Path.iterdir() | Files.walk()       |

#### 3. 序列化对比

| 方案       | Python         | Java               |
| ---------- | -------------- | ------------------ |
| 原生序列化 | pickle         | Serializable       |
| JSON       | json模块       | Gson/Jackson       |
| 安全性     | pickle不安全   | Serializable不安全 |
| 推荐       | JSON（跨平台） | JSON（跨平台）     |

------

### 最佳实践建议

**异常处理：**

- ✅ 捕获具体的异常类型，避免捕获所有异常
- ✅ 提供有意义的错误信息
- ✅ 使用异常链保留原始异常信息
- ✅ 在合适的层级处理异常
- ❌ 不要用异常控制正常流程

**文件操作：**

- ✅ 始终使用上下文管理器（with/try-with-resources）
- ✅ 明确指定文件编码（UTF-8）
- ✅ 处理大文件时使用分块读取
- ✅ 验证文件路径和权限
- ❌ 不要忘记关闭文件句柄

**序列化：**

- ✅ 优先使用JSON等文本格式
- ✅ 不要序列化敏感信息
- ✅ 添加版本控制
- ✅ 验证反序列化的数据
- ❌ 不要反序列化不可信的数据

------

### 下节课预告

**第八课：并发编程基础**

- 多线程编程
- 线程同步机制
- 线程池与执行器
- 协程与异步编程

------

## 🎓 课后作业

### 作业1：文件加密工具（难度：★★★★☆）

实现一个简单的文件加密/解密工具：

- 支持对称加密（如AES）
- 异常处理（文件不存在、权限不足等）
- 进度显示（大文件）
- 支持批量处理

### 作业2：数据备份系统（难度：★★★★☆）

开发一个自动备份系统：

- 定期备份指定目录
- 增量备份支持
- 压缩备份文件
- 备份验证和恢复
- 日志记录

### 作业3：配置热更新（难度：★★★★★）

实现配置文件的热更新机制：

- 监控配置文件变化
- 自动重新加载配置
- 配置验证
- 回滚机制
- 通知机制

------

**恭喜完成第七课！** 🎉

你已经掌握了：

- ✅ 异常处理的完整机制
- ✅ 文件I/O的各种操作
- ✅ 路径和目录管理
- ✅ 序列化与反序列化技术

继续加油，下一课我们将学习并发编程！💪