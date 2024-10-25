---
title: "Java基础面试题"
date: 2024-09-02
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage119.jpg?raw=true
tags: ["JavaSE"]
category: "面试"
updated: 2024-09-03
  
top_group_index: 
---

# JRE和JDK的区别？

1. JRE，Java Runtime Environment，Java运行环境：包含了JVM、核心类库和其他支持运行Java程序的文件。

2. JDK，Java Development Kit，Java开发工具包：包含了JRE，以及用于开发、调试、监控Java应用程序的工具。

# 使用过哪些JDK提供的工具？

主要工具：

- javac：Java编译器，负责将Java源代码编译成字节码(.class 文件)。

- java：运行Java应用程序的命令，使用JVM来解释并执行编译后的字节码文件。

- javadoc：生成API文档的工具，能够根据源代码中的注释生成HTML格式的文档。

- jar：用于创建和管理JAR文件的工具，可以将多个.class文件打包为单一文件便于分发和管理。

- jdb：Java调试工具，用于在命令行中调试Java应用程序，支持断点设置、变量查看等功能。

性能监控和分析工具：

- jps：Java进程工具，显示所有正在运行的Java进程，便于监控和诊断。

- jstack：生成线程堆栈信息的工具，常用于分析死锁和线程问题。

- jmap：内存映射工具，可以生成堆转储(heap dump)文件，便于内存泄漏分析和垃圾回收优化。

- jhat：堆分析工具，配合jmap使用，分析生成的堆转储文件，帮助开发者了解内存使用情况。

- jstat：JVM统计监控工具，实时监控垃圾回收、内存、类加载等信息，帮助开发者调优JVM性能。

- jconsole：图形化的JVM监控工具，可以监控应用程序的内存、线程和类加载情况，常用于监控和调试。

- jvisualvm：功能强大的性能分析工具，支持堆、线程、GC 的详细监控，还提供内存分析和CPU性能分析。

诊断工具：

- jinfo:用于查看和修改正在运行的JVM参数，便于动态调优和调整JVM行为。

- jstatd：远程JVM监控工具，可以通过网络远程监控JVM的状态，适合分布式系统中的性能监控。

# Java中的序列化和反序列化是什么？

1. 序列化是把对象转换成字节流的过程，这样对象就可以通过网络传输、持久化存储或者缓存。

Java中的类可以通过继承Serializable接口来实现序列化。

如果某些数据不想被序列化，可以在字段面前添加transient关键字。

2. 反序列化是把字节流转换成对象的过程，这样可以从存储中读取数据并生成对象。

# GET和POST的区别？

1. 参数传递的方式

GET请求参数是在URL中以键值对的形式传递的。

POST请求参数是在请求体中以键值对的形式传递的。

2. 参数传递大小

GET请求参数依靠URL传递，URL长度限制一般为2048个字符。

POST请求参数依靠请求体，无大小限制。

3. 安全性

GET请求参数是明文传输的，在URL中，容易被获取或暴露在浏览器历史记录里。

POST请求参数是通过请求体传输，相对安全一些，但是也要注意参数加密和防止CSRF攻击等问题。

4. 适用场景不同

GET请求适用于获取数据，如浏览网页、搜索等。

POST请求适用于提交数据，如登录、注册、发布内容等。

# 封装、继承、多态？

OOP，面向对象编程的三大核心概念：封装、继承、多态。

1. 封装

封装就是把数据（对象的状态）和方法（对象的行为）封装在一个类内部，并通过公开的接口与外部进行交互。

封装的主要目的是隐藏对象内部实现细节，只暴露必要的功能，从而保护数据的完整性，减少系统的复杂性。

```java
public class Pet {
    private String name;  // 宠物名字
    private int age;      // 宠物年龄

    public Pet(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 获取宠物名字
    public String getName() {
        return name;
    }

    // 设置宠物名字
    public void setName(String name) {
        this.name = name;
    }

    // 获取宠物年龄
    public int getAge() {
        return age;
    }

    // 设置宠物年龄
    public void setAge(int age) {
        if (age > 0) {  // 确保年龄是正数
            this.age = age;
        }
    }

    // 宠物的行为：发出声音
    public void makeSound() {
        System.out.println("Some generic pet sound");
    }
}
```

