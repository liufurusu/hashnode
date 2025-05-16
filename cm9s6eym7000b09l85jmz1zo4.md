---
title: "玩客云刷海纳思系统，自带的alist版本如何升级"
datePublished: Tue Apr 22 2025 07:21:19 GMT+0000 (Coordinated Universal Time)
cuid: cm9s6eym7000b09l85jmz1zo4
slug: alist
tags: alist

---

**alist版本升级教程**

alist-linux-musleabihf-armv7l.tar.gz ,文件下载下来后，要解压出 alist 文件。用终端工具，比如 winscp，用一条命令先把系统正在运行的原 alist 给停了。  
killall alist ,接下来，把新的 alist 给拖到 /opt/alist 文件夹去覆盖原来的，然后给 alist 赋予一个权限 0777，再输命令 shutdown -r 让 hinas 重新启动。稍等一下，系统已经可以使用最新版的 alist 了。

#Alist升级  
systemctl stop alist  
systemctl disable alist  
#进入alist目录  
cd /opt/alist  
#查看文件  
ls  
#把下载的最新版本Alist文件上传到这个文件夹，并修改属性为0755  
ls  
#授予程序执行权限：  
chmod +x alist  
#运行程序  
./alist server  
#获得管理员信息  
./alist admin  
#启动:  
systemctl start alist  
#配置开机自启:  
systemctl enable alist  
#状态:  
systemctl status alist  
#重启:  
systemctl restart alist  
#其它命令  
#关闭:  
systemctl stop alist  
#取消开机自启:  
systemctl disable alist