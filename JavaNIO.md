# Java NIO

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

非阻塞，多路复用，用于实现一个线程管理多个Channel

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



# Java非阻塞IO和异步IO

即NIO和AIO

## 阻塞模式IO

来一个新的连接，就新开一个线程来处理这个连接，之后的操作全部由那个线程来完成。

性能瓶颈：

- 每次来一个连接都开一个新的线程这肯定是不合适的，活跃连接数高时，内存占用高，线程切换开销大
- accept() 是一个阻塞操作，当 accept() 返回的时候，代表有一个连接可以使用

## 非阻塞IO

核心在于使用一个Selector管理多个通道，各个通道注册到Selector，指定监听的事件，可以只用一个线程轮询Selector

- select：上世纪 80 年代就实现了，它支持注册 FD_SETSIZE(1024) 个 socket
- poll：不限制socket数量。select 和 poll 都有一个共同的问题，那就是**它们都只会告诉你有几个通道准备好了，但是不会告诉你具体是哪几个通道**
- epoll：能直接返回具体的准备好的通道，时间复杂度 O(1)。

## NIO.2 异步IO

JDK 1.7发布，引入异步 IO 接口和 Paths 等文件访问接口

由线程池负责执行任务，使用回调或自己去查询结果

异步 IO 主要是为了控制线程数量，减少过多的线程带来的内存消耗和 CPU 在线程调度上的开销。

Java异步IO提供了两种方式，分别是返回Future实例和使用回调函数：

### 1. 返回 Future 实例

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

### 2. 提供 CompletionHandler 回调函数

java.nio.channels.CompletionHandler 接口定义：

```java
public interface CompletionHandler<V,A> {

    void completed(V result, A attachment);

    void failed(Throwable exc, A attachment);
}
```

### AsynchronousFileChannel

文件 IO 在所有的操作系统中都不支持非阻塞模式，但是我们可以对文件 IO 采用异步的方式来提高性能。

### AsynchronousServerSocketChannel

这个类对应的是非阻塞 IO 的 ServerSocketChannel，大家可以类比下使用方式。

### AsynchronousSocketChannel

### Asynchronous Channel Groups

AsynchronousServerSocketChannels 和     AsynchronousSocketChannels 是属于 group 的，当我们调用 AsynchronousServerSocketChannel 或 AsynchronousSocketChannel 的 open() 方法的时候，相应的 channel 就属于默认的 group，这个 group 由 JVM 自动构造并管理

**AsynchronousFileChannels 不属于 group**

# Tomcat 中的 NIO 源码分析













