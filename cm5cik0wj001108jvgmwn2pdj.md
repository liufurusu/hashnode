---
title: "【降级篇】猫盘群晖引导dsm7.0降dsm6.23"
datePublished: Tue Dec 31 2024 13:38:03 GMT+0000 (Coordinated Universal Time)
cuid: cm5cik0wj001108jvgmwn2pdj
slug: dsm70dsm623
tags: 576k5pmw, 54yr55uy

---

刚一出来DSM7.0体验版，小伙伴就跃跃欲试，殊不知道这是一道毒药，吃了，你不得不找解药的那种，我不是说这个版本不好，而是这还是体验版本，很多功能不完善，猫盘对它支持也不是很好，这里特别指出的什么渣论坛，没有求证就放到首页，造成不必要的麻烦，这不，博主在测试后将问题列出一二，并将降级的方法和工具一并放上，只要你能举一反三，这方法同样适合DS119J刷DS120J，这里讲得详细一点，慢一点，以求大家能懂。

注：此教程纯小白保姆级，经常玩机的可以直接跳到“猫盘刷机/指令输入”。

刷机前准备

一条TTL线：CH340G（很多人说这刷那刷，说实话一条TTL线才最实在，以后也能用，才10块钱包邮，真心不贵）

软件工具包：（登陆可看）

登录访问

***本站用户*** 免费查看

登录账号

您未登录，请登录 或 注册后查看

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第1张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_81253.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_81253.png)

这里面分别有ds119j固件和ds120j固件，

但是注意的是这个不是能防掉IP的固件，有的刷机后可能掉IP，如果之前有因为掉IP而刷一机一码的ds120j固件的，需要找回之前的固件，还有在花王群购买的需要刷专用的固件,切记！！！

所有的接线图

所谓上来一张图，接线全靠图，一点也不错，不说了，上图：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第2张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_49891.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_49891.jpg)

再来一张接线图，看了这个还不懂的，那博主真没办法了：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第3张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_48119.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084857_48119.jpg)

这下应该解决困扰大家很多的接线问题了，不用管这么多，你就按图片接就好了。

文章没发多久就有人问，大猫怎么接呢，这不，还是我的错。。。。不废话，直接再上大猫的接线图：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第4张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084858_65495.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084858_65495.jpg)

猫盘刷机

为什么博主这么推荐用TTL线刷机呢，因为它最实在，能知道进度，是否刷成功，还有这种刷机也是适用范围最广的，只要有一根TTL线，猫盘刷机还真不是个事，好吧，废话不多话，继续。。。

## 软件工具的使用

### 首先我们使用putty来登陆（当然也可用xshell）

这里讲的就是putty的使用，开始我们查看TTL线的端口号，方法：电脑--属性--硬件管理--端口--COM号（我的是com3)

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第5张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_39408.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_39408.png)

然后在putty进行以下设置，端口号：com3(我的，你改你自已的），串口号：115200，其它选择none，如图：（当然你也可用汉化版，这无所谓）

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第6张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_80989.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_80989.png)

再选择登陆，如图：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第7张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_15383.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084859_15383.png)

### 再来使用TFTP工具

这个是用TFTP协议通过网线传输数据的工具，当然它需设置两个东西，一个是文件的路径，一个是服务器的IP（即电脑的IP），电脑IP的查看方法：打开网络链接--属性--详情

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第8张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084900_88184.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084900_88184.png)

接下来是TFTP设置如图：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第9张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084900_95707.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084900_95707.png)

## 指令输入开刷

```plaintext
setenv ipaddr 192.168.2.116    #指定猫盘为局域网不跟其它冲突的IP
setenv serverip 192.168.2.3    #电脑IP
bubt ds119j.bin spi tftp       #固件名，你刷120的就是ds120j.bin
reset                          #刷机完成后输入它重启
```

只要4个指令就完成了，如果你是个老手，真的非常容易，我这讲得很详细，也是照顾一下其它和我开始接触猫盘的小白，再来一张图，看看真相吧：

这里刷的固件我随便命名的，你刷的时候要改成对应的固件名。

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第10张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_54787.png align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_54787.png)

方法是很简单，大家可以举一反三，同样适用于ds119j升级到DS120J等，这里用到的方法同样在其它刷机也适用。

猫盘群晖DSM7.0问题

为什么最后讲DSM7.0，因为它就是毒药，为什么呢？你听我慢慢道来。。。

dsm7.0的体验版本可以通过离线包刷固件，实现刷机的，这个刷机也是非常简单的，如图：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第11张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_65097.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_65097.jpg)

刷完后界面确实很漂亮了，非常扁平化的UI，类似于当年的苹果iso7.0升级，来看看它的界面吧：

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第12张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_46269.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_46269.jpg)

[![【降级篇】猫盘群晖引导dsm7.0降dsm6.23 猫盘专栏 第13张](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_93634.jpg align="left")](https://www.huakings.cn/zb_users/upload/2021/08/20210824084901_93634.jpg)

它的问题是：1、不能安装第三方套件，它会提示你权限不足，2、不能安装套件，只能安装测试套件，3、猫盘兼容性不好，如果没有安装好的7.0硬盘插上，那么就会掉IP，没错，就是掉IP，也就是“砖了”。。。。（跟spi砖有区别）

有的小伙伴说，无所谓，我就是想体验一把，博主能放个下载地址吗？这个没问题，马上放下载地址：

ds119j固件：https://cloud.189.cn/t/EBv6fqjIjYBj(访问码:2ku8)

ds120j固件：https://cloud.189.cn/t/nUvIZvVn2qee(访问码:e1oz)

总结：反正毒药和解药你们都有了，也不怕了，开干吧，折腾吧，什么？不折腾？那样我会和死一样难受，大家收藏此文也就不怕折腾了，放心大胆的开干吧，之前后悔的也有后悔药了，慢慢食用吧。。。

[猫盘救砖记 - 知乎](https://zhuanlan.zhihu.com/p/655665257)