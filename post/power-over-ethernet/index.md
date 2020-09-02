
![](/1a88345c-908c-4ffb-9ca0-fd0d46895968.jpg)

Power over ethernet，即 PoE，是一种使用网线供电的技术。有了这项技术，可以通过一根网线为设备同时供电和传输数据。

<!--more-->

### 功能与优点

PoE 的供电设备被称为 **PSE（power sourcing equipment）**，受电设备被称为 **PD（powered device）**[^4]。其中 PSE 可以是交换机，也可以是非 PoE 交换机搭配 PoE 供电模块；常见的 PD 包括 VoIP 电话、无线接入点和 IP 摄像头等[^3]。

PoE 的优点[^1]有：

- 安装方便、布线简单，只需要一根网线即可，即使在没有电源插座的房间中，也能为 PD 供电。
- 安全可靠。和 USB 充电一样，PoE 在设备接通前会有「握手」测试，确保二者兼容。同时，PoE 也具备对于断电等突发情况的保护措施。

### 原理

这张图表明了 PoE 的原理[^2]。

{{<figure src="/1b235464-1239-4caa-88d8-2c7c9a285a9a.png" title="PoE 的原理框图">}}

为什么 PoE 能使用数据线来供电？因为以太网传送数据的方式是使用一对线传输**差分信号**，而 PoE 是在此基础上，在这一对线上传输**共模直流电压**。这样，对微小噪声敏感的数据部分体现在差模电压上，能够可靠地被传输和解析；对噪声不敏感的能量部分体现在共模电压上，经过一定损耗传输到受电设备，两全其美。

### 版本与供电能力

目前 PoE 一共有三个版本的标准，定义了四个供电等级。

- IEEE 802.3af 定义了 Type 1 的供电等级，PSE 能提供 15.40W 的功率，距离交换机 100 米能提供 12.95W 功率，被称为「PoE」。
- IEEE 802.3at 定义了 Type 2 的供电等级，PSE 能提供 30.0W 的功率，距离交换机 100 米能提供 25.50W 功率，被称为「PoE+」。而前一个版本也因为它而被称为「IEEE 802.3at Type 1」。
- IEEE 802.3bt 定义了 Type 3 和 Type 4 的供电等级，前者 PSE 能提供 60W 的功率，距离交换机 100 米能提供 51W 功率，被称为「4PPoE」或「PoE++」；而后者 PSE 能提供 100W 的功率，距离交换机 100 米能提供 71W 功率，没有特殊称号。

总之，PoE 的标准和命名有点乱。三个先后出台的标准加起来，定义了 **1-4 四个等级的供电方式**，分别能输出 **15W、30W、60W、100W** 的功率，而前三种方式，分别有别名 **PoE、PoE+、4PPoE/PoE++**。关于四个等级的更多细节，请看[这里](https://en.wikipedia.org/wiki/Power_over_Ethernet#Standard_implementation)。

### 三种传输模式[^1]

PoE 有三种供电模式：

- **模式 B（alternative B 或 mode B）**：对于 100Mbps 以太网而言，传输数据使用的是其中两对差分线（引脚 1、2、3、6），而模式 B 就用剩下空闲的两对线（引脚 4、5、7、8）来供电。
- **模式 A（alternative A 或 mode A）**：这种方式下，使用引脚 1、2、3、6 来供电。无论何种速率的以太网，都会用这几个引脚来传输数据，因此模式 A 是将电力在数据线中传输。
- **4PPoE**：使用全部的四对线进行电力输送。

{{<figure src="/d29f7647-f8e8-4dbf-98b3-f7df4df5daf1.jpg" title="三种供电模式">}}

### 使用现状

经过了一点网上的简单调研，发现 PoE 主要还是用于一些简单的网络接入设备的供电，如路由器、IP 摄像头等，并且使用得也不是特别广泛。许多情境下，需要使用一个转接头，将同时携带信息和能量的网线，转换成只携带信息的网线和另一根电源线。

{{<figure src="/d5f493b8-ecd8-11ea-adc1-0242ac120002.jpg" title="PoE 经过转接后给设备供网和供电">}}

使用 PoE 给笔记本电脑供电也不实际，完全没有使用 USB-PD 充电灵活性强。因为：

- USB 充电功率更高，能达到 100W，而 PoE 只有较新的 Type 4 可以在源端达到 100W；在经过网络的长距离传输后，功率已经打折扣。
- USB 充电仅需要一个电源和充电头即可，而 PoE 充电必须要交换机支持 PoE。
- USB 本身的其它功能很多，可以支持数据传输、显示，甚至用来提供网络，而以太网只能提供网络接入而已，而且以太网不是唯一的联网方式（还有无线）。
- USB 线的便携性比以太网线要好很多。
- 许多新出的轻薄本已经没有以太网接口。

综上，PoE 是一项有趣的技术，用于小型联网设备很不错。

[^1]: [干货 | PoE 供电是什么？一文全解读 - 知乎](https://zhuanlan.zhihu.com/p/89045111)
[^2]: [以太网供电 | 概述 | 电源 IC | 德州仪器 TI.com.cn](https://www.ti.com.cn/zh-cn/power-management/power-over-ethernet-poe/overview.html)
[^3]: [什么是 PoE？（以太网供电） | Answer | NETGEAR Support](https://kb.netgear.com/zh_CN/209/%E4%BB%80%E4%B9%88%E6%98%AF-PoE-%E4%BB%A5%E5%A4%AA%E7%BD%91%E4%BE%9B%E7%94%B5)
[^4]: [Power over Ethernet - Wikipedia](https://en.wikipedia.org/wiki/Power_over_Ethernet#Standard_implementation)