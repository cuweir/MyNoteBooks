# 消息队列

存储和收发消息的应用程序，保证了应用程序见消息传递的可靠性

#### 为什么需要，使用场景

- 系统解耦
- 流量削峰
- 日志处理
- 广播消息（分布式系统）

#### MQ模型

所有的MQ的模型抽象出来都是一样的: 消费者(订阅者)订阅某个消息队列, 生产者(发布者)发布消息到消息队列,最后消息队列将消息发送给消费者。

## RabbitMQ

支持AMQP协议

![RabbitMQ结构图](https://qsjzwithguang19forever.gitee.io/framework-learning/img/mq/rabbitmq/RabbitMQ%E5%86%85%E9%83%A8%E7%BB%93%E6%9E%84.png)

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

#### RabbitMQ Exchange的类型

RabbitMQ共有3种Exchange类型:

1. fanout: fanout即发布者 / 订阅者模式,发布者将消息发送到fanout类型的Exchange,Exchange再将消息广播给与此交换机绑定的Queue。
2. direct: direct即路由模式,Exchange根据Routing Key,将消息发送到匹配的队列中。
3. topic: topic也属于路由模式,不过它支持"*","#"等通配符进行路由。

### 如何避免消息重复消费

![消息被重复消费](https://qsjzwithguang19forever.gitee.io/framework-learning/img/mq/rabbitmq/%E6%B6%88%E6%81%AF%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9.png)

#### Broker重复消费

1-2中，如果MQ相应的ACK由于网络原因丢失了，那么发布者会再次发送消息给服务器,这就造成了服务器收到了多次相同的消息的问题

#### Consumer重复消费

3-4中，如果Consumer响应的ACK由于网络原因丢失了,那么MQ会再次发送消息给Consumer客户端,这也造成了Consumer多次消费相同的消息的问题

#### 解决消息的重复消费

无论是MQ服务器还是Consumer消费端,它们都需要判断消息的唯一性, 如果判断消息已经接受过了,那么可以选择不再处理相同的消息。 解决办法是可以给消息添加全局唯一的标识,如唯一ID,用于保证消息的全局唯一性。

### 如何保证消息传递可靠性

消息传递不可靠的情况主要有:

- Producer丢失消息: 即 1 - 2。
- Broker丢失消息: 即消息存储在MQ Broker时丢失。
- Consumer丢失消息: 即 3 - 4。

#### Producer消息丢失

Producer丢失消息有2种解决方案:

1. transaction: 如果消息发送成功，则提交事务。如果在发送消息的过程中出现了异常,那么事务就会回滚。 虽然事务可以保证Producer发送消息的可靠性,但是会降低吞吐量, 所以更推荐使用confirm机制保证Producer发送消息的可靠性。
2. confirm: 当消息被发送给消息队列后,Broker会响应一个ACK给Producer。 如果Broker无法处理该消息,则返回一个NACK给Producer,Producer可以根据返回的NACK再做处理。

#### Broker消息丢失

Broker丢失消息一般是MQ服务器宕机或出现其它较为严重的问题, 所以Broker丢失消息的问题,可以通过持久化解决。

#### Consumer消息丢失

Consumer接受到消息队列的数据后,会自动回复Broker,Broker就会删除这条消息, 但如果Consumer在此时处理消息的过程中,发生了意外,消息就丢失了。 Consumer可以在处理完消息后再手动回复Broker,这样即使发生了意外,Broker也能重新发送消息给Consumer。



### 如何保证消息队列高可用

#### RabbitMQ

基于主从。有三种模式：单机，普通集群（无高可用，用来提高吞吐量用的），镜像集群（高可用）

#### Kafka

由多个 broker 组成，每个 broker 是一个节点

天然分布式，每个机器放一部分数据

0.8版本提供了HA机制，副本机制

### 如何保证消息的可靠性传输？或者说，如何处理消息丢失的问题

有重复消费和幂等性的问题

#### RabbitMQ

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

#### kafka

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

### 如何保证顺序性

场景：**RabbitMQ**：一个queue，多个consumer

![rabbitmq-order-01](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/rabbitmq-order-01.png)

**Kafka**：建了一个 topic，有三个 partition

![kafka-order-01](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/kafka-order-01.png)

#### 解决方案

##### RabbitMQ

拆分多个 queue，每个 queue 一个 consumer， consumer 内部用内存队列做排队，然后分发给底层不同的 worker 来处理

![rabbitmq-order-02](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/rabbitmq-order-02.png)

##### Kafka

- 一个 topic，一个 partition，一个 consumer，内部单线程消费，单线程吞吐量太低，一般不会用这个。
- 写 N 个内存 queue，具有相同 key 的数据都到同一个内存 queue；然后对于 N 个线程，每个线程分别消费一个内存 queue 即可，这样就能保证顺序性。

![kafka-order-02](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/kafka-order-02.png)

### 如何保证消息不被重复消费？如何保证消息消费的幂等性？

得结合业务来思考，我这里给几个思路：

- 比如你拿个数据要写库，你先根据主键查一下，如果这数据都有了，你就别插入了，update 一下好吧。
- 比如你是写 Redis，那没问题了，反正每次都是 set，天然幂等性。
- 比如你不是上面两个场景，那做的稍微复杂一点，你需要让生产者发送每条数据的时候，里面加一个全局唯一的 id，类似订单 id 之类的东西，然后你这里消费到了之后，先根据这个 id 去比如 Redis 里查一下，之前消费过吗？如果没有消费过，你就处理，然后这个 id 写 Redis。如果消费过了，那你就别处理了，保证别重复处理相同的消息即可。
- 比如基于数据库的唯一键来保证重复数据不会重复插入多条。因为有唯一键约束了，重复数据插入只会报错，不会导致数据库中出现脏数据。

![mq-11](https://github.com/doocs/advanced-java/raw/main/docs/high-concurrency/images/mq-11.png)

### 如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？

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