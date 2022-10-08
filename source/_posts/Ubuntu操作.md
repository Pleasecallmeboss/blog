---
title: Ubuntu操作
date: 2022-03-22 16:23:31
tags:
---



## apt和apt-get的区别
简单来说就是：apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合。
apt 命令	取代的命令	命令的功能
apt install	apt-get install	安装软件包
apt remove	apt-get remove	移除软件包
apt purge	apt-get purge	移除软件包及配置文件
apt update	apt-get update	刷新存储库索引
apt upgrade	apt-get upgrade	升级所有可升级的软件包
apt autoremove	apt-get autoremove	自动删除不需要的包
apt full-upgrade	apt-get dist-upgrade	在升级软件包时自动处理依赖关系
apt search	apt-cache search	搜索应用程序
apt show	apt-cache show	显示安装细节

当然，apt 还有一些自己的命令：
新的apt命令	命令的功能
apt list	列出包含条件的包（已安装，可升级等）
apt edit-sources	编辑源列表

# 挂载命令：
mount [-t vfstype] [-o options] device dir
umount /mnt/cdrom


systemd:

hostnamectl:查看主机名，操作系统，内核，架构
systemd启动的第一个进程
Systemd 默认从目录/etc/systemd/system/读取配置文件。但是，里面存放的大部分文件都是符号链接，指向目录/usr/lib/systemd/system/，真正的配置文件存放在那个目录。

systemctl enable命令用于在上面两个目录之间，建立符号链接关系。


    $ sudo systemctl enable clamd@scan.service
    # 等同于
    $ sudo ln -s '/usr/lib/systemd/system/clamd@scan.service' '/etc/systemd/system/multi-user.target.wants/clamd@scan.service'

如果配置文件里面设置了开机启动，systemctl enable命令相当于激活开机启动。

与之对应的，systemctl disable命令用于在两个目录之间，撤销符号链接关系，相当于撤销开机启动。


    $ sudo systemctl disable clamd@scan.service

配置文件的后缀名，就是该 Unit 的种类，比如sshd.socket。如果省略，Systemd 默认后缀名为.service，所以sshd会被理解成sshd.service。

学习systemd：http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

https://cloud.tencent.com/developer/article/1516125

新建开机服务：https://zhuanlan.zhihu.com/p/396272999


开机自动运行任务的四种方法：

1. 开机启动时自动运行程序

Linux加载后, 它将初始化硬件和设备驱动, 然后运行第一个进程init。init根据配置文件继续引导过程，启动其它进程。通常情况下，修改放置在

/etc/rc或

/etc/rc.d 或

/etc/rc?.d

目录下的脚本文件，可以使init自动启动其它程序。例如：编辑/etc/rc.d/rc.local 文件(该文件通常是系统最后启动的脚本)，在文件最末加上一行“xinit”或“startx”，可以在开机启动后直接进入X－Window。

2. 登录时自动运行程序

用户登录时，bash先自动执行系统管理员建立的全局登录script ：

/ect/profile

然后bash在用户起始目录下按顺序查找三个特殊文件中的一个：

/.bash_profile、

/.bash_login、

/.profile，

但只执行最先找到的一个。因此，只需根据实际需要在上述文件中加入命令就可以实现用户登录时自动运行某些程序（类似于DOS下的Autoexec.bat）。

3. 退出登录时自动运行程序

退出登录时，bash自动执行个人的退出登录脚本

/.bash_logout。

例如，在/.bash_logout中加入命令“tar －cvzf c.source.tgz ＊.c”，则在每次退出登录时自动执行 “tar” 命令备份 ＊.c 文件。

4. 定期自动运行程序

Linux有一个称为crond的守护程序，主要功能是周期性地检查 /var/spool/cron目录下的一组命令文件的内容，并在设定的时间执行这些文件中的命令。用户可以通过crontab 命令来建立、修改、删除这些命令文件。

例如，建立文件crondFile，内容为“00 9 23 Jan ＊ HappyBirthday”，运行“crontabcronFile”命令后，每当元月23日上午9:00系统自动执行“HappyBirthday”的程序（“＊”表示不管当天是星期几）。

设置定时任务：

