---
layout: post
title: "Ubuntu安装指南"
description: "记录的我安装使用ubuntu的种种，会不定时更新"
category: "指南"
tags: [Ubuntu, 指南]
---
{% include setup %}

##准备工作
###下载ISO
在[官方网站](http://www.ubuntu.com/ubuntu)貌似只有CD版的ISO下载。  
我不是很推荐这个版本，这里提供一个DVD版本的下载地址:
[CD](ftp://ftp.ukc.mirrorservice.org/sites/releases.ubuntu.com//precise/ubuntu-12.04-desktop-i386.iso)/[DVD](http://cdimage.ubuntu.com/releases/12.04/release/ubuntu-12.04-dvd-i386.iso)

tips:这里提供的都是32Bit的版本，64Bit请自行到[这里](http://cdimage.ubuntu.com/releases/12.04/release/)查找

###分区

- 硬盘分区知识
    1. 在 Linux 中，每一个硬件设备都映射到一个系统的文件，对于硬盘、光驱等 IDE 或 SCSI 或SATA 设备也不例外。
    2. Linux 把各种 IDE 设备分配了一个由`hd`前缀组成的文件；而对于各种SCSI或SATA设备，则分配了一个由 sd 前缀组成的文件。例如，第一个IDE设备，Linux 就定义为`hda`;第二个 IDE 设备就定义为`hdb`下面以此类推。而SCSI或SATA设备就应该是`sda`、`sdb`、`sdc`等。  
    3. 硬盘分区一共有三种：主分区，扩展分区和逻辑分区。在一块硬盘上最多只能有四个主分区。您可以另外建立一个扩展分区来代替四个主分区的其中一个，然后在扩展分区下您可以建立更多的逻辑分区。扩展分区只不过是逻辑分区的“容器”。实际上只有主分区和逻辑分区进行数据存储。一个磁盘的扩展分区只能有一个，并且只能是连续的磁盘空间。

- 常见分区图示  
    1. Windows分区编号
![alt ](http://pic.yupoo.com/arthur623/C2oOHFcj/medish.jpg "Windows分区编号实例")
    2. Linux分区编号
![alt ](http://pic.yupoo.com/arthur623/C2oOHurq/medish.jpg "Windows分区编号实例")

- 安装linux需要的几个主要分区  
    1. `/`，可以使用Ext4格式，这个区是你的文件分区，必须。  
    2. `Swap`，物理内存的两倍就好，必须。  
    3. `/boot`，如果要装多个发行版，建议/boot单独，留个100多，200MB差不多，也别太小。  
    4. `/home` ，这个就需要根据需要，一般用户的音频、视频、文档等文件都存储在这里，不单独分出来的话，就在`/`里。


###安装方式
- 刻录DVD  
- 硬盘安装  
- WUBI  
参考[这里](http://www.ubuntu.com/download/help/install-ubuntu-with-windows)。  
简单快速，适合玩票的用户，一说会少许影响硬盘的读取速度。  
- USB  
    1. 你需要一个U盘
    2. 你的主板支持USB启动。这很重要。
    3. 你需要把ISO写入到U盘，推荐[LiveLinux USB Creator](http://www.linuxliveusb.com/)，比Ubuntu的好用，还支持其他Linux发行版的写入。  
    Tips:最好把U盘格式化到FAT32格式。


##安装
###取消安装中更新
默认使用的源因为速度很慢，会严重拖慢安装的速度，我们选择在安装完成后来更新这些东西。
###设置分区
根据你自己的需要。参照上面的准备里的介绍。

- 只有Ubuntu的情况可以参考
![alt ](http://pic.yupoo.com/arthur623/C2p4ASuG/medish.jpg "单Ubuntu分区实例")
- 如果要安装双系统，并且已经有Windows存在，可以先删掉一部分分区：
![alt ](http://pic.yupoo.com/arthur623/C2p4C8b8/medish.jpg "Windows加Ubuntu分区实例1")
- 安装时选择使用未使用的空间，Ubuntu就会自动创建分区如下
![alt ](http://pic.yupoo.com/arthur623/C2p4BYcH/medish.jpg "Windows加Ubuntu分区实例2")

###引导

- 系统引导过程简介

        1. 开机；
        2. BIOS加电自检(POST---Power On Self Test)，内存地址为0fff：0000；
        3. 将硬盘第一个扇区(0头0道1扇区,也就是Boot Sector)读入内存地址0000：7c00处；
        4. 检查(WORD)0000：7dfe是否等于0xaa55。若不等于则转去尝试其他介质；如果没有其他启动介质，则显示 ”No ROM BASIC” ，然后死机；
        5. 跳转到0000：7c00处执行MBR中的程序；
        6. MBR先将自己复制到0000：0600处，然后继续执行；
        7. 在主分区表中搜索标志为活动的分区.如果发现没有活动分区或者不止一个活动分区，则停止；
        8. 将活动分区的第一个扇区读入内存地址0000：7c00处；
        9. 检查(WORD)0000：7dfe是否等于0xaa55，若不等于则显示 “Missing Operating System”，然后停止,或尝试软盘启动；
        10. 跳转到0000：7c00处继续执行特定系统的启动程序；
        11. 启动系统。

        Tip：以上步骤中(2),(3),(4),(5)步由BIOS的引导程序完成;(6),(7),(8),(9),(10)步由MBR中的引导程序完成。  

        Tip：一般多系统引导程序（如Smart Boot Manager，BootStar，PQBoot等）都是将标准主引导记录替换成自己的引导程序，在运行系统启动程序之前让用户选择想要启动的分区。而某些系统自带的多系统引导程序(如 LILO，NT Loader等)则可以将自己的引导程序放在系统所处分区的第一个扇区中，在Linux中即为两个扇区的SuperBlock。  

- 更多信息可以参照这篇[Blog](http://oilbeater.com/2012/06/29/the-secret-of-os-startup/)

- 选择引导的安装位置。
    1. 安装在`/dev/sda1`上这样做会替换Windows的引导程序，不过可以直接引导Ubuntu或者Windows启动。
    2. 安装在`/boot`挂载的分区上这样做适合有多个系统的情况。但是安装完成后是无法引导进入Ubuntu的。需要在Windows下使用[EasyBCD](http://www.xiazaiba.com/html/6227.html)，为Ubuntu建立引导项。  


###安装完成
至此，我们的Ubuntu已经安装完成了，但是我们的工作还有很多。


##安装完成后的工作
### 更新源   
上文提到了，Ubuntu默认使用的原速度非常慢，即使使用官方中国的源，速度也不是很快，这个时候，我们就要更换更快的源。  
较快速的升级源有163,台湾源，科大源，搜狐源等，请自行Google。这里提供了一个我使用的源列表。
    
    deb http://mirrors.163.com/ubuntu/ precise main restricted  
    deb-src http://mirrors.163.com/ubuntu/ precise main restricted  
    deb http://mirrors.163.com/ubuntu/ precise-updates main restricted  
    deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted  
    deb http://mirrors.163.com/ubuntu/ precise universe  
    deb-src http://mirrors.163.com/ubuntu/ precise universe  
    deb http://mirrors.163.com/ubuntu/ precise-updates universe  
    deb-src http://mirrors.163.com/ubuntu/ precise-updates universe  
    deb http://mirrors.163.com/ubuntu/ precise multiverse  
    deb-src http://mirrors.163.com/ubuntu/ precise multiverse  
    deb http://mirrors.163.com/ubuntu/ precise-updates multiverse  
    deb-src http://mirrors.163.com/ubuntu/ precise-updates multiverse      
    deb http://mirrors.163.com/ubuntu/ precise-security main restricted  
    deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted  
    deb http://mirrors.163.com/ubuntu/ precise-security universe  
    deb-src http://mirrors.163.com/ubuntu/ precise-security universe  
    deb http://mirrors.163.com/ubuntu/ precise-security multiverse  
    deb-src http://mirrors.163.com/ubuntu/ precise-security multiverse  
    deb http://extras.ubuntu.com/ubuntu precise main  
    deb-src http://extras.ubuntu.com/ubuntu precise main  
    deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse  
    deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse

1. 更换源

        sudo gedit /etc/apt/sources.list
        //大家将新的升级源全部覆盖原文件sources.list的内容，保存退出  
2. 更新软件列表

        sudo apt-get update
3. 安装更新

        sudo apt-get update && sudo apt-get upgrade


###使用Gnome

* 安装  
我一直对于Ubuntu自带的桌面环境不爽，如果你和我一样，可以尝试用下面的方法安装Gnome.

        sudo apt-get install gnome-shell
* 使用
注销之后选择使用Gnome登陆即可，下一次会默认使用。
    ![alt ](http://pic.yupoo.com/arthur623/C2welvRl/medish.jpg "切换Gnome") 

* 更换主题
登陆Gnome后，可能会有输入发状态栏闪烁的情况，更换一个主题就好了，现在的支持Gnome3的优秀主题比比皆是。可以到[Gonme-look](http://gnome-look.org/)找找你喜欢的。使用Gnome Tweak Tool安装主题：  

        sudo apt-get install gnome-tweak-tool  
        sudo add-apt-repository ppa:ferramroberto/gnome3  
        sudo apt-get update  
        sudo apt-get install gnome-shell-extensions-user-theme  

![alt ](http://pic.yupoo.com/arthur623/C2whhr65/medish.jpg "Evolve") 

* 字体 
Ubuntu自带的字体在浏览网页的时候看着会很别扭。  
这里我选择安装MS的雅黑字体  

        wget -O get-fonts.sh.zip http://files.cnblogs.com/DengYangjun/get-fonts.sh.zip  
        unzip -o get-fonts.sh.zip 1>/dev/null  
        chmod a+x get-fonts.sh  
        ./get-fonts.sh  

* 安装Gdebi  
如果不安装 Gdebi，直接双击下载的 deb 格式软件包，系统会调用 Ubuntu 软件中心安装，这个及其糟糕。当然，不安装 Gdebi，可以使用命令 dpkg 安装 deb 软件包也可以。

        sudo apt-get install gdebi

##软件仓库  

用电脑就是用软件，如果你不能把你的日长生活工作平滑的迁移过来，你最终之后抛弃Ubuntu，再回到Windows中。  
在软件支持上，其实Mac OS比Linux做的好，很多常用软件，例如迅雷、QQ都有官方的Mac版本，并且非常稳定。

* 爱壁纸  
这是一个国产的壁纸查找更换软件，非常好用。  
壁纸库很大，总有适合你风格的壁纸！  
[下载地址](http://www.lovebizhi.com/linux.html)

* 输入法  
自带的输入法对于中文输入已经够用，不过我选择的是fcitx，12.04的fcitx已经集成了google拼音。
  
        sudo apt-get install fcitx  
        im-switch

* 浏览器  
我使用Chrome作为我的主要浏览器。  
在软件中心搜索google-chrome，安装即可。


* QQ  
我尝试了各种不同版本的QQ，例如PyWebQQ、GTKQQ、LinuxQQ、WebQQ3.0他们不是年久失修，就是无法连接。当年官方LinuxQQ一出，各个版本纷纷放弃维护......之后官方就不在更新了。这就是他们的目的么？  
这里有两个选择：
    1. WebQQ  
	简单方便，不需要折腾。  
	[WebQQ]（http://web.qq.com/）  

	2. WineQQ  
	通过Wine来运行QQ，有些不稳定，单可以接受  
	可以参见[这篇Blog](http://www.ubuntusoft.com/wine-qq-2012.html)
  
* 梯子  
我使用Goagent，怎么用，先打开baidu，搜索Google，在Google里搜索Goagent。  
我这里说下怎么让Goagent随Ubuntu系统启动。当然，你要先配置好Goagent。  
打开启动管理器，新建一个叫Goagent的项，在命令里输入：  

        python goagent所在的地址\local\proxy.py


* ubuntu 启动拥有root权限的文件管理器  

        sudo nautilus

* 新立得软件中心  
这东西某种程度上说比ubuntu自带的软件中心好用  

        apt-get install synaptic


* 豆瓣音乐的Banshee插件  
我就这点爱好了。  
	1. 先安装banshee  
	2. 然后豆瓣的插件参见[这篇Blog](https://bitbucket.org/pro711/banshee-doubanfm/wiki/Home)    

* Office
	自带的Office我用着不是很习惯，所以果断换了永中的。  
	[下载地址](http://www.yozosoft.com/person/)  

	当然还有更好的选择金山WPS Office  
	参见[这里](http://linux.wps.cn/)  

* OC  
如果你也用Office Communicator的话  
    
    	//把以下PPA添加到sources里 `sudo gedit /etc/apt/sources.list`  
        deb http://ppa.launchpad.net/aavelar/ppa/ubuntu jaunty main  
        deb-src http://ppa.launchpad.net/aavelar/ppa/ubuntu jaunty main  
	    sudo apt-get update  
        sudo apt-get install pidgin-sipe  

##其他

###如何通过状态栏菜单关机  
现在用户点击右上角的状态栏菜单时会发现，关机选项似乎被隐藏了起来。如果您想通过状态栏菜单关闭您的系统，点击它，然后按下Alt。"待机"选项将立刻变为"关机..."，它将使您能正确地关闭您的系统。  
您也可以安装 "Alternative Status Menu" 扩展。这将在通常状态菜单中的"挂起"选项下新增一个常驻的"关机..."选项。

###其他基于Ubuntu的发行版  

- LinuxDeepin  
这是一个非常好的基于Ubuntu的发行版，特别适合中文用户  
[官方网站](http://www.linuxdeepin.com/)  

- ElementaryOS  
同样基于Ubuntu，我只能说，他太好看了，等Luna版出来，我就会折腾一把
[官方网站](http://elementaryos.org/)  
[介绍](http://wowubuntu.com/elementary-os-2.html)


##参考资料
>[Ubuntu 12.04 配置指南](http://www.bentutu.com/2012/04/ubuntu-12-04-configure-guide.html)  
>[使用ubuntu 12.04 precise手记](http://yishanhe.net/using-ubuntu-precise-pangolin)