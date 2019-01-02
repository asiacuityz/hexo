title: Eureka配置大全
author: Asia Cui
tags:
  - Spring Cloud
categories:
  - Eureka
date: 2019-01-28 18:28:00
---
##  Eureka客户端配置

1、RegistryFetchIntervalSeconds

从 eureka 服务器注册表中获取注册信息的时间间隔（s），默认为 30 秒

2、InstanceInfoReplicationIntervalSeconds

复制实例变化信息到 eureka 服务器所需要的时间间隔（s），默认为 30 秒

3、InitialInstanceInfoReplicationIntervalSeconds

最初复制实例信息到 eureka 服务器所需的时间（s），默认为 40 秒

4、EurekaServiceUrlPollIntervalSeconds

询问 Eureka 服务 url 信息变化的时间间隔（s），默认为 300 秒

5、ProxyHost

获取 eureka 服务的代理主机，默认为 null

6、ProxyProxyPort

获取 eureka 服务的代理端口, 默认为 null 

 7、ProxyUserName

获取 eureka 服务的代理用户名，默认为 null

 8、ProxyPassword

获取 eureka 服务的代理密码，默认为 null 

 9、GZipContent

 eureka 注册表的内容是否被压缩，默认为 true，并且是在最好的网络流量下被压缩

10、EurekaServerReadTimeoutSeconds

eureka 需要超时读取之前需要等待的时间，默认为 8 秒

11、EurekaServerConnectTimeoutSeconds

eureka 需要超时连接之前需要等待的时间，默认为 5 秒

12、BackupRegistryImpl

获取实现了 eureka 客户端在第一次启动时读取注册表的信息作为回退选项的实现名称

13、EurekaServerTotalConnections

 eureka 客户端允许所有 eureka 服务器连接的总数目，默认是 200

 14、EurekaServerTotalConnectionsPerHost

 eureka 客户端允许 eureka 服务器主机连接的总数目，默认是 50

 15、EurekaServerURLContext

表示 eureka 注册中心的路径，如果配置为 eureka，则为 http://x.x.x.x:x/eureka/，在 eureka 的配置文件中加入此配置表示 eureka 作为客户端向注册中心注册，从而构成 eureka 集群。此配置只有在 eureka 服务器 ip 地址列表是在 DNS 中才会用到，默认为 null

16、EurekaServerPort

获取 eureka 服务器的端口，此配置只有在 eureka 服务器 ip 地址列表是在 DNS 中才会用到。默认为 null

17、EurekaServerDNSName

获取要查询的 DNS 名称来获得 eureka 服务器，此配置只有在 eureka 服务器 ip 地址列表是在 DNS 中才会用到。默认为 null

18、UseDnsForFetchingServiceUrls

eureka 客户端是否应该使用 DNS 机制来获取 eureka 服务器的地址列表，默认为 false

19、RegisterWithEureka

实例是否在 eureka 服务器上注册自己的信息以供其他服务发现，默认为 true

20、PreferSameZoneEureka

实例是否使用同一 zone 里的 eureka 服务器，默认为 true，理想状态下，eureka 客户端与服务端是在同一 zone 下

21、AllowRedirects

服务器是否能够重定向客户端请求到备份服务器。 如果设置为 false，服务器将直接处理请求，如果设置为 true，它可能发送 HTTP 重定向到客户端。默认为 false

22、LogDeltaDiff

是否记录 eureka 服务器和客户端之间在注册表的信息方面的差异，默认为 false

23、DisableDelta(*)

默认为 false

24、fetchRegistryForRemoteRegions

eureka服务注册表信息里的以逗号隔开的地区名单，如果不这样返回这些地区名单，则客户端启动将会出错。默认为 null

25、Region

获取实例所在的地区。默认为 us-east-1

26、AvailabilityZones

获取实例所在的地区下可用性的区域列表，用逗号隔开。

27、EurekaServerServiceUrls

Eureka 服务器的连接，默认为 http：//XXXX：X/eureka/, 但是如果采用 DNS方式获取服务地址，则不需要配置此设置。

28、FilterOnlyUpInstances（*）

是否获得处于开启状态的实例的应用程序过滤之后的应用程序。默认为 true

29、EurekaConnectionIdleTimeoutSeconds

Eureka 服务的 http 请求关闭之前其响应的时间，默认为 30 秒

30、FetchRegistry

此客户端是否获取 eureka 服务器注册表上的注册信息，默认为 true

31、RegistryRefreshSingleVipAddress

此客户端只对一个单一的 VIP 注册表的信息感兴趣。默认为 null

