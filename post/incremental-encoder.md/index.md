
增量式编码器（incremental encoder）是一种将被测物体的移动转换成电脉冲的设备。

<!--more-->

增量式编码器的原理如图所示。当被测物移动时，会带动转盘旋转（类似绞盘的原理）。当转盘匀速旋转时，就会输出 A 与 B 两路方波信号，并且 A 与 B 有 $\frac{\pi}2$ 的相位差。

{{<figure src="/07fdd030-3659-41df-9854-a924cdd0b20c.gif" title="原理图">}}

{{<figure src="/c0155b92-dc92-454e-8d79-332b82b9b6f4.svg" title="输出的 A 与 B 信号">}}

输出两路信号的目的是让接收端能够判断旋转方向，从而判断运动方向。

有时候，这种编码器会额外输出一路 Z（或称为 index）信号，每转一圈，Z 信号会输出一个脉冲，用于记录圈数。

还有时候，这种编码器还会再输出一路 alarm 状态信号，表示编码器是否遇到了内部错误（例如转速过快，或传感器不正常工作）。

## 参考资料

- [Incremental encoder - Wikipedia](https://en.wikipedia.org/wiki/Incremental_encoder)