# 1. Java基础

## Java解释执行

我们开发的Java的源代码，首先通过Javac编译成为字节码（bytecode），然后，在运行时，通过 Java虚拟机（JVM）内嵌的 解释器将字节码转换成为最终的机器码。但是常见的JVM，比如我们大多数情况使用的Oracle JDK提供的Hotspot JVM，都提供了JIT（Just-In-Time）编译器，也就是通常所说的 动态编译器，JIT能够在运行时将热点代码编译成机器码，这种情况下部分热点代码就属于编译执行，而不是解释执行了

## JDK和JRE区别

**JRE(Java Runtime Enviroment)** 是 Java 的运行环境。面向 Java 程序的使用者，而不是开发者。如果你仅下载并安装了 JRE，那么你的系统只能运行 Java 程序。JRE 是运行 Java 程序所必须环境的集合，包含 JVM 标准实现及 Java 核心类库。它包括 Java 虚拟机、Java 平台核心类和支持文件。它不包含开发工具(编译器、调试器等)。

**JDK(Java Development Kit)** 又称 J2SDK(Java2 Software Development Kit)，是 Java 开发工具包，它提供了 Java 的开发环境(提供了编译器 javac 等工具，用于将 java 文件编译为 class 文件)和运行环境(提 供了 JVM 和 Runtime 辅助包，用于解析 class 文件使其得到运行)。如果你下载并安装了 JDK，那么你不仅可以开发 Java 程序，也同时拥有了运行 Java 程序的平台。JDK 是整个 Java 的核心，包括了 Java 运行环境(JRE)，一堆 Java 工具 tools.jar 和 Java 标准类库 (rt.jar)。

## final关键字

使用前必须初始化

1. final参数

不能改变参数指向的对象或基本类型

2. final方法

防止子类复写

过去出于效率考虑（编译器采用内嵌调用）

private方法隐式指定final

3. final类

不能被继承

类中所有方法隐式指定为final

## static关键字



## 类初始化

类的代码首次使用时加载

构造器方法隐式static

任意一个static成员被访问，就会被加载

## 类加载过程

3个阶段:

1. 加载
   1. **在加载阶段，类加载器将类的class文件的二进制数据读取到内存， 并保存到方法区，并在堆区生成该类的Class对象。**
2. 连接
   1. 验证：**在加载阶段，类加载器将类的class文件的二进制数据读取到内存， 并保存到方法区，并在堆区生成该类的Class对象。**JVM会对字节流进行如下验证:
      - 文件格式验证:会验证class文件是否符合虚拟机规范，如是否以0×CAFEBABE开头， 主次版本号是否在虚拟机规定范围类，常量池中的类型是否有JVM不支持的类型。
      - 元数据验证: 会对类的元信息进行语义分析，确保符合Java语法规范。
      - 字节码验证: 通过分析数据流和控制流，确保类的方法体的程序语义是合法的， 符合逻辑的。
      - 符号引用验证: 确保常量池中的符号引用能在解析阶段正常解析。
   2. 准备: 准备阶段会为类的静态变量初始化零值，如(0,0L,null,false).
   3. 解析: 解析阶段会将常量池中的符号引用转为直接引用。 符号引用包括类的全限定名，方法名的描述符，字段名的描述符。直接引用可以简单理解为目标的内存地址。
3. 初始化：**只有主动使用类才会初始化类，总共有8种情况会涉及到主动使用类。**
   1. 当jvm执行new指令时会初始化类没，即当程序创建一个类的实例对象。
   2. 当jvm执行getstatic指令时会初始化类，即程序访问类的静态变量(不是静态常量，常量归属于运行时常量池)。
   3. 当jvm执行putstatic指令时会初始化类，即程序给类的静态变量赋值。
   4. 当jvm执行invokestatic指令时会初始化类，即程序调用类的静态方法。
   5. 当使用反射主动访问这个类时,也会初始化类,如Class.forname("..."),newInstance()等等。
   6. 当初始化一个子类的时候，会先初始化这个子类的所有父类，然后才会初始化这个子类。
   7. 当一个类是启动类时，即这个类拥有main方法，那么jvm会首先初始化这个类。
   8. MethodHandle和VarHandle可以看作是轻量级的反射调用机制，而要想使用这2个调用， 就必须先使用findStatic/findStaticVarHandle来初始化要调用的类。

## 封装、继承和多态概念

**封装**的目的是隐藏事务内部的实现细节，以便提高安全性和简化编程。封装提供了合理的边界，避免外部调用者接触到内部的细节。我们在日常开发中，因为无意间暴露了细节导致的难缠 bug 太多了，比如在多线程环境暴露内部状态，导致的并发修改问题。从另外一个角度看，封装这种隐藏，也提供了简化的界面，避免太多无意义的细节浪费调用者的精力。

**继承**是代码复用的基础机制，类似于我们对于马、白马、黑马的归纳总结。但要注意，继承可以看作是非常紧耦合的一种关系，父类代码修改，子类行为也会变动。在实践中，过度滥用继承，可能会起到反效果。

**多态**，你可能立即会想到重写（override）和重载（overload）、向上转型。简单说，重写是父子类中相同名字和参数的方法，不同的实现；重载则是相同名字的方法，但是不同的参数，本质上这些方法签名是不一样的

## OOP设计原则

进行面向对象编程，掌握基本的设计原则是必须的，我今天介绍最通用的部分，也就是所谓的 S.O.L.I.D 原则。单一职责（Single Responsibility），类或者对象最好是只有单一职责，在程序设计中如果发现某个类承担着多种义务，可以考虑进行拆分。开关原则（Open-Close, Open for extension, close for modification），设计要对扩展开放，对修改关闭。换句话说，程序设计应保证平滑的扩展性，尽量避免因为新增同类功能而修改已有实现，这样可以少产出些回归（regression）问题。里氏替换（Liskov Substitution），这是面向对象的基本要素之一，进行继承关系抽象时，凡是可以用父类或者基类的地方，都可以用子类替换。接口分离（Interface Segregation），我们在进行类和接口设计时，如果在一个接口里定义了太多方法，其子类很可能面临两难，就是只有部分方法对它是有意义的，这就破坏了程序的内聚性。对于这种情况，可以通过拆分成功能单一的多个接口，将行为进行解耦。在未来维护中，如果某个接口设计有变，不会对使用其他接口的子类构成影响。依赖反转（Dependency Inversion），实体应该依赖于抽象而不是实现。也就是说高层次模块，不应该依赖于低层次模块，而是应该基于抽象。实践这一原则是保证产品代码之间适当耦合度的法宝。

## 多态

### 构造器多态方法行为

初始化实际过程：

1. 分配给对象的存储空间初始化为零
2. 调用基类构造器（基类构造器如果有派生类的重写方法，则会调用重写后的方法，在调用派生类构造器**之前**）
3. 按声明顺序初始化成员
4. 调用派生类构造器

## 接口&抽象类

### 抽象类、抽象方法

抽象方法：只有声明没有方法体

抽象类：包含一个或多个抽象方法（充分不必要条件），不包含也可设为抽象类（在类中的抽象方法没啥意义但想阻止创建类的对象时，这么做就很有用）

**private abstract**禁止

### 接口创建

只允许抽象方法，不用加**abstract**

Java8允许接口包含默认方法(default)和静态方法

接口属性被隐式指明为**static**和**final**

来自接口的方法必定为**public**，也可以显式声明

### 抽象类和接口

| 特性                 | 接口                                                       | 抽象类                                   |
| -------------------- | ---------------------------------------------------------- | ---------------------------------------- |
| 组合                 | 新类可以组合多个接口                                       | 只能继承单一抽象类                       |
| 状态                 | 不能包含属性（除了静态属性，不支持对象状态）               | 可以包含属性，非抽象方法可能引用这些属性 |
| 默认方法 和 抽象方法 | 不需要在子类中实现默认方法。默认方法可以引用其他接口的方法 | 必须在子类中实现抽象方法                 |
| 构造器               | 没有构造器                                                 | 可以有构造器                             |
| 可见性               | 隐式 **public**                                            | 可以是 **protected** 或 "friendly"       |

## 内部类

内部类自动拥有对其外部类所有成员的访问权

> 每个内部类都能独立地继承自一个（接口的）实现，所以无论外部类是否已经继承了某个（接口的）实现，对于内部类没有影响

有效地实现了“多重继承”

内部类的其他特性：

1. 可以有多个实例，每个实例都有自己的状态信息，并且与外部类对象的信息相互独立
2. 单个外部类中，可以让多个内部类以不同的方式实现同一个接口，或继承同一个类
3. 创建内部类对象的时刻不依赖于外部类对象的创建
4. 没有”is-a“关系，它就是一个独立的实体



## 异常

要做好异常处理就必须了解 Exception 和 Error 的区别，它们主要有以下异同：

- 首先 Exception 和 Error 都是继承于 Throwable 类，在 Java 中只有 Throwable 类型的实例才可以被抛出（throw）或者捕获（catch），Exception 和 Error 体现了JAVA 这门语言对于异常处理的两种方式。
- Exception 是 Java 程序运行中可预料的异常情况，我们可以获取到这种异常，并且对这种异常进行业务外的处理。它分为检查性异常和非检查性（RuntimeException）异常。两个根本的区别在于，检查性异常必须在编写代码时，使用 try catch 捕获（比如：IOException异常）。非检查性异常 在代码编写时，可以忽略捕获操作（比如：ArrayIndexOutOfBoundsException），这种异常是在代码编写或者使用过程中通过规范可以避免发生的。
- Error 是 Java 程序运行中不可预料的异常情况，这种异常发生以后，会直接导致 JVM 不可处理或者不可恢复的情况。所以这种异常不可能抓取到，比如 OutOfMemoryError、NoClassDefFoundError等。

## 强引用、弱引用

所谓强引用（“Strong” Reference），就是我们最常见的普通对象引用，只要还有强引用指向一个对象，就能表明对象还“活着”，垃圾收集器不会碰这种对象。对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显式地将相应（强）引用赋值为 null，就是可以被垃圾收集的了，当然具体回收时机还是要看垃圾收集策略。

软引用（SoftReference），是一种相对强引用弱化一些的引用，可以让对象豁免一些垃圾收集，只有当 JVM 认为内存不足时，才会去试图回收软引用指向的对象。JVM 会确保在抛出 OutOfMemoryError 之前，清理软引用指向的对象。软引用通常用来实现内存敏感的缓存，如果还有空闲内存，就可以暂时保留缓存，当内存不足时清理掉，这样就保证了使用缓存的同时，不会耗尽内存。

弱引用（WeakReference）并不能使对象豁免垃圾收集，仅仅是提供一种访问在弱引用状态下对象的途径。这就可以用来构建一种没有特定约束的关系，比如，维护一种非强制性的映射关系，如果试图获取时对象还在，就使用它，否则重现实例化。它同样是很多缓存实现的选择。

对于幻象引用，有时候也翻译成虚引用，你不能通过它访问对象。幻象引用仅仅是提供了一种确保对象被 finalize 以后，做某些事情的机制，比如，通常用来做所谓的 Post-Mortem 清理机制，我在专栏上一讲中介绍的 Java 平台自身 Cleaner 机制等，也有人利用幻象引用监控对象的创建和销毁。

## 双亲委派机制

**双亲委派机制是为了防止类被重复加载，避免核心API遭到恶意破坏。** 如Object类，它由BootstrapClassLoader在JVM启动时加载。 如果没有双亲委派机制，那么Object类就可以被重写，其带来的后果将无法想象。

#### 双亲委派机制的实现原理?

每个类都有其对应的类加载器。

双亲委派机制是指在加载一个类的时候，JVM会判断这个类是否已经被其类加载器加载过了。 如果已经加载过了，那么直接返回这个类。 **如果没有加载，就使用这个类对应的加载器的父类加载器判断， 一层一层的往上判断，最终会由BootstrapClassLoader判断。** 如果BootstrapClassLoader判断都没有加载这个类, **那么就由BootstrapClassLoader尝试加载。 如果BootstrapClassLoader加载失败了， 就由BootstrapClassLoader的子类加载器们加载。**

**在jdk9之后，由于模块化的到来，双亲委派机制也变化了一点: 如果类没有被加载，那么会根据类名找到这个类的模块。 如果找到了这个类的模块， 就由这个类的模块加载，否则仍然使用父类加载器加载。**

可以看出:在加载一个类时，是由下自上判断类是否被加载的。如果类没有被加载， 就由上自下尝试加载类。

## 反射&动态代理

反射机制是 Java 语言提供的一种基础功能，赋予程序在运行时自省（introspect，官方用语）的能力。通过反射我们可以直接操作类或者对象，比如获取某个对象的类定义，获取类声明的属性和方法，调用方法或者构造对象，甚至可以运行时修改类定义。

### 反射

1 关于反射
反射最大的作用之一就在于我们可以不在编译时知道某个对象的类型，而在运行时通过提供完整的”包名+类名.class”得到。注意：不是在编译时，而是在运行时。

功能：
•在运行时能判断任意一个对象所属的类。
•在运行时能构造任意一个类的对象。
•在运行时判断任意一个类所具有的成员变量和方法。
•在运行时调用任意一个对象的方法。
说大白话就是，利用Java反射机制我们可以加载一个运行时才得知名称的class，获悉其构造方法，并生成其对象实体，能对其fields设值并唤起其methods。

应用场景：
反射技术常用在各类通用框架开发中。因为为了保证框架的通用性，需要根据配置文件加载不同的对象或类，并调用不同的方法，这个时候就会用到反射——运行时动态加载需要加载的对象。

特点：
由于反射会额外消耗一定的系统资源，因此如果不需要动态地创建一个对象，那么就不需要用反射。另外，反射调用方法时可以忽略权限检查，因此可能会破坏封装性而导致安全问题。

### 动态代理

首先，它是一个代理机制。如果熟悉设计模式中的代理模式，我们会知道，代理可以看作是对调用目标的一个包装，这样我们对目标代码的调用不是直接发生的，而是通过代理完成。其实很多动态代理场景，我认为也可以看作是装饰器（Decorator）模式的应用。

**动态代理**
为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在两者之间起到中介的作用（可类比房屋中介，房东委托中介销售房屋、签订合同等）。
所谓动态代理，就是实现阶段不用关心代理谁，而是在运行阶段才指定代理哪个一个对象（不确定性）。如果是自己写代理类的方式就是静态代理（确定性）。

**组成要素：**
(动态)代理模式主要涉及三个要素：
其一：抽象类接口
其二：被代理类（具体实现抽象接口的类）
其三：动态代理类：实际调用被代理类的方法和属性的类

**实现方式:**
实现动态代理的方式很多，比如 JDK 自身提供的动态代理，就是主要利用了反射机制。还有其他的实现方式，比如利用字节码操作机制，类似 ASM、CGLIB（基于 ASM）、Javassist 等。
举例，常可采用的JDK提供的动态代理接口InvocationHandler来实现动态代理类。其中invoke方法是该接口定义必须实现的，它完成对真实方法的调用。通过InvocationHandler接口，所有方法都由该Handler来进行处理，即所有被代理的方法都由InvocationHandler接管实际的处理任务。此外，我们常可以在invoke方法实现中增加自定义的逻辑实现，实现对被代理类的业务逻辑无侵入。

通过代理可以让调用者与实现者之间解耦。比如进行 RPC 调用，框架内部的寻址、序列化、反序列化等，对于调用者往往是没有太大意义的，通过代理，可以提供更加友善的界面。

JDK Proxy 例子，非常简单地实现了动态代理的构建和代理操作。首先，实现对应的 InvocationHandler；然后，以接口 Hello 为纽带，为被调用目标构建代理对象，进而应用程序就可以使用代理对象间接运行调用目标的逻辑，代理为应用插入额外逻辑（这里是 println）提供了便利的入口。

如果被调用者没有实现接口，而我们还是希望利用动态代理机制，那么可以考虑其他方式。我们知道 Spring AOP 支持两种模式的动态代理，JDK Proxy 或者 cglib，如果我们选择 cglib 方式，你会发现对接口的依赖被克服了。cglib 动态代理采取的是创建目标类的子类的方式，因为是子类化，我们可以达到近似使用被调用者本身的效果。

## Integer和int

### 自动拆箱&装箱

自动装箱实际上算是一种语法糖。什么是语法糖？可以简单理解为 Java 平台为我们自动进行了一些转换，保证不同的写法在运行时等价，它们发生在编译阶段，也就是生成的字节码是一致的。像前面提到的整数，javac 替我们自动把装箱转换为 Integer.valueOf()，把拆箱替换为 Integer.intValue()，这似乎这也顺道回答了另一个问题，既然调用的是 Integer.valueOf，自然能够得到缓存的好处啊。

1、编译期 2、自动拆装箱就是编译期自己调用 valueOf 和intValue 方法有使用缓存 3、 （1）空间对比int而言，Integer占用更多内存空间有不必要开销 （2）int 访问是直接访问数据内存地址，Integer是通过引用找到数据内存地址 4、 （1）使用缓存默认 -128～127。调用valueOf才使用缓存，new 不使用缓存 （2）和String一样数据不可变，有final 修饰。 （3）使用BYTES 字段修饰，避免了因为环境 64位或32位不同造成的影响。

这种缓存机制并不是只有 Integer 才有，同样存在于其他的一些包装类，比如：

Boolean，缓存了 true/false 对应实例，确切说，只会返回两个常量实例 Boolean.TRUE/FALSE。

Short，同样是缓存了 -128 到 127 之间的数值。

Byte，数值有限，所以全部都被缓存。

Character，缓存范围’\u0000’ 到 ‘\u007F’。

**缓存上限值实际是可以根据需要调整的，JVM 提供了参数设置：-XX:AutoBoxCacheMax=N**

## 文件

### 文件拷贝方式

1. 利用 java.io 类库，直接为源文件构建一个 FileInputStream 读取，然后再为目标文件构建一个 FileOutputStream，完成写入工作。

```java

public static void copyFileByStream(File source, File dest) throws
        IOException {
    try (InputStream is = new FileInputStream(source);
         OutputStream os = new FileOutputStream(dest);){
        byte[] buffer = new byte[1024];
        int length;
        while ((length = is.read(buffer)) > 0) {
            os.write(buffer, 0, length);
        }
    }
 }

```

2. 利用 java.nio 类库提供的 transferTo 或 transferFrom 方法实现

```java
public static void copyFileByChannel(File source, File dest) throws
        IOException {
    try (FileChannel sourceChannel = new FileInputStream(source)
            .getChannel();
         FileChannel targetChannel = new FileOutputStream(dest).getChannel
                 ();){
        for (long count = sourceChannel.size() ;count>0 ;) {
            long transferred = sourceChannel.transferTo(
                    sourceChannel.position(), count, targetChannel);            sourceChannel.position(sourceChannel.position() + transferred);
            count -= transferred;
        }
    }
 }

```

总体上来说，NIO transferTo/From 的方式可能更快，因为它更能利用现代操作系统底层机制，避免不必要拷贝和上下文切换。

### 拷贝实现机制

当我们使用输入输出流进行读写时，实际上是进行了多次上下文切换，比如应用读取数据时，先在内核态将数据从磁盘读取到内核缓存，再切换到用户态将数据从内核缓存读取到用户缓存。

所以，这种方式会带来一定的额外开销，可能会降低 IO 效率。而基于 NIO transferTo 的实现方式，在 Linux 和 Unix 上，则会使用到零拷贝技术，数据传输并不需要用户态参与，省去了上下文切换的开销和不必要的内存拷贝，进而可能提高应用拷贝性能。注意，transferTo 不仅仅是可以用在文件拷贝中，与其类似的，例如读取磁盘文件，然后进行 Socket 发送，同样可以享受这种机制带来的性能和扩展性提高。

**如何提高类似拷贝等IO操作的性能**

如何提高类似拷贝等 IO 操作的性能，有一些宽泛的原则：在程序中，使用缓存等机制，合理减少 IO 次数（在网络通信中，如 TCP 传输，window 大小也可以看作是类似思路）。

使用 transferTo 等机制，减少上下文切换和额外 IO 操作。

尽量减少不必要的转换过程，比如编解码；对象序列化和反序列化，比如操作文本文件或者网络通信，如果不是过程中需要使用文本信息，可以考虑不要将二进制信息转换成字符串，直接传输二进制信息。

# 2. Java集合

## ArrayList

扩容1.5倍

![image-20210618111127275](/Users/cuweir/Library/Application Support/typora-user-images/image-20210618111127275.png)

ArrayList在执行插入元素是超过当前数组预定义的最大值时，数组需要扩容，扩容过程需要调用底层System.arraycopy()方法进行大量的数组复制操作；在删除元素时并不会减少数组的容量（如果需要缩小数组容量，可以调用trimToSize()方法）；在查找元素时要遍历数组，对于非null的元素采取equals的方式寻找。

## Vector

扩容2倍

## LinkedList

LinkedList在插入元素时，须创建一个新的Entry对象，并更新相应元素的前后元素的引用；在查找元素时，需遍历链表；在删除元素时，要遍历链表，找到要删除的元素，然后从链表上将此元素删除即可。
Vector与ArrayList仅在插入元素时容量扩充机制不一致。对于Vector，默认创建一个大小为10的Object数组，并将capacityIncrement设置为0；当插入元素数组大小不够时，如果capacityIncrement大于0，则将Object数组的大小扩大为现有size+capacityIncrement；如果capacityIncrement<=0,则将Object数组的大小扩大为现有大小的2倍。

## HashMap

### 散列之原理

1. 扰动函数

![image-20210618111306249](/Users/cuweir/Library/Application Support/typora-user-images/image-20210618111306249.png)

2. 低位掩码

![image-20210618111351111](/Users/cuweir/Library/Application Support/typora-user-images/image-20210618111351111.png)

### 结构

它可以看作是数组（Node[] table）和链表结合组成的复合结构，数组被分为一个个桶（bucket），通过哈希值决定了键值对在这个数组的寻址；哈希值相同的键值对，则以链表形式存储，你可以参考下面的示意图。这里需要注意的是，如果链表大小超过阈值（TREEIFY_THRESHOLD, 8），图中的链表就会被改造为树形结构。还需要桶数量超过 MIN_TREEIFY_CAPACITY 64 才会树化,否则只是扩容 resize()。

resize发生在创建初始存储表格，或者在容量不满足需求的时候，进行扩容（resize）。

依据 resize 源码，不考虑极端情况（容量理论最大极限由 MAXIMUM_CAPACITY 指定，数值为 1<<30，也就是 2 的 30 次方），我们可以归纳为：

- 门限值等于（负载因子）x（容量），如果构建 HashMap 的时候没有指定它们，那么就是依据相应的默认常量值。
- 门限通常是以倍数进行调整 （newThr = oldThr << 1），我前面提到，根据 putVal 中的逻辑，当元素个数超过门限大小时，则调整 Map 大小。
- 扩容后，需要将老的数组中的元素重新放置到新的数组，这是扩容的一个主要开销来源。

index取决于：

```java
i = (n - 1) & hash
```

这种方式能避免只取低位进行计算的结果，通常我们的n都不会很大，如果直接拿hashCode的值，那么高位的哈希值其实是没有参与进来的；这样将自己的高位与低于异或，其实是混合了高低位，变相了保留了高位的信息，以此来加大低位的随机性。

**解决哈希冲突的办法**

开放定址法
基本思想是：当关键字key的哈希地址p=H（key）出现冲突时，以p为基础，产生另一个哈希地址p1，如果p1仍然冲突，再以p为基础，产生另一个哈希地址p2，…，直到找出一个不冲突的哈希地址pi ，将相应元素存入其中。

再哈希法
这种方法是同时构造多个不同的哈希函数：
Hi=RH1（key） i=1，2，…，k
当哈希地址Hi=RH1（key）发生冲突时，再计算Hi=RH2（key）……，直到冲突不再产生。这种方法不易产生聚集，但增加了计算时间。

链地址法
这种方法的基本思想是将所有哈希地址为i的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第i个单元中，因而查找、插入和删除主要在同义词链中进行。链地址法适用于经常进行插入和删除的情况。

建立公共溢出区
这种方法的基本思想是：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表。

**红黑树**

1. 节点是红色或黑色。
2. 根是黑色。
3. 所有叶子都是黑色（叶子是 NIL 节点）。
4. 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）
5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

**多线程不安全**

1. 1.7中拉链法插⼊入节点采⽤用头插法，可能形成环从⽽而导致死锁; 
2. 2. 1.8改为了了尾插法，不不会造成死锁，但有数据丢失的危险。

## ConcurrentHashMap

ConcurrentHashMap 的核心就在于其put元素时，利用synchronized局部锁和CAS乐观锁机制大大提升了本集合的并发能力，比JDK7的分段锁性能更强

当前指定数组位置无元素时，使用CAS操作 将 Node键值对 放入对应的数组下标。

出现hash冲突，则用synchronized局部锁锁住，若当前hash对应的节点是链表的头节点，遍历链表

若找到对应的node节点，则修改node节点的val，否则在链表末尾添加node节点；倘若当前节点是红黑树的根节点，在树结构上遍历元素，更新或增加节点

**与 JDK1.7 在同步机制上的区别** 总结如下：
JDK1.7 使用的是分段锁机制，其内部类 Segment 继承了 ReentrantLock，将 容器内的数组划分成多段区域，每个区域对应一把锁，相比于 HashTable 确实提升了不少并发能力，但在数据量庞大的情况下，性能依然不容乐观，只能通过不断的增加锁来维持并发性能。而 JDK1.8 则使用了 CAS 乐观锁 + synchronized 局部锁 处理并发问题，锁粒度更细，即使数据量很大也能保证良好的并发性。

在 Java 8 和之后的版本中，ConcurrentHashMap 发生了哪些变化呢？

- 总体结构上，它的内部存储变得和我在专栏上一讲介绍的 HashMap 结构非常相似，同样是大的桶（bucket）数组，然后内部也是一个个所谓的链表结构（bin），同步的粒度要更细致一些。
- 其内部仍然有 Segment 定义，但仅仅是为了保证序列化时的兼容性而已，不再有任何结构上的用处。
- 因为不再使用 Segment，初始化操作大大简化，修改为 lazy-load 形式，这样可以有效避免初始开销，解决了老版本很多人抱怨的这一点。
- 数据存储利用 volatile 来保证可见性。
- 使用 CAS 等操作，在特定场景进行无锁并发操作。
- 使用 Unsafe、LongAdder 之类底层手段，进行极端情况的优化。

先看看现在的数据存储内部实现，我们可以发现 Key 是 final 的，因为在生命周期中，一个条目的 Key 发生变化是不可能的；与此同时 val，则声明为 volatile，以保证可见性。

**put**

CAS 加锁
1.8中不依赖与segment加锁，segment数量与桶数量一致；
首先判断容器是否为空，为空则进行初始化利用volatile的sizeCtl作为互斥手段，如果发现竞争性的初始化，就暂停在那里，等待条件恢复，否则利用CAS设置排他标志（U.compareAndSwapInt(this, SIZECTL, sc, -1)）;否则重试
对key hash计算得到该key存放的桶位置，判断该桶是否为空，为空则利用CAS设置新节点
否则使用synchronize加锁，遍历桶中数据，替换或新增加点到桶中
最后判断是否需要转为红黑树，转换之前判断是否需要扩容

**size**

真正的逻辑是在 sumCount 方法中，虽然思路仍然和以前类似，都是分而治之的进行计数，然后求和处理，但实现却基于一个奇怪的 CounterCell。

对于 CounterCell 的操作，是基于 java.util.concurrent.atomic.LongAdder 进行的，是一种 JVM 利用空间换取更高效率的方法，利用了Striped64内部的复杂逻辑。这个东西非常小众，大多数情况下，建议还是使用 AtomicLong，足以满足绝大部分应用的性能需求。

## LinkedHashMap

LinkedHashMap 的有序可以按两种顺序排列，一种是按照插入的顺序，一种是按照访问的顺序（初始化 LinkedHashMap 对象时设置 accessOrder 参数为 true），而其内部是靠 建立一个双向链表 来维护这个顺序的，在每次插入、删除后，都会调用一个函数来进行 双向链表的维护，这也是实现 LRU Cache 功能的基础。

## HashSet

HashSet 的特点如下：

- 内部使用 HashMap 的 key 存储元素，以此来保证**元素不重复**；
- HashSet 是无序的，因为 HashMap 的 key 是**无序**的；
- HashSet 中允许有一个 null 元素，因为 HashMap 允许 key 为 null；
- HashSet 是**非线程安全**的。

## String

Object的 hashCode()返回该对象的内存地址编号，而equals()比较的是内存地址是否相等；

需要注意的是当equals()方法被重写时，hashCode()也要被重写；
按照一般hashCode()方法的实现来说，equals()相等的两个对象，hashcode()必须保持相等；

equals()不相等的两个对象，hashcode()未必不相等

一个类如果要作为 HashMap 的 key，必须重写equals()和hashCode()方法

```java
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                // 选择值31是因为它是奇数素数。如果它是偶数，乘法溢出，信息就会丢失，因为乘2等于移位
                // 乘法可以用移位和减法代替，以获得更好的性能：31*i==（i<<5）-i
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```

### StringBuffer和StringBuilder



![image-20210615181913032](/Users/cuweir/Library/Application Support/typora-user-images/image-20210615181913032.png)







# 3. JVM

## 常用命令

### jvm常用命令

- jps jps：显示当前用户的所有java进程的PID
   jps -v 3331：显示虚拟机参数
   jps -m 3331：显示传递给main()函数的参数
   jps -l 3331：显示主类的全路径
- jstat jstat -gc 3331 250 20 ：查询进程2764的垃圾收集情况，每250毫秒查询一次，一共查询20次
   jstat -gccause：额外输出上次GC原因
   jstat -calss：件事类装载、类卸载、总空间以及所消耗的时间
- jmap jmap -histo 3331：查看堆内存(histogram)中的对象数量及大小
   jmap -heap 3331：查看java 堆（heap）使用情况
   jmap -histo:live 3331：JVM会先触发gc，然后再统计信息，查看堆内存中的对象数量及大小
   jmap -dump:format=b,file=heapDump 3331：将内存使用的详细情况输出到文件，之后一般使用其他工具进行分析
- jstack jstack 3331：查看线程情况
   jstack -F 3331：正常输出不被响应时，使用该指令
   jstack -l 3331：除堆栈外，显示关于锁的附件信息

### 常见问题定位过程

1. 使用uptime查看当前load，发现load飙高

   ```
   ➜  ~ uptime
   13:29  up 23:41, 3 users, load averages: 10 10 10
   复制代码
   ```

2. 使用top命令，查看占用CPU较高的进程ID

   top -c

   ```
   ➜  ~ top
   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
   1893 admin     20   0 7127m 2.6g  38m S 181.7 32.6  10:20.26 java
   复制代码
   ```

   发现PID为1893的进程占用CPU 181%,而且是一个Java进程，基本断定是软件问题

3. 使用 top命令，查看具体是哪个线程占用率较高

   ```
   ➜  ~ top -Hp 1893
   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
   4519 admin     20   0 7127m 2.6g  38m R 18.6 32.6   0:40.11 java
   复制代码
   ```

4. 使用printf命令查看这个线程的16进制

   ```
   ➜  ~ printf %x 4519
   11a7
   复制代码
   ```

5. 使用jstack命令查看当前线程正在执行的方法

   ```
   ➜  ~ jstack 1893 |grep -A 200 11a7
   "thread-5" #500 daemon prio=10 os_prio=0 tid=0x00007f632314a800 nid=0x11a2 runnable [0x000000005442a000]
   java.lang.Thread.State: RUNNABLE
   at sun.misc.URLClassPath$Loader.findResource(URLClassPath.java:684)
   at sun.misc.URLClassPath.findResource(URLClassPath.java:188)
   at java.net.URLClassLoader$2.run(URLClassLoader.java:569)
   at java.net.URLClassLoader$2.run(URLClassLoader.java:567)
   at java.security.AccessController.doPrivileged(Native Method)
   at java.net.URLClassLoader.findResource(URLClassLoader.java:566)
   at org.hibernate.validator.internal.xml.ValidationXmlParser.getInputStreamForPath(ValidationXmlParser.java:248)
   at com.hollis.test.util.BeanValidator.validate(BeanValidator.java:30)
   
   复制代码
   ```

   从上面的线程的栈日志中，可以发现，当前占用CPU较高的线程正在执行我代码的com.hollis.test.util.BeanValidator.validate(BeanValidator.java:30)类。那么就可以去排查这个类是否用法有问题了。

6. 使用jmap内存使用的详细情况输出到文件，之后一般使用其他工具进行分    析

   ```
   ➜ ~ jmap -dump:format=b,file=heapDump 1893
   ```



## 常用的参数

**-XX:+UnlockExperimentalVMOptions **

**-XX:+UseCGroupMemoryLimitForHeap**

以便jvm可以检查控制组内存限制并计算最大堆大小

-**Xms20m**

JVM启动堆内存最小值20m

**-Xmx20m**

最大值20m

**-verbose:gc**

输出GC详细情况

-**Xss128k**

虚拟机栈大小

-**Xoss128k**

表示设置本地方法栈的大小为128k。不过HotSpot并不区分虚拟机栈和本地方法栈，因此对于HotSpot来说这个参数是无效的

**-XX:PermSize=10M**

表示JVM初始分配的永久代的容量，必须以M为单位

**-XX:MaxPermSize=10M**

表示JVM允许分配的永久代的最大容量，必须以M为单位，大部分情况下这个参数默认为64M

**-Xnoclassgc**

表示关闭JVM对类的垃圾回收

**-XX:+TraceClassLoading**

表示查看类的加载信息

**-XX:+TraceClassUnLoading**

表示查看类的卸载信息

**-XX:NewRatio=4**

表示设置年轻代：老年代的大小比值为1：4，这意味着年轻代占整个堆的1/5

**-XX:SurvivorRatio=8**

表示设置2个Survivor区：1个Eden区的大小比值为2:8，这意味着Survivor区占整个年轻代的1/5，这个参数默认为8

**-Xmn20M**

表示设置年轻代的大小为20M

**-XX:+HeapDumpOnOutOfMemoryError**

表示可以让虚拟机在出现内存溢出异常时Dump出当前的堆内存转储快照

**-XX:+UseG1GC**

表示让JVM使用G1垃圾收集器

**-XX:+PrintGCDetails**

表示在控制台上打印出GC具体细节

**-XX:+PrintGC**

表示在控制台上打印出GC信息

**-XX:PretenureSizeThreshold=3145728**

表示对象大于3145728（3M）时直接进入老年代分配，这里只能以字节作为单位

**-XX:MaxTenuringThreshold=1**

表示对象年龄大于1，自动进入老年代

**-XX:CompileThreshold=1000**

表示一个方法被调用1000次之后，会被认为是热点代码，并触发即时编译

**-XX:+PrintHeapAtGC**

表示可以看到每次GC前后堆内存布局

**-XX:+PrintTLAB**

表示可以看到TLAB的使用情况

**-XX:+UseSpining**

开启自旋锁

**-XX:PreBlockSpin**

更改自旋锁的自旋次数，使用这个参数必须先开启自旋锁


## JMM

### 结构

1. 线程私有区域
   1. PC：当前thread执行字节码的行号指示
   2. 虚拟机栈
      1. 栈帧：
         - 局部变量表
         - 操作数栈
         - 动态链接
         - 方法出口
   3. 本地方法区（C、C++）
2. 线程共享
   1. Java堆
      1. 存储的是数组和对象，new出来的
   2. 方法区
      1. 运行时常量池。类、静态变量、静态方法、常量、普通方法
3. 直接内存

### 运行时内存（堆，分代）

在 HotSpot 虚拟机中，内存被组织成三个分代：年轻代、老年代、永久代。

大部分对象初始化的时候都是在年轻代中的。

老年代存放经过了几次年轻代垃圾收集依然还活着的对象，还有部分大对象因为比较大所以分配的时候直接在老年代分配。

永久代，通常也叫 **方法区**，用于存储已加载类的元数据，以及存储运行时常量池等。

- 新生代（minor GC）
  - Eden区（8/10）
  - From Survivor区
  - To Survivor区
- 老年代（major GC）
- 永久代（Java8被元数据区取代，区别是其使用了本地内存）

### 线程

- 虚拟机线程
- 编译器线程
- 周期性任务线程
- 信号分发线程
- GC线程

### 对象的结构

对象由三部分组成，对象头，对象实例，对齐填充。
其中对象头一般是十六个字节，包括两部分，第一部分有哈希码，锁状态标志，线程持有的锁，偏向线程id，gc分代年龄等。第二部分是类型指针，也就是对象指向它的类元数据指针，可以理解，对象指向它的类。
对象实例就是对象存储的真正有效信息，也是程序中定义各种类型的字段包括父类继承的和子类定义的，这部分的存储顺序会被虚拟机和代码中定义的顺序影响（这里问一下，这个被虚拟机影响是不是就是重排序？？如果是的话，我知道的volatile定义的变量不会被重排序应该就是这里不会受虚拟机影响吧？？）。
第三部分对齐填充只是一个类似占位符的作用，因为内存的使用都会被填充为八字节的倍数。

## GC

### GC、GCRoots、stop-the-world概念

GC做三件事

- 分配内存
- 确保在使用的对象的内存还在
- 释放不再使用对象的空间

GC Roots引用的对象成为**活的**，不再被引用的就是**死的**

#### GC Roots：

- 当前各线程执行方法局部变量引用的对象
- 被加载类static域引用的对象
- 方法区中常量引用的对象
- JNI引用

GC管理内存成为**堆**

当 stop-the-world 垃圾收集器工作的时候，应用将完全被挂起

stop-the-world 垃圾收集器比并发收集器简单很多，因为应用挂起后**堆空间不再发生变化**，它的缺点是在某些场景下挂起的时间我们是不能接受的（如 web 应用）

#### GC类型（major、minor）

当年轻代被填满后，会进行一次年轻代垃圾收集（也叫做 **minor GC**）。

当老年代或永久代被填满了，会触发 **full GC**（也叫做 **major GC**），full GC 会收集所有区域

### 垃圾收集器

包括**串行收集器**、**并行收集器**、**并行压缩收集器** 和 **CMS 垃圾收集器**

#### 串行收集器

使用串行收集器，年轻代和老年代都使用单线程进行收集（使用一个 CPU），收集过程中会 stop-the-world

年轻代分为**一个 Eden 区和两个 Survivor 区（From 区和 To 区）**

#### 并行收集器

现在大多数 Java 应用都运行在大内存、多核环境中，**并行收集器**，也就是大家熟知的**吞吐量收集器**，利用多核的优势来进行垃圾收集，而不是像串行收集器一样将程序挂起后只使用单线程来收集垃圾。

#### Concurrent Mark-Sweep（CMS）收集器

**在年轻代中使用 CMS 收集器**

在年轻代中，CMS 和 **并行收集器** 一样，即：**并行、stop-the-world、复制**。

**在老年代中使用 CMS 收集器**

在老年代的垃圾收集过程中，大部分收集任务是和应用程序**并发**执行的。

**何时使用 CMS 收集器**

适用于应用程序要求低停顿，同时能接受在垃圾收集阶段和垃圾收集线程一起共享 CPU 资源的场景，典型的就是 web 应用了。

#### 小结

串行收集器：在年轻代和老年代都采用单线程，年轻代中使用 **stop-the-world、复制** 算法；老年代使用 **stop-the-world、标记 -> 清理 -> 压缩** 算法。