sudo crontab -e

填入以下内容

//每月1号和15号的4点30分开始更新
30 4 1,15 * * sh [脚本目录]/[脚本名称]

重启crontab，使配置生效

sudo systemctl restart cron.service



github的push操作认证：

https://www.cnblogs.com/sober-orange/p/git-token-push.html



hexo环境上传到github

https://www.cnblogs.com/eidolonw/p/13066869.html

(参考)https://blog.csdn.net/qq_39431829/article/details/88959366



双系统引导过程

https://blog.csdn.net/liao20081228/article/details/82591728



git ssh配置

https://blog.csdn.net/lqlqlq007/article/details/78983879



### 设置root用户密码

sudo passwd

输入的密码为root用户的新密码



## zsh的安装

查看系统当前使用的shell

> echo $SHELL

查看系统安装了哪些shell

> cat /etc/shells

查看zsh的安装位置

> which zsh

把默认的Shell改成zsh（为当前用户）

```bash
chsh -s /bin/zsh
```

安装主题 on-my-zsh

sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

### 安装incr自动补全插件

下载插件

> wget http://mimosa-pudica.net/src/incr-0.2.zsh

移动插件到oh-my-zsh目录的插件库下

> sudo mkdir ~/.oh-my-zsh/plugins/incr/
>
> sudo mv incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/incr-0.2.zsh

进入vim编辑

> sudo vim ~/.zshrc

在vim界面按i进入输入模式在末尾加上如下代码

> source ~/.oh-my-zsh/plugins/incr/incr*.zsh

按esc进入一般模式输入以下内容后回车以保存退出

> ：qw

更新配置

> source ~/.zshrc

### 更换agnoster主题

安装字体

> sudo apt install fonts-powerline

进入vim编辑

> sudo vim ~/.zshrc

在vim界面按i进入输入模式找到ZSH_THEME="robbyrussell"将其中robbyrussell修改为agnoster

按esc进入一般模式输入：qw回车保存退出

更新配置

> source ~/.zshrc

### 其他问题 git卡顿

https://blog.csdn.net/anlz729/article/details/108768918



## 验证代理是否正常

#### 测试http

curl 'ip111.cn'

curl -s http://myip.ipip.net

#### 测试https

curl ‘https://api.myip.la/cn?json'

curl 'https://ipinfo.io/'



### netstat的使用

https://www.cnblogs.com/ggjucheng/archive/2012/01/08/2316661.html



## 远程桌面连接

vnc方式：https://blog.csdn.net/langyou0/article/details/107959002



## df命令

df

df -a 显示全部文件系统

df -h 单位以人类易于阅读的方式

df -T 显示文件系统类型



## 查看cpu

lscpu



## 查看内存

free



## 安装KDE桌面环境

https://blog.csdn.net/weixin_39670511/article/details/110594726

https://ubuntuqa.com/article/9604.html



## 命令行配置无线有线网络

https://www.cnblogs.com/luotingliang/p/7248672.html



## PS命令

tty[1-6]就是你用ctr+alt+f[1-6]所看到的那个终端; 即虚拟控制台。其他的是外部终端和网络终端。

