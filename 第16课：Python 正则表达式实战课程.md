#  第16课：Python 正则表达式实战课程

> **面向对象**：Java 开发者转型 Python
>  **课程时长**：2节课 × 15分钟
>  **学习目标**：掌握 Python 正则表达式的实际应用

------

## 📚 课程大纲

### 第一节：正则表达式基础与常用方法（15分钟）

- Java vs Python 正则对比
- re 模块核心方法
- 基础匹配模式
- 实战案例：验证手机号和邮箱

### 第二节：高级匹配与文本处理（15分钟）

- 分组与捕获
- 贪婪与非贪婪匹配
- 文本替换与提取
- 实战案例：网页数据提取

------

# 📖 第一节：正则表达式基础与常用方法

## 🔄 Java vs Python 正则对比

### Java 的正则使用方式

```java
import java.util.regex.*;

public class RegexDemo {
    public static void main(String[] args) {
        // Java 方式
        String text = "我的手机号是 13812345678";
        
        // 1. 创建 Pattern 对象
        Pattern pattern = Pattern.compile("1[3-9]\\d{9}");
        
        // 2. 创建 Matcher 对象
        Matcher matcher = pattern.matcher(text);
        
        // 3. 执行匹配
        if (matcher.find()) {
            System.out.println("找到手机号：" + matcher.group());
        }
        
        // 替换
        String result = text.replaceAll("\\d", "*");
        System.out.println(result);  // 我的手机号是 ***********
    }
}
```

### Python 的正则使用方式

```python
import re

# Python 方式 - 更简洁！
text = "我的手机号是 13812345678"

# 1. 直接使用 re 模块方法（推荐）
match = re.search(r"1[3-9]\d{9}", text)
if match:
    print(f"找到手机号：{match.group()}")

# 2. 或者编译后使用（性能更好，适合重复使用）
pattern = re.compile(r"1[3-9]\d{9}")
match = pattern.search(text)

# 替换
result = re.sub(r"\d", "*", text)
print(result)  # 我的手机号是 ***********
```

### 🎯 关键区别

| 特性         | Java                       | Python                    |
| ------------ | -------------------------- | ------------------------- |
| **导入方式** | `import java.util.regex.*` | `import re`               |
| **转义字符** | `\\d`（需要双反斜杠）      | `r"\d"`（使用原始字符串） |
| **使用方式** | Pattern → Matcher → 方法   | 直接调用 re 模块方法      |
| **代码量**   | 较多（3步）                | 较少（1步）               |

------

## 📝 re 模块核心方法

### 1️⃣ `re.search()` - 查找第一个匹配

**类比 Java**：`Matcher.find()`

```python
import re

text = "联系方式：13812345678 或 13987654321"

# 查找第一个手机号
match = re.search(r"1[3-9]\d{9}", text)
if match:
    print(f"找到：{match.group()}")  # 找到：13812345678
    print(f"位置：{match.span()}")   # 位置：(5, 16)
else:
    print("未找到")
```

**Java 等价代码**：

```java
Pattern pattern = Pattern.compile("1[3-9]\\d{9}");
Matcher matcher = pattern.matcher(text);
if (matcher.find()) {
    System.out.println("找到：" + matcher.group());
}
```

------

### 2️⃣ `re.match()` - 从开头匹配

**类比 Java**：`Matcher.lookingAt()`

```python
import re

# ✅ 成功：从开头匹配
text1 = "13812345678 是我的手机号"
match1 = re.match(r"1[3-9]\d{9}", text1)
print(match1.group() if match1 else "未匹配")  # 13812345678

# ❌ 失败：不是从开头
text2 = "我的手机号是 13812345678"
match2 = re.match(r"1[3-9]\d{9}", text2)
print(match2.group() if match2 else "未匹配")  # 未匹配
```

**使用场景**：验证整个字符串格式（配合 `^` 和 `$`）

------

### 3️⃣ `re.findall()` - 查找所有匹配

**类比 Java**：循环调用 `Matcher.find()`