并行收集器：在年轻代中使用 **并行、stop-the-world、复制** 算法；老年代使用串行收集器的 **串行、stop-the-world、标记 -> 清理 -> 压缩** 算法。

并行压缩收集器：在年轻代中使用并行收集器的 **并行、stop-the-world、复制** 算法；老年代使用 **并行、stop-the-world、标记 -> 清理 -> 压缩** 算法。和并行收集器的区别是老年代使用了并行。

CMS 收集器：在年轻使用并行收集器的 **并行、stop-the-world、复制** 算法；老年代使用 **并发、标记 -> 清理** 算法，不压缩。本文介绍的唯一一个并发收集器，也是唯一一个不对老年代进行压缩的收集器。

### G1

G1 的主要关注点在于**达到可控的停顿时间**

CMS的区别在于，G1 没有 CMS 的碎片化问题（或者说不那么严重），同时提供了更加可控的停顿时间

#### G1 总览

 G1 将整个堆划分为一个个大小相等的小块（每一块称为一个 region），每一块的内存是连续的。和分代算法一样，G1 中每个块也会充当 Eden、Survivor、Old 三种角色

G1 使用了**停顿预测模型**来满足用户指定的停顿时间目标

#### G1 工作流程

 G1 的收集过程，G1 收集器主要包括了以下 4 种操作：

- 1、年轻代收集
- 2、并发收集，和应用线程同时执行
- 3、混合式垃圾收集
- 4、必要时的 Full GC

#### G1 参数配置和最佳实践

G1 调优的目标是尽量避免出现 Full GC，其实就是给老年代足够的空间，或相对更多的空间

有以下几点我们可以进行调整的方向：

- 增加堆大小，或调整老年代和年轻代的比例，这个很好理解
- 增加并发周期的线程数量，其实就是为了加快并发周期快点结束
- 让并发周期尽早开始，这个是通过设置堆使用占比来调整的（默认 45%）
- 在混合垃圾回收周期中回收更多的老年代区块

通过设置 -XX:MaxGCPauseMillis=N 来指定停顿时间（单位 ms，默认 200ms），如果没有达到这个目标，G1 会通过各种方式来补救：调整年轻代和老年代的比例，调整堆大小，调整晋升的年龄阈值，调整混合垃圾回收周期中处理的老年代的区块数量等等

##### 参数介绍

- **-XX:+UseG1GC**

  使用 G1 收集器

- **-XX:MaxGCPauseMillis=200**

  指定目标停顿时间，默认值 200 毫秒。

- **-XX:InitiatingHeapOccupancyPercent=45**

  整堆使用达到这个比例后，触发并发 GC 周期，默认 45%。

- **-XX:NewRatio=n**

  老年代/年轻代，默认值 2，即 1/3 的年轻代，2/3 的老年代

- **-XX:SurvivorRatio=n**

  Eden/Survivor，默认值 8，这个和其他分代收集器是一样的

- **-XX:MaxTenuringThreshold =n**

  从年轻代晋升到老年代的年龄阈值，也是和其他分代收集器一样的

- **-XX:ParallelGCThreads=n**

  并行收集时候的垃圾收集线程数

- **-XX:ConcGCThreads=n**

  并发标记阶段的垃圾收集线程数

- **-XX:G1ReservePercent=n**

  堆内存的预留空间百分比，默认 10，用于降低晋升失败的风险，即默认地会将 10% 的堆内存预留下来。

- **-XX:G1HeapRegionSize=n**

  每一个 region 的大小，默认值为根据堆大小计算出来，取值 1MB~32MB，这个我们通常指定整堆大小就好了。

### Metaspace元空间























# 4. 多线程

## AQS

AbstractQueuedSynchronizer

用来构建同步器和锁的框架

主要做了三件事情：

1. 管理 同步状态；
2. 维护 同步队列；
3. 阻塞和唤醒 线程。

从行为上来区分就是 获取锁 和 释放锁，从共享资源的访问方式上来区分就是 独占锁 和 共享锁

**独占锁**

独占式(Exclusive): **同一时间只有一个线程可以访问共享资源,也就是独占锁。** 如:Synchronized,ReentrantLock。 **对于独占式锁的实现,在AQS中对应tryAcquire获取锁和tryRelease释放锁。**

**共享锁**

共享式(Share): **同一时间允许多个线程同时访问共享资源,也就是共享锁。** CountDownLatch,Semaphore,ReentrantReadWriteLock的ReadLock都是共享锁。 **对于共享式锁的实现,在AQS中对应tryAcquireShare获取锁和tryReleaseShare释放锁。**

### 实现原理

AQS 内部维护了一个 FIFO 队列来管理锁。线程首先会尝试获取锁，如果失败，则将当前线程以及等待状态等信息包成一个 Node 节点放入同步队列阻塞起来，当持有锁的线程释放锁时，就会唤醒队列中的后继线程

AQS的核心思想是如果被请求的资源空闲，那么就将当前请求资源的线程设置为有效的工作线程; 如果请求的资源被其他线程所占有， 那么就使用CLH线程阻塞队列来提供阻塞线程并唤线程分配资源的机制。 在CLH队列中，每个请求资源的线程都会被封装成队列中的一个节点。

在AQS内部有一个int类型的state表示线程同步状态， 当线程lock获取到锁后，该state计数就加1,unlock就减1， 这就是为什么解锁次数要对应加锁次数的原因。

AQS主要实现技术为:CLH队列(Craig,Landin and Hagersten)， 自旋CAS，park(阻塞线程)以及unparkSuccessor(唤醒阻塞队列中的后继线程)。

**AQS中的锁计数指的是 state 变量**

### 数据结构

内部采用了volatile变量state来作为资源的标识

本身实现的是一些排队和阻塞机制，它内部是一个先进先出的双端队列，使用了两个指针：head、tail

![img](http://concurrent.redspider.group/article/02/imgs/AQS%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.png)

但他它本身不是直接存储线程，而是存储拥有线程的节点Node

### 主要方法

AQS的设计是基于**模板方法模式**的，它有一些方法必须要子类去实现的，它们主要有：

- isHeldExclusively()：该线程是否正在独占资源。只有用到condition才需要去实现它。
- tryAcquire(int)：独占方式。尝试获取资源，成功则返回true，失败则返回false。
- tryRelease(int)：独占方式。尝试释放资源，成功则返回true，失败则返回false。
- tryAcquireShared(int)：共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
- tryReleaseShared(int)：共享方式。尝试释放资源，如果释放后允许唤醒后续等待结点返回true，否则返回false。

### 获取资源

tryAcquire(int arg)

arg独占模式为1

如果获取资源失败，则通过addWaiter(Node.EXCLUSIVE)方法把这个线程插入到等待队列中。其中传入的参数代表要插入的Node是独占式的

流程图：

![acquire流程](http://concurrent.redspider.group/article/02/imgs/acquire%E6%B5%81%E7%A8%8B.jpg)



### 生命周期

new,runnable,running,blocked,dead

- 新建状态：新建线程对象，并没有调用start()方法之前
- 就绪状态：调用start()方法之后线程就进入就绪状态，但是并不是说只要调用start()方法线程就马上变为当前线程，在变为当前线程之前都是为就绪状态。值得一提的是，线程在睡眠和挂起中恢复的时候也会进入就绪状态哦。
- 运行状态：线程被设置为当前线程，开始执行run()方法。就是线程进入运行状态
- 阻塞状态：线程被暂停，比如说调用sleep()方法后线程就进入阻塞状态
- 死亡状态：线程执行结束

### CountDownLatch

> CountDownLatch允许count个线程阻塞在一个地方，直至所有线程的任务都执行完毕。

CountDownLatch是共享锁的一种实现,**它默认构造AQS的state为count。 当线程使用countDown方法时,其实使用了tryReleaseShared方法以CAS的操作来减少state, 直至state为0就代表所有的线程都调用了countDown方法。** 假如某线程A调用await方法时，如果state不为0，就代表还有线程未执行countDown方法， 那么就把线程A放入阻塞队列Park，并自旋CAS判断state == 0。 直至最后一个线程调用了countDown，使得state == 0， 于是阻塞的线程判断成功，并被唤醒，就继续往下执行。

用法：

```java
 private void assemblyActivityTag(CartItemDTO itemDTO){            
   //1.查询立减活动信息，耗时1s
   //2.查询阶梯满减活动信息，耗时1s
   //3.查询团购活动信息，耗时1s
   //4.组装活动标签信息，耗时1s    
   // 串行执行下来整个耗时4s
 }
// 使用CountDownLatch：
  private void assemblyActivityTag(CartItemDTO itemDTO){
        ExecutorService executorService = Executors.newCachedThreadPool();
        CountDownLatch latch = new CountDownLatch(3);
        executorService.execute(new Runnable() {
            @Override
            public void run() {
            //1.查询立减活动信息
                latch.countDown();
            }
        });
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                //2.查询阶梯满减活动信息
                latch.countDown();
            }
        });
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                //3.查询团购活动信息
                latch.countDown();
            }
        });
        try {
            // 一定记得加上timeout时间，防止阻塞主线程
            latch.await(3000,TimeUnit.MILLISECONDS);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //4.等待所有子任务完成，组装活动标签信息
        //5.关闭线程池
        executorService.shutdown();
    }
```



### Semaphore

> Semaphore允许一次性最多(不是同时)permits个线程执行任务。

Semaphore与CountDownLatch一样，也是共享锁的一种实现。 **它默认构造AQS的state为permits。 当执行任务的线程数量超出permits,那么多余的线程将会被放入阻塞队列Park,并自旋判断state是否大于0。 只有当state大于0的时候，阻塞的线程才有机会继续执行,此时先前执行任务的线程继续执行release方法， release方法使得state的变量会加1，那么自旋的线程便会判断成功。**

如此，**每次只有不超过permits个的线程能自旋成功，便限制了执行任务线程的数量。** 所以这也是我为什么说它可能不是permits个线程同时执行， 因为只要state>0,线程就有机会执行.

### CycliBarrier

CycliBarrier的功能与CountDownLatch相似，但是**CountDownLatch的实现是基于AQS的， 而CycliBarrier是基于ReentrantLock(ReentrantLock也属于AQS同步器)和Condition的。**

CountDownLatch虽然可以令多个线程阻塞在同一代码处，但只能await一次就不能使用了。 而CycliBarrier有Generation代的概念，一个代，就代表CycliBarrier的一个循环， 这也是CycliBarrier支持重复await的原因。

## synchronized

java6之前的synchronized属于重量锁,性能较差, 它是基于操作系统的Mutex Lock互斥量实现的

Synchronized是通过对象内部的一个叫做监视器锁（monitor）来实现的，监视器锁本质又是依赖于底层的操作系统的 Mutex Lock（互斥锁）来实现的。而操作系统实现线程之间的切换需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么 Synchronized 效率低的原因。因此，这种依赖于操作系统 Mutex Lock 所实现的锁我们称之为重量级锁。

随着锁的竞争，锁可以从偏向锁升级到轻量级锁，再升级的重量级锁（但是锁的升级是单向的，也就是说只能从低到高升级，不会出现锁的降级）。JDK 1.6中默认是开启偏向锁和轻量级锁的，我们也可以通过-XX:-UseBiasedLocking=false来禁用偏向锁。

- Synchronized修饰普通同步方法：锁对象当前实例对象；
- Synchronized修饰静态同步方法：锁对象是当前的类Class对象；
- Synchronized修饰同步代码块：锁对象是Synchronized后面括号里配置的对象，这个对象可以是某个对象（xlock），也可以是某个类（Xlock.class）；



**代码块的同步**通过在字节码前后添加 monitorenter 和 monitorexit 指令实现: monitorenter :每个对象都是⼀一个监视器器锁(monitor)。当monitor被占⽤用时就会处于锁定状态，

线程执⾏行行monitorenter指令时尝试获取monitor的所有权，过程如下:

1. 如果monitor的进⼊入数为0，则该线程进⼊入monitor，然后将进⼊入数设置为1，该线程即为monitor的所 有者;

2. 如果线程已经占有该monitor，只是重新进⼊入，则进⼊入monitor的进⼊入数加1;
3. 如果其他线程已经占⽤用了了monitor，则该线程进⼊入阻塞状态，直到monitor的进⼊入数为0，再重新尝试获取monitor的所有权;

**monitorexit** :执行monitorexit的线程必须是objectref所对应的monitor的所有者。指令执⾏行行时， monitor的进入数减1，如果减1后进⼊入数为0，那线程退出monitor，不再是这个monitor的所有者 。其他 被这个monitor阻塞的线程可以尝试去获取这个 monitor 的所有权。

**方法的同步**并没有通过指令 monitorenter 和 monitorexit 来完成(理理论上其实也可以通过这 两条指令来实现)，不不过相对于普通⽅方法，其常量量池中多了了 ACC_SYNCHRONIZED 标示符。 JVM就是 根据该标示符来实现⽅方法的同步的 :

> 当⽅方法调⽤用时，调⽤用指令将会检查⽅方法的 ACC_SYNCHRONIZED 访问标志是否被设置，如果设置 了了， 执⾏行行线程将先获取monitor ，获取成功之后才能执⾏行行⽅方法体， ⽅方法执⾏行行完后再释放monitor 。 在⽅方法执⾏行行期间，其他任何线程都⽆无法再获得同⼀一个monitor对象。

两种同步⽅方式本质上没有区别，只是⽅方法的同步是⼀一种隐式的⽅方式来实现，⽆无需通过字节码来完成。 两 个指令的执⾏行行是JVM通过调⽤用操作系统的互斥原语mutex来实现，被阻塞的线程会被挂起、等待重新调 度 ，会导致“⽤用户态和内核态”两个态之间来回切换，对性能有较⼤大影响。

### 锁升级过程

- 无锁：所有的线程都能访问并修改同一个资源，但同时只有一个线程能修改成功。
- 偏向锁：初次执行到synchronized代码块的时候，锁对象变成偏向锁（通过CAS修改对象头里的锁标志位），执行完同步代码块后，线程并不会主动释放偏向锁。当第二次到达同步代码块时，线程会判断此时持有锁的线程是否就是自己，如果是则正常往下执行。由于之前没有释放锁，这里也就不需要重新加锁。如果自始至终使用锁的线程只有一个，很明显偏向锁几乎没有额外开销，性能极高。偏向锁只有遇到其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁，线程是不会主动释放偏向锁的
- 轻量级锁：轻量级锁是指当锁是偏向锁的时候，却被另外的线程所访问，此时偏向锁就会升级为轻量级锁，其他线程会通过自旋（关于自旋的介绍见文末）的形式尝试获取锁，线程不会阻塞。

  轻量级锁的获取主要由两种情况：
  ① 当关闭偏向锁功能时；
  ② 由于多个线程竞争偏向锁导致偏向锁升级为轻量级锁。

> 锁竞争：如果多个线程轮流获取一个锁，但是每次获取锁的时候都很顺利，没有发生阻塞，那么就不存在锁竞争。只有当某线程尝试获取锁的时候，发现该锁已经被占用，只能等待其释放，这才发生了锁竞争。

如果多个线程用一个锁，但是没有发生锁竞争，或者发生了很轻微的锁竞争，那么synchronized就用轻量级锁，允许短时间的忙等现象。这是一种折衷的想法，短时间的忙等，换取线程在用户态和内核态之间切换的开销

- 重量级锁：显然，此忙等是有限度的（有个计数器记录自旋次数，默认允许循环10次，可以通过虚拟机参数更改）。如果锁竞争情况严重，某个达到最大自旋次数的线程，会将轻量级锁升级为重量级锁（依然是CAS修改锁标志位，但不修改持有锁的线程ID）。当后续线程尝试获取锁时，发现被占用的锁是重量级锁，则直接将自己挂起（而不是忙等），等待将来被唤醒。

  重量级锁是指当有一个线程获取锁之后，其余所有等待获取该锁的线程都会处于阻塞状态。

  简言之，就是所有的控制权都交给了操作系统，由操作系统来负责线程间的调度和线程的状态变更。而这样会出现频繁地对线程运行状态的切换，线程的挂起和唤醒，从而消耗大量的系统资

能进行锁升级（从低级别到高级别），不能锁降级（高级别到低级别）

![image-20210607004534023](/Users/cuweir/Library/Application Support/typora-user-images/image-20210607004534023.png)

synchronized 用的锁是存在Java对象头里的，Hopspot 对象头主要包括两部分数据：Mark Word（标记字段） 和 Klass Pointer（类型指针）。存在锁对象的对象头的Mark Word中

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210607004747597.png" alt="image-20210607004747597" style="zoom:80%;" />

Monitor 可以理解为一个同步工具或一种同步机制，通常被描述为一个对象。每一个 Java 对象就有一把看不见的锁，称为内部锁或者 Monitor 锁。







## ThreadLocal

ThreadLocal 类 提供了 get/set 线程局部变量的实现，ThreadLocal 成员变量与正常的成员变量不同，每个线程都可以通过 ThreadLocal 成员变量 get/set 自己的专属值。

ThreadLocal 实例 通常是类中的私有静态变量，常用于将状态与线程关联，例如：用户 ID 或事务 ID。

每个Thread对象都持有一个 ThreadLocalMap类型的成员变量，其key为ThreadLocal对象，value为绑定的值，所以每个线程调用 ThreadLocal对象 的set(T value)方法时，都会将该ThreadLocal对象和绑定的值以键值对的形式存入当前线程，这样，同一个ThreadLocal对象就可以为每个线程绑定一个专属值咯。
每个线程调用 ThreadLocal对象的get()方法时，就可以根据 当前ThreadLocal对象 get到 绑定的值。

与大部分 Map 的实现相同，底层也是使用 动态数组来保存 键值对Entry，也有rehash、resize等
 操作

存储键值对，key 为 ThreadLocal对象，value 为 与该ThreadLocal对象绑定的值

Entry的key是对ThreadLocal的弱引用，当抛弃掉ThreadLocal对象时，垃圾收集器会
个key的引用而清理掉ThreadLocal对象，防止了内存泄漏

最后强调一下 ThreadLocal 的使用注意事项：

1. ThreadLocal 不是用来解决线程安全问题的，多线程不共享，不存在竞争！其目的是使线程能够使用本地变量。
2. 项目如果使用了线程池，那么线程回收后 ThreadLocal 变量要 remove 掉，否则线程池回收线程后，变量还在内存中，可能会带来意想不到的后果！例如 Tomcat 容器的线程池，可以在拦截器中处理：继承 HandlerInterceptorAdapter，然后复写 afterCompletion()方法，remove 掉变量！！！

## volatile

**保证内存的可见性**

 jvm的内存模型是: **线程总是从主内存读取变量到工作内存，然后在工作内存中进行修改， 在修改完变量后，才把变量同步到主内存中**。解决这个问题的办法就是给变量加上volatile关键字修饰。 volatile使得被它修饰的变量在被线程修改后，那么线程就需要把修改后的变量重新同步到主内存， 且其他线程每次使用这个变量，都需要从主内存读取。

将变量的更更新直接写回主存，并使其他线程⼯工作内存中的值失效

**禁止指令重排序**

volatile通过提供内存屏障来防止指令重排序。 java内存模型会在每个volatile写操作前后都会插入store指令，将工作内存中的变量同步回主内存。 在每个volatile读操作前后都会插入load指令，从主内存中读取变量。

普通的变量量仅仅会保证在该⽅方法的执⾏行行过程中所有依赖赋值结果的地⽅方都能获取到正确的结果，⽽而

无法保证复制操作顺序和代码中顺序⼀一致。 Within-Thread As-If-Serial Semantic 编译后，有volatile的变量量在赋值后多执⾏行行了了⼀一个 lock add1 $F0x0, (%esp) 操作

lock前缀 (StoreBarrier)

1. 使得本CPU的cache写⼊入内存，相当于store+write，从⽽而使得对其他线程可⻅见;

2. 写⼊入内存要求指令重排序不不影响结果的正确性，意味着所有之前的操作都已经执⾏行行完成

**happens before**

Happens-before原则:由JMM(Java Memory Model)⾃自动实现的天然有序性保证

A和B满⾜足Happens-before规则并不不代表A⼀一定在B于时间上先⾏行行发⽣生。只要A->B和B->A的执⾏行行结果相 同，JMM就认可该种重排序。

程序顺序规则: ⼀一个线程中的每个操作，happens-before于该线程中的任意后续操作 监视器器锁规则 :对⼀一个线程的解锁，happens-before于随后对这个线程的加锁

volatile变量量规则: 对⼀一个volatile域的写，happens-before于后续对这个volatile域的读(时间上 的先后!!! )

传递性:如果A happens-before B ,且 B happens-before C, 那么 A happens-before C start()规则: 如果线程A执⾏行行操作 ThreadB_start() (启动线程B) , 那么A线程的

ThreadB_start() happens-before 于B中的任意操作
 join()原则: 如果A执⾏行行 ThreadB.join() 并且成功返回，那么线程B中的任意操作happens-

before于线程A从 ThreadB.join() 操作成功返回。
 interrupt()原则: 对线程 interrupt() ⽅方法的调⽤用先⾏行行发⽣生于被中断线程代码检测到中断

事件的发⽣生，可以通过 Thread.interrupted() ⽅方法检测是否有中断发⽣生 finalize()原则:⼀一个对象的初始化完成先⾏行行发⽣生于它的 finalize() ⽅方法的开始

## CAS

比较成功并交换，乐观锁。将指定内存地址的值与期盼值相比较，如果比较成功就将内存地址的值修改为目标值。

**Atomic原子类**

并且有个重要的细节就是,Atomic原子类内部的value值都是由volatile修饰的, 这就使得Atomic的value值是对其他线程可见的。

### 缺点

1.ABA：

问题描述：线程t1将它的值从A变为B，再从B变为A。同时有线程t2要将值从A变为C。但CAS检查的时候会发现没有改变，但是实质上它已经发生了改变 。可能会造成数据的缺失。

解决方法：CAS还是类似于乐观锁，同数据乐观锁的方式给它加一个版本号或者时间戳，如AtomicStampedReference

2.自旋消耗资源：

问题描述：多个线程争夺同一个资源时，如果自旋一直不成功，将会一直占用CPU。

解决方法：破坏掉for死循环，当超过一定时间或者一定次数时，return退出。JDK8新增的LongAddr,和ConcurrentHashMap类似的方法。当多个线程竞争时，将粒度变小，将一个变量拆分为多个变量，达到多个线程访问多个资源的效果，最后再调用sum把它合起来。

虽然base和cells都是volatile修饰的，但感觉这个sum操作没有加锁，可能sum的结果不是那么精确。

2.多变量共享一致性问题：

解决方法： CAS操作是针对一个变量的，如果对多个变量操作，

1) 可以加锁来解决。

2) 封装成对象类解决。



## 死锁

必要条件：

1. 互斥条件：进程要求对所分配的资源进行排它性控制，即在一段时间内某资源仅为一进程所占用。
2. 请求和保持条件：当进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件：进程已获得的资源在未使用完之前，不能剥夺，只能在使用完时由自己释放。
4. 环路等待条件：在发生死锁时，必然存在一个进程--资源的环形链。

预防：

资源一次性分配：一次性分配所有资源，这样就不会再有请求了：（破坏请求条件）
只要有一个资源得不到分配，也不给这个进程分配其他的资源：（破坏请保持条件）
可剥夺资源：即当某进程获得了部分资源，但得不到其它资源，则释放已占有的资源（破坏不可剥夺条件）
资源有序分配法：系统给每类资源赋予一个编号，每一个进程按编号递增的顺序请求资源，释放则相反（破坏环路等待条件）

银行家算法：首先需要定义状态和安全状态的概念。系统的状态是当前给进程分配的资源情况。因此，状态包含两个向量Resource（系统中每种资源的总量）和Available（未分配给进程的每种资源的总量）及两个矩阵Claim（表示进程对资源的需求）和Allocation（表示当前分配给进程的资源）。安全状态是指至少有一个资源分配序列不会导致死锁。当进程请求一组资源时，假设同意该请求，从而改变了系统的状态，然后确定其结果是否还处于安全状态。如果是，同意这个请求；如果不是，阻塞该进程知道同意该请求后系统状态仍然是安全的。



# 5. 线程池

使用线程池有三个原因：

1. 复用已创建线程
2. 控制并发数量
3. 对线程做统一管理

顶层Executor接口，实现类是：ThreadPoolExecutor

先简单说说这个继承结构，Executor 位于最顶层，也是最简单的，就一个 execute(Runnable runnable) 接口方法定义。

ExecutorService 也是接口，在 Executor 接口的基础上添加了很多的接口方法，所以**一般来说我们会使用这个接口**。

然后再下来一层是 AbstractExecutorService，从名字我们就知道，这是抽象类，这里实现了非常有用的一些方法供子类直接使用，之后我们再细说。

然后才到我们的重点部分 ThreadPoolExecutor 类，这个类提供了关于线程池所需的非常丰富的功能。

## 原理

### 构造方法

参数：

**corePoolSize：**核心线程数最大值

核心线程默认情况下会一直存在于线程池中，即使这个核心线程什么都不干（铁饭碗），而非核心线程如果长时间的闲置，就会被销毁（临时工）。

**maximumPoolSize：**

线程总数最大值

**keepAliveTime：**

非核心线程闲置超时时长，超过该时长会被销毁。可以通过调用 `allowCoreThreadTimeOut(true)`使核心线程数内的线程也可以被回收

**unit：**

keepAliveTime单位

**BlockingQueue workQueue：**

阻塞队列，维护着**等待执行的Runnable任务对象**。

常用的几个阻塞队列：

1. **LinkedBlockingQueue**

   链式阻塞队列，底层数据结构是链表，默认大小是`Integer.MAX_VALUE`，也可以指定大小。

2. **ArrayBlockingQueue**

   数组阻塞队列，底层数据结构是数组，需要指定队列的大小。是 BlockingQueue 接口的有界队列实现类。其并发控制采用可重入锁来控制，不管是插入操作还是读取操作，都需要获取到锁才能进行操作。它采用一个 ReentrantLock 和相应的两个 Condition 来实现。

3. **SynchronousQueue**

   同步队列，内部容量为0，每个put操作必须等待一个take操作，反之亦然。

4. **DelayQueue**

   延迟队列，该队列中的元素只有当其指定的延迟时间到了，才能够从队列中获取到该元素 。

非必须参数：

**ThreadFactory threadFactory：**

创建线程的工厂，用于批量创建线程，统一在创建线程时设置一些参数，如是否守护线程、线程的优先级等。如果不指定，会新建一个默认的线程工厂。

**RejectedExecutionHandler handler：**

拒绝处理策略，线程数量大于最大线程数就会采用拒绝处理策略，四种拒绝处理的策略为 ：

1. **ThreadPoolExecutor.AbortPolicy**：**默认拒绝处理策略**，丢弃任务并抛出RejectedExecutionException异常。
2. **ThreadPoolExecutor.DiscardPolicy**：丢弃新来的任务，但是不抛出异常。
3. **ThreadPoolExecutor.DiscardOldestPolicy**：丢弃队列头部（最旧的）的任务，然后重新尝试执行程序（如果再次失败，重复此过程）。
4. **ThreadPoolExecutor.CallerRunsPolicy**：由调用线程处理该任务。

### 阻塞队列

 BlockingQueue 支持当获取队列元素但是队列为空时，会阻塞等待队列中有元素再返回；也支持添加元素时，如果队列已满，那么等到队列可以放入新元素时再放入。
BlockingQueue 是一个接口，继承自 Queue，所以其实现类也可以作为 Queue 的实现来使用，而 Queue 又继承自 Collection 接口。
BlockingQueue 对插入操作、移除操作、获取元素操作提供了四种不同的方法用于不同的场景中使用：

1、抛出异常；2、返回特殊值（null 或 true/false，取决于具体的操作）；3、阻塞等待此操作，直到这个操作成功；4、阻塞等待此操作，直到成功或者超时指定时间。

BlockingQueue 是设计用来实现生产者-消费者队列的

BlockingQueue 的实现都是线程安全的，但是批量的集合操作如 addAll, containsAll, retainAll 和 removeAll  不一定是原子操作。如 addAll(c) 有可能在添加了一些元素后中途抛出异常，此时 BlockingQueue 中已经添加了部分元素，这个是允许的，取决于具体的实现。

**ArrayBlockingQueue**

是 BlockingQueue 接口的有界队列实现类。其并发控制采用可重入锁来控制，不管是插入操作还是读取操作，都需要获取到锁才能进行操作。它采用一个 ReentrantLock 和相应的两个 Condition 来实现。Condition 经常可以用在生产者-消费者的场景中

![](/Users/cuweir/Library/Application Support/typora-user-images/image-20210609155029005.png)

ArrayBlockingQueue 实现并发同步的原理就是，读操作和写操作都需要获取到 AQS 独占锁才能进行操作。如果队列为空，这个时候读操作的线程进入到读线程队列排队，等待写线程写入新的元素，然后唤醒读线程队列的第一个等待线程。如果队列已满，这个时候写操作的线程进入到写线程队列排队，等待读线程将队列元素移除腾出空间，然后唤醒写线程队列的第一个等待线程。

对于 ArrayBlockingQueue，我们可以在构造的时候指定以下三个参数：
队列容量，其限制了队列中最多允许的元素个数；指定独占锁是公平锁还是非公平锁。非公平锁的吞吐量比较高，公平锁可以保证每次都是等待最久的线程获取到锁；可以指定用一个集合来初始化，将此集合中的元素在构造方法期间就先添加到队列中。

**LinkedBlockingQueue**

底层基于单向链表实现的阻塞队列，可以当做无界队列也可以当做有界队列来使用

![image-20210609160111179](/Users/cuweir/Library/Application Support/typora-user-images/image-20210609160111179.png)

**SynchronousQueue**
它是一个特殊的队列，它的名字其实就蕴含了它的特征 - - 同步的队列....这里说的并不是多线程的并发问题，而是因为当一个线程往队列中写入一个元素时，写入操作不会立即返回，需要等待另一个线程来将这个元素拿走；同理，当一个读线程做读操作的时候，同样需要一个相匹配的写线程的写操作。这里的 Synchronous 指的就是读线程和写线程需要同步，一个读线程匹配一个写线程。

**PriorityBlockingQueue**
带排序的 BlockingQueue 实现，其并发控制采用的是 ReentrantLock，队列为无界队列（ArrayBlockingQueue 是有界队列，LinkedBlockingQueue 也可以通过在构造函数中传入 capacity 指定队列最大的容量，但是 PriorityBlockingQueue 只能指定初始的队列大小，后面插入元素的时候，如果空间不够的话会自动扩容）。
简单地说，它就是 PriorityQueue 的线程安全版本。不可以插入 null 值，同时，插入队列的对象必须是可比较大小的（comparable），否则报 ClassCastException 异常。它的插入操作 put 方法不会 block，因为它是无界队列（take 方法在队列为空的时候会阻塞）。

### ThreadPoolExcecutor策略

定义了变量runState来表示线程池状态：

- 线程池创建后处于**RUNNING**状态。

- 调用shutdown()方法后处于**SHUTDOWN**状态，线程池不能接受新的任务，清除一些空闲worker,会等待阻塞队列的任务完成。

- 调用shutdownNow()方法后处于**STOP**状态，线程池不能接受新的任务，中断所有线程，阻塞队列中没有被执行的任务全部丢弃。此时，poolsize=0,阻塞队列的size也为0。

- 当所有的任务已终止，ctl记录的”任务数量”为0，线程池会变为**TIDYING**状态。接着会执行terminated()函数。

  > ThreadPoolExecutor中有一个控制状态的属性叫ctl，它是一个AtomicInteger类型的变量。

- 线程池处在TIDYING状态时，**执行完terminated()方法之后**，就会由 **TIDYING -> TERMINATED**， 线程池被设置为TERMINATED状态。

### 任务处理流程

核心方法是execute

![image-20210609155608047](/Users/cuweir/Library/Application Support/typora-user-images/image-20210609155608047.png)

### 如何线程复用

工作线程worker，工作线程组

首先去执行创建这个worker时就有的任务，当执行完这个任务后，worker的生命周期并没有结束，在`while`循环中，worker会不断地调用`getTask`方法从**阻塞队列**中获取任务然后调用`task.run()`执行任务,从而达到**复用线程**的目的。只要`getTask`方法不返回`null`,此线程就不会退出。

### 怎么设置大小

maxPoolSize

**CPU密集型任务**：即需要大量计算的任务，因为计算是由CPU来完成的，所以称之为CPU密集型任务。这个时候设置为**CPU核心数+1**就可以了。防止由于其他因素导致线程阻塞。

**IO密集型任务**：即需要通过网络来交互的任务，通常是指数据库数据交互、文件上传下载、网络数据传输等任务。这个时候需要设置为**CPU核心数\*2。**但是实际情况中可以通过自己的测试来设置合理的数值，通过与大牛的交流得知，这个数值可以设置为**CPU核心数/（1-阻塞系数），**阻塞系数一般在**0.8~0.9**之间。比如8核CPU可以设置为：8*（1-0.9）=80。

## 常见线程池

### newCachedThreadPool

1. 提交任务进线程池。
2. 因为**corePoolSize**为0的关系，不创建核心线程，线程池最大为Integer.MAX_VALUE。
3. 尝试将任务添加到**SynchronousQueue**队列。
4. 如果SynchronousQueue入列成功，等待被当前运行的线程空闲后拉取执行。如果当前没有空闲线程，那么就创建一个非核心线程，然后从SynchronousQueue拉取任务并在当前线程执行。
5. 如果SynchronousQueue已有任务在等待，入列操作将会阻塞。

当需要执行很多**短时间**的任务时，CacheThreadPool的线程复用率比较高， 会显著的**提高性能**。而且线程60s后会回收，意味着即使没有任务进来，CacheThreadPool并不会占用很多资源。

### newFixedThreadPool

**与CachedThreadPool的区别**：

- 因为 corePoolSize == maximumPoolSize ，所以FixedThreadPool只会创建核心线程。 而CachedThreadPool因为corePoolSize=0，所以只会创建非核心线程。
- 在 getTask() 方法，如果队列里没有任务可取，线程会一直阻塞在 LinkedBlockingQueue.take() ，线程不会被回收。 CachedThreadPool会在60s后收回。
- 由于线程不会被回收，会一直卡在阻塞，所以**没有任务的情况下， FixedThreadPool占用资源更多**。
- 都几乎不会触发拒绝策略，但是原理不同。FixedThreadPool是因为阻塞队列可以很大（最大为Integer最大值），故几乎不会触发拒绝策略；CachedThreadPool是因为线程池很大（最大为Integer最大值），几乎不会导致线程数量大于最大线程数，故几乎不会触发拒绝策略。

### newSingleThreadExecutor

有且仅有一个核心线程（ corePoolSize == maximumPoolSize=1），使用了LinkedBlockingQueue（容量很大），所以，**不会创建非核心线程**。所有任务按照**先来先执行**的顺序执行。如果这个唯一的线程不空闲，那么新来的任务会存储在任务队列里等待执行。

### newScheduledThreadPool

创建一个定长线程池，支持定时及周期性任务执行。

## 我怎么用的

```java
jiraEventExecutor.setCorePoolSize(2);
jiraEventExecutor.setMaxPoolSize(20);
jiraEventExecutor.setQueueCapacity(200);
jiraEventExecutor.setKeepAliveSeconds(60);
jiraEventExecutor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
jiraEventExecutor.initialize();
```

# 6. MySQL

## 索引

### 聚簇索引和非聚簇索引

```mysql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
    [USING index_type]
    ON tbl_name (index_col_name,...)
 
index_col_name:
    col_name [(length)] [ASC | DESC]
```

创建的索引，如复合索引、前缀索引、唯一索引，都是属于非聚簇索引，在有的书籍中，又将其称为辅助索引(secondary index)。在后文中，我们称其为**非聚簇索引**，其数据结构为B+树。

**聚簇索引**是按照每张表的主键来构造一颗B+树，叶子节点存放的就是整张表的行数据，因此一张表只能有一个聚簇索引。

在Innodb中，聚簇索引默认就是主键索引。

非聚簇索引的叶子节点不是真实数据，它的叶子节点依然是索引节点，存放的是该索引字段的值以及对应的主键索引(聚簇索引)。

多加一个索引，就会多生成一颗非聚簇索引树，做插入操作的时候，需要同时维护这几颗树的变化。因此，如果索引太多，插入性能就会下降。

#### **自增主键和uuid作为主键的区别么？**

由于主键使用了聚簇索引，如果主键是自增id，，那么对应的数据一定也是相邻地存放在磁盘上的，写入性能比较高。如果是uuid的形式，频繁的插入会使innodb频繁地移动磁盘块，写入性能就比较低了。

![index](/index.png)

### B树和B+树的关系

B树也称B-树,它是一颗多路平衡查找树。

- 每个节点最多有m-1个**关键字**（可以存有的键值对）。
- 根节点最少可以只有1个**关键字**。
- 非根节点至少有m/2个**关键字**。
- 每个节点中的关键字都按照从小到大的顺序排列，每个关键字的左子树中的所有关键字都小于它，而右子树中的所有关键字都大于它。
- 所有叶子节点都位于同一层，或者说根节点到每个叶子节点的长度都相同。
- 每个节点都存有索引和数据，也就是对应的key和value。

B+树其实和B树是非常相似的，我们首先看看**相同点**。

- 根节点至少一个元素
- 非根节点元素范围：m/2 <= k <= m-1

**不同点**

- B+树有两种类型的节点：内部结点（也称索引结点）和叶子结点。内部节点就是非叶子节点，内部节点不存储数据，只存储索引，数据都存储在叶子节点。
- 内部结点中的key都按照从小到大的顺序排列，对于内部结点中的一个key，左树中的所有key都小于它，右子树中的key都大于等于它。叶子结点中的记录也按照key的大小排列。
- 每个叶子结点都存有相邻叶子结点的指针，叶子结点本身依关键字的大小自小而大顺序链接。
- 父节点存有右孩子的第一个元素的索引。

**B+树优势**

- 单一节点存储的元素更多，使得查询的IO次数更少，所以也就使得它更适合做为数据库MySQL的底层数据结构了。
- 所有的查询都要查找到叶子节点，查询性能是稳定的，而B树，每个节点都可以查找到数据，所以不稳定。
- 所有的叶子节点形成了一个有序链表，更加便于查找。

### 索引创建原则

**选择唯一性索引**

唯一性索引的值是唯一的，可以更快速的通过该索引来确定某条记录。例如，学生表中学号是具有唯一性的字段。为该字段建立唯一性索引可以很快的确定某个学生的信息。

**为经常需要排序、分组和联合操作的字段建立索引**

经常需要ORDER BY、GROUP BY、DISTINCT和UNION等操作的字段，排序操作会浪费很多时间。如果为其建立索引，可以有效地避免排序操作。

**为常作为查询条件的字段建立索引**

如果某个字段经常用来做查询条件，那么该字段的查询速度会影响整个表的查询速度。因此，为这样的字段建立索引，可以提高整个表的查询速度。

**限制索引的数目**

索引的数目不是越多越好。每个索引都需要占用磁盘空间，索引越多，需要的磁盘空间就越大。修改表时，对索引的重构和更新很麻烦。越多的索引，会使更新表变得很浪费时间。

**尽量使用数据量少的索引**

如果索引的值很长，那么查询的速度会受到影响。例如，对一个CHAR(100)类型的字段进行全文检索需要的时间肯定要比对CHAR(10)类型的字段需要的时间要多。

**尽量使用前缀来索引**

