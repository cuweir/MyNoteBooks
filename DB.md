# MySQL

## MySQL（Innodb）索引的原理

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

**自增主键和uuid作为主键的区别么？**

由于主键使用了聚簇索引，如果主键是自增id，，那么对应的数据一定也是相邻地存放在磁盘上的，写入性能比较高。如果是uuid的形式，频繁的插入会使innodb频繁地移动磁盘块，写入性能就比较低了。

![image](https://www.cnblogs.com/images/cnblogs_com/rjzheng/1281019/o_index04.png)

非聚簇索引的叶子节点不是真实数据，它的叶子节点依然是索引节点，存放的是该索引字段的值以及对应的主键索引(聚簇索引)。

多加一个索引，就会多生成一颗非聚簇索引树，做插入操作的时候，需要同时维护这几颗树的变化。因此，如果索引太多，插入性能就会下降。

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
- `Read Committed (RC)`：读已提交，一个事务可以读到另一个事务已提交的数据!
- `Repeatable Read (RR)`:可重复读，加入间隙锁，一定程度上避免了幻读的产生！注意了，只是一定程度上，并没有完全避免!我会在下一篇文章说明!另外就是记住从该级别才开始加入间隙锁(这句话记下来，后面有用到)!
- `Serializable`：串行化，该级别下读写串行化，且所有的`select`语句后都自动加上`lock in share mode`，即使用了共享锁。因此在该隔离级别下，使用的是当前读，而不是快照读。

**PS：**并不是用表锁来实现锁表的操作，而是利用了`Next-Key Locks`，也可以理解为是用了行锁+间隙锁来实现锁表的操作。

RU和RC不存在间隙锁

## MVCC

### 版本链

InnoDB的聚簇索引记录包含两个必要的隐藏列：

`trx_id`：每次对某条聚簇索引记录进行改动时，都会把对应的事务id赋值给`trx_id`隐藏列

`roll_pointer`：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到`undo日志`中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

![image_1d6vfrv111j4guetptcts1qgp40.png-57.1kB](https://user-gold-cdn.xitu.io/2019/3/27/169bf198524e1b34?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### ReadView

这个`ReadView`中主要包含当前系统中还有哪些活跃的读写事务，把它们的事务id放到一个列表中，我们把这个列表命名为为`m_ids`。

使用`READ UNCOMMITTED`隔离级别的事务来说，直接读取记录的最新版本就好了，对于使用`SERIALIZABLE`隔离级别的事务来说，使用加锁的方式来访问记录。

`READ COMMITTED` --- 每次读取数据前都生成一个ReadView

`REPEATABLE READ` --- 在第一次读取数据时生成一个ReadView

所谓的MVCC（Multi-Version Concurrency Control ，多版本并发控制）指的就是在使用`READ COMMITTD`、`REPEATABLE READ`这两种隔离级别的事务在执行普通的`SEELCT`操作时访问记录的版本链的过程，这样子可以使不同事务的`读-写`、`写-读`操作并发执行，从而提升系统性能。

`READ COMMITTD`、`REPEATABLE READ`这两个隔离级别的一个很大不同就是生成`ReadView`的时机不同，`READ COMMITTD`在每一次进行普通`SELECT`操作前都会生成一个`ReadView`，而`REPEATABLE READ`只在第一次进行普通`SELECT`操作前生成一个`ReadView`，之后的查询操作都重复这个`ReadView`就好了。

## redo log和bin log

![img](https://upload-images.jianshu.io/upload_images/5148507-ca8930bca4e10d05.png?imageMogr2/auto-orient/strip|imageView2/2/w/830/format/webp)

**mysql通过WAL(write-ahead logging)技术保证事务**

![img](https://upload-images.jianshu.io/upload_images/5148507-ce39f679f490b64f.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

**redo log记录方式**

![img](https://upload-images.jianshu.io/upload_images/5148507-bef2727de72b311f.png?imageMogr2/auto-orient/strip|imageView2/2/w/854/format/webp)

write pos表示日志当前记录的位置，当ib_logfile_4写满后，会从ib_logfile_1从头开始记录；check point表示将日志记录的修改写进磁盘，完成数据落盘，数据落盘后checkpoint会将日志上的相关记录擦除掉，即write pos->checkpoint之间的部分是redo log空着的部分，用于记录新的记录，checkpoint->write pos之间是redo log待落盘的数据修改记录。当writepos追上checkpoint时，得先停下记录，先推动checkpoint向前移动，空出位置记录新的日志。
 有了redo log，当数据库发生宕机重启后，可通过redo log将未落盘的数据恢复，即保证已经提交的事务记录不会丢失。
 **有了redo log，为啥还需要binlog呢？**

1、redo log的大小是固定的，日志上的记录修改落盘后，日志会被覆盖掉，无法用于数据回滚/数据恢复等操作。
2、redo log是innodb引擎层实现的，并不是所有引擎都有。



1、binlog是server层实现的，意味着所有引擎都可以使用binlog日志
 2、binlog通过追加的方式写入的，可通过配置参数max_binlog_size设置每个binlog文件的大小，当文件大小大于给定值后，日志会发生滚动，之后的日志记录到新的文件上。
 3、binlog有两种记录模式，statement格式的话是记sql语句， row格式会记录行的内容，记两条，更新前和更新后都有。

![img](https://upload-images.jianshu.io/upload_images/5148507-a47b099cea71c913.png?imageMogr2/auto-orient/strip|imageView2/2/w/686/format/webp)

![img](https://upload-images.jianshu.io/upload_images/5148507-1a29473c24f0c5b7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

1、innodb_flush_log_at_trx_commit：设置为1，表示每次事务的redolog都直接持久化到磁盘（注意是这里指的是redolog日志本身落盘），保证mysql重启后数据不丢失。
 2、sync_binlog： 设置为1，表示每次事务的binlog都直接持久化到磁盘（注意是这里指的是binlog日志本身落盘），保证mysql重启后binlog记录是完整的。

# Redis

Redis单线程指的是一个线程处理所有网络请求，其他模块仍用多线程

### Redis处理过期数据

**Redis处理过期键有三种策略**

1. 被动清除：读/写⼀个已经过期的key时，会触发惰性删除策略，直接删除掉这个过期key
2. 主动删除（定期删除）：由于惰性删除策略⽆法保证冷数据被及时删掉，所以Redis会定期主动淘汰⼀批已过期的key
3. 当前已⽤内存超过maxmemory限定时，触发主动清理策略

### 内存淘汰算法

1. volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使⽤的数据淘汰
2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
4.  allkeys-lru：当内存不⾜以容纳新写⼊数据时，在键空间中，移除最近最少使⽤的key（这个是最常 ⽤的）
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
6. no-eviction：禁⽌驱逐数据，也就是说当内存不⾜以容纳新写⼊数据时，新写⼊操作会报错。这个 应该没⼈使⽤吧

4.0版本后增加了两种：

1. volatile-lfu：从已设置过期时间的数据集(server.db[i].expires)中挑选最不经常使⽤的数据淘汰
2. allkeys-lfu：当内存不⾜以容纳新写⼊数据时，在键空间中，移除最不经常使⽤的key

### Redis持久化

RDB全量持久化

AOF（append-only file）增量持久化

#### RDB

- RDB 将数据库的快照（snapshot）以二进制的方式保存到磁盘中。

Redis 客户端还同时提供两个命令来生成 RDB 存储文件，也就是 `SAVE` 和 `BGSAVE`

使用 `BGSAVE` 命令时，Redis 会立刻 `fork` 出一个子进程

子进程和主进程共享一份内存空间

写时拷贝

#### AOF

- AOF 则以协议文本的方式，将所有对数据库进行过写入的命令（及其参数）记录到 AOF 文件，以此达到记录数据库状态的目的

当 Redis 重启时， 它会优先使⽤ AOF ⽂件 来还原数据集， 因为 AOF ⽂件保存的数据集通常⽐ RDB ⽂件所保存的数据集更完整

##### 重写

bgrewriteaof

#### 优缺点

RDB紧凑，适合用于备份；发生故障停机会丢失几分钟数据

AOF耐久，发生故障停机也没啥，自动重写，到处容易；缺点体积大于RDB

### 五种数据结构

string

hash

linklist

set

zset

### 架构模式

#### 主从复制

建立连接——数据同步——命令传播

##### 建立连接

##### 数据同步

完整重同步

部分同步

##### 命令传播

为了能够使主从服务器的数据保持⼀致性，主服务器会对从服务器执⾏命令传 播操作，即每执⾏⼀个写命令就会向从服务器发送同样的写命令

从服务器会默认以每秒⼀次的频率向主服务器发送⼼跳检测

#### 哨兵

每秒钟向它所知的主、从服务器及其他实例发送一个PING

如果⼀个 实例（ instance ）距离 最后⼀次 有效回复 PING 命令的时间超过 down-after-milliseconds 所指定的值，那么这个实例会被 Sentinel 标记为 主观下线。

 如果⼀个 主服务器 被标记为 主观下线，那么正在 监视 这个 主服务器 的所有 Sentinel 节点， 要以 每秒⼀次 的频率确认 主服务器 的确进⼊了 主观下线 状态。

### 分布式锁

setnx

### 为什么要用缓存

**高性能，高并发**

#### 用了缓存的不良后果

缓存、数据库双写不一致

缓存雪崩、缓存穿透、缓存击穿

缓存并发竞争

### Redis和Memcached的区别

#### redis支持复杂的数据结构

支持更丰富的操作

#### redis原生支持集群

#### 性能

Redis 只使用**单核**，而 Memcached 可以使用**多核**，所以平均每一个核上 Redis 在存储小数据时比 Memcached 性能更高。而在 100k 以上的数据中，Memcached 性能要高于 Redis

### Redis线程模型

Redis 内部使用文件事件处理器 `file event handler` ，这个文件事件处理器是单线程的，所以 Redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

- 多个 socket
- IO 多路复用程序
- 文件事件分派器
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

#### 为什么单线程效率也高

1. 纯内存操作
2. 基于非阻塞的IO多路复用机制
3. C语言
4. 避免多线程频繁上下文切换

#### 6.0引入多线程

**Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。**

Redis 选择使用单线程模型处理客户端的请求主要还是因为 CPU 不是 Redis 服务器的瓶颈，所以使用多线程模型带来的性能提升并不能抵消它带来的开发成本和维护成本，系统的性能瓶颈也主要在网络 I/O 操作上；而 Redis 引入多线程操作也是出于性能上的考虑，对于一些大键值对的删除操作，通过多线程非阻塞地释放内存空间也能减少对 Redis 主线程阻塞的时间，提高执行的效率。

### Redis 都有哪些数据类型？分别在哪些场景下使用比较合适？

Redis 主要有以下几种数据类型：

- Strings
  - 最简单的类型，就是普通的 set 和 get，做简单的 KV 缓存
- Hashes
  - 类似 map 的一种结构，这个一般就是可以将结构化的数据，比如一个对象（前提是**这个对象没嵌套其他的对象**）给缓存在 Redis 里，然后每次读写缓存的时候，可以就操作 hash 里的**某个字段**
- Lists
  - 有序列表
- Sets
  - 无序集合，自动去重
- Sorted Sets
  - 排序的set，，去重但可以排序，写进去的时候给一个分数，自动根据分数排序

> Redis 除了这 5 种数据类型之外，还有 Bitmaps、HyperLogLogs、Streams 等。

### Redis 的过期策略、内存淘汰机制、手写一下 LRU 代码实现

常见问题：

1. 写入的数据怎么没了？写的多了，干掉不常用的数据。
2. 数据过期了怎么还占内存？Redis过期策略决定的

#### 过期策略

#### 手写LRU算法

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





















