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









# HashMap和ConcurrentHashMap

## Java7 HashMap

数组加单向链表

Entry类，包含key，value，hash值，next

capacity：数组容量，2的n次方

loadFactor：负载因子，默认0.75

threshold：扩容阈值，容量乘以负载因子