如果索引字段的值很长，最好使用值的前缀来索引。例如，TEXT和BLOG类型的字段，进行全文检索会很浪费时间。如果只检索字段的前面的若干个字符，这样可以提高检索速度。

### 索引下推 index condition pushdown

在不不使⽤用ICP的情况下，在使⽤用 ⾮非主键索引(⼜又叫普通索引或者⼆二级索引) 进⾏行行查询时，存储引 擎通过索引检索到数据，然后返回给MySQL服务器器，服务器器然后判断数据是否符合条件 。 在使⽤用ICP的情况下，如果存在某些被索引的列列的判断条件时，MySQL服务器器将这⼀一部分判断条 件传递给存储引擎，然后由存储引擎通过判断索引是否符合MySQL服务器器传递的条件，只有当 索引符合条件时才会将数据检索出来返回给MySQL服务器器 。

![image-20210618110713745](/Users/cuweir/Library/Application Support/typora-user-images/image-20210618110713745.png)

## select加锁分析

### 锁类型

**共享锁**(S锁)、**排他锁**(X锁)、**意向共享锁**(IS锁)、**意向排他锁**(IX锁)

**意向锁的目的**：。假设事务T1，用X锁来锁住了表上的几条记录，那么此时表上存在IX锁，即意向排他锁。那么此时事务T2要进行`LOCK TABLE … WRITE`的表级别锁的请求，可以直接根据意向锁是否存在而判断是否有锁冲突。

### 加锁算法

Record Locks：行锁，对索引记录进行加锁。

Gap Locks：间隙锁，防止其他事务插入数据。在`Read Committed`隔离级别下，不会使用间隙锁。

Next-Key Locks：Record Lock + 索引前面的Gap Lock

### 快照读和当前读

快照读，读的是数据库记录的快照版本，是不加锁的

事务的四个隔离级别，他们由弱到强如下所示:

- `Read Uncommited(RU)`：读未提交，一个事务可以读到另一个事务未提交的数据！
  - 脏读：一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据
- `Read Committed (RC)`：读已提交，一个事务可以读到另一个事务已提交的数据!
  - 不可重复读：在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致
- `Repeatable Read (RR)`:可重复读，加入间隙锁，一定程度上避免了幻读的产生！注意了，只是一定程度上，并没有完全避免!我会在下一篇文章说明!另外就是记住从该级别才开始加入间隙锁(这句话记下来，后面有用到)!
  - 幻读：事务A 按照一定条件进行数据读取， 期间事务B 插入了相同搜索条件的新数据，事务A再次按照原先条件进行读取时，发现了事务B `新插入的数据 称为幻读`
- `Serializable`：串行化，该级别下读写串行化，且所有的`select`语句后都自动加上`lock in share mode`，即使用了共享锁。因此在该隔离级别下，使用的是当前读，而不是快照读。

**PS：**并不是用表锁来实现锁表的操作，而是利用了`Next-Key Locks`，也可以理解为是用了行锁+间隙锁来实现锁表的操作。

RU和RC不存在间隙锁

## 死锁

### 开启锁监控

在遇到线上死锁问题时，我们应该第一时间获取相关的死锁日志。我们可以通过 `show engine innodb status` 命令来获取死锁信息，但是它有个限制，只能拿到最近一次的死锁日志。MySQL 提供了一套 InnoDb 的监控机制，用于周期性（每隔 15 秒）输出 InnoDb 的运行状态到 mysqld 服务的标准错误输出（stderr）。默认情况下监控是关闭的，只有当需要分析问题时再开启，并且在分析问题之后，建议将监控关闭，因为它对数据库的性能有一定影响，另外每 15 秒输出一次日志，会使日志文件变得特别大。

InnoDb 的监控主要分为四种：标准监控（Standard InnoDB Monitor）、锁监控（InnoDB Lock Monitor）、表空间监控（InnoDB Tablespace Monitor）和表监控（InnoDB Table Monitor）。后两种监控已经基本上废弃了。要获取死锁日志，我们需要开启 InnoDb 的标准监控，我推荐将锁监控也打开，它可以提供一些额外的锁信息，在分析死锁问题时会很有用。开启监控的方法有两种：

1. 基于系统表

MySQL 使用了几个特殊的表名来作为监控的开关，比如在数据库中创建一个表名为 `innodb_monitor` 的表开启标准监控，创建一个表名为 `innodb_lock_monitor` 的表开启锁监控。MySQL 通过检测是否存在这个表名来决定是否开启监控，至于表的结构和表里的内容无所谓。相反的，如果要关闭监控，则将这两个表删除即可。

2. 基于系统参数

在 MySQL 5.6.16 之后，可以通过设置系统参数来开启锁监控，如下：

```
-- 开启标准监控``set` `GLOBAL` `innodb_status_output=``ON``;` `-- 关闭标准监控``set` `GLOBAL` `innodb_status_output=``OFF``;` `-- 开启锁监控``set` `GLOBAL` `innodb_status_output_locks=``ON``;` `-- 关闭锁监控``set` `GLOBAL` `innodb_status_output_locks=``OFF``;
```

另外，MySQL 提供了一个系统参数 `innodb_print_all_deadlocks` 专门用于记录死锁日志，当发生死锁时，死锁日志会记录到 MySQL 的错误日志文件中。

```
set` `GLOBAL` `innodb_print_all_deadlocks=``ON``;
```

### 读懂死锁日志

一切准备就绪之后，我们从 DBA 那里拿到了死锁日志（其中的SQL语句做了省略）：

```
------------------------``LATEST DETECTED DEADLOCK``------------------------``2017-09-06 11:58:16 7ff35f5dd700``*** (1) TRANSACTION:``TRANSACTION 182335752, ACTIVE 0 sec inserting``mysql tables in use 1, locked 1``LOCK WAIT 11 lock struct(s), heap size 1184, 2 row lock(s), undo log entries 15``MySQL thread id 12032077, OS thread handle 0x7ff35ebf6700, query id 196418265 10.40.191.57 RW_bok_db update``INSERT INTO bok_task``         ``( order_id ...``*** (1) WAITING FOR THIS LOCK TO BE GRANTED:``RECORD LOCKS space id 300 page no 5480 n bits 552 index `order_id_un` of table `bok_db`.`bok_task` ``  ``trx id 182335752 lock_mode X insert intention waiting``*** (2) TRANSACTION:``TRANSACTION 182335756, ACTIVE 0 sec inserting``mysql tables in use 1, locked 1``11 lock struct(s), heap size 1184, 2 row lock(s), undo log entries 15``MySQL thread id 12032049, OS thread handle 0x7ff35f5dd700, query id 196418268 10.40.189.132 RW_bok_db update``INSERT INTO bok_task``         ``( order_id ...``*** (2) HOLDS THE LOCK(S):``RECORD LOCKS space id 300 page no 5480 n bits 552 index `order_id_un` of table `bok_db`.`bok_task` ``  ``trx id 182335756 lock_mode X``*** (2) WAITING FOR THIS LOCK TO BE GRANTED:``RECORD LOCKS space id 300 page no 5480 n bits 552 index `order_id_un` of table `bok_db`.`bok_task` ``  ``trx id 182335756 lock_mode X insert intention waiting``*** WE ROLL BACK TRANSACTION (2)
```

日志中列出了死锁发生的时间，以及导致死锁的事务信息（只显示两个事务，如果由多个事务导致的死锁也只显示两个），并显示出每个事务正在执行的 SQL 语句、等待的锁以及持有的锁信息等。下面我们就来研究下这份死锁日志，看看从这份死锁日志中能不能发现死锁的原因？

首先看事务一的信息：

> *** (1) TRANSACTION:
> TRANSACTION 182335752, ACTIVE 0 sec inserting

ACTIVE 0 sec 表示事务活动时间，inserting 为事务当前正在运行的状态，可能的事务状态有：fetching rows，updating，deleting，inserting 等。

> mysql tables in use 1, locked 1
> LOCK WAIT 11 lock struct(s), heap size 1184, 2 row lock(s), undo log entries 15

--------------------------------

一共有四种类型的行锁：记录锁，间隙锁，Next-key 锁和插入意向锁。这四种锁对应的死锁日志各不相同，如下：

- 记录锁（LOCK_REC_NOT_GAP）: lock_mode X locks rec but not gap
- 间隙锁（LOCK_GAP）: lock_mode X locks gap before rec
- Next-key 锁（LOCK_ORNIDARY）: lock_mode X
- 插入意向锁（LOCK_INSERT_INTENTION）: lock_mode X locks gap before rec insert intention

事务二和事务一的日志基本类似，不过它多了一部分 HOLDS THE LOCK(S)，表示事务二持有什么锁，这个锁往往就是事务一处于锁等待的原因。这里可以看到事务二正在等待索引 order_id_un 上的插入意向锁，并且它已经持有了一个 X 锁（Next-key 锁，也有可能是 supremum 上的间隙锁）。

所以，对死锁的诊断不能仅仅靠死锁日志，还应该结合应用程序的代码来进行分析，如果实在接触不到应用代码，还可以通过数据库的 binlog 来分析（只要你的死锁不是 100% 必现，那么 binlog 日志里肯定能找到一份完整的事务一和事务二的 SQL 语句）。通过应用代码或 binlog 理出每个事务的 SQL 执行顺序，这样分析死锁时就会容易很多。

### 案例

1. 死锁的根本原因是有两个或多个事务之间加锁顺序的不一致导致的，这个死锁案例其实是最经典的死锁场景。

   首先，事务 A 获取 id = 20 的锁（lock_mode X locks rec but not gap），事务 B 获取 id = 30 的锁；然后，事务 A 试图获取 id = 30 的锁，而该锁已经被事务 B 持有，所以事务 A 等待事务 B 释放该锁，然后事务 B 又试图获取 id = 20 的锁，这个锁被事务 A 占有，于是两个事务之间相互等待，导致死锁。

2. 首先事务 A 和事务 B 执行了两条 UPDATE 语句，但是由于 id = 25 和 id = 26 记录都不存在，事务 A 和 事务 B 并没有更新任何记录，但是由于数据库隔离级别为 RR，所以会在 (20, 30) 之间加上间隙锁（lock_mode X locks gap before rec），间隙锁和间隙锁并不冲突。之后事务 A 和事务 B 分别执行 INSERT 语句要插入记录 id = 25 和 id = 26，需要在 (20, 30) 之间加插入意向锁（lock_mode X locks gap before rec insert intention），插入意向锁和间隙锁冲突，所以两个事务互相等待，最后形成死锁。

   要解决这个死锁很简单，显然，前面两条 UPDATE 语句是无效的，将其删除即可。另外也可以将数据库隔离级别改成 RC，这样在 UPDATE 的时候就不会有间隙锁了。这个案例正是文章开头提到的死锁日志中的死锁场景，别看这个 UPDATE 语句是无效的，看起来很傻，但是确实是真实的场景，因为在真实的项目中代码会非常复杂，比如采用了 ORM 框架，应用层和数据层代码分离，一般开发人员写代码时都不知道会生成什么样的 SQL 语句，我也是从 DBA 那里拿到了 binlog，然后从里面找到了事务执行的所有 SQL 语句，发现其中竟然有一行无效的 UPDATE 语句，最后追本溯源，找到对应的应用代码，将其删除，从而修复了这个死锁。

3. 别看这个案例里每个事务都只有一条 SQL 语句，但是却实实在在可能会导致死锁问题，其实说起来，这个死锁和案例一并没有什么区别，只不过理解起来要更深入一点。要知道在范围查询时，加锁是一条记录一条记录挨个加锁的，所以虽然只有一条 SQL 语句，如果两条 SQL 语句的加锁顺序不一样，也会导致死锁。

   在案例一中，事务 A 的加锁顺序为： id = 20 -> 30，事务 B 的加锁顺序为：id = 30 -> 20，正好相反，所以会导致死锁。这里的情景也是一样，事务 A 的范围条件为 id < 30，加锁顺序为：id = 15 -> 18 -> 20，事务 B 走的是二级索引 age，加锁顺序为：(age, id) = (24, 18) -> (24, 20) -> (25, 15) -> (25, 49)，其中，对 id 的加锁顺序为 id = 18 -> 20 -> 15 -> 49。可以看到事务 A 先锁 15，再锁 18，而事务 B 先锁 18，再锁 15，从而形成死锁。

### 如何避免死锁

对于 MySQL 的 InnoDb 存储引擎来说，死锁问题是避免不了的，没有哪种解决方案可以说完全解决死锁问题，但是我们可以通过一些可控的手段，降低出现死锁的概率。

1. 如上面的案例一和案例三所示，对索引加锁顺序的不一致很可能会导致死锁，所以如果可以，尽量以相同的顺序来访问索引记录和表。在程序以批量方式处理数据的时候，如果事先对数据排序，保证每个线程按固定的顺序来处理记录，也可以大大降低出现死锁的可能；
2. 如上面的案例二所示，Gap 锁往往是程序中导致死锁的真凶，由于默认情况下 MySQL 的隔离级别是 RR，所以如果能确定幻读和不可重复读对应用的影响不大，可以考虑将隔离级别改成 RC，可以避免 Gap 锁导致的死锁；
3. 为表添加合理的索引，如果不走索引将会为表的每一行记录加锁，死锁的概率就会大大增大；
4. 我们知道 MyISAM 只支持表锁，它采用一次封锁技术来保证事务之间不会发生死锁，所以，我们也可以使用同样的思想，在事务中一次锁定所需要的所有资源，减少死锁概率；
5. 避免大事务，尽量将大事务拆成多个小事务来处理；因为大事务占用资源多，耗时长，与其他事务冲突的概率也会变高；
6. 避免在同一时间点运行多个对同一表进行读写的脚本，特别注意加锁且操作数据量比较大的语句；我们经常会有一些定时脚本，避免它们在同一时间点运行；
7. 设置锁等待超时参数：`innodb_lock_wait_timeout`，这个参数并不是只用来解决死锁问题，在并发访问比较高的情况下，如果大量事务因无法立即获得所需的锁而挂起，会占用大量计算机资源，造成严重性能问题，甚至拖跨数据库。我们通过设置合适的锁等待超时阈值，可以避免这种情况发生。



## MVCC（多版本并发控制）

### 版本链

InnoDB的聚簇索引记录包含两个必要的隐藏列：

`trx_id`：每次对某条聚簇索引记录进行改动时，都会把对应的事务id赋值给`trx_id`隐藏列

`roll_pointer`：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到`undo日志`中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

![image-20210529183419187](/Users/cuweir/Library/Application Support/typora-user-images/image-20210529183419187.png)

### ReadView

这个`ReadView`中主要包含当前系统中还有哪些活跃的读写事务，把它们的事务id放到一个列表中，我们把这个列表命名为为`m_ids`。

使用`READ UNCOMMITTED`隔离级别的事务来说，直接读取记录的最新版本就好了，对于使用`SERIALIZABLE`隔离级别的事务来说，使用加锁的方式来访问记录。

`READ COMMITTED` --- 每次读取数据前都生成一个ReadView

`REPEATABLE READ` --- 在第一次读取数据时生成一个ReadView

所谓的MVCC（Multi-Version Concurrency Control ，多版本并发控制）指的就是在使用`READ COMMITTD`、`REPEATABLE READ`这两种隔离级别的事务在执行普通的`SEELCT`操作时访问记录的版本链的过程，这样子可以使不同事务的`读-写`、`写-读`操作并发执行，从而提升系统性能。

`READ COMMITTD`、`REPEATABLE READ`这两个隔离级别的一个很大不同就是生成`ReadView`的时机不同，`READ COMMITTD`在每一次进行普通`SELECT`操作前都会生成一个`ReadView`，而`REPEATABLE READ`只在第一次进行普通`SELECT`操作前生成一个`ReadView`，之后的查询操作都重复这个`ReadView`就好了。

## redo log和bin log

### bin_log

`binlog `用于记录数据库执行的写入性操作(不包括查询)信息，以二进制的形式保存在磁盘中。 `binlog `是 `mysql`
的逻辑日志，并且由 `Server `层进行记录，使用任何存储引擎的 `mysql `数据库都会记录 `binlog `日志。

- **逻辑日志**： 可以简单理解为记录的就是sql语句 。
- **物理日志**： `mysql `数据最终是保存在数据页中的，物理日志记录的就是数据页变更 。

`binlog `是通过追加的方式进行写入的，可以通过 `max_binlog_size `参数设置每个 `binlog`
文件的大小，当文件大小达到给定值之后，会生成新的文件来保存日志。

在实际应用中， `binlog `的主要使用场景有两个，分别是 **主从复制** 和 **数据恢复** 。

1. **主从复制** ：在 `Master `端开启 `binlog `，然后将 `binlog `发送到各个 `Slave `端， `Slave `端重放 `binlog `从而达到主从数据一致。
2. **数据恢复** ：通过使用 `mysqlbinlog `工具来恢复数据。

### redo_log

`redo log `包括两部分：一个是内存中的日志缓冲( `redo log buffer `)，另一个是磁盘上的日志文件( ` redo log
file `)。 `mysql `每执行一条 `DML `语句，先将记录写入 `redo log buffer `
，后续某个时间点再一次性将多个操作记录写到 `redo log file `。这种 **先写日志，再写磁盘** 的技术就是 `MySQL`
里经常说到的 `WAL(Write-Ahead Logging) `技术。

在计算机操作系统中，用户空间( `user space `)下的缓冲区数据一般情况下是无法直接写入磁盘的，中间必须经过操作系统内核空间( `
kernel space `)缓冲区( `OS Buffer `)。因此， `redo log buffer `写入 `redo log
file `实际上是先写入 `OS Buffer `，然后再通过系统调用 `fsync() `将其刷到 `redo log file `
中

**redo log记录方式**

![redo_log](/Users/cuweir/Documents/github/MyNoteBooks/redo_log.png)

write pos表示日志当前记录的位置，当ib_logfile_4写满后，会从ib_logfile_1从头开始记录；check point表示将日志记录的修改写进磁盘，完成数据落盘，数据落盘后checkpoint会将日志上的相关记录擦除掉，即write pos->checkpoint之间的部分是redo log空着的部分，用于记录新的记录，checkpoint->write pos之间是redo log待落盘的数据修改记录。当writepos追上checkpoint时，得先停下记录，先推动checkpoint向前移动，空出位置记录新的日志。
 有了redo log，当数据库发生宕机重启后，可通过redo log将未落盘的数据恢复，即保证已经提交的事务记录不会丢失。

### redo log与binlog区别

|          | redo log                                                     | binlog                                                       |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 文件大小 | `redo log `的大小是固定的。                                  | `binlog `可通过配置参数 `max_binlog_size `设置每个` binlog `文件的大小。 |
| 实现方式 | `redo log `是 `InnoDB `引擎层实现的，并不是所有引擎都有。    | `binlog `是 `Server` 层实现的，所有引擎都可以使用 `binlog `日志 |
| 记录方式 | redo log 采用循环写的方式记录，当写到结尾时，会回到开头循环写日志。 | binlog通过追加的方式记录，当文件大小大于给定值后，后续的日志会记录到新的文件上 |
| 适用场景 | `redo log `适用于崩溃恢复(crash-safe)                        | `binlog `适用于主从复制和数据恢复                            |

![image-20210604142308907](/Users/cuweir/Library/Application Support/typora-user-images/image-20210604142308907.png)

**mysql通过WAL(write-ahead logging)技术保证事务**

![image-20210604142403508](/Users/cuweir/Library/Application Support/typora-user-images/image-20210604142403508.png)


 **有了redo log，为啥还需要binlog呢？**

1、redo log的大小是固定的，日志上的记录修改落盘后，日志会被覆盖掉，无法用于数据回滚/数据恢复等操作。
2、redo log是innodb引擎层实现的，并不是所有引擎都有。



1、binlog是server层实现的，意味着所有引擎都可以使用binlog日志
 2、binlog通过追加的方式写入的，可通过配置参数max_binlog_size设置每个binlog文件的大小，当文件大小大于给定值后，日志会发生滚动，之后的日志记录到新的文件上。
 3、binlog有两种记录模式，statement格式的话是记sql语句， row格式会记录行的内容，记两条，更新前和更新后都有。

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210607001507939.png" alt="image-20210607001507939" style="zoom:67%;" />

![image-20210607001535498](/Users/cuweir/Library/Application Support/typora-user-images/image-20210607001535498.png)

1、innodb_flush_log_at_trx_commit：设置为1，表示每次事务的redolog都直接持久化到磁盘（注意是这里指的是redolog日志本身落盘），保证mysql重启后数据不丢失。
 2、sync_binlog： 设置为1，表示每次事务的binlog都直接持久化到磁盘（注意是这里指的是binlog日志本身落盘），保证mysql重启后binlog记录是完整的。

### undo_log