2. 继承

继承就是允许子类继承父类的属性和方法。

继承机制使得类之间可以形成层次结构，支持代码重用和扩展。

```java
// 狗类，继承自Pet
public class Dog extends Pet {
    public Dog(String name, int age) {
        super(name, age);  // 调用父类构造器
    }

    @Override
    public void makeSound() {
        System.out.println("汪汪！");
    }
}

// 猫类，继承自Pet
public class Cat extends Pet {
    public Cat(String name, int age) {
        super(name, age);  // 调用父类构造器
    }

    @Override
    public void makeSound() {
        System.out.println("喵喵~");
    }
}
```

3. 多态

多态就是同一个接口或父类引用变量可以指向不同的对象实例，并根据实际指向的对象类型执行相应的方法。

多态允许同一方法在不同对象上表现出不同的行为。

```java
public class PetManager {
    public static void main(String[] args) {
        Pet myDog = new Dog("旺财", 3);
        Pet myCat = new Cat("小花", 2);

        // 使用多态调用makeSound方法
        makePetSound(myDog);  // 输出: 汪汪！
        makePetSound(myCat);  // 输出: 喵喵~
    }

    public static void makePetSound(Pet pet) {
        pet.makeSound();
    }
}
```

# Java为什么不支持多重继承？

多重继承会导致菱形继承的问题，多重继承可能导致方法的冲突和不确定性，增加了继承结构的复杂度和维护难度。

Java允许实现多个接口，提供了类似于多重继承的功能，又避免了菱形继承的问题。

# 基本数据类型和包装类型有什么区别？

基本数据类型：byte、char、short、int、long、double、float、boolean。

包装类型：Byte、Character、Short、Integer、Long、Double、Float、Boolean。

区别：

1. 性能

基本类型：效率高，占用内存小，适合频繁使用的简单操作。

包装类型：因为是对象，涉及内存分配和垃圾回收，性能较低。

2. 比较方式

基本类型：使用`==`，直接比较数值。

包装类型：使用`==`比较的是对象的内存地址，使用`equals`比较的是对象的内容。

3. 默认值

基本类型：默认值为0，false。

包装类型：默认值为null。

4. 初始化

基本类型：直接赋值。

包装类型：需要采用new的方式创建。

5. 存储方式

基本类型：如果是局部变量，保存在栈上；如果是成员变量，保存在堆上。

包装类型：保存在堆上。

# 使用基本数据类型的包装类有什么好处？

包装类让基本数据类型具有了对象相关的属性和方法，可以增加基本数据类型的操作。

此外，例如集合声明泛型时，也必须要求Object类型，即包装类。

# 自动装箱和自动拆箱是什么？

1. 自动装箱(Autoboxing)：指的是Java 编译器自动将基本数据类型转换为它们对应的包装类型。比如，将int转换为Integer。

2. 自动拆箱(Unboxing)：指的是Java 编译器自动将包装类型转换为基本数据类型。比如，将Integer 转换为 int。

# 重载和重写的区别?

重载：同一个类中，允许有多个同名方法，只要它们的参数列表（参数个数、类型或顺序）不同。

重载通常用于提供同一操作的不同实现，例如构造函数的重载、不同类型输入的处理等。

重写：子类在继承父类的时候，可以重写父类的某个方法（参数列表、方法名必须相同），从而为该方法提供新的实现。

重写通常用于子类提供父类方法的具体实现，以实现多态性。例如子类对父类方法进行拓展或修改以适应特定需求。

# ==、equals和hashcode的区别？

`==`：比较两个对象的内存地址，如果是基本数据类型则比较数值。

`equals`：比较两个对象的内容。

`hashcode`：计算对象的hash值。如果两个对象计算出的hash值不同，则这两个对象一定不同。

# &和&&的区别?

&，逻辑与：前后条件都会进行判断，无论前面的条件是否成立，都会判断后面的条件。容易造成空指针异常。

