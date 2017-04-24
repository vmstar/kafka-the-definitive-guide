## counsumer
consumer可以订阅到一个或者更多topic，以消息被生产的顺序来消费message，用offset来标记消费的进度，
offset属于元数据，被存在kafka或者kafka本身，所以consumer可以随时停止和加入
1.counsumer以counsumer group的形式工作的，group确保一个分区只能由一个counsumer来消费

## broker要加入集群必须有两个要求:
1. 有一样的zookeeper配置: zookeeper.connect。因为kafka的meta信息都是在zookeeper上存储.
2. 有区别于其他beoker的broker.id.如果有重复的broker.id，之后的broker会启动失败

## 操作系统调整
大多数linux的发行版本内核配置对于多数应用都是公平的，但是稍微调整可以对kafka的性能有更好的提升,
比如虚拟内存，网络，硬盘，通常都配置于/etc/sysctl.conf 

### 虚拟内存
通常linux的虚拟内存都是系统根据work load自动调节的。但是一般应用程序都会避免使用swap space，内存页面置换进入
磁盘对kafka的性能有很大的影响。另外kafka使用很多页面缓存，说明系统的内存不够了。

通常避免使用swapping就是配置swap space是0.但是有交换分区可以防止操作系统因为内存不足而杀进程。
因为这个原因一般建议把vm.swappiness设置的非常低，比如1.

为什么不把vm.swappiness设置成0
早期vm.swappiness是建议设置成0的，0的含义是在内存不够用的情况下使用swap。
但是自从linux kernel 3.5-rcl之后0的含义变成了永远不要使用swap。