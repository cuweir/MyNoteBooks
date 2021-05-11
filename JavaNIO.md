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











