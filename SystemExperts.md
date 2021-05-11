### Latency

延迟

### Throughput

吞吐

RPS: Requests Per Second

QPS: Query Per Second

### SLA

service-level agreement

### SLO

service-level objective

**SLA = SLO + 后果**

### Cache

Cache Eviction Policy: LRU(least-recently used), FIFO, LF(frequency)U

### CDN

Content Delivery Network

第三方服务，扮演类似于服务器缓存的角色，web应用跨区域（region）慢，CDN再全世界都有服务器，所以延迟低。流行的CDN有：Cloudflare, Google Cloud CDN

相关概念：points of presence

### Proxy

Forward Proxy: 一个Client和servers之间的代理，扮演client，可以掩盖客户端IP

Reverse Proxy: 反向代理，clients和servers之间的代理，扮演server角色，常用来logging，负载均衡，缓存

Nginx: 反向代理，负载均衡

### Load Balance

Server-Selection Strategy: 1. round-robin轮询；2. random；3. performance based；4. IP-based routing

Hot Spot：流量大的服务器

### Hashing

Consistent Hashing: 一致哈希

Rendezvous Hashing: Highest Random Weight算法，最高随机加权哈希

SHA: Secure Hash Algorithms，常用SHA-3算法

### Relational Databases

**ACID Transaction**

atomicity，原子性，事务操作要么成功要么失败，没有中间状态

consistency，一致性，事务不能给数据库带来无效的状态

isolation，隔离性，多个事务并行执行要有相同的影响，就好像他们顺序执行一样

durability，持久性，事务写入非易失存储中，不会因为崩溃，没电收到影响

**Database Index**

查询更快

强一致，最终一致（只保证数据库状态最终会反应出来，再一定时间段后）

### Key-Value Stores

Etcd：强一致高可用键值存储，可实现系统中选主

Redis：内存存储，常用作缓存，还可限流

ZK：强一致高可用键值存储，常用存重要的配置，或用来选主

### Replication And Sharding

replication：复制数据，从一个server到另一些，增强系统冗余，提高容错率；还可将数据放在离客户端近的地方，减少延迟

sharding：分片，将数据划分成好多片，为了增加数据库吞吐量，策略有：基于客户端地区分片、基于数据类型分片、基于列的hash

### Peer-To-Peer Network

常用于分布式文件系统

### Polling And Streaming

polling：有规律地隔一段时间获取资源，保证数据新鲜

streaming：两个机器或进程之间，不断获取信息，连接一直保持

### Rate Limiting

限流：为了避免攻击

DoS Attack：denial-of-service attack

DDoS Attack：distributed DoS attack



# 设计AlgoExpert

1. 收集系统需求

核心功能：登录、访问问题，标记已完成，编写代码，运行代码，保存代码等

不需要关心：付款，身份认证，代码执行引擎

面向用户：美国印度

不需要考虑可用性

2. 指定计划

考虑网站有哪些组成部分：

有很多静态内容，例如图片等；还有很多动态内容，例如编写代码等。因此需要强大的api，还要数据库

三个核心组件：1. 静态UI；2. 访问并与问题互动（问题完成状态，保存解答）；3. 运行代码的能力

3. 静态UI内容

可将图像、js包放在blob store：s3 or Google Cloud Storage等；由于需要响应式网站，可以通过CDN提供内容，对于移动端体验会好

4. 主集群和负载均衡

2个主要集群在两个区域：美国、印度

需要DNS负载均衡将API请求就近发到请求用户的集群，一个区域内可以使用基于路由的负载均衡分离服务：支付，认证和代码执行等。尤其是代码执行服务，可能要在不同的服务器上运行。每个服务可能有一组服务器，可以用循环负载的方式

5. 静态API内容

如问题列表和所有解决方案，可以将数据存储在Blob存储中

6. 缓存

可以为静态API实现2级缓存

首先，客户端缓存，改善用户体验。每个会话加载一次问题即可。这也将减少后端服务器负载

其次，服务器上加内存缓存，如果10种语言，一百个问题，每个答案5kB，则小于10 * 100 * 5000 = 5MB总数据保存在内存中，完全可行。

由于我们希望每天都对内容进行更改，而且希望它尽快生效，可以将缓存失效时间调整为30min

7. 访问控制

本系统，问题内容具有直接的访问控制：没购买就看不成。所以只要用户请求，可以在返回内容前搞清楚用户是否具有该产品即可

8. 用户数据存储

必须设计问题完成状态和用户解决方案的存储，用sql数据库

两个表：第一个表question_complete_status：id, user_id, question_id, complete_status（user_id和question_id有唯一约束，user_id建立索引快速查询）

第二个表user_solutions：id, user_id, question_id, language, solution（user_id,question_id,language唯一索引，并且user，question索引）

9. 存储性能

标记问题完成，或者键入代码会写入数据库，考虑到用户量，每秒不会收到超过1000次。

可以有两个数据库服务器，每个服务器服务于我们的2个主要区域，如果需要则扩容

10. 区域内的数据复制

由于2个主数据库服务器，因此需要彼此保持最新。

由于不存在用户共享内容，因此可以不需要实施写入另一台。但也需要保持最新状态，可以添加异步复制，每12小时一次，或者根据系统行为和跨大洲复制的数据量调整时间

11. 代码执行

首先要进行速率限制。可以使用redis k-v存储实现基于曾的限速，防止DoS攻击，可设置为每秒执行一次

由于期望延迟1-3秒，所以希望一组特殊服务器worker随时准备运行代码