32、HeartbeatExecutorThreadPoolSize(*)

心跳执行程序线程池的大小, 默认为 5

33、HeartbeatExecutorExponentialBackOffBound(*)

心跳执行程序回退相关的属性，是重试延迟的最大倍数值，默认为 10

34、CacheRefreshExecutorThreadPoolSize(*)

执行程序缓存刷新线程池的大小，默认为 5

35、CacheRefreshExecutorExponentialBackOffBound

执行程序指数回退刷新的相关属性，是重试延迟的最大倍数值，默认为 10

36、DollarReplacement

eureka 服务器序列化 / 反序列化的信息中获取 “$” 符号的的替换字符串。默认为 “_-”

37、EscapeCharReplacement

eureka 服务器序列化 / 反序列化的信息中获取 “_” 符号的的替换字符串。默认为 “__”

38、OnDemandUpdateStatusChange（*）

如果设置为 true, 客户端的状态更新将会点播更新到远程服务器上，默认为 true

39、EncoderName

这是一个短暂的编码器的配置，如果最新的编码器是稳定的，则可以去除，默认为 null

40、DecoderName

这是一个短暂的解码器的配置，如果最新的解码器是稳定的，则可以去除，默认为 null

41、ClientDataAccept（*）

客户端数据接收

42、Experimental（*）

当尝试新功能迁移过程时，为了避免配置 API 污染，相应的配置即可投入实验配置部分，默认为 null

##  Eureka微服务端配置

1、InstanceId

此实例注册到 eureka 服务端的唯一的实例 ID, 其组成为 、`${spring.application.name}:${spring.application.instance_id:${random.value}}`

2、Appname

获得在 eureka 服务上注册的应用程序的名字，默认为 unknow

3、AppGroupName

获得在 eureka 服务上注册的应用程序组的名字，默认为 unknow

4、InstanceEnabledOnit（*）

实例注册到 eureka 服务器时，是否开启通讯，默认为 false

5、NonSecurePort

获取该实例应该接收通信的非安全端口。默认为 80

6、SecurePort

获取该实例应该接收通信的安全端口，默认为 443

7、NonSecurePortEnabled

该实例应该接收通信的非安全端口是否启用，默认为 true

8、SecurePortEnabled

该实例应该接收通信的安全端口是否启用，默认为 false

9、LeaseRenewalIntervalInSeconds

eureka 客户需要多长时间发送心跳给 eureka 服务器，表明它仍然活着, 默认为 30 秒

10、LeaseExpirationDurationInSeconds

Eureka 服务器在接收到实例的最后一次发出的心跳后，需要等待多久才可以将此实例删除，默认为 90 秒

11、VirtualHostName

此实例定义的虚拟主机名，其他实例将通过使用虚拟主机名找到该实例。

12、SecureVirtualHostName

此实例定义的安全虚拟主机名

13、ASGName（*）

与此实例相关联 AWS 自动缩放组名称。此项配置是在 AWS环境专门使用的实例启动，它已被用于流量停用后自动把一个实例退出服务。

14、HostName

与此实例相关联的主机名，是其他实例可以用来进行请求的准确名称

15、MetadataMap(*)

获取与此实例相关联的元数据 (key,value)。这个信息被发送到 eureka 服务器，其他实例可以使用。

16、DataCenterInfo（*）

该实例被部署在数据中心

17、IpAddress

获取实例的 ip 地址

18、StatusPageUrlPath（*）

获取此实例状态页的 URL 路径，然后构造出主机名，安全端口等，默认为 /info

19、StatusPageUrl(*)

获取此实例绝对状态页的 URL 路径，为其他服务提供信息时来找到这个实例的状态的路径，默认为 null

20、HomePageUrlPath（*）

获取此实例的相关主页 URL 路径，然后构造出主机名，安全端口等，默认为 /

21、HomePageUrl(*)

获取此实例的绝对主页 URL 路径，为其他服务提供信息时使用的路径, 默认为 null

22、HealthCheckUrlPath

获取此实例的相对健康检查 URL 路径，默认为 /health

23、HealthCheckUrl

获取此实例的绝对健康检查 URL 路径, 默认为 null

24、SecureHealthCheckUrl

获取此实例的绝对安全健康检查网页的 URL 路径，默认为 null

25、DefaultAddressResolutionOrder

获取实例的网络地址，默认为 []

26、Namespace

获取用于查找属性的命名空间，默认为 eureka

Eureka服务端配置

1、AWSAccessId

