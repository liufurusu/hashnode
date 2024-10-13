---
title: "关于甲骨文服务器搭建的服务不能正常访问的问题"
datePublished: Sun Oct 13 2024 00:37:50 GMT+0000 (Coordinated Universal Time)
cuid: cm26uwdc700000al9dt3gdbhe
slug: ubuntu
tags: ubuntu, 5lqr5pyn5yqh5zmo, 55sy6aqo5pah

---

因为centos停止维护了，所以这次我使用了ubuntu22.04 的系统，搭建好网站后发现不能正常访问，以下是处理方法 1.首先就是要在甲骨文面板里面放行你应用所对应的端口（如果安装了宝塔之类的面板要在面板里也放行端口） 2.不知道是Ubuntu还是Oracle的原因，放行端口后并不能正常访问，后面查询是因为Ubuntu22.04系统的自带的iptables策略的原因我们只要关掉就行了，这里有三种方法（我使用的是第二种方法）出于安全推荐使用第三种方法 1.直接删除iptables规则 sudo rm -rf /etc/iptables && reboot

2.开放所有端口 sudo iptables -P INPUT ACCEPT

sudo iptables -P FORWARD ACCEPT

sudo iptables -P OUTPUT ACCEPT

sudo iptables -F

sudo apt-get purge netfilter-persistent reboot

3.使用iptables开放你需要的端口 sudo iptables -I INPUT -s 0.0.0.0/0 -p tcp --dport 8888 -j ACCEPT

sudo iptables -I INPUT -s 0.0.0.0/0 -p tcp --dport 80 -j ACCEPT

sudo iptables -I INPUT -s 0.0.0.0/0 -p tcp --dport 443 -j ACCEPT

sudo iptables-save sudo apt-get update

sudo apt-get install iptables-persistent -y sudo netfilter-persistent save sudo netfilter-persistent reload