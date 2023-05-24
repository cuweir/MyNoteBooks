# Fundamentals

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



# Questions

## 设计AlgoExpert

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

首先要进行速率限制。可以使用redis k-v存储实现基于层的限速，防止DoS攻击，可设置为每秒执行一次

由于期望延迟1-3秒，所以希望一组特殊服务器worker随时准备运行代码

12. 图

![Final Systems Architecture](https://prod.api.algoexpert.io/api/problems/v1/assets?path=algoexpert.png)

## 设计Reddit API

给定以下信息，为Reddit子reddit设计一个API。

该API包括以下2个实体：

用户| userId：字符串，...

SubReddit | subredditId：字符串，...

这两个实体可能都具有其他字段，但是出于这个问题的目的，不需要这些其他字段。

您的API应该支持Reddit上subreddit的基本功能。

**subreddit**是一个在线社区，用户可以在这里写帖子、评论帖子、向上投票/向下投票帖子、共享帖子、报告帖子、成为版主等等

只关注写文章、写评论和投赞成票/反对票。您可以忘记所有辅助功能，如共享、报告、调节等。

有发帖奖励机制

1. 收集需求

我们正在Reddit上设计subreddit功能的核心用户流。用户可以在subreddits上写文章，可以对文章发表评论，还可以对文章和评论进行上/下投票。

我们将定义三个主要实体：帖子Posts、评论Comments和投票Votes，以及它们各自的CRUD操作。

我们还将设计一个API，用于购买和奖励Reddit。

2. 制定计划

重要的是要组织自己，并就如何处理设计制定清晰的计划。 API的主要部分（可能有争议的部分）是什么？ 我们为什么要做出某些设计决策？

争论的第一个主要点是是否只在Comment和Posts上存储投票，并通过调用EditComment和EditPost方法来投票，或者是否将它们存储为完全独立的实体（可以说是Posts和Comments的同级兄弟）。 将它们存储为单独的实体可以更轻松地编辑或删除特定用户的投票（例如，仅通过调用EditVote），因此我们将采用这种方法。

然后，我们可以计划按此顺序处理帖子，评论和投票，因为它们可能会共享一些共同的结构。

3. Posts

文章将有一个id，其创建者的id（即编写它们的用户），它们所在的subReddit的id，描述和标题，以及创建它们的时间戳。

帖子还将在用户界面上显示其投票、评论和奖励的计数。我们可以想象，当执行一些评论、投票和奖励CRUD操作时，一些后端服务将计算或更新这些数字。

最后，Posts将有可选的deletedAt和currentVote字段。用特殊信息替换已删除的显示柱；我们可以使用deletedAt字段来实现这一点。currentVote字段将用于向用户显示他们是否对帖子投过票。此字段可能会在获取帖子或投票时由后端填充。

- postId: *string*
- creatorId: *string*
- subredditId: *string*
- title: *string*
- description: *string*
- createdAt: *timestamp*
- votesCount: *int*
- commentsCount: *int*
- awardsCount: *int*
- deletedAt?: *timestamp*
- currentVote?: *enum UP/DOWN*

我们的CreatePost，EditPost，GetPost和DeletePost方法将非常简单。 但是要注意的一件事是，所有这些操作都将采用执行这些操作的用户的userId； 此ID（可能包含身份验证信息）将用于ACL（访问控制列表）检查，以查看执行操作的用户是否具有执行此操作所需的权限。

```
CreatePost(userId: string, subredditId: string, title: string, description: string)
  => Post

EditPost(userId: string, postId: string, title: string, description: string)
  => Post

GetPost(userId: string, postId: string)
  => Post

DeletePost(userId: string, postId: string)
  => Post
```

由于我们可以预期在给定的subreddit上有数百甚至数千个帖子，因此必须对ListPosts方法进行分页。 该方法将接受可选的pageSize和pageToken参数，并将返回最大长度为pageSize的帖子列表以及nextPageToken-要馈送到该方法以检索帖子的下一页的令牌

```
ListPosts(userId: string, subredditId: string, pageSize?: int, pageToken?: string)
  => (Post[], nextPageToken?)
```

4. Comments

评论将类似于帖子。 他们将拥有一个ID，其创建者的ID（即编写它们的用户），他们所在的帖子的ID，内容字符串以及与Posts相同的其他字段。 唯一的区别是，Comment还将具有一个可选的parentId，它指向该评论的父帖子或父评论。 此ID将允许Reddit UI重建评论树以正确显示（缩进）回复。 用户界面还可以按其创建时间时间戳或votesCount对回复线程中的注释进行排序。

- commentId: *string*
- creatorId: *string*
- postId: *string*
- createdAt: *timestamp*
- content: *string*
- votesCount: *int*
- awardsCount: *int*
- parentId?: *string*
- deletedAt?: *timestamp*
- currentVote?: *enum UP/DOWN*

我们的Comment的CRUD操作将与Post的CRUD操作非常相似，不同之处在于，如果CreateComment方法还将包含一个可选的parentId，则它会指向要回复的评论（如果相关）。

```
CreateComment(userId: string, postId: string, content: string, parentId?: string)
  => Comment

EditComment(userId: string, commentId: string, content: string)
  => Comment

GetComment(userId: string, commentId: string)
  => Comment

DeleteComment(userId: string, commentId: string)
  => Comment

ListComments(userId: string, postId: string, pageSize?: int, pageToken?: string)
  => (Comment[], nextPageToken?)
```

5. Votes

投票将具有一个ID，其创建者的ID（即对其进行投射的用户），其目标的ID（即他们所在的帖子或评论）以及一个类型，该类型将是简单的UP / DOWN枚举。 他们也可能有一个createdAt时间戳，以作很好的衡量。

- voteId: *string*
- creatorId: *string*
- targetId: *string*
- type: *enum UP/DOWN*
- createdAt: *timestamp*

由于对于我们的功能而言，获得单票表决或列出投票似乎不太有用，因此我们将跳过设计这些端点的过程（尽管它们很简单）。

我们的CreateVote，EditVote和DeleteVote方法将简单实用。 当用户对帖子或评论进行新的投票时，将使用CreateVote方法。 当用户已经对某个帖子或评论投了票，并对同一帖子或评论投了相反的票时，将使用EditVote方法； 当用户已经对帖子或评论投了一个票而只是删除了相同的票时，将使用DeleteVote方法。

```
CreateVote(userId: string, targetId: string, type: enum UP/DOWN)
  => Vote

EditVote(userId: string, voteId: string, type: enum UP/DOWN)
  => Vote

DeleteVote(userId: string, voteId: string)
  => Vote
```

6. Awards

我们可以定义两个简单的端点来处理购买和给予奖励。 购买奖励的端点将接收一个paymentToken，这是一个字符串，其中包含处理付款所需的所有必要信息。 给予奖励的端点将采用targetId，这将是给予奖励的帖子或评论的ID。

```
BuyAwards(userId: string, paymentToken: string, quantity: int)

GiveAward(userId: string, targetId: string)
```

## 设计Netflix

只设计核心产品

我们将需要访问大量的用户活动数据，这些数据需要进行处理才能启用Netflix的推荐系统。 您需要提出一种在网站上汇总和处理用户活动数据的方法。

不必担心为推荐引擎实现任何算法或公式。 您只需要考虑如何收集和处理用户活动数据。

视频流服务实际上是系统中我们关心的快速延迟的唯一部分。

推荐引擎是一个系统，它使用您提到的用户活动数据，并在后台异步运行

2亿用户

1. 收集需求

我们正在设计核心Netflix服务，该服务允许用户从Netflix网站流式传输电影和节目。

具体来说，我们将重点关注：

在没有太多缓冲的情况下，向全球数亿用户提供了大量的高清视频内容。

处理大量的用户活动数据以支持Netflix的推荐引擎。

2. 制定计划

我们将把这个系统分为四个主要部分：

存储（视频内容、静态内容和用户元数据）

一般客户机-服务器交互（即查询的生命周期）

视频内容交付

用户活动数据处理

3. 视频内容存储

由于满足数百万客户的Netflix服务以视频内容为中心，因此我们可能需要大量的存储空间和复杂的存储解决方案。 让我们首先估计我们需要多少空间。

有人告诉我们Netflix有大约2亿用户。 我们可以对其他Netflix指标做出一些假设（或者，我们可以在这里向我们的采访者寻求指导）：

Netflix随时提供约1万部电影和节目

由于电影的最长时长可能超过2小时，而且每集节目的放映时间通常在20到40分钟之间，因此我们可以假设平均视频时长为1小时

每个电影/节目都将具有标准清晰度版本和高清晰度版本。 每小时，SD将占用约10GB的空间，而HD将占用约20GB的空间。

```
  ~10K videos (stored in SD & HD)
  ~1 hour average video length
  ~10 GB/h for SD + ~20 GB/h for HD = 30 GB/h per video
  ~30 GB/h * 10K videos = 300,000 GB = 300 TB
```

这个数字突出了估计的重要性。 天真的，人们可能会认为Netflix存储了数十PB的视频，因为其核心产品围绕视频内容。 但简单的事后估算表明我们实际上存储了非常少量的视频。

这是因为Netflix与YouTube，Google云端硬盘和Facebook等其他服务不同，其视频内容的数量是有限的：提供的电影和节目； 这些其他服务允许用户上传无限量的视频。

由于我们仅处理数百TB的数据，因此我们可以使用简单的Blob存储解决方案（例如S3或GCS）来可靠地处理Netflix视频内容的存储和复制。 我们不需要更复杂的数据存储解决方案。

4. 静态内容存储

除了视频内容之外，我们还要存储Netflix电影和节目的各种静态内容，包括视频标题，说明和演员表。

此内容的大小将受视频内容的大小限制，因为它将与视频内容一样与电影和电视节目的数量绑定在一起，并且自然会比视频数据占用更少的空间。

我们可以轻松地将所有这些静态内容存储在关系数据库甚至文档存储中，并且可以将大多数静态内容缓存在我们的API服务器中。

5. 用户元数据存储

我们可以期望在Netflix平台上为每个视频存储一些用户元数据。 例如，我们可能要存储用户留下视频的时间戳记，用户对视频的评分等。

就像上面提到的静态内容一样，此用户元数据将与Netflix上的视频数量绑定在一起。 但是，与静态内容不同，此用户元数据将随着Netflix用户群的增长而增长，因为每个用户都将拥有用户元数据。

我们可以快速估计此用户元数据需要多少空间：

```
  ~200M users
  ~1K videos watched per user per lifetime (~10% of total content)
  ~100 bytes/video/user
  ~100 bytes * 1K videos * 200M users = 100 KB * 200M = 1 GB * 20K = 20 TB
```

也许令人惊讶的是，我们将要存储的用户元数据量与要存储的视频内容量相同。 再一次，这是由于Netflix视频内容的有限性，这与其用户群的无限性形成了鲜明的对比。

我们可能需要查询此元数据，因此将其存储在像Postgres这样的经典关系数据库中是有意义的。

由于Netflix用户实际上是彼此隔离的（例如，它们没有像在社交媒体平台上那样相互连接），因此我们可以期望我们所有对延迟敏感的数据库操作仅与单个用户有关。 换句话说，诸如GetUserInfo和GetUserWatchedVideos之类的可能需要快速等待时间的操作特定于特定用户。 另一方面，涉及多个用户元数据的复杂数据库操作很可能是后台数据工程作业的一部分，这些作业不关心延迟。

鉴于此，我们可以将用户元数据数据库拆分为几个分片，每个分片可以管理1到10 TB的索引数据。 这将保持给定用户的非常快的读写。

6. 通用C/S交互

系统中处理为用户提供用户元数据和静态内容的部分应该不会太复杂。

我们可以使用一些简单的循环负载平衡来在我们的API服务器上分配最终用户网络请求，然后可以根据userId对数据库请求进行负载平衡（因为我们的数据库将基于userId进行分片）。

如上所述，我们可以将静态内容缓存在我们的API服务器中，在发布新电影和电视节目时定期对其进行更新，甚至可以使用透写缓存机制在其中缓存用户元数据。

7. 视频内容交付

我们需要弄清楚如何在不增加延迟的情况下在全球范围内交付Netflix的视频内容。 首先，我们将估计在任何时间点都可以预期的最大带宽消耗量。 我们假设在流量高峰时，例如当一部热门电影上映时，相当多的Netflix用户可能会同时流式传输视频内容。

```
  ~200M total users
  ~5% of total users streaming concurrently during peak hours
  ~20 GB/h of HD video ~= 5 MB/s of HD video
  ~5% of 200M * 5 MB/s = 10M * 5 MB/s = 50 TB/s 
```

这种带宽消耗水平意味着我们不能仅仅从单个数据中心甚至数十个数据中心中天真地提供视频内容。 我们需要在世界各地成千上万个地点为我们分发此内容。 值得庆幸的是，CDN解决了这个精确的问题，因为它们在全球拥有成千上万的存在点。 因此，我们可以使用Cloudflare这样的CDN，并在CDN的PoP中提供我们的视频内容。

由于PoP无法将Netflix的全部视频内容保留在缓存中，因此我们可以使用外部服务，该服务会定期用最重要的内容（最有可能被观看的电影和电视节目）填充CDN PoP。

8. 用户活动数据处理

我们需要弄清楚如何处理大量用户活动数据以将其输入到Netflix的推荐引擎中。 我们可以想象，该用户活动数据将以各种用户操作生成的日志的形式收集。 我们可以预期每天都会生成TB级的日志。

MapReduce可以在这里为我们提供帮助。 我们可以将日志存储在HDFS之类的分布式文件系统中，并运行MapReduce作业以并行处理大量数据。 然后，可以将这些作业的结果输入到某些机器学习管道中，或简单地存储在数据库中。

Map输入：

我们的map输入可以是我们的原始日志，如下所示：

```
{"userId": "userId1", "videoId": "videoId1", "event": "CLICK"}
{"userId": "userId2", "videoId": "videoId2", "event": "PAUSE"}
{"userId": "userId3", "videoId": "videoId3", "event": "MOUSE_MOVE"}
```

Map输出/Reduce输入：

我们的Map函数将基于userId聚合日志，并返回在每个userId上建立索引的中间键值对，指向包含videoId和相关事件的元组列表。

这些中间的k / v对将被适当地改组并输入到我们的Reduce函数中。

```
{"userId1": [("CLICK", "videoId1"), ("CLICK", "videoId1"), ..., ("PAUSE", "videoId2")]}
{"userId2": [("PLAY", "videoId1"), ("MOUSE_MOVE", "videoId2"), ..., ("MINIMIZE", "videoId3")]}
```

Reduce输出：

我们的Reduce函数可以返回许多不同的输出。 他们可以为每个userId | videoId组合返回k / v对，指向该用户/视频对的计算得分； 他们可以返回在每个userId处索引的k / v对，指向（videoId，得分）元组的列表； 或者他们可以返回k / v对，它们也以每个userId索引，但根据计算出的分数指向videoId的堆栈排名。

```
("userId1|videoId1", score)
("userId1|videoId2", score)

OR

{"userId1": [("videoId1", score), ("videoId2", score), ..., ("videoId3", score)]}
{"userId2": [("videoId1", score), ("videoId2", score), ..., ("videoId3", score)]}  

OR

("userId1", ["videoId1", "videoId2", ..., "videoId3"])
("userId2", ["videoId1", "videoId2", ..., "videoId3"])
```

9. 系统架构图

![Final Systems Architecture](https://assets.algoexpert.io/se-diagrams/netflix.svg)

## 设计Stockbroker

一个平台，作为最终客户和一些中央证券交易所之间的中介。

1. 收集需求

我们正在构建像Robinhood这样的股票经纪平台，该平台充当最终客户和某些中央证券交易所之间的中介。 这个想法是，中央证券交易所是实际执行股票交易的平台，而股票经纪人只是客户想要进行交易时与之交谈的平台-股票经纪业务“更简单”且更“易于理解” “， 可以这么说。

我们只关心支持市场交易-以当前股价执行的交易-并且我们可以假设我们的系统将客户余额（即客户先前可能存入的资金）存储在SQL表中。

我们需要设计一个PlaceTrade API调用，并且我们知道中央交易所的等效API方法将接受一个回调，该回调保证在对该API方法的调用完成后执行。

我们正在设计此系统，以支持来自单个地区（例如美国）数百万客户的每日数百万笔交易。 我们希望该系统高度可用。

2. 制定计划

我们将从头到尾处理设计：

客户将进行的PlaceTrade API调用

API服务器处理客户端API调用

负责为每个客户执行订单的系统

我们需要确保以下条件成立：

没有成功或失败，交易永远不会永远停滞不前

单个客户的交易必须按照其下达顺序执行

余额不能为负

3. API调用

我们必须实现的核心API调用是PlaceTrade。

我们将其签名定义为：

```
  PlaceTrade(
    customerId: string,
    stockTicker: string,
    type: string (BUY/SELL),
    quantity: integer,
  ) => (
    tradeId: string,
    stockTicker: string,
    type: string (BUY/SELL),
    quantity: integer,
    createdAt: timestamp,
    status: string (PLACED),
    reason: string,
  )
```

客户ID可以从仅用户知道并传递到API调用中的身份验证令牌派生。

状态可以是以下之一：

PLACED

IN PROGRESS

FILLED

REJECTED

话虽如此，PLACED实际上将是事实上的状态，因为一旦交易所执行了我们的回调，其他状态将被异步设置。 换句话说，当PlaceTrade API调用返回时，交易状态将始终为PLACED，但是我们可以想象GetTrade API调用可以返回除PLACED之外的其他状态。

拒绝交易的潜在原因可能是：

不充足的资金

随机错误

过期的交易时间

4. API Server

我们将需要多个API服务器来处理所有传入请求。 由于进行交易时不需要任何缓存，因此不需要任何服务器粘性，我们可以使用一些循环负载平衡来在API服务器之间分配传入请求。

API服务器收到PlaceTrade调用后，会将交易存储在SQL表中。 该表必须与余额表所在的数据库位于同一SQL数据库中，因为我们需要使用ACID事务以原子方式更改两个表。

用于trades的SQL表将如下所示：

id：字符串，一个随机的，自动生成的字符串

customer_id：字符串，进行交易的客户的id

stockTicker：字符串，正在交易的股票的股票代号

type：字符串，BUY或SELL

quantity：整数（无小数份额），要交易的份额数

status：字符串，交易状态； 从PLACED开始

created_at：时间戳记，创建交易的时间

reason：字符串，易于理解的交易状态证明



balance的SQL表如下所示：



id：字符串，一个随机的，自动生成的字符串

customer_id：字符串，与余额相关的客户的id

amount：浮点，客户拥有的美元金额

last_modified：时间戳，最后一次修改余额的时间

5. 交易执行队列









