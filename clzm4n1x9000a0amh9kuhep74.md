---
title: "多个硬盘怎么 映射到 到一个文件夹用"
datePublished: Fri Aug 09 2024 03:07:57 GMT+0000 (Coordinated Universal Time)
cuid: clzm4n1x9000a0amh9kuhep74
slug: 5asa5liq56gs55uy5oco5lmiioayoowwhowiscdlildkuidkukrmlofku7blplnnlkg
tags: windows, 5pah5lu2566h55cg5zmo

---

# .Windows下文件夹映射的实现（将文件夹从一个盘映射到另一个盘）

### 1，需求描述

（1）有时我们想让两个文件夹下的内容完全一样（这种需求在服务器上比较常见）。比如我们的文件存放在文件夹**A**中，但又希望通过文件夹**B**也能访问到。同时不管是对**A**文件夹里的内容做修改，还是对**B**文件夹里的内容做修改，另一个文件夹里的内容也会同步更新。

（2）而如果使用文件夹快捷方式的话，双击打开或在资源管理器中打开会是链接对应的文件夹。而且它毕竟还是快捷方式，如果在程序中读取时，它的后缀是 **.link** 而不是所链接的文件夹。  

### 2，解决办法

要实现上面的需求，除了用同步软件来做外，还可以用 **windows** 的文件夹映射来实现。具体的操作命令如下：

<table><tbody><tr><td colspan="1" rowspan="1"><p>1</p></td><td colspan="1" rowspan="1"><p><code>MKLINK [[/D] | [/H] | [/J]] Link Target</code></p></td></tr></tbody></table>

* **/D**：创建目录符号链接。默认为文件符号链接。
    
* **/H**：创建硬链接，而不是符号链接。
    
* **/J**：创建目录联接。
    
* **Link**：指定新的符号链接名称。
    