获取 aws 访问的 id，主要用于弹性 ip 绑定，此配置是用于 aws 上的，默认为 null

2、AWSSecretKey

获取 aws 私有秘钥，主要用于弹性 ip 绑定，此配置是用于 aws 上的，默认为 null

3、EIPBindRebindRetries

获取服务器尝试绑定到候选的 EIP 的次数，默认为 3

4、EIPBindingRetryIntervalMsWhenUnbound(*)

服务器检查 ip 绑定的时间间隔，单位为毫秒，默认为 1 * 60 * 1000

5、EIPBindingRetryIntervalMs

与上面的是同一作用，仅仅是稳定状态检查，默认为 5 * 60 * 1000

6、EnableSelfPreservation

自我保护模式，当出现出现网络分区、eureka 在短时间内丢失过多客户端时，会进入自我保护模式，即一个服务长时间没有发送心跳，eureka 也不会将其删除，默认为 true

7、RenewalPercentThreshold(*)

![](/images/微服务/952935-20170623150643570-1733575926.png)

阈值因子，默认是 0.85，如果阈值比最小值大，则自我保护模式开启

8、RenewalThresholdUpdateIntervalMs

阈值更新的时间间隔，单位为毫秒，默认为 15 * 60 * 1000

9、PeerEurekaNodesUpdateIntervalMs(*)

集群里 eureka 节点的变化信息更新的时间间隔，单位为毫秒，默认为 10 * 60 * 1000

10、EnableReplicatedRequestCompression

复制的数据在发送请求时是否被压缩，默认为 false

11、NumberOfReplicationRetries

获取集群里服务器尝试复制数据的次数，默认为 5

12、PeerEurekaStatusRefreshTimeIntervalMs

服务器节点的状态信息被更新的时间间隔，单位为毫秒，默认为 30 * 1000

13、WaitTimeInMsWhenSyncEmpty(*)

在 Eureka 服务器获取不到集群里对等服务器上的实例时，需要等待的时间，单位为毫秒，默认为 1000 * 60 * 5

14、PeerNodeConnectTimeoutMs

连接对等节点服务器复制的超时的时间，单位为毫秒，默认为 200

15、PeerNodeReadTimeoutMs

读取对等节点服务器复制的超时的时间，单位为毫秒，默认为 200

16、PeerNodeTotalConnections

获取对等节点上 http 连接的总数，默认为 1000

17、PeerNodeTotalConnectionsPerHost(*)

获取特定的对等节点上 http 连接的总数，默认为 500

18、PeerNodeConnectionIdleTimeoutSeconds(*)

http 连接被清理之后服务器的空闲时间，默认为 30 秒

19、RetentionTimeInMSInDeltaQueue(*)

客户端保持增量信息缓存的时间，从而保证不会丢失这些信息，单位为毫秒，默认为 3 * 60 * 1000

20、DeltaRetentionTimerIntervalInMs

清理任务程序被唤醒的时间间隔，清理过期的增量信息，单位为毫秒，默认为 30 * 1000

21、EvictionIntervalTimerInMs

过期实例应该启动并运行的时间间隔，单位为毫秒，默认为 60 * 1000

22、ASGQueryTimeoutMs（*）

查询 AWS 上 ASG（自动缩放组）信息的超时值，单位为毫秒，默认为 300

23、ASGUpdateIntervalMs

从 AWS 上更新 ASG 信息的时间间隔，单位为毫秒，默认为 5 * 60 * 1000

24、ASGCacheExpiryTimeoutMs(*)

缓存 ASG 信息的到期时间，单位为毫秒，默认为 10 * 60 * 1000

25、ResponseCacheAutoExpirationInSeconds（*）

当注册表信息被改变时，则其被保存在缓存中不失效的时间，默认为 180 秒

26、ResponseCacheUpdateIntervalMs（*）

客户端的有效负载缓存应该更新的时间间隔，默认为 30 * 1000 毫秒

27、UseReadOnlyResponseCache（*）

目前采用的是二级缓存策略，一个是读写高速缓存过期策略，另一个没有过期只有只读缓存，默认为 true，表示只读缓存

28、DisableDelta（*）

增量信息是否可以提供给客户端看，默认为 false

29、MaxIdleThreadInMinutesAgeForStatusReplication（*）

状态复制线程可以保持存活的空闲时间，默认为 10 分钟

30、MinThreadsForStatusReplication

被用于状态复制的线程的最小数目，默认为 1

31、MaxThreadsForStatusReplication

被用于状态复制的线程的最大数目，默认为 1