```python
import re

text = "联系方式：13812345678 或 13987654321，备用：15912345678"

# 查找所有手机号
phones = re.findall(r"1[3-9]\d{9}", text)
print(phones)  
# ['13812345678', '13987654321', '15912345678']

# 遍历处理
for i, phone in enumerate(phones, 1):
    print(f"手机号{i}：{phone}")
    
 phones=pattern.finditer(r"1[3-9]\d{9}", orgin_sr);
# for i,phone in enumerate(phones):
#     print(f" 第{i} 次 匹配到 {phone.group()}")
#     print(f" 第{i} 次 位置 {phone.span()}")
#     print(f" 第{i} 次 匹配到的开始 {phone.start()}")
#     print(f" 第{i} 次 匹配到的开始 {phone.end()}")
```

**Java 等价代码**：

```java
Pattern pattern = Pattern.compile("1[3-9]\\d{9}");
Matcher matcher = pattern.matcher(text);
List<String> phones = new ArrayList<>();
while (matcher.find()) {
    phones.add(matcher.group());
}
```

------

### 4️⃣ `re.sub()` - 替换匹配内容

**类比 Java**：`Matcher.replaceAll()`

```python
import re

text = "我的手机号是 13812345678"

# 1. 简单替换
result1 = re.sub(r"\d", "*", text)
print(result1)  # 我的手机号是 ***********

# 2. 使用函数替换（强大！）
def hide_phone(match):
    phone = match.group()
    return phone[:3] + "****" + phone[7:]

result2 = re.sub(r"1[3-9]\d{9}", hide_phone, text)
print(result2)  # 我的手机号是 138****5678

# 3. 限制替换次数
text3 = "价格：100元，原价：200元，折扣：50元"
result3 = re.sub(r"\d+", "***", text3, count=2)
print(result3)  # 价格：***元，原价：***元，折扣：50元
```

------

### 5️⃣ `re.split()` - 按模式分割

**类比 Java**：`String.split()`

```python
import re

# 1. 按多种分隔符分割
text1 = "apple,banana;orange:grape"
fruits = re.split(r"[,;:]", text1)
print(fruits)  # ['apple', 'banana', 'orange', 'grape']

# 2. 按空白字符分割
text2 = "Python  Java\tC++\nGo"
languages = re.split(r"\s+", text2)
print(languages)  # ['Python', 'Java', 'C++', 'Go']

# 3. 限制分割次数
text3 = "a-b-c-d-e"
parts = re.split(r"-", text3, maxsplit=2)
print(parts)  # ['a', 'b', 'c-d-e']
```

------

## 🎯 实战案例1：手机号验证器

```python
import re

class PhoneValidator:
    """手机号验证器"""
    
    def __init__(self):
        # 中国大陆手机号规则：
        # 1. 以1开头
        # 2. 第二位是 3,4,5,7,8,9
        # 3. 后面9位是数字
        self.pattern = re.compile(r"^1[3-9]\d{9}$")
    
    def validate(self, phone: str) -> bool:
        """验证手机号"""
        return bool(self.pattern.match(phone))
    
    def extract(self, text: str) -> list:
        """从文本中提取所有手机号"""
        return re.findall(r"1[3-9]\d{9}", text)
    
    def hide(self, phone: str) -> str:
        """隐藏手机号中间4位"""
        if not self.validate(phone):
            return phone
        return phone[:3] + "****" + phone[7:]


# 测试
validator = PhoneValidator()

# 1. 验证手机号
test_phones = [
    "13812345678",  # ✅ 合法
    "11456817239",  # ❌ 第二位不合法
    "19912345678",  # ✅ 合法
    "1234567890",   # ❌ 位数不对
]

print("=" * 50)
print("手机号验证测试")
print("=" * 50)
for phone in test_phones:
    status = "✅ 合法" if validator.validate(phone) else "❌ 非法"
    print(f"{phone:15} {status}")

# 2. 提取手机号
text = """
客服热线：400-123-4567
联系人：张三 13812345678
        李四 13987654321
备用：15912345678
"""

print("\n" + "=" * 50)
print("提取手机号")
print("=" * 50)
phones = validator.extract(text)
for i, phone in enumerate(phones, 1):
    print(f"{i}. {phone} → {validator.hide(phone)}")
```

**输出**：

```markdown
==================================================
手机号验证测试
==================================================
13812345678     ✅ 合法
11456817239     ❌ 非法
19912345678     ✅ 合法
1234567890      ❌ 非法

==================================================
提取手机号
==================================================
1. 13812345678 → 138****5678
2. 13987654321 → 139****4321
3. 15912345678 → 159****5678
```