&&，短路与：如果前面的条件不成立，则不会判断后面的条件。可以避免空指针异常。

# String、StringBuffer和StringBuilder区别?

String是不可变的。

StringBuffer是可变的，线程安全的。

StringBuilder是可变的，线程不安全的。

细节：由于StringBuffer和StringBuilder底层是数组实现，会涉及到扩容机制，因此在创建实例的适合，可以赋初始大小，减少扩容次数，提高性能。

# String类能被继承吗?

不能被继承，因为string是final修饰的。

# 谈谈static关键字？

1. 属于类而不是对象，可以通过类名直接访问。

2. 方法和变量都可以用。

3. 抽象类的抽象方法不能被static修饰，因为静态方法不能被重写。

4. 静态方法里面不能调用非静态方法，因为非静态方法调用需要创建对象。

5. static不能修饰局部变量，因为static修饰的变量作用域是全局。

# 静态方法能被重写吗?

不能，因为方法重写是在程序运行时选择要执行哪个方法，而static修饰的方法是在程序编译时就已经确定要执行哪个方法。

# 接口和抽象类的区别？

1. 设计思路

接口是自上而下的，先约定接口，再实现。

抽象类是自下而上的，先有一些类，再抽象成共同的父类。

2. 方法实现

接口中的方法默认都是public，字段默认都是public static final。

抽象类中的方法和字段可以是任何访问修饰符。

3. 定义

接口用于定义一组行为规范，表示“能做什么”。

抽象类用于定义一个类的公共行为和状态，表示“是什么”。

4. 继承

一个类可以实现多个接口。

一个类只能继承一个抽象类。

# 如何自定义一个异常？

首先，申明一个异常类继承exception或者runtimeException类。

其次编写两个构造方法，一个有参，一个无参。

# 深拷贝和浅拷贝的区别？

1. 浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝。

2. 深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容。

根本区别：如果B拷贝了A，修改A，看B是否发生变化。

如果B改变，为浅拷贝；如果B不变，为深拷贝。

# Comparator和Comparable的区别？

1. Comparable是排序接口，他的泛型只能是自己，Comparator是比较接口，泛型是任意的。

2. Comparable接口用于定义对象的自然顺序，是排序接口，Comparator通常用于定义用户定制的顺序，是比较接口。

我们如果需要控制某个类的次序，而该类本身不支持排序（即没有实现Comparable接口），那我们就可以建立一个“该类的比较器”来进行排序。

# 你知道创建一个对象有哪几种方式吗？

1. 序列化、反序列化

2. 通过new一个对象

3. 通过反射创建

4. 还可以调用对象的clone方法克隆一个对象

# 什么是反射？

反射机制是在运行时，获取类的结构信息（如方法、字段、构造函数），并操作对象的一种机制。

优点：反射机制提供了在运行时动态地创建对象、调用方法、访问字段等功能，而无需在编译时知道这些类的具体信息。

好处：提高了代码灵活性和扩展性，降低耦合性，不用硬编码具体哪个类。

# 获得class对象的几种方式？

1. 类名.class

2. 对象.getClass

3. Class.forName(包名类名)

# 开发中，你觉得哪些地方用到了反射机制？

1. JDBC中，利用反射动态加载了数据库驱动程序。
`java Class.forName('com.mysql.jdbc.Driver');//加载MySQL的驱动类` 

2. Tomcat服务器中通过反射调用了Sevlet的方法

3. 很多框架都用到反射机制，注入属性，调用方法，如Spring通过xml文件生成bean对象并给属性赋值

4. Eclispe、IDEA等开发工具利用反射动态刨析对象的类型与结构，动态提示对象的属性和方法

# 如何通过反射创建对象并给属性赋值和调用方法?

1. 先通过反射创建class对象

```java
Class<?> clazz = Class.forName("com.mianshiya.MyClass");
// 或者
Class<?> clazz = MyClass.class;
// 或者
Class<?> clazz = obj.getClass();

```

2. 获取构造器，创建实例

```java
Object obj = clazz.newInstance(); // 已过时
Constructor<?> constructor = clazz.getConstructor();
Object obj = constructor.newInstance();
```