数据库事务四大特性中有一个是 **原子性** ，具体来说就是 **原子性是指对数据库的一系列操作，要么全部成功，要么全部失败，不可能出现部分成功的情况**
。实际上， **原子性** 底层就是通过 `undo log `实现的。 `undo log `主要记录了数据的逻辑变化，比如一条 ` INSERT
`语句，对应一条 `DELETE `的 `undo log `，对于每个 `UPDATE `语句，对应一条相反的 `UPDATE `的`
undo log `，这样在发生错误时，就能回滚到事务之前的数据状态。同时， `undo log `也是 `MVCC

## InnoDB和Myisam如何选择

- 一、InnoDB支持事务，MyISAM不支持，这一点是非常之重要。事务是一种高级的处理方式，如在一些列增删改中只要哪个出错还可以回滚还原，而MyISAM就不可以了。
- 二、MyISAM适合查询以及插入为主的应用，InnoDB适合频繁修改以及涉及到安全性较高的应用
- 三、InnoDB支持外键，MyISAM不支持
- 四、MyISAM是默认引擎，InnoDB需要指定
- 五、InnoDB不支持FULLTEXT类型的索引
- 六、InnoDB中不保存表的行数，如select count(*) from table时，InnoDB需要扫描一遍整个表来计算有多少行，但是MyISAM只要简单的读出保存好的行数即可。注意的是，当count(*)语句包含where条件时MyISAM也需要扫描整个表
- 七、对于自增长的字段，InnoDB中必须包含只有该字段的索引，但是在MyISAM表中可以和其他字段一起建立联合索引
- 八、清空整个表时，InnoDB是一行一行的删除，效率非常慢。MyISAM则会重建表
- 九、InnoDB支持行锁（某些情况下还是锁整表，如 **update table set a=1 where user like '%lee%'**

## 分库分表

为了**支撑高并发、数据量大**两个问题

#### 分表

**单表数据量太大**，会极大影响你的 sql **执行的性能**。单表到几百万的时候，性能就会相对差一些了，你就得分表

#### 分库

| #            | 分库分表前                   | 分库分表后                                   |
| ------------ | ---------------------------- | -------------------------------------------- |
| 并发支撑情况 | MySQL 单机部署，扛不住高并发 | MySQL 从单机到多机，能承受的并发增加了多倍   |
| 磁盘使用情况 | MySQL 单机磁盘容量几乎撑满   | 拆分为多个库，数据库服务器磁盘使用率大大降低 |
| SQL 执行性能 | 单表数据量太大，SQL 越跑越慢 | 单表数据量减少，SQL 执行效率明显提升         |

### 中间件

Sharding-jdbc 这种 client 层方案的**优点在于不用部署，运维成本低，不需要代理层的二次转发请求，性能很高**，但是如果遇到升级啥的需要各个系统都重新升级版本再发布，各个系统都需要**耦合** Sharding-jdbc 的依赖

Mycat 这种 proxy 层方案的**缺点在于需要部署**，自己运维一套中间件，运维成本高，但是**好处在于对于各个项目是透明的**，如果遇到升级之类的都是自己中间件那里搞就行了

### 垂直拆分或水平拆分

**水平拆分**的意思，就是把一个表的数据给弄到多个库的多个表里去，但是每个库的表结构都一样

水平拆分的意义，就是将数据均匀放更多的库里，然后用多个库来扛更高的并发，还有就是用多个库的存储容量来进行扩容。

**垂直拆分**的意思，就是**把一个有很多字段的表给拆分成多个表**，**或者是多个库上去**

一般来说，会**将较少的访问频率很高的字段放到一个表里去**，然后**将较多的访问频率很低的字段放到另外一个表里去**

还有两种**分库分表的方式**：

- 一种是按照 range 来分，就是每个库一段连续的数据，这个一般是按比如**时间范围**来的，但是这种一般较少用，因为很容易产生热点问题，大量的流量都打在最新的数据上了。
- 或者是按照某个字段 hash 一下均匀分散，这个较为常用。

range 来分，好处在于说，扩容的时候很简单，因为你只要预备好，给每个月都准备一个库就可以了，到了一个新的月份的时候，自然而然，就会写新的库了；缺点，但是大部分的请求，都是访问最新的数据。实际生产用 range，要看场景。

hash 分发，好处在于说，可以平均分配每个库的数据量和请求压力；坏处在于说扩容起来比较麻烦，会有一个数据迁移的过程，之前的数据需要重新计算 hash 值重新分配到不同的库或表。

### 如何动态切换成分库分表

#### 停机迁移方案

之前得写好一个**导数的一次性工具**，此时直接跑起来，然后将单库单表的数据哗哗哗读出来，写到分库分表里面去。比较low

![image-20210604163237964](/Users/cuweir/Library/Application Support/typora-user-images/image-20210604163237964.png)

#### 双写迁移方案

就是在线上系统里面，之前所有写库的地方，增删改操作，**除了对老库增删改，都加上对新库的增删改**，这就是所谓的**双写**，同时写俩库，老库和新库。

然后**系统部署**之后，新库数据差太远，用之前说的导数工具，跑起来读老库数据写新库，写的时候要根据 gmt_modified 这类字段判断这条数据最后修改的时间，除非是读出来的数据在新库里没有，或者是比新库的数据新才会写。简单来说，就是不允许用老数据覆盖新数据。

导完一轮之后，有可能数据还是存在不一致，那么就程序自动做一轮校验，比对新老库每个表的每条数据，接着如果有不一样的，就针对那些不一样的，从老库读数据再次写。反复循环，直到两个库每个表的数据都完全一致为止。

接着当数据完全一致了，就 ok 了，基于仅仅使用分库分表的最新代码，重新部署一次，不就仅仅基于分库分表在操作了么，还没有几个小时的停机时间，很稳。所以现在基本玩儿数据迁移之类的，都是这么干的。

![image-20210604164542456](/Users/cuweir/Library/Application Support/typora-user-images/image-20210604164542456.png)

### 动态扩缩容

一个实践是利用 `32 * 32` 来分库分表，即分为 32 个库，每个库里一个表分为 32 张表。一共就是 1024 张表。根据某个 id 先根据 32 取模路由到库，再根据 32 取模路由到库里的表。

| orderId | id % 32 (库) | id / 32 % 32 (表) |
| ------- | ------------ | ----------------- |
| 259     | 3            | 8                 |
| 1189    | 5            | 5                 |
| 352     | 0            | 11                |
| 4593    | 17           | 15                |

刚开始的时候，这个库可能就是逻辑库，建在一个数据库上的，就是一个 MySQL 服务器可能建了 n 个库，比如 32 个库。后面如果要拆分，就是不断在库和 MySQL 服务器之间做迁移就可以了。然后系统配合改一下配置即可。

比如说最多可以扩展到 32 个数据库服务器，每个数据库服务器是一个库。如果还是不够？最多可以扩展到 1024 个数据库服务器，每个数据库服务器上面一个库一个表。因为最多是 1024 个表。

这么搞，是不用自己写代码做数据迁移的，都交给 DBA 来搞好了，但是 DBA 确实是需要做一些库表迁移的工作，但是总比你自己写代码，然后抽数据导数据来的效率高得多吧。

哪怕是要减少库的数量，也很简单，其实说白了就是按倍数缩容就可以了，然后修改一下路由规则。

这里对步骤做一个总结：

1. 设定好几台数据库服务器，每台服务器上几个库，每个库多少个表，推荐是 32 库 * 32 表，对于大部分公司来说，可能几年都够了。
2. 路由的规则，orderId 模 32 = 库，orderId / 32 模 32 = 表
3. 扩容的时候，申请增加更多的数据库服务器，装好 MySQL，呈倍数扩容，4 台服务器，扩到 8 台服务器，再到 16 台服务器。
4. 由 DBA 负责将原先数据库服务器的库，迁移到新的数据库服务器上去，库迁移是有一些便捷的工具的。
5. 我们这边就是修改一下配置，调整迁移的库所在数据库服务器的地址。
6. 重新发布系统，上线，原先的路由规则变都不用变，直接可以基于 n 倍的数据库服务器的资源，继续进行线上系统的提供服务。

### id怎么生成

#### 基于数据库实现

1. 数据库自增id

**缺点就是单库生成**自增 id，要是高并发的话，就会有瓶颈的

除非是你**并发不高，但是数据量太大**导致的分库分表扩容，你可以用这个方案，因为可能每秒最高并发最多就几百，那么就走单独的一个库和表生成自增主键即可

2. 设置数据库 sequence 或者表自增字段步长

#### 雪花算法

snowflake 算法是 twitter 开源的分布式 id 生成算法，采用 Scala 语言实现，是把一个 64 位的 long 型的 id，1 个 bit 是不用的，用其中的 41 bits 作为毫秒数，用 10 bits 作为工作机器 id，12 bits 作为序列号。

- 1 bit：不用，为啥呢？因为二进制里第一个 bit 为如果是 1，那么都是负数，但是我们生成的 id 都是正数，所以第一个 bit 统一都是 0。
- 41 bits：表示的是时间戳，单位是毫秒。41 bits 可以表示的数字多达 `2^41 - 1` ，也就是可以标识 `2^41 - 1` 个毫秒值，换算成年就是表示 69 年的时间。
- 10 bits：记录工作机器 id，代表的是这个服务最多可以部署在 2^10 台机器上，也就是 1024 台机器。但是 10 bits 里 5 个 bits 代表机房 id，5 个 bits 代表机器 id。意思就是最多代表 `2^5` 个机房（32 个机房），每个机房里可以代表 `2^5` 个机器（32 台机器）。
- 12 bits：这个是用来记录同一个毫秒内产生的不同 id，12 bits 可以代表的最大正整数是 `2^12 - 1 = 4096` ，也就是说可以用这个 12 bits 代表的数字来区分**同一个毫秒内**的 4096 个不同的 id。

就是说 41 bit 是当前毫秒单位的一个时间戳，就这意思；然后 5 bit 是你传递进来的一个**机房** id（但是最大只能是 32 以内），另外 5 bit 是你传递进来的**机器** id（但是最大只能是 32 以内），剩下的那个 12 bit 序列号，就是如果跟你上次生成 id 的时间还在一个毫秒内，那么会把顺序给你累加，最多在 4096 个序号以内。



## 读写分离

就是基于主从复制架构，简单来说，就搞一个主库，挂多个从库，然后我们就单单只是写主库，然后主库会自动把数据给同步到从库上去。

### 主从复制原理

1. 当Master节点进行insert、update、delete操作时，会按顺序写⼊入到binlog中;

2. slave从库连接master主库，Master有多少个slave就会创建多少个binlog dump线程。

3. 当Master节点的binlog发⽣生变化时，binlog dump 线程会通知所有的slave节点，并将相应的binlog内容

   推送给slave节点。

4. I/O线程接收到binlog内容后，将内容写⼊入到本地的relay-log。

5. SQL线程读取I/O线程写⼊入的relay-log，并且根据relay-log的内容对从数据库做对应的操作。

主库将变更写入 binlog 日志，然后从库连接到主库之后，从库有一个 IO 线程，将主库的 binlog 日志拷贝到自己本地，写入一个 relay 中继日志中。接着从库中有一个 SQL 线程会从中继日志读取 binlog，然后执行 binlog 日志中的内容，也就是在自己本地再次执行一遍 SQL，这样就可以保证自己跟主库的数据是一样的。

![image-20210604154858715](/Users/cuweir/Library/Application Support/typora-user-images/image-20210604154858715.png)

这里有一个非常重要的一点，就是从库同步主库数据的过程是串行化的，也就是说主库上并行的操作，在从库上会串行执行。所以这就是一个非常重要的点了，由于从库从主库拷贝日志以及串行执行 SQL 的特点，在高并发场景下，从库的数据一定会比主库慢一些，是**有延时**的。所以经常出现，刚写入主库的数据可能是读不到的，要过几十毫秒，甚至几百毫秒才能读取到。

而且这里还有另外一个问题，就是如果主库突然宕机，然后恰好数据还没同步到从库，那么有些数据可能在从库上是没有的，有些数据可能就丢失了。

所以 MySQL 实际上在这一块有两个机制，一个是**半同步复制**，用来解决主库数据丢失问题；一个是**并行复制**，用来解决主从同步延时问题。

这个所谓**半同步复制**，也叫 `semi-sync` 复制，指的就是主库写入 binlog 日志之后，就会将**强制**此时立即将数据同步到从库，从库将日志写入自己本地的 relay log 之后，接着会返回一个 ack 给主库，主库接收到**至少一个从库**的 ack 之后才会认为写操作完成了。

所谓**并行复制**，指的是从库开启多个线程，并行读取 relay log 中不同库的日志，然后**并行重放不同库的日志**，这是库级别的并行。

### MySQL 主从同步延时问题

以前线上确实处理过因为主从同步延时问题而导致的线上的 bug，属于小型的生产事故。

是这个么场景。有个同学是这样写代码逻辑的。先插入一条数据，再把它查出来，然后更新这条数据。在生产环境高峰期，写并发达到了 2000/s，这个时候，主从复制延时大概是在小几十毫秒。线上会发现，每天总有那么一些数据，我们期望更新一些重要的数据状态，但在高峰期时候却没更新。用户跟客服反馈，而客服就会反馈给我们。

我们通过 MySQL 命令：

```
show slave status
```

查看 `Seconds_Behind_Master` ，可以看到从库复制主库的数据落后了几 ms。

一般来说，如果主从延迟较为严重，有以下解决方案：

- 分库，将一个主库拆分为多个主库，每个主库的写并发就减少了几倍，此时主从延迟可以忽略不计。
- 打开 MySQL 支持的并行复制，多个库并行复制。如果说某个库的写入并发就是特别高，单库写并发达到了 2000/s，并行复制还是没意义。
- 重写代码，写代码的同学，要慎重，插入数据时立马查询可能查不到。
- 如果确实是存在必须先插入，立马要求就查询到，然后立马就要反过来执行一些操作，对这个查询**设置直连主库**。**不推荐**这种方法，你要是这么搞，读写分离的意义就丧失了。

## explain

1. select_type：表示select查询的类型

select_type属性下有好几种类型

- **SIMPLLE**：简单查询，该查询不包含 UNION 或子查询
- **PRIMARY**：如果查询包含UNION 或子查询，则**最外层的查询**被标识为PRIMARY
- UNION：表示此查询是 UNION 中的第二个或者随后的查询
- DEPENDENT：UNION 满足 UNION 中的第二个或者随后的查询，其次取决于外面的查询
- UNION RESULT：UNION 的结果
- **SUBQUERY**：子查询中的第一个select语句(该子查询不在from子句中)
- DEPENDENT SUBQUERY：子查询中的 第一个 select，同时取决于外面的查询
- **DERIVED**：包含在from子句中子查询(也称为派生表)
- UNCACHEABLE SUBQUERY：满足是子查询中的第一个 select 语句，同时意味着 select 中的某些特性阻止结果被缓存于一个 Item_cache 中
- UNCACHEABLE UNION：满足此查询是 UNION 中的第二个或者随后的查询，同时意味着 select 中的某些特性阻止结果被缓存于一个 Item_cache 中

2. table：该列显示了对应行正在访问哪个表(有别名就显示别名)。

3. type：该列称为**关联类型或者访问类型**，它指明了MySQL决定如何查找表中符合条件的行，同时**是我们判断查询是否高效的重要依据**。

以下为常见的取值

- ALL：**全表扫描**，这个类型是性能最差的查询之一。通常来说，我们的查询不应该出现 ALL 类型，因为这样的查询，在数据量最大的情况下，对数据库的性能是巨大的灾难。
- index：**全索引扫描**，和 ALL 类型类似，只不过 ALL 类型是全表扫描，而 index 类型是扫描全部的索引，主要优点是避免了排序，但是开销仍然非常大。如果在 Extra 列看到 Using index，说明正在使用覆盖索引，只扫描索引的数据，它比按索引次序全表扫描的开销要少很多。
- range：**范围扫描**，就是一个有限制的索引扫描，它开始于索引里的某一点，返回匹配这个值域的行。这个类型通常出现在 `=、<>、>、>=、<、<=、IS NULL、<=>、BETWEEN、IN()` 的操作中，key 列显示使用了哪个索引，当 type 为该值时，则输出的 ref 列为 NULL，并且 key_len 列是此次查询中使用到的索引最长的那个。
- ref：一种索引访问，也称索引查找，它返回所有匹配某个单个值的行。此类型通常出现在多表的 join 查询, 针对于非唯一或非主键索引, 或者是使用了最左前缀规则索引的查询。
- eq_ref：使用这种索引查找，最多只返回一条符合条件的记录。在使用唯一性索引或主键查找时会出现该值，非常高效。
- const、system：该表至多有一个匹配行，在查询开始时读取，或者该表是系统表，只有一行匹配。其中 const 用于在和 primary key 或 unique 索引中有固定值比较的情形。
- NULL：在执行阶段不需要访问表

4. extra：其他的信息

常见的取值如下：

- **Using index**：使用覆盖索引，表示查询索引就可查到所需数据，不用扫描表数据文件，往往说明性能不错。
- Using Where：在存储引擎检索行后再进行过滤，使用了where从句来限制哪些行将与下一张表匹配或者是返回给用户。
- Using temporary：在查询结果排序时会使用一个临时表，一般出现于排序、分组和多表 join 的情况，查询效率不高，建议优化。
- Using filesort：对结果使用一个外部索引排序，而不是按索引次序从表里读取行，一般有出现该值，都建议优化去掉，因为这样的查询 CPU 资源消耗大。

5. possible_key
6. key

## limit效率

limit的效率问题

在offset很⼤大的时候，使⽤用limit会出现效率问题

select * from test where val=4 limit 300000,5;
 -- 5 rows in set (15.98 sec)
 select * from test a inner join (select id from test where val=4 limit 300000,5) b on a.id=b.id;
 -- 5 rows in set (0.38 sec)

原因:limit查询的过程:查询到索引叶⼦子结点上的数据，然后在根据叶⼦子节点上的PRIMARY KEY去聚 簇索引上查找全部字段。查找前300005个，抛弃前300000个。

解决⽅方法:先使⽤用limit查出主键id

# 7. Redis

## 清除策略

1. 被动清除：读/写⼀个已经过期的key时，会触发惰性删除策略，直接删除掉这个过期key
2. 主动删除（定期删除）：由于惰性删除策略⽆法保证冷数据被及时删掉，所以Redis会定期主动淘汰⼀批已过期的key
3. 当前已⽤内存超过maxmemory限定时，触发主动清理策略

### 内存淘汰算法

1. volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使⽤的数据淘汰
2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
4. allkeys-lru：当内存不⾜以容纳新写⼊数据时，在键空间中，移除最近最少使⽤的key（这个是最常 ⽤的）
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
6. no-eviction：禁⽌驱逐数据，也就是说当内存不⾜以容纳新写⼊数据时，新写⼊操作会报错。这个 应该没⼈使⽤吧

4.0版本后增加了两种：

1. volatile-lfu：从已设置过期时间的数据集(server.db[i].expires)中挑选最不经常使⽤的数据淘汰
2. allkeys-lfu：当内存不⾜以容纳新写⼊数据时，在键空间中，移除最不经常使⽤的key

## RDB和AOF

RDB全量持久化

AOF（append-only file）增量持久化

### RDB

- RDB 将数据库的快照（snapshot）以二进制的方式保存到磁盘中，对 Redis 中的数据执行**周期性**的持久化

Redis 客户端还同时提供两个命令来生成 RDB 存储文件，也就是 `SAVE` 和 `BGSAVE`

使用 `BGSAVE` 命令时，Redis 会立刻 `fork` 出一个子进程

子进程和主进程共享一份内存空间

写时拷贝

### AOF

- AOF 则以协议文本的方式，将所有对数据库进行过写入的命令（及其参数）记录到 AOF 文件，以此达到记录数据库状态的目的
- AOF 机制对每条写入命令作为日志，以 `append-only` 的模式写入一个日志文件中，在 Redis 重启的时候，可以通过**回放** AOF 日志中的写入指令来重新构建整个数据集

当 Redis 重启时， 它会优先使⽤ AOF ⽂件 来还原数据集， 因为 AOF ⽂件保存的数据集通常⽐ RDB ⽂件所保存的数据集更完整（如果同时使用 RDB 和 AOF 两种持久化机制，那么在 Redis 重启的时候，会使用 **AOF** 来重新构建数据，因为 AOF 中的**数据更加完整**。）

重写

bgrewriteaof

### 优缺点

**RDB 优缺点**

- RDB 会生成多个数据文件，每个数据文件都代表了某一个时刻中 Redis 的数据，这种多个数据文件的方式，**非常适合做冷备**，可以将这种完整的数据文件发送到一些远程的安全存储上去，比如说 Amazon 的 S3 云服务上去，在国内可以是阿里云的 ODPS 分布式存储上，以预定好的备份策略来定期备份 Redis 中的数据。
- RDB 对 Redis 对外提供的读写服务，影响非常小，可以让 Redis **保持高性能**，因为 Redis 主进程只需要 fork 一个子进程，让子进程执行磁盘 IO 操作来进行 RDB 持久化即可。
- 相对于 AOF 持久化机制来说，直接基于 RDB 数据文件来重启和恢复 Redis 进程，更加快速。
- 如果想要在 Redis 故障时，尽可能少的丢失数据，那么 RDB 没有 AOF 好。一般来说，RDB 数据快照文件，都是每隔 5 分钟，或者更长时间生成一次，这个时候就得接受一旦 Redis 进程宕机，那么会丢失最近 5 分钟（甚至更长时间）的数据。
- RDB 每次在 fork 子进程来执行 RDB 快照数据文件生成的时候，如果数据文件特别大，可能会导致对客户端提供的服务暂停数毫秒，或者甚至数秒。

**AOF 优缺点**

- AOF 可以更好的保护数据不丢失，一般 AOF 会每隔 1 秒，通过一个后台线程执行一次 `fsync` 操作，最多丢失 1 秒钟的数据。
- AOF 日志文件以 `append-only` 模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复。
- AOF 日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在 `rewrite` log 的时候，会对其中的指令进行压缩，创建出一份需要恢复数据的最小日志出来。在创建新日志文件的时候，老的日志文件还是照常写入。当新的 merge 后的日志文件 ready 的时候，再交换新老日志文件即可。
- AOF 日志文件的命令通过可读较强的方式进行记录，这个特性非常**适合做灾难性的误删除的紧急恢复**。比如某人不小心用 `flushall` 命令清空了所有数据，只要这个时候后台 `rewrite` 还没有发生，那么就可以立即拷贝 AOF 文件，将最后一条 `flushall` 命令给删了，然后再将该 `AOF` 文件放回去，就可以通过恢复机制，自动恢复所有数据。
- 对于同一份数据来说，AOF 日志文件通常比 RDB 数据快照文件更大。
- AOF 开启后，支持的写 QPS 会比 RDB 支持的写 QPS 低，因为 AOF 一般会配置成每秒 `fsync` 一次日志文件，当然，每秒一次 `fsync` ，性能也还是很高的。（如果实时写入，那么 QPS 会大降，Redis 性能会大大降低）
- 以前 AOF 发生过 bug，就是通过 AOF 记录的日志，进行数据恢复的时候，没有恢复一模一样的数据出来。所以说，类似 AOF 这种较为复杂的基于命令日志 `merge` 回放的方式，比基于 RDB 每次持久化一份完整的数据快照文件的方式，更加脆弱一些，容易有 bug。不过 AOF 就是为了避免 rewrite 过程导致的 bug，因此每次 rewrite 并不是基于旧的指令日志进行 merge 的，而是**基于当时内存中的数据进行指令的重新构建**，这样健壮性会好很多。

**RDB 和 AOF 到底该如何选择**

- 不要仅仅使用 RDB，因为那样会导致你丢失很多数据；
- 也不要仅仅使用 AOF，因为那样有两个问题：第一，你通过 AOF 做冷备，没有 RDB 做冷备来的恢复速度更快；第二，RDB 每次简单粗暴生成数据快照，更加健壮，可以避免 AOF 这种复杂的备份和恢复机制的 bug；
- Redis 支持同时开启开启两种持久化方式，我们可以综合使用 AOF 和 RDB 两种持久化机制，用 AOF 来保证数据不丢失，作为数据恢复的第一选择；用 RDB 来做不同程度的冷备，在 AOF 文件都丢失或损坏不可用的时候，还可以使用 RDB 来进行快速的数据恢复。

RDB紧凑，适合用于备份；发生故障停机会丢失几分钟数据

AOF耐久，发生故障停机也没啥，自动重写，到处容易；缺点体积大于RDB

## 五种数据结构（sds）

- Strings
  - 最简单的类型，就是普通的 set 和 get，做简单的 KV 缓存
  - 可以存储字符串，整型和浮点型等类型的数据。适合于简单的K-V数据场景，定义了简单动态字符串(SDS: Simple Dynamic String)，类似于Java的ArrayList

SDS：在 Redis 3.2 版本以前，SDS 的结构如下：

```
struct sdshdr {
    unsigned int len;
    unsigned int free;
    char buf[];
};
```

其中，**buf 表示数据空间，用于存储字符串；len 表示 buf 中已占用的字节数，也即字符串长度；free 表示 buf 中剩余可用字节数。**

这样做有以下几个好处：

- 用单独的变量 len 和 free，可以**方便地获取字符串长度和剩余空间**；
- 内容存储在动态数组 buf 中，**SDS 对上层暴露的指针指向 buf，而不是指向结构体 SDS**。因此，上层可以像读取 C 字符串一样读取 SDS 的内容，兼容 C 语言处理字符串的各种函数，同时也能通过 buf 地址的偏移，方便地获取其他变量；
- 读写字符串不依赖于 `\0`，保证**二进制安全**。

在 Redis 3.2 版本之后（v3.2 - v6.0），Redis 将 SDS 划分为 5 种类型：

- sdshdr5：长度小于 1 字节
- sdshdr8：长度 1 字节
- sdshdr16：长度 2 字节
- sdshdr32：长度 4 字节
- sdshdr64：长度 8 字节

Redis 增加了一个 flags 字段来标识类型，用一个字节(8 位)来存储。

其中：前 3 位表示字符串的类型；剩余 5 位，可以用来存储长度小于 32 的短字符串。

而对于长度大于 31 的字符串，仅仅靠 flags 的后 5 位来存储长度明显是不够的，需要用另外的变量来存储。sdshdr8、sdshdr16、sdshdr32、sdshdr64 的数据结构定义如下，其中 len 表示已使用的长度，alloc 表示总长度，buf 存储实际内容，而 flags 的前 3 位依然存储类型，后 5 位则预留。

- Hashes
  - 类似 map 的一种结构，这个一般就是可以将结构化的数据，比如一个对象（前提是**这个对象没嵌套其他的对象**）给缓存在 Redis 里，然后每次读写缓存的时候，可以就操作 hash 里的**某个字段**
  - Hash字典和压缩列表实现
- Lists
  - 有序列表，还可以当做栈来使用，它适合存储列表性质的数据
  - 使用的是双端链表和压缩列表实现的，这就解释了它为什么能在头尾操作元素
- Sets
  - 无序集合，自动去重
  - 整数集合是Set的底层实现之一，当集合只有整型元素且元素数量不多的时候，Redis就会使用整数集合来实现Set
- Sorted Sets
  - 排序的set，，去重但可以排序，写进去的时候给一个分数，自动根据分数排序
  - 跳表实现的

> Redis 除了这 5 种数据类型之外，还有 Bitmaps、HyperLogLogs、Streams 等。

## 架构模式

### 主从复制

建立连接——数据同步——命令传播

##### 建立连接

这个阶段主要是从服务器器发出 slaveof 命令之后，与主服务器器如何建⽴立连接，为数据同步做准备的过 程。

1)在 slaveof 命令执⾏行行之后，从服务器器根据设置的master的ip地址和端⼝口，创建连向主服务器器的socket 套接字连接，连接成功后，从服务器器会为这个套接字关联⼀一个专⻔门的处理理器器，⽤用于处理理后续的复制⼯工作

2)建⽴立连接之后，从服务器器会向主服务器器发送 ping 命令，确认主服务器器是否可⽤用，以及当前是否可⽤用 接受处理理命令。如果收到主服务器器的 pong 回复说明是可⽤用的，否则有可能是⽹网络超时或主服务器器阻 塞，从服务器器会断开连接发起重连

3)身份验证。如果主服务器器设置了了 requirepass 选项，那么从服务器器必须配置 masterauth 选项， 且保证密码⼀一致才能通过验证

4)身份验证完成之后，从服务器器会发送⾃自⼰己的监听端⼝口，主服务器器会保存下来

##### 数据同步

在主从服务器器建⽴立连接确认各⾃自身份之后，就开始数据同步，从服务器器向主服务器器发送 PSYNC 命令，执 ⾏行行同步操作，并把⾃自⼰己的数据库状态更更新⾄至主服务器器的数据库状态

Redis的主从同步分为:完整重同步(fullresynchronization)*和* 部分重同步(partial resynchronization)

完整重同步

有两种情况下是完整重同步，⼀一是slave连接上master第⼀一次复制的时候;⼆二是如果当主从断线，重新连 接复制的时候有可能是完整重同步，这个在后⾯面说

1. 从服务器器连接主服务器器，发送SYNC命令

2. 主服务器器接收到SYNC命名后，开始执⾏行行 bgsave 命令⽣生成RDB⽂文件并使⽤用缓冲区记录此后执⾏行行的所 有写命令
3. 主服务器器 bgsave 执⾏行行完后，向所有从服务器器发送快照⽂文件，并在发送期间继续记录被执⾏行行的写命 令
4. 从服务器器收到快照⽂文件后丢弃所有旧数据，载⼊入收到的快照 
5. 主服务器快照发送完毕后开始向从服务器器发送缓冲区中的写命令 
6. 从服务器完成对快照的载⼊入，开始接收命令请求，并执⾏行行来⾃自主服务器器缓冲区的写命令

部分同步

部分重同步是⽤用于处理理断线后重复制的情况，先介绍⼏几个⽤用于部分重同步的部分

runid (replication ID)，主服务器器运⾏行行id，Redis实例例在启动时，随机⽣生成⼀一个⻓长度40的唯⼀一字符串串 来标识当前节点

offset ，复制偏移量量。主服务器器和从服务器器各⾃自维护⼀一个复制偏移量量，记录传输的字节数。当主 节点向从节点发送N个字节数据时，主节点的offset增加N，从节点收到主节点传来的N个字节数据 时，从节点的offset增加N

replication backlog buffer ，复制积压缓冲区。是⼀一个固定⻓长度的FIFO队列列，⼤大⼩小由配置参 数 repl-backlog-size 指定，默认⼤大⼩小1MB。需要注意的是该缓冲区由master维护并且有且只有 ⼀一个，所有slave共享此缓冲区，其作⽤用在于备份最近主库发送给从库的数据

当slave连接到master，会执⾏行行 PSYNC <runid> <offset> 发送记录旧的master的 runid (replication ID)和偏移量量 offset ，这样master能够只发送slave所缺的增量量部分。但是如果master的复制积压缓存 区没有⾜足够的命令记录，或者slave传的 runid (replication ID)不不对，就会进⾏行行完整重同步，即slave会获 得⼀一个完整的数据集副本

##### 命令传播

为了能够使主从服务器的数据保持⼀致性，主服务器会对从服务器执⾏命令传 播操作，即每执⾏⼀个写命令就会向从服务器发送同样的写命令

从服务器会默认以每秒⼀次的频率向主服务器发送⼼跳检测

REPLCONF ACK <replication_offset>

其中 replication_offset 是当前从服务器器的复制偏移量量，该命令的作⽤用有三个

检测主从服务器器的⽹网络连接状态 

辅助实现 min-slaves 选项 

检测命令丢失

#### 主从持久化

Master关闭持久化:RDB持久化需要调⽤用 fork() 函数构建⼦子进程进⾏行行持久化。在这个过程中，父 进程会阻塞，影响性能;AOF rewrite也会造成同样结果

Slave开RDB，必要时开启AOF

### 哨兵

每个 Sentinel 节点都需要 定期执⾏行行 以下任务:

1. 每秒钟向它所知的主、从服务器及其他实例发送一个PING

2. 如果⼀个 实例（ instance ）距离 最后⼀次 有效回复 PING 命令的时间超过 down-after-milliseconds 所指定的值，那么这个实例会被 Sentinel 标记为 主观下线。

3. 如果⼀个 主服务器 被标记为 主观下线，那么正在 监视 这个 主服务器 的所有 Sentinel 节点， 要以 每秒⼀次 的频率确认 主服务器 的确进⼊了 主观下线 状态。
4. 如果⼀一个 主服务器器 被标记为 主观下线，并且有 ⾜足够数量量 的 Sentinel (⾄至少要达到 配置⽂文件 指 定的数量量)在指定的 时间范围 内同意这⼀一判断，那么这个 主服务器器 被标记为 客观下线。
5. 在一般情况下， 每个 Sentinel 会以每 10 秒一次的频率，向它已知的所有 主服务器 和 从服务 器 发送 INFO 命令。当一个 主服务器 被 Sentinel 标记为 客观下线 时， Sentinel 向 下线主 服务器 的所有 从服务器 发送 INFO 命令的频率，会从 10 秒一次改为 每秒⼀次。
6. Sentinel 和其他 Sentinel 协商 主节点 的状态，如果 主节点 处于 SDOWN 状态，则投票⾃自动 选出新的 主节点。将剩余的 从节点 指向 新的主节点 进⾏行行 数据复制。
7. 当没有⾜足够数量量的 Sentinel 同意 主服务器器 下线时， 主服务器器 的 客观下线状态 就会被移除。当 主服务器器 重新向 Sentinel 的 PING 命令返回 有效回复 时，主服务器器 的 主观下线状态 就会被 移除。

注意:⼀一个有效的 PING 回复可以是: +PONG 、 -LOADING 或者 -MASTERDOWN 。如果 服务

器器 返回除以上三种回复之外的其他回复，⼜又或者在 指定时间 内没有回复 PING Sentinel 认为服务器器返回的回复 ⽆无效 ( non-valid )。

**sentinel**

假设这时，主服务器server1进入下线状态，那么从服务器server2、server3、server4对主服务器的复制操作将被中止，并且Sentinel系统会察觉到server1已下线。

当server1的下线时长超过用户设定的下线时长上限时，Sentinel系统就会对server1执行故障转移操作：  

 •首先，Sentinel系统会挑选server1属下的其中一个从服务器，并将这个被选中的从服务器升级为新的主服务器。   

•之后，Sentinel系统会向server1属下的所有从服务器发送新的复制指令，让它们成为新的主服务器的从服务器，当所有从服务器都开始复制新的主服务器时，故障转移操作执行完毕。  

•另外，Sentinel还会继续监视已下线的server1，并在它重新上线时，将它设置为新的主服务器的从服务器。

## 分布式锁

**setnx**

不推荐原因:
1.根据流程图可看出其流程较为繁琐

2.使⽤用较为⽼老老式的 setnx⽅方法获取锁及expire⽅方法(⽆无法保证原⼦子操作) 

3.redis单点，⽆无法做到错误兼容性;
 更更好的⽅方法: 加锁和解锁使⽤用lua脚本保证原⼦子性(redis保证执⾏行行此脚本时不不执⾏行行其他操作)。

**redlock**

官方叫做 `RedLock` 算法，是 Redis 官方支持的分布式锁算法。这个锁的算法实现了了多Redis实例例的情况。相对于单Redis节点来说，防⽌止了了单节点⿎鼓掌造成整个服务停⽌止

运行的情况。

这个分布式锁有 3 个重要的考量点：

- 互斥（只能有一个客户端获取锁）
- 不能死锁
- 容错（只要大部分 Redis 节点创建了这把锁就可以）

为了了取到锁，客户端应该执⾏行行以下操作:

获取当前Unix时间，以毫秒为单位。
 依次尝试从5个实例例，使用相同的key和 具有唯⼀一性的value (例例如UUID)获取锁。当向Redis请求获 取锁时，客户端应该设置⼀一个⽹网络连接和响应超时时间，这个超时时间应该⼩小于锁的失效时间。例例 如你的锁⾃自动失效时间为10秒，则超时时间应该在5-50毫秒之间。这样可以避免服务器器端Redis已经 挂掉的情况下，客户端还在死死地等待响应结果。如果服务器器端没有在规定时间内响应，客户端应 该尽快尝试去另外⼀一个Redis实例例请求获取锁。 

客户端使⽤当前时间减去开始获取锁时间(步骤1记录的时间)就得到获取锁使⽤用的时间。 当且仅 当从⼤大多数 (N/2+1，这⾥里里是3个节点) 的Redis节点都取到锁，并且使⽤用的时间⼩小于锁失效时间 时，锁才算获取成功 。

 如果取到了了锁，key的真正有效时间等于有效时间减去获取锁所使⽤用的时间(步骤3计算的结果)。 

如果因为某些原因，获取锁失败(没有在⾄至少N/2+1个Redis实例例取到锁或者取锁时间已经超过了了有 效时间)，客户端应该在 所有的Redis实例例上进⾏行行解锁 (即便便某些Redis实例例根本就没有加锁成功， 防⽌止某些节点获取到锁但是客户端没有得到响应⽽而导致接下来的⼀一段时间不不能被重新获取锁)。

#### Redis 最普通的分布式锁

第一个最普通的实现方式，就是在 Redis 里使用 `SET key value [EX seconds] [PX milliseconds] NX` 创建一个 key，这样就算加锁。其中：

- `NX`：表示只有 `key` 不存在的时候才会设置成功，如果此时 redis 中存在这个 `key`，那么设置失败，返回 `nil`。
- `EX seconds`：设置 `key` 的过期时间，精确到秒级。意思是 `seconds` 秒后锁自动释放，别人创建的时候如果发现已经有了就不能加锁了。
- `PX milliseconds`：同样是设置 `key` 的过期时间，精确到毫秒级。

释放锁就是删除 key ，但是一般可以用 `lua` 脚本删除，判断 value 一样才删除：为啥要用 `random_value` 随机值呢？因为如果某个客户端获取到了锁，但是阻塞了很长时间才执行完，比如说超过了 30s，此时可能已经自动释放锁了，此时可能别的客户端已经获取到了这个锁，要是你这个时候直接删除 key 的话会有问题，所以得用随机值加上面的 `lua` 脚本来释放锁。

#### RedLock算法

有 5 个 Redis master 实例。然后执行如下步骤获取一把锁：

1. 获取当前时间戳，单位是毫秒；
2. 跟上面类似，轮流尝试在每个 master 节点上创建锁，超时时间较短，一般就几十毫秒（客户端为了获取锁而使用的超时时间比自动释放锁的总时间要小。例如，如果自动释放时间是 10 秒，那么超时时间可能在 `5~50` 毫秒范围内）；
3. 尝试在**大多数节点**上建立一个锁，比如 5 个节点就要求是 3 个节点 `n / 2 + 1` ；
4. 客户端计算建立好锁的时间，如果建立锁的时间小于超时时间，就算建立成功了；
5. 要是锁建立失败了，那么就依次之前建立过的锁删除；
6. 只要别人建立了一把分布式锁，你就得**不断轮询去尝试获取锁**。

### Redisson

Redisson是Java的redis客户端之一。 实现锁主要⽤用到的是RedissonLock这个类，其加锁/释放锁都是⽤用lua脚本完成的。

#### 和zk分布式锁对比

- redis 分布式锁，其实**需要自己不断去尝试获取锁**，比较消耗性能。
- zk 分布式锁，获取不到锁，注册个监听器即可，不需要不断主动尝试获取锁，性能开销较小。

另外一点就是，如果是 Redis 获取锁的那个客户端 出现 bug 挂了，那么只能等待超时时间之后才能释放锁；而 zk 的话，因为创建的是临时 znode，只要客户端挂了，znode 就没了，此时就自动释放锁。

Redis 分布式锁大家没发现好麻烦吗？遍历上锁，计算时间等等......zk 的分布式锁语义清晰实现简单。

## 为什么要用缓存（雪崩、穿透、击穿）

**高性能，高并发**

### 用了缓存的不良后果

缓存、数据库双写不一致

#### 缓存雪崩

缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，引起数据库压力过大甚至down机。和缓存击穿不同的是，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。

解决方案：

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210606232828058.png" alt="image-20210606232828058"  />

缓存雪崩的事前事中事后的解决方案如下：

- 事前：Redis 高可用，主从+哨兵，Redis cluster，避免全盘崩溃。
- 事中：本地 ehcache 缓存 + hystrix 限流&降级，避免 MySQL 被打死。
- 事后：Redis 持久化，一旦重启，自动从磁盘上加载数据，快速恢复缓存数据。

用户发送一个请求，系统 A 收到请求后，先查本地 ehcache 缓存，如果没查到再查 Redis。如果 ehcache 和 Redis 都没有，再查数据库，将数据库中的结果，写入 ehcache 和 Redis 中。

限流组件，可以设置每秒的请求，有多少能通过组件，剩余的未通过的请求，怎么办？**走降级**！可以返回一些默认的值，或者友情提示，或者空值。

好处：

- 数据库绝对不会死，限流组件确保了每秒只有多少个请求能通过。
- 只要数据库不死，就是说，对用户来说，2/5 的请求都是可以被处理的。
- 只要有 2/5 的请求可以被处理，就意味着你的系统没死，对用户来说，可能就是点击几次刷不出来页面，但是多点几次，就可以刷出来了。

缓存数据的过期时间设置随机，防止同一时间大量数据过期现象发生。
如果缓存数据库是分布式部署，将热点数据均匀分布在不同搞得缓存数据库中。
设置热点数据永远不过期。

#### 缓存穿透

缓存穿透是指缓存和数据库中都没有的数据，而用户不断发起请求，如发起为id为“-1”的数据或id为特别大不存在的数据，这时的用户很可能是攻击者，攻击会导致数据库压力过大。

解决方案：

每次系统 A 从数据库中只要没查到，就写一个空值到缓存里去，比如 `set -999 UNKNOWN` 。然后设置一个过期时间，这样的话，下次有相同的 key 来访问的时候，在缓存失效之前，都可以直接从缓存中取数据。

接口层增加校验，如用户鉴权校验，id做基础校验，id<=0的直接拦截；
从缓存取不到的数据，在数据库中也没有取到，这时也可以将key-value对写为key-null，缓存有效时间可以设置短点，如30秒（设置太长会导致正常情况也没法使用）。这样可以防止攻击用户反复用同一个id暴力攻击

#### 缓存击穿

缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期），这时由于并发用户特别多，同时读缓存没读到数据，又同时去数据库去取数据，引起数据库压力瞬间增大，造成过大压力

解决方案：

不同场景下的解决方式可如下：

- 若缓存的数据是基本不会发生更新的，则可尝试将该热点数据设置为永不过期。
- 若缓存的数据更新不频繁，且缓存刷新的整个流程耗时较少的情况下，则可以采用基于 Redis、zookeeper 等分布式中间件的分布式互斥锁，或者本地互斥锁以保证仅少量的请求能请求数据库并重新构建缓存，其余线程则在锁释放后能访问到新缓存。
- 若缓存的数据更新频繁或者在缓存刷新的流程耗时较长的情况下，可以利用定时线程在缓存过期前主动地重新构建缓存或者延后缓存的过期时间，以保证所有的请求能一直访问到对应的缓存。

设置热点数据永远不过期；加互斥锁，互斥锁

缓存并发竞争

## Redis和Memcached的区别

### redis支持复杂的数据结构

支持更丰富的操作

### redis原生支持集群

### 性能

Redis 只使用**单核**，而 Memcached 可以使用**多核**，所以平均每一个核上 Redis 在存储小数据时比 Memcached 性能更高。而在 100k 以上的数据中，Memcached 性能要高于 Redis

## Redis线程模型

Redis 内部使用文件事件处理器 `file event handler` ，这个文件事件处理器是单线程的，所以 Redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

- 多个 socket
- IO 多路复用程序
- 文件事件分派器
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

### 为什么单线程效率也高

1. 纯内存操作
2. 基于非阻塞的IO多路复用机制
3. C语言
4. 避免多线程频繁上下文切换

### 6.0引入多线程

**Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。**

Redis 选择使用单线程模型处理客户端的请求主要还是因为 CPU 不是 Redis 服务器的瓶颈，所以使用多线程模型带来的性能提升并不能抵消它带来的开发成本和维护成本，系统的性能瓶颈也主要在网络 I/O 操作上；而 Redis 引入多线程操作也是出于性能上的考虑，对于一些大键值对的删除操作，通过多线程非阻塞地释放内存空间也能减少对 Redis 主线程阻塞的时间，提高执行的效率。

## Redis 的过期策略、内存淘汰机制、手写一下 LRU 代码实现

常见问题：

1. 写入的数据怎么没了？写的多了，干掉不常用的数据。
2. 数据过期了怎么还占内存？Redis过期策略决定的

### 过期策略

### 手写LRU算法

```java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private int capacity;

    /**
     * 传递进来最多能缓存多少数据
     *
     * @param capacity 缓存大小
     */
    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }

    /**
     * 如果map中的数据量大于设定的最大容量，返回true，再新加入对象时删除最老的数据
     *
     * @param eldest 最老的数据项
     * @return true则移除最老的数据
     */
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        // 当 map中的数据量大于指定的缓存个数的时候，自动移除最老的数据
        return size() > capacity;
    }
}
```

## 如何保证 redis 的高并发和高可用？

redis 实现**高并发**主要依靠**主从架构**，一主多从，一般来说，很多项目其实就足够了，单主用来写入数据，单机几万 QPS，多从用来查询数据，多个从实例可以提供每秒 10w 的 QPS。

如果想要在实现高并发的同时，容纳大量的数据，那么就需要 redis 集群，使用 redis 集群之后，可以提供每秒几十万的读写并发。

redis 高可用，如果是做主从架构部署，那么加上哨兵就可以了，就可以实现，任何一个实例宕机，可以进行主备切换。

### 主从架构

一主多从，主负责写，并且将数据复制到其它的 slave 节点，从节点负责读。所有的**读请求全部走从节点**。这样也可以很轻松实现水平扩容，**支撑读高并发**。

#### Redis replication 的核心机制

- Redis 采用**异步方式**复制数据到 slave 节点，不过 Redis2.8 开始，slave node 会周期性地确认自己每次复制的数据量；
- 一个 master node 是可以配置多个 slave node 的；
- slave node 也可以连接其他的 slave node；
- slave node 做复制的时候，不会 block master node 的正常工作；
- slave node 在做复制的时候，也不会 block 对自己的查询操作，它会用旧的数据集来提供服务；但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了；
- slave node 主要用来进行横向扩容，做读写分离，扩容的 slave node 可以提高读的吞吐量。

注意，如果采用了主从架构，那么建议必须**开启** master node 的持久化，不建议用 slave node 作为 master node 的数据热备，因为那样的话，如果你关掉 master 的持久化，可能在 master 宕机重启的时候数据是空的，然后可能一经过复制， slave node 的数据也丢了。

另外，master 的各种备份方案，也需要做。万一本地的所有文件丢失了，从备份中挑选一份 rdb 去恢复 master，这样才能**确保启动的时候，是有数据的**，即使采用了高可用机制，slave node 可以自动接管 master node，但也可能 sentinel 还没检测到 master failure，master node 就自动重启了，还是可能导致上面所有的 slave node 数据被清空。

#### Redis 主从复制的核心原理

当启动一个 slave node 的时候，它会发送一个 `PSYNC` 命令给 master node。

如果这是 slave node 初次连接到 master node，那么会触发一次 `full resynchronization` 全量复制。此时 master 会启动一个后台线程，开始生成一份 `RDB` 快照文件，同时还会将从客户端 client 新收到的所有写命令缓存在内存中。 `RDB` 文件生成完毕后， master 会将这个 `RDB` 发送给 slave，slave 会先**写入本地磁盘，然后再从本地磁盘加载到内存**中，接着 master 会将内存中缓存的写命令发送到 slave，slave 也会同步这些数据。slave node 如果跟 master node 有网络故障，断开了连接，会自动重连，连接之后 master node 仅会复制给 slave 部分缺少的数据。

[![Redis-master-slave-replication](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/redis-master-slave-replication.png)](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/images/redis-master-slave-replication.png)

**主从复制的断点续传**

从 Redis2.8 开始，就支持主从复制的断点续传，如果主从复制过程中，网络连接断掉了，那么可以接着上次复制的地方，继续复制下去，而不是从头开始复制一份。

master node 会在内存中维护一个 backlog，master 和 slave 都会保存一个 replica offset 还有一个 master run id，offset 就是保存在 backlog 中的。如果 master 和 slave 网络连接断掉了，slave 会让 master 从上次 replica offset 开始继续复制，如果没有找到对应的 offset，那么就会执行一次 `resynchronization` 。

> 如果根据 host+ip 定位 master node，是不靠谱的，如果 master node 重启或者数据出现了变化，那么 slave node 应该根据不同的 run id 区分。

**无磁盘化复制**

master 在内存中直接创建 `RDB` ，然后发送给 slave，不会在自己本地落地磁盘了。只需要在配置文件中开启 `repl-diskless-sync yes` 即可。

```
repl-diskless-sync yes