32、MaxElementsInStatusReplicationPool

可允许的状态复制池备份复制事件的最大数量，默认为 10000

33、SyncWhenTimestampDiffers

当时间变化实例是否跟着同步，默认为 true

34、RegistrySyncRetries

当 eureka 服务器启动时尝试去获取集群里其他服务器上的注册信息的次数，默认为 5

35、RegistrySyncRetryWaitMs

当 eureka 服务器启动时获取其他服务器的注册信息失败时，会再次尝试获取，期间需要等待的时间，默认为 30 * 1000 毫秒

36、MaxElementsInPeerReplicationPool（*）

复制池备份复制事件的最大数量，默认为 10000

37、MaxIdleThreadAgeInMinutesForPeerReplication（*）

复制线程可以保持存活的空闲时间，默认为 15 分钟

38、MinThreadsForPeerReplication（*）

获取将被用于复制线程的最小数目，默认为 5

39、MaxThreadsForPeerReplication

获取将被用于复制线程的最大数目，默认为 20

40、MaxTimeForReplication（*）

尝试在丢弃复制事件之前进行复制的时间，默认为 30000 毫秒

41、PrimeAwsReplicaConnections（*）

对集群中服务器节点的连接是否应该准备，默认为 true

42、DisableDeltaForRemoteRegions（*）

增量信息是否可以提供给客户端或一些远程地区，默认为 false

43、RemoteRegionConnectTimeoutMs（*）

连接到对等远程地 eureka 节点的超时时间，默认为 1000 毫秒

44、RemoteRegionReadTimeoutMs（*）

获取从远程地区 eureka 节点读取信息的超时时间，默认为 1000 毫秒

45、RemoteRegionTotalConnections

获取远程地区对等节点上 http 连接的总数，默认为 1000

46、RemoteRegionTotalConnectionsPerHost

获取远程地区特定的对等节点上 http 连接的总数，默认为 500

47、RemoteRegionConnectionIdleTimeoutSeconds

http 连接被清理之后远程地区服务器的空闲时间，默认为 30 秒

48、GZipContentFromRemoteRegion（*）

eureka 服务器中获取的内容是否在远程地区被压缩，默认为 true

49、RemoteRegionUrlsWithName

针对远程地区发现的网址域名的 map

50、RemoteRegionUrls

远程地区的 URL 列表

51、RemoteRegionAppWhitelist（*）

必须通过远程区域中检索的应用程序的列表

52、RemoteRegionRegistryFetchInterval

从远程区域取出该注册表的信息的时间间隔，默认为 30 秒

53、RemoteRegionFetchThreadPoolSize

用于执行远程区域注册表请求的线程池的大小，默认为 20

54、RemoteRegionTrustStore

用来合格请求远程区域注册表的信任存储文件，默认为空

55、RemoteRegionTrustStorePassword

获取偏远地区信任存储文件的密码，默认为 “changeit”

56、disableTransparentFallbackToOtherRegion(*)

如果在远程区域本地没有实例运行，对于应用程序回退的旧行为是否被禁用， 默认为 false

57、BatchReplication(*)

表示集群节点之间的复制是否为了网络效率而进行批处理，默认为 false

58、LogIdentityHeaders(*)

Eureka 服务器是否应该登录 clientAuthHeaders，默认为 true

59、RateLimiterEnabled

限流是否应启用或禁用，默认为 false

60、RateLimiterThrottleStandardClients

是否对标准客户端进行限流，默认 false

61、RateLimiterPrivilegedClients（*）

认证的客户端列表，这里是除了标准的 eureka Java 客户端。

62、RateLimiterBurstSize（*）

速率限制的 burst size ，默认为 10，这里用的是令牌桶算法

63、RateLimiterRegistryFetchAverageRate(*)

速率限制器用的是令牌桶算法，此配置指定平均执行注册请求速率，默认为 500

64、RateLimiterFullFetchAverageRate（*）

速率限制器用的是令牌桶算法，此配置指定平均执行请求速率，默认为 100

65、ListAutoScalingGroupsRoleName（*）

用来描述从 AWS 第三账户的自动缩放组中的角色名称，默认为 “ListAutoScalingGroups”

66、JsonCodecName（*）

如果没有设置默认的编解码器将使用全 JSON 编解码器，获取的是编码器的类名称

67、XmlCodecName(*)

如果没有设置默认的编解码器将使用 xml 编解码器，获取的是编码器的类名称

68、BindingStrategy(*)

获取配置绑定 EIP 或 Route53 的策略。

