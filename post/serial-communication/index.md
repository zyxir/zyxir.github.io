
最近的工作涉及到了一些串行通信的知识，了解到了有关 SerDes（Serializer/Deserializer，串行器/解串器）和 MGT（Multi-gigabit transceiver，数 G 比特的收发器）的许多知识，发现这块知识非常有趣，并且涉及到了许多有用的名词，于是进行了一些稍微深入的了解、调研。

<!--more-->

## 串行通信的基本知识[^1][^2][^3][^4][^5]

通信是一种信息交换的过程。数字信号在两个设备之间的传输，可以是串行（Serial）的，也可以是并行（Parallel）的。

{{< figure src="/post/serial-communication/parallel-and-serial-communication.svg" title="并行与串行通信" >}}

人的直觉通常是——并行通信同时用到了多根线，所以一定要快一些。可是，现实中绝大部分的数据传输方式都是串行的，例如 USB、以太网、RS 485、HDMI 等等。并行通信只在少数芯片之间的连接上能被找到，例如 CPU 的地址线与数据线就是并行的（为了确认这句话对于现代的 CPU 仍然有效，我特意翻了 [Intel® Core™ i7-900 的 Datasheet](https://www.intel.com/content/www/us/en/products/docs/processors/core/core-i7-900-ee-and-desktop-processor-series-datasheet-vol-2.html)）。这是为什么呢？这里就要谈到并行通信的局限性了。

### 并行通信的局限性

（由于这一块经过了许多独立思考，所以会罗嗦一点）

首先，需要明确的一点是，如果发送比特的频率相同，那么并行通信是比串行通信要快的。但是，并行通信本身有一些局限性，使其实际发送比特的频率比串行通信更低，或者更不适用于实际的系统。

#### 局限性一：Skew 与 Jitter

并行通信必须要保证每根线的传输是同步的，<u>每个数据进行传输时，必须要等到每一个比特都到达，才能进行下一个数据的传输</u>；如果每根线自顾自地传，各自的数据率都不相同，那么接收端多个通道的数据就无法组合成正确的数据了。因此，并行通信必须要<u>额外传输一个时钟信号，每根线上的传输需要和时钟信号同步</u>。但是，实际的传输中，没办法保证各个每根线的信道特性完全一样，复杂的外界条件，例如线的长度和物理特性的不统一，线的不同弯曲变形方式导致的电气特性改变，外界的干扰不一致等等，都会导致<u>每根传输线的信道特征各不相同，它们的传输延迟也将不一致</u>。这种不一致的延迟，被称作 **skew**；同时，由于电磁干扰、码间干扰、外界噪声等原因，哪怕是每根线自己，每次传输的延迟都是不一致的，这种各个周期之间的不同被称作 **jitter**。哪怕各个导线传输数据的频率是完全符合时钟信号的节拍的，也无法保证它们接收时仍然保持一致。在这种情况下，两个数据的传输之间的等待时间，或者说时钟周期，就应该要长一些，应该比 skew+jitter 的时间还要长，如下图所示。

{{< figure src="/post/serial-communication/parallel-skew-and-jitter.svg" title="并行通信的时钟周期受限" >}}

skew 和 jitter 的影响，对于速度不快（即时钟周期本来就比较长），或者距离较近的传输来说，不是很大。当传输的距离增大时，每个信道受到的各种干扰会更多，skew 和 jitter 也会更大；而当传输的速度非常快时，时钟周期非常短，那么同样大小的 skew 和 jitter 就会对传输的可靠性造成更大的影响。因此，**skew 和 jitter 使得并行通信只适合距离非常短或数据率不高的传输**，例如 CPU 与其它设备的数据通信。

#### 局限性二：串扰

数据传输时，传输线上的传播的是一个变化的电压，那么变化的电压就会在空间中激发出变化的电磁场。由于传输线的电容和电感特性，一根线上的电压和电流变化时，会在其它线上产生感应电压或感应电流，与其它线上本身传输的电压与电流信号发生叠加，这种现象叫做**串扰（crosstalk）**。两根传输线彼此的串扰大小，是随着信号频率的增大而增大的。沿着传输线的每一点上都会发生串扰，因此当传输的距离增大时，各个点的串扰信号叠加起来，在接收端观察到的串扰就会非常大；而当信号的数据率增加时，它的频率也会增大，串扰便会增大。虽然数字信号有一定的抗干扰能力，但是过大的串扰也会使得接收端不再能读取出正确的信号。因此，**串扰的存在，同样使并行通信只适合距离非常短或数据率不高的传输**。

{{< figure src="/post/serial-communication/Crosstalk_models.png" title="串扰的示意图" >}}

#### 局限性三：成本

第三个因素虽然不限制传输的速率和距离，但仍然是并行通信的一个重要的局限性。并行通信会需要用到更多根连接线、更复杂的接口，极大地提高成本和使用难度。

如果我们的鼠标使用并行通信，那么鼠标的连线可能会和你的手机的充电线一样又粗又难以弯折；如果以太网使用并行通信，那么以太网的接头会比现在复杂十多倍，巨大的接头难以设计在笔记本电脑上，网线会变的像铠装电缆一样粗，全世界因特网的布线成本也会增加几十倍（这里都没有考虑并行通信能不能传输那么远的问题了）。而且线多了之后，更容易坏；接头复杂了之后，更难用。对于复杂集成电路来说，更多的接口，也会使其制造难度和成本急剧增加。并行通信带来的复杂程度、制造成本与维护成本的提升，实在是太大。

{{< figure src="/post/serial-communication/Centronics_50_SCSI_connector.jfif" title="Parallel SCSI，一种并行接口，的插头" >}}

除此之外，并行传输的线太占空间，导致没有空间再为之设计屏蔽层了，因此并行线的抗干扰能力也会差得多。

### 串行通信的优势

并行通信的各种限制，使得它基本只适合一块印刷电路板上的短距离传输，而无法用于正常距离的传输。在大多数通信情形下，串行通信更为常见。它的优点，正是与并行通信的缺点一一对应的：

- 结构非常简单，成本低廉，容易实现。
- 对于异步串行通信（后面介绍）而言，不存在 skew，只需要考虑 jitter 的影响。
- 只有一根数据线，没有串扰的影响。
- 由于占用的空间更小，更容易设计屏蔽层来屏蔽外界的影响。
- 可以使用两根线来传输一对[差分信号](https://en.wikipedia.org/wiki/Differential_signaling)，非常有效地抵抗外界干扰。

{{< figure src="/post/serial-communication/800px-DiffSignaling.png" title="差分传输" >}}

正是因此，并行通信成了中远距离信号传输的主流方式。

### 单工、全双工与半双工

只要是通信，就都存在单工、全双工与半双工的概念。

**单工（Simplex）** 是指只能在一个方向上传输数据，一边固定发送，另一边固定接收。参考广播。

**半双工（Half duplex）** 是指虽然可以在任一个方向上传输数据，但是在同一时间只能在一个方向上传输数据，无法一边发一边收。参考对讲机。

**全双工（Full duplex）** 是指两边均可以同时收发数据。参考电话。

{{< figure src="/post/serial-communication/duplex.svg" title="三种工作模式" >}}

### 波特率

常用**波特率**（Baud Rate）来表示串行通信的速率，单位为波特（baud）。它与比特率不同的是，波特率是每秒内发送的码（符号）数，而并不是符号和比特并不是一一对应的，因为串行通信通常要对原始数据进行特殊的编码，以及加入一些额外的位。如果一个串行字符由 1 个起始位、7 个数据位、1 个奇偶校验位和 1 个停止位，一共 10 个数位构成，每秒传送 120 个字符，那么波特率就是：

$$
10 \\, \text{bit}/\text{char} \times 120 \\, \text{char}/\text{sec} = 1200 \\, \text{baud}
$$

### 串并转换的原理

这是一个简单的电路，能够将四位的串行数据转换为并行数据，耗时为四个时钟周期。

{{< figure src="serial-to-parallel-converter-circuit.gif" title="串/并转换" >}}

这是另一个简单的电路，能够将四位的并行数据转换为串行数据，耗时也是四个时钟周期。

{{< figure src="paste_image5.png" title="并/串转换" >}}

高速通信中用于进行串/并和并/串转换的功能模块，通常被称为 **SerDes**，即 serializer/deserializer。

### 同步与异步通信

**同步通信**是指发送方与接收方使用一个一致的参考时钟信号来同步波特率。当距离较短、时钟信号可以较稳定可靠地传输时，这个时钟信号由发送方提供，另外开一根线来传输到接收方（如 I2C 和 SPI 协议）；有时也可以由收发两端独立产生，并用特定机制保持波特率一致（如 USB 协议）。这里使用时钟来同步数据率的做法，和并行通信中的做法是一致的，所以<u>从某种意义上说，并行通信都是同步通信</u>。

{{< figure src="/post/serial-communication/synchronous-communication.svg" title="同步通信示意图" >}}

**异步通信**的方式下，发送方与接收方没有时钟来同步数据率，只能约定一个大致的数据率，当发送方要开始发送数据时，会发送一个特殊的开始符号，然后开始发送待传输的符号；传输完成后，还要发送一个停止符号。由于没有统一的时钟来规定，发送方和接收方遵照的数据率可能会有误差，这种误差积累之后，可能会导致误码，因此异步通信通常还会有一些同步符号。

{{< figure src="/post/serial-communication/asynchronous-communication.svg" title="异步通信示意图" >}}

异步通信的每次传输需要许多额外信息，并且很容易产生误差，因此常用在对速度要求不高的场合。例如，键盘在不使用时传输信号，而当某个键被按下时，才会传输一串码流；而按键被按下的时间也不是固定的，因此键盘与计算机的通信特别适合使用异步通信。而同步通信，则更适合高速传输的场合。

### 串行信号的编码

数字信号在使用串行传输时，通常要用特定的编码方式。原因有三：

1. 高速串行传输的接收端往往要使用 CDR 技术来恢复时钟（后文详细讲）。如果信号的跳变沿太少（过多连 0 或连 1），会难以 CDR。
2. 如果数据的重复性太强，在高数据率下，会产生电磁干扰。
3. 如果数据的 0 或 1 中的一个比另一个更多，电路中耦合电容的充电与放电不均匀，会产生 DDJ（data dependent jitter，是一种 jitter，会限制数据率的提高）。

这一张图总结较为常见的一些数字信号编码方式。

{{< figure src="/post/serial-communication/encoding-techniques.png" title="常见的编码方式" >}}

除了这些方式之外，常用在高速串行通信中的编码方式有：

#### 8B/10B 编码

8B/10B 编码将 8-bit 长度的字，编码成 10-bit 的符号，从而达到电平平衡，并且提供足够的电平变化，方便时钟恢复。使用 8B/10B 编码的技术有千兆以太网、Aurora、光纤通道、USB 3.0，等等。

#### 64B/66B 编码

64B/66B 编码与 8B/10B 编码类似，但是它的编码开销更小。使用它的技术有万兆以太网、十万兆以太网、Aurora、SATA 3.2、USB 3.1 Gen2、DisplayPort 2.0 等等。

## 两种简单的串行通信规范

### UART[^1][^6]

UART 的全称为 universal asynchronous receiver-transmitter，是一种通用的异步串行通信设备，被集成在微处理器周边，用于计算机的串行通信。如今，UART 的功能已经基本被 USB 取代了，但是仍然可以在一些地方，例如单片机上，找到它的身影。 UART 的原理比较简单，框图如下所示：

{{< figure src="/post/serial-communication/UART_block_diagram.svg" title="UART" >}}

它传输数据的模式就是最典型的异步传输模式。当不传输数据时，输出高电平；开始传输数据时，输出一个低电平，标志着传输的开始；随后输出 5 到 9 个数据比特，具体数量取决于采用的编码集；如果使用奇偶校验位，那就再传输它；最后再保持 1 到 2 个比特的高电平，标志着传输的结束。

{{< figure src="/post/serial-communication/UART_timing_diagram.svg" title="UART 的异步传输" >}}

只有收发端的 UART 芯片设置成相同的数据率、字符长度、奇偶校验和停止位，才有可能正常通信。

UART 定义了一种标准的异步串行通信方式，而且是全双工的。它可以说是定义了串行通信的协议，以及接口（只有 TX 和 RX 两个端口），但是没有定义插头标准（一般使用 RS232 标准所定义的插头），并且是一种点到点的传输方式，不是一种总线标准。

### SPI[^7]

SPI 全称 serial peripheral interface，是一种**同步的**、**全双工的**、**master-slave 架构**的串行通信接口标准，常用于短距离通信，例如芯片之间的互连。它一共只有四根线，所以又被称作 four-wire serial bus。它的四根线的作用分别为：

- SCLK：Serial clock，串行时钟。
- MOSI：Master out slave in，从 master 发出的数据。
- MISO：Master in slave out，从 slave 发出的数据。
- SS：Slave select，slave 选择信号。

只要一个 slave 被选中，master 和 slave 之间就可以用 MOSI 和 MISO 两根线进行全双工的通信了，这个通信使用 SCLK 传输的时钟来同步。下面两张图展示了使用 SPI 接口的两种工作模式：普通模式和菊花链模式。

{{< figure src="/post/serial-communication/SPI_three_slaves.svg" title="普通模式" >}}

{{< figure src="/post/serial-communication/SPI_three_slaves_daisy_chained.svg" title="菊花链模式" >}}

SPI 定义了一种芯片级的总线连接标准，没有定义插头（因为芯片互连不用插头），也没有定义通讯协议。

## MGT 相关技术[^8]

MGT 的全称为 multi-gigabit transceiver，是一种能够以超过 1 Gbps 的速度收发数据的 SerDes。收发如此高速的数据，使得 MGT 涉及到许多高深的技术，这里翻译、列举一下这些技术。

### 差分信号

MGT 通常都使用差分信号来传输数据，从而增强对电磁干扰和串扰的抵抗力。

### MCML

即 MOS current-mode logic，是一种基于差分的数字逻辑，它的 swing 很低，所以功耗非常低。

### Emphasis

由于高速信号的频带非常宽，而一般的信道特性都是低通滤波器，高频部分的衰减会造成 ISI。因此需要在发送端强化信号的高频部分，使其在通过低通信道后保持正常的频谱。

### Receive Equalization

接上条，也可以在接收端强化接收到的信号的高频部分。这样做的缺点是信噪比比较差。

### 终端阻抗匹配

频率很高的情况下，传输信号的通道必须用分布参数模型来解释，传输线理论适用。此时，为了避免反射，保证信号完整性，MGT 设计时，就应该能够匹配传输线的阻抗。对于差分传输来说，匹配阻抗一般是 100Ω。

### PLL

为了串行化高速信号，串行时钟的频率一般会是并行时钟的好几倍，大部分 MGT 会使用 PLL（锁相环）来对时钟进行倍频。

### CDR

同步串行传输需要一根额外的时钟线来传输时钟，但是在数据率非常高的时候，这样做并不现实，因为线长稍有一点差异，就会引起非常严重的 skew。因此，高速传输通常只用一根线来传输数据，并且使用 CDR（clock data recovery）技术来从数据中恢复出时钟，再来解析数据。

### 编码/解码

[串行信号的编码](#串行信号的编码) 已经说清楚了对高速串行信号进行编码的必要性。常用的编码方式有 8B/10B、64B/66B、64B/67B、128B/130B、PAM 等等。

### 误码检测

大部分系统都需要误码检测机制。有的系统使用基于编码的误码检测，有的系统使用 CRC 校验。

## 参考资料

[^1]: 周荷琴，《微型计算机原理与接口技术》，第 6 版，中国科学技术大学出版社。[在京东上购买](https://item.jd.com/12496635.html)
[^2]: [Serial communication - Wikipedia](https://en.wikipedia.org/wiki/Serial_communication)
[^3]: [Advantages And Disadvantages Of Serial And Parallel Data Transmission - energylifegiant](http://energylifegiant.eklablog.com/advantages-and-disadvantages-of-serial-and-parallel-data-transmission-a181790032)
[^4]: [Synchronous vs. Asynchronous](http://et.engr.iupui.edu/~skoskie/ECE362/lecture_notes/LNB25_html/text12.html)
[^5]: [Difference Between Synchronous and Asynchronous Transmission (with Comparison Chart) - Tech Differences](https://techdifferences.com/difference-between-synchronous-and-asynchronous-transmission.html)
[^6]: [Universal asynchronous receiver-transmitter - Wikipedia](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter)
[^7]: [Serial Peripheral Interface - Wikipedia](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface)
[^8]: [Multi-gigabit transceiver - Wikipedia](https://en.wikipedia.org/wiki/Multi-gigabit_transceiver)