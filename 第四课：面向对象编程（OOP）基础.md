# 第四课：面向对象编程（OOP）基础

**预计时长：180分钟**
 **难度等级：⭐⭐⭐**

------

## 📚 课程目录

1. [类与对象的概念](#41-类与对象的概念-30分钟)
2. [封装](#42-封装-25分钟)
3. [继承](#43-继承-30分钟)
4. [多态](#44-多态-25分钟)
5. [构造函数与析构函数](#45-构造函数与析构函数-20分钟)
6. [类属性与实例属性](#46-类属性与实例属性-20分钟)
7. [静态方法与类方法](#47-静态方法与类方法-20分钟)
8. [实战项目：银行账户管理系统](#48-实战项目银行账户管理系统-40分钟)

------

## 4.1 类与对象的概念 (30分钟)

### 4.1.1 什么是面向对象编程？

**面向对象编程（OOP）** 是一种编程范式，它将数据和操作数据的方法组织在一起，形成"对象"。

#### 核心概念

```
markdown复制现实世界                    编程世界
─────────                  ─────────
🚗 汽车                     Car 类
  - 品牌：Tesla              - brand: String
  - 颜色：红色               - color: String
  - 速度：0                  - speed: int
  - 启动()                   - start()
  - 加速()                   - accelerate()
  - 刹车()                   - brake()
```

**类（Class）**：对象的模板/蓝图
 **对象（Object）**：类的实例

------

### 4.1.2 第一个类

#### Python实现

```python
# 定义一个简单的类
class Dog:
    """狗类"""
    
    def __init__(self, name, age):
        """构造函数"""
        self.name = name  # 实例属性
        self.age = age
    
    def bark(self):
        """狗叫方法"""
        print(f"{self.name} says: Woof! Woof!")
    
    def get_info(self):
        """获取狗的信息"""
        return f"{self.name} is {self.age} years old"


# 创建对象（实例化）
my_dog = Dog("Buddy", 3)
your_dog = Dog("Max", 5)

# 使用对象
print(my_dog.name)        # Buddy
print(my_dog.age)         # 3
my_dog.bark()             # Buddy says: Woof! Woof!
print(my_dog.get_info())  # Buddy is 3 years old

print(your_dog.name)      # Max
your_dog.bark()           # Max says: Woof! Woof!
```

#### Java实现

```java
// 定义一个简单的类
public class Dog {
    // 实例属性（成员变量）
    private String name;
    private int age;
    
    // 构造函数
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 方法
    public void bark() {
        System.out.println(name + " says: Woof! Woof!");
    }
    
    public String getInfo() {
        return name + " is " + age + " years old";
    }
    
    // Getter和Setter
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
}

// 使用类
public class Main {
    public static void main(String[] args) {
        // 创建对象
        Dog myDog = new Dog("Buddy", 3);
        Dog yourDog = new Dog("Max", 5);
        
        // 使用对象
        System.out.println(myDog.getName());    // Buddy
        System.out.println(myDog.getAge());     // 3
        myDog.bark();                           // Buddy says: Woof! Woof!
        System.out.println(myDog.getInfo());    // Buddy is 3 years old
        
        System.out.println(yourDog.getName());  // Max
        yourDog.bark();                         // Max says: Woof! Woof!
    }
}
```

------

### 4.1.3 类与对象的关系

```python
# Python示例：理解类与对象的关系

class Car:
    """汽车类"""
    
    def __init__(self, brand, color):
        self.brand = brand
        self.color = color
        self.speed = 0
    
    def accelerate(self, increment):
        """加速"""
        self.speed += increment
        print(f"{self.brand} accelerates to {self.speed} km/h")
    
    def brake(self):
        """刹车"""
        self.speed = 0
        print(f"{self.brand} stops")


# 类是模板
print(type(Car))  # <class 'type'>

# 创建多个对象
car1 = Car("Tesla", "Red")
car2 = Car("BMW", "Black")
car3 = Car("Audi", "White")

# 每个对象都是独立的
print(type(car1))  # <class '__main__.Car'>
print(car1.brand)  # Tesla
print(car2.brand)  # BMW

# 对象有自己的状态
car1.accelerate(50)  # Tesla accelerates to 50 km/h
car2.accelerate(80)  # BMW accelerates to 80 km/h

print(car1.speed)  # 50
print(car2.speed)  # 80

# 验证对象的独立性
print(id(car1))  # 不同的内存地址
print(id(car2))  # 不同的内存地址
print(car1 == car2)  # False
java复制// Java示例：理解类与对象的关系

public class Car {
    private String brand;
    private String color;
    private int speed;
    
    public Car(String brand, String color) {
        this.brand = brand;
        this.color = color;
        this.speed = 0;
    }
    
    public void accelerate(int increment) {
        this.speed += increment;
        System.out.println(brand + " accelerates to " + speed + " km/h");
    }
    
    public void brake() {
        this.speed = 0;
        System.out.println(brand + " stops");
    }
    
    public String getBrand() {
        return brand;
    }
    
    public int getSpeed() {
        return speed;
    }
}

public class Main {
    public static void main(String[] args) {
        // 类是模板
        System.out.println(Car.class);  // class Car
        
        // 创建多个对象
        Car car1 = new Car("Tesla", "Red");
        Car car2 = new Car("BMW", "Black");
        Car car3 = new Car("Audi", "White");
        
        // 每个对象都是独立的
        System.out.println(car1.getClass());  // class Car
        System.out.println(car1.getBrand());  // Tesla
        System.out.println(car2.getBrand());  // BMW
        
        // 对象有自己的状态
        car1.accelerate(50);  // Tesla accelerates to 50 km/h
        car2.accelerate(80);  // BMW accelerates to 80 km/h
        
        System.out.println(car1.getSpeed());  // 50
        System.out.println(car2.getSpeed());  // 80
        
        // 验证对象的独立性
        System.out.println(System.identityHashCode(car1));  // 不同的哈希码
        System.out.println(System.identityHashCode(car2));  // 不同的哈希码
        System.out.println(car1 == car2);  // false
    }
}
```

------

## 4.2 封装 (25分钟)

### 4.2.1 什么是封装？

**封装（Encapsulation）** 是将数据和操作数据的方法绑定在一起，并隐藏内部实现细节。

#### 封装的三个层次

```vbnet
公开（Public）    ← 任何地方都可以访问
    ↓
保护（Protected） ← 子类可以访问
    ↓
私有（Private）   ← 只有类内部可以访问
```

------

### 4.2.2 Python中的封装

```python
class BankAccount:
    """银行账户类 - 演示封装"""
    
    def __init__(self, account_number, owner, balance=0):
        self.account_number = account_number  # 公开属性
        self._owner = owner                   # 受保护属性（约定）
        self.__balance = balance              # 私有属性（名称改编）
    
    # 公开方法
    def deposit(self, amount):
        """存款"""
        if amount > 0:
            self.__balance += amount
            print(f"Deposited ${amount}. New balance: ${self.__balance}")
            return True
        else:
            print("Invalid amount!")
            return False
    
    def withdraw(self, amount):
        """取款"""
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
            print(f"Withdrew ${amount}. New balance: ${self.__balance}")
            return True
        else:
            print("Invalid amount or insufficient funds!")
            return False
    
    # Getter方法
    def get_balance(self):
        """获取余额"""
        return self.__balance
    
    # Setter方法
    def set_balance(self, balance):
        """设置余额（需要验证）"""
        if balance >= 0:
            self.__balance = balance
        else:
            print("Balance cannot be negative!")
    
    # 私有方法
    def __validate_transaction(self, amount):
        """验证交易（私有方法）"""
        return amount > 0 and amount <= self.__balance
    
    def __str__(self):
        """字符串表示"""
        return f"Account {self.account_number}: {self._owner}, Balance: ${self.__balance}"


# 使用封装
account = BankAccount("123456", "Alice", 1000)

# 公开属性可以直接访问
print(account.account_number)  # 123456

# 受保护属性（约定不应该直接访问，但技术上可以）
print(account._owner)  # Alice

# 私有属性无法直接访问
# print(account.__balance)  # AttributeError

# 通过公开方法访问私有属性
print(account.get_balance())  # 1000

# 存款和取款
account.deposit(500)   # Deposited $500. New balance: $1500
account.withdraw(200)  # Withdrew $200. New balance: $1300

# Python的名称改编机制
# 私有属性实际上被改名为 _ClassName__attribute
print(account._BankAccount__balance)  # 1300（不推荐这样做！）

print(account)  # Account 123456: Alice, Balance: $1300
```

------

### 4.2.3 Java中的封装

```java
public class BankAccount {
    // 私有属性
    private String accountNumber;
    private String owner;
    private double balance;
    
    // 构造函数
    public BankAccount(String accountNumber, String owner, double balance) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = balance >= 0 ? balance : 0;
    }
    
    // 重载构造函数
    public BankAccount(String accountNumber, String owner) {
        this(accountNumber, owner, 0);
    }
    
    // 公开方法
    public boolean deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited $" + amount + ". New balance: $" + balance);
            return true;
        } else {
            System.out.println("Invalid amount!");
            return false;
        }
    }
    
    public boolean withdraw(double amount) {
        if (validateTransaction(amount)) {
            balance -= amount;
            System.out.println("Withdrew $" + amount + ". New balance: $" + balance);
            return true;
        } else {
            System.out.println("Invalid amount or insufficient funds!");
            return false;
        }
    }
    
    // Getter方法
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getOwner() {
        return owner;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Setter方法（带验证）
    public void setOwner(String owner) {
        if (owner != null && !owner.isEmpty()) {
            this.owner = owner;
        }
    }
    
    // 私有方法
    private boolean validateTransaction(double amount) {
        return amount > 0 && amount <= balance;
    }
    
    @Override
    public String toString() {
        return String.format("Account %s: %s, Balance: $%.2f", 
            accountNumber, owner, balance);
    }
}

// 使用封装
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123456", "Alice", 1000);
        
        // 无法直接访问私有属性
        // System.out.println(account.balance);  // 编译错误
        
        // 通过公开方法访问
        System.out.println(account.getAccountNumber());  // 123456
        System.out.println(account.getOwner());          // Alice
        System.out.println(account.getBalance());        // 1000.0
        
        // 存款和取款
        account.deposit(500);   // Deposited $500.0. New balance: $1500.0
        account.withdraw(200);  // Withdrew $200.0. New balance: $1300.0
        
        System.out.println(account);  // Account 123456: Alice, Balance: $1300.00
    }
}
```

------

### 4.2.4 封装的优点

```python
# 示例：为什么需要封装？

# ❌ 不好的设计（没有封装）
class BadBankAccount:
    def __init__(self, balance):
        self.balance = balance  # 公开属性

# 问题：可以随意修改余额
bad_account = BadBankAccount(1000)
bad_account.balance = -500  # 余额变成负数！
bad_account.balance = 999999999  # 随意修改余额！


# ✓ 好的设计（使用封装）
class GoodBankAccount:
    def __init__(self, balance):
        self.__balance = balance if balance >= 0 else 0
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False
    
    def get_balance(self):
        return self.__balance

# 优点：
# 1. 数据验证
good_account = GoodBankAccount(1000)
# good_account.__balance = -500  # 无法直接修改

# 2. 控制访问
good_account.withdraw(2000)  # 失败，余额不足

# 3. 易于维护
# 如果需要修改内部实现，不影响外部代码
```

------

## 4.3 继承 (30分钟)

### 4.3.1 什么是继承？

**继承（Inheritance）** 允许一个类获得另一个类的属性和方法。

```markdown
父类（基类/超类）
    ↓ 继承
子类（派生类）
```

------

### 4.3.2 Python中的继承

```python
# 父类（基类）
class Animal:
    """动物类"""
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def eat(self):
        print(f"{self.name} is eating")
    
    def sleep(self):
        print(f"{self.name} is sleeping")
    
    def make_sound(self):
        print("Some generic animal sound")
    
    def __str__(self):
        return f"{self.name} ({self.age} years old)"


# 子类1：狗
class Dog(Animal):
    """狗类 - 继承自Animal"""
    
    def __init__(self, name, age, breed):
        super().__init__(name, age)  # 调用父类构造函数
        self.breed = breed
    
    # 重写父类方法
    def make_sound(self):
        print(f"{self.name} says: Woof! Woof!")
    
    # 新增方法
    def fetch(self):
        print(f"{self.name} is fetching the ball")
    
    def __str__(self):
        return f"{self.name} ({self.breed}, {self.age} years old)"


# 子类2：猫
class Cat(Animal):
    """猫类 - 继承自Animal"""
    
    def __init__(self, name, age, color):
        super().__init__(name, age)
        self.color = color
    
    # 重写父类方法
    def make_sound(self):
        print(f"{self.name} says: Meow! Meow!")
    
    # 新增方法
    def scratch(self):
        print(f"{self.name} is scratching")
    
    def __str__(self):
        return f"{self.name} ({self.color} cat, {self.age} years old)"


# 使用继承
print("=== 使用父类 ===")
animal = Animal("Generic Animal", 5)
print(animal)
animal.eat()
animal.make_sound()

print("\n=== 使用子类：Dog ===")
dog = Dog("Buddy", 3, "Golden Retriever")
print(dog)
dog.eat()          # 继承自父类
dog.sleep()        # 继承自父类
dog.make_sound()   # 重写的方法
dog.fetch()        # 新增的方法

print("\n=== 使用子类：Cat ===")
cat = Cat("Whiskers", 2, "Orange")
print(cat)
cat.eat()          # 继承自父类
cat.make_sound()   # 重写的方法
cat.scratch()      # 新增的方法

# 验证继承关系
print("\n=== 验证继承关系 ===")
print(isinstance(dog, Dog))      # True
print(isinstance(dog, Animal))   # True
print(isinstance(cat, Cat))      # True
print(isinstance(cat, Animal))   # True
print(isinstance(dog, Cat))      # False

print(issubclass(Dog, Animal))   # True
print(issubclass(Cat, Animal))   # True
print(issubclass(Dog, Cat))      # False
```

------

### 4.3.3 Java中的继承

```python
x// 父类（基类）
public class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    public void makeSound() {
        System.out.println("Some generic animal sound");
    }
    
    @Override
    public String toString() {
        return name + " (" + age + " years old)";
    }
}

// 子类1：狗
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // 调用父类构造函数
        this.breed = breed;
    }
    
    // 重写父类方法
    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof! Woof!");
    }
    
    // 新增方法
    public void fetch() {
        System.out.println(name + " is fetching the ball");
    }
    
    @Override
    public String toString() {
        return name + " (" + breed + ", " + age + " years old)";
    }
}

// 子类2：猫
public class Cat extends Animal {
    private String color;
    
    public Cat(String name, int age, String color) {
        super(name, age);
        this.color = color;
    }
    
    // 重写父类方法
    @Override
    public void makeSound() {
        System.out.println(name + " says: Meow! Meow!");
    }
    
    // 新增方法
    public void scratch() {
        System.out.println(name + " is scratching");
    }
    
    @Override
    public String toString() {
        return name + " (" + color + " cat, " + age + " years old)";
    }
}

// 使用继承
public class Main {
    public static void main(String[] args) {
        System.out.println("=== 使用父类 ===");
        Animal animal = new Animal("Generic Animal", 5);
        System.out.println(animal);
        animal.eat();
        animal.makeSound();
        
        System.out.println("\n=== 使用子类：Dog ===");
        Dog dog = new Dog("Buddy", 3, "Golden Retriever");
        System.out.println(dog);
        dog.eat();          // 继承自父类
        dog.sleep();        // 继承自父类
        dog.makeSound();    // 重写的方法
        dog.fetch();        // 新增的方法
        
        System.out.println("\n=== 使用子类：Cat ===");
        Cat cat = new Cat("Whiskers", 2, "Orange");
        System.out.println(cat);
        cat.eat();          // 继承自父类
        cat.makeSound();    // 重写的方法
        cat.scratch();      // 新增的方法
        
        // 验证继承关系
        System.out.println("\n=== 验证继承关系 ===");
        System.out.println(dog instanceof Dog);      // true
        System.out.println(dog instanceof Animal);   // true
        System.out.println(cat instanceof Cat);      // true
        System.out.println(cat instanceof Animal);   // true
        System.out.println(dog instanceof Cat);      // false
    }
}
```

------

### 4.3.4 多层继承

```python
# Python多层继承示例

class LivingBeing:
    """生物类"""
    def __init__(self, name):
        self.name = name
    
    def breathe(self):
        print(f"{self.name} is breathing")


class Animal(LivingBeing):
    """动物类"""
    def __init__(self, name, age):
        super().__init__(name)
        self.age = age
    
    def move(self):
        print(f"{self.name} is moving")


class Mammal(Animal):
    """哺乳动物类"""
    def __init__(self, name, age, fur_color):
        super().__init__(name, age)
        self.fur_color = fur_color
    
    def feed_young(self):
        print(f"{self.name} is feeding its young")


class Dog(Mammal):
    """狗类"""
    def __init__(self, name, age, fur_color, breed): 
        super().__init__(name, age, fur_color)
        self.breed = breed
    
    def bark(self):
        print(f"{self.name} says: Woof!")


# 使用多层继承
dog = Dog("Max", 3, "Brown", "Labrador")
dog.breathe()      # 从LivingBeing继承
dog.move()         # 从Animal继承
dog.feed_young()   # 从Mammal继承
dog.bark()         # Dog自己的方法

# 继承链
print(Dog.__mro__)  # 方法解析顺序
# (<class '__main__.Dog'>, <class '__main__.Mammal'>, 
#  <class '__main__.Animal'>, <class '__main__.LivingBeing'>, 
#  <class 'object'>)
java复制// Java多层继承示例

class LivingBeing {
    protected String name;
    
    public LivingBeing(String name) {
        this.name = name;
    }
    
    public void breathe() {
        System.out.println(name + " is breathing");
    }
}

class Animal extends LivingBeing {
    protected int age;
    
    public Animal(String name, int age) {
        super(name);
        this.age = age;
    }
    
    public void move() {
        System.out.println(name + " is moving");
    }
}

class Mammal extends Animal {
    protected String furColor;
    
    public Mammal(String name, int age, String furColor) {
        super(name, age);
        this.furColor = furColor;
    }
    
    public void feedYoung() {
        System.out.println(name + " is feeding its young");
    }
}

class Dog extends Mammal {
    private String breed;
    
    public Dog(String name, int age, String furColor, String breed) {
        super(name, age, furColor);
        this.breed = breed;
    }
    
    public void bark() {
        System.out.println(name + " says: Woof!");
    }
}

// 使用多层继承
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Max", 3, "Brown", "Labrador");
        dog.breathe();      // 从LivingBeing继承
        dog.move();         // 从Animal继承
        dog.feedYoung();    // 从Mammal继承
        dog.bark();         // Dog自己的方法
    }
}
```

------

## 4.4 多态 (25分钟)

### 4.4.1 什么是多态？

**多态（Polymorphism）** 意味着"多种形态"，允许不同类的对象对同一消息做出不同的响应。

```markdown
多态的两种形式：
1. 方法重写（Override）- 运行时多态
2. 方法重载（Overload）- 编译时多态
```

------

### 4.4.2 Python中的多态

```python
# Python多态示例

class Shape:
    """形状基类"""
    def __init__(self, name):
        self.name = name
    
    def area(self):
        """计算面积（子类需要重写）"""
        raise NotImplementedError("Subclass must implement area()")
    
    def perimeter(self):
        """计算周长（子类需要重写）"""
        raise NotImplementedError("Subclass must implement perimeter()")


class Circle(Shape):
    """圆形"""
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius


class Rectangle(Shape):
    """矩形"""
    def __init__(self, width, height):
        super().__init__("Rectangle")
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)


class Triangle(Shape):
    """三角形"""
    def __init__(self, a, b, c):
        super().__init__("Triangle")
        self.a = a
        self.b = b
        self.c = c
    
    def area(self):
        # 使用海伦公式
        s = (self.a + self.b + self.c) / 2
        import math
        return math.sqrt(s * (s - self.a) * (s - self.b) * (s - self.c))
    
    def perimeter(self):
        return self.a + self.b + self.c


# 多态的威力：统一处理不同类型的对象
def print_shape_info(shape):
    """打印形状信息（多态函数）"""
    print(f"\n{shape.name}:")
    print(f"  Area: {shape.area():.2f}")
    print(f"  Perimeter: {shape.perimeter():.2f}")


# 创建不同类型的形状
shapes = [
    Circle(5),
    Rectangle(4, 6),
    Triangle(3, 4, 5)
]

# 统一处理（多态）
print("=== 多态示例 ===")
for shape in shapes:
    print_shape_info(shape)

# 输出：
# Circle:
 Area: 78.54
#   Perimeter: 31.42
#
# Rectangle:
#   Area: 24.00
#   Perimeter: 20.00
#
# Triangle:
#   Area: 6.00
#   Perimeter: 12.00


# 鸭子类型（Duck Typing）- Python特有的多态
class Duck:
    def speak(self):
        print("Quack!")

class Dog:
    def speak(self):
        print("Woof!")

class Cat:
    def speak(self):
        print("Meow!")

class Robot:
    def speak(self):
        print("Beep boop!")

# 不需要继承关系，只要有speak方法就行
def make_it_speak(obj):
    obj.speak()

animals = [Duck(), Dog(), Cat(), Robot()]
for animal in animals:
    make_it_speak(animal)
# Quack!
# Woof!
# Meow!
# Beep boop!
```

------

4.4.3 Java中的多态

```java
// Java多态示例

// 抽象基类
abstract class Shape {
    protected String name;
    
    public Shape(String name) {
        this.name = name;
    }
    
    // 抽象方法（子类必须实现）
    public abstract double area();
    public abstract double perimeter();
    
    public String getName() {
        return name;
    }
}

// 圆形
class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        super("Circle");
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double perimeter() {
        return 2 * Math.PI * radius;
    }
}

// 矩形
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        super("Rectangle");
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double area() {
        return width * height;
    }
    
    @Override
    public double perimeter() {
        return 2 * (width + height);
    }
}

// 三角形
class Triangle extends Shape {
    private double a, b, c;
    
    public Triangle(double a, double b, double c) {
        super("Triangle");
        this.a = a;
        this.b = b;
        this.c = c;
    }
    
    @Override
    public double area() {
        // 海伦公式
        double s = (a + b + c) / 2;
        return Math.sqrt(s * (s - a) * (s - b) * (s - c));
    }
    
    @Override
    public double perimeter() {
        return a + b + c;
    }
}

// 多态的威力
public class PolymorphismDemo {
    // 多态方法：接受Shape类型，可以处理所有子类
    public static void printShapeInfo(Shape shape) {
        System.out.println("\n" + shape.getName() + ":");
        System.out.printf("  Area: %.2f%n", shape.area());
        System.out.printf("  Perimeter: %.2f%n", shape.perimeter());
    }
    
    public static void main(String[] args) {
        // 创建不同类型的形状
        Shape[] shapes = {
            new Circle(5),
            new Rectangle(4, 6),
            new Triangle(3, 4, 5)
        };
        
        // 统一处理（多态）
        System.out.println("=== 多态示例 ===");
        for (Shape shape : shapes) {
            printShapeInfo(shape);
        }
        
        // 运行时类型判断
        System.out.println("\n=== 运行时类型 ===");
        for (Shape shape : shapes) {
            if (shape instanceof Circle) {
                System.out.println("这是一个圆形");
            } else if (shape instanceof Rectangle) {
                System.out.println("这是一个矩形");
            } else if (shape instanceof Triangle) {
                System.out.println("这是一个三角形");
            }
        }
    }
}
```

------

### 4.4.4 方法重载（Overload）

```python
# Python中的方法重载（通过默认参数实现）

class Calculator:
    """计算器类"""
    
    def add(self, a, b=None, c=None):
        """加法 - 支持2个或3个参数"""
        if c is not None:
            return a + b + c
        elif b is not None:
            return a + b
        else:
            return a
    
    # 更Pythonic的方式：使用*args
    def multiply(self, *args):
        """乘法 - 支持任意数量参数"""
        result = 1
        for num in args:
            result *= num
        return result


calc = Calculator()
print(calc.add(5))           # 5
print(calc.add(5, 3))        # 8
print(calc.add(5, 3, 2))     # 10

print(calc.multiply(2, 3))           # 6
print(calc.multiply(2, 3, 4))        # 24
print(calc.multiply(2, 3, 4, 5))     # 120
java复制// Java中的方法重载

public class Calculator {
    // 两个整数相加
    public int add(int a, int b) {
        return a + b;
    }
    
    // 三个整数相加
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // 两个浮点数相加
    public double add(double a, double b) {
        return a + b;
    }
    
    // 可变参数
    public int multiply(int... numbers) {
        int result = 1;
        for (int num : numbers) {
            result *= num;
        }
        return result;
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println(calc.add(5, 3));        // 8
        System.out.println(calc.add(5, 3, 2));     // 10
        System.out.println(calc.add(5.5, 3.2));    // 8.7
        
        System.out.println(calc.multiply(2, 3));           // 6
        System.out.println(calc.multiply(2, 3, 4));        // 24
        System.out.println(calc.multiply(2, 3, 4, 5));     // 120
    }
}
```

------

## 4.5 构造函数与析构函数 (20分钟)

### 4.5.1 构造函数

**构造函数（Constructor）** 在创建对象时自动调用，用于初始化对象。

#### Python构造函数

```python
class Person:
    """人类"""
    
    # 构造函数
    def __init__(self, name, age, city="Unknown"):
        """
        初始化Person对象
        :param name: 姓名
        :param age: 年龄
        :param city: 城市（可选）
        """
        print(f"Creating Person object: {name}")
        self.name = name
        self.age = age
        self.city = city
        self.id = id(self)  # 对象的唯一标识
    
    def __str__(self):
        return f"{self.name}, {self.age} years old, from {self.city}"


# 使用构造函数
person1 = Person("Alice", 25)
# 输出: Creating Person object: Alice

person2 = Person("Bob", 30, "New York")
# 输出: Creating Person object: Bob

print(person1)  # Alice, 25 years old, from Unknown
print(person2)  # Bob, 30 years old, from New York
```

#### Java构造函数

```java
public class Person {
    private String name;
    private int age;
    private String city;
    
    // 默认构造函数
    public Person() {
        this("Unknown", 0, "Unknown");
        System.out.println("Default constructor called");
    }
    
    // 带参数的构造函数
    public Person(String name, int age) {
        this(name, age, "Unknown");
    }
    
    // 完整构造函数
    public Person(String name, int age, String city) {
        System.out.println("Creating Person object: " + name);
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    @Override
    public String toString() {
        return name + ", " + age + " years old, from " + city;
    }
    
    public static void main(String[] args) {
        Person person1 = new Person("Alice", 25);
        // 输出: Creating Person object: Alice
        
        Person person2 = new Person("Bob", 30, "New York");
        // 输出: Creating Person object: Bob
        
        Person person3 = new Person();
        // 输出: Creating Person object: Unknown
        //       Default constructor called
        
        System.out.println(person1);  // Alice, 25 years old, from Unknown
        System.out.println(person2);  // Bob, 30 years old, from New York
    }
}
```

------

### 4.5.2 析构函数

**析构函数（Destructor）** 在对象被销毁时调用，用于清理资源。

#### Python析构函数

```python
class FileHandler:
    """文件处理类"""
    
    def __init__(self, filename):
        print(f"Opening file: {filename}")
        self.filename = filename
        self.file = open(filename, 'w')
    
    def write(self, content):
        self.file.write(content)
    
    # 析构函数
    def __del__(self):
        print(f"Closing file: {self.filename}")
        if hasattr(self, 'file'):
            self.file.close()


# 使用析构函数
handler = FileHandler("test.txt")
handler.write("Hello, World!")
del handler  # 手动删除对象，触发析构函数
# 输出: Opening file: test.txt
#       Closing file: test.txt


# 更好的方式：使用上下文管理器
class BetterFileHandler:
    def __init__(self, filename):
        self.filename = filename
        self.file = None
    
    def __enter__(self):
        print(f"Opening file: {self.filename}")
        self.file = open(self.filename, 'w')
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Closing file: {self.filename}")
        if self.file:
            self.file.close()
    
    def write(self, content):
        self.file.write(content)


# 使用with语句（推荐）
with BetterFileHandler("test2.txt") as handler:
    handler.write("Hello, World!")
# 自动调用__exit__，确保文件关闭
```

#### Java中的资源清理

```java
// Java没有析构函数，但有finalize()方法（已过时）
// 推荐使用try-with-resources

import java.io.*;

// 实现AutoCloseable接口
public class FileHandler implements AutoCloseable {
    private String filename;
    private BufferedWriter writer;
    
    public FileHandler(String filename) throws IOException {
        System.out.println("Opening file: " + filename);
        this.filename = filename;
        this.writer = new BufferedWriter(new FileWriter(filename));
    }
    
    public void write(String content) throws IOException {
        writer.write(content);
    }
    
    @Override
    public void close() throws IOException {
        System.out.println("Closing file: " + filename);
        if (writer != null) {
            writer.close();
        }
    }
    
    public static void main(String[] args) {
        // 使用try-with-resources（推荐）
        try (FileHandler handler = new FileHandler("test.txt")) {
            handler.write("Hello, World!");
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 自动调用close()方法
        
        // 输出:
        // Opening file: test.txt
        // Closing file: test.txt
    }
}
```

------

## 4.6 类属性与实例属性 (20分钟)

### 4.6.1 实例属性 vs 类属性

```python
# Python示例

class Dog:
    # 类属性（所有实例共享）
    species = "Canis familiaris"
    count = 0  # 统计创建了多少只狗
    
    def __init__(self, name, age):
        # 实例属性（每个实例独有）
        self.name = name
        self.age = age
        Dog.count += 1  # 修改类属性
    
    def description(self):
        return f"{self.name} is {self.age} years old"
    
    @classmethod
    def get_count(cls):
        return f"Total dogs created: {cls.count}"


# 创建实例
dog1 = Dog("Buddy", 3)
dog2 = Dog("Max", 5)
dog3 = Dog("Charlie", 2)

# 访问实例属性
print(dog1.name)  # Buddy
print(dog2.name)  # Max

# 访问类属性
print(dog1.species)  # Canis familiaris
print(Dog.species)   # Canis familiaris

# 类属性被所有实例共享
print(Dog.count)     # 3
print(dog1.count)    # 3
print(Dog.get_count())  # Total dogs created: 3

# 修改类属性
Dog.species = "Dog"
print(dog1.species)  # Dog（所有实例都受影响）
print(dog2.species)  # Dog

# 注意：给实例属性赋值不会影响类属性
dog1.species = "Special Dog"
print(dog1.species)  # Special Dog（实例属性）
print(dog2.species)  # Dog（类属性）
print(Dog.species)   # Dog（类属性）
java复制// Java示例

public class Dog {
    // 类属性（静态变量）
    public static String species = "Canis familiaris";
    private static int count = 0;
    
    // 实例属性（成员变量）
    private String name;
    private int age;
    
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
        count++;  // 修改类属性
    }
    
    public String description() {
        return name + " is " + age + " years old";
    }
    
    // 静态方法（类方法）
    public static int getCount() {
        return count;
    }
    
    // Getter
    public String getName() {
        return name;
    }
    
    public static void main(String[] args) {
        // 创建实例
        Dog dog1 = new Dog("Buddy", 3);
        Dog dog2 = new Dog("Max", 5);
        Dog dog3 = new Dog("Charlie", 2);
        
        // 访问实例属性
        System.out.println(dog1.getName());  // Buddy
        System.out.println(dog2.getName());  // Max
        
        // 访问类属性
        System.out.println(dog1.species);    // Canis familiaris
        System.out.println(Dog.species);     // Canis familiaris（推荐）
        
        // 类属性被所有实例共享
        System.out.println(Dog.count);       // 3（错误：count是private）
        System.out.println(Dog.getCount());  // 3（正确）
        
        // 修改类属性
        Dog.species = "Dog";
        System.out.println(dog1.species);    // Dog
        System.out.println(dog2.species);    // Dog
    }
}
```

------

### 4.6.2 类属性的应用场景

```python
# 示例1：计数器
class User:
    total_users = 0
    
    def __init__(self, username):
        self.username = username
        User.total_users += 1
    
    @classmethod
    def get_total_users(cls):
        return cls.total_users


# 示例2：配置信息
class DatabaseConnection:
    # 类属性：配置信息
    host = "localhost"
    port = 3306
    database = "mydb"
    
    def __init__(self, username, password):
        self.username = username
        self.password = password
    
    def connect(self):
        return f"Connecting to {self.database} at {self.host}:{self.port}"


# 示例3：常量
class MathConstants:
    PI = 3.14159265359
    E = 2.71828182846
    GOLDEN_RATIO = 1.61803398875


# 使用
user1 = User("alice")
user2 = User("bob")
print(User.get_total_users())  # 2

db = DatabaseConnection("admin", "password")
print(db.connect())  # Connecting to mydb at localhost:3306

print(MathConstants.PI)  # 3.14159265359
```

------

## 4.7 静态方法与类方法 (20分钟)

### 4.7.1 三种方法类型对比

```python
# Python示例

class MyClass:
    class_variable = "I'm a class variable"
    
    def __init__(self, value):
        self.instance_variable = value
    
    # 实例方法（需要self）
    def instance_method(self):
        """可以访问实例属性和类属性"""
        return f"Instance: {self.instance_variable}, Class: {self.class_variable}"
    
    # 类方法（需要cls）
    @classmethod
    def class_method(cls):
        """可以访问类属性，不能访问实例属性"""
        return f"Class: {cls.class_variable}"
    
    # 静态方法（不需要self或cls）
    @staticmethod
    def static_method(x, y):
        """独立的工具函数，不访问类或实例属性"""
        return x + y


# 使用
obj = MyClass("Hello")

# 实例方法：通过实例调用
print(obj.instance_method())  
# Instance: Hello, Class: I'm a class variable

# 类方法：可以通过类或实例调用
print(MyClass.class_method())  # Class: I'm a class variable
print(obj.class_method())      # Class: I'm a class variable

# 静态方法：可以通过类或实例调用
print(MyClass.static_method(5, 3))  # 8
print(obj.static_method(5, 3))      # 8
java复制// Java示例

public class MyClass {
    private static String classVariable = "I'm a class variable";
    private String instanceVariable;
    
    public MyClass(String value) {
        this.instanceVariable = value;
    }
    
    // 实例方法
    public String instanceMethod() {
        return "Instance: " + instanceVariable + ", Class: " + classVariable;
    }
    
    // 静态方法
    public static String staticMethod(int x, int y) {
        // 只能访问静态变量
        return "Result: " + (x + y) + ", Class: " + classVariable;
    }
    
    public static void main(String[] args) {
        MyClass obj = new MyClass("Hello");
        
        // 实例方法：通过实例调用
        System.out.println(obj.instanceMethod());
        // Instance: Hello, Class: I'm a class variable
        
        // 静态方法：通过类调用（推荐）
        System.out.println(MyClass.staticMethod(5, 3));
        // Result: 8, Class: I'm a class variable
        
        // 静态方法也可以通过实例调用（不推荐）
        System.out.println(obj.staticMethod(5, 3));
    }
}
```

------

### 4.7.2 实际应用示例

```python
# Python实际应用

from datetime import datetime

class Person:
    def __init__(self, name, birth_year):
        self.name = name
        self.birth_year = birth_year
    
    # 实例方法
    def get_age(self):
        """计算年龄"""
        current_year = datetime.now().year
        return current_year - self.birth_year
    
    # 类方法：工厂方法
    @classmethod
    def from_birth_date(cls, name, birth_date):
        """从出生日期创建Person对象"""
        birth_year = datetime.strptime(birth_date, "%Y-%m-%d").year
        return cls(name, birth_year)
    
    # 静态方法：工具函数
    @staticmethod
    def is_adult(age):
        """判断是否成年"""
        return age >= 18
    
    def __str__(self):
        return f"{self.name}, born in {self.birth_year}"


# 使用实例方法
person1 = Person("Alice", 1990)
print(person1.get_age())  # 35（假设当前是2025年）

# 使用类方法（工厂方法）
person2 = Person.from_birth_date("Bob", "1995-05-15")
print(person2)  # Bob, born in 1995

# 使用静态方法
print(Person.is_adult(20))  # True
print(Person.is_adult(15))  # False


# 更多类方法示例：单例模式
class Singleton:
    _instance = None
    
    def __init__(self):
        if Singleton._instance is not None:
            raise Exception("This is a singleton class!")
        Singleton._instance = self
    
    @classmethod
    def get_instance(cls):
        """获取单例实例"""
        if cls._instance is None:
            cls._instance = cls()
        return cls._instance


# 使用单例
s1 = Singleton.get_instance()
s2 = Singleton.get_instance()
print(s1 is s2)  # True（同一个实例）

-------------------------------------------------------java------------------------------------------------

// Java实际应用

import java.time.*;
import java.time.format.DateTimeFormatter;

public class Person {
    private String name;
    private int birthYear;
    
    public Person(String name, int birthYear) {
        this.name = name;
        this.birthYear = birthYear;
    }
    
    // 实例方法
    public int getAge() {
        int currentYear = LocalDate.now().getYear();
        return currentYear - birthYear;
    }
    
    // 静态工厂方法
    public static Person fromBirthDate(String name, String birthDate) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        LocalDate date = LocalDate.parse(birthDate, formatter);
        return new Person(name, date.getYear());
    }
    
    // 静态方法：工具函数
    public static boolean isAdult(int age) {
        return age >= 18;
    }
    
    @Override
    public String toString() {
        return name + ", born in " + birthYear;
    }
    
    public static void main(String[] args) {
        // 使用实例方法
        Person person1 = new Person("Alice", 1990);
        System.out.println(person1.getAge());  // 35
        
        // 使用静态工厂方法
        Person person2 = Person.fromBirthDate("Bob", "1995-05-15");
        System.out.println(person2);  // Bob, born in 1995
        
        // 使用静态方法
        System.out.println(Person.isAdult(20));  // true
        System.out.println(Person.isAdult(15));  // false
    }
}


// 单例模式示例
class Singleton {
    private static Singleton instance;
    
    private Singleton() {
        // 私有构造函数
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

------

## 4.8 实战项目：银行账户管理系统 (40分钟)

### 4.8.1 项目需求

**功能需求：**

1. 创建账户（储蓄账户、支票账户）
2. 存款、取款
3. 转账
4. 查询余额和交易历史
5. 计算利息（储蓄账户）
6. 透支保护（支票账户）

**技术要点：**

- 继承：Account基类，SavingsAccount和CheckingAccount子类
- 封装：私有属性，公开方法
- 多态：统一的账户接口
- 类方法：生成账户号
- 静态方法：验证金额

------

### 4.8.2 Python完整实现

```python
from datetime import datetime
from typing import List
import json

class Transaction:
    """交易记录类"""
    
    def __init__(self, trans_type, amount, balance, description=""):
        self.type = trans_type  # "deposit", "withdraw", "transfer"
        self.amount = amount
        self.balance = balance
        self.description = description
        self.timestamp = datetime.now()
    
    def __str__(self):
        return (f"[{self.timestamp.strftime('%Y-%m-%d %H:%M:%S')}] "
                f"{self.type.upper()}: ${self.amount:.2f} "
                f"(Balance: ${self.balance:.2f}) {self.description}")
    
    def to_dict(self):
        """转换为字典"""
        return {
            'type': self.type,
            'amount': self.amount,
            'balance': self.balance,
            'description': self.description,
            'timestamp': self.timestamp.isoformat()
        }


class Account:
    """账户基类"""
    
    _account_counter = 1000  # 类属性：账户号计数器
    
    def __init__(self, owner, initial_balance=0):
        self.account_number = Account._generate_account_number()
        self.owner = owner
        self._balance = max(0, initial_balance)
        self._transactions: List[Transaction] = []
        self._is_active = True
        
        if initial_balance > 0:
            self._add_transaction("deposit", initial_balance, "Initial deposit")
    
    @classmethod
    def _generate_account_number(cls):
        """生成账户号（类方法）"""
        cls._account_counter += 1
        return f"ACC{cls._account_counter:06d}"
    
    @staticmethod
    def _validate_amount(amount):
        """验证金额（静态方法）"""
        if not isinstance(amount, (int, float)):
            raise ValueError("Amount must be a number")
        if amount <= 0:
            raise ValueError("Amount must be positive")
        return True
    
    def _add_transaction(self, trans_type, amount, description=""):
        """添加交易记录（私有方法）"""
        transaction = Transaction(trans_type, amount, self._balance, description)
        self._transactions.append(transaction)
    
    def deposit(self, amount):
        """存款"""
        if not self._is_active:
            print("Account is closed!")
            return False
        
        try:
            self._validate_amount(amount)
            self._balance += amount
            self._add_transaction("deposit", amount)
            print(f"Deposited ${amount:.2f}. New balance: ${self._balance:.2f}")
            return True
        except ValueError as e:
            print(f"Deposit failed: {e}")
            return False
    
    def withdraw(self, amount):
        """取款（子类可能重写）"""
        if not self._is_active:
            print("Account is closed!")
            return False
        
        try:
            self._validate_amount(amount)
            if amount > self._balance:
                print("Insufficient funds!")
                return False
            
            self._balance -= amount
            self._add_transaction("withdraw", amount)
            print(f"Withdrew ${amount:.2f}. New balance: ${self._balance:.2f}")
            return True
        except ValueError as e:
            print(f"Withdrawal failed: {e}")
            return False
    
    def transfer(self, target_account, amount):
        """转账"""
        if not self._is_active:
            print("Account is closed!")
            return False
        
        if self.withdraw(amount):
            target_account.deposit(amount)
            self._add_transaction("transfer", amount, 
                                f"To {target_account.account_number}")
            target_account._add_transaction("transfer", amount,
                                          f"From {self.account_number}")
            print(f"Transferred ${amount:.2f} to {target_account.account_number}")
            return True
        return False
    
    def get_balance(self):
        """获取余额"""
        return self._balance
    
    def get_transaction_history(self, limit=None):
        """获取交易历史"""
        transactions = self._transactions[-limit:] if limit else self._transactions
        return transactions
   
    def print_statement(self):
        """打印账户对账单"""
        print("\n" + "="*60)
        print(f"ACCOUNT STATEMENT - {self.account_number}")
        print("="*60)
        print(f"Owner: {self.owner}")
        print(f"Account Type: {self.__class__.__name__}")
        print(f"Current Balance: ${self._balance:.2f}")
        print(f"Status: {'Active' if self._is_active else 'Closed'}")
        print("-"*60)
        print("Recent Transactions:")
        
        if not self._transactions:
            print("  No transactions yet")
        else:
            for trans in self._transactions[-10:]:  # 显示最近10条
                print(f"  {trans}")
        print("="*60 + "\n")
    
    def close_account(self):
        """关闭账户"""
        self._is_active = False
        print(f"Account {self.account_number} has been closed")
    
    def __str__(self):
        return (f"{self.__class__.__name__}({self.account_number}): "
                f"{self.owner}, Balance: ${self._balance:.2f}")


class SavingsAccount(Account):
    """储蓄账户"""
    
    def __init__(self, owner, initial_balance=0, interest_rate=0.02):
        super().__init__(owner, initial_balance)
        self.interest_rate = interest_rate  # 年利率
        self.min_balance = 100  # 最低余额
    
    def withdraw(self, amount):
        """重写取款方法：检查最低余额"""
        if not self._is_active:
            print("Account is closed!")
            return False
        
        try:
            self._validate_amount(amount)
            if self._balance - amount < self.min_balance:
                print(f"Cannot withdraw: minimum balance of ${self.min_balance} required")
                return False
            
            self._balance -= amount
            self._add_transaction("withdraw", amount)
            print(f"Withdrew ${amount:.2f}. New balance: ${self._balance:.2f}")
            return True
        except ValueError as e:
            print(f"Withdrawal failed: {e}")
            return False
    
    def calculate_interest(self):
        """计算利息"""
        interest = self._balance * self.interest_rate
        return interest
    
    def apply_interest(self):
        """应用利息"""
        interest = self.calculate_interest()
        self._balance += interest
        self._add_transaction("interest", interest, 
                            f"Interest at {self.interest_rate*100}%")
        print(f"Interest applied: ${interest:.2f}. New balance: ${self._balance:.2f}")
        return interest


class CheckingAccount(Account):
    """支票账户"""
    
    def __init__(self, owner, initial_balance=0, overdraft_limit=500):
        super().__init__(owner, initial_balance)
        self.overdraft_limit = overdraft_limit  # 透支限额
        self.transaction_fee = 0.5  # 每笔交易费用
        self.free_transactions = 5  # 免费交易次数
        self.transaction_count = 0  # 当前月交易次数
    
    def withdraw(self, amount):
        """重写取款方法：允许透支"""
        if not self._is_active:
            print("Account is closed!")
            return False
        
        try:
            self._validate_amount(amount)
            
            # 检查透支限额
            if amount > self._balance + self.overdraft_limit:
                print(f"Exceeds overdraft limit of ${self.overdraft_limit}")
                return False
            
            self._balance -= amount
            self._add_transaction("withdraw", amount)
            self.transaction_count += 1
            
            # 收取交易费用
            if self.transaction_count > self.free_transactions:
                self._balance -= self.transaction_fee
                self._add_transaction("fee", self.transaction_fee, 
                                    "Transaction fee")
                print(f"Withdrew ${amount:.2f} (Fee: ${self.transaction_fee}). "
                      f"New balance: ${self._balance:.2f}")
            else:
                print(f"Withdrew ${amount:.2f}. New balance: ${self._balance:.2f}")
            
            # 透支警告
            if self._balance < 0:
                print(f"⚠️  WARNING: Account is overdrawn by ${abs(self._balance):.2f}")
            
            return True
        except ValueError as e:
            print(f"Withdrawal failed: {e}")
            return False
    
    def reset_monthly_transactions(self):
        """重置月度交易计数"""
        self.transaction_count = 0
        print("Monthly transaction count reset")


class Bank:
    """银行类：管理所有账户"""
    
    def __init__(self, name):
        self.name = name
        self.accounts = {}  # 账户号 -> 账户对象
    
    def create_savings_account(self, owner, initial_balance=0, interest_rate=0.02):
        """创建储蓄账户"""
        account = SavingsAccount(owner, initial_balance, interest_rate)
        self.accounts[account.account_number] = account
        print(f"✓ Savings account created: {account.account_number}")
        return account
    
    def create_checking_account(self, owner, initial_balance=0, overdraft_limit=500):
        """创建支票账户"""
        account = CheckingAccount(owner, initial_balance, overdraft_limit)
        self.accounts[account.account_number] = account
        print(f"✓ Checking account created: {account.account_number}")
        return account
    
    def get_account(self, account_number):
        """获取账户"""
        return self.accounts.get(account_number)
    
    def list_accounts(self):
        """列出所有账户"""
        print(f"\n{'='*60}")
        print(f"{self.name} - All Accounts")
        print(f"{'='*60}")
        if not self.accounts:
            print("No accounts found")
        else:
            for account in self.accounts.values():
                print(f"  {account}")
        print(f"{'='*60}\n")
    
    def total_deposits(self):
        """计算总存款"""
        return sum(acc.get_balance() for acc in self.accounts.values())
    
    def save_to_file(self, filename):
        """保存到文件"""
        data = {
            'bank_name': self.name,
            'accounts': []
        }
        
        for account in self.accounts.values():
            account_data = {
                'account_number': account.account_number,
                'owner': account.owner,
                'balance': account.get_balance(),
                'type': account.__class__.__name__,
                'transactions': [t.to_dict() for t in account.get_transaction_history()]
            }
            data['accounts'].append(account_data)
        
        with open(filename, 'w') as f:
            json.dump(data, f, indent=2)
        print(f"✓ Data saved to {filename}")


# ============================================================
# 测试代码
# ============================================================

def demo_bank_system():
    """演示银行系统"""
    
    print("\n" + "="*60)
    print("BANK ACCOUNT MANAGEMENT SYSTEM DEMO")
    print("="*60 + "\n")
    
    # 创建银行
    bank = Bank("Python Bank")
    
    # 创建账户
    print("\n--- Creating Accounts ---")
    alice_savings = bank.create_savings_account("Alice", 1000, 0.03)
    bob_checking = bank.create_checking_account("Bob", 500, 300)
    charlie_savings = bank.create_savings_account("Charlie", 2000, 0.025)
    
    # 存款
    print("\n--- Deposits ---")
    alice_savings.deposit(500)
    bob_checking.deposit(200)
    
    # 取款
    print("\n--- Withdrawals ---")
    alice_savings.withdraw(300)
    bob_checking.withdraw(100)
    
    # 测试储蓄账户最低余额
    print("\n--- Testing Minimum Balance (Savings) ---")
    alice_savings.withdraw(1500)  # 应该失败
    
    # 测试支票账户透支
    print("\n--- Testing Overdraft (Checking) ---")
    bob_checking.withdraw(700)  # 会透支
    
    # 转账
    print("\n--- Transfer ---")
    alice_savings.transfer(bob_checking, 200)
    
    # 应用利息
    print("\n--- Applying Interest ---")
    alice_savings.apply_interest()
    charlie_savings.apply_interest()
    
    # 打印账户对账单
    print("\n--- Account Statements ---")
    alice_savings.print_statement()
    bob_checking.print_statement()
    
    # 列出所有账户
    bank.list_accounts()
    
    # 显示总存款
    print(f"Total deposits in {bank.name}: ${bank.total_deposits():.2f}")
    
    # 保存数据
    print("\n--- Saving Data ---")
    bank.save_to_file("bank_data.json")
    
    print("\n" + "="*60)
    print("DEMO COMPLETED")
    print("="*60 + "\n")


if __name__ == "__main__":
    demo_bank_system()
```

### 4.8.3 Java完整实现

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.io.*;

// 交易记录类
class Transaction {
    private String type;
    private double amount;
    private double balance;
    private String description;
    private LocalDateTime timestamp;
    
    public Transaction(String type, double amount, double balance, String description) {
        this.type = type;
        this.amount = amount;
        this.balance = balance;
        this.description = description;
        this.timestamp = LocalDateTime.now();
    }
    
    @Override
    public String toString() {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        return String.format("[%s] %s: $%.2f (Balance: $%.2f) %s",
            timestamp.format(formatter),
            type.toUpperCase(),
            amount,
            balance,
            description);
    }
    
    public String getType() { return type; }
    public double getAmount() { return amount; }
    public double getBalance() { return balance; }
    public String getDescription() { return description; }
}

// 账户基类
abstract class Account {
    private static int accountCounter = 1000;
    
    protected String accountNumber;
    protected String owner;
    protected double balance;
    protected List<Transaction> transactions;
    protected boolean isActive;
    
    public Account(String owner, double initialBalance) {
        this.accountNumber = generateAccountNumber();
        this.owner = owner;
        this.balance = Math.max(0, initialBalance);
        this.transactions = new ArrayList<>();
        this.isActive = true;
        
        if (initialBalance > 0) {
            addTransaction("deposit", initialBalance, "Initial deposit");
        }
    }
    
    // 类方法：生成账户号
    private static synchronized String generateAccountNumber() {
        accountCounter++;
        return String.format("ACC%06d", accountCounter);
    }
    
    // 静态方法：验证金额
    protected static boolean validateAmount(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        return true;
    }
    
    // 私有方法：添加交易记录
    protected void addTransaction(String type, double amount, String description) {
        Transaction transaction = new Transaction(type, amount, balance, description);
        transactions.add(transaction);
    }
    
    // 存款
    public boolean deposit(double amount) {
        if (!isActive) {
            System.out.println("Account is closed!");
            return false;
        }
        
        try {
            validateAmount(amount);
            balance += amount;
            addTransaction("deposit", amount, "");
            System.out.printf("Deposited $%.2f. New balance: $%.2f%n", amount, balance);
            return true;
        } catch (IllegalArgumentException e) {
            System.out.println("Deposit failed: " + e.getMessage());
            return false;
        }
    }
    
    // 取款（子类可能重写）
    public boolean withdraw(double amount) {
        if (!isActive) {
            System.out.println("Account is closed!");
            return false;
        }
        
        try {
            validateAmount(amount);
            if (amount > balance) {
                System.out.println("Insufficient funds!");
                return false;
            }
            
            balance -= amount;
            addTransaction("withdraw", amount, "");
            System.out.printf("Withdrew $%.2f. New balance: $%.2f%n", amount, balance);
            return true;
        } catch (IllegalArgumentException e) {
            System.out.println("Withdrawal failed: " + e.getMessage());
            return false;
        }
    }
    
    // 转账
    public boolean transfer(Account targetAccount, double amount) {
        if (!isActive) {
            System.out.println("Account is closed!");
            return false;
        }
        
        if (this.withdraw(amount)) {
            targetAccount.deposit(amount);
            addTransaction("transfer", amount, "To " + targetAccount.accountNumber);
            targetAccount.addTransaction("transfer", amount, "From " + this.accountNumber);
            System.out.printf("Transferred $%.2f to %s%n", amount, targetAccount.accountNumber);
            return true;
        }
        return false;
    }
    
    // 获取余额
    public double getBalance() {
        return balance;
    }
    
    // 获取交易历史
    public List<Transaction> getTransactionHistory(int limit) {
        int size = transactions.size();
        int fromIndex = Math.max(0, size - limit);
        return transactions.subList(fromIndex, size);
    }
    
    // 打印对账单
    public void printStatement() {
        System.out.println("\n" + "=".repeat(60));
        System.out.println("ACCOUNT STATEMENT - " + accountNumber);
        System.out.println("=".repeat(60));
        System.out.println("Owner: " + owner);
        System.out.println("Account Type: " + this.getClass().getSimpleName());
        System.out.printf("Current Balance: $%.2f%n", balance);
        System.out.println("Status: " + (isActive ? "Active" : "Closed"));
        System.out.println("-".repeat(60));
        System.out.println("Recent Transactions:");
        
        if (transactions.isEmpty()) {
            System.out.println("  No transactions yet");
        } else {
            List<Transaction> recent = getTransactionHistory(10);
            for (Transaction trans : recent) {
                System.out.println("  " + trans);
            }
        }
        System.out.println("=".repeat(60) + "\n");
    }
    
    // 关闭账户
    public void closeAccount() {
        isActive = false;
        System.out.println("Account " + accountNumber + " has been closed");
    }
    
    @Override
    public String toString() {
        return String.format("%s(%s): %s, Balance: $%.2f",
            this.getClass().getSimpleName(),
            accountNumber,
            owner,
            balance);
    }
    
    // Getter
    public String getAccountNumber() { return accountNumber; }
    public String getOwner() { return owner; }
}

// 储蓄账户
class SavingsAccount extends Account {
    private double interestRate;
    private double minBalance;
    
    public SavingsAccount(String owner, double initialBalance, double interestRate) {
        super(owner, initialBalance);
        this.interestRate = interestRate;
        this.minBalance = 100;
    }
    
    @Override
    public boolean withdraw(double amount) {
        if (!isActive) {
            System.out.println("Account is closed!");
            return false;
        }
        
        try {
            validateAmount(amount);
            if (balance - amount < minBalance) {
                System.out.printf("Cannot withdraw: minimum balance of $%.2f required%n", minBalance);
                return false;
            }
            
            balance -= amount;
            addTransaction("withdraw", amount, "");
            System.out.printf("Withdrew $%.2f. New balance: $%.2f%n", amount, balance);
            return true;
        } catch (IllegalArgumentException e) {
            System.out.println("Withdrawal failed: " + e.getMessage());
            return false;
        }
    }
    
    public double calculateInterest() {
        return balance * interestRate;
    }
    
    public double applyInterest() {
        double interest = calculateInterest();
        balance += interest;
        addTransaction("interest", interest, 
            String.format("Interest at %.1f%%", interestRate * 100));
        System.out.printf("Interest applied: $%.2f. New balance: $%.2f%n", interest, balance);
        return interest;
    }
}

// 支票账户
class CheckingAccount extends Account {
    private double overdraftLimit;
    private double transactionFee;
    private int freeTransactions;
    private int transactionCount;
    
    public CheckingAccount(String owner, double initialBalance, double overdraftLimit) {
        super(owner, initialBalance);
        this.overdraftLimit = overdraftLimit;
        this.transactionFee = 0.5;
        this.freeTransactions = 5;
        this.transactionCount = 0;
    }
    
    @Override
    public boolean withdraw(double amount) {
        if (!isActive) {
            System.out.println("Account is closed!");
            return false;
        }
        
        try {
            validateAmount(amount);
            
            // 检查透支限额
            if (amount > balance + overdraftLimit) {
                System.out.printf("Exceeds overdraft limit of $%.2f%n", overdraftLimit);
                return false;
            }
            
            balance -= amount;
            addTransaction("withdraw", amount, "");
            transactionCount++;
            
            // 收取交易费用
            if (transactionCount > freeTransactions) {
                balance -= transactionFee;
                addTransaction("fee", transactionFee, "Transaction fee");
                System.out.printf("Withdrew $%.2f (Fee: $%.2f). New balance: $%.2f%n",
                    amount, transactionFee, balance);
            } else {
                System.out.printf("Withdrew $%.2f. New balance: $%.2f%n", amount, balance);
            }
            
            // 透支警告
            if (balance < 0) {
                System.out.printf("⚠️  WARNING: Account is overdrawn by $%.2f%n", Math.abs(balance));
            }
            
            return true;
        } catch (IllegalArgumentException e) {
            System.out.println("Withdrawal failed: " + e.getMessage());
            return false;
        }
    }
    
    public void resetMonthlyTransactions() {
        transactionCount = 0;
        System.out.println("Monthly transaction count reset");
    }
}

// 银行类
class Bank {
    private String name;
    private Map<String, Account> accounts;
    
    public Bank(String name) {
        this.name = name;
        this.accounts = new HashMap<>();
    }
    
    public SavingsAccount createSavingsAccount(String owner, double initialBalance, double interestRate) {
        SavingsAccount account = new SavingsAccount(owner, initialBalance, interestRate);
        accounts.put(account.getAccountNumber(), account);
        System.out.println("✓ Savings account created: " + account.getAccountNumber());
        return account;
    }
    
    public CheckingAccount createCheckingAccount(String owner, double initialBalance, double overdraftLimit) {
        CheckingAccount account = new CheckingAccount(owner, initialBalance, overdraftLimit);
        accounts.put(account.getAccountNumber(), account);
        System.out.println("✓ Checking account created: " + account.getAccountNumber());
        return account;
    }
    
    public Account getAccount(String accountNumber) {
        return accounts.get(accountNumber);
    }
    
    public void listAccounts() {
        System.out.println("\n" + "=".repeat(60));
        System.out.println(name + " - All Accounts");
        System.out.println("=".repeat(60));
        if (accounts.isEmpty()) {
            System.out.println("No accounts found");
        } else {
            for (Account account : accounts.values()) {
                System.out.println("  " + account);
            }
        }
        System.out.println("=".repeat(60) + "\n");
    }
    
    public double totalDeposits() {
        return accounts.values().stream()
            .mapToDouble(Account::getBalance)
            .sum();
    }
}

// 主程序
public class BankSystem {
    public static void main(String[] args) {
        System.out.println("\n" + "=".repeat(60));
        System.out.println("BANK ACCOUNT MANAGEMENT SYSTEM DEMO");
        System.out.println("=".repeat(60) + "\n");
        
        // 创建银行
        Bank bank = new Bank("Java Bank");
        
        // 创建账户
        System.out.println("\n--- Creating Accounts ---");
        SavingsAccount aliceSavings = bank.createSavingsAccount("Alice", 1000, 0.03);
        CheckingAccount bobChecking = bank.createCheckingAccount("Bob", 500, 300);
        SavingsAccount charlieSavings = bank.createSavingsAccount("Charlie", 2000, 0.025);
        
        // 存款
        System.out.println("\n--- Deposits ---");
        aliceSavings.deposit(500);
        bobChecking.deposit(200);
        
        // 取款
        System.out.println("\n--- Withdrawals ---");
        aliceSavings.withdraw(300);
        bobChecking.withdraw(100);
        
        // 测试储蓄账户最低余额
        System.out.println("\n--- Testing Minimum Balance (Savings) ---");
        aliceSavings.withdraw(1500);  // 应该失败
        
        // 测试支票账户透支
        System.out.println("\n--- Testing Overdraft (Checking) ---");
        bobChecking.withdraw(700);  // 会透支
        
        // 转账
        System.out.println("\n--- Transfer ---");
        aliceSavings.transfer(bobChecking, 200);
        
        // 应用利息
        System.out.println("\n--- Applying Interest ---");
        aliceSavings.applyInterest();
        charlieSavings.applyInterest();
        
        // 打印账户对账单
        System.out.println("\n--- Account Statements ---");
        aliceSavings.printStatement();
        bobChecking.printStatement();
        
        // 列出所有账户
        bank.listAccounts();
        
        // 显示总存款
        System.out.printf("Total deposits in %s: $%.2f%n", "Java Bank", bank.totalDeposits());
        
        System.out.println("\n" + "=".repeat(60));
        System.out.println("DEMO COMPLETED");
        System.out.println("=".repeat(60) + "\n");
    }
}
```

------

## 📝 课后作业

### 作业1：扩展动物类（基础）

创建一个动物园管理系统：

1. 创建`Animal`基类，包含`name`, `age`, `species`属性
2. 创建至少3个子类（如`Lion`, `Elephant`, `Penguin`）
3. 每个子类添加特有的属性和方法
4. 实现`make_sound()`方法（多态）
5. 创建`Zoo`类管理所有动物

**要求：**

- 使用继承和多态
- 实现`__str__`方法（Python）或`toString`方法（Java）
- 添加喂食功能

------

### 作业2：学生成绩管理系统（中级）

创建一个学生成绩管理系统：

**类设计：**

1. `Person`基类：`name`, `age`, `id`
2. `Student`子类：继承`Person`，添加`grades`（成绩字典）
3. `Teacher`子类：继承`Person`，添加`subject`（科目）
4. `Course`类：课程信息
5. `School`类：管理学生、教师和课程

**功能要求：**

- 添加/删除学生
- 录入成绩
- 计算平均分、最高分、最低分
- 生成成绩单
- 统计及格率

------

### 作业3：增强银行系统（高级）

在课程项目基础上添加以下功能：

1. **信用卡账户**（`CreditCardAccount`）
   - 信用额度
   - 最低还款额
   - 利息计算
2. **账户安全**
   - 密码验证
   - 交易限额
   - 异常交易检测
3. **银行统计**
   - 账户类型分布
   - 交易量统计
   - 利息收入计算
4. **数据持久化**
   - 保存到文件
   - 从文件加载
   - 导出报表

------

## 🎯 知识点总结

### 核心概念

```scss
面向对象编程三大特性：
┌─────────────────────────────────────┐
│ 1. 封装 (Encapsulation)            │
│    - 隐藏内部实现                   │
│    - 提供公开接口                   │
│    - 数据保护                       │
├─────────────────────────────────────┤
│ 2. 继承 (Inheritance)               │
│    - 代码复用                       │
│    - 建立类层次结构                 │
│    - is-a 关系                      │
├─────────────────────────────────────┤
│ 3. 多态 (Polymorphism)              │
│    - 同一接口，不同实现             │
│    - 方法重写                       │
│    - 方法重载                       │
└─────────────────────────────────────┘
```

### Python vs Java 对比

| 特性         | Python                    | Java                         |
| ------------ | ------------------------- | ---------------------------- |
| **类定义**   | `class ClassName:`        | `public class ClassName {}`  |
| **构造函数** | `__init__(self)`          | `ClassName()`                |
| **私有属性** | `__attribute` (名称改编)  | `private type attribute`     |
| **继承**     | `class Child(Parent):`    | `class Child extends Parent` |
| **多继承**   | 支持                      | 不支持（使用接口）           |
| **方法重写** | 直接重写                  | 使用`@Override`              |
| **类方法**   | `@classmethod`            | `static`                     |
| **静态方法** | `@staticmethod`           | `static`                     |
| **抽象类**   | `ABC` + `@abstractmethod` | `abstract class`             |
| **接口**     | 无（用抽象类）            | `interface`                  |

------

## 💡 最佳实践

### 1. 类设计原则

```python
# ✓ 好的设计
class BankAccount:
    def __init__(self, owner, balance):
        self._owner = owner  # 受保护
        self.__balance = balance  # 私有
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    
    def get_balance(self):
        return self.__balance

# ❌ 不好的设计
class BadAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance  # 公开，可以随意修改
```

### 2. 继承 vs 组合

```python
# 继承：is-a 关系
class Dog(Animal):  # Dog IS AN Animal
    pass

# 组合：has-a 关系
class Car:
    def __init__(self):
        self.engine = Engine()  # Car HAS AN Engine
        self.wheels = [Wheel() for _ in range(4)]
```

### 3. 方法命名规范

```python
# Python
class MyClass:
    def public_method(self):      # 公开方法
        pass
    
    def _protected_method(self):  # 受保护方法（约定）
        pass
    
    def __private_method(self):   # 私有方法
        pass
java复制// Java
public class MyClass {
    public void publicMethod() {}      // 公开方法
    
    protected void protectedMethod() {} // 受保护方法
    
    private void privateMethod() {}     // 私有方法
}
```

------

## 🔍 常见错误

### 错误1：忘记调用父类构造函数

```python
# ❌ 错误
class Child(Parent):
    def __init__(self, name, age):
        self.name = name
        # 忘记调用super().__init__()

# ✓ 正确
class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)
        self.age = age
```

### 错误2：直接修改私有属性

```python
# ❌ 错误
account.__balance = 999999  # 不应该这样做

# ✓ 正确
account.deposit(999999)  # 通过公开方法
```

### 错误3：循环继承

```python
# ❌ 错误
class A(B):
    pass

class B(A):  # 循环继承！
    pass

# ✓ 正确：建立清晰的继承层次
class Animal:
    pass

class Mammal(Animal):
    pass

class Dog(Mammal):
    pass
```

### 错误4：过度使用继承

```python
# ❌ 不好的设计
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Square(Rectangle):  # 正方形继承矩形？
    def __init__(self, side):
        super().__init__(side, side)
    
    # 问题：正方形的width和height应该始终相等
    # 但继承Rectangle后可以分别设置

# ✓ 更好的设计：使用组合或独立类
class Square:
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2
```

------

## 🚀 进阶话题

### 1. 抽象类和接口

#### Python抽象类

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    """抽象形状类"""
    
    @abstractmethod
    def area(self):
        """计算面积（抽象方法）"""
        pass
    
    @abstractmethod
    def perimeter(self):
        """计算周长（抽象方法）"""
        pass
    
    def description(self):
        """具体方法"""
        return f"This is a {self.__class__.__name__}"


class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius


# 不能实例化抽象类
# shape = Shape()  # TypeError

# 可以实例化具体类
circle = Circle(5)
print(circle.area())
print(circle.description())
```

#### Java接口和抽象类

```java
// 接口
interface Drawable {
    void draw();  // 抽象方法
    
    // Java 8+: 默认方法
    default void display() {
        System.out.println("Displaying...");
    }
}

// 抽象类
abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    // 抽象方法
    public abstract double area();
    public abstract double perimeter();
    
    // 具体方法
    public String getColor() {
        return color;
    }
}

// 实现接口和继承抽象类
class Circle extends Shape implements Drawable {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double perimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " circle");
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Circle circle = new Circle("Red", 5);
        System.out.println(circle.area());
        circle.draw();
        circle.display();  // 调用接口的默认方法
    }
}
```

------

### 2. 多重继承（Python）

```python
# Python支持多重继承

class Flyable:
    """可飞行的"""
    def fly(self):
        print(f"{self.name} is flying")

class Swimmable:
    """可游泳的"""
    def swim(self):
        print(f"{self.name} is swimming")

class Duck(Flyable, Swimmable):
    """鸭子：既能飞又能游"""
    def __init__(self, name):
        self.name = name
    
    def quack(self):
        print(f"{self.name} says: Quack!")


# 使用
duck = Duck("Donald")
duck.fly()    # Donald is flying
duck.swim()   # Donald is swimming
duck.quack()  # Donald says: Quack!

# 方法解析顺序（MRO）
print(Duck.__mro__)
# (<class '__main__.Duck'>, <class '__main__.Flyable'>, 
#  <class '__main__.Swimmable'>, <class 'object'>)


# 钻石继承问题
class A:
    def method(self):
        print("A.method")

class B(A):
    def method(self):
        print("B.method")

class C(A):
    def method(self):
        print("C.method")

class D(B, C):
    pass

# D继承自B和C，B和C都继承自A
# 调用D的method会调用哪个？
d = D()
d.method()  # B.method（按照MRO顺序）
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, 
#  <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

------

### 3. 属性装饰器（Python）

```python
class Temperature:
    """温度类：演示属性装饰器"""
    
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """获取摄氏温度"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """设置摄氏温度"""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """获取华氏温度"""
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """设置华氏温度"""
        self.celsius = (value - 32) * 5/9
    
    @property
    def kelvin(self):
        """获取开尔文温度"""
        return self._celsius + 273.15


# 使用属性装饰器
temp = Temperature(25)

# 像访问属性一样使用
print(temp.celsius)     # 25
print(temp.fahrenheit)  # 77.0
print(temp.kelvin)      # 298.15

# 设置温度
temp.celsius = 30
print(temp.fahrenheit)  # 86.0

temp.fahrenheit = 100
print(temp.celsius)     # 37.77777777777778

# 验证
# temp.celsius = -300  # ValueError: Temperature below absolute zero!
```

------

### 4. 运算符重载

```python
class Vector:
    """二维向量类：演示运算符重载"""
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        """字符串表示"""
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        """开发者表示"""
        return f"Vector({self.x!r}, {self.y!r})"
    
    def __add__(self, other):
        """向量加法：v1 + v2"""
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        """向量减法：v1 - v2"""
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        """标量乘法：v * 3"""
        return Vector(self.x * scalar, self.y * scalar)
    
    def __rmul__(self, scalar):
        """反向标量乘法：3 * v"""
        return self.__mul__(scalar)
    
    def __eq__(self, other):
        """相等比较：v1 == v2"""
        return self.x == other.x and self.y == other.y
    
    def __abs__(self):
        """向量长度：abs(v)"""
        import math
        return math.sqrt(self.x**2 + self.y**2)
    
    def __len__(self):
        """维度：len(v)"""
        return 2
    
    def __getitem__(self, index):
        """索引访问：v[0], v[1]"""
        if index == 0:
            return self.x
        elif index == 1:
            return self.y
        else:
            raise IndexError("Vector index out of range")


# 使用运算符重载
v1 = Vector(2, 3)
v2 = Vector(4, 5)

print(v1)           # Vector(2, 3)
print(v1 + v2)      # Vector(6, 8)
print(v1 - v2)      # Vector(-2, -2)
print(v1 * 3)       # Vector(6, 9)
print(3 * v1)       # Vector(6, 9)
print(abs(v1))      # 3.605551275463989
print(len(v1))      # 2
print(v1[0], v1[1]) # 2 3
print(v1 == v2)     # False
java复制// Java不支持运算符重载（除了String的+）
// 但可以通过方法实现类似功能

public class Vector {
    private double x;
    private double y;
    
    public Vector(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // 加法
    public Vector add(Vector other) {
        return new Vector(this.x + other.x, this.y + other.y);
    }
    
    // 减法
    public Vector subtract(Vector other) {
        return new Vector(this.x - other.x, this.y - other.y);
    }
    
    // 标量乘法
    public Vector multiply(double scalar) {
        return new Vector(this.x * scalar, this.y * scalar);
    }
    
    // 长度
    public double magnitude() {
        return Math.sqrt(x * x + y * y);
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Vector vector = (Vector) obj;
        return Double.compare(vector.x, x) == 0 &&
               Double.compare(vector.y, y) == 0;
    }
    
    @Override
    public String toString() {
        return String.format("Vector(%.2f, %.2f)", x, y);
    }
    
    public static void main(String[] args) {
        Vector v1 = new Vector(2, 3);
        Vector v2 = new Vector(4, 5);
        
        System.out.println(v1);                    // Vector(2.00, 3.00)
        System.out.println(v1.add(v2));            // Vector(6.00, 8.00)
        System.out.println(v1.subtract(v2));       // Vector(-2.00, -2.00)
        System.out.println(v1.multiply(3));        // Vector(6.00, 9.00)
        System.out.println(v1.magnitude());        // 3.605551275463989
        System.out.println(v1.equals(v2));         // false
    }
}
```

------

### 5. 设计模式示例

#### 单例模式（Singleton）

```python
# Python单例模式

class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
    
    def __init__(self):
        self.value = 0


# 使用
s1 = Singleton()
s2 = Singleton()

print(s1 is s2)  # True（同一个实例）

s1.value = 100
print(s2.value)  # 100


# 更Pythonic的方式：使用装饰器
def singleton(cls):
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class DatabaseConnection:
    def __init__(self):
        print("Creating database connection")
        self.connected = True

# 使用
db1 = DatabaseConnection()  # Creating database connection
db2 = DatabaseConnection()  # 不会再次创建
print(db1 is db2)  # True
------------------------------------------------------------------------------------------------------------
                                                 java
------------------------------------------------------------------------------------------------------------

//Java单例模式

public class Singleton {
    // 私有静态实例
    private static Singleton instance;
    
    // 私有构造函数
    private Singleton() {
        System.out.println("Creating singleton instance");
    }
    
    // 公开静态方法获取实例
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    
    // 示例方法
    private int value;
    
    public int getValue() {
        return value;
    }
    
    public void setValue(int value) {
        this.value = value;
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        
        System.out.println(s1 == s2);  // true
        
        s1.setValue(100);
        System.out.println(s2.getValue());  // 100
    }
}
```

#### 工厂模式（Factory）

```python
# Python工厂模式

class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class Duck(Animal):
    def speak(self):
        return "Quack!"

# 工厂类
class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        """工厂方法"""
        animals = {
            'dog': Dog,
            'cat': Cat,
            'duck': Duck
        }
        
        animal_class = animals.get(animal_type.lower())
        if animal_class:
            return animal_class()
        else:
            raise ValueError(f"Unknown animal type: {animal_type}")


# 使用工厂
factory = AnimalFactory()

dog = factory.create_animal('dog')
cat = factory.create_animal('cat')
duck = factory.create_animal('duck')

print(dog.speak())   # Woof!
print(cat.speak())   # Meow!
print(duck.speak())  # Quack!
java复制// Java工厂模式

interface Animal {
    String speak();
}

class Dog implements Animal {
    @Override
    public String speak() {
        return "Woof!";
    }
}

class Cat implements Animal {
    @Override
    public String speak() {
        return "Meow!";
    }
}

class Duck implements Animal {
    @Override
    public String speak() {
        return "Quack!";
    }
}

// 工厂类
class AnimalFactory {
    public static Animal createAnimal(String animalType) {
        switch (animalType.toLowerCase()) {
            case "dog":
                return new Dog();
            case "cat":
                return new Cat();
            case "duck":
                return new Duck();
            default:
                throw new IllegalArgumentException("Unknown animal type: " + animalType);
        }
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Animal dog = AnimalFactory.createAnimal("dog");
        Animal cat = AnimalFactory.createAnimal("cat");
        Animal duck = AnimalFactory.createAnimal("duck");
        
        System.out.println(dog.speak());   // Woof!
        System.out.println(cat.speak());   // Meow!
        System.out.println(duck.speak());  // Quack!
    }
}
```

------

## 📚 扩展阅读

### 推荐书籍

1. **《Head First 设计模式》** - 设计模式入门
2. **《重构：改善既有代码的设计》** - Martin Fowler
3. **《Effective Java》** - Joshua Bloch
4. **《流畅的Python》** - Luciano Ramalho

### 在线资源

- [Python官方文档 - 类](https://docs.python.org/3/tutorial/classes.html)
- [Java官方教程 - OOP概念](https://docs.oracle.com/javase/tutorial/java/concepts/)
- [Refactoring Guru - 设计模式](https://refactoring.guru/design-patterns)

------

## 🎓 课程总结

### 你学到了什么？

✅ **类与对象**

- 类是对象的模板
- 对象是类的实例
- 每个对象有自己的状态

✅ **封装**

- 隐藏内部实现
- 提供公开接口
- 保护数据安全

✅ **继承**

- 代码复用
- 建立类层次
- is-a关系

✅ **多态**

- 同一接口，不同实现
- 方法重写和重载
- 灵活的代码设计

✅ **构造函数与析构函数**

- 对象初始化
- 资源清理
- 生命周期管理

✅ **类属性与实例属性**

- 共享数据 vs 独立数据
- 类方法 vs 实例方法
- 静态方法的使用

✅ **实战项目**

- 银行账户管理系统
- 综合运用OOP概念
- 真实场景应用

------

## 🎯 下节预告

**第五课：高级数据结构**

- 列表、元组、集合、字典深入
- 栈、队列、链表
- 树和图基础
- 算法复杂度分析
- 实战：实现常用数据结构

------

## ❓ 常见问题（FAQ）

**Q1: 什么时候使用继承，什么时候使用组合？**

A:

- **继承**：当存在明确的"is-a"关系时（狗是动物）
- **组合**：当存在"has-a"关系时（汽车有引擎）
- 一般建议：优先使用组合，因为更灵活

**Q2: Python的私有属性真的私有吗？**

A: 不是真正私有。Python使用名称改编（name mangling）机制，将`__attribute`改为`_ClassName__attribute`。这只是一种约定，技术上仍可访问。

**Q3: Java为什么不支持多重继承？**

A: 为了避免"钻石继承"问题（菱形继承）。Java通过接口实现类似功能，一个类可以实现多个接口。

**Q4: 什么时候使用静态方法？**

A:

- 工具函数（不需要访问实例或类状态）
- 工厂方法
- 验证函数
- 常量定义

**Q5: 如何选择使用抽象类还是接口（Java）？**

A:

- **抽象类**：有共同的实现代码，is-a关系
- **接口**：定义行为契约，can-do关系
- Java 8+接口可以有默认方法，界限变模糊

------

## 💻 练习答案提示

### 作业1提示：动物园系统

```python
class Animal:
    def __init__(self, name, age, species):
        self.name = name
        self.age = age
        self.species = species
        self.hunger = 0  # 饥饿值
    
    def make_sound(self):
        raise NotImplementedError
    
    def feed(self):
        self.hunger = 0
        print(f"{self.name} has been fed")

class Lion(Animal):
    def __init__(self, name, age):
        super().__init__(name, age, "Lion")
        self.mane_color = "golden"
    
    def make_sound(self):
        return "ROAR!"
    
    def hunt(self):
        print(f"{self.name} is hunting")

class Zoo:
    def __init__(self, name):
        self.name = name
        self.animals = []
    
    def add_animal(self, animal):
        self.animals.append(animal)
    
    def feed_all(self):
        for animal in self.animals:
            animal.feed()
```

### 作业2提示：学生成绩系统

```python
class Person:
    def __init__(self, name, age, id_number):
        self.name = name
        self.age = age
        self.id = id_number

class Student(Person):
    def __init__(self, name, age, id_number):
        super().__init__(name, age, id_number)
        self.grades = {}  # {科目: 成绩}
    
    def add_grade(self, subject, grade):
        self.grades[subject] = grade
    
    def average_grade(self):
        if not self.grades:
            return 0
        return sum(self.grades.values()) / len(self.grades)
    
    def print_report_card(self):
        print(f"\n成绩单 - {self.name}")
        print("-" * 30)
        for subject, grade in self.grades.items():
            print(f"{subject}: {grade}")
        print(f"平均分: {self.average_grade():.2f}")
```

------

**恭喜你完成第四课！** 🎉

面向对象编程是现代软件开发的基石。通过本课学习，你已经掌握了OOP的核心概念和实践技巧。继续练习，多写代码，你会越来越熟练！

**记住：好的代码不是写出来的，是重构出来的！** 💪

------

*课程结束时间：预计180分钟*