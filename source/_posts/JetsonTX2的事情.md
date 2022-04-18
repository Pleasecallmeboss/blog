---
title: JetsonTX2的事情
date: 2022-03-23 19:38:04
tags:
---

## APT换源

注意使用hostnamectl查看系统架构是arm64

使用阿里云\清华大学\北京外国语大学的ubuntu ports开源软件镜像站都存在问题

使用中科大镜像站即可



## 控制风扇转速

在任意路径下，输入：

sudo nvpmodel --query
显示工作在m2模式下，即15w模式，此时风扇默认不开，此时输入：
sudo jetson_clocks
会听到风扇转起来了，声音很大，再次查看当前Xavier的工作模式：
sudo nvpmodel --query
输出m0模式，也就是全速模式。

查看及更改风扇转速文件（要失败的话，重启试下）：
cat /sys/devices/pwm-fan/target_pwm
正常情况下会显示 0，此时风扇不转。如果用了上文的全速模式jetson_clocks会显示255。

我们尝试更改一下数字：
sudo gedit /sys/devices/pwm-fan/target_pwm
将转速改为200，保存，转速立即发生变化。重启后失效。



### 温度检测

安装硬件温度检测工具sensors。sudo apt install lm-sensors
安装成功以后，输入：
sensors
系统会显示当前温度，一般不开风扇的话系统温度能达到40度左右。



## 查看L4T版本

```
head -n 1 /etc/nv_tegra_release
```



## 重装系统

### 使用命令：

https://blog.csdn.net/greepex/article/details/103876921

先下载文件系统

进入Linux_for_Tegra目录，执行一次（以后要用就不执行了）`sudo ./apply_binaries.sh`

查看是否处于强制恢复模式lsusb

看看有没有0955：7c18的Nvidia crop

烧写到emmc的方式`sudo ./flash.sh jetson-tx2 mmcblk0p1`



## TX2的不同模式及切换

https://blog.csdn.net/xxradon/article/details/87932441



https://www.cnblogs.com/luotingliang/p/7248672.html



## TX2控制界面 desktop sharing打不开

https://blog.csdn.net/qq_38129331/article/details/107859137



## TX2远程桌面

https://blog.csdn.net/jiangchao3392/article/details/73252291



## 将固态作为启动盘

1. 准备 TX2 系统 准备一个有系统 TX2 设备，该系统版本要和 host pc 机上烧录环境的系统版本要一致。查看 TX2 系统 版 本 $ head –n 1 /etc/nv_tegra_release 如 果 host pc 上 未 搭 建 过 烧 录 环 境 ， 参 考 https://www.realtimes.cn/cn/help.html 帮助文档-《TX2 系统烧录说明》搭建烧录环境。 该文档以使用 9002u 载板为例，50GB mSATA SSD。 

2. 检查 SSD 设备名称 把 mSATASSD 插到设备上的 mSATA 接口上。使用 sudo fdisk -l 查看 ssd 设备名称。比如：sda，本文 档中之后 ssd 设备名都以 sda 为例。 
3.  创建一个新的 GPT $ sudo parted /dev/sda mklabel gpt
4.  添加分区 $ sudo parted /dev/sda mkpart primary 0GB  Size 是分区的大小，最小 8GB，建议 32GB 以上 例如：准备分区大小为 40GB $ sudo parted /dev/sda mkpart primary 0GB 40GB 添加完分区后，使用 sudo fdisk -l 可以看到 sda 新增一个分区，比如 sda1。 
5. 格式化分区 $ sudo mkfs.ext4 /dev/sda1 把分区格式化为 ext4 格式 
6.  查看分区的 UUID $sudo blkid /dev/sda1 例如：/dev/sda1: UUID="7b568e36-831e-4975-babe-6d2eaf585aa4" TYPE="ext4" PARTLABEL="primary"  PARTUUID="4f71e1b5-70b5-4582-8a1e-51ef993b3bd0" 
7. 保存 PARTUUID 的值 把 PARTUUID 的值保存到临时存储设备上，u 盘或者其他方式保存，以备后续使用。 
8. 拷贝根文件系统到 sdd 分区上 $ sudo dd if=/dev/mmcblk0p1 of=/dev/sda1 
9.  调整分区大小 1)卸载磁盘：umount /media/xxx(路径) 2)$ e2fsck -f /dev/sda1 3)$ sudo resize2fs /dev/sda1 
10. 向 l4t-rootfs-uuid.txt 写入 PARTUUID 的值 在 host pc 机上切换到烧录环境 Linux_for_Tegra 目录 $ echo ‘4f71e1b5-70b5-4582-8a1e-51ef993b3bd0’ > /bootloader/l4t-rootfs-uuid.txt 
11. 向 TX2 设备烧写一个从外部设备启动的系统 在 host pc 机 以使用 RTSO-9002u 载板为例 使 TX2 设备进入 recovery 模式 $sudo ./flash.sh realtimes/rtso-9002u external 
12. 查看是否从 SSD 中启动系统 在 TX2 系统启动后 $ df -h  显示 sda1 已经成为根目录以及文件系统大



