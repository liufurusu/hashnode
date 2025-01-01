---
title: "猫盘刷 One Space 及群晖固件教程"
datePublished: Wed Jan 01 2025 00:58:11 GMT+0000 (Coordinated Universal Time)
cuid: cm5d6uotr00020akxgx5ydaji
slug: one-space
tags: 576k5pmw, 54yr55uy

---

[猫盘刷 ONE SPACE 及群晖固件教程 | 梨数码](https://lishuma.com/archives/2974)

猫盘是近期比较热门的矿难产物之一，主要亮点是可以内置硬盘，而不用像玩客云这类只能外挂硬盘盒。而且配置和群晖 DS119J 基本一样，内存还大了一倍。

实际一个多月前 60 块钱捡的，不过一直拖延症+空余时间不多，到现在才开始折腾，目前这玩意儿行情也涨到了 100 块钱左右。

![](https://lishuma.com/wp-content/uploads/2019/12/IMG_20191208_162710-.jpg align="left")

![](https://lishuma.com/wp-content/uploads/2019/12/IMG_20191208_155251-.jpg align="left")

这里我买到的是 HALOS 版本，市面上还有另一种 catdrive 版本的，部分硬件细节微微有点不同。本教程只保证 HALOS 版本可用，catdrive 版请自行研究。

**另外还有一种大号的猫盘 plus 型号，可以装 3.5 寸硬盘，也不适用于此教程，务必注意！不要手贱乱刷！**

刷群晖的话，由于开启猫盘原始系统的 SSH 需要用到 TTL，本人比较懒，所以这里借助另一款跑路矿渣 X3P 的固件来作为过渡包，然后在 X3P 的固件基础上再刷到群晖。

首先下载所需的所有资料并解压：

```plaintext
百度云链接：https://pan.baidu.com/s/18SlbzsQPfu3FneP24FuS4A 
提取码：rw23 
```

```plaintext
天翼云盘备份链接：https://cloud.189.cn/t/N7vYNj2aqU73（访问码：1303）
```

把猫盘插电连上路由， 硬盘不用接 ，并到路由器管理界面查看对应的 IP，我这里是 192.168.2.150：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-02.jpg align="left")

进入 一键刷机工具目录，管理员身份运行 CatDrive\_flasher，有杀软啥的都先退掉，win 系统的防火墙也暂时关掉：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-01.jpg align="left")

IP 地址都填写好对应的，MAC 地址前六位固定为 78C2C0 ，后六位填上自己猫盘上的后六位，比如你 MAC 后六位为 A1B2C3 ，那么整个 MAC 就是 78C2C0A1B2C3 。序列号也填写好，注意 MAC 和 SN 都要全部大写，如下图：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-03-1.jpg align="left")

看到成功后，软件就可以关了，此时猫盘指示灯会是红色常亮。

把猫盘电源拔了，网线不用动，按住复位键的同时再插上电，看到指示灯变成蓝色闪烁后就可以松开复位键了，会自动进行 X3P 系统安装，耐心等待大约 5-10 分钟，看到指示灯又变成红色常亮后，就说明刷好了。

这时候注意，由于刷完机后设备名字就变了，需要再次到路由器管理界面查看对应的 IP，比如我这里就变成了 192.168.2.40：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-04.jpg align="left")

浏览器里输入对应 IP 即可看到 ONESPACE 界面，默认用户名 admin，密码 123456：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-05.jpg align="left")

ONE SPACE 系统实际本身已经很完善了，各项 NAS 的基本功能都很齐全，足够满足日用，打算使用这系统的话，搭配的硬盘需要格式化成 ext4 再放进去 。下载链接的 x3plus 文件夹里提供了从旧到新的固件，升级的话，注意按照日期顺序，从旧到新一个一个逐步升级，不要直接跨版本。

不过这里目标还是刷群晖系统，继续往下走。

**这时候可以把硬盘装进去了，注意先删掉所有分区。**

刷完 X3P 系统后，就可以通过 SSH 连接猫盘了，使用 WinSCP 这类客户端，选择 SFTP 模式，输入猫盘当前 IP 地址，用户名 root，密码 Etech12，点击连接：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-06.jpg align="left")

然后把群晖的引导文件 full.bin 放进去，我这里直接丢到 tmp 目录下，然后用 putty 这类客户端 SSH 连接上猫盘，root 用户登陆，切换到存放引导文件的目录下：

```plaintext
cd /tmp
```

然后执行以下四条命令，可以复制粘贴，输入一个回车执行一次，等到有反馈后再执行下一条：

```plaintext
dd if="full.bin" of=/dev/mtdblock0 bs=1 skip=0 count=$((0xc8000)) 
```

```plaintext
dd if="full.bin" of=/dev/mtdblock1 bs=1 skip=$((0x000c8000)) count=$((0x00004000))
```

```plaintext
dd if="full.bin" of=/dev/mtdblock2 bs=1 skip=$((0x000cc000)) count=$((0x00434000)) 
```

```plaintext
dd if="full.bin" of=/dev/mtdblock3 bs=1 skip=$((0x00500000)) count=$((0x00300000)) 
```

四条命令执行完后应该如图所示：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-07.jpg align="left")

最后直接输入 reboot 重启即可。

然后浏览器里打开群晖的在线安装地址：[http://find.synology.com/](http://find.synology.com/)  ，点一下搜索设备，可以看到已经把猫盘当成了 DS119J，也显示了设备的 IP 等信息，点击联机，按照提示一步步进行系统安装即可：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-08.jpg align="left")

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-09.jpg align="left")

默认是直接在线获取最新固件，如果猫盘没联网的话，也可以手动下载官网固件进行安装，官方下载地址为：[https://www.synology.cn/zh-cn/support/download/DS119j#firmware](https://www.synology.cn/zh-cn/support/download/DS119j#firmware)

装完提示重启，页面会有 10 分钟的倒计时，实际不需要这么久，大概等个三四分钟就行。重启后设定好设备名称和管理员账户后就完成了群晖设置，可以直接使用了：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-10.jpg align="left")

另外再说一下硬盘休眠问题，由于猫盘装的是 2.5 寸盘，不如 3.5 寸那么耐艹，开启休眠可以保护下硬盘寿命，现在控制面板里开启 SSH 功能，然后使用 putty 等客户端进行连接，用户名密码就是群晖管理员的用户名密码：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-11.jpg align="left")

编辑/etc/init 目录下的 syslog-ng.conf 文件，输入：

```plaintext
sudo vi /etc/init/syslog-ng.conf
```

会提示再次输入密码，然后按 i 进入编辑模式，定位到 50 行左右，如红框里所示插入这一段语句：

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-12.jpg align="left")

```plaintext
#scemd bind 
touch /tmp/scemd.log.new || true 
chmod 660 /tmp/scemd.log.new || true 
chown system:log /tmp/scemd.log.new || true 
mount -o bind /tmp/scemd.log.new /var/log/scemd.log || true 
# 
```

改完后按 esc，然后输入`:wq` 退出编辑，重启群晖。

重启后去套件中心里安装日志中心工具，过一段时间去查看日志，如果看到有箭头所指的这种 Internal disks woke up from hibernation 日志，就说明硬盘休眠功能正常。

![](https://lishuma.com/wp-content/uploads/2019/12/catdrive-13.jpg align="left")