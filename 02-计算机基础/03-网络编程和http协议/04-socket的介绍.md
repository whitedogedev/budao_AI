# socket 的介绍

**学习目标**

* 能够说出 socket 的作用

---

### 1. 问题思考

到目前为止我们学习了 ip 地址和端口号还有 tcp 传输协议，为了保证数据的完整性和可靠性我们使用 tcp 传输协议进行数据的传输，为了能够找到对应设备我们需要使用 ip 地址，为了区别某个端口的应用程序接收数据我们需要使用端口号，那么通信数据是如何完成传输的呢？  

使用 **socket** 来完成

### 2. socket 的概念

socket (简称 套接字) 是**进程之间通信一个工具**，好比现实生活中的**插座**，所有的家用电器要想工作都是基于插座进行，**进程之间想要进行网络通信需要基于这个 socket**。

**插座效果图:**


![插座](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z93815v4j20go0b0gn7.jpg)

**socket 效果图:**


![socket](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z938xha9j20go096jrm.jpg)

### 3. socket 的作用

负责**进程之间的网络数据传输**，好比数据的搬运工。

### 4. socket 使用场景

不夸张的说，只要跟**网络相关的应用程序或者软件都使用到了 socket **。


![socket使用场景](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z938fdmij20l406a0t3.jpg)


### 5. 小结

进程之间**网络数据的传输**可以通过 ** socket ** 来完成，** socket 就是进程间网络数据通信的工具。**