# 等待 5s 后再开始复制，因为要等更多 slave 重新连接过来
repl-diskless-sync-delay 5
```

**过期 key 处理**

slave 不会过期 key，只会等待 master 过期 key。如果 master 过期了一个 key，或者通过 LRU 淘汰了一个 key，那么会模拟一条 del 命令发送给 slave。

#### 复制的完整流程

slave node 启动时，会在自己本地保存 master node 的信息，包括 master node 的 `host` 和 `ip` ，但是复制流程没开始。

slave node 内部有个定时任务，每秒检查是否有新的 master node 要连接和复制，如果发现，就跟 master node 建立 socket 网络连接。然后 slave node 发送 `ping` 命令给 master node。如果 master 设置了 requirepass，那么 slave node 必须发送 masterauth 的口令过去进行认证。master node **第一次执行全量复制**，将所有数据发给 slave node。而在后续，master node 持续将写命令，异步复制给 slave node。

[![Redis-master-slave-replication-detail](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/redis-master-slave-replication-detail.png)](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/images/redis-master-slave-replication-detail.png)

**全量复制**

- master 执行 bgsave ，在本地生成一份 rdb 快照文件。
- master node 将 rdb 快照文件发送给 slave node，如果 rdb 复制时间超过 60 秒（repl-timeout），那么 slave node 就会认为复制失败，可以适当调大这个参数(对于千兆网卡的机器，一般每秒传输 100MB，6G 文件，很可能超过 60s)
- master node 在生成 rdb 时，会将所有新的写命令缓存在内存中，在 slave node 保存了 rdb 之后，再将新的写命令复制给 slave node。
- 如果在复制期间，内存缓冲区持续消耗超过 64MB，或者一次性超过 256MB，那么停止复制，复制失败。

```
client-output-buffer-limit slave 256MB 64MB 60
```

- slave node 接收到 rdb 之后，清空自己的旧数据，然后重新加载 rdb 到自己的内存中，同时**基于旧的数据版本**对外提供服务。
- 如果 slave node 开启了 AOF，那么会立即执行 BGREWRITEAOF，重写 AOF。

**增量复制**

- 如果全量复制过程中，master-slave 网络连接断掉，那么 slave 重新连接 master 时，会触发增量复制。
- master 直接从自己的 backlog 中获取部分丢失的数据，发送给 slave node，默认 backlog 就是 1MB。
- master 就是根据 slave 发送的 psync 中的 offset 来从 backlog 中获取数据的。

**heartbeat**

主从节点互相都会发送 heartbeat 信息。

master 默认每隔 10 秒发送一次 heartbeat，slave node 每隔 1 秒发送一个 heartbeat。

**异步复制**

master 每次接收到写命令之后，先在内部写入数据，然后异步发送给 slave node。

### Redis 哨兵集群实现高可用

- 集群监控：负责监控 Redis master 和 slave 进程是否正常工作。
- 消息通知：如果某个 Redis 实例有故障，那么哨兵负责发送消息作为报警通知给管理员。
- 故障转移：如果 master node 挂掉了，会自动转移到 slave node 上。
- 配置中心：如果故障转移发生了，通知 client 客户端新的 master 地址。

哨兵用于实现 Redis 集群的高可用，本身也是分布式的，作为一个哨兵集群去运行，互相协同工作。

- 故障转移时，判断一个 master node 是否宕机了，需要大部分的哨兵都同意才行，涉及到了分布式选举的问题。
- 即使部分哨兵节点挂掉了，哨兵集群还是能正常工作的，因为如果一个作为高可用机制重要组成部分的故障转移系统本身是单点的，那就很坑爹了。

#### 导致数据丢失的两种情况

- 异步复制导致的数据丢失

因为 master->slave 的复制是异步的，所以可能有部分数据还没复制到 slave，master 就宕机了，此时这部分数据就丢失了。

- 脑裂导致的数据丢失

脑裂，也就是说，某个 master 所在机器突然**脱离了正常的网络**，跟其他 slave 机器不能连接，但是实际上 master 还运行着。此时哨兵可能就会**认为** master 宕机了，然后开启选举，将其他 slave 切换成了 master。这个时候，集群里就会有两个 master ，也就是所谓的**脑裂**。

此时虽然某个 slave 被切换成了 master，但是可能 client 还没来得及切换到新的 master，还继续向旧 master 写数据。因此旧 master 再次恢复的时候，会被作为一个 slave 挂到新的 master 上去，自己的数据会清空，重新从新的 master 复制数据。而新的 master 并没有后来 client 写入的数据，因此，这部分数据也就丢失了。

如果说一旦所有的 slave，数据复制和同步的延迟都超过了 10 秒钟，那么这个时候，master 就不会再接收任何请求了。

**解决方案**

```
min-slaves-to-write 1
min-slaves-max-lag 10
```

- 减少异步复制数据的丢失

有了 `min-slaves-max-lag` 这个配置，就可以确保说，一旦 slave 复制数据和 ack 延时太长，就认为可能 master 宕机后损失的数据太多了，那么就拒绝写请求，这样可以把 master 宕机时由于部分数据未同步到 slave 导致的数据丢失降低的可控范围内。

- 减少脑裂的数据丢失

如果一个 master 出现了脑裂，跟其他 slave 丢了连接，那么上面两个配置可以确保说，如果不能继续给指定数量的 slave 发送数据，而且 slave 超过 10 秒没有给自己 ack 消息，那么就直接拒绝客户端的写请求。因此在脑裂场景下，最多就丢失 10 秒的数据。

#### sdown 和 odown 转换机制

- sdown 是主观宕机，就一个哨兵如果自己觉得一个 master 宕机了，那么就是主观宕机
- odown 是客观宕机，如果 quorum 数量的哨兵都觉得一个 master 宕机了，那么就是客观宕机

sdown 达成的条件很简单，如果一个哨兵 ping 一个 master，超过了 `is-master-down-after-milliseconds` 指定的毫秒数之后，就主观认为 master 宕机了；如果一个哨兵在指定时间内，收到了 quorum 数量的其它哨兵也认为那个 master 是 sdown 的，那么就认为是 odown 了。

#### slave->master 选举算法

如果一个 master 被认为 odown 了，而且 majority 数量的哨兵都允许主备切换，那么某个哨兵就会执行主备切换操作，此时首先要选举一个 slave 来，会考虑 slave 的一些信息：

- 跟 master 断开连接的时长
- slave 优先级
- 复制 offset
- run id

如果一个 slave 跟 master 断开连接的时间已经超过了 `down-after-milliseconds` 的 10 倍，外加 master 宕机的时长，那么 slave 就被认为不适合选举为 master。

```
(down-after-milliseconds * 10) + milliseconds_since_master_is_in_SDOWN_state
```

接下来会对 slave 进行排序：

- 按照 slave 优先级进行排序，slave priority 越低，优先级就越高。
- 如果 slave priority 相同，那么看 replica offset，哪个 slave 复制了越多的数据，offset 越靠后，优先级就越高。
- 如果上面两个条件都相同，那么选择一个 run id 比较小的那个 slave。

## Redis 集群模式的工作原理能说一下么？了解一致性 hash 算法吗？

**redis cluster 介绍**

- 自动将数据进行分片，每个 master 上放一部分数据
- 提供内置的高可用支持，部分 master 不可用时，还是可以继续工作的

在 Redis cluster 架构下，每个 Redis 要放开两个端口号，比如一个是 6379，另外一个就是 加 1w 的端口号，比如 16379。

**节点间的内部通信机制**

基本通信原理

集群元数据的维护有两种方式：集中式、Gossip 协议。Redis cluster 节点间采用 gossip 协议进行通信。

**集中式**是将集群元数据（节点信息、故障等等）集中存储在某个节点上。集中式元数据集中存储的一个典型代表，就是大数据领域的 `storm` 。它是分布式的大数据实时计算引擎，是集中式的元数据存储的结构，底层基于 zookeeper（分布式协调的中间件）对所有元数据进行存储维护。

**集中式**的**好处**在于，元数据的读取和更新，时效性非常好，一旦元数据出现了变更，就立即更新到集中式的存储中，其它节点读取的时候就可以感知到；**不好**在于，所有的元数据的更新压力全部集中在一个地方，可能会导致元数据的存储有压力。

gossip 好处在于，元数据的更新比较分散，不是集中在一个地方，更新请求会陆陆续续打到所有节点上去更新，降低了压力；不好在于，元数据的更新有延时，可能导致集群中的一些操作会有一些滞后。

### 分布式寻址算法

- hash 算法（大量缓存重建）
- 一致性 hash 算法（自动缓存迁移）+ 虚拟节点（自动负载均衡）
- Redis cluster 的 hash slot 算法

**一致性 hash 算法**

一致性 hash 算法将整个 hash 值空间组织成一个虚拟的圆环，整个空间按顺时针方向组织，下一步将各个 master 节点（使用服务器的 ip 或主机名）进行 hash。这样就能确定每个节点在其哈希环上的位置。

来了一个 key，首先计算 hash 值，并确定此数据在环上的位置，从此位置沿环**顺时针“行走”**，遇到的第一个 master 节点就是 key 所在位置。

在一致性哈希算法中，如果一个节点挂了，受影响的数据仅仅是此节点到环空间前一个节点（沿着逆时针方向行走遇到的第一个节点）之间的数据，其它不受影响。增加一个节点也同理。

燃鹅，一致性哈希算法在节点太少时，容易因为节点分布不均匀而造成**缓存热点**的问题。为了解决这种热点问题，一致性 hash 算法引入了虚拟节点机制，即对每一个节点计算多个 hash，每个计算结果位置都放置一个虚拟节点。这样就实现了数据的均匀分布，负载均衡。

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210606235800092.png" alt="image-20210606235800092" style="zoom:67%;" />

## 如何保证缓存与数据库的双写一致性？

最好不要做这个方案，即：**读请求和写请求串行化**，串到一个**内存队列**里去。

### Cache Aside Pattern

最经典的缓存+数据库读写的模式，就是 Cache Aside Pattern。

- 读的时候，先读缓存，缓存没有的话，就读数据库，然后取出数据后放入缓存，同时返回响应。
- 更新的时候，**先更新数据库，然后再删除缓存**。

问题：先更新数据库，再删除缓存。如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据就出现了不一致。

解决思路：先删除缓存，再更新数据库。如果数据库更新失败了，那么数据库中是旧数据，缓存中是空的，那么数据不会不一致。因为读的时候缓存没有，所以去读了数据库中的旧数据，然后更新到缓存中。

还是有问题，怎么办？

更新数据的时候，根据**数据的唯一标识**，将操作路由之后，发送到一个 jvm 内部队列中。读取数据的时候，如果发现数据不在缓存中，那么将重新执行“读取数据+更新缓存”的操作，根据唯一标识路由之后，也发送到同一个 jvm 内部队列中。

一个队列对应一个工作线程，每个工作线程**串行**拿到对应的操作，然后一条一条的执行。这样的话，一个数据变更的操作，先删除缓存，然后再去更新数据库，但是还没完成更新。此时如果一个读请求过来，没有读到缓存，那么可以先将缓存更新的请求发送到队列中，此时会在队列中积压，然后同步等待缓存更新完成。

**如果一个内存队列中可能积压的更新操作特别多**，那么你就要**加机器**，让每个机器上部署的服务实例处理更少的数据，那么每个内存队列中积压的更新操作就会越少。

## Redis 的并发竞争问题是什么？如何解决这个问题？了解 Redis 事务的 CAS 方案吗？

 Redis 自己就有天然解决这个问题的 CAS 类的乐观锁方案.

某个时刻，多个系统实例都去更新某个 key。可以基于 zookeeper 实现分布式锁。每个系统通过 zookeeper 获取分布式锁，确保同一时间，只能有一个系统实例在操作某个 key，别人都不允许读和写。

[![zookeeper-distributed-lock](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/zookeeper-distributed-lock.png)](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/images/zookeeper-distributed-lock.png)

你要写入缓存的数据，都是从 mysql 里查出来的，都得写入 mysql 中，写入 mysql 中的时候必须保存一个时间戳，从 mysql 查出来的时候，时间戳也查出来。

每次要**写之前，先判断**一下当前这个 value 的时间戳是否比缓存里的 value 的时间戳要新。如果是的话，那么可以写，否则，就不能用旧的数据覆盖新的数据。

## 生产环境中的 Redis 是怎么部署的？

一般线上生产环境，Redis 的内存尽量不要超过 10g，超过 10g 可能会有问题

## Redis rehash 的过程

Redis 主要用于存储键值对(`Key-Value Pair`)，而键值对的存储方式是由字典实现，而 Redis 中字典的底层又是通过哈希表来实现的

Redis 中字典的数据结构如下：

```c
// 字典对应的数据结构，有关hash表的结构可以参考redis源码，再次就不进行描述
typedef struct dict {
    dictType *type;  // 字典类型
    void *privdata;  // 私有数据
    dictht ht[2];    // 2个哈希表，这也是进行rehash的重要数据结构，从这也看出字典的底层通过哈希表进行实现。
    long rehashidx;   // rehash过程的重要标志，值为-1表示rehash未进行
    int iterators;   //  当前正在迭代的迭代器数
} dict;
```

### 渐进式 rehash

rehash 过程在数据量非常大（几千万、亿）的情况下并不是一次性地完成的，而是**渐进式地**完成的。**渐进式 rehash**的好处在于避免对服务器造成影响。

渐进式 rehash 的本质：

1. 借助 rehashidx，将 rehash 键值对所需的计算工作均摊到对字典的每个添加、删除、查找和更新操作上，从而避免了集中式 rehash 而带来的庞大计算量。
2. 在 rehash 进行期间，每次对字典执行添加、删除、查找或者更新操作时，程序除了执行指定的操作以外，还会顺带将原哈希表在 rehashidx 索引上的所有键值对 rehash 到备用哈希表，当 rehash 工作完成之后，程序将 rehashidx 属性的值加 1。

# 8. Spring

## 基础问答

#### Spring 主要提供了哪些模块?

1. core模块提供IOC和DI等核心功能
2. aop模块提供面向切面编程的实现
3. web模块提供对web应用的支持
4. dao模块提供数据库方面的支持
5. test模块提供测试方面的支持

#### Spring主要使用了哪些设计模式?

1. 工厂模式 BeanFactory 就是简单工厂的实现，用来创建和获取Bean
2. 单例模式 Spring容器的Bean默认是单例的
3. 代理模式 aop使用的就是代理模式
4. 模板方法模式 jdbcTemplate等就使用模板方法模式
5. 观察者模式 当有事件触发时，该事件的监听者(观察者)就会做出相应的动作。

#### BeanFactory和ApplicationContext的区别是什么?

BeanFactory是最底层，最顶级的IOC容器接口，它提供了对bean的基本操作，属于低级容器。 而ApplicationContext是BeanFactory的应用扩展接口， 提供了比BeanFactory更多高级的功能和扩展接口，属于高级容器。

#### Spring依赖注入的方式有几种?

1. setter方法注入: 通过构造器或工厂方法(静态工厂方法或实例bean工厂方法)构造bean所需要的依赖后， 使用setter方法设置bean的依赖。
2. 构造器注入: 构造器的每个参数都可以代表对其他bean的依赖。

Spring 中触发 IoC 容器“依赖注入” 的方式有两种，一个是应用程序通过 getBean()方法 向容器索要 bean 实例 时触发依赖注入；另一个是提前给 bean 配置了 lazy-init 属性为 false，Spring 在 IoC 容器 初始化会自动调用此 bean 的 getBean() 方法，提前完成依赖注入。总的来说，想提高运行时获取 bean 的效率，可以考虑配置此属性。

#### 一个bean的定义包含了什么?(BeanDefinition)

BeanDefinition 是对Bean的定义，它定义了Bean的元数据， 如Bean的Scope，Class，beanName，bean的实例化方式等等。

#### bean的作用域有哪些?

- 单例
- 原型
- 请求域
- 会话域
- web应用作用域
- websocket作用域

#### Spring 的扩展点主要有哪些?

> 究其根本，还是在bean实例化/初始化前后的扩展。

#### Spring如何解决循环依赖?

- 构造器注入的循环依赖无法解决，直接抛出BeanCurrentlyInCreationException异常。

- Setter方法注入的循环依赖可以通过缓存解决。 三级缓存：
  1. 初始化完成的Bean池。
  2. 实例化完成，但是没有填充属性的Bean池。
  3. 刚刚实例化完成的Bean的工厂缓存，用于提前曝光Bean。

**因为Setter方法注入的前提是首先需要实例化这个对象，而构造器注入的参数正是bean， 怎么实例化，所以无法解决这个问题。**

- 非单例bean不能缓存，无法解决循环依赖: IOC容器是不会缓存非单例bean的，所以无法解决循环依赖问题。

## Spring事务的传播行为

### @transactional

实现原理：将对应的方法通过注解元数据，标注在业务方法或者所在的对象上，然后在业务执行期间，通过AOP拦截器反射读取元数据信息，最终将根据读取的业务信息构建事务管理支持。

不同的方法之间的事务传播保证在同一个事务内，是通过统一的数据源来实现的，事务开始时将数据源绑定到ThreadLocal中，后续加入的事务从ThreadLocal获取数据源来保证数据源的统一。

7种：

1. REQUIRED: 当一个方法A(REQUIRED)被另一个方法B调用时，如果B已经开启了事务，那么A就加入到B的事务中去， A和B要么同时成功，要么同时失败。如果B没有开启事务，那么A将自己开启新的事务,独立运行。
2. REQUIRES_NEW: 当一个方法A(REQUIRE_NEW)被非事务方法调用时，A会自己开启事务。 如果A被另一个方法B调用时，无论B是否开启事务，A都会开启自己的事务，且会挂起B的事务， 然后执行A，A提交后才会恢复B，这样外层的B即使失败也不会影响A，但是如果A失败了，B也会回滚。
3. NESTED: 如果方法A(NESTED)被另一个方法B调用，如果B已经开启了事务， 那么A将作为B的子事务运行，B失败，A也失败，但是A失败却不影响B。A是作为B的嵌套事务运行的， 所以并不会影响B。如果B没有事务，那么A将自己新开启一个事务运行。 **PS:NESTED和REQUIRES_NEW很相似，但是可以理解为他们是相反的，NESTED的方法作为嵌套事务运行在 外层事务内部，外层事务失败则内层事务也失败，但内层事务失败却不影响外层事务；REQUIRES_NEW则是 自己开启自己的事务，外层事务失败也不影响内层事务，但内层事务失败会导致外层事务回滚。**
4. SUPPORTS: 当一个方法A(SUPPORTS)被另一个方法B调用时，如果B开启了事务， 那么A就加入到B的事务中去。如果B没有开启事务,那就以非事务方式运行执行。
5. MANDATORY: 当一个方法A(MANDATORY)被另一个方法B调用时，如果B没有开启事务，那么A将抛出异常， 如果B开启了事务，则A加入到B中共用事务。
6. NOT_SUPPORTED: 当一个方法A(NOT_SUPPORTED)被另一个方法B调用时，如果B开启了事务，那么B的事务将挂起， 直到A执行完，B再以事务的方式运行。
7. NEVER: 当一个方法A(NEVER)被另一个方法B调用时，如果B开启了事务，那么A将抛出异常。

## IOC原理

核心问题是：

1. 谁负责创建组件？
2. 谁负责根据依赖关系组装组件？
3. 销毁时，如何按依赖顺序正确销毁？

却带来了一系列好处：

1. `BookService`不再关心如何创建`DataSource`，因此，不必编写读取数据库配置之类的代码；
2. `DataSource`实例被注入到`BookService`，同样也可以注入到`UserService`，因此，共享一个组件非常简单；
3. 测试`BookService`更容易，因为注入的是`DataSource`，可以使用内存数据库，而不是真实的MySQL配置。

因此，IoC又称为依赖注入（DI：Dependency Injection），它解决了一个最主要的问题：将组件的创建+配置与组件的使用相分离，并且，由IoC容器负责管理组件的生命周期。

因为IoC容器要负责实例化所有的组件，因此，有必要告诉容器如何创建组件，以及各组件的依赖关系。一种最简单的配置是通过XML文件来实现，例如：

```xml
<beans>
    <bean id="dataSource" class="HikariDataSource" />
    <bean id="bookService" class="BookService">
        <property name="dataSource" ref="dataSource" />
    </bean>
    <bean id="userService" class="UserService">
        <property name="dataSource" ref="dataSource" />
    </bean>
</beans>
```

上述XML配置文件指示IoC容器创建3个JavaBean组件，并把id为`dataSource`的组件通过属性`dataSource`（即调用`setDataSource()`方法）注入到另外两个组件中。

在Spring的IoC容器中，我们把所有组件统称为JavaBean，即配置一个组件就是配置一个Bean。

### **依赖注入方式**

我们从上面的代码可以看到，依赖注入可以通过`set()`方法实现。但依赖注入也可以通过构造方法实现。

很多Java类都具有带参数的构造方法，如果我们把`BookService`改造为通过构造方法注入，那么实现代码如下：

```
public class BookService {
    private DataSource dataSource;

    public BookService(DataSource dataSource) {
        this.dataSource = dataSource;
    }
}
```

Spring的IoC容器同时支持属性注入和构造方法注入，并允许混合使用。

### 反射

Spring到底是怎么运行的

```java
1 public static void main(String[] args) {   
2        ApplicationContext context = new FileSystemXmlApplicationContext(   
3                "applicationContext.xml");   
4        Animal animal = (Animal) context.getBean("animal");   
5        animal.say();   
6    } 

```

这段代码你一定很熟悉吧，不过还是让我们分析一下它吧，首先是applicationContext.xml

```xml
1 <bean id="animal" class="phz.springframework.test.Cat">   
2        <property name="name" value="kitty" />   
3    </bean>

```

他有一个类phz.springframework.test.Cat

```java
1 public class Cat implements Animal {   
2    private String name;   
3    public void say() {   
4        System.out.println("I am " + name + "!");   
5    }   
6    public void setName(String name) {   
7        this.name = name;   
8    }   
9 }

```

很明显上面的代码输出I am kitty! 那么到底Spring是如何做到的呢？ 接下来就让我们自己写个Spring 来看看Spring 到底是怎么运行的吧！ 首先，我们定义一个Bean类，这个类用来存放一个Bean拥有的属性

```java
1/* Bean Id */  
2    private String id;   
3    /* Bean Class */  
4    private String type;   
5    /* Bean Property */  
6    private Map<String, Object> properties = new HashMap<String, Object>();
一个Bean包括id,type,和Properties。

```

接下来Spring 就开始加载我们的配置文件了，将我们配置的信息保存在一个HashMap中，HashMap的key就是Bean 的 Id ，HasMap 的value是这个Bean，只有这样我们才能通过context.getBean("animal")这个方法获得Animal这个类。我们都知道Spirng可以注入基本类型，而且可以注入像List，Map这样的类型，接下来就让我们以Map为例看看Spring是怎么保存的吧

Spring 到底是怎么依赖注入的吧，其实依赖注入的思想也很简单，它是通过反射机制实现的，在实例化一个类时，它通过反射调用类中set方法将事先保存在HashMap中的类属性注入到类中。让我们看看具体它是怎么做的吧。 首先实例化一个类，

```java
 1public static Object newInstance(String className) {   
 2        Class<?> cls = null;   
 3        Object obj = null;   
 4        try {   
 5            cls = Class.forName(className);   
 6            obj = cls.newInstance();   
 7        } catch (ClassNotFoundException e) {   
 8            throw new RuntimeException(e);   
 9        } catch (InstantiationException e) {   
10            throw new RuntimeException(e);   
11        } catch (IllegalAccessException e) {   
12            throw new RuntimeException(e);   
13        }   
14        return obj;   
15    }  

```

接着它将这个类的依赖注入进去，像这样

```java
 1public static void setProperty(Object obj, String name, String value) {   
 2        Class<? extends Object> clazz = obj.getClass();   
 3        try {   
 4            String methodName = returnSetMthodName(name);   
 5            Method[] ms = clazz.getMethods();   
 6            for (Method m : ms) {   
 7                if (m.getName().equals(methodName)) {   
 8                    if (m.getParameterTypes().length == 1) {   
 9                        Class<?> clazzParameterType = m.getParameterTypes()[0];   
10                        setFieldValue(clazzParameterType.getName(), value, m,   
11                                obj);   
12                        break;   
13                    }   
14                }   
15            }   
16        } catch (SecurityException e) {   
17            throw new RuntimeException(e);   
18        } catch (IllegalArgumentException e) {   
19            throw new RuntimeException(e);   
20        } catch (IllegalAccessException e) {   
21            throw new RuntimeException(e);   
22        } catch (InvocationTargetException e) {   
23            throw new RuntimeException(e);   
24        }   
25}  

```

最后它将这个类的实例返回给我们，我们就可以用了。我们还是以Map为例看看它是怎么做的，我写的代码里面是创建一个HashMap并把该HashMap注入到需要注入的类中，像这样：

```java
 1if (value instanceof Map) {   
 2                Iterator<?> entryIterator = ((Map<?, ?>) value).entrySet()   
 3                        .iterator();   
 4                Map<String, Object> map = new HashMap<String, Object>();   
 5                while (entryIterator.hasNext()) {   
 6                    Entry<?, ?> entryMap = (Entry<?, ?>) entryIterator.next();   
 7                    if (entryMap.getValue() instanceof String[]) {   
 8                        map.put((String) entryMap.getKey(),   
 9                                getBean(((String[]) entryMap.getValue())[0]));   
10                    }   
11                }   
12                BeanProcesser.setProperty(obj, property, map);   
13            }  

```







### Autowired和Resource区别

@Autowired注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以结合@Qualifier注解一起使用。(通过类型匹配找到多个candidate,在没有@Qualifier、@Primary注解的情况下，会使用对象名作为最后的fallback匹配)

@Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不制定name也不制定type属性，这时将通过反射机制使用byName自动注入策略。

@Resource装配顺序：

①如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常。

②如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常。

③如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会抛出异常。

④如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。

@Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入。

## 什么是AOP?

AOP(Aspect Oriented Programming)面向切面编程。将系统的核心逻辑和辅助逻辑分离开来，并将通用的辅助逻辑封装成一个模块， 提高了代码的重用性和程序的可维护性，降低了系统模块之间的耦合度

### AOP的组成元素和概念有哪些?

1. 连接点(join point): 连接点指程序执行的某个位置，能够执行辅助逻辑(通知/增强)。 如方法执行前，方法抛出异常时，方法执行完，方法返回后等等，这些点都被称为连接点。
2. 通知/增强(advice): 通知/增强 可以理解为辅助逻辑，就是在连接点要做的事情。
3. 切点(pointcut): 连接点可以看做是一个方法的执行辅助逻辑的不同位置的集合， 切入点指的就是这个方法。切入点会匹配 通知/增强 需要作用的类或方法。
4. 切面(aspect): 切面是切入点的集合，可以看作是拥有多个切入点的类。
5. 织入(weave): 织入是一个概念。它描述的是将切入点的 通知/增强 应用到连接点的过程。

### AOP实现方式有哪些?

常见的AOP实现的方式有代理和织入。

- 代理分为静态代理和动态代理。 由于静态代理没有动态代理灵活，所以现在几乎都使用动态代理来实现AOP。 以动态代理实现AOP的框架主要有cglib和jdk原生的这2种。
- 织入可以理解为以操作字节码的方式对class源文件进行修改，从而实现通知/增强。 以织入实现AOP的框架主要有AspectJ。

### AspectJ AOP 和 Spring AOP的区别?

- AspectJ AOP: AspectJ是一整套AOP的工具，它提供了切面语法(切入点表达式)以及织入等强大的功能。 Aspect提供文件和注解2种方式来进行AOP编程， 并且它允许在编译时，编译后和加载时织入， 但是需要使用它特定的ajc编译器才能实现织入这一功能。
- Spring AOP: Spring AOP吸收了AspectJ的优点，采用了AspectJ的切入点语法以及AspectJ式的注解， 但却并未使用AspectJ的一整套工具, 而是集cglib和jdk于一体(动态代理)的方式来实现AOP功能，真的很强。

### cglib动态代理和jdk动态代理的区别?

- jdk动态代理: jdk只提供基于接口式的动态代理来对目标进行增强。

一般会使用实现了 InvocationHandler接口 的类作为代理对象的生产工厂，

并且通过持有 被代理对象target，来在 invoke()方法 中对被代理对象的目标方法进行调用和增强，

```java
@Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {

        Object ret = null;
        System.out.println("前置增强");
        ret = method.invoke(target, args);
        System.out.println("后置增强");
        return ret;
    }
    
```

总的来说，就是在 invoke()方法 中完成 target目标方法 的调用，及前置后置增强，
JDK 动态生成的代理类中对 invoke()方法 进行了回调

- cglib动态代理: cglib则是使用字节码技术，动态生成目标的子类，以继承的方式来对目标方法进行重写， 所以如果方法是final的，那么cglib将无法对方法进行增强。 **在SpringAOP 中，如果目标类实现了接口，那么默认使用jdk动态代理来实现AOP， 如果目标类没有实现接口，那么将使用cglib来实现AOP。**

创建并配置 Enhancer对象，Enhancer 是 CGLIB 中主要的操作类，通过 enhancer 生成代理对象

为 目标对象 target 生成 代理对象 之后，在调用 代理对象 的目标方法时，目标方法会进行 invoke()回调（JDK 动态代理） 或 callbacks()回调（CGLIB），然后就可以在回调方法中对目标对象的目标方法进行拦截和增强处理了。

JDK Proxy 的优势：最小化依赖关系，减少依赖意味着简化开发和维护，JDK 本身的支持，可能比 cglib 更加可靠。平滑进行 JDK 版本升级，而字节码类库通常需要进行更新以保证在新版 Java 上能够使用。代码实现简单。

基于类似 cglib 框架的优势：有的时候调用目标可能不便实现额外接口，从某种角度看，限定调用者实现接口是有些侵入性的实践，类似 cglib 动态代理就没有这种限制。只操作我们关心的类，而不必为其他相关类增加工作量。高性能。

## Spring MVC

### 什么是DispatcherServlet?

一个Servlet，负责拦截所有的请求，并以调用各种组件来对请求进行分发并处理。

### Spring MVC有哪些组件?(见:DispatcherServlet源码)

1. MultipartResolver: 核心组件之一，处理文件上传请求。MultipartResolver负责判断普通请求是否为文件上传请求， 并将普通请求(HttpServletRequest)解析为文件上传请求(MultipartHttpServletRequest)。
2. LocalResolver: 区域解析器。它主要被用于国际化的资源方面的解析。
3. ThemeResolver: 主题资源解析器。SpringMVC允许用户提供不同主题，主题就是一系列资源的集合使用这些主题可以提高用户体验。
4. ViewResolver: 核心组件之一，视图解析器。在Handler执行完请求后，ViewResolver将ModelAndView解析成物理视图， 并对物理视图进行model渲染。
5. HandlerMapping: 核心组件之一，请求处理器。根据用户的请求来匹配对应的Handler。
6. HandlerAdapter: 核心组件之一，Handler适配器。使用Handler处理请求，并返回处理后的视图(ModelAndView)。
7. HandlerExceptionResolver: 核心组件之一，异常处理解析器。在Handler执行请求的过程中可能出现异常， HandlerExceptionResolver就负责处理Handler执行请求过程中的异常。
8. RequestToViewNameTranslator: 核心组件之一,请求到视图的转换器。根据Request设置最终的视图名,当Handler执行完请求后，它会将Request解析成视图名。
9. FlashMapManager: 核心组件之一，在请求进行重定向时，FlashMapManager用于保存请求中的参数。

### 简述SpringMVC原理/执行流程

1. 用户发出的请求被DispatcherServlet拦截。
2. DispatcherServlet使用HandlerMapping根据请求匹配到相应的Handler。Handler实际上是一个(HandlerMethod)。
3. DispatcherServlet根据Handler适配合适的HandlerAdapter。
4. HandlerAdapter使用Handler执行请求，并返回ModelAndView。
5. 使用RequestToViewNameTranslator,HandlerExceptionResolver和ViewResolver等解析器， 解析并渲染ModelAndView，并处理相关异常信息。
6. 渲染后的结果反馈给用户。

### Spring MVC 拦截器是什么 / 有什么作用 / 与 Filter有什么区别?

- HandlerInterceptor: Spring MVC拦截器是Spring MVC提供的对用户请求的目标资源做出拦截扩展的处理器。 它允许在目标方法执行前后以及View渲染后做出处理。
- Servlet Filter: Filter是Servlet提供的过滤器，它会在目标方法执行前后做出拦截处理。

## Spring源码分析

SpringMVC通过一个DispatcherServlet拦截所有请求,也就是url为 /** 。 通过拦截所有请求,在内部通过路由匹配的方式把请求转给对应Controller的某个RequestMapping处理。 这就是SpringMVC的基本工作流程,我们传统的JavaWeb是一个Servlet对应一个URL, 而DispatcherServlet是一个Servlet拦截全部URL,并做分发处理. 这也是SpringMVC的设计的精妙之处。

肯定有一个意识:在SpringMVC框架中,所有的核心方法几乎都是do开头, 并且都有2个必要的参数:HttpServletRequest和HttpServletResponse

DispatcherServlet的doDispatch方法：

九大组件：

```java
public class DispatcherServlet extends FrameworkServlet {
    ....
    @Nullable
    private MultipartResolver multipartResolver;
    @Nullable
    private LocaleResolver localeResolver;
    @Nullable
    private ThemeResolver themeResolver;
    @Nullable
    private List<HandlerMapping> handlerMappings;
    @Nullable
    private List<HandlerAdapter> handlerAdapters;
    @Nullable
    private List<HandlerExceptionResolver> handlerExceptionResolvers;
    @Nullable
    private RequestToViewNameTranslator viewNameTranslator;
    @Nullable
    private FlashMapManager flashMapManager;
    @Nullable
    private List<ViewResolver> viewResolvers;
    ....
   }
```

SpringMVC工作的流程:

SpringMVC通过DispatcherServlet拦截所有的请求, 并通过HandlerMapping与指定的请求找出匹配的handler, handler实际是HandlerMethod对象。 再通过与handler适配的HandlerAdapter执行目标方法, 执行完目标方法后会返回ModelAndView对象, 最后通过ViewResolver解析ModelAndView的View视图。

## Springboot

#### SpringBoot的核心注解是哪个?

核心注解自然是启动SpringBoot应用的那个注解啦:@SpringBootApplication。

它由

```text
@SpringBootConfiguration(@Configuration)

@EnableAutoConfiguration

@ComponentScan
```

3个核心注解组成。

- @SpringBootConfiguration等同于@Configuration,它声明一个类为配置类。
- @EnableAutoConfiguration是SpringBoot自动配置的核心注解，没有它， SpringBoot的自动配置就不会生效
- @ComponentScan 容器组件扫描的注解，负责扫描所有的容器组件，也是最核心的注解之一。

#### Java API配置的好处

Java配置灵活，编写简单，修改方便

XML优点：通俗易懂；缺点：繁琐

#### SpringBoot自动配置原理

**@EnableAutoConfiguration使得容器会读取类路径下 META-INF/spring.factories 文件， 并导入文件中声明的自动配置类。** spring.factories里定义的都是扩展组件的自动配置类, 这些配置类往往都会依赖于XXXProperties.java的属性类， 我们在application.properties/yml文件里配置的属性， 就是配置的这些XXXProperties.java类的属性。

**SpringBoot自动配置体现的是SPI(Service Provider Interface)的思想， 包括JDK中都大量使用到了这种思想。**

#### SpringBoot配置文件加载

1. application.properties
2. application.yml
3. ...

#### SpringBoot是如何推断应用类型和main的

在SpringBoot中，有一个WebApplicationType的枚举类定义了3种Web类型:

1. NONE
2. SERVLET(Servlet 类型)
3. REACTIVE(响应式类型)，

SpringBoot在启动时会根据对应类型的核心类是否存在，据此判断应用的类型。

而判断main方法是根据异常堆栈来判断的。

### Springboot启动方法

#### run方法

1. 秒表

2. 获取监听器
   1. 获取Spring Factory实例对象
   2. 读取META-INF/spring.factories，配置接口的实现类名称，然后在程序中读取这些配置文件并实例化
   3. 创建SpringFactory实例
      1. 通过名字创建类的class对象
      2. 构造器获取
      3. 创建具体实例
      4. 加入实例表中
3. 监听器启动
4. try：application启动参数列表
5. 配置忽略的bean信息
6. 创建应用上下文createApplicationContext
7. 准备上下文，装配bean
8. 上下文刷新，刷新后做啥
9. 监听器开始
10. 唤醒runners
11. 监听器正式运行

# 9. 消息队列

存储和收发消息的应用程序，保证了应用程序见消息传递的可靠性

## 为什么需要，使用场景

- 系统解耦
- 流量削峰
- 日志处理
- 广播消息（分布式系统）

## MQ模型

所有的MQ的模型抽象出来都是一样的: 消费者(订阅者)订阅某个消息队列, 生产者(发布者)发布消息到消息队列,最后消息队列将消息发送给消费者。

## Kafka、RabbitMQ对比

### RabbitMQ架构

RabbitMQ是一个分布式系统，这里面有几个抽象概念。

- broker：每个节点运行的服务程序，功能为维护该节点的队列的增删以及转发队列操作请求。
- master queue：每个队列都分为一个主队列和若干个镜像队列。
- mirror queue：镜像队列，作为master queue的备份。在master queue所在节点挂掉之后，系统把mirror queue提升为master queue，负责处理客户端队列操作请求。注意，mirror queue只做镜像，设计目的不是为了承担客户端读写压力。

![image-20210617010410343](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617010410343.png)

如上图所示，集群中有两个节点，每个节点上有一个broker，每个broker负责本机上队列的维护，并且borker之间可以互相通信。集群中有两个队列A和B，每个队列都分为master queue和mirror queue（备份）。那么队列上的生产消费怎么实现的呢？

![image-20210617010538181](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617010538181.png)

如上图有两个consumer消费队列A，这两个consumer连在了集群的不同机器上。RabbitMQ集群中的任何一个节点都拥有集群上所有队列的元信息，所以连接到集群中的任何一个节点都可以，主要区别在于有的consumer连在master queue所在节点，有的连在非master queue节点上。

因为mirror queue要和master queue保持一致，故需要同步机制，正因为一致性的限制，导致所有的读写操作都必须都操作在master queue上（想想，为啥读也要从master queue中读？和数据库读写分离是不一样的。），然后由master节点同步操作到mirror queue所在的节点。即使consumer连接到了非master queue节点，该consumer的操作也会被路由到master queue所在的节点上，这样才能进行消费。

生产

![image-20210617011403890](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617011403890.png)

原理和消费一样，如果连接到非 master queue 节点，则路由过去。

所以，到这里小伙伴们就可以看到 RabbitMQ的不足：由于master queue单节点，导致性能瓶颈，吞吐量受限。虽然为了提高性能，内部使用了Erlang这个语言实现，但是终究摆脱不了架构设计上的致命缺陷。

### Kafka架构

改进的点就是：把一个队列的单一master变成多个master，即一台机器扛不住qps，那么我就用多台机器扛qps，把一个队列的流量均匀分散在多台机器上不就可以了么？注意，多个master之间的数据没有交集，即一条消息要么发送到这个master queue，要么发送到另外一个master queue。

这里面的每个master queue 在Kafka中叫做Partition，即一个分片。一个队列有多个主分片，每个主分片又有若干副分片做备份，同步机制类似于RabbitMQ。

![image-20210617011539143](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617011539143.png)

如上图，我们省略了不同的queue，假设集群上只有一个queue（Kafka中叫Topic）。每个生产者随机把消息发送到主分片上，之后主分片再同步给副分片。

![image-20210617011626998](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617011626998.png)

队列读取的时候虚拟出一个Group的概念，一个Topic内部的消息，只会路由到同Group内的一个consumer上，同一个Group中的consumer消费的消息是不一样的；Group之间共享一个Topic，看起来就是一个队列的多个拷贝。所以，为了达到多个Group共享一个Topic数据，Kafka并不会像RabbitMQ那样消息消费完毕立马删除，而是必须在后台配置保存日期，即只保存最近一段时间的消息，超过这个时间的消息就会从磁盘删除，这样就保证了在一个时间段内，Topic数据对所有Group可见（这个特性使得Kafka非常适合做一个公司的数据总线）。队列读同样是读主分片，并且为了优化性能，消费者与主分片有一一的对应关系，如果消费者数目大于分片数，则存在某些消费者得不到消息。

由此可见，Kafka绝对是为了高吞吐量设计的，比如设置分片数为100，那么就有100台机器去扛一个Topic的流量，当然比RabbitMQ的单机性能好。

## 数据持久化

### Kafka

Kafka 直接将数据写到了文件系统的日志中：

- 写操作：将数据顺序追加到文件中
- 读操作：从文件中读取

这样实现的好处：

- 读操作不会阻塞写操作和其他操作，数据大小不对性能产生影响
- 硬盘空间相对于内存空间容量限制更小
- 线性访问磁盘，速度快，可以保存更长的时间，更稳定。

一个 Topic 被分成多 Partition，每个 Partition 在存储层面是一个 append-only 日志文件，属于一个 Partition 的消息都会被直接追加到日志文件的尾部，每条消息在文件中的位置称为 offset（偏移量）。

日志文件由“日志条目（log entries）”序列组成，每一个日志条目包含一个4字节整型数（值为N），其后跟N个字节的消息体。每条消息都有一个当前 Partition 下唯一的64字节的 offset，标识这条消息的起始位置。

**写**

日志文件允许串行附加，并且总是附加到最后一个文件。当文件达到配置指定的大小（`log.segment.bytes` = 1073741824 (bytes)）时，就会被滚动到一个新文件中（每个文件称为一个 segment file）。日志有两个配置参数：M，强制操作系统将文件刷新到磁盘之前写入的消息数；S，强制操作系统将文件刷新到磁盘之前的时间（秒）。在系统崩溃的情况下，最多会丢失M条消息或S秒的数据。

**读**

通过给出消息的偏移量（offset）和最大块大小（S）来读取数据。返回一个缓冲区为S大小的消息迭代器，S应该大于任何单个消息的大小，如果消息异常大，则可以多次重试读取，每次都将缓冲区大小加倍，直到成功读取消息为止。可以指定最大消息大小和缓冲区大小，以使服务器拒绝大于某个大小的消息。读取缓冲区可能以部分消息结束，这很容易被大小分隔检测到。

读取指定偏移量的数据时，需要首先找到存储数据的 segment file，由全局偏移量计算 segment file 中的偏移量，然后从此位置开始读取。

**删除**

消息数据随着 segment file 一起被删除。Log manager 允许可插拔的删除策略来选择哪些文件符合删除条件。当前策略为删除修改时间超过 N 天前的任何日志，或者是保留最近的 N GB 的数据。

为了避免在删除时阻塞读操作，采用了 copy-on-write 技术：删除操作进行时，读取操作的二分查找功能实际是在一个静态的快照副本上进行的。

**文件索引**

上面提到日志文件非由一个文件构成，而是分成多个 segment（文件达到一定大小时进行滚动），每个 segment 名为该 segment 第一条消息的 offset 和 ".kafka" 组成。另外会有一个索引文件，标明了每个 segment 下包含的日志条目的 offset 范围。

有了索引文件，消费者可以从 Kafka 的任意可用偏移量位置开始读取消息。索引也被分成片段，所以在删除消息时，也可以删除相应的索引。Kafka 不维护索引的校验和，如果索引出现损坏，Kafka 会通过重新读取消息来重新生成索引。

### RabbitMQ

RabbitMQ在两种情况下会将消息写入磁盘：

1. 消息本身在 `publish` 的时候就要求消息写入磁盘；
2. `内存紧张` 需要将部分内存中的消息转移到磁盘；

**消息什么时候会刷到磁盘？**

1. 写入文件前会有一个Buffer，大小为1M（1048576），数据在写入文件时，首先会写入到这个Buffer，如果Buffer已满，则会将Buffer写入到文件（未必刷到磁盘）；
2. 有个固定的刷盘时间：25ms，也就是不管Buffer满不满，每隔25ms，Buffer里的数据及未刷新到磁盘的文件内容必定会刷到磁盘；
3. 每次消息写入后，如果没有后续写入请求，则会直接将已写入的消息刷到磁盘：使用Erlang的receive x after 0来实现，只要进程的信箱里没有消息，则产生一个timeout消息，而timeout会触发刷盘操作。

消息保存于$MNESIA/msg_store_persistent/x.rdq文件中，其中x为数字编号，从1开始，每个文件最大为16M（16777216），超过这个大小会生成新的文件，文件编号加1。消息以以下格式存在于文件中：

```
<<Size:64, MsgId:16/binary, MsgBody>>
```

MsgId为RabbitMQ通过rabbit_guid:gen()每一个消息生成的GUID，MsgBody会包含消息对应的exchange，routing_keys，消息的内容，消息对应的协议版本，消息内容格式（二进制还是其它）等等。



## RabbitMQ

支持AMQP协议

![RabbitMQ结构图](/Users/cuweir/Documents/github/MyNoteBooks/RabbitMQ.png)

RabbitMQ主要设计以下概念:

- Message消息: 消息即需要发送的数据,由消息体和消息头组成。 消息体是消息的内容,消息头则是对消息体的描述,由许多Property属性组成, 如路由键,优先级,持久化等等组成。
- Producer生产者: 生产者即发布者,它是发布消息的一方,可以是一个RabbitMQ客户端应用程序。
- Consumer消费者: 消费者即订阅者,他是接受消息的一方,也可以是一个RabbitMQ客户端应用程序。
- Exchange交换机: 交换机用于接受 生产者发布的消息,并按消息的路由规则将消息发送到指定的消息队列。
- Binding绑定: 绑定一种规则,它将Exchange交换机与Queue消息队列 按照路由规则绑定起来。
- Routing-Key路由键: 路由键即路由规则, Exchange交换机根据Routing Key将消息投递到指定的消息队列。
- Queue消息队列: 消息队列是消息容器,用于存储和收发消息。一个消息可以投递到一个或多个消息队列中,直到Consumer将消息消费。
- Connection网络连接: 无论是发布消息还是订阅消息,都需要通过TCP连接来完成。
- Channel信道: 对于计算机服务器来说,网络是非常宝贵的资源,如果每次发布消息或订阅消息都需要建立连接, 那就太耗费Connection资源了,所以引入了Channel信道的概念。 发布消息和订阅消息通过Channel信道来完成,而多个Channel可以共享一条Connection ,这样就节约了很多Connection资源。
- Virtual Host 虚拟主机: 虚拟主机是一个虚拟概念。 在Redis中,一个Redis服务器实例可以被分为16个库,但不是说这16个库的大小就是相等的, 比如1个库也可以占9g内存,其他15个库可以共占1g内存。 虚拟主机也是如此,它可以看做是一个独立的rabbitmq服务器实例, 它包含了属于他的一系列的Exchange,Queue等内容。
- Broker: Broker即RabbitMQ服务实例。

### RabbitMQ Exchange的类型

RabbitMQ共有3种Exchange类型:

1. fanout: fanout即发布者 / 订阅者模式,发布者将消息发送到fanout类型的Exchange,Exchange再将消息广播给与此交换机绑定的Queue。
2. direct: direct即路由模式,Exchange根据Routing Key,将消息发送到匹配的队列中。
3. topic: topic也属于路由模式,不过它支持"*","#"等通配符进行路由。

### 死信队列

消息被否定确认，使用 `channel.basicNack` 或 `channel.basicReject` ，并且此时`requeue` 属性被设置为`false`。

消息在队列的存活时间超过设置的TTL时间。

消息队列的消息数量已经超过最大队列长度。

## 如何避免消息重复消费

![消息被重复消费](/Users/cuweir/Documents/github/MyNoteBooks/消息重复消费.png)

### Broker重复消费

1-2中，如果MQ相应的ACK由于网络原因丢失了，那么发布者会再次发送消息给服务器,这就造成了服务器收到了多次相同的消息的问题

### Consumer重复消费

3-4中，如果Consumer响应的ACK由于网络原因丢失了,那么MQ会再次发送消息给Consumer客户端,这也造成了Consumer多次消费相同的消息的问题

### 解决消息的重复消费

无论是MQ服务器还是Consumer消费端,它们都需要判断消息的唯一性, 如果判断消息已经接受过了,那么可以选择不再处理相同的消息。 解决办法是可以给消息添加全局唯一的标识,如唯一ID,用于保证消息的全局唯一性。

## 如何保证消息传递可靠性

消息传递不可靠的情况主要有:

- Producer丢失消息: 即 1 - 2。
- Broker丢失消息: 即消息存储在MQ Broker时丢失。
- Consumer丢失消息: 即 3 - 4。

### Producer消息丢失

Producer丢失消息有2种解决方案:

1. transaction: 如果消息发送成功，则提交事务。如果在发送消息的过程中出现了异常,那么事务就会回滚。 虽然事务可以保证Producer发送消息的可靠性,但是会降低吞吐量, 所以更推荐使用confirm机制保证Producer发送消息的可靠性。
2. confirm: 当消息被发送给消息队列后,Broker会响应一个ACK给Producer。 如果Broker无法处理该消息,则返回一个NACK给Producer,Producer可以根据返回的NACK再做处理。

### Broker消息丢失

Broker丢失消息一般是MQ服务器宕机或出现其它较为严重的问题, 所以Broker丢失消息的问题,可以通过持久化解决。

### Consumer消息丢失

Consumer接受到消息队列的数据后,会自动回复Broker,Broker就会删除这条消息, 但如果Consumer在此时处理消息的过程中,发生了意外,消息就丢失了。 Consumer可以在处理完消息后再手动回复Broker,这样即使发生了意外,Broker也能重新发送消息给Consumer。



## 如何保证消息队列高可用

### RabbitMQ

基于主从。有三种模式：单机，普通集群（无高可用，用来提高吞吐量用的），镜像集群（高可用）

### Kafka

由多个 broker 组成，每个 broker 是一个节点

天然分布式，每个机器放一部分数据

0.8版本提供了HA机制，副本机制，每个 partition 的数据都会同步到其它机器上，形成自己的多个 replica 副本。所有 replica 会选举一个 leader 出来

## 如何保证消息的可靠性传输？或者说，如何处理消息丢失的问题

有重复消费和幂等性的问题

### RabbitMQ

##### 生产者弄丢

选择事务功能，发送数据前开启事务channel.txSelect

```erlang
// 开启事务
channel.txSelect
try {
    // 这里发送消息
} catch (Exception e) {
    channel.txRollback

    // 这里再次重发这条消息
}