* **Target**：指定新[链接引用的路径（相对或绝对）](https://www.hangge.com/)
    

# FreeMove - 快速移动已安装软件的文件夹（把C盘软件移到其它盘下）

    我们日常使用电脑时，可能图方便会直接把软件都安装在 **C** 盘下面。日积月累会发现 **C** 盘空间慢慢地快要被挤爆了。如果直接把已安装的软件目录剪切移动到其它盘下，由于注册表等问题，很多程序直接就无法运行。重装系统又很麻烦。

    这里推荐一款软件：**FreeMove**。使用它可以安全地将已安装的应用转移到另一个磁盘。

### 1，软件介绍

（1）**FreeMove** 的原理是为文件夹创建一个软链接，之后指向到新位置，从而实现移动的功能。

（2）由于整个过程不涉及修改注册表，所以风险是零。

（3）软件下载地址：

* 百度网盘：[https://pan.baidu.com/s/1skKPxXr](https://pan.baidu.com/s/1skKPxXr)
    
* [GitHub主页：](https://pan.baidu.com/s/1skKPxXr)[https://github.com/imDema/FreeMove](https://github.com/imDema/FreeMove)
    

### [2，使用说明](https://github.com/imDema/FreeMove)

[打开软件后](https://github.com/imDema/FreeMove)，在"**Move From**”处选择我们需要移动的软件文件夹位置，再在“**To**”[处选择目标移动位置，最后点击“**Move**”按钮即可。](https://pan.baidu.com/s/1skKPxXr)

![原文:FreeMove - 快速移动已安装软件的文件夹（把C盘软件移到其它盘下）](https://www.hangge.com/blog_uploads/201801/2018011917302688130.png align="left")

[注意：由](https://pan.baidu.com/s/1skKPxXr)于这个移动是通过软连接[实现，那么](https://github.com/imDema/FreeMove)[原文件夹的位置会有个快捷方式。如果觉得这个碍眼，可以在移动时钩选上“**Set or**](https://pan.baidu.com/s/1skKPxXr)**igi**[**nal folder to**](https://github.com/imDema/FreeMove) [**hidden**”，那么移动后原位置的快捷方式就会被隐藏。](https://pan.baidu.com/s/1skKPxXr)

# [Windows下多个硬盘显示为一个分区的方案](https://pan.baidu.com/s/1skKPxXr)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723171454964/6a87f8a8-1bf6-4c4f-bd4b-a225a8e9649f.png align="center")

# [**1、跨区卷**](https://pan.baidu.com/s/1skKPxXr)

[特点：](https://pan.baidu.com/s/1skKPxXr)

* [相当](https://www.hangge.com/)[于jbod（如下图）](https://github.com/imDema/FreeMove)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723171563634/bd402915-7d0b-4d3f-bdee-ed09172cd5b2.png align="center")

[优点：](https://github.com/imDema/FreeMove)

* [方便管理。](https://github.com/imDema/FreeMove)
    
* [可以](https://github.com/imDema/FreeMove)[扩展（“带区卷”则不可扩展）。](https://pan.baidu.com/s/1skKPxXr)
    

[缺点：](https://pan.baidu.com/s/1skKPxXr)

* [若其中一个硬](https://pan.baidu.com/s/1skKPxXr)[盘损坏，则可能所有数据](https://www.hangge.com/)[都无法读取。](https://github.com/imDema/FreeMove)
    

[其他：](https://github.com/imDema/FreeMove)

* [会使用完一个硬盘所划出的空间后，再](https://github.com/imDema/FreeMove)[使用另块硬盘；速度没](https://www.hangge.com/)[有提升。](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    

# [**2、带区卷**](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

[特点：](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

* [每块数据64KB；](https://pan.baidu.com/s/1skKPxXr)
    
* [相当于“多盘raid 0”（如下](https://pan.baidu.com/s/1skKPxXr)[图）](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723171588978/046ed2f9-f754-44a6-a835-4882c61c513e.png align="center")

[优点](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)[：](https://github.com/imDema/FreeMove)

* [方便管理，同时读取一处的数据时，磁盘速度比“跨区卷”快约3](https://github.com/imDema/FreeMove)[0％](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    

[缺点：](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

* [若其中一个硬盘损坏，则可能所有数据都无法读取（更甚）；](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    
* [读写时会多占用一点CPU](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)[资源。](https://www.hangge.com/)
    

> [*以上图片摘自*](https://www.hangge.com/)[*：https://blog.csdn.net/ensp1/article/details/81318135*](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

# [**3、文件**](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)[**夹映射**](https://www.hangge.com/blog/cache/detail_1879.html)

[举例一：](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

* [将 C 盘下的 Windows 文件夹](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)[，映射成 E 盘下](https://www.hangge.com/)
    

[命](https://www.hangge.com/)[令：](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

* [mklink /j E:\\Windo](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)ws C:\\Windows
    

举例二：

* 将 F 盘下的 “分类十” 文[件夹，映射成 E 盘下](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    

[命令：](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)

* [mklink /j E:\\分类十 F:\\分类十](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)
    

> [*以上参考：https://www*](https://www.hangge.com/blog/cache/detail_1944.html#google_vignette)[*.hangge.com/blog/cache/detail\_1879.html*](https://www.hangge.com/blog/cache/detail_1879.html)

# **4、装载的驱动器（装入NTFS文件夹）**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723171615469/6730d3f9-c8b7-4729-97c2-75790c279164.png align="center")

[**在Windows系统中，‌可以使用“符号链**](https://www.cnblogs.com/xiaohi/p/15321421.html)**接”(Symbolic Link)或“硬链接”(Hard Link)来实现将多个文件夹的文件映射到一个新的文件夹的效果。‌**

在Windows中，‌你可以使用命令提示符(CMD)或者PowerShell以管理员身份运行，‌并使用`mklink`命令来创建符号链接或硬链接。‌具体操作如下：‌

* **使用符号链接(Symbolic Link)**：‌
    
    1. 打开命令提示符(CMD)或者PowerShell以管理员身份运行。‌
        
    2. 使用以下命令创建符号链接：‌`mklink /D "新文件夹路径" "源文件夹路径"`  
        将"新文件夹路径"替换为你希望创建的新文件夹的路径，‌将"源文件夹路径"替换为你想要映射的源文件夹的路径。‌这将创建一个指向源文件夹的符号链接。‌在新文件夹中，‌你将可以看到源文件夹中的所有文件。‌
        
* **使用硬链接(Hard Link)**：‌
    
    1. 同样，‌首先打开命令提示符(CMD)或者PowerShell以管理员身份运行。‌
        
    2. 使用以下命令创建硬链接：‌`mklink /J "新文件夹路径" "源文件夹路径"`  
        将"新文件夹路径"替换为你希望创建的新文件夹的路径，‌将"源文件夹路径"替换为你想要映射的源文件夹的路径。‌这将创建一个指向源文件夹的硬链接。‌
        

需要注意的是，‌创建符号链接或硬链接需要管理员权限。‌另外，‌对于某些特殊文件夹（‌如系统文件夹）‌，‌可能需要使用其他技巧来创建链接。‌这种方法可以方便地组织和管理文件，‌尤其是当你需要将多个硬盘或分区的文件整合到一个统一的访问点时，‌非常有用