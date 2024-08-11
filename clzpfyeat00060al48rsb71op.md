---
title: "斐讯N1折腾记：运行 Linux 及优化"
datePublished: Sun Aug 11 2024 10:48:01 GMT+0000 (Coordinated Universal Time)
cuid: clzpfyeat00060al48rsb71op
slug: n1-linux
tags: nas, n1

---

咳咳咳，上篇教程教大家给斐讯 N1 降级并且刷了官改系统，可以当作一个电视盒子和下载机来使用。

有些小伙伴可能不想把它当作电视盒子，就想把它当作 NAS 或者是服务器，但是总不能拿 Android 玩吧，Android 也不是针对服务器设计的。

那么我说过，N1 不仅能刷官改，还能运行 Linux，而且是完整的 Linux 发行版，用 Linux 当服务器、NAS、下载机，体验总是要比 Android 好的。

这篇教程就教大家如何在斐讯 N1 运行 Linux 以及后续的优化，在此之前，**需要将你的 N1 降级并刷入官改固件，也可以只刷入降级关键分区（感谢群里的小伙伴反馈）**。传送门：[《斐讯N1折腾记：降级及刷入官改》](https://www.mivm.cn/phicomm-n1-unofficial/)

## **制作 Linux 启动盘**

N1 运行 Linux 的原理是通电的时候引导 U 盘里的系统

所以需要准备一个U盘来制作启动盘，U 盘大小推荐不低于8G，连续读写速度推荐达到30M/s，随机读写速度推荐达到10M/s。最好是买个全新的，如果是旧的，**提前备份好U盘内数据**。

本教程以 Linux 发行版 Armbian 为例，玩过树莓派的小伙伴应该对这个系统不陌生，是适用于 ARM 架构的 Debian 和 Ubuntu。

你可以在这里找到一些其他的 Linux 发行版：[https://github.com/yangxuan8282/phicomm-n1/wiki/OS](https://github.com/yangxuan8282/phicomm-n1/wiki/OS)，使用方法大致相同。

如果想运行 ArchLinuxARM，可以参考我的另一篇教程：《[斐讯 N1 运行 ArchLinuxARM](https://www.mivm.cn/phicomm-n1-archlinuxarm/)》

下载地址

Armbian：[https://yadi.sk/d/\_rQgn\_FosYuW0g](https://yadi.sk/d/_rQgn_FosYuW0g)

启动盘制作工具：[https://www.alexpage.de/usb-image-tool/download/](https://www.alexpage.de/usb-image-tool/download/)

如果你不喜欢 Armbian，想体验其他 Linux 发行版，可以前往这里下载：[https://github.com/yangxuan8282/phicomm-n1](https://github.com/yangxuan8282/phicomm-n1)

下载地址里有不同的 Armbian 版本，有些基于 Ubuntu，有些则是基于 Debian。

小白推荐使用 Ubuntu，我推荐大家使用Server 版，由于 Linux 没有很好的 N1 GPU 驱动，导致桌面版性能低下，非常卡顿。

首先格式化你的U盘分区格式为 FAT32 或者把U盘分区全部删除。

打开启动盘制作工具：左侧选择你的U盘 → 点击 \[Reset\] 重置U盘 →  点击 \[Restore\] → 选择 Linux 镜像文件 → 提示框点击 \[是\] → 等待制作完成

制作完成后，会出现一个名为 BOOT 的分区，打开它。如果你把分区全部删除了，BOOT 分区默认是 FAT16 格式，Windows 是不会显示的，可以使用 DiskGenius 来访问。

然后需要对 BOOT 的分区 dtb 文件进行一些配置和修改

配置 dtb

以下配置方法基于最新版本，旧版本配置方法可能有所不同。

将 BOOT 分区的`u-boot-s905x-s912`重命名为`u-boot.ext`，然后下载 [https://cdn.mivm.cn/www.mivm.cn/archives/phicomm-n1-linux/extlinux.conf](https://cdn.mivm.cn/www.mivm.cn/archives/phicomm-n1-linux/extlinux.conf) 并将其复制到 BOOT 分区的`extlinux`文件夹。

现有的 dtb 可能会导致系统负载偏高，可以前往 [QQ 群](https://jq.qq.com/?_wv=1027&k=46T5isq)获取修复的 dtb 文件。

将 N1 断电，插上U盘、网线，最好是插靠近 HDMI 接口那个 USB 接口，通电开机。

正常情况下顺利开机并进入 Linux 系统，如果没有进入 Linux 系统而是进入了 Android 系统，ADB 连接至 N1，输入`adb shell reboot update`。

接下来的操作我会以 SSH 远程连接为例，如果你使用 HDMI 连接显示器也可以，这种情况推荐你连接一个 USB HUB，方便连接鼠标和键盘，当然，如果你用的是 Server，那么就不需要鼠标了。

用户：root

密码：1234

如何获取 N1 运行 Linux 的 IP？打开 cmd，输入`nslookup aml`，如果无法解析IP，可以显示器和键盘连接 N1，然后输入`ifconfig eth0 | grep -w inet | awk '{print $2}'`。

如果你不清楚如何使用 SSH 连接 N1，可以参考[《OpenWrt/LEDE 新手指南》](https://www.mivm.cn/openwrt-help/)里的 SSH 连接步骤。

首次登陆需要重置密码，先输入当前密码（1234），再输入新密码以及确认新密码。

设置完密码之后会让你新建一个普通用户，如果是使用 Server，直接按 Ctrl + C 跳过。如果是使用桌面版，推荐创建一个普通用户，按照提示操作即可。

跳过之后，再次使用 SSH 连接。

这时候，整个系统就可以用了，不过我们需要做一些小工作。

## **小工作**

设置中国时区：`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone`

删除默认 DNS：`rm /etc/resolvconf/resolv.conf.d/head && touch /etc/resolvconf/resolv.conf.d/head`

注：在重启系统后，输入`cat /etc/resolv.conf`，查看返回结果是否为你路由器的 DNS，如果是，则表示删除成功，如果不是，则再执行一次删除命令。

更新软件包：`apt update && apt upgrade -y`

重启系统：`reboot`

OpenMediaVault

如果你使用的是 Debian 发行版，就可以安装专门为 NAS 设计的 OpenMediaVault。

OpenMediaVault 是一个基于 Debian 的开源 NAS 解决方案，具有 Web 可视化操作，可以通过各种插件来增强。

如果你是个 Linux 小白，却想轻松的使用 Linux 作为 NAS 系统，强烈推荐你使用 OpenMediaVault。

安装的方式也很简单

启动 Armbian 配置菜单：`armbian-config`

选择 \[Software\] → \[Softy\] → 选中 (空格) \[OMV\] → 回车 → 等待安装完成

安装完成后，在浏览器输入 IP 即可访问，默认用户名和密码为：`admin / openmediavault`

更多有关于 OpenMediaVault 的使用方法可以加群或者上网搜索了解。

如果你安装 OpenMediaVault，下面的操作忽略即可。

挂载外置存储设备

推荐把外置存储设备分区格式化为 ext4 格式，不推荐使用 NTFS 格式。

如果你的外置存储设备是 NTFS 格式，可以使用`mkfs.ext4`命令将你的外置存储设备格式化为 ext4 格式，**格式化前提前备份数据**。

可以用`fdisk -l`查看你的外置存储设备是那个设备，一般是`/dev/sdb`，分区是`/dev/sdb1`，如果有多个分区，依次类推。

输入`mkfs.ext4 /dev/sdb1`将分区格式化为 ext4，格式化完成后使用`fdisk -l`查看是否格式化成功，如果分区的 Type 属性为 Linux（如下图所示），表示分区为 ext4/3。

格式化成功后，为了方便挂载，可以给分区设置一个卷标。

命令：`e2label`

设置卷标示例：`e2label /dev/sdb1 H1`

查看卷标示例：`e2label /dev/sdb1`

然后就可以挂载分区了

挂载分区示例：`echo "LABEL=H1 /mnt ext4 defaults,noatime,nofail 0 2" >> /etc/fstab`

此命令会将卷标为 H1 的分区挂载到 /mnt 目录下

重启系统使挂载生效

Samba 网络共享

\[info\]如果你的主力设备不是 Windows，推荐使用 NFS 等方式共享给其他设备。\[/info\]

使用 Samba 将外置存储设备共享给其他设备

Samba 可以使用 Armbian 自带的一个可视化菜单来配置，非常方便。

启动 Armbian 配置菜单：`armbian-config`

选择 \[Software\] → \[Softy\] → 选中 (空格) \[Samba\] → 回车 → 输入 samba 用户名和密码 → 工作组填写 WORKGROUP → 等待安装完成

安装后，它会自动弹出修改配置文件的界面。

将红框里的内容删除

将 ext 的 path 改为 /mnt，如果你的挂载路径不是 /mnt，则改为你的挂载路径。

按下 TAB 键 → 选择 \[OK\] → 保存退出

Samba 网络共享配置完成

Armbian 自带的可视化配置工具非常的好用，如果你想配置你的网络（不管是无线还是有线）或者是桌面环境等，都可以尝试使用`armbian-config`来完成。

**如果你不想浪费一个 U 盘来当 Linux 启动盘，Armbian 也可以安装到 N1 的内部存储。当然，这个操作稍微麻烦点，而且有风险，如果你想知道怎么做，可以加入** [**QQ 群**](https://jq.qq.com/?_wv=1027&k=46T5isq)**了解。**

剩下的工作，就交给你们，想让它干什么几乎都可以，当然，务必在它的性能承受范围之内。比如离线下载，参考《[斐讯N1折腾记：远程离线下载](https://www.mivm.cn/phicomm-n1-offline-download/)》。

## **后话**

[小山](https://github.com/Hill-98)觉得，既然斐讯 N1 的定位是“NAS”，那么运行 Android 系统是显然满足不了这个属性的，只有运行稳定且高效的 Linux 才可以称为 NAS。虽然 USB 2.0 和 arm 限制了它成为一个完美可用的 NAS，不过如果你对于存储要求没那么高，当一个小型 NAS 也是绰绰有余的。

总而言之，几十块钱买个 N1 这样的玩具，[小山](https://github.com/Hill-98)觉得是非常值的。

还是那句话：文章有什么不懂的地方，欢迎在下方或者在[QQ 群](https://jq.qq.com/?_wv=1027&k=46T5isq)告诉我。

如果我以后想到更多玩法会继续分享给大家，再次感谢所有为 N1 可玩性付出的人。

本篇文章参考了以下资料：

[https://forum.armbian.com/topic/2419-armbian-for-amlogic-s905-and-s905x/](https://forum.armbian.com/topic/2419-armbian-for-amlogic-s905-and-s905x/)

[http://www.right.com.cn/forum/thread-323198-1-1.html](http://www.right.com.cn/forum/thread-323198-1-1.html)

[http://www.right.com.cn/forum/thread-326034-1-1.html](http://www.right.com.cn/forum/thread-326034-1-1.html)