// 提交事务
channel.txCommit
```

吞吐量降低，性能下降

或者开启confirm模式，每次写的消息都会分配一个唯一的 id，然后如果写入了 RabbitMQ 中，RabbitMQ 会给你回传一个 `ack` 消息，告诉你说这个消息 ok 了。如果 RabbitMQ 没能处理这个消息，会回调你的一个 `nack` 接口，告诉你这个消息接收失败，你可以重试。而且你可以结合这个机制自己在内存里维护每个消息 id 的状态，如果超过一定时间还没接收到这个消息的回调，那么你可以重发。

**事务机制是同步的**，你提交一个事务之后会**阻塞**在那儿，但是 `confirm` 机制是**异步**的

##### RabbitMQ 弄丢了数据

持久化

设置持久化有**两个步骤**：

- 创建 queue 的时候将其设置为持久化

这样就可以保证 RabbitMQ 持久化 queue 的元数据，但是它是不会持久化 queue 里的数据的。

- 第二个是发送消息的时候将消息的 `deliveryMode` 设置为 2

持久化可以跟生产者那边的 `confirm` 机制配合起来

##### 消费端弄丢了数据

**刚消费到，还没处理，结果进程挂了**。得用 RabbitMQ 提供的 `ack` 机制，简单来说，就是你必须关闭 RabbitMQ 的自动 `ack` ，可以通过一个 api 来调用就行，然后每次你自己代码里确保处理完的时候，再在程序里 `ack` 一把。

![rabbitmq-message-lose-solution](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/rabbitmq-message-lose-solution.png)

### kafka

##### 消费端弄丢了

关闭自动提交offset

##### kafka弄丢了

某个broker宕机

设置如下 4 个参数：

- 给 topic 设置 `replication.factor` 参数：这个值必须大于 1，要求每个 partition 必须有至少 2 个副本。
- 在 Kafka 服务端设置 `min.insync.replicas` 参数：这个值必须大于 1，这个是要求一个 leader 至少感知到有至少一个 follower 还跟自己保持联系，没掉队，这样才能确保 leader 挂了还有一个 follower 吧。
- 在 producer 端设置 `acks=all` ：这个是要求每条数据，必须是**写入所有 replica 之后，才能认为是写成功了**。
- 在 producer 端设置 `retries=MAX` （很大很大很大的一个值，无限次重试的意思）：这个是**要求一旦写入失败，就无限重试**，卡在这里了。

##### 生产者？

acks=all不会丢

## 如何保证顺序性

场景：**RabbitMQ**：一个queue，多个consumer

![rabbitmq-order-01](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/rabbitmq-order-01.png)

**Kafka**：建了一个 topic，有三个 partition

![kafka-order-01](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/kafka-order-01.png)

### 解决方案

#### RabbitMQ

拆分多个 queue，每个 queue 一个 consumer， consumer 内部用内存队列做排队，然后分发给底层不同的 worker 来处理

![rabbitmq-order-02](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/rabbitmq-order-02.png)

#### Kafka

- 一个 topic，一个 partition，一个 consumer，内部单线程消费，单线程吞吐量太低，一般不会用这个。
- 写 N 个内存 queue，具有相同 key 的数据都到同一个内存 queue；然后对于 N 个线程，每个线程分别消费一个内存 queue 即可，这样就能保证顺序性。

![kafka-order-02](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/kafka-order-02.png)

## 如何保证消息不被重复消费？如何保证消息消费的幂等性？

得结合业务来思考，我这里给几个思路：

- 比如你拿个数据要写库，你先根据主键查一下，如果这数据都有了，你就别插入了，update 一下好吧。
- 比如你是写 Redis，那没问题了，反正每次都是 set，天然幂等性。
- 比如你不是上面两个场景，那做的稍微复杂一点，你需要让生产者发送每条数据的时候，里面加一个全局唯一的 id，类似订单 id 之类的东西，然后你这里消费到了之后，先根据这个 id 去比如 Redis 里查一下，之前消费过吗？如果没有消费过，你就处理，然后这个 id 写 Redis。如果消费过了，那你就别处理了，保证别重复处理相同的消息即可。
- 比如基于数据库的唯一键来保证重复数据不会重复插入多条。因为有唯一键约束了，重复数据插入只会报错，不会导致数据库中出现脏数据。

![mq-11](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/mq-11.png)

## 如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？

#### 大量消息在 mq 里积压了几个小时了还没解决

临时紧急扩容了，具体操作步骤和思路如下：

- 先修复 consumer 的问题，确保其恢复消费速度，然后将现有 consumer 都停掉。
- 新建一个 topic，partition 是原来的 10 倍，临时建立好原先 10 倍的 queue 数量。
- 然后写一个临时的分发数据的 consumer 程序，这个程序部署上去消费积压的数据，**消费之后不做耗时的处理**，直接均匀轮询写入临时建立好的 10 倍数量的 queue。
- 接着临时征用 10 倍的机器来部署 consumer，每一批 consumer 消费一个临时 queue 的数据。这种做法相当于是临时将 queue 资源和 consumer 资源扩大 10 倍，以正常的 10 倍速度来消费数据。
- 等快速消费完积压数据之后，**得恢复原先部署的架构**，**重新**用原先的 consumer 机器来消费消息。

#### mq 中的消息过期失效了

RabbtiMQ 是可以设置过期时间的，也就是 TTL。

可以采取一个方案，就是**批量重导**

#### mq 都快写满了

临时写程序，接入数据来消费，**消费一个丢弃一个，都不要了**，快速消费掉所有的消息。然后走第二个方案，到了晚上再补数据吧

RocketMQ，官方针对消息积压问题，提供了解决方案：

##### 1. 提高消费并行度

同一个 ConsumerGroup 下，通过增加 Consumer 实例数量来提高并行度（需要注意的是超过订阅队列数的 Consumer 实例无效）。可以通过加机器，或者在已有机器启动多个进程的方式。 提高单个 Consumer 的消费并行线程，通过修改参数 consumeThreadMin、consumeThreadMax 实现。

##### 2. 批量方式消费

某些业务流程如果支持批量方式消费，则可以很大程度上提高消费吞吐量，例如订单扣款类应用，一次处理一个订单耗时 1 s，一次处理 10 个订单可能也只耗时 2 s，这样即可大幅度提高消费的吞吐量，通过设置 consumer 的 consumeMessageBatchMaxSize 返个参数，默认是 1，即一次只消费一条消息，例如设置为 N，那么每次消费的消息数小于等于 N。

##### 3. 跳过非重要消息

选择丢弃不重要的消息。例如，当某个队列的消息数堆积到 100000 条以上，则尝试丢弃部分或全部消息，这样就可以快速追上发送消息的速度。

##### 4. 优化每条消息消费过程

优化代码，如DB查询、插入等

## 设计一个MQ

- 首先这个 mq 得支持可伸缩性吧，就是需要的时候快速扩容，就可以增加吞吐量和容量，那怎么搞？设计个分布式的系统呗，参照一下 kafka 的设计理念，broker -> topic -> partition，每个 partition 放一个机器，就存一部分数据。如果现在资源不够了，简单啊，给 topic 增加 partition，然后做数据迁移，增加机器，不就可以存放更多数据，提供更高的吞吐量了？
- 其次你得考虑一下这个 mq 的数据要不要落地磁盘吧？那肯定要了，落磁盘才能保证别进程挂了数据就丢了。那落磁盘的时候怎么落啊？顺序写，这样就没有磁盘随机读写的寻址开销，磁盘顺序读写的性能是很高的，这就是 kafka 的思路。
- 其次你考虑一下你的 mq 的可用性啊？这个事儿，具体参考之前可用性那个环节讲解的 kafka 的高可用保障机制。多副本 -> leader & follower -> broker 挂了重新选举 leader 即可对外服务。
- 能不能支持数据 0 丢失啊？可以的，参考我们之前说的那个 kafka 数据零丢失方案。

# 10. ZooKeeper

## 场景

- 分布式协调
  - 就好比，你 A 系统发送个请求到 mq，然后 B 系统消息消费之后处理了。那 A 系统如何知道 B 系统的处理结果？用 zookeeper 就可以实现分布式系统之间的协调工作。A 系统发送请求之后可以在 zookeeper 上**对某个节点的值注册个监听器**，一旦 B 系统处理完了就修改 zookeeper 那个节点的值，A 系统立马就可以收到通知，完美解决。
- 分布式锁
  - 举个栗子。对某一个数据连续发出两个修改操作，两台机器同时收到了请求，但是只能一台机器先执行完另外一个机器再执行。那么此时就可以使用 zookeeper 分布式锁，一个机器接收到了请求之后先获取 zookeeper 上的一把分布式锁，就是可以去创建一个 znode，接着执行操作；然后另外一个机器也**尝试去创建**那个 znode，结果发现自己创建不了，因为被别人创建了，那只能等着，等第一个机器执行完了自己再执行。
- 元数据/配置信息管理：zookeeper 可以用作很多系统的配置信息的管理，比如 kafka、storm 等等很多分布式系统都会选用 zookeeper 来做一些元数据、配置信息的管理，包括 dubbo 注册中心不也支持 zookeeper 么？
- HA 高可用性：这个应该是很常见的，比如 hadoop、hdfs、yarn 等很多大数据系统，都选择基于 zookeeper 来开发 HA 高可用机制，就是一个**重要进程一般会做主备**两个，主进程挂了立马通过 zookeeper 感知到切换到备用进程。

![image-20210607010225078](/Users/cuweir/Library/Application Support/typora-user-images/image-20210607010225078.png)

## 四种类型的数据节点 Znode

（1）PERSISTENT-持久节点
 除非手动删除，否则节点一直存在于 Zookeeper 上

（2）EPHEMERAL-临时节点
 临时节点的生命周期与客户端会话绑定，一旦客户端会话失效（客户端与zookeeper 连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除。

（3）PERSISTENT_SEQUENTIAL-持久顺序节点
 基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

（4）EPHEMERAL_SEQUENTIAL-临时顺序节点
 基本特性同临时节点，增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

## 分布式锁

某个节点尝试创建临时 znode，此时创建成功了就获取了这个锁；这个时候别的客户端来创建锁会失败，只能**注册个监听器**监听这个锁。释放锁就是删除这个 znode，一旦释放掉就会通知客户端，然后有一个等待着的客户端就可以再次重新加锁。

也可以采用另一种方式，创建临时顺序节点：

如果有一把锁，被多个人给竞争，此时多个人会排队，第一个拿到锁的人会执行，然后释放锁；后面的每个人都会去监听**排在自己前面**的那个人创建的 node 上，一旦某个人释放了锁，排在自己后面的人就会被 ZooKeeper 给通知，一旦被通知了之后，就 ok 了，自己就获取到了锁，就可以执行代码了。

但是，使用 zk 临时节点会存在另一个问题：由于 zk 依靠 session 定期的心跳来维持客户端，如果客户端进入长时间的 GC，可能会导致 zk 认为客户端宕机而释放锁，让其他的客户端获取锁，但是客户端在 GC 恢复后，会认为自己还持有锁，从而可能出现多个客户端同时获取到锁的情形。

针对这种情况，可以通过 JVM 调优，尽量避免长时间 GC 的情况发生。

# 11. JAVA IO&NIO

## IO

第一，传统的 java.io 包，它基于流模型实现，提供了我们最熟知的一些 IO 功能，比如 File 抽象、输入输出流等。交互方式是**同步、阻塞**的方式，也就是说，在读取输入流或者写入输出流时，在读、写动作完成之前，线程会一直阻塞在那里，它们之间的调用是可靠的线性顺序。

第二，在 Java 1.4 中引入了 NIO 框架（java.nio 包），提供了 Channel、Selector、Buffer 等新的抽象，可以构建**多路复用的、同步非阻塞** IO 程序，同时提供了更接近操作系统底层的高性能数据操作方式。

第三，在 Java 7 中，NIO 有了进一步的改进，也就是 NIO 2，引入了**异步非阻塞** IO 方式，也有很多人叫它 AIO（Asynchronous IO）。异步 IO 操作基于事件和回调机制，可以简单理解为，应用操作直接返回，而不会阻塞在那里，当后台处理完成，操作系统会通知相应线程进行后续工作。

## 为什么要NIO，多路复用

大家知道 Java 语言目前的线程实现是比较重量级的，启动或者销毁一个线程是有明显开销的，每个线程都有单独的线程栈等结构，需要占用非常明显的内存，所以，每一个 Client 启动一个线程似乎都有些浪费。那么，稍微修正一下这个问题，我们引入线程池机制来避免浪费。

如果连接数并不是非常多，只有最多几百个连接的普通应用，这种模式往往可以工作的很好。但是，如果连接数量急剧上升，这种实现方式就无法很好地工作了，因为线程上下文切换开销会在高并发时变得很明显，这是同步阻塞方式的低扩展性劣势。

NIO 引入的多路复用机制，提供了另外一种思路：

主要步骤和元素：

- 首先，通过 Selector.open() 创建一个 Selector，作为类似调度员的角色。
- 然后，创建一个 ServerSocketChannel，并且向 Selector 注册，通过指定 SelectionKey.OP_ACCEPT，告诉调度员，它关注的是新的连接请求。注意，为什么我们要明确配置非阻塞模式呢？这是因为阻塞模式下，注册操作是不允许的，会抛出 IllegalBlockingModeException 异常。
- Selector 阻塞在 select 操作，当有 Channel 发生接入请求，就会被唤醒。
- 在 sayHelloWorld 方法中，通过 SocketChannel 和 Buffer 进行数据操作，在本例中是发送了一段字符串。

## AIO

在 Java 7 引入的 NIO 2 中，又增添了一种额外的异步 IO 模式，利用事件和回调，处理 Accept、Read 等操作。

基本抽象很相似，AsynchronousServerSocketChannel 对应于上面例子中的 ServerSocketChannel；AsynchronousSocketChannel 则对应 SocketChannel。业务逻辑的关键在于，通过指定 CompletionHandler 回调接口，在 accept/read/write 等关键节点，通过事件机制调用，这是非常不同的一种编程思路。



## Buffer

本质上是内存中的一块

### position, limit, capacity

capacity：容量

position：下一次写入、读取的位置

limit：写模式时，最大能写入的数据，同capacity；读模式时，buffer中实际的数据大小

### 提取、填充Buffer

NIO的读操作：Channel读数据到Buffer，对应的是Buffer的写入操作

**flip()**切换模式：设置position和limit

```java
public final Buffer flip() {
    limit = position; // 将 limit 设置为实际写入的数据数量
    position = 0; // 重置 position 为 0
    mark = -1; // mark 之后再说
    return this;
}
```

### mark()和reset()

mark()：临时存放postion的值

reset()：回到mark的的地方

### rewind() & clear() & compact()

rewind()：重置position为0，通常用于重新从头读写 Buffer

clear()：有点重置 Buffer 的意思，相当于重新实例化了一样

compact()：和 clear() 一样的是，它们都是在准备往 Buffer 填充新的数据之前调用

compact() 方法有点不一样，调用这个方法以后，会先处理还没有读取的数据，也就是 position 到 limit 之间的数据（还没有读过的数据），先将这些数据移到左边，然后在这个基础上再开始写入。很明显，此时 limit 还是等于 capacity，position 指向原来数据的右边

## Channel

​	类似在 Linux 之类操作系统上看到的文件描述符，是 NIO 中被用来支持批量式 IO 操作的一种抽象。File 或者 Socket，通常被认为是比较高层次的抽象，而 Channel 则是更加操作系统底层的一种抽象，这也使得 NIO 得以充分利用现代操作系统底层机制，获得特定场景的性能优化，例如，DMA（Direct Memory Access）等。不同层次的抽象是相互关联的，我们可以通过 Socket 获取 Channel，反之亦然。


通道，数据来源或写入的目的地

读操作：channel to buffer，channel.read(buffer)

写操作：buffer to channel，channel.write(buffer)

### FileChannel

### SocketChannel

不仅仅是 TCP 客户端，它代表的是一个网络通道，可读可写

### ServerSocketChannel

对应服务端，用于监听机器端口

不和buffer打交道，因为不实际处理数据

### DatagramChannel

UDP链接，处理服务端和客户端

## Selector

![image-20210615011555876](/Users/cuweir/Library/Application Support/typora-user-images/image-20210615011555876.png)

非阻塞，多路复用，用于实现一个线程管理多个Channel

多路复用器 Selector 就是基于 epoll 的多路复用技术实现。

在 IO 编程 过程中，当需要同时处理多个客户端接入请求时，可以利用多线程或者 IO 多路复用技术 进行处理。IO 多路复用技术 通过把多个 IO 的阻塞复用到同一个 select 的阻塞上，从而使得系统在单线程的情况下可以同时处理多个客户端请求。与传统的多线程/多进程模型比，IO 多路复用 的最大优势是系统开销小，系统不需要创建新的额外进程或线程，也不需要维护这些进程和线程的运行，降低了系统的维护工作量，节省了系统资源，IO 多路复用 的主要应用场景如下。

- 服务器需要同时处理多个处于监听状态或者多个连接状态的套接字;
- 服务器需要同时处理多种网络协议的套接字。

目前支持 IO 多路复用 的系统调用有 select、pselect、poll、epoll，在 Linux 网络编程 过程中，很长一段时间都使用 select 做轮询和网络事件通知，然而 select 的一些固有缺陷导致了它的应用受到了很大的限制，最终 Linux 选择了 epoll。epoll 与 select 的原理比较类似，为了克服 select 的缺点，epoll 作了很多重大改进，现总结如下。

1. 支持一个进程打开的 socket 描述符 (fd) 不受限制(仅受限于操作系统的最大文件句柄数)；
2. IO 效率 不会随着 FD 数目的增加而线性下降；
3. epoll 的 API 更加简单。

值得说明的是，用来克服 select/poll 缺点的方法不只有 epoll, epoll 只是一种 Linux 的实现方案。

```java
// 将通道设置为非阻塞模式，因为默认都是阻塞模式的
channel.configureBlocking(false);
// 注册
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
```

方法：

select()：将上次select之后准备好的channel对应的SelectionKey复制到selected set，会阻塞

selectNow()：和select()一样，通道没有准备好会立即返回0

select(long timeout)：

wakeup()：唤醒等待的select()上的线程







## Java非阻塞IO和异步IO

即NIO和AIO

### 阻塞模式IO

来一个新的连接，就新开一个线程来处理这个连接，之后的操作全部由那个线程来完成。

性能瓶颈：

- 每次来一个连接都开一个新的线程这肯定是不合适的，活跃连接数高时，内存占用高，线程切换开销大
- accept() 是一个阻塞操作，当 accept() 返回的时候，代表有一个连接可以使用

### 非阻塞IO

核心在于使用一个Selector管理多个通道，各个通道注册到Selector，指定监听的事件，可以只用一个线程轮询Selector

- select：上世纪 80 年代就实现了，它支持注册 FD_SETSIZE(1024) 个 socket
- poll：不限制socket数量。select 和 poll 都有一个共同的问题，那就是**它们都只会告诉你有几个通道准备好了，但是不会告诉你具体是哪几个通道**
- epoll：能直接返回具体的准备好的通道，时间复杂度 O(1)。

### NIO.2 异步IO

JDK 1.7发布，引入异步 IO 接口和 Paths 等文件访问接口

由线程池负责执行任务，使用回调或自己去查询结果

异步 IO 主要是为了控制线程数量，减少过多的线程带来的内存消耗和 CPU 在线程调度上的开销。

Java异步IO提供了两种方式，分别是返回Future实例和使用回调函数：

#### 1. 返回 Future 实例

- future.isDone();

  判断操作是否已经完成，包括了**正常完成、异常抛出、取消**

- future.cancel(true);

  取消操作，方式是中断。参数 true 说的是，即使这个任务正在执行，也会进行中断。

- future.isCancelled();

  是否被取消，只有在任务正常结束之前被取消，这个方法才会返回 true

- future.get(); 

  这是我们的老朋友，获取执行结果，阻塞。

- future.get(10, TimeUnit.SECONDS);

  如果上面的 get() 方法的阻塞你不满意，那就设置个超时时间。

#### 2. 提供 CompletionHandler 回调函数

java.nio.channels.CompletionHandler 接口定义：

```java
public interface CompletionHandler<V,A> {

    void completed(V result, A attachment);

    void failed(Throwable exc, A attachment);
}
```

#### AsynchronousFileChannel

文件 IO 在所有的操作系统中都不支持非阻塞模式，但是我们可以对文件 IO 采用异步的方式来提高性能。

#### AsynchronousServerSocketChannel

这个类对应的是非阻塞 IO 的 ServerSocketChannel，大家可以类比下使用方式。

#### AsynchronousSocketChannel

#### Asynchronous Channel Groups

AsynchronousServerSocketChannels 和     AsynchronousSocketChannels 是属于 group 的，当我们调用 AsynchronousServerSocketChannel 或 AsynchronousSocketChannel 的 open() 方法的时候，相应的 channel 就属于默认的 group，这个 group 由 JVM 自动构造并管理

**AsynchronousFileChannels 不属于 group**

## NIO服务端序列图

![image-20210615002829494](/Users/cuweir/Library/Application Support/typora-user-images/image-20210615002829494.png)



## Netty

### 为什么选择netty

Netty 是业界最流行的 NIO 框架 之一，它的健壮性、功能、性能、可定制性和可扩展性在同类框架中都是首屈一指的，已经得到成百上千的商用项目验证，例如 Hadoop 的 RPC 框架 Avro ，阿里的 RPC 框架 Dubbo 就使用了 Netty 作为底层通信框架。通过对 Netty 的分析，我们将它的优点总结如下。

- API 使用简单，开发门槛低；
- 功能强大，预置了多种编解码功能，支持多种主流协议；
- 定制能力强，可以通过 ChannelHandler 对通信框架进行灵活地扩展；
- 性能高，通过与其他业界主流的 NIO 框架 对比，Netty 的综合性能最优；
- 成熟、稳定，Netty 修复了已经发现的所有 JDK NIO BUG，业务开发人员不需要再为 NIO 的 BUG 而烦恼；
- 社区活跃，版本迭代周期短，发现的 BUG 可以被及时修复，同时，更多的新功能会加入；
- 经历了大规模的商业应用考验，质量得到验证。Netty 在互联网、大数据、网络游戏、企业应用、电信软件等众多行业已经得到了成功商用，证明它已经完全能够满足不同行业的商业应用了

### 解决粘包拆包

**TCP 粘包/拆包发生的原因**

问题产生的原因有三个，分别如下。

1. **应用程序 write 写入的字节大小 超出了 套接口发送缓冲区大小；**
2. 进行 MSS 大小的 TCP 分段；
3. 以太网帧的 payload 大于 MTU 进行 IP 分片。

**粘拆包问题的解决策略**

由于底层的 TCP 无法理解上层的业务数据，所以在底层是无法保证数据包不被拆分和重组的，这个问题只能通过上层的应用协议栈设计来解决，根据业界的主流协议的解决方案，可以归纳如下。

1. 固定消息长度，例如，每个报文的大小为 固定长度 200 字节，如果不够，空位补空格；
2. 在包尾使用 “回车换行符” 等特殊字符，作为消息结束的标志，例如 FTP 协议，这种方式在文本协议中应用比较广泛；
3. 将消息分为消息头和消息体，在消息头中定义一个 长度字段 Len 来标识消息的总长度;
4. 更复杂的应用层协议。

**Netty 的解码器 解决 TCP 粘拆包问题**

根据上面的 粘拆包问题解决策略，Netty 提供了相应的解码器实现。有了这些解码器，用户不需要自己对读取的报文进行人工解码，也不需要考虑 TCP 的粘包和拆包。

LineBasedFrameDecoder 的工作原理是它依次遍历 ByteBuf 中的可读字节，判断看是否有 “\n” 或者 “\r\n”，如果有，就以此位置为结束位置，从可读索引到结束位置区间的字节就组成了一行。它是以换行符为结束标志的解码器，支持携带结束符或者不携带结束符两种解码方式，同时支持配置单行的最大长度。如果连续读取到最大长度后仍然没有发现换行符，就会抛出异常，同时忽略掉之前读到的异常码流。

StringDecoder 的功能非常简单，就是将接收到的对象转换成字符串，然后继续调用后面的 Handler。LineBasedFrameDecoder + StringDecoder 组合 就是按行切换的文本解码器，它被设计用来支持 TCP 的粘包和拆包。

### 服务端

![image-20210615013357244](/Users/cuweir/Library/Application Support/typora-user-images/image-20210615013357244.png)







### 客户端

![image-20210615014434014](/Users/cuweir/Library/Application Support/typora-user-images/image-20210615014434014.png)



### 架构设计

三层网络架构

**通信调度层 Reactor**

它由一系列辅助类完成，包括 Reactor 线程 NioEventLoop 及其父类，NioSocketChannel / NioServerSocketChannel 及其父类，Buffer 组件，Unsafe 组件 等。该层的主要职责就是**监听网络的读写和连接操作**，负责**将网络层的数据读取到内存缓冲区**，然后触发各种网络事件，例如连接创建、连接激活、读事件、写事件等，将这些事件触发到 PipeLine 中，由 PipeLine 管理的责任链来进行后续的处理。

**责任链层 Pipeline**

它负责上述的各种网络事件 在责任链中的有序传播，同时负责动态地编排责任链。责任链可以选择监听和处理自己关心的事件，它可以拦截处理事件，以及向前向后传播事件。不同应用的 Handler 节点 的功能也不同，通常情况下，往往会开发 编解码 Hanlder 用于消息的编解码，可以将外部的协议消息转换成 内部的 POJO 对象，这样上层业务则只需要关心处理业务逻辑即可，不需要感知底层的协议差异和线程模型差异，实现了架构层面的分层隔离。

**业务逻辑编排层 Service ChannelHandler**

业务逻辑编排层通常有两类：一类是纯粹的业务逻辑编排，还有一类是其他的应用层协议插件，用于特定协议相关的会话和链路管理。例如，CMPP 协议，用于管理和中国移动短信系统的对接。

架构的不同层面，需要关心和处理的对象都不同，通常情况下，对于业务开发者，只需要关心责任链的拦截和 业务 Handler 的编排。因为应用层协议栈往往是开发一次，到处运行，所以实际上对于业务开发者来说，只需要关心服务层的业务逻辑开发即可。各种应用协议以插件的形式提供，只有协议开发人员需要关注协议插件，对于其他业务开发人员来说，只需关心业务逻辑定制。这种分层的架构设计理念实现了 NIO 框架 各层之间的解耦，便于上层业务协议栈的开发和业务逻辑的定制。

### Socket和SocketChannel

1. 所属包不同。Socket 在 java.net 包 中，而 SocketChannel 在 java.nio 包 中。
2. 异步方式不同。从包的不同，我们大体可以推断出他们主要的区别：Socket 是阻塞连接，SocketChannel 可以设置为非阻塞连接。使用 ServerSocket 与 Socket 的搭配，服务端 Socket 往往要为每一个 客户端 Socket 分配一个线程，而每一个线程都有可能处于长时间的阻塞状态中。过多的线程也会影响服务器的性能。而使用 SocketChannel 与 ServerSocketChannel 的搭配可以非阻塞通信，这样使得服务器端只需要一个线程就能处理所有 客户端 Socket 的请求。
3. 性能不同。一般来说，高并发场景下，使用 SocketChannel 与 ServerSocketChannel 的搭配会有更好的性能。
4. 使用方式不同。Socket、ServerSocket 类 可以传入不同参数直接实例化对象并绑定 IP 和 端口。而 SocketChannel、ServerSocketChannel 类 需要借助 Selector 类。



# 12. Linux操作系统

## 线程模型

操作内核线程的模型主要有如下三种：

1. 使用内核线程（1:1 模型）
2. 使用用户线程（1:N 模型）
3. 使用用户线程 + 轻量级进程（LWP）（N:M 模型）

操作系统中的几个关键概念：

- **内核线程 KLT**：内核级线程（Kemel-Level Threads, KLT 也有叫做内核支持的线程），直接由操作系统内核支持，线程创建、销毁、切换开销较大
- **用户线程 UT**：用户线程(User Thread,UT)，建立在用户空间，系统内核不能感知用户线程的存在，线程创建、销毁、切换开销小
- **轻量级进程 LWP**： （LWP，Light weight process）用户级线程和内核级线程之间的中间层，是由操作系统提供给用户的操作内核线程的接口的实现 。
- **进程 P**：用户进程

### 多对一模型

多对一线程模型，又叫作用户级线程模型，即多个用户线程对应到同一个内核线程上，线程的创建、调度、同步的所有细节全部由进程的用户空间线程库来处理。

**优点：**

- 用户线程的很多操作对内核来说都是透明的，不需要用户态和内核态的频繁切换，使线程的创建、调度、同步等非常快；

**缺点：**

- 由于多个用户线程对应到同一个内核线程，如果其中一个用户线程阻塞，那么该其他用户线程也无法执行；
- 内核并不知道用户态有哪些线程，无法像内核线程一样实现较完整的调度、优先级等；

许多语言实现的协程库基本上都属于这种方式，比如python的gevent。

### 一对一线程模型

一对一模型，又叫作内核级线程模型，即一个用户线程对应一个内核线程，内核负责每个线程的调度，可以调度到其他处理器上面。

**优点：**

- 实现简单

**缺点：**

- 对用户线程的大部分操作都会映射到内核线程上，引起用户态和内核态的频繁切换；
- 内核为每个线程都映射调度实体，如果系统出现大量线程，会对系统性能有影响；

Java使用的就是一对一线程模型，所以在Java中启一个线程要谨慎。

### 多对多线程模型

多对多模型，又叫作两级线程模型，它是博采众长之后的产物，充分吸收前两种线程模型的优点且尽量规避它们的缺点。

在此模型下，用户线程与内核线程是多对多（m : n，通常m>=n）的映射模型。

首先，区别于多对一模型，多对多模型中的一个进程可以与多个内核线程关联，于是进程内的多个用户线程可以绑定不同的内核线程，这点和一对一模型相似；

其次，又区别于一对一模型，它的进程里的所有用户线程并不与内核线程一一绑定，而是可以动态绑定内核线程， 当某个内核线程因为其绑定的用户线程的阻塞操作被内核调度让出CPU时，其关联的进程中其余用户线程可以重新与其他内核线程绑定运行。

所以，多对多模型既不是多对一模型那种完全靠自己调度的也不是一对一模型完全靠操作系统调度的，而是中间态（自身调度与系统调度协同工作），因为这种模型的高度复杂性，操作系统内核开发者一般不会使用，所以更多时候是作为第三方库的形式出现。

**优点：**

- 兼具多对一模型的轻量；
- 由于对应了多个内核线程，则一个用户线程阻塞时，其他用户线程仍然可以执行；
- 由于对应了多个内核线程，则可以实现较完整的调度、优先级等；

**缺点：**

- 实现复杂

Go语言中的goroutine调度器就是采用的这种实现方案，在Go语言中一个进程可以启动成千上万个goroutine，这也是其出道以来就自带“高并发”光环的重要原因。



（3）**操作系统**一般只实现到一对一模型；

（4）**Java**使用的是一对一线程模型，所以它的一个线程对应于一个内核线程，调度完全交给操作系统来处理；

（5）Go语言使用的是多对多线程模型，这也是其高并发的原因，它的线程模型与Java中的ForkJoinPool非常类似；

（6）python的gevent使用的是多对一线程模型；

## 操作系统的用户态与内核态

- 内核态(核心态,特权态): **内核态是操作系统内核运行的模式。** 内核态控制计算机的硬件资源，如硬件设备，文件系统等等，并为上层应用程序提供执行环境。
- 用户态: **用户态是用户应用程序运行的状态。** 应用程序必须依托于内核态运行,因此用户态的态的操作权限比内核态是要低的， 如磁盘，文件等，访问操作都是受限的。
- 系统调用: 系统调用是操作系统为应用程序提供能够访问到内核态的资源的接口。

### 架构

一个完整的 Linux 操作系统体系架构通常由下列几个核心层级组成：

Applications：在操作系统上安装并运行的用户态应用程序
Shell：支持编程的命令行解析器
Libs：操作系统标准库函数
System Calls：暴露给用户态的内核态系统调用接口
Kernel：操作系统的核心，真正对接硬件平台的软件程序

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210621210832875.png" alt="image-20210621210832875" style="zoom:50%;" />

Linux Kernel 本质上看是一种软件，实现了进程管理器、内存管理器、文件系统、设备驱动以及网络管理组件来负责对接、管理计算机硬件平台，并通过系统调用（System Calls）为上层应用程序暴露硬件资源以提供程序运行环境。

以系统调用为边界将 Linux 操作系统的体系架构分为用户态和内核态（包括系统调用）。

操作系统的用户态和内核态实际上对应了 CPU 指令集中的非特权指令和特权指令的执行状态，CPU 划分了不同的执行级别来执行具有相应特权的指令，例如：Intel x86 CPU 具有四种不同的执行级别 [RING0, RING1, RING2, RING3]，Linux 操作系统只使用了其中的 RING0 和 RING3 分别表示内核态与用户态。处于 RING3 状态的用户态代码不能直接访问处于 RING0 的内核态代码的地址空间（包括代码和数据）。
我们知道有些 CPU 特权指令的操作实际是比较危险的，比如：写入系统配置文件、杀掉其他用户的进程或重启系统。所以在操作系统的设计中，为了保障操作系统的稳定性，尤其是在多用户环境中的可靠性，操作系统根据 CPU 的指令类型来抽象并实现了用户态和内核态两种代码运行模式，两种运行模式之间的切换也成为模式切换。用户态的代码被限制了可以执行的操作以及可以访问的资源范围，而内核态的代码则可以执行任何操作并且没有资源使用上的限制。

所以，**为什么要划分核心态和用户态？**简单来说：

禁止用户程序和底层硬件平台直接交互
禁止用户程序直接访问任意内存地址空间

用户运行一个程序，该程序创建的进程开始运行在用户态，如果程序要执行诸如文件操作，网络数据发送操作等内核态操作的话，就必须通过系统调用中的 Write，Send 等功能单元完成，根本是通过调用内核代码完成的。此时，运行该进程的处理器会从 RING3 切换到 RING0 级别，然后进入 3-4GB 内核地址空间中完成内核代码的执行。执行完成后，处理器再从 RING0 切换回 RING3，进程也回到用户态。
![image-20210621211322530](/Users/cuweir/Library/Application Support/typora-user-images/image-20210621211322530.png)

用户态的应用程序可以通过三种方式来访问内核态的资源：

- 系统调用
- 库函数
- Shell 脚本





### **用户态和内核态切换的开销大**

，但是它的开销大在那里呢？简单点来说有下面几点

- 保留用户态现场（上下文、寄存器、用户栈等）
- 复制用户态参数，用户栈切到内核栈，进入内核态
- 额外的检查（因为内核代码对用户不信任）
- 执行内核态代码
- 复制内核态代码执行结果，回到用户态
- 恢复用户态现场（上下文、寄存器、用户栈等）

实际上操作系统会比上述的更复杂，这里只是个大概，我们可以发现一次切换经历了「用户态 -> 内核态 -> 用户态」。

用户态要主动切换到内核态，那必须要有入口才行，实际上内核态是提供了统一的入口，下面是Linux整体架构图

### 什么情况会导致用户态到内核态切换

- 系统调用：用户态进程主动切换到内核态的方式，用户态进程通过系统调用向操作系统申请资源完成工作，例如 fork（）就是一个创建新进程的系统调用，系统调用的机制核心使用了操作系统为用户特别开放的一个中断来实现，如Linux 的 int 80h 中断，也可以称为软中断
- 异常：当 C P U 在执行用户态的进程时，发生了一些没有预知的异常，这时当前运行进程会切换到处理此异常的内核相关进程中，也就是切换到了内核态，如缺页异常
- 中断：当 C P U 在执行用户态的进程时，外围设备完成用户请求的操作后，会向 C P U 发出相应的中断信号，这时 C P U 会暂停执行下一条即将要执行的指令，转到与中断信号对应的处理程序去执行，也就是切换到了内核态。如硬盘读写操作完成，系统会切换到硬盘读写的中断处理程序中执行后边的操作等。

Unix/Linux 的设计哲学之一就是：对不同的操作赋予不同的执行等级，就是所谓特权的概念。简单说就是有多大能力做多大的事，与系统相关的一些特别关键的操作必须由最高特权的程序来完成。Intel 的 X86 架构的 CPU 提供了 0 到 3 四个特权级，数字越小，特权越高。

Linux 操作系统中主要采用了 0 和 3 两个特权级，分别对应的就是内核态和用户态。运行于用户态的进程可以执行的操作和访问的资源都会受到极大的限制，而运行在内核态的进程则可以执行任何操作并且在资源的使用上没有限制。

很多程序开始时运行于用户态，但在执行的过程中，一些操作需要在内核权限下才能执行，这就涉及到一个从用户态切换到内核态的过程。比如C函数库中的内存分配函数 malloc()，它具体是使用 sbrk() 系统调用来分配内存，当malloc() 调用 sbrk() 的时候就涉及一次从用户态到内核态的切换，类似的函数还有 printf()，调用的是 wirte() 系统调用来输出字符串，等等。

### 内核空间

内核空间总是驻留在内存中，它是为操作系统的内核保留的。应用程序是不允许直接在该区域进行读写或直接调用内核代码定义的函数的。

上图左侧区域为内核进程对应的虚拟内存，按访问权限可以分为进程私有和进程共享两块区域：

进程私有的虚拟内存：每个进程都有单独的内核栈、页表、task 结构以及 mem_map 结构等。
进程共享的虚拟内存：属于所有进程共享的内存区域，包括物理存储器、内核数据和内核代码区域。

### 用户空间

每个普通的用户进程都有一个单独的用户空间，处于用户态的进程不能访问内核空间中的数据，也不能直接调用内核函数的 ，因此要进行系统调用的时候，就要将进程切换到内核态才行。

用户空间包括以下几个内存区域：

运行时栈：由编译器自动释放，存放函数的参数值，局部变量和方法返回值等。每当一个函数被调用时，该函数的返回类型和一些调用的信息被存储到栈顶，调用结束后调用信息会被弹出并释放掉内存。栈区是从高地址位向低地址位增长的，是一块连续的内在区域，最大容量是由系统预先定义好的，申请的栈空间超过这个界限时会提示溢出，用户能从栈中获取的空间较小。
运行时堆：用于存放进程运行中被动态分配的内存段，位于 BSS 和栈中间的地址位。由卡发人员申请分配（malloc）和释放（free）。堆是从低地址位向高地址位增长，采用链式存储结构。频繁地 malloc/free 造成内存空间的不连续，产生大量碎片。当申请堆空间时，库函数按照一定的算法搜索可用的足够大的空间。因此堆的效率比栈要低的多。
代码段：存放 CPU 可以执行的机器指令，该部分内存只能读不能写。通常代码区是共享的，即其他执行程序可调用它。假如机器中有数个进程运行相同的一个程序，那么它们就可以使用同一个代码段。
未初始化的数据段：存放未初始化的全局变量，BSS 的数据在程序开始执行之前被初始化为 0 或 NULL。
已初始化的数据段：存放已初始化的全局变量，包括静态全局变量、静态局部变量以及常量。
内存映射区域：例如将动态库，共享内存等虚拟空间的内存映射到物理空间的内存，一般是 mmap 函数所分配的虚拟内存空间。

## 进程、线程、协程

### 进程和线程的区别：

对于进程来说，子进程是父进程的复制品，从父进程那里获得父进程的数据空间，堆和栈的复制品。

而线程，相对于进程而言，是一个更加接近于执行体的概念，可以和同进程的其他线程之间直接共享数据，而且拥有自己的栈空间，拥有独立序列。

共同点： 它们都能提高程序的并发度，提高程序运行效率和响应时间。线程和进程在使用上各有优缺点。 线程执行开销比较小，但不利于资源的管理和保护，而进程相反。同时，线程适合在SMP机器上运行，而进程可以跨机器迁移。

他们之间根本区别在于 多进程中每个进程有自己的地址空间，线程则共享地址空间。所有其他区别都是因为这个区别产生的。比如说：

1. 速度。线程产生的速度快，通讯快，切换快，因为他们处于同一地址空间。
2. 线程的资源利用率好。
3. 线程使用公共变量或者内存的时候需要同步机制，但进程不用。

而他们通信方式的差异也仍然是由于这个根本原因造成的。

通信方式之间的差异
因为那个根本原因，实际上只有进程间需要通信,同一进程的线程共享地址空间,没有通信的必要，但要做好同步/互斥,保护共享的全局变量。

而进程间通信无论是信号，管道pipe还是共享内存都是由操作系统保证的，是系统调用.

#### 一、进程间的通信方式

管道( pipe )：
管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。
有名管道 (namedpipe) ：
有名管道也是半双工的通信方式，但是它允许无亲缘关系进程间的通信。
信号量(semophore ) ：
信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。
消息队列( messagequeue ) ：
消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。
信号 (sinal ) ：
信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生。
共享内存(shared memory ) ：
共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。
套接字(socket ) ：
套接口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同设备及其间的进程通信。

#### 二、线程间的通信方式

锁机制：包括互斥锁、条件变量、读写锁
互斥锁提供了以排他方式防止数据结构被并发修改的方法。
读写锁允许多个线程同时读共享数据，而对写操作是互斥的。
条件变量可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
信号量机制(Semaphore)：包括无名线程信号量和命名线程信号量
信号机制(Signal)：类似进程间的信号处理
线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。

#### 协程

不是被操作系统内核所管理的，而是完全由程序所控制，也就是在用户态执行。这样带来的好处是性能大幅度的提升，因为不会像线程切换那样消耗资源。

协程不是进程也不是线程，而是一个特殊的函数，这个函数可以在某个地方挂起，并且可以重新在挂起处外继续运行。

一个进程可以包含多个线程，一个线程也可以包含多个协程。简单来说，一个线程内可以由多个这样的特殊函数在运行，但是有一点必须明确的是，一个线程的多个协程的运行是串行的。如果是多核CPU，多个进程或一个进程内的多个线程是可以并行运行的，但是一个线程内协程却绝对是串行的，无论CPU有多少个核。毕竟协程虽然是一个特殊的函数，但仍然是一个函数。一个线程内可以运行多个函数，但这些函数都是串行运行的。当一个协程运行时，其它协程必须挂起。

 协程和进程/线程的对比
协程既不是进程也不是线程，协程仅仅是一个特殊的函数，协程它进程和进程不是一个维度的。
一个进程可以包含多个线程，一个线程可以包含多个协程。
一个线程内的多个协程虽然可以切换，但是多个协程是串行执行的，只能在一个线程内运行，没法利用CPU多核能力。
协程与进程一样，是存在上下文切换问题的。
协程是完全由程序控制的，不是由操作系统内核所管理的

- 协程可以比作子程序，但执行过程中，子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来继续执行。协程之间的切换不需要涉及任何系统调用或任何阻塞调用
- 协程只在一个线程中执行，是子程序之间的切换，发生在用户态上。而且，线程的阻塞状态是由操作系统内核来完成，发生在内核态上，因此协程相比线程节省了线程创建和切换的开销
- 协程中不存在同时写变量冲突，因此，也就不需要用来守卫关键区块的同步性原语，比如互斥锁、信号量等，并且不需要来自操作系统的支持。

协程适用于IO阻塞且需要大量并发的场景，当发生IO阻塞，由协程的调度器进行调度，通过将数据流yield掉，并且记录当前栈上的数据，阻塞完后立刻再通过线程恢复协程栈，并把阻塞的结果放到这个线程上去运行。

## IO复用

Linux 提供 select/poll，进程通过将一个或多个 fd 传递给 select 或 poll 系统 调用，阻塞发生在 select/poll 操作上。select/poll 可以帮我们侦测多个 fd 是否处于就绪状态，它们顺序扫描 fd 是否就绪，但支持的 fd 数量有限，因此它的使用也受到了一些制约。Linux 还提供了一个 epoll 系统调用，epoll 使用 基于事件驱动方式 代替 顺序扫描，因此性能更高，当有 fd 就绪时，立即回调函数 rollback。

多路复用器 Selector 就是基于 epoll 的多路复用技术实现。

在 IO 编程 过程中，当需要同时处理多个客户端接入请求时，可以利用多线程或者 IO 多路复用技术 进行处理。IO 多路复用技术 通过把多个 IO 的阻塞复用到同一个 select 的阻塞上，从而使得系统在单线程的情况下可以同时处理多个客户端请求。与传统的多线程/多进程模型比，IO 多路复用 的最大优势是系统开销小，系统不需要创建新的额外进程或线程，也不需要维护这些进程和线程的运行，降低了系统的维护工作量，节省了系统资源，IO 多路复用 的主要应用场景如下。

- 服务器需要同时处理多个处于监听状态或者多个连接状态的套接字;
- 服务器需要同时处理多种网络协议的套接字。

目前支持 IO 多路复用 的系统调用有 select、pselect、poll、epoll，在 Linux 网络编程 过程中，很长一段时间都使用 select 做轮询和网络事件通知，然而 select 的一些固有缺陷导致了它的应用受到了很大的限制，最终 Linux 选择了 epoll。epoll 与 select 的原理比较类似，为了克服 select 的缺点，epoll 作了很多重大改进，现总结如下。

1. 支持一个进程打开的 socket 描述符 (fd) 不受限制(仅受限于操作系统的最大文件句柄数)；
2. IO 效率 不会随着 FD 数目的增加而线性下降；
3. epoll 的 API 更加简单。

值得说明的是，用来克服 select/poll 缺点的方法不只有 epoll, epoll 只是一种 Linux 的实现方案。

# 13. 系统设计



## 设计一个高并发系统

![image-20210606180627829](/Users/cuweir/Library/Application Support/typora-user-images/image-20210606180627829.png)

### 系统拆分

将一个系统拆分为多个子系统，用 dubbo 来搞。然后每个系统连一个数据库，这样本来就一个库，现在多个数据库，不也可以扛高并发么。

### 缓存

缓存，必须得用缓存。大部分的高并发场景，都是**读多写少**，那你完全可以在数据库和缓存里都写一份，然后读的时候大量走缓存不就得了。毕竟人家 redis 轻轻松松单机几万的并发。所以你可以考虑考虑你的项目里，那些承载主要请求的**读场景，怎么用缓存来抗高并发**。

### MQ

MQ，必须得用 MQ。可能你还是会出现高并发写的场景，比如说一个业务操作里要频繁搞数据库几十次，增删改增删改，疯了。那高并发绝对搞挂你的系统，你要是用 redis 来承载写那肯定不行，人家是缓存，数据随时就被 LRU 了，数据格式还无比简单，没有事务支持。所以该用 mysql 还得用 mysql 啊。那你咋办？用 MQ 吧，大量的写请求灌入 MQ 里，排队慢慢玩儿，**后边系统消费后慢慢写**，控制在 mysql 承载范围之内。所以你得考虑考虑你的项目里，那些承载复杂写业务逻辑的场景里，如何用 MQ 来异步写，提升并发性。MQ 单机抗几万并发也是 ok 的，这个之前还特意说过。

### 分库分表

分库分表，可能到了最后数据库层面还是免不了抗高并发的要求，好吧，那么就将一个数据库拆分为多个库，多个库来扛更高的并发；然后将一个表**拆分为多个表**，每个表的数据量保持少一点，提高 sql 跑的性能。

### 读写分离

读写分离，这个就是说大部分时候数据库可能也是读多写少，没必要所有请求都集中在一个库上吧，可以搞个主从架构，**主库写**入，**从库读**取，搞一个读写分离。**读流量太多**的时候，还可以**加更多的从库**。

### ElasticSearch

Elasticsearch，简称 es。es 是分布式的，可以随便扩容，分布式天然就可以支撑高并发，因为动不动就可以扩容加机器来扛更高的并发。那么一些比较简单的查询、统计类的操作，可以考虑用 es 来承载，还有一些全文搜索类的操作，也可以考虑用 es 来承载。

上面的 6 点，基本就是高并发系统肯定要干的一些事儿，大家可以仔细结合之前讲过的知识考虑一下，到时候你可以系统的把这块阐述一下，然后每个部分要注意哪些问题，之前都讲过了，你都可以阐述阐述，表明你对这块是有点积累的。

说句实话，毕竟你真正厉害的一点，不是在于弄明白一些技术，或者大概知道一个高并发系统应该长什么样？其实实际上在真正的复杂的业务系统里，做高并发要远远比上面提到的点要复杂几十倍到上百倍。你需要考虑：哪些需要分库分表，哪些不需要分库分表，单库单表跟分库分表如何 join，哪些数据要放到缓存里去，放哪些数据才可以扛住高并发的请求，你需要完成对一个复杂业务系统的分析之后，然后逐步逐步的加入高并发的系统架构的改造，这个过程是无比复杂的，一旦做过一次，并且做好了，你在这个市场上就会非常的吃香。

其实大部分公司，真正看重的，不是说你掌握高并发相关的一些基本的架构知识，架构中的一些技术，RocketMQ、Kafka、Redis、Elasticsearch，高并发这一块，你了解了，也只能是次一等的人才。对一个有几十万行代码的复杂的分布式系统，一步一步架构、设计以及实践过高并发架构的人，这个经验是难能可贵的。

## 高可用架构

### Hystrix

Hystrix 可以让我们在分布式系统中对服务间的调用进行控制，加入一些**调用延迟**或者**依赖故障**的**容错机制**。

Hystrix 通过将依赖服务进行**资源隔离**，进而阻止某个依赖服务出现故障时在整个系统所有的依赖服务调用中进行蔓延；同时 Hystrix 还提供故障时的 fallback 降级机制。

**Hystrix 的设计原则**

- 对依赖服务调用时出现的调用延迟和调用失败进行**控制和容错保护**。
- 在复杂的分布式系统中，阻止某一个依赖服务的故障在整个系统中蔓延。比如某一个服务故障了，导致其它服务也跟着故障。
- 提供 `fail-fast`（快速失败）和快速恢复的支持。
- 提供 fallback 优雅降级的支持。
- 支持近实时的监控、报警以及运维操作。

## 分布式服务

### 为什么要进行系统拆分

中大型项目，几十人团队，如果不拆分，开发效率极其低下，拆分后可以大大提升开发效率

**如何拆**

个人建议，一个服务的代码不要太多，1 万行左右，两三万撑死了吧。

大部分的系统，是要进行**多轮拆分**的，第一次拆分，可能就是将以前的多个模块该拆分开来了，比如说将电商系统拆分成订单系统、商品系统、采购系统、仓储系统、用户系统，等等吧。**核心意思就是根据情况，先拆分一轮，后面如果系统更复杂了，可以继续分拆**

**用不用dubbo**

http 接口通信维护起来成本很高，你要考虑**超时重试**、**负载均衡**等等各种乱七八糟的问题

dubbo 说白了，是一种 rpc 框架，就是说本地就是进行接口调用，但是 dubbo 会代理这个调用请求，跟远程机器网络通信，给你处理掉负载均衡、服务实例上下线自动感知、超时重试等等乱七八糟的问题

### Dubbo

#### **工作原理**

先说说 dubbo 支撑 rpc 分布式调用的架构

- 第一层：service 层，接口层，给服务提供者和消费者来实现的
- 第二层：config 层，配置层，主要是对 dubbo 进行各种配置的
- 第三层：proxy 层，服务代理层，无论是 consumer 还是 provider，dubbo 都会给你生成代理，代理之间进行网络通信
- 第四层：registry 层，服务注册层，负责服务的注册与发现
- 第五层：cluster 层，集群层，封装多个服务提供者的路由以及负载均衡，将多个实例组合成一个服务
- 第六层：monitor 层，监控层，对 rpc 接口的调用次数和调用时间进行监控
- 第七层：protocal 层，远程调用层，封装 rpc 调用
- 第八层：exchange 层，信息交换层，封装请求响应模式，同步转异步
- 第九层：transport 层，网络传输层，抽象 mina 和 netty 为统一接口
- 第十层：serialize 层，数据序列化层

**工作流程**

- 第一步：provider 向注册中心去注册
- 第二步：consumer 从注册中心订阅服务，注册中心会通知 consumer 注册好的服务
- 第三步：consumer 调用 provider
- 第四步：consumer 和 provider 都异步通知监控中心

![image-20210611211238389](/Users/cuweir/Library/Application Support/typora-user-images/image-20210611211238389.png)

**注册中心挂了可以继续通信吗？**

可以，因为刚开始初始化的时候，消费者会将提供者的地址等信息**拉取到本地缓存**，所以注册中心挂了可以继续通信。

#### 通信协议，Hessian，PB

**序列化**，就是把数据结构或者是一些对象，转换为二进制串的过程，而**反序列化**是将在序列化过程中所生成的二进制串转换成数据结构或者对象的过程。

duboo**支持不同的通信协议**

- dubbo 协议 `dubbo://`

