---
title: Ubuntu操作
date: 2022-03-22 16:23:31
tags:
---

apt换源
/etc/apt/sources.list
https://mirrors.bfsu.edu.cn/ubuntu

apt和apt-get的区别
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

挂载命令：
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
