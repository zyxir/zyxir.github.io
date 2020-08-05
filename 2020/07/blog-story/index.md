# 博客之路


距离我第一次创立作为独立网站的个人博客，已经过去了两个年头，但是直到今天，我还在折腾博客的方式，比如平台、管理方式、外观、布局等，而不是内容本身。

<!--more-->

固然，博客文章的内容，其实才应该是博客最重要的部分，但是这似乎一直被我放到了第二的优先级。反倒是一些不太重要的地方，被我来回折腾了两年。这两年间，我尝试过各种不同的东西：在平台选取上，我尝试过第三方网站（CSDN、简书等），静态网站生成器（Jekyll、Hexo、Hugo 等），动态网站框架（WordPress.com 与 WordPress.org）；在主题风格上，我选择过极简主义的主题，也尝试过 UI 比较复杂的主题；在托管方式上，我选择过用第三方网站托管（Github 与 WordPress.com），也选择过使用自己租用的云服务器；甚至在站点的主要语言上，我都尝试过英语、简体中文与繁体中文三种。我就是个这么喜欢折腾的人，但是这些折腾也并非完全是浪费时间——除了让我更加清晰地认识到自己适合什么样的方式之外，也算是带来了一段美好的回忆吧！

## 这个博客的折腾之路

用一个 todo list 来记录我折腾这个博客的过程吧！

