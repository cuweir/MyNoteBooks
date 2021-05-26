# SpringIOC源码分析

maven引：

```java
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.3.11.RELEASE</version>
</dependency>
```

spring-context会将spring-core、spring-beans、spring-aop、spring-expression这几个基础的jar包引入

![1](https://www.javadoop.com/blogimages/spring-context/1.png)

## BeanFactory

![2](https://www.javadoop.com/blogimages/spring-context/2.png)

生产和管理各个bean实例

ApplicationContext也是一个BeanFactory

ListableBeanFactory可获取多个Bean

### BeanDefinition

保存了Bean信息，比如Bean指向那个类、是否是单例、是否懒加载、依赖了哪些Bean等

# Spring AOP

## AOP，AspectJ，Spring AOP

AOP要我们实现一个代理，实际运行的实例其实是生成的代理类的实例

**SpringAOP**：

- 基于动态代理实现，如果没有接口，使用CGLIB实现
- 需要依赖于IOC容器来管理
- 只能作用于Spring容器中的Bean
- Spring AOP 是基于代理实现的，在容器启动的时候需要生成代理实例，在方法调用上也会增加栈的深度，使得 Spring AOP 的性能不如 AspectJ 那么好

**AspectJ**：

- 属于静态织入，通过修改代码来实现的
- 是AOP编程的完全解决方案

## Spring AOP

有三种配置方式：

### Spring1.2基于接口的配置





# AQS

AbstractQueuedSynchronizer

用来构建同步器和锁的框架

## 数据结构

内部采用了volatile变量state来作为资源的标识

本身实现的是一些排队和阻塞机制，它内部是一个先进先出的双端队列，使用了两个指针：head、tail

![img](http://concurrent.redspider.group/article/02/imgs/AQS%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.png)

但他它本身不是直接存储线程，而是存储拥有线程的节点Node

## 资源共享模式

- 独占模式（exclusive）：一个线程一次只能获取一个资源，如ReentrantLock
- 共享模式（share）：同时可以被多个线程获取，如：Semaphore、CountDownLatch

## 主要方法

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





# HashMap和ConcurrentHashMap

## Java7 HashMap

数组加单向链表

Entry类，包含key，value，hash值，next

capacity：数组容量，2的n次方

loadFactor：负载因子，默认0.75

threshold：扩容阈值，容量乘以负载因子