------

## 🎯 实战案例2：邮箱验证器

```
python复制import re

class EmailValidator:
    """邮箱验证器"""
    
    def __init__(self):
        # 邮箱规则（简化版）：
        # 用户名：字母、数字、下划线、点、连字符
        # @
        # 域名：字母、数字、点、连字符
        # .
        # 顶级域名：2-6个字母
        self.pattern = re.compile(
            r"^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$"
        )
    
    def validate(self, email: str) -> bool:
        """验证邮箱"""
        return bool(self.pattern.match(email))
    
    def extract_domain(self, email: str) -> str:
        """提取域名"""
        match = re.search(r"@([a-zA-Z0-9.-]+)", email)
        return match.group(1) if match else ""
    
    def hide(self, email: str) -> str:
        """隐藏邮箱用户名"""
        if not self.validate(email):
            return email
        
        # 分离用户名和域名
        username, domain = email.split("@")
        
        # 隐藏用户名
        if len(username) <= 2:
            hidden = "*" * len(username)
        else:
            hidden = username[0] + "*" * (len(username) - 2) + username[-1]
        
        return f"{hidden}@{domain}"


# 测试
validator = EmailValidator()

test_emails = [
    "example@example.com",           # ✅ 合法
    "user.name@subdomain.example.co", # ✅ 合法
    "username@.com",                  # ❌ 域名不合法
    "@missingusername.com",           # ❌ 缺少用户名
    "no-at-sign.com",                 # ❌ 缺少 @
]

print("=" * 60)
print("邮箱验证测试")
print("=" * 60)
for email in test_emails:
    status = "✅ 合法" if validator.validate(email) else "❌ 非法"
    domain = validator.extract_domain(email)
    hidden = validator.hide(email)
    
    print(f"{email:35} {status}")
    if domain:
        print(f"  域名：{domain}")
        print(f"  隐藏：{hidden}")
    print()
```

**输出**：

```
kotlin复制============================================================
邮箱验证测试
============================================================
example@example.com                 ✅ 合法
  域名：example.com
  隐藏：e*****e@example.com

user.name@subdomain.example.co      ✅ 合法
  域名：subdomain.example.co
  隐藏：u*******e@subdomain.example.co

username@.com                       ❌ 非法

@missingusername.com                ❌ 非法

no-at-sign.com                      ❌ 非法
```

------

## 📚 第一节小结

### ✅ 你已经学会了

1. **re 模块核心方法**
   - `search()` - 查找第一个
   - `match()` - 从开头匹配
   - `findall()` - 查找所有
   - `sub()` - 替换
   - `split()` - 分割
2. **与 Java 的对比**
   - Python 更简洁（无需 Pattern/Matcher）
   - 使用原始字符串 `r"..."` 避免转义
   - 直接调用方法，无需多步操作
3. **实战应用**
   - 手机号验证、提取、隐藏
   - 邮箱验证、域名提取、隐藏

### 🎯 练习题

```
python复制# 练习1：验证身份证号（18位）
# 规则：前17位是数字，最后一位是数字或X

# 练习2：提取文本中的所有URL
text = "访问 https://www.python.org 或 http://github.com"

# 练习3：验证密码强度
# 规则：8-16位，必须包含大小写字母和数字
```

------

# 📖 第二节：高级匹配与文本处理

## 🎯 分组与捕获

### 基本分组 `()`

**类比 Java**：`Matcher.group(1)`, `Matcher.group(2)`

```
python复制import re

# 1. 基本分组
text = "张三的手机号是 13812345678"
match = re.search(r"(\w+)的手机号是 (1[3-9]\d{9})", text)

if match:
    print(f"完整匹配：{match.group(0)}")  # 张三的手机号是 13812345678
    print(f"姓名：{match.group(1)}")      # 张三
    print(f"手机号：{match.group(2)}")    # 13812345678
    print(f"所有分组：{match.groups()}")  # ('张三', '13812345678')
```

------

### 命名分组 `(?P<name>...)`

**Python 特色功能**（Java 也支持，但语法不同）

