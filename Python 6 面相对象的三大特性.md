# Python | 6 面相对象的三大特性


这里不方便展示代码运行结果，大家可以在 **[https://github.com/hzyao/Python-Cookbook](https://github.com/hzyao/Python-Cookbook)** 
进行查看，这样可以更直观更清晰！边学习边实操，及时挖掘真美妙！搭配食用或许口味更佳哟！

# 面相对象的三大特性

面相对象编程，是许多编程语言都支持的一种编程思想。简单来说就是，基于模板（类）去创建实体（对象），使用对象完成功能开发。

面向对象包含三大主要特性：封装、继承、多态。

## 1 封装

### 1.1 什么是封装

封装表示的是，将现实世界事物的**属性和行为**封装到类中，描述为**成员变量和成员方法**，从而完成程序对现实世界事物的描述。

### 1.2 私有成员

现实事物会有部分不公开的属性和行为，不对使用者开放。那么作为现实事物在程序中映射的类，当然也会支持啦！

类中提供了**私有成员**的形式来支持，即**私有成员变量和私有成员方法**。其定义只需在变量名或方法名前加`__`（两个下划线），即可完成私有成员的设置。如：`__urrent_voltage`。

注：私有成员方法无法直接被类对象使用，但可以被其它成员使用；私有变量也无法赋值，无法获取值。

```python
# 定义一个类，内含私有成员变量和私有成员方法
class Phone:
    __current_voltage = None

    def __keep_single_core(self):
        print("CPU single core run")

phone = Phone()

phone.__current_voltage = 22        # 不报错，但无效
# phone.__keep_single_core()        # 无法使用
# print(phone.__current_voltage)    # 无法使用
```

```python
# 公开成员方法可以使用私有成员方法
class Phone:
    __current_voltage = 0.8

    def __keep_single_core(self):
        print("CPU single core run")

    def call_by_5g(self):
        if self.__current_voltage >= 1:
            print("5g ok")
        else:
            self.__keep_single_core()
            print("5g no, single core")

phone = Phone()
phone.call_by_5g()
```

```python
# 练习：设计带有私有成员的手机
class PHONE:
    __is_5g_enable = False

    def __check_5g(self):
        if self.__is_5g_enable:
            print("5g yes")
        else:
            print("5g no, use 4g")

    def call_by_5g(self):
        self.__check_5g()
        print("calling")

phone = PHONE()

phone.call_by_5g()
```

## 2 继承

### 2.1 什么是继承

继承：就是一个类，继承（复制）另外一个类的成员变量和成员方法（不含私有）。

```python
class 子类名(父类名0, 父类名1, ......, 父类名N):
		类内容体
```

子类构建的类对象，可以：

- 有自己的成员变量和成员方法；
- 使用父类的成员变量和成员方法。

### 2.2 继承的使用方式

假如有一个类，如下：

```python
# 原有类
class Phone:
    IMEI = None                 # 序列号
    producer = "xmy"            # 厂商

    def call_by_4g(self):
        print("4g")
```

你想要在此基础上，增加一些新功能，我们是不是有两种方式：一种是从头开始重新设计，一种是在当前基础上进行修修补补。

从头开始设计的话，像下面这样：

```python
# 定义新类——从头设计
class PhoneNew:
    IMEI = None                 # 序列号
    producer = "xmy"            # 厂商
    faceid = 1324               # 面部识别

    def call_by_4g(self):
        print("4g")

    def call_by_5g(self):
        print("5g")
```

是不是比较麻烦，相信大家一定更愿意在原有基础上。修修补补又是一年！那么我们就可以这样写：

```python
# 定义新类——使用单继承
class PhoneNew(Phone):
    faceid = 1324               # 面部识别

    def call_by_5g(self):
        print("5g")
```

```python
# 测试
phone = PhoneNew()
print(phone.producer)
phone.call_by_4g()              # 原有类中的旧功能与新类中的新功能均可被调用
phone.call_by_5g()
```

这样就可以完成类的**单继承**。

继承分为**单继承**和**多继承**。

单继承：一个类继承另一个类。

语法：

```python
class 类名(父类名):
		类内容体
```

多继承：一个类继承多个类，按照顺序**从左向右依次继承**（后续介绍）。

它的写法也很简单，语法如下：

```python
class 类名(父类名0, 父类名1, ......, 父类名N):
		类内容体
```

```python
## 定义新类1
class NFCReader:
    nfc = "第六代"
    producer = "run"

    def read_card(self):
        print("NFC读卡")

    def write_card(self):
        print("NFC写卡")

## 定义新类2
class RemoteControl:
    rc = "红外遥控"

    def control(self):
        print("红外遥控已开启")

# 定义新类——使用多继承
class MyPhone(Phone, NFCReader, RemoteControl):
    pass                # pass关键字用于补全语法（后续会详细介绍）

phone = MyPhone()
phone.call_by_4g()
phone.read_card()
phone.control()
print(phone.producer)
```

注：多个父类中，如果已经有同名的方法或属性，那么会**默认**以继承顺序（从左到右）为优先级。即**先继承的保留，后继承的被覆盖**。比如上述例子中的`producer`，在`Phone`类中为`xmy`，在`NFCReader`类中为`run`，在使用多继承的新类`MyPhone`中，`producer`输出为`xmy`，即先继承的`Phone`类中的`producer`。

> **这里展示代码输出结果不太方便，大家可以去 [https://github.com/hzyao/Python-Cookbook](https://github.com/hzyao/Python-Cookbook) 进行查看！**
> 

### 2.3 pass 关键字的作用

`pass`是占位语句，它的作用就是帮助我们补全语法，用来保证函数（方法）或类定义的完整性，表示无内容、空的意思。

比如我们在写一个类的时候，这个类继承了很多个别的类（像上面的`MyPhone`类），它所包含的功能已经满足我们的需求了，我们不想再添加新的功能。但是由于语法需求，在定义类的时候，类的内部必须有内容，这个时候我们就可以用`pass`替代一下，以保证我们的语法不报错。pass本身其实就表示这里是空的，啥也莫得！

### 2.4 复写和调用父类成员

首先，什么是复写呢？

复写其实就是对父类的成员属性或成员方法进行重新定义。

子类继承父类的成员属性和成员方法后，如果对其“不满意”，可以进行复写，即在子类中重新定义同名的属性或方法即可。

```python
# 定义父类
class Phone:
    IMEI = None                 # 序列号
    producer = "xmy"            # 厂商

    def call_by_5g(self):
        print("5g")

# 定义子类，复写父类成员
class MyPhone(Phone):
    producer = "run"            # 复写父类的成员属性

    def call_by_5g(self):       # 复写父类的成员方法
        print("singleCPU")
        print("5g")
        print("closeCPU")

phone = MyPhone()
phone.call_by_5g()
print(phone.producer)
```

一旦复写父类成员，那么类对象调用成员的时候，就会调用复写后的新成员。

如果需要使用被复写的父类的成员，就需要特殊的调用方式：

1. 调用父类成员
    
    使用成员变量：`父类名.成员变量`
    
    使用成员方法：`父类名.成员方法(self)`
    
    ```python
    # 调用父类成员方法1
    class MyPhone(Phone):
        producer = "run"            # 复写父类的成员属性
    
        def call_by_5g(self):       # 复写父类的成员方法
            print("singleCPU")
            # 方法1
            print(f"父类的厂商是: {Phone.producer}")
            Phone.call_by_5g(self)
            print("closeCPU")
    
    phone = MyPhone()
    phone.call_by_5g()
    print(phone.producer)
    ```
    
2. 使用`super()`调用父类成员
    
    使用成员变量：`super().成员变量`
    
    使用成员方法：`super().成员方法()`
    
    ```python
    # 调用父类成员方法2
    class MyPhone(Phone):
        producer = "run"            # 复写父类的成员属性
    
        def call_by_5g(self):       # 复写父类的成员方法
            print("singleCPU")
            # 方法2
            print(f"父类的厂商是: {super().producer}")
            super().call_by_5g()    # 注意这里就不需要传self进去啦
            print("closeCPU")
    
    phone = MyPhone()
    phone.call_by_5g()
    print(phone.producer)
    ```
    

注：只可以在子类内部调用父类的同名成员，子类的实体类对象调用默认是调用子类复写的。

## 3 多态

### 3.1 什么是多态

多态其实就是指多种状态，即完成某个行为时，使用不同的对象获得不同的状态。

```python
# 演示面向对象的多态特性以及抽象类（接口）的使用
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        print("汪汪汪")

class Cat(Animal):
    def speak(self):
        print("喵喵喵")

def make_noise(animal: Animal):     # 接受对象为animal，并注解类型为Animal（类型注解如有需求会在后续进行详细介绍）
    """制造点噪音，需要传入Animal对象"""
    animal.speak()

# 使用两个子类对象来调用函数
dog = Dog()
cat = Cat()

make_noise(dog)
make_noise(cat)                     # 同样的行为，使用不同的对象获得不同的运行状态，也就是我们介绍的多态
```

上述我们可以看出，**同样的行为（函数），传入不同的对象，得到不同的状态。**

多态常会用在继承关系上，比如：

- 函数（方法）形参声明接收父类对象（类型注解如有需求会在后续进行详细介绍）
    
    ```markdown
    def make_noise(animal: Animal):   # 接受对象为animal，并注解类型为Animal
        animal.speak()
    ```
    
- 实际传入父类的子类对象进行工作
    
    ```markdown
    dog = Dog()
    cat = Cat()
    
    make_noise(dog)
    make_noise(cat) 
    ```
    

也就是说，定义函数（方法），通过类型注解声明需要父类对象，实际传入子类对象进行工作，从而获得不同的工作状态。

### 3.2 抽象类（接口）的编程思想

可能大家已经发现了，我们前面提到的父类`Animal`的`speak`方法，是空实现。

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        print("汪汪汪")

class Cat(Animal):
    def speak(self):
        print("喵喵喵")
```

那么，这种设计有什么含义呢？

其实就是希望用父类来确定有哪些方法，具体的方法实现由子类自行决定。这种写法就叫做**抽象类**（也可以称为**接口**）。

**抽象类**：含有抽象方法的类。

**抽象方法**：没有具体实现的方法，也就是方法体是空实现的（`pass`）。

**那么，我们为什么要使用抽象类呢？**

我们来举一个形象的例子帮助大家理解！比如生产空调，我们可以提出一个空调制作标准，要求空调可以制冷、可以制热、左右摇摆等等。现在有了生产空调的标准，那么各个厂家就可以按照这个标准去自行实现，他们可以有不一样的核心技术，但是最终达到这个制造标准的要求就可以啦！

抽象类呢，就可以看作是一个标准，它包含了一些抽象的方法，要求子类必须实现。

```python
# 演示抽象类
class AC:
    def cool_wind(self):
        """制冷"""
        pass

    def hot_wind(self):
        """制热"""
        pass

    def swing_lr(self):
        """左右摇摆"""
        pass

# Midea_AC 继承 AC
class Midea_AC(AC):
    def cool_wind(self):
        print("Midea cool")

    def hot_wind(self):
        print("Midea hot")

    def swing_lr(self):
        print("Midea swing")

# GREE_AC 继承 AC
class GREE_AC(AC):
    def cool_wind(self):
        print("GREE cool")

    def hot_wind(self):
        print("GREE hot")

    def swing_lr(self):
        print("GREE swing")

# 定义一个制冷函数
def make_cool(ac: AC):          # 通过类型注解告诉我们以AC作为传入对象
    ac.cool_wind()              # 调用制冷方法

# 构建具体子类进行具体实现，而不是AC，因为AC为抽象类，是用来做顶层设计的，里面方法为空
midea_ac = Midea_AC()
gree_ac = GREE_AC()

# 调用制冷函数传入子类对象
make_cool(midea_ac)
make_cool(gree_ac)
```

从上面我们也可以看出，抽象类一般会配合多态完成以下任务：

- 抽象的父类设计（设计标准）
- 具体的子类实现（实现标准）

最后我们总结一下！抽象类主要用于做顶层设计（也就是设计标准），以便子类做具体实现。这也是对子类的一种软性约束，要求子类必须复写（实现）父类的一些方法，并配合多态使用，从而获得不同的工作状态。