3. 通过class对象获得一个属性对象，允许访问私有字段

```java
Field field = clazz.getField("myField");
field.setAccessible(true); // 允许访问 private 字段
Object value = field.get(obj);
field.set(obj, newValue);
```

4. 通过class对象获得一个方法对象，调用方法

```java
Method method = clazz.getMethod("myMethod", String.class);
Object result = method.invoke(obj, "param");
```

范例：

```java
public class Person {
    private String name;
    private int age;

    public Person() {}

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

    public void introduce() {
        System.out.println("Hello, my name is " + name + " and I am " + age + " years old.");
    }
}
```

```java
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ReflectionExample {
    public static void main(String[] args) {
        try {
            // 加载Person类
            // 先通过反射创建class对象
            Class<?> personClass = Class.forName("Person");

            // 创建Person对象
            // 获取构造器
            Constructor<?> constructor = personClass.getConstructor();
            // 创建实例
            Object personInstance = constructor.newInstance();

            // 设置name属性
            // 通过class对象获得一个属性对象
            Field nameField = personClass.getDeclaredField("name");
            // 允许访问私有字段
            nameField.setAccessible(true);
            nameField.set(personInstance, "Alice");

            // 设置age属性
            // 通过class对象获得一个属性对象
            Field ageField = personClass.getDeclaredField("age");
            // 允许访问私有字段
            ageField.setAccessible(true);
            ageField.setInt(personInstance, 30);

            // 调用introduce方法
            // 通过class对象获得一个方法对象
            Method introduceMethod = personClass.getMethod("introduce");
            // 调用方法
            introduceMethod.invoke(personInstance);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

# Java中的Optional类是什么？有什么作用？

Optional类是Java 8引入的一个容器类，用于表示一个值存在或不存在的请客。

作用：处理null值的问题，避免空指针异常。

# 讲一下I/O流？

输入流与输出流：

输入流(Input Stream)：用于读取数据的流。

输出流(Output Stream)：用于写入数据的流。

按照处理的数据类型，基于这两种输入输出的类型进行分类。

1. 字节流(Byte Streams)

输入流：Inputstream，常用以下几个输入流：

    FilelnputStream：从文件中读取字节数据。

    BufferedInputStream：为输入流提供缓冲功能，提高读取性能。

    DatalnputStream：读取基本数据类型的数据。

输出流：OutputStream，常用以下几个输出流：

    FileOutputStream：将字节数据写入文件。

    BufferedOutputStream：为输出流提供缓冲功能，提高写入性能。

    DataOutputStream：写入基本数据类型的数据。

2. 字符流(Character Streams)

输入流：Reader，常用以下几个输入流：

    FileReader：从文件中读取字符数据。

    BufferedReader：为字符输入流提供缓冲功能提高读取性能。

    InputStreamReader：将字节流转换为字符流。

输出流：Writer，常用以下几个输出流：

    FileWriter：将字符数据写入文件。

    BufferedWriter：为字符输出流提供缓冲功能，提高写入性能。

    OutputStreamWriter：将字符流转换为字节流。

缓冲流：

缓冲流是对基础流的包装，可以显著提高 I0 性能。

常见的缓冲流有BufferedReader、BufferedInputStream、BufferedOutputStream、BufferedWriter。

它们通过内部缓冲区减少实际 I/0 操作的次数。

在处理大文件或频繁 VO 操作时，使用缓冲流可以有效提高性能。

# 访问修饰符有哪些？

访问控制修饰符：

1. public：
    任何地方都可以访问。
    类、接口、构造器、方法和变量都可以使用。
    可以看作是“公开”的意思，即对所有人都开放。

2. protected：
    同一个包内可以访问；不同包内的子类也可以访问。
    主要用于方法和变量。
    比较像是“半公开”，只对自己人（同包）及自己的后代（子类）开放。

3. default (无修饰符)：
    同一个包内可以访问。
    如果没有写任何访问修饰符，默认就是这个。
    就像是一家人内部的事，外人不知道也不需要知道。

4. private：
    只能在定义它的类中被访问。
    用于方法和变量。
    完全私密，就像个人日记一样，只有自己能看。

非访问控制修饰符：

5. final：
    对于变量：一旦赋值就不能再改变。
    对于方法：不能被子类重写。
    对于类：不能被继承。
    简单来说，就是“最后的决定”或“不可更改”。

6. static：
    属于类而不是对象，可以通过类名直接访问。
    方法和变量都可以用。
    就像是公司的公共设施，每个人都能用，但不需要每个人都拥有一个副本。

7. abstract：
    可以用来修饰类和方法。
    抽象类不能实例化，必须被继承。
    抽象方法只有声明没有实现，必须由子类重写。
    像是给未来留下的空白页，等着后来者去填充内容。

8. synchronized：
    保证线程安全，一次只能有一个线程执行该方法或代码块。
    通常用于多线程环境下的同步问题。
    好比是锁门，确保同一时间只有一个用户进入房间。

9. volatile：
    保证变量的可见性，当一个线程修改了这个变量的值，新值对其他线程来说是可以立即得知的。
    适用于共享资源的情况。
    可以想象成即时通讯工具，发送的信息能够立刻被接收方看到。

10. transient：
    用在字段上，表示该字段不会被序列化。
    当你不想保存某些敏感信息（如密码）时会用到。
    就好比是旅行时不带贵重物品，防止丢失。

11. native：
    用在方法上，表明该方法是由底层操作系统提供的，通常是通过C/C++编写的。
    Java本身不提供具体实现，而是调用外部库。
    类似于聘请外籍专家来解决特定的技术难题。

总结：

- public, protected, default, private 控制谁可以看到你的东西。
- final 说定了就别改了。
- static 大家共用的东西。
- abstract 先画个框架，细节留给别人填。
- synchronized 一次只能一个人进。
- volatile 一有变化大家都知道。
- transient 不想留下痕迹的东西。
- native 找外援帮忙干活。

# 静态方法和实例方法的区别？

1. 静态方法
    使用static关键字声明的方法。
    属于类，而不是类的实例。
    可以通过类名直接调用，也可以通过对象调用，但这种方式不推荐，因为它暗示了实例相关性。
    可以访问类的静态变量和静态方法。不能直接访问实例变量和实例方法(因为实例变量和实例方法属于对象)
    随着类的加载而加载，随着类的卸载而消失。
    
    典型用途:
    工具类方法。
    工厂方法，用于返回类的实例。

    注意事项：
    静态方法中不能使用this关键字，因为this代表当前对象实例，而静态方法属于类，不属于任何实例。
    静态方法可以被重载(同类中方法名相同，但参数不同)，但不能被子类重写(因为方法绑定在编译时已确定)。实例方法可以被重载，也可以被子类重写。
    实例方法中可以直接调用静态方法和访问静态变量，即不支持方法的运行时动态绑定。
    静态方法不具有多态性，即不支持方法的运行时动态绑定。

2. 实例方法
    不使用static关键字声明的方法。
    属于类的实例。
    必须通过对象来调用。
    可以访问实例变量和实例方法。也可以访问类的静态变量和静态方法。
    随着对象的创建而存在，随着对象的销毁而消失。

    典型用途：
    操作或修改对象的实例变量。
    执行与对象状态相关的操作。

# for和foreach的区别？

for循环较为灵活，可以通过索引访问数组元素，对其进行修改和删除。

foreach适用与简单的遍历，不需要访问当前索引。

# Object类有什么方法？

1. equlas：比较。

2. hasCode：获取对象的hash值。

3. finalize：垃圾回收。

4. notify：线程同步中，唤醒某一个等待的线程。

5. notifyAll：线程同步中，唤醒所有等待的线程。

6. wait：让该线程进去等待状态。

7. toString：返回该对象的字符串形式。

8. getClass：获取该通过的class对象，用于反射。

9. clone 拷贝，创建一个相同的副本，有深拷贝和浅拷贝。

# 什么是不可变类？

不可变类是指初始化以后就不能修改的类。

特点：

1. 类用final修饰。

2. 变量用private final修饰。

3. 不提供任何修改对象状态的方法，如setter方法。

4. 通过构造函数初始化所有字段。

5. 如果类包含可变对象的引用，确保这些引用在对象外部无法被修改，如getter方法中返回对象的副本（new一个对象）来保护可变对象。

例如常见的String、Integer、BigDecimal、LocalDate等。

好处：线程安全、缓存友好、防止状态不一致。

坏处：频繁修改会浪费资源。

# Exception和Error的区别？

1. Exception表示可以被处理的程序异常。

编译期异常：在编译时需要显示处理（使用try-catch或者throws声明抛出）。
    例如IO异常。

运行时异常：不需要显示处理。
    例如空指针异常、非法参数异常。

2. Error表示系统级别的不可恢复错误。
    例如OOM、StackOverflowError。

注意事项：

1. 尽量不要捕获Exception，捕获特定的异常。除了别人可以看懂之外，也可以避免捕获不想捕获的异常。

2. 捕获异常之后，需要明确的将异常信息记录到日志中。

3. 在可能出现异常的地方尽早捕获，不要在调用了好多个方法之后捕获。

4. 只在有必要try-catch的地方try-catch。

5. 可以使用if/else来判断的就不要用异常，因为异常肯定比条件语句低效。

6. 不要在finally中return或者处理返回值。

# Java中的参数传递是按值传递还是按引用传递？

无论是基本数据类型，还是应用数据类型，都是按值传递。

基本数据类型传递的是值的副本，即基本数据类型的数组本身。

引用数据类型传递的是引用的副本，即对象引用的内存地址。

范例：

```java
public class ParameterPassing {
    public static void main(String[] args) {
        int a = 5;
        modifyPrimitive(a);
        System.out.println("After modifyPrimitive: " + a); // 输出: 5

        MyObject obj = new MyObject();
        obj.value = 10;
        modifyObject(obj);
        System.out.println("After modifyObject: " + obj.value); // 输出: 20

        resetReference(obj);
        System.out.println("After resetReference: " + obj.value); // 输出: 20
    }