默认，单一长链接，NIO异步通信，基于Hessian做序列化协议。场景是：传输数据量小（每次请求在 100kb 以内），但是并发量很高，以及服务消费者机器数远大于服务提供者机器数的情况。

为了要支持高并发场景，一般是服务提供者就几台机器，但是服务消费者有上百台，可能每天调用量达到上亿次！此时用长连接是最合适的，就是跟每个服务消费者维持一个长连接就可以，可能总共就 100 个连接。然后后面直接基于长连接 NIO 异步通信，可以支撑高并发请求。

长连接，通俗点说，就是建立连接过后可以持续发送请求，无须再建立连接。

![image-20210611211818437](/Users/cuweir/Library/Application Support/typora-user-images/image-20210611211818437.png)

而短连接，每次要发送请求之前，需要先重新建立一次连接。

![image-20210611211856573](/Users/cuweir/Library/Application Support/typora-user-images/image-20210611211856573.png)

- rmi 协议 `rmi://`

RMI 协议采用 JDK 标准的 java.rmi.* 实现，采用阻塞式短连接和 JDK 标准序列化方式。多个短连接，适合消费者和提供者数量差不多的情况，适用于文件的传输，一般较少用。

- hessian 协议 `hessian://`

Hessian 1 协议用于集成 Hessian 的服务，Hessian 底层采用 Http 通讯，采用 Servlet 暴露服务，Dubbo 缺省内嵌 Jetty 作为服务器实现。走 hessian 序列化协议，多个短连接，适用于提供者数量比消费者数量还多的情况，适用于文件的传输，一般较少用。

- http 协议 `http://`

基于 HTTP 表单的远程调用协议，采用 Spring 的 HttpInvoker 实现。走表单序列化。

- thrift 协议 `thrift://`

当前 dubbo 支持的 thrift 协议是对 thrift 原生协议的扩展，在原生协议的基础上添加了一些额外的头信息，比如 service name，magic number 等。

- webservice `webservice://`

基于 WebService 的远程调用协议，基于 Apache CXF 的 frontend-simple 和 transports-http 实现。走 SOAP 文本序列化。

- memcached 协议 `memcached://`

基于 memcached 实现的 RPC 协议。

- redis 协议 `redis://`

基于 Redis 实现的 RPC 协议。

- rest 协议 `rest://`

基于标准的 Java REST API——JAX-RS 2.0（Java API for RESTful Web Services 的简写）实现的 REST 调用支持。

- gPRC 协议 `grpc://`

Dubbo 自 2.7.5 版本开始支持 gRPC 协议，对于计划使用 HTTP/2 通信，或者想利用 gRPC 带来的 Stream、反压、Reactive 编程等能力的开发者来说， 都可以考虑启用 gRPC 协议。

**支持的序列化协议**

dubbo 支持 hession、Java 二进制序列化、json、SOAP 文本序列化多种序列化协议。但是 hessian 是其默认的序列化协议。

**Hessian数据结构**

Hessian 的对象序列化机制有 8 种原始类型：

- 原始二进制数据
- boolean
- 64-bit date（64 位毫秒值的日期）
- 64-bit double
- 32-bit int
- 64-bit long
- null
- UTF-8 编码的 string

另外还包括 3 种递归类型：

- list for lists and arrays
- map for maps and dictionaries
- object for objects

还有一种特殊的类型：

- ref：用来表示对共享对象的引用。

**为什么PB效率最高**

其实 PB 之所以性能如此好，主要得益于两个：**第一**，它使用 proto 编译器，自动进行序列化和反序列化，速度非常快，应该比 `XML` 和 `JSON` 快上了 `20~100` 倍；**第二**，它的数据压缩效果好，就是说它序列化后的数据量体积小。因为体积小，传输起来带宽和速度上会有优化。

#### 负载均衡，集群容错，动态代理

> 负载均衡

**RandomLoadBalance**

随机调用实现负载均衡，对provider不同实力设置不同的权重

**RoundRobinLoadBalance**

这个的话默认就是均匀地将流量打到各个机器上去，但是如果各个机器的性能不一样，容易导致性能差的机器负载过高。所以此时需要调整权重，让性能差的机器承载权重小一些，流量少一些。

**LeastActiveLoadBalance**

最小活跃数负载均衡，活跃调用数越小说明服务提供者效率越高，单位时间可处理更多的请求

**ConsistentHashLoadBalance**

一致哈希

------

>  容错机制

**Failover Cluster 模式**

失败自动切换，自动重试其他机器，**默认**就是这个，常见于读操作。（失败重试其它机器）

**Failfast Cluster 模式**

一次调用失败就立即失败，常见于非幂等性的写操作，比如新增一条记录（调用失败就立即失败）

**Failback Cluster 模式**

失败了后台自动记录请求，然后定时重发，比较适合于写消息队列这种。

**Forking Cluster 模式**

**并行调用**多个 provider，只要一个成功就立即返回。常用于实时性要求比较高的读操作，但是会浪费更多的服务资源，可通过 `forks="2"` 来设置最大并行数。

**Broadcast Cluster 模式**

逐个调用所有的 provider。任何一个 provider 出错则报错（从 `2.1.0` 版本开始支持）。通常用于通知所有提供者更新缓存或日志等本地资源信息。

> 动态代理策略

默认使用 javassist 动态字节码生成，创建代理类。但是可以通过 spi 扩展机制配置自己的动态代理策略。

#### SPI

service provider interface，需要**根据指定的配置**或者是**默认的配置**，去**找到对应的实现类**加载进来，然后用这个实现类的实例对象。

spi 机制一般用在哪儿？**插件扩展的场景**，比如说你开发了一个给别人使用的开源框架，如果你想让别人自己写个插件，插到你的开源框架里面，从而扩展某个功能，这个时候 spi 思想就用上了。

**Java spi**

spi 经典的思想体现，大家平时都在用，比如说 jdbc。

Java 定义了一套 jdbc 的接口，但是 Java 并没有提供 jdbc 的实现类。

但是实际上项目跑的时候，要使用 jdbc 接口的哪些实现类呢？一般来说，我们要**根据自己使用的数据库**，比如 mysql，你就将 `mysql-jdbc-connector.jar` 引入进来；oracle，你就将 `oracle-jdbc-connector.jar` 引入进来。

在系统跑的时候，碰到你使用 jdbc 的接口，他会在底层使用你引入的那个 jar 中提供的实现类。

**dubbo spi**

没有用jdk的spi，自己实现了

#### 服务治理，服务降级，失败重试

1. 调用链路自动生成
2. 服务访问压力以及时常统计
3. 服务分层（避免循环依赖）
4. 调用链路失败监控和报警
5. 服务鉴权
6. 每个服务的可用性的监控（接口调用成功率？几个 9？99.99%，99.9%，99%）

#### 如何自己设计类似Dubbo的RPC框架

最简单的回答思路：

- 上来你的服务就得去注册中心注册吧，你是不是得有个注册中心，保留各个服务的信息，可以用 zookeeper 来做，对吧。
- 然后你的消费者需要去注册中心拿对应的服务信息吧，对吧，而且每个服务可能会存在于多台机器上。
- 接着你就该发起一次请求了，咋发起？当然是基于动态代理了，你面向接口获取到一个动态代理，这个动态代理就是接口在本地的一个代理，然后这个代理会找到服务对应的机器地址。
- 然后找哪个机器发送请求？那肯定得有个负载均衡算法了，比如最简单的可以随机轮询是不是。
- 接着找到一台机器，就可以跟它发送请求了，第一个问题咋发送？你可以说用 netty 了，nio 方式；第二个问题发送啥格式数据？你可以说用 hessian 序列化协议了，或者是别的，对吧。然后请求过去了。
- 服务器那边一样的，需要针对你自己的服务生成一个动态代理，监听某个网络端口了，然后代理你本地的服务代码。接收到请求的时候，就调用对应的服务代码，对吧。

### CAP 定理（CAP theorem）

在理论计算机科学中，CAP 定理（CAP theorem），又被称作布鲁尔定理（Brewer's theorem），它指出对于一个分布式计算系统来说，不可能同时满足以下三点：

- 一致性（Consistency） （等同于所有节点访问同一份最新的数据副本）
- 可用性（Availability）（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）
- 分区容错性（Partition tolerance）（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在 C 和 A 之间做出选择。）
- 

#### 分区容错性（Partition tolerance）

理解 CAP 理论的最简单方式是想象两个节点分处分区两侧。允许至少一个节点更新状态会导致数据不一致，即丧失了 C 性质。如果为了保证数据一致性，将分区一侧的节点设置为不可用，那么又丧失了 A 性质。除非两个节点可以互相通信，才能既保证 C 又保证 A，这又会导致丧失 P 性质。

- P 指的是分区容错性，分区现象产生后需要容错，容错是指在 A 与 C 之间选择。如果分布式系统没有分区现象（没有出现不一致不可用情况） 本身就没有分区 ，既然没有分区则就更没有分区容错性 P。
- 无论我设计的系统是 AP 还是 CP 系统如果没有出现不一致不可用。 则该系统就处于 CA 状态
- P 的体现前提是得有分区情况存在

#### 框架对比

| 框架      | 所属 |
| --------- | ---- |
| Eureka    | AP   |
| Zookeeper | CP   |
| Consul    | CP   |

**Eureka**

> Eureka 保证了可用性，实现最终一致性。

Eureka 所有节点都是平等的所有数据都是相同的，且 Eureka 可以相互交叉注册。
Eureka client 使用内置轮询负载均衡器去注册，有一个检测间隔时间，如果在一定时间内没有收到心跳，才会移除该节点注册信息；如果客户端发现当前 Eureka 不可用，会切换到其他的节点，如果所有的 Eureka 都跪了，Eureka client 会使用最后一次数据作为本地缓存；所以以上的每种设计都是他不具备`一致性`的特性。

注意：因为 EurekaAP 的特性和请求间隔同步机制，在服务更新时候一般会手动通过 Eureka 的 api 把当前服务状态设置为`offline`，并等待 2 个同步间隔后重新启动，这样就能保证服务更新节点对整体系统的影响

**Zookeeper**

> 强一致性

Zookeeper 在选举 leader 时会停止服务，只有成功选举 leader 成功后才能提供服务，选举时间较长；内部使用 paxos 选举投票机制，只有获取半数以上的投票才能成为 leader，否则重新投票，所以部署的时候最好集群节点不小于 3 的奇数个（但是谁能保证跪掉后节点也是基数个呢）；Zookeeper 健康检查一般是使用 tcp 长链接，在内部网络抖动时或者对应节点阻塞时候都会变成不可用，这里还是比较危险的；

**Consul**

和 Zookeeper 一样数据 CP

Consul 注册时候只有过半的节点都写入成功才认为注册成功；leader 挂掉时，重新选举期间整个 Consul 不可用,保证了强一致性但牺牲了可用性

### 接口幂等性如何设计（不重复扣款）

其实保证幂等性主要是三点：

- 对于每个请求必须有一个唯一的标识，举个栗子：订单支付请求，肯定得包含订单 id，一个订单 id 最多支付一次，对吧。
- 每次处理完请求之后，必须有一个记录标识这个请求处理过了。常见的方案是在 mysql 中记录个状态啥的，比如支付之前记录一条这个订单的支付流水。
- 每次接收请求需要进行判断，判断之前是否处理过。比如说，如果有一个订单已经支付了，就已经有了一条支付流水，那么如果重复发送这个请求，则此时先插入支付流水，orderId 已经存在了，唯一键约束生效，报错插入不进去的。然后你就不用再扣款了。

实际运作过程中，你要结合自己的业务来，比如说利用 Redis，用 orderId 作为唯一键。只有成功插入这个支付流水，才可以执行实际的支付扣款。

要求是支付一个订单，必须插入一条支付流水，order_id 建一个唯一键 `unique key` 。你在支付一个订单之前，先插入一条支付流水，order_id 就已经进去了。你就可以写一个标识到 Redis 里面去， `set order_id payed` ，下一次重复请求过来了，先查 Redis 的 order_id 对应的 value，如果是 `payed` 就说明已经支付过了，你就别重复支付了。

### 接口请求的顺序性如何保证

首先，一般来说，个人建议是，你们从业务逻辑上设计的这个系统最好是不需要这种顺序性的保证，因为一旦引入顺序性保障，比如使用**分布式锁**，会**导致系统复杂度上升**，而且会带来**效率低下**，热点数据压力过大等问题。

首先你得用 Dubbo 的一致性 hash 负载均衡策略，将比如某一个订单 id 对应的请求都给分发到某个机器上去，接着就是在那个机器上，因为可能还是多线程并发执行的，你可能得立即将某个订单 id 对应的请求扔一个**内存队列**里去，强制排队，这样来确保他们的顺序性。

![image-20210611210712725](/Users/cuweir/Library/Application Support/typora-user-images/image-20210611210712725.png)

后续问题就很多，比如说要是某个订单对应的请求特别多，造成某台机器成**热点**怎么办？解决这些问题又要开启后续一连串的复杂技术方案.....

建议合并成一个操作，就是一个删除

# 14. 设计模式

## 单例模式

```java
public class TestInstance {
  private volatile static TestInstance instance;
  public static TestInstance getInstance() { //1
    if (instance == null) {         //2
      synchronized (TestInstance.class) {//3
        if (instance == null) { //4
          instance = new TestInstance();//5
        }
      }
    }
    return instance;//6
  }
}
```

volatile关键字，在第5行会出现问题

对于第5行

```java
instance = new TestInstance(); 
```

可以分解为3行伪代码

1 memory=allocate();// 分配内存 相当于c的malloc

2 ctorInstanc(memory) //初始化对象

3 instance=memory //设置instance指向刚分配的地址

上面的代码在编译器运行时，可能会出现重排序 从1-2-3 排序为1-3-2

如此在多线程下就会出现问题

例如现在有2个线程A,B

线程A在执行第5行代码时，B线程进来，而此时A执行了 1和3，没有执行2，此时B线程判断instance不为null 直接返回一个未初始化的对象，就会出现问题

而用了volatile，上面的重排序就会在多线程环境中禁止，不会出现上述问题。

## 观察者模式

观察者模式是定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。 **优点**：

- 观察者模式可以实现表示层和数据逻辑层的分离，并定义了稳定的消息更新传递机制，抽象了更新接口，使得可以有各种各样不同的表示层作为具体观察者角色；
- 观察者模式在观察目标和观察者之间建立一个抽象的耦合；
- 观察者模式支持广播通信；
- 观察者模式符合开闭原则（对拓展开放，对修改关闭）的要求。

**缺点**：

- 如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间；
- 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃；
- 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

在观察者模式中有如下角色：

- Subject：抽象主题（抽象被观察者），抽象主题角色把所有观察者对象保存在一个集合里，每个主题都可以有任意数量的观察者，抽象主题提供一个接口，可以增加和删除观察者对象；
- ConcreteSubject：具体主题（具体被观察者），该角色将有关状态存入具体观察者对象，在具体主题的内部状态发生改变时，给所有注册过的观察者发送通知；
- Observer：抽象观察者，是观察者者的抽象类，它定义了一个更新接口，使得在得到主题更改通知时更新自己；
- ConcrereObserver：具体观察者，实现抽象观察者定义的更新接口，以便在得到主题更改通知时更新自身的状态。

## 装饰器模式

**什么是装饰器模式？**

答：装饰器模式是指动态地给一个对象增加一些额外的功能，同时又不改变其结构。

优点：装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。

装饰器模式的关键：装饰器中使用了被装饰的对象。

比如，创建一个对象“laowang”，给对象添加不同的装饰，穿上夹克、戴上帽子......，这个执行过程就是装饰者模式

## 代理模式

**代理模式**？

代理模式是给某一个对象提供一个代理，并由代理对象控制对原对象的引用。

**优点**：

- 代理模式能够协调调用者和被调用者，在一定程度上降低了系统的耦合度；
- 可以灵活地隐藏被代理对象的部分功能和服务，也增加额外的功能和服务。

**缺点**：

- 由于使用了代理模式，因此程序的性能没有直接调用性能高；
- 使用代理模式提高了代码的复杂度。

举一个生活中的例子：比如买飞机票，由于离飞机场太远，直接去飞机场买票不太现实，这个时候我们就可以上携程 App 上购买飞机票，这个时候携程 App 就相当于是飞机票的代理商。

# 微服务

## 架构特征

1. 通过服务进行组件化

组件是可独立更换和升级的软件单元。将服务作为组件（而不是库）的一个主要原因是服务可以独立部署。

将服务用作组件的另一个结果是更明确的组件接口。通过使用显式远程调用机制，服务可以更轻松地避免客户端破坏组件封装。

2. 围绕业务能力进行组织

3. 产品心态

产品心态与业务能力的联系紧密相连。要持续关注软件如何帮助用户提升业务能力，而不是把软件看成是将要完成的一组功能。

没有理由说为什么这种方法不能用在单一应用程序上，但较小的服务粒度，使得它更容易在服务开发者和用户之间建立个人关系。

4. 智能端点和哑管

基于微服务构建的应用程序的目标是尽可能的解耦和尽可能的内聚——他们拥有自己的领域逻辑，他们的行为更像经典 UNIX 理念中的过滤器——接收请求，应用适当的逻辑并产生响应。使用简单的 REST 风格的协议来编排它们，而不是使用像 WS-Choreography 或者 BPEL 或者通过中心工具编制(orchestration)等复杂的协议。

5. 去中心化管理

6. 分散数据管理

7. 基建自动化

8. 故障准备
9. 演化设计

强调可替换性

## 事件驱动数据管理

当某件重要事情发生时，微服务会发布一个事件，例如更新一个业务实体。当订阅这些事件的微服务接收此事件时，就可以更新自己的业务实体，也可能会引发更多的时间发布。

事件驱动架构也是既有优点也有缺点，此架构可以使得交易跨多个服务且提供最终一致性，并且可以使应用维护最终视图；而缺点在于编程模式比 ACID 交易模式更加复杂：为了从应用层级失效中恢复，还需要完成补偿性交易，例如，如果信用检查不成功则必须取消订单；另外，应用必须应对不一致的数据，这是因为临时（in-flight）交易造成的改变是可见的，另外当应用读取未更新的最终视图时也会遇见数据不一致问题。另外一个缺点在于订阅者必须检测和忽略冗余事件。

### 原子操作

1. 使用本地交易发布事件

获得原子性的一个方法是对发布事件应用采用 multi-step process involving only local transactions，技巧在于一个 EVENT 表，此表在存储业务实体数据库中起到消息列表功能。应用发起一个（本地）数据库交易，更新业务实体状态，向 EVENT 表中插入一个事件，然后提交此次交易。另外一个独立应用进程或者线程查询此 EVENT 表，向消息代理发布事件，然后使用本地交易标志此事件为已发布。

2. 挖掘数据库交易日志

应用更新数据库，在数据库交易日志中产生变化，交易日志挖掘进程或者线程读这些交易日志，将日志发布给消息代理。

交易日志挖掘也是优缺点并存。优点是确保每次更新发布事件不依赖于 2PC。交易日志挖掘可以通过将发布事件和应用业务逻辑分离开得到简化；而主要缺点在于交易日志对不同数据库有不同格式，甚至不同数据库版本也有不同格式；而且很难从底层交易日志更新记录转换为高层业务事件。

3. 使用事件源

Event sourcing （事件源）通过使用根本不同的事件中心方式来获得不需 2PC 的原子性，保证业务实体的一致性。 这种应用保存业务实体一系列状态改变事件，而不是存储实体现在的状态。应用可以通过重放事件来重建实体现在状态。只要业务实体发生变化，新事件就会添加到时间表中。因为保存事件是单一操作，因此肯定是原子性的。

事件源方法有很多优点：解决了事件驱动架构关键问题，使得只要有状态变化就可以可靠地发布事件，也就解决了微服务架构中数据一致性问题。另外，因为是持久化事件而不是对象，也就避免了 object relational impedance mismatch problem。

数据源方法提供了 100%可靠的业务实体变化监控日志，使得获取任何时点实体状态成为可能。另外，事件源方法可以使得业务逻辑可以由事件交换的松耦合业务实体构成。这些优势使得单体应用移植到微服务架构变的相对容易。

事件源方法也有不少缺点，因为采用不同或者不太熟悉的变成模式，使得重新学习不太容易；事件存储只支持主键查询业务实体，必须使用 Command Query Responsibility Segregation (CQRS) 来完成查询业务，因此，应用必须处理最终一致数据。

## 好处与不足

好处：

微服务架构模式有很多好处。首先，通过分解巨大单体式应用为多个服务方法解决了复杂性问题。在功能不变的情况下，应用 被分解为多个可管理的分支或服务。每个服务都有一个用 RPC-或者消息驱动 API 定义清楚的边界。微服务架构模式给采用单体式编码方式很难实现的功能提供 了模块化的解决方案，由此，单个服务很容易开发、理解和维护。

第二，这种架构使得每个服务都可以有专门开发团队来开发。开发者可以自由选择开发技术，提供 API 服务。当然，许多公司试图避免混乱，只提供某些 技术选择。然后，这种自由意味着开发者不需要被迫使用某项目开始时采用的过时技术，他们可以选择现在的技术。甚至于，因为服务都是相对简单，即使用现在技 术重写以前代码也不是很困难的事情。

第三，微服务架构模式是每个微服务独立的部署。开发者不再需要协调其它服务部署对本服务的影响。这种改变可以加快部署速度。UI 团队可以采用 AB 测试，快速的部署变化。微服务架构模式使得持续化部署成为可能。

最后，微服务架构模式使得每个服务独立扩展。你可以根据每个服务的规模来部署满足需求的规模。甚至于，你可以使用更适合于服务资源需求的硬件。比 如，你可以在 EC2 Compute Optimized instances 上部署 CPU 敏感的服务，而在 EC2 memory-optimized instances 上部署内存数据库。

不足：

其中一个跟他的名字类似，『微服务』强调了服务大小，实际上，有一些开发者鼓吹建立稍微大一 些的，10-100 LOC 服务组。尽管小服务更乐于被采用，但是不要忘了这只是终端的选择而不是最终的目的。微服务的目的是有效的拆分应用，实现敏捷开发和部署。

另外一个主要的不足是，微服务应用是分布式系统，由此会带来固有的复杂性。开发者需要在 RPC 或者消息传递之间选择并完成进程间通讯机制。更甚 于，他们必须写代码来处理消息传递中速度过慢或者不可用等局部失效问题。当然这并不是什么难事，但相对于单体式应用中通过语言层级的方法或者进程调用，微 服务下这种技术显得更复杂一些。

另外一个关于微服务的挑战来自于分区的数据库架构。商业交易中同时给多个业务分主体更新消息很普遍。这种交易对于单体式应用来说很容易，因为只有 一个数据库。在微服务架构应用中，需要更新不同服务所使用的不同的数据库。使用分布式交易并不一定是好的选择，不仅仅是因为 CAP 理论，还因为今天高扩展 性的 NoSQL 数据库和消息传递中间件并不支持这一需求。最终你不得不使用一个最终一致性的方法，从而对开发者提出了更高的要求和挑战。

测试一个基于微服务架构的应用也是很复杂的任务。比如，采用流行的 Spring Boot 架构，对一个单体式 web 应用，测试它的 REST API，是很容易的事情。反过来，同样的服务测试需要启动和它有关的所有服务（至少需要这些服务的 stubs）。再重申一次，不能低估了采用微服务架构带 来的复杂性。

另外一个挑战在于，微服务架构模式应用的改变将会波及多个服务。比如，假设你在完成一个案例，需要修改服务 A、B、C，而 A 依赖 B，B 依赖 C。在 单体式应用中，你只需要改变相关模块，整合变化，部署就好了。对比之下，微服务架构模式就需要考虑相关改变对不同服务的影响。比如，你需要更新服务 C，然 后是 B，最后才是 A，幸运的是，许多改变一般只影响一个服务，而需要协调多服务的改变很少。

部署一个微服务应用也很复杂，一个分布式应用只需要简单在复杂均衡器后面部署各自的服务器就好了。每个应用实例是需要配置诸如数据库和消息中间件等基础服务。相对比，一个微服务应用一般由大批服务构成。