```
python复制import re

text = "张三的手机号是 13812345678"

# 使用命名分组
pattern = r"(?P<name>\w+)的手机号是 (?P<phone>1[3-9]\d{9})"
match = re.search(pattern, text)

if match:
    # 方式1：通过名称访问
    print(f"姓名：{match.group('name')}")    # 张三
    print(f"手机号：{match.group('phone')}")  # 13812345678
    
    # 方式2：通过字典访问（推荐！）
    info = match.groupdict()
    print(f"信息：{info}")  # {'name': '张三', 'phone': '13812345678'}
```

------

### 实战：解析日志

```
python复制import re
from datetime import datetime

class LogParser:
    """日志解析器"""
    
    def __init__(self):
        # 日志格式：[时间] 级别 - 消息
        self.pattern = re.compile(
            r"\[(?P<time>[\d\-: ]+)\] "
            r"(?P<level>\w+) - "
            r"(?P<message>.+)"
        )
    
    def parse(self, log_line: str) -> dict:
        """解析单行日志"""
        match = self.pattern.match(log_line)
        if not match:
            return None
        
        data = match.groupdict()
        # 转换时间格式
        data['time'] = datetime.strptime(data['time'], "%Y-%m-%d %H:%M:%S")
        return data
    
    def filter_by_level(self, logs: list, level: str) -> list:
        """按级别过滤日志"""
        return [log for log in logs if log and log['level'] == level]


# 测试
parser = LogParser()

logs = [
    "[2024-10-27 10:30:15] INFO - 服务启动成功",
    "[2024-10-27 10:31:20] WARNING - 内存使用率达到80%",
    "[2024-10-27 10:32:45] ERROR - 数据库连接失败",
    "[2024-10-27 10:33:10] INFO - 重试连接成功",
]

print("=" * 60)
print("日志解析")
print("=" * 60)

parsed_logs = [parser.parse(log) for log in logs]

# 显示所有日志
for log in parsed_logs:
    if log:
        print(f"{log['time']} [{log['level']:7}] {log['message']}")

# 只显示错误日志
print("\n" + "=" * 60)
print("错误日志")
print("=" * 60)
error_logs = parser.filter_by_level(parsed_logs, "ERROR")
for log in error_logs:
    print(f"{log['time']} {log['message']}")
```

------

## 🎯 贪婪 vs 非贪婪匹配

### 贪婪匹配（默认）

```
python复制import re

html = '<div>内容1</div><div>内容2</div>'

# 贪婪匹配：尽可能多地匹配
greedy = re.search(r'<div>.*</div>', html)
print(f"贪婪：{greedy.group()}")
# 输出：<div>内容1</div><div>内容2</div>  ← 匹配了全部！
```

### 非贪婪匹配（加 `?`）

```
python复制import re

html = '<div>内容1</div><div>内容2</div>'

# 非贪婪匹配：尽可能少地匹配
non_greedy = re.search(r'<div>.*?</div>', html)
print(f"非贪婪：{non_greedy.group()}")
# 输出：<div>内容1</div>  ← 只匹配第一个！

# 查找所有
all_divs = re.findall(r'<div>(.*?)</div>', html)
print(f"所有内容：{all_divs}")
# 输出：['内容1', '内容2']
```

### 对比表

| 模式  | 说明   | 示例                   | 匹配结果  |
| ----- | ------ | ---------------------- | --------- |
| `.*`  | 贪婪   | `<.*>` 匹配 `<a>b<c>`  | `<a>b<c>` |
| `.*?` | 非贪婪 | `<.*?>` 匹配 `<a>b<c>` | `<a>`     |
| `.+`  | 贪婪   | `".+"` 匹配 `"a" "b"`  | `"a" "b"` |
| `.+?` | 非贪婪 | `".+?"` 匹配 `"a" "b"` | `"a"`     |

------

## 🎯 实战案例3：HTML 标签提取器