    public static void modifyPrimitive(int num) {
        num = 10; // 仅仅修改了副本，不影响原始变量
    }

    public static void modifyObject(MyObject obj) {
        obj.value = 20; // 修改了对象的属性，会影响原始对象
    }

    public static void resetReference(MyObject obj) {
        obj = new MyObject(); // 修改的是引用的副本，不影响原始对象
        obj.value = 30;
    }
}

class MyObject {
    int value;
}
```

# 面向对象和面向过程的区别？

1. 面向对象编程(Object OrientedProgramming)将数据和方法封装成对象，作为程序的基本单元来组织代码，并且运用提炼出的 封装、继承和多态来作为代码设计指导注重代码复用和灵活性。

2. 面向过程编程是以过程作为基本单元来组织代码的，是过程对应到代码中来就是函数，将函数和数据分离，更关注步骤和流程，其实就是一条道的思路，关心如何设计一系列顺序执行的过程实现。

相对来说，OOP更加适合需要扩展性的复杂系统，而面向过程更适合简单任务的实现。

# 什么是内部类？

内部类是指在一个类的内部定义的类。

1. 成员内部类：可以访问外部类的所有成员。

2. 静态内部类：可以访问外部类的静态成员。

3. 局部内部类：定义在方法或代码块中，仅在代码块或方法中可见，通常用于临时的对象构建。

4. 匿名内部类：没有类名的内部类，通常用于创建短期使用的对象实例，尤其是在接口回调或者事件处理时被广泛使用。

# JDK8的新特性？

1. 用元空间替代了永久代。

2. 引入了Lambda表达式。

3. 引入了日期类、接口默认方法、静态方法。

4. 新增Stream流式接口。

5. 引入Optional类。

6. 新增了CompletableFuture、StampedLock等并发实现类。

# 什么是动态代理？

动态代理是一种在运行时创建代理对象的机制。

Java中的动态代理主要通过java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口来实现。

基本概念：

- 代理对象：这是一个实现了目标接口的类的实例，它可以将所有方法调用转发到真实的业务逻辑对象。

- InvocationHandler：这是处理代理对象上方法调用的接口。每当调用代理对象的方法时，实际执行的是InvocationHandler中定义的invoke方法。

- Proxy：这是用来生成代理对象的类。它提供了一个静态方法newProxyInstance，用于创建代理对象。

范例：

```java
package com.gujintao.ProxyExample.JDK;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理类的接口
 * @author: gujintao
 * @create: 2024-10-25 23:09
 **/
