# 如何在 Matlab 中启用 UTF-8 编码


今天遇到了 Matlab 的中文编码问题，经过查询之后发现应该有许多人遇到了这个问题，可是网上能查到的相关文章只是抛出莫名其妙的解决方案，几乎都没有任何分析。在此我详细地分析了这个问题，以及其解决方案。

<!--more-->

大概两三周前，我在中文 Windows 10 上的 Matlab 中打开一份之前在 Linux 上写的 Matlab 代码，发现其中的中文居然全部成了乱码，那时我以为是 Windows 和 Linux 操作系统之间字符编码的问题，就没太在意。今天，我发现我在 Visual Studio Code 中写 Matlab 代码时，`mlint.exe` 程序（即 Matlab 的自动纠错程序）给出的 Warning 信息全都是乱码；而用 Matlab 打开同一份代码，代码中的中文也变成了乱码。此时我就意识到，这个问题值得探索了。

## 定位问题

同一个文件在同一个操作系统上的不同程序中显示出不同的面貌，说明一定是字符编码出了问题。我心里的第一个想法就是「现在不是全宇宙通用 UTF-8 编码吗？难不成是 Matlab 还在使用其它编码方式？」。在确认了 VS Code 使用的是 UTF-8 之后（VS Code 的界面右下角会显示使用的编码方式。毕竟它是文本编辑器嘛！），我就明白了，一定是 Matlab 在用什么小众的编码方式。

网上很容易搜到，在 Matlab 的命令行中可以使用 `feature('locale')` 命令来查看当前的编码方式。我的结果如下：

{{< image src="https://i.loli.net/2020/07/21/isnvfjDq4X3T6oh.png" alt="feature('locale') 的输出结果" caption="feature('locale') 的输出结果" >}}

可以看到，默认的编码方式，果然基本都是 GBK，真的是害人。我用的已经是 Matlab 的 R2019a 版本了，居然还默认使用 GBK 编码，这是多么顽固？

## 为什么应该使用 UTF-8

由于编码问题产生的麻烦和乱码，相信你也遇到过不少。要解决这个问题的一个重要方式，就是统一地球上的所有编码方式，让尽可能所有的程序都使用某一种编码方式。这就是 Unicode 和 UTF 编码诞生的理由。Unicode 字符集几乎包含了所有可以使用计算机来显示的字符，而 UTF 系列编码则是对这一字符集的编码。UTF 编码有 UTF-8，UTF-16，UTF-32 等等，而 UTF-8 是其中最通用、占用存储空间相对较少的一种。

要让乱码不再产生，要么就都让 Matlab 和 VS Code 都使用 UTF-8，要么就让 Matlab 和 VS Code 都使用 GBK。但是 GBK 是专用于中文的编码方式，并不通用，让程序强制使用 GBK，可能会解决一个问题，却带来更多问题。从长远看，"everything UTF-8" 才是好的解决方案。

## 怎么让 Matlab 使用 UTF-8

如果你搜索 Matlab 的文档，文档会告诉你，使用 `slCharacterEncoding()` 函数来设置字符编码，那就试试它。

如果不带参数地运行这一函数，会显示

{{< image src="https://i.loli.net/2020/07/21/cSfHXaWwQipDsBR.png" alt="slCharacterEncoding() 的输出结果" caption="输出结果" >}}

果然是 GBK。那我把它改掉呢？但是事实是，**改不掉**。即使输入 `slCharacterEncoding('UTF-8')` 之后，在输入 `slCharacterEncoding()` 的结果已经变成了 'UTF-8'，文件还是会乱码。哪怕重启 Matlab，也会乱码。所以说，官方文档没用。

真正解决问题的是网上的另外一些文章，虽然它们都没说为什么，但是我大概能理解为什么那么做。首先，打开 Matlab 安装目录下的 `bin` 目录，对我来说，这个目录的路径是

```
D:\Program Files\MATLAB\R2019a\bin
```

在此目录下，会有两个文件，`lcdata.xml` 和 `lcdata_utf8.xml`。其中 `lcdata.xml` 决定了 Matlab 的编码方式，而 `lcdata_utf8.xml` 似乎是一个不生效，但是专门用来替代 `lcdata.xml` 的文件。

首先，把 `lcdata.xml` 重命名为 `lcdata_old.xml`，即把它备份一份，防止以后要用到。

然后，把 `lcdata_utf8.xml` 复制一份，就叫做 `lcdata.xml`，并且打开它，作出一些改动。首先要删掉关于 GBK 的这一部分：

```xml
<encoding name="GBK'>
    <encoding_alias name="936"/>
</encoding>
```

然后，改动一下关于 UTF-8 的部分，高亮的那一行是新加入的内容：

```xml {hl_lines=[3]}
<encoding name="UTF-8">
    <encoding_alias name="utf8"/>
    <encoding_alias name="GBK"/> 
</encoding>
```

这么做的目的就是，将 GBK 编码从一种独立的编码方式，改成了 UTF-8 的一个别名，从而欺骗 Matlab，让它使用 UTF-8。**虽然很不优雅，但是真的有用。**

重启 Matlab 之后，乱码的中文应该就不再乱码了。

## 一点吐槽

在写下这篇文章之后，关于 Matlab 编码问题的折腾还没有结束，因为当我使用 VS Code 编写 Matlab 程序时，`mlint.exe` 程序仍然输出乱码，错误信息也没法用。

不可能选择屈从于 GBK 编码，否则哪天我在 Linux 上看 Matlab 程序又没法看了。GBK 绝对是一个已经过时了的、没有前途的、故步自封的编码选择。

我现在想尝试的是，使用第三方编辑器（例如 VS Code，我更看好 Emacs）来编写 Matlab 源文件，存储为 UTF-8 格式，使用 `mlint.exe` 进行代码检查，并且使用集成的 Matlab Terminal 进行即时的运行。这就做好比是把 VS Code（或者 Emacs）变成了 Matlab 的第三方客户端。

既然有 Matlab，为什么还要用第三方客户端？还不是因为 Matlab 太令人不满意了。一个付费商用科学计算软件，不能改字体和主题，编码方式写死在代码里，编程规则各种特立独行，真的让人很难喜欢起来。
