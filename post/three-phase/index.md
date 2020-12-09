
{{<figure src="/c185d153-2e47-4f65-bc67-50547145eac3.jfif" width="200">}}

有时候，在机房或是实验室里，我们会看到这种有四个孔的插头，它就是三相电插头了。那么它到底是干什么的呢。

<!--more-->

## 什么是三相电

普通的家庭供电，供电的就是零线和火线两根线，其中零线作为零电位参考，而火线上则是携带了有效值为 220V、频率为 50Hz 的交流电。为了安全考虑，通常还有一根地线，连到大地深处。

而三相电则有四根线，除了提供参考零电位的零线外，还有三根线。这三根线可以认为都是火线，因为它们上面传播的都是交流电，只是彼此之间有一个 $\frac{2\pi}3$ 的相位差。

{{<figure src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/3_phase_AC_waveform.svg/300px-3_phase_AC_waveform.svg.png">}}

三根线的电势可以写作

$$
V_1 = V_P \sin(\theta)\\\\
V_2 = V_P \sin(\theta + \frac23 \pi)\\\\
V_3 = V_P \sin(\theta + \frac43 \pi)
$$

## 230V/400V

许多三相电系统是 230V/400V 系统，意为单相电压有效值为 230V，而两相之间的电压有效值为 400V。为什么是 400V 而不是 460V（230V 的两倍）呢？下面通过计算说明。

对于单独一相电，以及两相电之间的电压，它们的交流有效值（又称为方均根值）分别为

$$
V_{1,\text{RMS}} = \sqrt{\frac{\int_0^{2\pi}V_1^2 d\theta}{2\pi}}\\\\
V_{12,\text{RMS}} = \sqrt{\frac{\int_0^{2\pi}(V_1 - V_2)^2 d\theta}{2\pi}}
$$

把上面的两个积分展开来计算

$$
\begin{align}
\int_0^{2\pi} V_1^2 d\theta &= \int_0^{2\pi} V_P^2 \sin^2(\theta) d\theta\\\\
&= \frac12 V_P^2 \int_0^{2\pi} \left[ 1 - \cos(2\theta) \right] d\theta\\\\
&= \pi V_P^2
\end{align}
$$
$$
\begin{align}
\int_0^{2\pi} (V_1 - V_2)^2 d\theta &= \int_0^{2\pi} V_P^2 \left[\sin(\theta) - \sin(\theta + \frac23 \pi)\right]^2 d\theta\\\\
&= V_P^2 \int_0^{2\pi} \left( \sin\theta - \sin\theta\cos\frac43\pi - \cos\theta\sin\frac43\theta \right)^2 d\theta\\\\
&= V_P^2 \int_0^{2\pi} \left( \frac32\sin\theta - \frac{\sqrt{3}}2\cos\theta \right)^2 d\theta\\\\
&= V_P^2 \int_0^{2\pi} \left( \frac34 + \frac32\sin^2\theta - \frac{3\sqrt{3}}2\sin\theta\cos\theta \right) d\theta\\\\
&= V_P^2 \int_0^{2\pi} \left( \frac32 - \frac34\cos 2\theta - \frac{3\sqrt{3}}4\sin 2\theta \right) d\theta\\\\
&= 3\pi V_P^2
\end{align}
$$

于是有

$$
V_{1,\text{RMS}} = \frac{\sqrt2}2 V_P \quad V_{12,\text{RMS}} = \frac{\sqrt6}2 V_P
$$

可以看到，两相之间的电压有效值是单相有效值的 $\sqrt3$ 倍，因此是 230V 与 400V。

## 三相电有什么优点

三相电会存在，正是由于它相对于单相电来说有一些难以取代的优点。

### 恒定的功率供给

对于一个阻性元件 $R$，三相电对其提供的瞬时功率为

$$
\begin{align}
p &= \frac{V_1^2 + V_2^2 + V_3^2}R\\\\
&= \frac{V_P^2}R \left[ \sin^2\theta + \sin^2\left( \theta + \frac23\pi \right) + \sin^2\left( \theta + \frac43\pi \right) \right]\\\\
&= \frac{3 V_P^2}{2 R}
\end{align}
$$

这是一个恒定的值，而普通单相电的功率是会随着时间不断变化的。（虽然我暂时还想不通怎么用三个电压同时给一个电阻供电，但是维基百科上就是这么写的）

### 零线无电流

与上面一样的条件，可以计算出零线上的瞬时电流为

$$
\begin{align}
i &= \frac{V_1 + V_2 + V_3}R\\\\
&= \frac{V_P}R \left[ \sin\theta + \sin\left( \theta + \frac23\pi \right) + \sin\left( \theta + \frac43\pi \right) \right]\\\\
&= 0
\end{align}
$$

相比之下，传统的单相电，电流是通过零线返回的。不过，只有当三根线的负载完全相同时（例如，三相发动机）才会有零电流。

### 电源与电能传输方面的优点

三相电发电机构造简单，传输同样的功率时，三相电比单相电能更节约有色金属。