public interface GreetingService {
    void greet(String name);
}
```

```java
package com.gujintao.ProxyExample.JDK;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理类
 * @author: gujintao
 * @create: 2024-10-25 23:10
 **/
public class GreetingServiceImpl implements GreetingService {
    @Override
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
}
```

1. 创建InvocationHandler：实现InvocationHandler接口，定义如何处理代理对象上的方法调用

```java
package com.gujintao.ProxyExample.JDK;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：实现了InvocationHandler接口的切面类
 * @author: gujintao
 * @create: 2024-10-25 23:12
 */
public class LoggingInvocationHandler implements InvocationHandler {
    // 真实的服务对象
    private final Object target;

    public LoggingInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 在方法调用前打印日志
        System.out.println("Before method: " + method.getName());

        // 调用真实的方法
        Object result = method.invoke(target, args);

        // 在方法调用后打印日志
        System.out.println("After method: " + method.getName());

        return result;
    }
}
```

2. 创建代理对象并使用：使用Proxy.newProxyInstance方法创建代理对象，传入类加载器、接口数组和InvocationHandler

```java
package com.gujintao.ProxyExample.JDK;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理对象
 * @author: gujintao
 * @create: 2024-10-25 23:13
 **/
public class ProxyExample {
    public static void main(String[] args) {
        // 创建真实的服务对象
        GreetingService realService = new GreetingServiceImpl();

        // 创建InvocationHandler
        InvocationHandler handler = new LoggingInvocationHandler(realService);

        // 创建代理对象
        GreetingService proxyService = (GreetingService) Proxy.newProxyInstance(
                // 类加载器
                GreetingService.class.getClassLoader(),
                // 接口数组
                new Class<?>[]{GreetingService.class},
                // 处理器
                handler
        );

        // 通过代理对象调用方法
        proxyService.greet("World");
    }
}
```

# JDK动态代理和CGLIB动态代理有什么区别？

- JDK动态代理：基于反射，只能代理实现了接口的类。通过实现InvocationHandler接口进行方法增强操作，通过proxy创建代理对象。

- CGLIB动态代理：基于字节码生成技术，通过继承的方式实现代理，所有要求被代理类和方法不能使用final修饰。

CGLIB范例：

```java
package com.gujintao.ProxyExample.CGLIB;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：代理类
 * @author: gujintao
 * @create: 2024-10-25 23:21
 **/