69、Route53DomainTTL（*）

用于建立 route53 域的 ttl，默认为 301

70、Route53BindRebindRetries（*）

服务器尝试绑定到候选 Route53 域的次数，默认为 3

71、Route53BindingRetryIntervalMs（*）

服务器应该检查是否和 Route53 域绑定的时间间隔，默认为 5 * 60 * 1000 毫秒

72、Experimental(*)

当尝试新功能迁移过程时，为了避免配置 API 污染，相应的配置即可投入实验配置部分，默认为 null

以上是 Eureka 配置项的详细说明，分为 Eureka 客户端配置、Eureka 服务端配置和微服务端配置，一共 100 多项，其中有很多配置参数并不需要我们去修改，使用默认的就好，有些跟我们业务相关的配置参数可根据需要自行设置。

## Eureka配置案例

### 开发环境服务实例及时下线

##### [Eureka自我保护机制](https://www.cnblogs.com/xishuai/p/spring-cloud-eureka-safe.html)

Eureka Server 在运行期间会去统计心跳失败比例在 15 分钟之内是否低于 85%，如果低于 85%，Eureka Server 会将这些实例保护起来，让这些实例不会过期，但是在保护期内如果服务刚好这个服务提供者非正常下线了，此时服务消费者就会拿到一个无效的服务实例，此时会调用失败，对于这个问题需要服务消费者端要有一些容错机制，如重试，断路器等。

我们在单机测试的时候很容易满足心跳失败比例在 15 分钟之内低于 85%，这个时候就会触发 Eureka 的保护机制，一旦开启了保护机制，则服务注册中心维护的服务实例就不是那么准确了，此时我们可以使用`eureka.server.enable-self-preservation=false`来关闭保护机制，这样可以确保注册中心中不可用的实例被及时的剔除（**不推荐**）。

自我保护模式被激活的条件是：在 1 分钟后，`Renews (last min) < Renews threshold`。

这两个参数的意思：

- `Renews threshold`：**Eureka Server 期望每分钟收到客户端实例续约的总数**。
- `Renews (last min)`：**Eureka Server 最后 1 分钟收到客户端实例续约的总数**。

具体的值，我们可以在 Eureka Server 界面可以看到。

Renews threshold 计算代码：

```java
this.expectedNumberOfRenewsPerMin = count * 2;
this.numberOfRenewsPerMinThreshold = (int) (this.expectedNumberOfRenewsPerMin * serverConfig.getRenewalPercentThreshold());
```

解决方式有三种：

- 关闭自我保护模式（`eureka.server.enable-self-preservation`设为`false`），**不推荐**。
- 降低`renewalPercentThreshold`的比例（`eureka.server.renewal-percent-threshold`设置为`0.5`以下，比如`0.49`），**不推荐**。
- 部署多个 Eureka Server 并开启其客户端行为（`eureka.client.register-with-eureka`不要设为`false`，默认为`true`），**推荐**。

Eureka 的自我保护模式是有意义的，该模式被激活后，它不会从注册列表中剔除因长时间没收到心跳导致租期过期的服务，而是等待修复，直到心跳恢复正常之后，它自动退出自我保护模式。这种模式旨在避免因网络分区故障导致服务不可用的问题。例如，两个客户端实例 C1 和 C2 的连通性是良好的，但是由于网络故障，C2 未能及时向 Eureka 发送心跳续约，这时候 Eureka 不能简单的将 C2 从注册表中剔除。因为如果剔除了，C1 就无法从 Eureka 服务器中获取 C2 注册的服务，但是这时候 C2 服务是可用的。

所以，Eureka 的自我保护模式最好还是开启它。

##### 开发环境需要及时快速下线配置

- 服务端配置

  ```yaml
  eureka:
    instance:
      #心跳时间间隔（秒）
      lease-renewal-interval-in-seconds: 5
    server:
      #eureka自我保护机制
      enable-self-preservation: false
      #删除失效或下线服务间隔 （毫秒），具体删除时间还要加上心跳时间间隔和等待时间间隔
      eviction-interval-timer-in-ms: 1000
  ```

- 客户端配置

  ```yaml
  eureka:
    instance:
    	#Eureka客户端向服务端发送心跳的时间间隔（秒）（客户端告诉服务端自己会按照该规则）
      lease-renewal-interval-in-seconds: 3
      ##Eureka服务端在收到最后一次心跳之后等待的时间上限（秒），超过则剔除（客户端告诉服务端按照此规则等待自己）
      lease-expiration-duration-in-seconds: 4
  ```
