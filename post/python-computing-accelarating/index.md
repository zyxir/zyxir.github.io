
这篇文章介绍我所知的一些提升 Python 的科学计算性能的办法。

<!--more-->

## 一、多使用 NumPy 库

Python 的列表用于处理数值计算十分低效，而 NumPy 库提供的各种运算函数，由于底层使用 C++ 实现，并且使用了一些向量与矩阵运算的加速技巧，运算速度比列表要快上不少。下面用一个例子来体现它们的速度差。

这个 Python 脚本制造了一个长度为一百万的纯数字列表，将其所有的元素加 2，然后计算出之后的最小值，并且输出结果和运算所使用的总时间。

```python
import time

# Start ticking.

start_time = time.time()

# Define the array, and print its length.

array = list(range(1000000))
print('The length of the array is ', len(array))

# Increase each element by 2.

for i in range(len(array)):
    array[i] += 2

# Find the minimum and print it.

min = min(array)
print('The minimum is ', min)

# Print the elapsed time.

end_time = time.time()
print('Elapsed time is ', end_time - start_time)
```

输出的结果为：

```
The length of the array is  1000000
The minimum is  2
Elapsed time is  0.12588071823120117
```

这说明它的运行大概要花费 0.12 秒。那么，如果稍微修改一下它，使用 NumPy 库的数组来替换原脚本中的列表，更新后的脚本如下（注意高亮的行）：

```python {hl_lines=[2,10,15,19]}
import time
import numpy as np

# Start ticking.

start_time = time.time()

# Define the array, and print its length.

array = np.arange(1000000)
print('The length of the array is ', len(array))

# Increase each element by 2.

array += 2

# Find the minimum and print it.

min = np.min(array)
print('The minimum is ', min)

# Print the elapsed time.

end_time = time.time()
print('Elapsed time is ', end_time - start_time)
```

运行这个脚本，输出为：

```
The length of the array is  1000000
The minimum is  2
Elapsed time is  0.002527952194213867
```

速度直接从 0.12 秒，提升到了 0.002 秒，对比已经非常明显了。虽然没人会直接去用 Python 原生列表去做科学计算，可是就我自己而言，有时候有些简单的小功能，为了图省事，可能就直接用列表去实现了。列表的低效叠加起来，可能就会给程序的运行效率带来许多负面影响。

## 二、操作系统很重要

在我自己**实际做项目**的过程中，我注意到，在 Windows 和 Linux 上，Python 的计算效率有非常大的区别。我在**同一台电脑**（八代十二核 i7 处理器，16 GB 内存，集成显卡）上，安装了 Windows 10 和 Arch Linux 操作系统，但是在它们上分别运行一样的代码，速度能相差好几倍（大概是不到十倍，自己的粗略估计，没有精确计时对比）。因此，我现在**只在 Linux 上跑 Python 计算了**。但是，**这只是个人经验**。

我也在搜索引擎上进行了一些调查，得到的结论是：**Python 在 Linux 上的确比 Windows 要快很多，而且是 much faster，快得相当多**（当然，许多网上的说法也只是基于经验），但是**NumPy 在 Linux 上的性能并没有明显高于 Windows**（参考[官方的性能测试](https://numpy.org/doc/stable/reference/random/performance.html)）。

我的 Arch Linux 是自己从最基本的命令行界面一点点“组装”起来的，哪怕和同为 Linux 的 Ubuntu 相比，它都要轻量许多，因此自然成了我跑计算程序的首选操作系统。

我没有 mac 电脑，所以没有测试它的性能。有机会的话一定也要测试一下。

## 三、并行计算、GPU 计算等

这一点感谢实验室的 ZK 师兄给我提供思路。可惜我对于这两点的理解都不够多，这里只能提供少许信息。

并行计算，可以使用 [Python 内置的 multiprocessing 包](https://docs.python.org/3/library/multiprocessing.html) 来实现。这里借用[这个网站](https://pymotw.com/2/multiprocessing/basics.html)上的例子来展示它的基本用法：

```python
import multiprocessing

def worker():
    """worker function"""
    print 'Worker'
    return

if __name__ == '__main__':
    jobs = []
    for i in range(5):
        p = multiprocessing.Process(target=worker)
        jobs.append(p)
        p.start()
```

```
$ python multiprocessing_simple.py

Worker
Worker
Worker
Worker
Worker
```

使用 GPU 做计算，是现在非常火的方向。这里仅提供一篇 [NVIDIA 提供的在 Python 中使用 GPU 加速计算的教程](https://developer.nvidia.com/how-to-cuda-python)。