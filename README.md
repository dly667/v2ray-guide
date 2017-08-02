# V2Ray 配置指南

## 声明

V2Ray 配置指南（下称本指南）是网友自发的编写的关于 V2Ray 如何配置及使用的指南，与 V2Ray 官方并无任何关系。

## 简介
### 什么是 V2Ray？

> V2Ray 是一个模块化的代理软件包，它的目标是提供常用的代理软件模块，简化网络代理软件的开发。

听起来有点绕口的，是什么意思呢？简单来说，它就是一个代理，可以用来科学上网。至于什么模块化、简化开发那是跟开发者说的，咱吃瓜群众会用 V2Ray 就好了。

V2Ray 用户手册：[https://www.v2ray.com](https://www.v2ray.com)

V2Ray 项目地址：[https://github.com/v2ray/v2ray-core](https://github.com/v2ray/v2ray-core)

V2Ray Telegram 使用开发群链接：[https://t.me/projectv2ray](https://t.me/projectv2ray)

### V2Ray 跟 Shadowsocks 有什么区别？

区别还是有的，Shadowsocks 只是一个简单的代理工具，而 V2Ray 作者将 V2Ray 定位为一个平台，任何开发者都可以利用 V2Ray 提供的模块开发出新的代理软件。

了解 Shadowsocks 历史的同学都知道，Shadowsocks 是 clowwindy 开发的自用的软件，开发的初衷只是为了让自己能够简单高效地科学上网，自己使用了很长一段时间后觉得不错才共享出来的。V2Ray 是 clowwindy 被喝茶之后某个程序员作为抗议开发的，一开始就致力于让大家更好更快的科学上网。

由于出生时的历史背景不同，导致了它们性格特点的差异。

简单来说，Shadowsocks 功能单一，V2Ray 功能强大。听起来似乎有点贬低 Shadowsocks 呢？当然不！换一个角度来看，Shadowsocks 简单好上手，V2Ray 复杂配置多。

### 既然 V2Ray 复杂，为什么要用它？

童鞋，某事物的优点和缺点总是相生相随的，优点是缺点，缺点也是优点。相对来说，V2Ray 有以下优势：

* **更完善的协议:** V2Ray 使用了新的自行研发的 VMess 协议，改进了 Shadowsocks 一些已有的缺点，更难被墙检测到
* **更强大的性能:** 相对于 Shadowsocks，V2Ray 的网络性能更好，具体数据可以看[V2Ray 官方博客](https://www.v2ray.com/blog/2017/speed.html)
* **更丰富的功能:** V2Ray 比起 Shadowsocks 多了许多功能，以下是部分 V2Ray 的功能
    * mKCP: KCP 协议在 V2Ray 上的实现，不必另行安装 kcptun
    * 动态端口：动态改变通信的端口，对抗对长时间大流量端口的限速封锁
    * 路由功能：可以随意设定指定数据包的流向，去广告、反跟踪都可以
    * 传出代理：看名字可能不太好理解，其实差不多可以称之为多重代理。类似于 Tor 的代理
    * 数据包伪装：类似于 Shadowsocks-rss 的混淆，另外对于 mKCP 的数据包也可伪装，伪装常见流量，令识别更困难
    * WebSocket 协议：可以 PaaS 平台搭建V2Ray，通过 WebSocket 代理。也可以通过它使用 CDN（如 cloudflare） 中转，抗封锁效果更好
    * Mux:多路复用，进一步提高科学上网的并发性能

### 哪有十全十美的东西？

少年悟性很高啊！当然没有！目前来说，V2Ray 有下面的缺点：
- 配置复杂
- GUI 客户端不完善
- 产业链不成熟

### 为什么要写这篇文章？

虽然其文档很详细，换个说法就是很长，对于一般用户来说看到这么长的使用文档都有点望而却步。另外我用 Google 搜索过 V2Ray，搜出来的文章非常少，只能寥寥数篇，而且至少都是好几月之前的，由于 V2Ray 的迭代速度快，一些文章对目前的 V2Ray 已经不适用了。所以我希望通过本指南：
- 让大家了解到最新的 V2Ray 是什么样子的
- 让大家知道利用 V2Ray 可以做些什么
- 尝试用浅显易懂的语言来讲解 V2Ray 的配置
- 共享一些配置案例

另外，借 @xiaokangwang 的一句话与大家共勉（出自于 [#175](https://github.com/v2ray/v2ray-core/issues/175)）：
> 让井底之蛙们知道在更广阔的天地间，还能有另一种活法，比委身于审查更好的活法。

### 听你说了这么多，好像 V2Ray 还不错的样子。但我只是要翻翻墙而已，不想花太多时间怎么办？

请购买付费的翻墙服务，商家会提供很全面的服务。

### 我决定尝试一下 V2Ray，那么我该如何使用这个指南？

V2Ray 的用户手册非常详细地解释了 V2Ray，本指南主要以实际可用的配置从易到难来讲解 V2Ray 的功能特性，力求降低新手使用 V2Ray 的难度。

本指南的目标用户是有一定的 Linux 操作基础，像怎么注册 VPS，怎么用 SSH 登录 VPS，怎么使用 nano（或 vim） 编辑一个文本以及一些 Linux 基本命令的使用件网上有一大堆的指南，没必要重复造轮子再写一篇教程。不管如何，只要你懂得安装配置 Shadowsocks 就完全可以跟着本指南安装配置 V2Ray，哪怕你是用别人写好的一键脚本安装的也是没问题的。

本指南可以看作 V2Ray 用户手册的简易版本，也可以看作 V2Ray 的应用举例。你可以在不参考 V2Ray 用户手册的情况下按照本指南的指导去搭建配置 V2Ray ，但我并不建议你这么做。因为本指南只是引导大家如何理解和配置 V2Ray，相较于用户手册来说有一定的取舍，会忽略一部分东西，另外我不是专业人士以及为了浅显地解释一些东西可能会使用不恰当的词句甚至可能就是错的。V2Ray 的用户手册非常精要，远不是我这种门外汉写得出来的，我建议大家多花点时间仔细研读 V2Ray 用户手册。

## 状态

### 2017-02-11

 - 暂时停更

 由于客观因素限制，V2Ray 配置指南将处于半停更状态，恢复时间未定。V2Ray 在设计之初已经充分考虑到了兼容性问题，之后的 V2Ray 的版本依然可以沿用如今的配置文件。配置指南还留下不少坑，恢复之后会一一填上。

### 2017-07-30

- 更新恢复

在未来的一段时间里，配置指南将逐步更新 V2Ray 近半年来所新增功能的相关配置，并添加其它内容。但由于有其它事情要忙，更新速度较慢。
