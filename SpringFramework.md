# Spring

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

#### Spring事务的传播行为

7种：

1. REQUIRED: 当一个方法A(REQUIRED)被另一个方法B调用时，如果B已经开启了事务，那么A就加入到B的事务中去， A和B要么同时成功，要么同时失败。如果B没有开启事务，那么A将自己开启新的事务,独立运行。
2. REQUIRES_NEW: 当一个方法A(REQUIRE_NEW)被非事务方法调用时，A会自己开启事务。 如果A被另一个方法B调用时，无论B是否开启事务，A都会开启自己的事务，且会挂起B的事务， 然后执行A，A提交后才会恢复B，这样外层的B即使失败也不会影响A，但是如果A失败了，B也会回滚。
3. NESTED: 如果方法A(NESTED)被另一个方法B调用，如果B已经开启了事务， 那么A将作为B的子事务运行，B失败，A也失败，但是A失败却不影响B。A是作为B的嵌套事务运行的， 所以并不会影响B。如果B没有事务，那么A将自己新开启一个事务运行。 **PS:NESTED和REQUIRES_NEW很相似，但是可以理解为他们是相反的，NESTED的方法作为嵌套事务运行在 外层事务内部，外层事务失败则内层事务也失败，但内层事务失败却不影响外层事务；REQUIRES_NEW则是 自己开启自己的事务，外层事务失败也不影响内层事务，但内层事务失败会导致外层事务回滚。**
4. SUPPORTS: 当一个方法A(SUPPORTS)被另一个方法B调用时，如果B开启了事务， 那么A就加入到B的事务中去。如果B没有开启事务,那就以非事务方式运行执行。
5. MANDATORY: 当一个方法A(MANDATORY)被另一个方法B调用时，如果B没有开启事务，那么A将抛出异常， 如果B开启了事务，则A加入到B中共用事务。
6. NOT_SUPPORTED: 当一个方法A(NOT_SUPPORTED)被另一个方法B调用时，如果B开启了事务，那么B的事务将挂起， 直到A执行完，B再以事务的方式运行。
7. NEVER: 当一个方法A(NEVER)被另一个方法B调用时，如果B开启了事务，那么A将抛出异常。

#### 什么是AOP?

AOP(Aspect Oriented Programming)面向切面编程。将系统的核心逻辑和辅助逻辑分离开来，并将通用的辅助逻辑封装成一个模块， 提高了代码的重用性和程序的可维护性，降低了系统模块之间的耦合度

#### AOP的组成元素和概念有哪些?

1. 连接点(join point): 连接点指程序执行的某个位置，能够执行辅助逻辑(通知/增强)。 如方法执行前，方法抛出异常时，方法执行完，方法返回后等等，这些点都被称为连接点。
2. 通知/增强(advice): 通知/增强 可以理解为辅助逻辑，就是在连接点要做的事情。
3. 切点(pointcut): 连接点可以看做是一个方法的执行辅助逻辑的不同位置的集合， 切入点指的就是这个方法。切入点会匹配 通知/增强 需要作用的类或方法。
4. 切面(aspect): 切面是切入点的集合，可以看作是拥有多个切入点的类。
5. 织入(weave): 织入是一个概念。它描述的是将切入点的 通知/增强 应用到连接点的过程。

#### AOP实现方式有哪些?

常见的AOP实现的方式有代理和织入。

- 代理分为静态代理和动态代理。 由于静态代理没有动态代理灵活，所以现在几乎都使用动态代理来实现AOP。 以动态代理实现AOP的框架主要有cglib和jdk原生的这2种。
- 织入可以理解为以操作字节码的方式对class源文件进行修改，从而实现通知/增强。 以织入实现AOP的框架主要有AspectJ。

#### AspectJ AOP 和 Spring AOP的区别?

- AspectJ AOP: AspectJ是一整套AOP的工具，它提供了切面语法(切入点表达式)以及织入等强大的功能。 Aspect提供文件和注解2种方式来进行AOP编程， 并且它允许在编译时，编译后和加载时织入， 但是需要使用它特定的ajc编译器才能实现织入这一功能。
- Spring AOP: Spring AOP吸收了AspectJ的优点，采用了AspectJ的切入点语法以及AspectJ式的注解， 但却并未使用AspectJ的一整套工具, 而是集cglib和jdk于一体(动态代理)的方式来实现AOP功能，真的很强。

