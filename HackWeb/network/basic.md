网络基础
========================================

计算机通信网的组成
----------------------------------------
计算机网络由通信子网和资源子网组成。其中通信子网负责数据的无差错和有序传递，其处理功能包括差错控制、流量控制、路由选择、网络互连等。

其中资源子网是计算机通信的本地系统环境，包括主机、终端和应用程序等， 资源子网的主要功能是用户资源配置、数据的处理和管理、软件和硬件共享以及负载 均衡等。

总的来说，计算机通信网就是一个由通信子网承载的、传输和共享资源子网的各类信息的系统。

通信协议
----------------------------------------
为了完成计算机之间有序的信息交换，提出了通信协议的概念，其定义是相互通信的双方（或多方）对如何进行信息交换所必须遵守的一整套规则。

协议涉及到三个要素，分别为：

- 语法：语法是用户数据与控制信息的结构与格式，以及数据出现顺序的意义
- 语义：用于解释比特流的每一部分的意义
- 时序：事件实现顺序的详细说明

OSI七层模型
----------------------------------------

简介
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
OSI（Open System Interconnection）共分为物理层、数据链路层、网络层、传输层、会话层、表示层、应用层七层，其具体的功能如下。

物理层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 提供建立、维护和释放物理链路所需的机械、电气功能和规程等特性
- 通过传输介质进行数据流(比特流)的物理传输、故障监测和物理层管理
- 从数据链路层接收帧，将比特流转换成底层物理介质上的信号

数据链路层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 在物理链路的两端之间传输数据
- 在网络层实体间提供数据传输功能和控制
- 提供数据的流量控制
- 检测和纠正物理链路产生的差错
- 格式化的消息称为帧

网络层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 负责端到端的数据的路由或交换，为透明地传输数据建立连接
- 寻址并解决与数据在异构网络间传输相关的所有问题
- 使用上面的传输层和下面的数据链路层的功能
- 格式化的消息称为分组

传输层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 提供无差错的数据传输
- 接收来自会话层的数据，如果需要，将数据分割成更小的分组，向网络层传送分组并确保分组完整和正确到达它们的目的地
- 在系统之间提供可靠的透明的数据传输,提供端到端的错误恢复和流量控制

会话层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 提供节点之间通信过程的协调
- 负责执行会话规则（如：连接是否允许半双工或全双工通信）、同步数据流以及当故障发生时重新建立连接
- 使用上面的表示层和下面的传输层的功能

表示层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 提供数据格式、变换和编码转换
- 涉及正在传输数据的语法和语义
- 将消息以合适电子传输的格式编码
- 执行该层的数据压缩和加密
- 从应用层接收消息，转换格式，并传送到会话层，该层常合并在应用层中

应用层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 包括各种协议，它们定义了具体的面向用户的应用：如电子邮件、文件传输等

总结
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
低三层模型属于通信子网，涉及为用户间提供透明连接，操作主要以每条链路（ hop-by-hop）为基础，在节点间的各条数据链路上进行通信。由网络层来控制各条链路上的通信，但要依赖于其他节点的协调操作。

高三层属于资源子网，主要涉及保证信息以正确可理解形式传送。

传输层是高三层和低三层之间的接口，它是第一个端到端的层次，保证透明的端到端连接，满足用户的服务质量（QoS）要求，并向高三层提供合适的信息形式。