---
title: "2025最新版：基于Cloudflare Tunnel的内网穿透实战指南"
datePublished: Tue May 20 2025 12:47:13 GMT+0000 (Coordinated Universal Time)
cuid: cmawidxc5000d09l9giy194oy
slug: 2025cloudflare-tunnel
tags: cloudflare

---

[2025最新版：基于Cloudflare Tunnel的内网穿透实战指南 - 阿雷的小窝](https://blog.leihub.cn/archives/873)

这两年折腾内网穿透，基本把市面方案试了个遍：

1. ‌**FRP 反向代理**‌：靠学生优惠白嫖的腾讯云服务器（4 核 4G+6M 带宽）确实香，但三年到期后续费直接翻倍。新买的 2 核 2G 云服务器做中继，转发网页服务勉强够用，但每年上千的续费账单实在肉疼‌。
    
2. ‌**虚拟局域网**‌：Zerotier 打洞成功率确实高，但每次知道服务器位于北美就心慌，生怕哪天断联；换成国人开发的 Easytier 后虽然数据走 P2P 更安全，但穿透率又掉回解放前 —— 现在直接两条腿走路，两套方案互为备份‌24。
    
3. ‌**Cloudflare Tunnel**‌：最近发现的宝藏方案，用托管在 CF 的域名就能把内网服务甩到公网。虽然节点在国外偶尔抽风，但胜在‌**零成本 + 免备案 + 银行级加密**‌，配个域名 10 分钟搞定，比 FRP 改配置重启服务省心十倍‌。‌
    

这次重点讲 Cloudflare Tunnel，就是看中它能把 NAS 低频访问的内网服务，用最低成本稳定暴露到外网。关键数据全程 SSL 加密，比自建 FRP 省了服务器钱还不用操心 DDoS 防护‌。

#### **‌一、前置准备‌**

1. ‌**域名与账号配置**‌
    
    * 注册 Cloudflare 账号并完成邮箱验证（建议使用 Gmail 等国际邮箱）‌。
        
    * 将已有域名托管至 Cloudflare：进入域名注册商后台（如阿里云、华为云），将 DNS 服务器替换为 Cloudflare 提供的地址（例如 `mike.ns.cloudflare.com`）‌。
        
    * ‌**绑定付款方式**‌：支持国际信用卡、借记卡或 PayPal，仅用于身份验证（支付金额为 0 元，服务免费）‌
        
        ![创建Cloudflare Zero Trust团队](http://imgbed.leiting2020.top//img/image-20250223141406253.png align="left")
        
    
    确认是 0 元免费就可以继续支付了
    
    ![免费计划](http://imgbed.leiting2020.top//img/image-20250223141416641.png align="left")
    
    不添加付款方式不让下一步，你可以点 Next 后输入付款方式
    
    ![零元购](http://imgbed.leiting2020.top//img/image-20250223141632849.png align="left")
    
2. ‌**内网环境要求**‌
    
    * 确保被穿透设备（如 NAS、Web 服务器）支持 Docker 或本地客户端运行（推荐 Linux 系统或群晖 DSM
        
    * 开放设备互联网访问权限，检查防火墙是否放行 Cloudflare 客户端通信端口（如 HTTP/80、HTTPS/443）‌
        

#### **‌二、创建 Cloudflare Tunnel 隧道‌**

1. ‌**进入 Zero Trust 控制台**‌
    
    * 登录 Cloudflare 控制台 → 左侧菜单点击 ‌**Zero Trust**‌ → 进入 ‌**Networks &gt; Tunnels**‌ ‌
        
    * 首次使用需选择免费计划（Free Plan）‌
        
2. ‌**新建隧道**‌
    
    * 点击 ‌**Create a tunnel**‌ → 输入隧道名称（如 `home-nas`）→ 保存‌
        
    * ‌**客户端安装**‌：根据设备系统选择安装方式（推荐 Docker），群晖可以通过矿神源第三方应用进行安装，本质上也是 docker。
        
        ```dockerfile
        # 拉取官方镜像docker pull cloudflare/cloudflared:latest# 运行隧道（替换YOUR_TOKEN为实际令牌）docker run -d cloudflare/cloudflared tunnel --no-autoupdate run --token YOUR_TOKEN   
        ```
        

![创建cloudflare tunnel](http://imgbed.leiting2020.top//img/image-20250223141830474.png align="left")

​ 隧道类型选 Cloudflared 就可以，然后下一步

![选择隧道类型](http://imgbed.leiting2020.top//img/image-20250223141912829.png align="left")

​ 创建隧道

![创建隧道](http://imgbed.leiting2020.top//img/image-20250223141956222.png align="left")

​ 因为我们要在群晖的 Docker 下用所以选择 Docker，你可以根据自己情况选择。在群晖第三方套件 中安装 Cloudflare Tunnel，输入 tocken 安装进行连接，在下方就可以看到连接器了。

![选择连接方式](http://imgbed.leiting2020.top//img/image-20250223142317799.png align="left")

#### **‌三、配置公网访问规则**

1. ‌**域名映射与协议设置**
    
    * 在隧道详情页点击 ‌**Configure Public Hostname**‌ → 选择已托管的域名和子域名（如 `blog.leitool.top`）
        
    * 设置协议（HTTP/HTTPS）及内网服务地址，比如我的内网服务部署在本地群晖的 8080 端口上，内网服务地址就填 `http://localhost:8080`。
        
    * 支持路径映射（如将 `/jellyfin` 指向本地端口 `8096`）‌
        
    * 一个 Tunnel 中可以添加多条三级域名来跳转到不同的内网服务，在 Tunnel 页面的 Public Hostname 中新增即可。
        
2. ‌**端口限制说明**‌
    
    * 免费版仅支持部分 HTTP/HTTPS 端口（如 80、443、2082 等），需避免使用非常用端口‌
        

​ 这样配置好托管的域名和群晖的 IP 和端口就 OK 了

![创建丑tunnel成功](http://imgbed.leiting2020.top//img/image-20250223142701535.png align="left")

#### **‌四、验证与访问‌**

1. ‌**测试穿透服务**‌
    
    * 通过浏览器访问配置的子域名（如 `https://blog.leitool.top`），若显示内网服务页面则成功‌。
        
        ![内网穿透服务成功](http://imgbed.leiting2020.top//img/image-20250224220147927.png align="left")
        
    * 若失败，检查客户端日志或 Cloudflare Tunnel 状态‌
        

#### **五、为你的服务添加额外验证**

如果你觉得这种直接暴露内网服务的方式有较高的安全风险，我们还可以使用 Application 功能为服务添加额外的安全验证。

1. 点击 Application - Get started。
    
    ![image-20250301163502956](https://imgbed.leiting2020.top/img/image-20250301163502956.png align="left")
    

​ 2. 选择自托管

![image-20250301163619757](https://imgbed.leiting2020.top/img/image-20250301163619757.png align="left")

3. 填写配置，**注意 域名和子域名需要使用刚刚创建的 Tunnel 服务相同的域名配置**。
    

![image-20250301163929962](https://imgbed.leiting2020.top/img/image-20250301163929962.png align="left")

4. 选择验证方式。填写 策略名称（任意）。在添加规则区域选择验证方式，示例图片中使用的是特定完整邮箱地址。你还可以选择使用指定的邮箱域名（如 @gmail.com）验证、IP 地址范围等方式。
    

![image-20250301164101588](https://imgbed.leiting2020.top/img/image-20250301164101588.png align="left")

5. 完成添加
    
6. 这样访问配置的子域名（如 `https://blog.leitool.top`）可以看到网站多了一个验证页面，使用刚刚设置的域名邮箱来访问。
    

#### **六、总结与适用场景‌**

* ‌**核心优势**‌：免费、无需公网 IP、集成 DDoS 防护和 HTTPS 加密‌。
    
* ‌**典型用途**‌：
    
    * 家庭 NAS 远程访问（如 Jellyfin 影音服务）‌。
        
    * 自建 GitLab、Nextcloud 等 Web 服务公网暴露‌。
        
    * 通过 SSH/RDP 隧道安全访问内网设备（需配置 TCP 协议映射）‌。
        

#### **七、参考**

1. [群晖搭建 Cloudflare 免费隧道穿透，无公网 IP 也能搞定！\_\_什么值得买](https://post.smzdm.com/p/aklp824e/)
    
2. [无需公网 IP！CloudFlare Tunnel 内网穿透实现内网访问\_路由器\_什么值得买](https://post.smzdm.com/p/admp85dn/)