```
python复制import re
from typing import List, Dict

class HTMLParser:
    """简单的 HTML 解析器"""
    
    @staticmethod
    def extract_links(html: str) -> List[str]:
        """提取所有链接"""
        # 非贪婪匹配 href 属性
        pattern = r'href=["\'](.*?)["\']'
        return re.findall(pattern, html)
    
    @staticmethod
    def extract_images(html: str) -> List[str]:
        """提取所有图片"""
        pattern = r'<img[^>]+src=["\'](.*?)["\']'
        return re.findall(pattern, html)
    
    @staticmethod
    def extract_text(html: str) -> str:
        """移除所有 HTML 标签"""
        # 移除标签
        text = re.sub(r'<[^>]+>', '', html)
        # 移除多余空白
        text = re.sub(r'\s+', ' ', text)
        return text.strip()
    
    @staticmethod
    def extract_tags(html: str, tag: str) -> List[Dict[str, str]]:
        """提取指定标签及其属性"""
        pattern = rf'<{tag}([^>]*)>(.*?)</{tag}>'
        matches = re.findall(pattern, html, re.DOTALL)
        
        results = []
        for attrs, content in matches:
            # 解析属性
            attr_pattern = r'(\w+)=["\'](.*?)["\']'
            attributes = dict(re.findall(attr_pattern, attrs))
            
            results.append({
                'attributes': attributes,
                'content': content.strip()
            })
        
        return results


# 测试
html = """
<html>
<head>
    <title>测试页面</title>
</head>
<body>
    <h1>欢迎来到 Python 世界</h1>
    <p>这是一个<a href="https://www.python.org">Python</a>教程。</p>
    <img src="python-logo.png" alt="Python Logo">
    <div class="content">
        <a href="https://github.com">GitHub</a>
        <a href="https://stackoverflow.com">Stack Overflow</a>
    </div>
    <img src="banner.jpg" alt="Banner">
</body>
</html>
"""

parser = HTMLParser()

print("=" * 60)
print("提取链接")
print("=" * 60)
links = parser.extract_links(html)
for i, link in enumerate(links, 1):
    print(f"{i}. {link}")

print("\n" + "=" * 60)
print("提取图片")
print("=" * 60)
images = parser.extract_images(html)
for i, img in enumerate(images, 1):
    print(f"{i}. {img}")

print("\n" + "=" * 60)
print("提取纯文本")
print("=" * 60)
text = parser.extract_text(html)
print(text)

print("\n" + "=" * 60)
print("提取所有 <a> 标签")
print("=" * 60)
a_tags = parser.extract_tags(html, 'a')
for i, tag in enumerate(a_tags, 1):
    print(f"{i}. {tag['content']}")
    print(f"   链接：{tag['attributes'].get('href', 'N/A')}")
```

**输出**：

```
markdown复制============================================================
提取链接
============================================================
1. https://www.python.org
2. https://github.com
3. https://stackoverflow.com

============================================================
提取图片
============================================================
1. python-logo.png
2. banner.jpg

============================================================
提取纯文本
============================================================
测试页面 欢迎来到 Python 世界 这是一个Python教程。 GitHub Stack Overflow

============================================================
提取所有 <a> 标签
============================================================
1. Python
   链接：https://www.python.org
2. GitHub
   链接：https://github.com
3. Stack Overflow
   链接：https://stackoverflow.com
```

------

## 🎯 实战案例4：数字替换器

```
python复制import re

class NumberReplacer:
    """数字替换器"""
    
    def __init__(self):
        # 数字到中文的映射
        self.num_map = {
            '0': '零', '1': '一', '2': '二', '3': '三', '4': '四',
            '5': '五', '6': '六', '7': '七', '8': '八', '9': '九'
        }
        
        # 英文数字映射
        self.en_map = {
            '0': 'zero', '1': 'one', '2': 'two', '3': 'three', '4': 'four',
            '5': 'five', '6': 'six', '7': 'seven', '8': 'eight', '9': 'nine'
        }
    
    def to_chinese(self, text: str) -> str:
        """将数字转为中文"""
        return re.sub(r'\d', lambda m: self.num_map[m.group()], text)
    
    def to_english(self, text: str) -> str:
        """将数字转为英文"""
        return re.sub(r'\d', lambda m: self.en_map[m.group()], text)
    
    def hide_numbers(self, text: str) -> str:
        """隐藏所有数字"""
        return re.sub(r'\d', '*', text)
    
    def format_phone(self, phone: str) -> str:
        """格式化手机号：138-1234-5678"""
        if not re.match(r'^1[3-9]\d{9}$', phone):
            return phone
        return f"{phone[:3]}-{phone[3:7]}-{phone[7:]}"
    
    def format_money(self, text: str) -> str:
        """格式化金额：添加千分位"""
        def add_comma(match):
            num = match.group()
            # 添加千分位
            return f"{int(num):,}"
        
        return re.sub(r'\d+', add_comma, text)


# 测试
replacer = NumberReplacer()

test_text = "我有2个苹果和3个橙子，总共5个水果。"

print("=" * 60)
print("数字替换测试")
print("=" * 60)
print(f"原文：{test_text}")
print(f"中文：{replacer.to_chinese(test_text)}")
print(f"英文：{replacer.to_english(test_text)}")
print(f"隐藏：{replacer.hide_numbers(test_text)}")

print("\n" + "=" * 60)
print("手机号格式化")
print("=" * 60)
phones = ["13812345678", "13987654321"]
for phone in phones:
    print(f"{phone} → {replacer.format_phone(phone)}")

print("\n" + "=" * 60)
print("金额格式化")
print("=" * 60)
money_text = "销售额：1000000元，成本：500000元，利润：500000元"
print(f"原文：{money_text}")
print(f"格式化：{replacer.format_money(money_text)}")
```