#### cglib动态代理和jdk动态代理的区别?

- jdk动态代理: jdk只提供基于接口式的动态代理来对目标进行增强。
- cglib动态代理: cglib则是使用字节码技术，动态生成目标的子类，以继承的方式来对目标方法进行重写， 所以如果方法是final的，那么cglib将无法对方法进行增强。 **在SpringAOP 中，如果目标类实现了接口，那么默认使用jdk动态代理来实现AOP， 如果目标类没有实现接口，那么将使用cglib来实现AOP。**

# Spring MVC

#### 什么是DispatcherServlet?

一个Servlet，负责拦截所有的请求，并以调用各种组件来对请求进行分发并处理。

#### Spring MVC有哪些组件?(见:DispatcherServlet源码)

1. MultipartResolver: 核心组件之一，处理文件上传请求。MultipartResolver负责判断普通请求是否为文件上传请求， 并将普通请求(HttpServletRequest)解析为文件上传请求(MultipartHttpServletRequest)。
2. LocalResolver: 区域解析器。它主要被用于国际化的资源方面的解析。
3. ThemeResolver: 主题资源解析器。SpringMVC允许用户提供不同主题，主题就是一系列资源的集合使用这些主题可以提高用户体验。
4. ViewResolver: 核心组件之一，视图解析器。在Handler执行完请求后，ViewResolver将ModelAndView解析成物理视图， 并对物理视图进行model渲染。
5. HandlerMapping: 核心组件之一，请求处理器。根据用户的请求来匹配对应的Handler。
6. HandlerAdapter: 核心组件之一，Handler适配器。使用Handler处理请求，并返回处理后的视图(ModelAndView)。
7. HandlerExceptionResolver: 核心组件之一，异常处理解析器。在Handler执行请求的过程中可能出现异常， HandlerExceptionResolver就负责处理Handler执行请求过程中的异常。
8. RequestToViewNameTranslator: 核心组件之一,请求到视图的转换器。根据Request设置最终的视图名,当Handler执行完请求后，它会将Request解析成视图名。
9. FlashMapManager: 核心组件之一，在请求进行重定向时，FlashMapManager用于保存请求中的参数。

#### 简述SpringMVC原理/执行流程

1. 用户发出的请求被DispatcherServlet拦截。
2. DispatcherServlet使用HandlerMapping根据请求匹配到相应的Handler。Handler实际上是一个(HandlerMethod)。
3. DispatcherServlet根据Handler适配合适的HandlerAdapter。
4. HandlerAdapter使用Handler执行请求，并返回ModelAndView。
5. 使用RequestToViewNameTranslator,HandlerExceptionResolver和ViewResolver等解析器， 解析并渲染ModelAndView，并处理相关异常信息。
6. 渲染后的结果反馈给用户。

#### Spring MVC 拦截器是什么 / 有什么作用 / 与 Filter有什么区别?

- HandlerInterceptor: Spring MVC拦截器是Spring MVC提供的对用户请求的目标资源做出拦截扩展的处理器。 它允许在目标方法执行前后以及View渲染后做出处理。
- Servlet Filter: Filter是Servlet提供的过滤器，它会在目标方法执行前后做出拦截处理。

# Spring源码分析

SpringMVC通过一个DispatcherServlet拦截所有请求,也就是url为 /** 。 通过拦截所有请求,在内部通过路由匹配的方式把请求转给对应Controller的某个RequestMapping处理。 这就是SpringMVC的基本工作流程,我们传统的JavaWeb是一个Servlet对应一个URL, 而DispatcherServlet是一个Servlet拦截全部URL,并做分发处理. 这也是SpringMVC的设计的精妙之处。

肯定有一个意识:在SpringMVC框架中,所有的核心方法几乎都是do开头, 并且都有2个必要的参数:HttpServletRequest和HttpServletResponse

DispatcherServlet的doDispatch方法：

九大组件

SpringMVC工作的流程:

**SpringMVC通过DispatcherServlet拦截所有的请求, 并通过HandlerMapping与指定的请求找出匹配的handler, handler实际是HandlerMethod对象。 再通过与handler适配的HandlerAdapter执行目标方法, 执行完目标方法后会返回ModelAndView对象, 最后通过ViewResolver解析ModelAndView的View视图。**

# Springboot

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

#### SpringBoot配置文件加载顺序

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
   2. 读取spring.factories
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