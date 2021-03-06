
今日尝试连接我新买的 HUAWEI FreeLace Pro（蓝牙耳机）与我的 Arch Linux 生产力机器，发现这款蓝牙耳机与 Linux 的配合还不是很好，Linux 居然无法将其识别为音频输入/输出设备。

<!--more-->

## 精心装修的家，不能住

我是真的挺享受定制自己的 Linux 操作系统的感觉的。大约三周前，我发现 Windows 上的一个 Python 计算程序运行得特别慢，就给机器装了 Arch Linux。我发现在 Linux 上，程序的运行速度提升了许多倍。这时，我便禁不住对着 Arch Wiki 又折腾了几天 Linux，细细地微调、配置各个部分，还边配置边做了笔记。

{{< figure src="/ca9c3c5c-a77d-4830-9401-d06c7cf826f0.jpg" title="我的笔记" >}}

这种具有一定挑战性、又能够不断获得反馈的 configuring，带来的快乐是无与伦比的：

- 我苦读 fontconfig 配置的格式，并且参考了网上几个人的经验之后，写出自己的字体配置，成功让屏幕上的字体显示舒服了许多。
- 我对着文档一点一点小心地配置 tlp，达到了极致的续航提升。
- 我一点一点地配置 fcitx 和 rime，让电脑的输入提升了许多。

这就像一点一点地装修自己的房子，精心设计、修缮每一个部分，并且把它变成了金光闪闪的艺术品。然而，现在这个房子存在的问题是，虽然我能够着手装修的地方已经比较完美了，却存在着许多我无法改变，也无法忍受的地方。这里就谈我遇到的三个问题：

1. 当我想从手机传输文件到电脑时，我有时会使用蓝牙。但是手机居然会把电脑识别为音频输出设备，导致蓝牙耳机中的音乐会断开；同时，电脑会把手机识别为外接的 4G 网卡。我当然可以去论坛里和大家讨论这个问题，但是它有一定复现难度，也难以被马上解决。在 Windows 上无此问题。**困扰程度：★★**。
2. 我专门买的 HUAWEI FreeLace Pro 蓝牙耳机无法和电脑一起使用。HUAWEI FreeLace Pro 是目前市面上最符合我的需求的耳机，是我调研了很久之后才决定买的。它无法和我的生产力设备共同使用的话，会让我「一个随身耳机满足所有需求」的目标彻底无效。虽然电脑不是主要的听音乐设备，但是偶尔有用电脑看视频的需求时，我不能接受只能外放。在 Windows 上无此问题。**困扰程度：★★★★★**。
3. 我专门买的、能为笔记本电脑充电的 45W 小米移动电源，为电脑充电时，Linux 会变得非常卡，鼠标难以移动，并且会出许多致命的 BUG。我不想在此详细描述这些 BUG 了，它们中的任何一个都非常致命。我选购这款充电宝的主要目的，就是为电脑充电，而居然产生了这样的结果，实在是难以让人接受。在 Windows 上无此问题。**困扰程度：★★★**。

精心装修的房子的确好，精致的装饰，舒适的通风系统，美丽的周围景观，让人心旷神怡。但是，它的屋顶漏水，它的车库停不进自己的爱车，它的天线甚至会引雷、产生危险，这还如何居住？

我的确装了 Linux 和 Windows 的双系统，可是 Linux 此时仍然是我的核心生产力系统。如果别人发我一个 Word 文档，我可以切换到 Windows 下去打开这个文档；但是我不能忍受我的核心生产力系统只能外放、不能连接我心爱的耳机。

我不知道将来还会遇到多少 BUG，但是生产力系统真的容不得这么多不确定性。我还是换回 Windows 吧！

## Alternative Solution

虽然为了稳定性和适配性，我必须放弃 Linux 的可玩性（鱼），选择 Windows（熊掌）。但是，也许存在一些折中的解决方案。

树莓派（Raspberry Pi）是一个信用卡大小的 Linux 电脑，搭载的是 ARM 架构的处理器，非常便携。如果将其配合电池或者移动电源使用，就可以随身携带，成为一个随身 Linux 机器与服务器了。可以预见的玩法有：

1. 配置一个脚本，让树莓派连上有线网时，自动开热点。这样，配合电池，它就成了一个**便携的、不用通电的随身路由器**。进一步地，可以给树莓派配置透明代理，实现更高级的网络功能（这些功能不方便在网络上讨论，不详述了）。
2. 用手机开热点，树莓派和生产力笔记本电脑同时连接这一热点。笔记本电脑使用 SSH 连接到树莓派，就可以方便地**随时随地配置 Linux** 了。
3. 给电脑加装 4G 模块（我的 ThinkPad X395 确实可以加装），电脑连 4G 网并开热点，就可以更方便地实现第 2 条了。
4. 给树莓派安装一张大一点的存储卡，把它当成「网络移动硬盘」使用，进行定期的**文件备份**。

把生产力笔记本电脑当成鱼，把树莓派当成熊掌，或许就能兼得？