# 15. Nginx

## 为什么要用Nginx？

跨平台、配置简单、方向代理、高并发连接：处理2-3万并发连接数，官方监测能支持5万并发，内存消耗小：开启10个nginx才占150M内存 ，nginx处理静态文件好，耗费内存少，

而且Nginx内置的健康检查功能：如果有一个服务器宕机，会做一个健康检查，再发送的请求就不会发送到宕机的服务器了。重新将请求提交到其他的节点上。

使用Nginx的话还能：

节省宽带：支持GZIP压缩，可以添加浏览器本地缓存
稳定性高：宕机的概率非常小
接收用户请求是异步的
为什么Nginx性能这么高？
因为他的事件处理机制：异步非阻塞事件处理机制：运用了epoll模型，提供了一个队列，排队解决
**Nginx怎么处理请求的？**
nginx接收一个请求后，首先由listen和server_name指令匹配server模块，再匹配server模块里的location，location就是实际地址

```c
    server {            		    	# 第一个Server区块开始，表示一个独立的虚拟主机站点
        listen       80；      		        # 提供服务的端口，默认80
        server_name  localhost；    		# 提供服务的域名主机名
        location / {            	        # 第一个location区块开始
            root   html；       		# 站点的根目录，相当于Nginx的安装目录
            index  index.html index.htm；    	# 默认的首页文件，多个用空格分开
        }          				# 第一个location区块结果
    }           

```

 **什么是正向代理和反向代理？**

正向代理就是一个人发送一个请求直接就到达了目标的服务器
反方代理就是请求统一被Nginx接收，nginx反向代理服务器接收到之后，按照一定的规 则分发给了后端的业务处理服务器进行处理了

### Nginx如何处理HTTP请求

`Nginx`使用反应器模式。主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。单个线程可以提供数万个并发连接。

### 如何使用未定义的服务器名称来阻止处理请求?

只需将请求删除的服务器就可以定义为：

```
Server {

listen 80;

server_name “ “ ;

return 444;

}
```

这里，服务器名被保留为一个空字符串，它将在没有“主机”头字段的情况下匹配请求，而一个特殊的`Nginx`的非标准代码`444`被返回，从而终止连接。

### “反向代理服务器”的优点是什么?

反向代理服务器可以隐藏源服务器的存在和特征。它充当互联网云和web服务器之间的中间层。这对于安全方面来说是很好的，特别是当您使用web托管服务时。

### 最佳用途

`Nginx`服务器的最佳用法是在网络上部署动态`HTTP`内容，使用`SCGI`、`WSGI`应用程序服务器、用于脚本的`FastCGI`处理程序。它还可以作为负载均衡器。

### Master和Worker进程分别是什么?

`Master`进程：读取及评估配置和维持

`Worker`进程：处理请求

### 请解释你如何通过不同于80的端口开启Nginx?

为了通过一个不同的端口开启`Nginx`，你必须进入`/etc/Nginx/sites-enabled/`，如果这是默认文件，那么你必须打开名为`“default”`的文件。编辑文件，并放置在你想要的端口：

```
Like server { listen 81; }
```

### 请解释是否有可能将`Nginx`的错误替换为`502`错误、`503`?

`502` =错误网关

`503` =服务器超载

有可能，但是您可以确保`fastcgi_intercept_errors`被设置为`ON`，并使用错误页面指令。

```
Location / {
fastcgi_pass 127.0.01:9001;
fastcgi_intercept_errors on;
error_page 502 =503/error_page.html;
#…
}
```

### 在`Nginx`中，解释如何在`URL`中保留双斜线?

要在`URL`中保留双斜线，就必须使用`merge_slashes_off`;

语法:`merge_slashes [on/off]`

默认值: `merge_slashes on`

环境: `http，server`

### 请解释`ngx_http_upstream_module`的作用是什么?

`ngx_http_upstream_module`用于定义可通过`fastcgi`传递、`proxy`传递、`uwsgi`传递、`memcached`传递和scgi传递指令来引用的服务器组。

### 请解释什么是`C10K`问题?

`C10K`问题是指无法同时处理大量客户端(10,000)的网络套接字。

### 请陈述`stub_status`和`sub_filter`指令的作用是什么?

`Stub_status`指令：该指令用于了解`Nginx`当前状态的当前状态，如当前的活动连接，接受和处理当前读/写/等待连接的总数

`Sub_filter`指令：它用于搜索和替换响应中的内容，并快速修复陈旧的数据

### 解释`Nginx`是否支持将请求压缩到上游?

您可以使用`Nginx`模块`gunzip`将请求压缩到上游。`gunzip`模块是一个过滤器，它可以对不支持“gzip”编码方法的客户机或服务器使用“内容编码:gzip”来解压缩响应。

### 解释如何在`Nginx`中获得当前的时间?

要获得Nginx的当前时间，必须使用`SSI`模块、`$date_gmt`和`$date_local`的变量。

`Proxy_set_header` `THE-TIME $date_gmt`;

### 用`Nginx`服务器解释`-s`的目的是什么?

用于运行`Nginx -s`参数的可执行文件。

### 解释如何在`Nginx`服务器上添加模块?

在编译过程中，必须选择`Nginx`模块，因为`Nginx`不支持模块的运行时间选择。

## 为什么Nginx性能好

它的高性能正是由于其优秀的架构设计，其架构主要包括这几点：模块化设计、事件驱动架构、请求的多阶段异步处理、管理进程与多工作进程设计、内存池的设计、**线程池的设计以及优秀的数据结构引用。**

### 模块化设计

高度模块化的设计是 Nginx 的架构基础。在 Nginx 中，除了少量的核心代码，其他一切皆为模块。

所有模块间是分层次、分类别的，Nginx 官方共有五大类型的模块：核心模块、配置模块、事件模块、HTTP 模块、mail 模块。它们之间的关系如下：

![image-20210616164241234](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616164241234.png)

在这 5 种模块中，配置模块和核心模块是与 Nginx 框架密切相关的。而事件模块则是 HTTP 模块和 mail 模块的基础。HTTP 模块和 mail 模块的 “地位” 类似，它们都是更关注于应用层面。在事件模块中，ngx_event_core_module事件模块时其他所有事件模块的基础；在HTTP模块中，ngx_http_core_module事件模块时其他所有HTTP模块的基础；在mail模块中，ngx_mail_core_module事件模块时其他所有mail模块的基础。

### 事件驱动架构

事件驱动架构，简单的说就是由一些事件发生源来产生事件，由事件收集器来收集、分发事件，然后由事件处理器来处理这些事件（事件处理器需要先在事件收集器里注册自己想处理的事件）。

对于 Nginx 服务器而言，一般由网卡、磁盘产生事件，Nginx 中的事件模块将负责事件的收集、分发操作；而所有的模块都可能是事件消费者，它们首先需要向事件模块注册感兴趣的事件类型，这样，在有事件产生时，事件模块会把事件分发到相应的模块中进行处理。

对于传统 web 服务器（如 Apache）而言，采用的所谓事件驱动往往局限在 TCP 连接建立、关闭事件上，一个连接建立以后，在其关闭之前的所有操作都不再是事件驱动，这时会退化成按顺序执行每个操作的批处理模式，这样每个请求在连接建立后都将始终占用着系统资源，直到关闭才会释放资源。这种请求占用着服务器资源等待处理的模式会造成服务器资源极大的浪费。如下图所示，传统 web 服务器往往把一个进程或线程作为时间消费者，当一个请求产生的事件被该进程处理时，直到这个请求处理结束时，进程资源都将被这一请求所占用。比较典型的例子如 Apache 同步阻塞的多进程模式就是这样的。

传统 web 服务器处理事件的简单模型（矩形代表进程）:

![image-20210616164532995](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616164532995.png)

Nginx 采用事件驱动架构处理业务的方式与传统的 web 服务器是不同的。它不使用进程或者线程来作为事件消费者，所谓的事件消费者只能是某个模块。只有事件收集、分发器才有资格占用进程资源，它们会在分发某个事件时调用事件消费模块使用当前占用的进程资源，如下图所示，该图中列出了 5 个不同的事件，在事件收集、分发者进程的一次处理过程中，这 5 个事件按照顺序被收集后，将开始使用当前进程分发事件，从而调用相应的事件消费者来处理事件。当然，这种分发、调用也是有序的。

Nginx 处理事件的简单模型：

![image-20210616164629553](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616164629553.png)

由上图可以看出，处理请求事件时，Nginx 的事件消费者只是被事件分发者进程短期调用而已，这种设计使得网络性能、用户感知的请求时延都得到了提升，每个用户的请求所产生的事件会及时响应，整个服务器的网络吞吐量都会由于事件的及时响应而增大。当然，这也带来一定的要求，即每个事件消费者都不能有阻塞行为，否则将会由于长时间占用事件分发者进程而导致其他事件得不到及时响应，Nginx 的非阻塞特性就是由于它的模块都是满足这个要求的。

### 请求的多阶段异步处理

多阶段异步处理请求与事件驱动架构是密切相关的，也就是说，请求的多阶段异步处理只能基于事件驱动架构实现。多阶段异步处理就是把一个请求的处理过程按照事件的触发方式划分为多个阶段，每个阶段都可以由事件收集、分发器来触发。

处理获取静态文件的 HTTP 请求时切分的阶段及各阶段的触发事件如下所示：

![image-20210616164710838](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616164710838.png)

这个例子中，该请求大致分为 7 个阶段，这些阶段是可以重复发生的，因此，一个下载静态资源请求可能会由于请求数据过大，网速不稳定等因素而被分解为成百上千个上图所列出的阶段。

异步处理和多阶段是相辅相成的，只有把请求分为多个阶段，才有所谓的异步处理。当一个时间被分发到事件消费者中进行处理时，事件消费者处理完这个事件只相当于处理完 1 个请求的阶段。什么时候可以处理下一个阶段呢？这只能等待内核的通知，即当下一次事件出现时，epoll 等事件分发器将会获取到通知，然后去调用事件消费者进行处理。

请求的多阶段异步处理优势在哪里呢？这种设计配合事件驱动架构，将会极大地提高网络性能，同时使得每个进程都能全力运转，不会或者尽量少地出现进程休眠状况，因为一旦出现进程休眠，必然减少兵法处理事件的数目，一定会降低网络性能，同时会增加请求处理时间的平均时延，这时，如果网络性能无法满足业务需求将只能增加进程数目，进程数目过多就会增加操作系统内核的额外操作：进程间切换，可是频繁地进行进程间的切换仍会消耗CPU等资源，从而降低网络性能。同时，休眠的进程会使进程占用的内存得不到有效释放，这最终必然导致可用内存的下降，从而影响系统能够处理的最大并发连接数。

### 管理进程与多工作进程设计

Nginx 在启动后，会有一个 master 进程和多个 worker 进程。master 进程主要用来管理 worker 进程，包括接收来自外界的信号，向各 worker 进程发送信号，监控 worker 进程的运行状态以及启动 worker 进程。 worker 进程是用来处理来自客户端的请求事件。多个 worker 进程之间是对等的，它们同等竞争来自客户端的请求，各进程互相独立，一个请求只能在一个 worker 进程中处理。worker 进程的个数是可以设置的，一般会设置与机器 CPU 核数一致，这里面的原因与事件处理模型有关。Nginx 的进程模型，可由下图来表示：

![image-20210616181726802](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616181726802.png)

在服务器上查看 Nginx 进程：

![image-20210616181804337](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616181804337.png)

这种设计带来以下优点：

1） 利用多核系统的并发处理能力

现代操作系统已经支持多核 CPU 架构，这使得多个进程可以分别占用不同的 CPU 核心来工作。Nginx 中所有的 worker 工作进程都是完全平等的。这提高了网络性能、降低了请求的时延。

2） 负载均衡

多个 worker 工作进程通过进程间通信来实现负载均衡，即一个请求到来时更容易被分配到负载较轻的 worker 工作进程中处理。这也在一定程度上提高了网络性能、降低了请求的时延。

3） 管理进程会负责监控工作进程的状态，并负责管理其行为

管理进程不会占用多少系统资源，它只是用来启动、停止、监控或使用其他行为来控制工作进程。首先，这提高了系统的可靠性，当 worker 进程出现问题时，管理进程可以启动新的工作进程来避免系统性能的下降。其次，管理进程支持 Nginx 服务运行中的程序升级、配置项修改等操作，这种设计使得动态可扩展性、动态定制性较容易实现。

### 内存池的设计

为了避免出现内存碎片，减少向操作系统申请内存的次数、降低各个模块的开发复杂度，Nginx 设计了简单的内存池，它的作用主要是把多次向系统申请内存的操作整合成一次，这大大减少了 CPU 资源的消耗，同时减少了内存碎片。

因此，通常每一个请求都有一个简易的独立内存池（如每个 TCP 连接都分配了一个内存池），而在请求结束时则会销毁整个内存池，把曾经分配的内存一次性归还给操作系统。这种设计大大提高了模块开发的简单些，因为在模块申请内存后不用关心它的释放问题；而且因为分配内存次数的减少使得请求执行的时延得到了降低。同时，通过减少内存碎片，提高了内存的有效利用率和系统可处理的并发连接数，从而增强了网络性能。

线程池的引用

通常情况下，NGINX 是一个事件处理器，即一个接收来自内核的所有连接事件的信息，然后向操作系统发出做什么指令的控制器。实际上，NGINX 干了编排操作系统的全部脏活累活，而操作系统做的是读取和发送字节这样的日常工作。所以，对于 NGINX 来说，快速和及时的响应是非常重要的。

![image-20210616182447337](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616182447337.png)

![image-20210616182513694](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616182513694.png)

但是，如果 NGINX 要处理的操作是一些又长又重的操作，譬如读取本地磁盘文件，又会发生什么呢？整个事件处理循环将会卡住，等待这个操作执行完毕。

因此，所谓“阻塞操作”是指任何导致事件处理循环显著停止一段时间的操作。操作可以由于各种原因成为阻塞操作。例如，NGINX 可能因长时间、CPU 密集型处理，或者可能等待访问某个资源（比如硬盘，或者一个互斥体，亦或要从处于同步方式的数据库获得相应的库函数调用等）而繁忙。关键是在处理这样的操作期间，工作进程无法做其他事情或者处理其他事件，即使有更多的可用系统资源可以被队列中的一些事件所利用。

在 NGINX 中会发生几乎同样的情况，比如当读取一个文件的时候，如果该文件没有缓存在内存中，就要从磁盘上读取。从磁盘（特别是旋转式的磁盘）读取是很慢的, 而当队列中等待的其他请求可能不需要访问磁盘时，它们也得被迫等待。导致的结果是，延迟增加并且系统资源没有得到充分利用。

![image-20210616182556724](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616182556724.png)

![image-20210616182624593](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616182624593.png)

对 NGINX 而言，线程池执行的就是配货服务的功能。它由一个任务队列和一组处理这个队列的线程组成。

当工作进程需要执行一个潜在的长操作时，工作进程不再自己执行这个操作，而是将任务放到线程池队列中，任何空闲的线程都可以从队列中获取并执行这个任务。

在极端场景下（活跃数据占满了内存），线程池可以将静态资源服务的性能提升9倍，具体测试可以参见这篇文章：https://www.nginx.com/blog/thread-pools-boost-performance-9x/。

### 优秀的数据结构引用

**红黑树**

红黑树是一种自平衡二叉查找树，普通的二叉查找树在极端情况可能会退化成单链表，操作效率低下。那么什么是自平衡二叉查找树？在不断的向二叉查找树种添加、删除节点时，二叉查找树自身通过形态的转换，始终保持一定程度上的平衡，即为自平衡二叉查找树。红黑树就是一种自平衡性较好的二叉查找树，它支持快速的快速的检索、插入、删除操作，同时支持范围查询、遍历操作，是一种应用场景很广泛的高级数据结构。

![image-20210616182729565](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616182729565.png)

特点: 

- 高度不会超过2倍的log(n)
- 增删改查算法复杂度O(log(n))
- 遍历复杂度O(n)

在nginx内部，有很多模块都用到了红黑树。

以ngx_http_limit_req_module为例。

先来看下它的定义：

```
typedef struct {
  ngx_rbtree_t        *rbtree;
} ngx_stream_limit_conn_ctx_t;
```

从上面的定义中可以看到，这个模块上下文其实就是定义了一颗红黑树，从后面的功能实现我们会发现Nginx就是利用这棵红黑树来组织不同客户端ip的并发连接信息的，一个客户端ip对应一个红黑树节点，红黑树的节点的内存都是从limit_conn_zone命令定义的共享内存中获取的。为什么要使用共享内存呢？因为一个客户端的多个并发连接请求不一定会被同一个worker子进程处理，所以需要用共享内存来存储同一个ip的并发连接信息已使所有的子进程都可见。

**链表**

以双向链表为例。

ngx_queue_t双向链表是Nginx提供的轻量级链表容器，链表作为顺序容器的优势在于，它可以高效地执行插入、删除、合并等操作，在移动链表中的元素时只需要修改指针的指向，因此，它很适合频繁修改容器的场合。

![image-20210616183013936](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616183013936.png)

**动态数组**

ngx_array_t是一个顺序容器，以数组的形式存储元素，并支持在达到数组容量的上限时动态改变数组的带下。 

数组的优势是它的访问速度。由于它使用一块完整的内存，并按照固定大小存储每一个元素，所以在访问数组的任意一个元素时，都可以根据下标直接寻址找到它，另外，数组的访问速度时常量级的，在所有的数据结构中它的速度都是最快的。但是，当数组大小无法确认时，动态数组就“登场”了，当数组的大小达到已经分配内存的上限时，会自动扩充数组的大小。

因此有以下3个优点：

1. 访问速度快
2. 允许元素个数具备不确定性
3. 负责元素占用内存的分配，这些内存将由内存池统一管理。

![image-20210616183039514](/Users/cuweir/Library/Application Support/typora-user-images/image-20210616183039514.png)



# 16. 网络

## TCP

**三次握手**

![image-20210617002145386](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617002145386.png)

假设 A 为客户端，B 为服务器端。

- 首先 B 处于 LISTEN（监听）状态，等待客户的连接请求。
- A 向 B 发送连接请求报文，SYN=1，ACK=0，选择一个初始的序号 x。
- B 收到连接请求报文，如果同意建立连接，则向 A 发送连接确认报文，SYN=1，ACK=1，确认号为 x+1，同时也选择一个初始的序号 y。
- A 收到 B 的连接确认报文后，还要向 B 发出确认，确认号为 y+1，序号为 x+1。
- B 收到 A 的确认后，连接建立。

**三次握手的原因**

第三次握手是为了防止失效的连接请求到达服务器，让服务器错误打开连接。

![image-20210621212222660](/Users/cuweir/Library/Application Support/typora-user-images/image-20210621212222660.png)

在两次握手中服务端不知道当前这个SYN是不是有效的，三次握手就很好的解决了这个问题，第三次握手就是客户端给服务端回复第二次握手，这也就是说服务端会等第三次握手的到来，如果第三次握手迟迟不来，服务端就可以识别这个SYN是无效的，就会将他的资源释放了。还有一种情况就是第三次握手由于网络中的种种原因失败了，这时候客户端认为自己已经连接好了，就会给服务端发送数据，服务端由于没有收到第三次握手，就会以RST包对客户端响应，收到RST的的客户端就知道第三次握手没有成功，就会重新连接。在谢希仁著《计算机网络》第四版中讲“三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。




**四次挥手**

以下描述不讨论序号和确认号，因为序号和确认号的规则比较简单。并且不讨论 ACK，因为 ACK 在连接建立之后都为 1。

- A 发送连接释放报文，FIN=1。
- B 收到之后发出确认，此时 TCP 属于半关闭状态，B 能向 A 发送数据但是 A 不能向 B 发送数据。
- 当 B 不再需要连接时，发送连接释放报文，FIN=1。
- A 收到后发出确认，进入 TIME-WAIT 状态，等待 2 MSL（最大报文存活时间）后释放连接。
- B 收到 A 的确认后释放连接。

**四次挥手的原因**

客户端发送了 FIN 连接释放报文之后，服务器收到了这个报文，就进入了 CLOSE-WAIT 状态。这个状态是为了让服务器端发送还未传送完毕的数据，传送完毕之后，服务器会发送 FIN 连接释放报文。

![image-20210617004455257](/Users/cuweir/Library/Application Support/typora-user-images/image-20210617004455257.png)

**TIME_WAIT**

客户端接收到服务器端的 FIN 报文后进入此状态，此时并不是直接进入 CLOSED 状态，还需要等待一个时间计时器设置的时间 2MSL。这么做有两个理由：

- 确保最后一个确认报文能够到达。如果 B 没收到 A 发送来的确认报文，那么就会重新发送连接释放请求报文，A 等待一段时间就是为了处理这种情况的发生。
- 等待一段时间是为了让本连接持续时间内所产生的所有报文都从网络中消失，使得下一个新的连接不会出现旧的连接请求报文。

## HTTP

**HTTP协议**是Hyper Text Transfer Protocol（超文本传输协议）的缩写。HTTP是万维网的数据通信的基础。HTTP是一个应用层协议，**通常**运行在TCP协议之上。它由请求和响应构成，是一个标准的客户端服务器模型（C/S模型）。HTTP是一个**无状态**的协议。

**无状态**怎么解释？HTTP协议永远都是客户端发起请求，服务器回送响应。每次连接只处理一个请求，当服务器返回本次请求的应答后便立即关闭连接，下次请求客户端再重新建立连接。也就无法实现在客户端没有发起请求的时候，服务器主动将消息推送给客户端。

HTTP协议运行在TCP协议之上，它无状态会导致客户端的每次请求都需要重新建立TCP连接，接受到服务端响应后，断开TCP连接。对于每次建立、断开TCP连接，还是有相当的性能损耗的。**那么，如何才能尽可能的减少性能损耗呢？**

### keep-alive

HTTP中是keep-alive，TCP中是keepalive

正如上面提出的问题：**在双方长时间未通讯时，如何得知对方还活着？如何得知这个TCP连接是健康且具有通讯能力的？**

**TCP的保活机制**就是用来解决此类问题，这个机制我们也可以称作：**keepalive**。保活机制默认是关闭的，TCP连接的任何一方都可打开此功能。有三个主要配置参数用来控制保活功能。

如果在一段时间（**保活时间：tcp_keepalive_time**）内此连接都不活跃，开启保活功能的一端会向对端发送一个保活探测报文。

- 若对端正常存活，且连接有效，对端必然能收到探测报文并进行响应。此时，发送端收到响应报文则证明TCP连接正常，重置保活时间计数器即可。
- 若由于网络原因或其他原因导致，发送端无法正常收到保活探测报文的响应。那么在一定**探测时间间隔（tcp_keepalive_intvl）**后，将继续发送保活探测报文。直到收到对端的响应，或者达到配置的**探测循环次数上限（tcp_keepalive_probes）**都没有收到对端响应，这时对端会被认为不可达，TCP连接随存在但已失效，需要将连接做中断处理。

在探测过程中，对端主机会处于以下四种状态之一：

![image-20210621213110684](/Users/cuweir/Library/Application Support/typora-user-images/image-20210621213110684.png)

当TCP空闲一定时间后会发送心跳包给对方，

如果对端回复ACK后，就认为对端是存活的，重置定时器；

如果对端回复RST应答（对端崩溃或者其他原因，导致的复位），那就关闭该连接；

如果对端无任何回应，那就会出发超时重传，直到达到重传的次数，如果对端依然没有回复，那么就关闭该连接。

**http的keep-alive**

它的特点是：客户端的每一次请求都要和服务端创建TCP连接，服务器响应后，断开TCP连接。下次客户端再有请求，则重新建立连接。

在早期的http1.0中，默认就是上述介绍的这种“请求-应答”模式。这种方式频繁的创建连接和销毁连接无疑是有一定性能损耗的。

所以引入了**keep-alive**机制。http1.0默认是关闭的，通过http请求头设置“connection: keep-alive”进行开启；http1.1中默认开启，通过http请求头设置“connection: close”关闭。

**keep-alive**机制：若开启后，在一次http请求中，服务器进行响应后，不再直接断开TCP连接，而是将TCP连接维持一段时间。在这段时间内，如果同一客户端再次向服务端发起http请求，便可以复用此TCP连接，向服务端发起请求，并重置timeout时间计数器，在接下来一段时间内还可以继续复用。这样无疑省略了反复创建和销毁TCP连接的损耗。

启用HTTP keep-Alive的优缺点： 优点：keep-alive机制避免了频繁建立和销毁连接的开销。 同时，减少服务端TIME_WAIT状态的TCP连接的数量(因为由服务端进程主动关闭连接) 缺点：若keep-alive timeout设置的时间较长，长时间的TCP连接维持，会一定程度的浪费系统资源。

总体而言，HTTP keep-Alive的机制还是利大于弊的，只要合理使用、配置合理的timeout参数。

# 00. 算法

## 01. 数组

### 两数之和

经典，输入一个数组和一个目标数，输出数组中和为目标数的索引数组

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1]
```

解法：

哈希表，key是数，value是索引

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            map.put(nums[i], i);
        }
        int[] result = new int[2];
        for (int i = 0; i < nums.length; ++i) {
            int tmp = target - nums[i];
            // 如果map中有符合条件的数，且不是他自己
            if (map.containsKey(tmp) && map.get(tmp) != i) {
                // 从小到大输出
                if (i < map.get(tmp)) {
                    result[0] = i;
                    result[1] = map.get(tmp);
                } else {
                    result[0] = map.get(tmp);
                    result[1] = i;
                }
            }
        }
        return result;
    }
}
```

### 三叔之和

三数之和，输入数组，输出和为零的全部子数组

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

解法：

先排序，再遍历数组，时间复杂度n平方

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; ++i) {
            int low = i + 1, high = nums.length - 1, sum = -nums[i];
            if (i == 0 || nums[i] != nums[i - 1]) {
                while (low < high) {
                    if (nums[low] + nums[high] == sum) {
                        result.add(Arrays.asList(nums[low], nums[high], nums[i]));
                        while (low < high && nums[low] == nums[low+1]) {
                            low++;
                        }
                        while (low < high && nums[high] == nums[high-1]) {
                            high--;
                        }
                        low++;
                        high--;
                    } else if (nums[low] + nums[high] < sum) {
                        low++;
                    } else {
                        high--;
                    }
                }
            }
        }
        return result;
}
}
```

### 买卖股票最佳时机

输入股票值，输出最大收益

解法：

循环迭代，每次计算当前值减去最小值，更新最小值

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) return 0;
        int min = prices[0];
        int result = 0;
        for (int i : prices) {
            result = Math.max(result, i - min);
            min = min < i ? min : i;
        }
        return result;
    }
}
```

### 数组中第K个最大的数

输入一个数组和一个数k，求数组第k个大的数

解法：

第一种办法，先排序再返回第k个数，时间复杂度n*logn，空间复杂度1：

```java
public int findKthLargest(int[] nums, int k) {
        final int N = nums.length;
        Arrays.sort(nums);
        return nums[N - k];
}
```

第二种办法，使用优先队列，时间复杂度n*logk，空间复杂度k：

```java
public int findKthLargest(int[] nums, int k) {
    final PriorityQueue<Integer> pq = new PriorityQueue<>();
    for(int val : nums) {
        pq.offer(val);

        if(pq.size() > k) {
            pq.poll();
        }
    }
    return pq.peek();
}
```

第三种方法，基于选择算法，和快排类似时间复杂度最好n，最差n平方，空间复杂度1：

```java
public int findKthLargest(int[] nums, int k) {

        k = nums.length - k;
        int lo = 0;
        int hi = nums.length - 1;
        while (lo < hi) {
            final int j = partition(nums, lo, hi);
            // 如果说明第k个数在j右边
            if(j < k) {
                lo = j + 1;
            } else if (j > k) { // k在j左边
                hi = j - 1;
            } else { // 相等说明找到了
                break;
            }
        }
        return nums[k];
    }

    private int partition(int[] a, int lo, int hi) {

        int i = lo;
        int j = hi + 1;
        while(true) {
            while(i < hi && less(a[++i], a[lo]));
            while(j > lo && less(a[lo], a[--j]));
            if(i >= j) {
                break;
            }
            exch(a, i, j);
        }
        exch(a, lo, j);
        return j;
    }

    private void exch(int[] a, int i, int j) {
        final int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }

    private boolean less(int v, int w) {
        return v < w;
    }
```

### 旋转数组查找

给一个旋转数组和target值，求target的索引

解法：

<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210606230223171.png" alt="image-20210606230223171" style="zoom:35%;" />

一个旋转数组，主元是pivot，问题的关键取决于mid是在主元左边还是右边

- 如果整个左侧部分是单调增加的，这意味着pivot位于右侧部分
  - 如果low<=target<high，丢弃右半部分
  - 否则丢左半部分

- 如果整个右侧部分是单调增加，这意味着pivot位于左侧部分
  - 如果mid<target<=右,丢左半部分
  - 否则掉一半

```java
public class Solution {
public int search(int[] A, int target) {
    int lo = 0;
    int hi = A.length - 1;
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (A[mid] == target) return mid;
        // 如果low小于等于mid，有两种情况
        if (A[lo] <= A[mid]) {
            // 1. target大于等于low且小于mid，说明在low和mid之间
            if (target >= A[lo] && target < A[mid]) {
                hi = mid - 1;
            } else { // 2. 否则不在
                lo = mid + 1;
            }
        } else {// 如果low大于mid
            // 1. 如果target大于mid且target小于等于high，说明在mid和high之间
            if (target > A[mid] && target <= A[hi]) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
    }
    return A[lo] == target ? lo : -1;
}
```

### 两个正序数组的中位数

解法：

递归

```java
class Solution {
 public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int l1 = nums1.length;
        int l2 = nums2.length;
        if (l1 > l2) {
            return findMedianSortedArrays(nums2, nums1);
        }
        int i = 0, j = 0, imin = 0, imax = l1, half = (l1 + l2 + 1) / 2;
        double maxLeft = 0, minRight = 0;
        while (imin <= imax) {
            i = (imin + imax) / 2;
            j = half - i;
            if (j > 0 && i < l1 && nums2[j - 1] > nums1[i]) {
                imin = i + 1;
            } else if (i > 0 && j < l2 && nums1[i - 1] > nums2[j]) {
                imax = i - 1;
            } else {
                if (i == 0) {
                    maxLeft = (double) nums2[j - 1];
                } else if (j == 0) {
                    maxLeft = (double) nums1[i - 1];
                } else {
                    maxLeft = (double) Math.max(nums1[i - 1], nums2[j - 1]);
                }
                break;
            }
        }
        if ((l1 + l2) % 2 == 1) {
            return maxLeft;
        }
        if (i == l1) {
            minRight = nums2[j];
        } else if (j == l2) {
            minRight = nums1[i];
        } else {
            minRight = Math.min(nums1[i], nums2[j]);
        }

        return (maxLeft + minRight) / 2;
    }
}
```

### 柱状图的最大矩形

```java
class Solution {
    public int largestRectangleArea(int[] height) {
                if (height == null || height.length == 0) {
            return 0;
        }
        int[] lessFromLeft = new int[height.length]; // 左边比当前值小的第一个数的索引
        int[] lessFromRight = new int[height.length];
        lessFromRight[height.length - 1] = height.length;
        for (int i = 1; i < height.length; ++i) {
            int p = i - 1;
            while (p >= 0 && height[p] > height[i]) {
                p = lessFromLeft[p];
            }
            lessFromLeft[i] = p;
        }

        for (int j = height.length - 2; j >= 0; --j) {
            int p = j + 1;
            while (p < height.length && height[p] >= height[j]) {
                p = lessFromRight[p];
            }
            lessFromRight[j] = p;
        }
        int maxArea = 0;
        for (int i = 0; i < height.length; i++) {
            maxArea = Math.max(maxArea, height[i] * (lessFromRight[i] - lessFromLeft[i] - 1));
        }

        return maxArea;
    }
}
```

### 最大子数组

已知一个数组，求子数组和的最大值

解法：

动态规划，dp[i]是最大子数组，必须以nums[i]结尾

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null && nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int max = dp[0];
        for (int i = 1; i < nums.length; ++i) {
            dp[i] = Math.max(dp[i-1] + nums[i], nums[i]);
            max = Math.max(dp[i], max);
        }
        return max;
    }
}	
```



## 02. 链表

### 反转链表

```java
// 迭代
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre = head, now = head.next;
        pre.next = null;
        while (now != null) {
            ListNode next = now.next;
            now.next = pre;
            pre = now;
            now = next;
        }
        return pre;
    }
}
```

```java
// 递归
public class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null ||head.next == null){
            return head;
        }
        
        ListNode root = reverseList(head.next);
        
        head.next.next = head;
        head.next = null;
        return root;
    }
}
```

### 旋转链表

给一个链表和一个k值，求从k位置开始旋转后的链表

解法：

先知道链表长度，然后把第（l-n%l）个节点后面的链表移到前面，完成循环。

```java
public ListNode rotateRight(ListNode head, int n) {
    if (head==null||head.next==null) return head;
    ListNode dummy=new ListNode(0);
    dummy.next=head;
    ListNode fast=dummy,slow=dummy;

    int i;
    for (i=0;fast.next!=null;i++)//Get the total length 
    	fast=fast.next;
    
    for (int j=i-n%i;j>0;j--) //Get the i-n%i th node
    	slow=slow.next;
    
    fast.next=dummy.next; //Do the rotation
    dummy.next=slow.next;
    slow.next=null;
    
    return dummy.next;
}
```



## 03. 字符串

### Decode-Ways

二十六个字母对应26个数字，给一串数字，求解码这个数字一共有几种方法

解法：

动态规划，dp[i]是前i个字符串共有几个方法

```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1; // 空字符串，1种方法
        // 判断只有一个字符时
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 2; i < n + 1; ++i) {
            // 第i个数
            int first = Integer.parseInt(s.substring(i - 1, i));
            // 第i-1和第i个数
            int second = Integer.parseInt(s.substring(i - 2, i));
            if (first >= 1 && first <= 9) {
                dp[i] += dp[i - 1];
            }
            if (second >= 10 && second <= 26) {
                dp[i] += dp[i - 2];
            }
        }
        return dp[n];
    }
}
```

### 验证IP地址

给一个IP地址，输出IPv4或者IPv6或者Neither

解法：



```java
class Solution {
    public static String validIPAddress(String IP) {
        if (isIPv4(IP)) {
            return "IPv4";
        }
        if (isIPv6(IP)) {
            return "IPv6";
        }
        return "Neither";
    }

    static boolean isIPv4(String IP) {
        if (!IP.contains(".")) {
            return false;
        }
        if (!String.valueOf(IP.charAt(0)).matches("[0-9]") || !String.valueOf(IP.charAt(IP.length()-1)).matches("[0-9]")) {
            return false;
        }
        String[] ips = IP.split("\\.");
        if (ips.length != 4) {
            return false;
        }
        for (String ip : ips) {
            if (ip.length() > 1 && ip.charAt(0) == '0') {
                return false;
            }
             if (!ip.matches("[0-9]{1,3}")) {
                return false;
            }
            int num = Integer.parseInt(ip);
            if (num < 0 || num > 255) {
                return false;
            }
        }
        return true;
    }

    static boolean isIPv6(String IP) {
        if (!IP.contains(":")) {
            return false;
        }
        if (!String.valueOf(IP.charAt(0)).matches("[0-9a-fA-F]") || !String.valueOf(IP.charAt(IP.length()-1)).matches("[0-9a-fA-F]")) {
            return false;
        }
        String[] ips = IP.split(":");
        if (ips.length != 8) {
            return false;
        }
        for (String ip : ips) {
            if (!ip.matches("[0-9a-fA-F]{1,4}")) {
                return false;
            }
        }
        return true;
    }
}
```



## 04. 树

### 二叉树最大路径和

求二叉树中两个节点间路径和的最大值，不需要经过根节点

解法：

递归查找最大路径

```java
class Solution {

    int result;
    
    public int maxPathSum(TreeNode root) {
        result = Integer.MIN_VALUE;
        maxPath(root);
        return result;
    }
    // 递归方法，返回值是可以传递到父节点的最大路径和
    public int maxPath(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = Math.max(0, maxPath(root.left));
        int right = Math.max(0, maxPath(root.right));
        result = Math.max(result, left + right + root.val);
        return Math.max(left, right) + root.val;
    }
}
```

### 二叉树最近公共祖先

递归

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
                if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null) {
            return right;
        }
        if (right == null) {
            return left;
        }
        return root;
    }
}
```



## 05. 图



## 06. 递归



## 07. 动态规划



## 08. 回溯

### 生成括号

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

解法：

回溯

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        combine(n, 0, 0, "", result);
        return result;
    }

    public void combine(int max, int open, int close, String str, List<String> result) {
        if (str.length() == 2 * max) {
            result.add(str);
            return;
        }
        if (open < max) {
            combine(max, open + 1, close, str + "(", result);
        }
        if (close < open) {
            combine(max, open, close + 1, str + ")", result);
        }
    }

}
```

### 排列

给一个无重复数组，返回所有可能的排列

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

解法：

回溯<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210606232135325.png" alt="image-20210606232135325" style="zoom:50%;" />



<img src="/Users/cuweir/Library/Application Support/typora-user-images/image-20210606232323557.png" alt="image-20210606232323557" style="zoom:50%;" />

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> temp, int[] nums) {
        if (temp.size() == nums.length) {
            result.add(new ArrayList<>(temp));
            return;
        }
        for (int num : nums) {
            if (temp.contains(num)) {
                continue;
            }
            temp.add(num);
            backtrack(result, temp, nums);
            temp.remove(temp.size() - 1);
            
        }
    }
}

```

## 09. 其他

### LRU

```java
class LRUCache {
    Node head = new Node(0, 0), tail = new Node(0, 0);
    Map<Integer, Node> map = new HashMap<>();
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            remove(node);
            insert(node);
            return node.value;
        } else {
            return -1;
        }
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            remove(map.get(key));
        }
        if (map.size() == capacity) {
            remove(tail.prev);
        }
        insert(new Node(key, value));
    }

    private void remove(Node node) {
        map.remove(node.key);
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void insert(Node node) {
        map.put(node.key, node);
        Node headNext = head.next;
        head.next = node;
        node.prev = head;
        headNext.prev = node;
        node.next = headNext;
    }

    class Node {
        Node prev, next;
        int key, value;
        Node (int _key, int _value) {
            key = _key;
            value = _value;
        }
    }
}
```

## 多线程死锁

```java
import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;

    public class DeadLockTest {
    //  private Object A = new Object();
    //  private Object B = new Object();



        public static void main(String[] args) {
            Lock A = new ReentrantLock();
            Lock B = new ReentrantLock();


            new Thread(){
                public void run(){
                    System.out.println("Thread1 -- trying to get lock A");
                    A.lock();
                    System.out.println("Thread1 -- get lock A successfully!");

                    try {
                        sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println("Thread1 -- trying to get lock B");
                    B.lock();
                    System.out.println("Thread1 -- get lock B successfully!");
                }
            }.start();


            new Thread(){
                public void run(){
                    System.out.println("Thread2 -- trying to get lock B");
                    B.lock();
                    System.out.println("Thread2 -- get lock A successfully!");

                    try {
                        sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println("Thread2 -- trying to get lock A");
                    A.lock();
                    System.out.println("Thread2 -- get lock B successfully!");
                }
            }.start();

        }
    }
```