### parted提示更新/etc/fstab 文件内容

添加固态的挂载到/media/nvidia(新建即可)

### dd命令速度太慢

以后应该使用 `pv` 来获取运行进度条。

```
sudo apt-get install pv
```

安装 `pv` 后，假设您要将 20GB 驱动器 `/dev/foo` 克隆到另一个驱动器(20GB 或更大！)， `/dev/baz` ：

```
sudo dd if=/dev/foo bs=4M | pv -s 20G | sudo dd of=/dev/baz bs=4M
```

需要注意的重要部分：`bs=4M` 参数将 dd 操作的块大小设置为 4MB，这大大提高了整个过程的速度。而 `-s 20G` 参数告诉 `pv` 这个操作预计有多大，所以它可以给你一个 ETA 以及当前速度。

我非常喜欢 `pv`，它应该是非法的。

请注意，虽然这样做从左到右的顺序是直观、美观和整洁的，但如果您谈论的是真正快速的数据流，则传入和传出 STDOUT 的管道可能会导致性能损失。如果您正在考虑移动数百 MB/秒，以下语法会更快：

```
pv -s 20G < /dev/foo > /dev/baz
```

`-s 20G` 是可选的，如果您确实知道流有多大(或大约有多大)，它允许 `pv` 为您提供完成时间的估计。如果没有这个，`pv` 将尝试找出数据集有多大(例如，它知道文件有多大)，但如果不能(例如，使用块设备，而不是文件)，它只会告诉你无需猜测事情将需要多长时间的传输速率。



## TX2安装ROS

https://www.guyuehome.com/10082



## 安装ros wrapper

https://github.com/IntelRealSense/realsense-ros官方git仓库

https://www.freesion.com/article/7219741032/ 不行

https://blog.csdn.net/qq_41839222/article/details/86503113 详细 和其他vins-momo之类的

### 在根目录创建工作空间后catkin_init_workspace找不到命令

sudo apt-get install python-catkin-tools

sudo su
echo "source /opt/ros/kinetic/setup.bash" >>/etc/profile

最开始我们都在.bashrc中配置过环境变量，这里也要改。/etc/profile是配置所有用户的环境变量



### Project ‘cv_bridge’ specifies ‘/usr/include/[opencv](https://so.csdn.net/so/search?q=opencv&spm=1001.2101.3001.7020)’ as an include dir, which is not found.

将cmake文件100行左右的两个opencv改为opencv4

这不是因为我们编写的程序有误，而是NVIDIA的32.3.1image自带的是opencv4版本，把opencv命名成了opencv4

修改方法：只需修改上述路径中的cv_bridgeconfig.cmke文件，将100行附近的两个opencv改成opencv4即可（注意，只需要改动单独一个的opencv,前面有连字符的opencv不需要改动）

注意：如果jetson nano的版本自带的opencv是3版本，可能不会出现此问题



## Frames didn’t arrived within 5 seconds

### 或者SDK中点击启动后会报错：device or resource is not available之类的

这其实是因为相机中的固件(firmware)需要更新版本。
连上相机，按照以下链接更新固件部分操作即可：
http://www.pianshen.com/article/6519141340/



### 报错cmake的问题或者和ddynamic_reconfigure有关

是缺少安装ddynamic_reconfigure的包,所以要再安装一下,打开链接https://github.com/pal-robotics/ddynamic_reconfigure/tree/kinetic-devel,直接将这个仓库下载到工作空间src中,也就是用git clone命令即可



### ros环境复习

https://zhuanlan.zhihu.com/p/94583753



## 安装opencv

一行命令

https://blog.csdn.net/weixin_45885232/article/details/108994685

sudo apt install python3-opencv



## 固态硬盘接口

https://blog.csdn.net/shuai0845/article/details/98330290



## ROS主从机

https://blog.csdn.net/FSKEps/article/details/106731579