**输出**：

```
diff复制============================================================
数字替换测试
============================================================
原文：我有2个苹果和3个橙子，总共5个水果。
中文：我有二个苹果和三个橙子，总共五个水果。
英文：我有two个苹果和three个橙子，总共five个水果。
隐藏：我有*个苹果和*个橙子，总共*个水果。

============================================================
手机号格式化
============================================================
13812345678 → 138-1234-5678
13987654321 → 139-8765-4321

============================================================
金额格式化
============================================================
原文：销售额：1000000元，成本：500000元，利润：500000元
格式化：销售额：1,000,000元，成本：500,000元，利润：500,000元
```

------

## 🎯 实战案例5：网页数据提取器

```
python复制import re
from typing import List, Dict

class WebScraper:
    """网页数据提取器"""
    
    @staticmethod
    def extract_urls(text: str) -> List[str]:
        """提取所有 URL"""
        pattern = r'https?://[^\s<>"]+|www\.[^\s<>"]+'
        return re.findall(pattern, text)
    
    @staticmethod
    def extract_emails(text: str) -> List[str]:
        """提取所有邮箱"""
        pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
        return re.findall(pattern, text)
    
    @staticmethod
    def extract_ip_addresses(text: str) -> List[str]:
        """提取所有 IP 地址"""
        pattern = r'\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b'
        ips = re.findall(pattern, text)
        # 验证 IP 地址有效性
        valid_ips = []
        for ip in ips:
            parts = ip.split('.')
            if all(0 <= int(part) <= 255 for part in parts):
                valid_ips.append(ip)
        return valid_ips
    
    @staticmethod
    def extract_dates(text: str) -> List[str]:
        """提取日期（YYYY-MM-DD 格式）"""
        pattern = r'\b\d{4}-\d{2}-\d{2}\b'
        return re.findall(pattern, text)
    
    @staticmethod
    def extract_prices(text: str) -> List[str]:
        """提取价格"""
        # 匹配：¥100, $99.99, 100元, 99.99美元
        patterns = [
            r'¥\d+(?:\.\d{2})?',
            r'\$\d+(?:\.\d{2})?',
            r'\d+(?:\.\d{2})?元',
            r'\d+(?:\.\d{2})?美元',
        ]
        prices = []
        for pattern in patterns:
            prices.extend(re.findall(pattern, text))
        return prices


# 测试
scraper = WebScraper()

sample_text = """
欢迎访问我们的网站：https://www.example.com 和 www.python.org
联系邮箱：support@example.com 或 admin@python.org
服务器IP：192.168.1.1 和 10.0.0.1
发布日期：2024-10-27，更新日期：2024-10-28
价格：¥99.99，原价：$199.99，折扣价：49元
"""

print("=" * 60)
print("网页数据提取")
print("=" * 60)

print("\n【URLs】")
for url in scraper.extract_urls(sample_text):
    print(f"  • {url}")

print("\n【邮箱】")
for email in scraper.extract_emails(sample_text):
    print(f"  • {email}")

print("\n【IP地址】")
for ip in scraper.extract_ip_addresses(sample_text):
    print(f"  • {ip}")

print("\n【日期】")
for date in scraper.extract_dates(sample_text):
    print(f"  • {date}")

print("\n【价格】")
for price in scraper.extract_prices(sample_text):
    print(f"  • {price}")
```