public class GreetingService {
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
}
```

1. 创建MethodInterceptor：实现MethodInterceptor接口，定义如何处理代理对象上的方法调用

```java
package com.gujintao.ProxyExample.CGLIB;


import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：实现了MethodInterceptor接口的切面类
 * @author: gujintao
 * @create: 2024-10-25 23:24
 **/
public class LoggingMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        // 在方法调用前打印日志
        System.out.println("Before method: " + method.getName());

        // 调用真实的方法
        Object result = methodProxy.invokeSuper(obj, args);

        // 在方法调用后打印日志
        System.out.println("After method: " + method.getName());

        return result;
    }
}
```

2. 创建代理对象并使用：使用enhancer.create()方法创建代理对象，传入代理类和MethodInterceptor

```java
package com.gujintao.ProxyExample.CGLIB;

import net.sf.cglib.proxy.Enhancer;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：代理对象
 * @author: gujintao
 * @create: 2024-10-25 23:29
 **/
public class ProxyExample {
    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        // 设置代理类
        enhancer.setSuperclass(GreetingService.class);
        // 设置回调处理器
        enhancer.setCallback(new LoggingMethodInterceptor());
        // 创建代理对象
        GreetingService proxy = (GreetingService) enhancer.create();
        // 通过代理对象调用方法
        proxy.greet("world");
    }
}
```

