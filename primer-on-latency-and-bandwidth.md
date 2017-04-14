# 速度是特性

最近几年里，网络性能优化（WPO）的出现和快速增长，表现出用户对于响应快速的用户体验的需求。这不仅仅是简单是加速连接世界带来的 “我们需要速度” 的心理作用，而是由实际经验显示出的需求。这些经验来自于对许多在线业务的底线性能的衡量。

* 更快的网站带来更高的用户参与度
* 更快的网站带来更高的用户留存
* 更快的网站带来更高的用户转换率

简单来说，速度是一个特性。为了实现这一点，我们需要去理解许多关于性能的因素和底层限制。在这一章中，我们会关注与两个描述所有网络流量的关键因素：延迟和带宽。

_延迟_

从信息源发送数据包到目的地接收到该数据包所需要的时间

_带宽_

逻辑或物理通道的最大吞吐量

![](/assets/import.png)

_图1-1 延迟和带宽_

为了更好的理解带宽和延迟是如何联系在一起的，在接下来有关 TCP， UDP 和所有应用层协议的章节中，我们会使用它们作为工具，深入挖掘其中的价值。

> **使用爱尔兰高速网络\(Hibernia Express\)降低跨洋延迟**
>
> 在金融领域中，对于高频交易算法来说延迟是一个重要的标准，几毫秒的变化会造成数百万的损失或者利润。
>
> 在 2015 年九月，爱尔兰网络下水了新的光纤网络\(Hibernia Express\)，这个网络设计用来确保纽约到伦敦的延迟最低。该项目的最低费用估计在三十亿美元智商，新的线路使得这两个城市之间的延迟达到了 58.95 ms，比所有现存的网络提升约 5 ms 左右，相当于每节约一毫秒需要花费至少 6 千万美元。
>
> 延迟是昂贵的 - 无论是字面上还是实际意义上

# 延迟的许多组成部分

延迟指的是消息或者数据包从源点到目的点的时间。这是一个简单又有用的定义，但是它隐藏了很多有用的信息。每一个系统都包含了很多的源或者组成部分，这些都在传递信息的过程中占用了一定的时间，理解哪些部分占用了时间对于描述系统性能是十分关键的。

让我们更仔细的了解一下在典型的互联网线路中常见的组成部分，了解它们在信息传递中的责任。

_传播延迟_

信息在发送者和接受者之间传播所需要的时间，这部分时间由距离和信号传递的速度决定。

_传播延迟_

将数据包中的所有比特放进线路中所需要的时间，这部分时间由数据包的长度和线路的数据速率决定。

_处理延迟_

处理数据包包头所需要的时间，用于检查比特级别的错误并且确定数据包的目的地。

_查询延迟_

数据包在队列中等待处理的时间。

The total latency between the client and the server is the sum of all the delays just listed. Propagation time is dictated by the distance and the medium through which the signal travels — as we will see, the propagation speed is usually within a small constant factor of the speed of light. On the other hand, transmission delay is dictated by the available data rate of the transmitting link and has nothing to do with the distance between the client and the server. As an example, let’s assume we want to transmit a 10 Mb file over two links: 1 Mbps and 100 Mbps. It will take 10 seconds to put the entire file "on the wire" over the 1 Mbps link and only 0.1 seconds over the 100 Mbps link.

note

Network data rates are typically measured in bits per second \(bps\), whereas data rates for non-network equipment are typically shown in bytes per second \(Bps\). This is a common source of confusion, pay close attention to the units.

For example, to put a 10 megabyte \(MB\) file "on the wire" over a 1Mbps link, we will need 80 seconds. 10MB is equal to 80Mb because there are 8 bits for every byte!