**输出**：

```
perl复制============================================================
网页数据提取
============================================================

【URLs】
  • https://www.example.com
  • www.python.org

【邮箱】
  • support@example.com
  • admin@python.org

【IP地址】
  • 192.168.1.1
  • 10.0.0.1

【日期】
  • 2024-10-27
  • 2024-10-28

【价格】
  • ¥99.99
  • $199.99
  • 49元
```

------

## 📚 第二节小结

### ✅ 你已经掌握了

1. **分组与捕获**
   - 基本分组 `()`
   - 命名分组 `(?P<name>...)`
   - 通过 `groupdict()` 获取字典
2. **贪婪 vs 非贪婪**
   - 默认贪婪：`.*`, `.+`, `*`, `+`
   - 非贪婪：`.*?`, `.+?`, `*?`, `+?`
3. **高级应用**
   - HTML 解析
   - 日志解析
   - 数据提取
   - 文本替换

### 🎯 完整练习项目

```
python复制"""
综合练习：用户信息提取器
从文本中提取并验证用户信息
"""

import re
from typing import Dict, List

class UserInfoExtractor:
    """用户信息提取器"""
    
    def __init__(self):
        self.patterns = {
            'name': r'姓名[：:]\s*([^\s，,。.]+)',
            'phone': r'(?:手机|电话)[：:]\s*(1[3-9]\d{9})',
            'email': r'邮箱[：:]\s*([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})',
            'id_card': r'身份证[：:]\s*(\d{17}[\dXx])',
            'address': r'地址[：:]\s*([^\n]+)',
        }
    
    def extract(self, text: str) -> Dict[str, str]:
        """提取用户信息"""
        info = {}
        for key, pattern in self.patterns.items():
            match = re.search(pattern, text)
            if match:
                info[key] = match.group(1).strip()
        return info
    
    def extract_all(self, text: str) -> List[Dict[str, str]]:
        """提取所有用户信息"""
        # 按段落分割
        paragraphs = re.split(r'\n\s*\n', text)
        return [self.extract(p) for p in paragraphs if p.strip()]


# 测试
extractor = UserInfoExtractor()

sample_data = """
姓名：张三
手机：13812345678
邮箱：zhangsan@example.com
身份证：110101199001011234
地址：北京市朝阳区XX路XX号

姓名：李四
电话：13987654321
邮箱：lisi@example.com
身份证：310101199002021234
地址：上海市浦东新区XX路XX号
"""

print("=" * 60)
print("用户信息提取")
print("=" * 60)

users = extractor.extract_all(sample_data)
for i, user in enumerate(users, 1):
    print(f"\n用户 {i}：")
    for key, value in user.items():
        print(f"  {key:10}: {value}")
```

------

## 🎓 课程总结

### Java vs Python 正则对比

| 特性     | Java                       | Python                      |
| -------- | -------------------------- | --------------------------- |
| **导入** | `import java.util.regex.*` | `import re`                 |
| **编译** | `Pattern.compile()`        | `re.compile()`              |
| **匹配** | `matcher.find()`           | `re.search()`               |
| **替换** | `matcher.replaceAll()`     | `re.sub()`                  |
| **分割** | `pattern.split()`          | `re.split()`                |
| **转义** | `\\d`                      | `r"\d"`                     |
| **分组** | `group(1)`                 | `group(1)` 或 `groupdict()` |

### 🎯 学习路径

```
复制基础 → 进阶 → 实战
  ↓      ↓      ↓
方法   分组   项目
验证   捕获   爬虫
提取   替换   解析
```

### 📚 推荐资源

1. **在线测试工具**
   - [Regex101](https://regex101.com/) - 在线测试和调试
   - [RegExr](https://regexr.com/) - 可视化正则表达式
2. **参考文档**
   - [Python re 官方文档](https://docs.python.org/zh-cn/3/library/re.html)
   - [正则表达式30分钟入门教程](https://deerchao.cn/tutorials/regex/regex.htm)

------

恭喜你完成了正则表达式课程！🎉

现在你已经可以：

- ✅ 验证各种格式（手机号、邮箱、身份证等）
- ✅ 从文本中提取数据
- ✅ 解析和处理复杂文本
- ✅ 构建实用的文本处理工具

继续练习，多写代码，你会越来越熟练！💪