pts/*为伪(虚拟)终端，当前打开了两个终端窗口，所以就有pts/0和pts/1

ps -a 显示现行终端机下的所有进程，包括其他用户的进程

-A 所有的进程均显示出来，与 -e 具有同样的效用

-l 详细的

-u 以



## 静态IP设置

https://www.itbulu.com/ubuntu-setting-ips.html



## source not found

https://www.cnblogs.com/erlou96/p/13398387.html



## 设置别名

1.vim ~/.bashrc

2.添加 alias name='command'即可。重启后即可生效,或执行source .bashrc也可立即生效



## 脚本编写

https://www.cnblogs.com/zhang-jun-jie/p/9266858.html



## 局域网主机之间测速

https://jingyan.baidu.com/article/25648fc16e8c9d9191fd00a0.html



## 实时测速

添加在上方图标 

https://www.cnblogs.com/jsdy/p/11461277.html

命令行 slurm

https://blog.csdn.net/zhengfushijie/article/details/49050607?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&utm_relevant_index=1



## 开机自动挂载u盘

[UUID=************] [挂载磁盘分区] [挂载磁盘格式] 0 2 

添加的行是13行，UUID和挂载目录/home/sqp/Data以及硬盘格式ext4

第一数字0，0是开机不检查磁盘，1是开机检查磁盘

第二个数2，0表示交换分区，1表示启动分区，2表示普通分区 



## script编写

### echo(选项)(参数)选项

-e：激活转义字符。使用-e选项时，若字符串中出现以下字符，则特别加以处理，而不会将它当成一般文字输出：

•\a 发出警告声；



### 指令运作的顺序： 

1. 以相对/绝对路径执行指令，例如『 /bin/ls 』或『 ./ls 』； 
2. 由 alias 找到该指令来执行； 
3. 由 bash 内建的 (builtin) 指令来执行； 
4. 透过 $PATH 这个变量的顺序搜寻到的第一个指令来执行。



### 快速删除和光标快速移动

[ctrl]+u/[ctrl]+k 分别是从光标处向前删除指令串 ([ctrl]+u) 及向后删除指令串 ([ctrl]+k)。 [ctrl]+a/[ctrl]+e 分别是让光标移动到整个指令串的最前面 ([ctrl]+a) 或最后面 ([ctrl]+e)



### 单引号和双引号的区别

变量内容若有空格符可使用双引号『"』或单引号『'』将变量内容结合起来，

但 o双引号内的特殊字符如 $ 等，可以保有原本的特性，如下所示： 

『var="lang is $LANG"』则『echo $var』可得『lang is zh_TW.UTF-8』 

单引号内的特殊字符则仅为一般字符 (纯文本)，如下所示： 

『var='lang is $LANG'』则『echo $var』可得『lang is $LANG』



### 追加变量内容

『PATH="$PATH":/home/bin』或『PATH=${PATH}:/home/bin』



### 使用其他指令提供的信息

『version=$(uname -r)』再『echo $version』可得『3.10.0-229.el7.x86_64』



### 仓库详解

整个软件源结构可以分解为四个部分:

第一部分	第二部分	第三部分	第四部分
软件包格式	软件包服务器地址	发行版版本代号	软件包的分类目录
deb/deb-src	http://mirrors.aliyun.com/ubuntu/ 	xenial/xenial-updates/xenial-security/xenial-backports/proposed	main、restricted、universe、multiverse
第一部分的deb是deb软件包，deb-src则是源代码包

第三部分严格来说不算是发行版版本代号，它应该是Ubuntu系统发布之后，在此基础上进行的安全性更新的分类。

第四部分是按照软件包的自由度来分类的：

main：即“基本”组件，其中只包含符合Ubuntu的协议要求并由Ubuntu团队维护支持的软件。

restricted：即“受限”组件，其中包含了非常重要的，但并不具有合适的自由协议的软件，如显卡驱动，同样有 Ubuntu团队维护支持。

universe：即“社区维护”组件，其中包含的软件种类繁多，它们可能采用受限于协议，可能不是，但都不为Ubuntu 团队维护。

multiverse：即“非自由”组件，其中包括了不符合自由软体要求而且不被Ubuntu团队支援的软件，通常为商业公司编写的软件。

下面我们来看一下Ubuntu软件源镜像站的目录结构（以阿里云镜像站为例）：

http://mirrors.aliyun.com/ubuntu/ ，在浏览器地址栏中输入此地址便进入了Ubuntu软件源镜像站，如下图所示：



重点看两个文件夹dists和pool

dists目录包含的全是Ubuntu发行版目录及其附加仓库目录（如：xenial、xenial-update、xenial-security、xenial-backports就是Ubuntu xenial发行版目录及其附加仓库目录）。

pool/:

所有 Ubuntu 发布版及已发布版的软件包的物理地址。按照源码包名称分类存放。pool目录下按属性再分类（main、restricted、 universe和multiverse），分类下面再按源码包名称的首字母归档。这些目录包含的文件有：运行于各种系统架构的二进制软件包，生成这些二进制软件包的源码包。  

我们知道Ubuntu还有其他的附加仓库，Ubuntu附加仓库的命名格式是“版本代号-限定词”，限定词是这update、security、proposed、backports四个词中的一个，比方说版本代号xenial和限定词update组合就是xenial-update附加仓库，xenial和security组合就是xenial-security附加仓库，以此类推可以自行写出Ubuntu所有的附加仓库的目录名称

在sources.list文件里只有一条包含发行版仓库xenial的软件源还不够，我们还要写出包含其他4个附加仓库的软件源，只要把已经写好的软件源中的xenial依次替换成xenial-update、xenial-security、xenial-proposed、xenial-backports即可，下面是完整的包含所有附加仓库的软件源：

deb http://mirrors.aliyun.com/ubuntu/ xenial-update main universe restricted multiverse

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main universe restricted multiverse

deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main universe restricted multiverse

deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main universe restricted multiverse

将这四条软件源再一并写入sources.list，再加上

deb http://mirrors.aliyun.com/ubuntu/ xenial main universe restricted multiverse

总共五条

另外为了防止运营商劫持大家可以使用https，但是要求镜像站支持https，一般现在大型镜像站都是支持https的，比如清华镜像站，阿里镜像站，163等等

### 版本号

版本	开发代号	中译	发布日期	支持结束时间	 内核版本
桌面版	服务器版
4.10	Warty Warthog	多疣的 疣猪	2004-10-20	2006-04-30	2.6.8
5.04	Hoary Hedgehog	白发的 刺猬	2005-04-08	2006-10-31	2.6.10
5.10	Breezy Badger	活泼的 獾	2005-10-13	2007-04-13	2.6.12
6.06 LTS	Dapper Drake	整洁的公 鸭	2006-06-01	2009-07-14	2011-06-01	2.6.15
6.10	Edgy Eft	尖利的小 蜥蜴	2006-10-26	2008-04-25	2.6.17
7.04	Feisty Fawn	烦躁不安的 鹿	2007-04-19	2008-10-19	2.6.20
7.10	Gutsy Gibbon	胆大的 长臂猿	2007-10-18	2009-04-18	2.6.22
8.04 LTS	Hardy Heron	坚强的 鹭	2008-04-24	2011-05-12	2013-05-09	2.6.24
8.10	Intrepid Ibex	无畏的 羱羊	2008-10-30	2010-04-30	2.6.27
9.04	Jaunty Jackalope	活泼的 鹿角兔	2009-04-23	2010-10-23	2.6.28
9.10	Karmic Koala	幸运的 树袋熊	2009-10-29	2011-04-30	2.6.31
10.04 LTS	Lucid Lynx	清醒的 山猫	2010-04-29	2013-05-09	2015-04-30	2.6.32
10.10	Maverick Meerkat	标新立异的 狐獴	2010-10-10	2012-04-10	2.6.35
11.04	Natty Narwhal	敏捷的 独角鲸	2011-04-28	2012-10-28	2.6.38
11.10	Oneiric Ocelot	有梦的 虎猫	2011-10-13	2013-05-09	3.0
12.04 LTS	Precise Pangolin	精准的 穿山甲	2012-04-26 [39]	2017-04-28 [40]	3.2 [41]
12.10	Quantal Quetzal	量子的 格查尔鸟	2012-10-18	2014-05-16 [42]	3.5 [43]
13.04	Raring Ringtail	铆足了劲的 环尾猫熊	2013-04-25	2014-01-27 [44]	3.8 [45]
13.10	Saucy Salamander	活泼的 蝾螈	2013-10-17 [46]	2014-07-17 [47]	3.11
14.04 LTS	Trusty Tahr	可靠的 塔尔羊	2014-04-17 [48]	2019-04	3.13
14.10	Utopic Unicorn	乌托邦的 独角兽	2014-10-23 [49]	2015-07-23 [50]	3.16 [51]
15.04	Vivid Vervet	活泼的 长尾黑颚猴	2015-04-23 [52]	2016-02-04 [53]	3.19 [54]
15.10	Wily Werewolf	老谋深算的 狼人	2015-10-22 [55]	2016-07-28 [56]	4.2 [57]
16.04 LTS	Xenial Xerus	好客的 非洲地松鼠	2016-04-21 [58]	2021-04	4.4 [59]
16.10	Yakkety Yak	喋喋不休的 牦牛	2016-10-13 [60]	2017-07-20	4.8
17.04	Zesty Zapus	热情的 美洲林跳鼠	2017-04-13 [61]	2018-01-13	4.10 [62]
17.10	Artful Aardvark	巧妙的 土豚	2017-10-19 [63]	2018-07-19	4.13 [64]
18.04 LTS	Bionic Beaver [65] [66]	仿生的 海狸	2018-04-26 [67]	2028-04 [68]	4.15
18.10	Cosmic Cuttlefish	宇宙的 墨鱼	2018-10-18 [69]	2019-07	4.18 [70]

19.04	Disco Dingo	迪斯可的 澳洲野犬	2019-04-18 [71]	2020-01	TBA
-----------------------------------



## ROS安装

1. ![img](https://img-blog.csdnimg.cn/20191118143433409.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDUwODEx,size_16,color_FFFFFF,t_70)

2.  设置软件源

```sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'```

3. 设置密钥

   ```sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F42ED6FBAB17C654```

4. 安装

   > sudo apt-get update
   > sudo apt-get install ros-melodic-desktop-full
   > sudo apt-get install ros-melodic-rqt*

5. 初始化rosdep

   > sudo apt-get install python-rosdep
   >
   > sudo rosdep init
   > rosdep update

6. 配置环境变量

   

   > source /opt/ros/melodic/setup.bash
   >
   > #ifconfig查看你的电脑ip地址
   > export ROS_HOSTNAME=localhost
   > export ROS_MASTER_URI=http://${ROS_HOSTNAME}:11311 

# 设置代理

> export http_proxy="http://<user>:<password>@<proxy_server>:<port>"
> export https_proxy="http://<user>:<password>@<proxy_server>:<port>"
> export http_proxy="socks5://127.0.0.1:1080"
> export https_proxy="socks5://127.0.0.1:1080"
> export ftp_proxy=http://<user>:<password>@<proxy_server>:<port>

# 更改文件夹下所有文件的权限

> sudo chmod -R 777 filename

# git常用操作

> $ git config --global user.name "username"
>
> $ git config --global user.email "email"
>
> 用vscode多方便

# 中文输入法
ubuntu 在最新的版本中已经可以不用用户自己单独去下载中文输入法使用了，本次使用为 ubuntu18.04LTS版本(登陆是界面选择的是ubuntu on wayland)，设置方式非常简单

1、打开设置，不知道的请点击右上角的工具栏即可看到。

2、找到设置中语言项，点击语言安装管理，安装中文语言后选择输入方式。


点击关闭，然后添加输入语言，在其中找到中文拼音添加即可


https://blog.csdn.net/yedongsheng8238045/article/details/107185130/

# eigen库路径问题
大多数的程序包含的都是 <Eigen/Dense> 头文件, 如果想这样包含的话需要将 eigen 下的 Eigen 文件提升一级目录, 即将 Eigen 从 /usr/include/eigen3 放到 /usr/include/ 下, 如下命令:

sudo cp -r  /usr/include/eigen3/Eigen /usr/include/


# eigen:vector3d
Eigen::Vector3d默认定义为一个列向量

Eigen::Vector3d T(2, 1, 6)
std::cout << "T = \n" << T << std::endl;
std::cout << "T.transpose() = \n" << T.transpose() << std::endl;
1
2
3
输出结果为：

T = 
2
1
6
T.transpose() = 
2  1  6

# eigen::matrix
直接输出


# man zh_cn
1.安装中文man手册
sudo apt-get install manpages-zh
2.查看中文man手册安装路径
dpkg -L manpages-zh | less

![](https://gitee.com/Pleasecallmeboss/tuchuang/raw/master/Note/man位置.png)
 

可见中文man手册是安装在路径/usr/share/man/zh_CN/下

3.给中文man设置一个命令
为了和系统原来的man区分开，用alias给中文man的命令设置一个别名

alias cman='man -M /usr/share/man/zh_CN'
为永久生效，可把上面的命令写进启动文件中
如：修改 ~/.bashrc ，添加上面的命令
我修改的是 /etc/bash.bashrc