- [x] `2020-07-21` 完成基本构建，设立好主题，写好第一篇文章。
- [x] `2020-07-21` 完善与 Github 协同工作的工作流（三个 repo 分别包含我修改的主题，源代码与生成的网页）。
- [ ] 转移有价值的旧博客文章到这个博客。
- [x] `2020-08-06` 完善各大平台的流量分析。
- [ ] 完善 SEO。
- [x] `2020=08-06` 完善评论系统。
- [ ] ~~创建 Gitee Pages 镜像。~~
- [x] 完成关于我、友情链接之类的辅助性页面。
- [ ] **网站正式上线**，取代原本的 [blog.ericzhuochen.com](https://blog.ericzhuochen.com/)。
- [ ] 加入邮件订阅功能。
- [ ] 在一些平台中推广我的新博客。

## 什么样的博客最适合我？

首先，两年前我就已经得出了，第三方网站（CSDN、简书等）不适合我的结论。

在你看到的这个博客之前，我最后一个博客使用的是 WordPress.org，一个强大的动态网站框架（事实上，在我写这篇文章的时候，我的网站还是 WordPress.org 那个，只有当这个静态博客比较完善了，我才会用它替代原来的博客）；而在 WordPress.org 之前，我使用的是 Hugo 生成的静态博客。在使用 WordPress.org 之后，我基本就断定，我需要纠结的只有静态或是动态网站了。

刚开始使用 WordPress.org 的时候，这个相对庞大的系统的强大的功能、高颜值的界面、所见即所得的写作方式和丰富的插件系统就深深吸引了我。相比 Hugo 上的主题，WordPress.org 上的主题是那么功能丰富、设计美观；相比 Hugo 的某些主题，WordPress.org 上的主题能提供的定制选项居然要多那么多；而且 WordPress.org 提供的插件功能如此强大，尤其是简繁转换插件，实在是深得我心。那段时间（大概是 2020 年 6 月），我确实投入了很多精力，使用 WordPress 制作了一个比我以往的所有博客都要更美观、优雅的博客。

但是很快，我就发现了 WordPress.org 的弊端了。

- 首先，它只能运行在自己的服务器上，这实际上对你自己的运维能力就提出了要求；网站小的时候还好，网站大起来，就可能会遭到攻击；除了攻击之外，你自己的服务器的访问（哪怕加了 CDN）可能不一定非常顺畅，而且 SEO 也会比使用大的第三方平台时要更难做一些。

- 其次，WordPress.org 的很多功能的确不行，哪怕装了插件也不行。例如它的代码块功能就差强人意，我每写一个代码块，都需要在一个有几十个选项的下拉列表中选择我这个代码块使用的编程语言。直接让我离开它的，是它的公式功能，没有一个插件能让我很方便地写、预览行内与行间公式的，这让我这个理工科学生怎么使用？这让我觉得 WordPress.org 其实并不适合用来做技术博客。

- 第三，在 WordPress.org 的框架下，想实现一些简单的小功能很麻烦，而在 Hugo 中，想实现小功能，只需要稍微改一改主题的代码就行了。

- 最后，我发现 WordPress.org 真的很费时间，因为它的一切都是在一个**在线的图形界面上**的。相比折腾静态博客来说，WordPress.org 真的多占用了我的太多时间。

于是，我重新回到了 Hugo，并且这次，我打算这样提高我的写作体验：

- 不管使用哪个平台，选择一个好的主题，都是高效工作的第一步。WordPress.org 上，好的主题几乎都是付费的，要想选到一个免费又好的主题很困难。但是 Hugo 上没有付费主题。经过一段时间的筛选，我选择了目前使用的 [LoveIt](https://github.com/dillonzq/LoveIt) 主题，它的功能非常强大，有非常多的参数可调，可以达到和使用 WordPress.org 相近的私人定制体验。
- 为了能够方便地添加自己的小功能，我把这个主题 fork 了一份：[zyxir/LoveIt](https://github.com/zyxir/LoveIt)，并且以后都使用自己的版本。同时，我把博客的源代码放在 [zyxir/blog](https://github.com/zyxir/blog)，借助 Hugo 的强大功能，我可以在博客中加入“查看源代码”按钮，读者就可以看到文章在 Github 上的源代码了。
- 善用 Hugo 的 shortcodes 以及主题内置的 shortcodes，达成强大的功能。我以前居然都不会用它们！（因为我以前坚持用 Emacs 的 Org-mode 写博客，它带来了额外的复杂度，让我难以再去学习 Hugo 的这些小玩意儿）
- 使用 Typora 来写 Markdown 博客，获得 WYSIWYG 的体验。Typora 真的是一个神一般的软件，我哪怕在商业软件中也很少见到如此这种把用户每一个需求都考虑得这么清楚、使用体验如此完美的软件了。
- 使用 Github 和 Gitee 来托管我的博客，不仅免去维护服务器的额外麻烦，而且获得国内和国外均不菲的访问速度。

## 博客功能测试

还是例行地，要测试一下这个博客的表现力。我所使用的主题 LoveIt 在 Hugo 的基础上，加入了一些自己的 shortcodes，同时，我也把它 fork 了一份，做了一些自己的修改（是的，我已经有这个能力了！现在 html 和 css 我都能看懂一些了）。所有的网页与文章，均是使用 Markdown 写成。

### 基本格式化

首先是一些基本的格式化能力，比如列表与 inline markup：

```
- 这是**加粗过的**与_斜体的_文字，这是 Markdown 的基本功能
- 这是~~带有删除线的~~文字，这是 Github Flavored Markdown 的功能
- 这是<u>带有下划线</u>的、<kbd>像快捷键的</kbd>文字，这是用 HTML 标签实现的
```

显示效果为：

- 这是**加粗过的**与_斜体的_文字，这是 Markdown 的基本功能
- 这是~~带有删除线的~~文字，这是 Github Flavored Markdown 的功能
- 这是<u>带有下划线</u>的、<kbd>像快捷键的</kbd>文字，这是用 HTML 标签实现的

同时还有有序列表与代办清单：

1. 这是有序的
2. 你看，是有序的吧

代办清单真的是很方便的功能了：

- [x] 事项1
- [x] 事项2
- [ ] 事项3

可以放入一条分割线：

---

也可以插入一条引用内容：

> 这是被引用的内容。

全部 Markdown 标准可以参照[Hugo Markdown Reference](https://www.markdownguide.org/tools/hugo/)。

### 表格与 Shortcodes

下面用表格的形式展示一下 shortcodes

Shortcodes 是 Hugo 用来拓展内容形式的重要方式。在 LoveIt 主题的文档中，介绍了几种内置的 shortcodes：

| Shortcode | 作用 |
| -- | -- |
| figure | 插入图片 |
| gist | 嵌入 gist |
| highlight | 插入代码块 |
| instagram | 嵌入 Instagram |
| param | 使用此页面的某个 param |
| ref/relref | |
| tweet | 嵌入 Twitter 推文 |
| vimeo | 嵌入 Vimeo 视频 |
| youtube | 嵌入 Youtube 视频 |

以及 LoveIt 主题引入的新 shortcodes:

| Shortcode | 作用 |
| -- | -- |
| style | 实现特定 HTML 样式 |
| link | 链接，但是能够在代码块中使用 |
| image | 更强大的图片插入方式 |
| admonition | 不知道怎么翻译，类似“警告”或者“横幅”吧 |
| mermaid | 标准化的流程图、时序图、甘特图等等 |
| echarts | 交互式的数据可视化库 |
| mapbox | 交互式地图 |
| music | 内嵌音乐播放器 |
| bilibili | 嵌入 Bilibili 内容 |
| typeit | 打字动画 |
| script | 插入 Javascript 脚本 |

这些 shortcode 的使用方式在文档中描述得很清楚了，几个经常要用到的在后文展示。

### 图片

一个绘声绘色的博客离不开丰富多彩的图片。首先是最基本的图片显示方式：用 Markdown 语法实现：

``` Markdown
![头像](/img/profile_picture.png)
```

效果为

![头像](/img/profile_picture.png)

同时，Hugo 的 figure shortcode 能够实现更棒的效果，比如包含字幕、更合适的大小与位置。同一张图片用 figure 来显示：

```Markdown
{{</* figure src="/img/profile_picture.png" alt="我的头像" title="我的头像" */>}}
```

{{< figure src="/img/profile_picture.png" alt="我的头像" title="我的头像" >}}

LoveIt 主题提供了一个更棒的 shortcode，叫做 image，甚至能支持 [lightgallery.js](https://github.com/sachinchoolur/lightgallery.js)。

```Markdown
{{</* figure src="/img/profile_picture.png" alt="我的头像" title="我的头像" */>}}
```

{{< figure src="/img/profile_picture.png" alt="我的头像" title="我的头像" >}}

### 公式与代码

LoveIt 主题默认使用 KaTeX 来显示公式，我却发现它连多行的矩阵都显示不出来，于是手动修改主题，使用 MathJax 了。

输入以下公式（由于 Markdown 引擎的原因，所有换行符需要输入四个反斜杠）：

```Markdown
$$
\frac14
\begin{bmatrix}
U_1 & U_2 & \ldots & U_n
\end{bmatrix}
\begin{bmatrix}
C_{11} & C_{12} & \ldots & C_{1n}\\\\
C_{21} & C_{22} & \ldots & C_{2n}\\\\
\vdots & \vdots & \ddots & \vdots\\\\
C_{n1} & C_{n2} & \ldots & C_{nn}
\end{bmatrix}
\begin{bmatrix}
U_1\\\\
U_2\\\\
\vdots\\\\
U_n
\end{bmatrix}
= W_e = \frac12 \int_{\Omega_\text{conductors}} \varepsilon \vec{E}^2 d\Omega
$$
```

这是公式显示效果：

$$
\frac14
\begin{bmatrix}
U_1 & U_2 & \ldots & U_n
\end{bmatrix}
\begin{bmatrix}
C_{11} & C_{12} & \ldots & C_{1n}\\\\
C_{21} & C_{22} & \ldots & C_{2n}\\\\
\vdots & \vdots & \ddots & \vdots\\\\
C_{n1} & C_{n2} & \ldots & C_{nn}
\end{bmatrix}
\begin{bmatrix}
U_1\\\\
U_2\\\\
\vdots\\\\
U_n
\end{bmatrix}
= W_e = \frac12 \int_{\Omega_\text{conductors}} \varepsilon \vec{E}^2 d\Omega
$$

同样地，代码块也是很容易的功能。我一般直接使用 Markdown 内置的功能来显示。在 Markdown 里写这些（其中还指明了行号从 20 开始，以及把第2、第4-5行高亮）：

{{< highlight Markdown >}}
```Python {hl_lines=2,"4-5", linenostart=20}
a = []
a[0] = 1
a[1] = 1
i = 2
while i < 100:
    a[i] = a[i-1] + a[i-2]
print(a[100])
```
{{< / highlight >}}

```Python {hl_lines=[2,"4-5"], linenostart=20}
a = []
a[0] = 1
a[1] = 1
i = 2
while i < 100:
    a[i] = a[i-1] + a[i-2]
print(a[100])
```

### 嵌入内容

用 shortcode 也可以方便地插入 Youtube 视频：

``` Markdown
{{</* youtube sGIm0-dQd8M */>}}
```

{{< youtube sGIm0-dQd8M >}}

（当然，如果你看不到这个 Youtube 视频也不要惊异，毕竟 Youtube 是一个不存在的网站嘛。）

或者是 Bilibili 视频：

``` Markdown
{{</* bilibili BV1Rz411i7bN */>}}
```

{{< bilibili BV1Rz411i7bN >}}

嵌入功能很丰富，也很耐玩。在后面的博文中再慢慢尝试啦！
