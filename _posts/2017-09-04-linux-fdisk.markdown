---
layout:     post
title:      "fdisk分区"
subtitle:   " \"正确分区方法\""
date:       2017-09-04
author:     "yubinWong"
header-img: "img/head-bg/git-linux.jpg"
catalog:    true
tags:
    - Linux
    - fdisk
---

>fdisk手动分区-本文使用的是虚拟机VMware


## fdisk分区过程

- #### 手动添加硬盘
1. 手动添加硬盘(关闭计算机)
2. 点击添加硬盘(记得选择SCSI硬盘类型)
3. 选择要添加硬盘大小
4. 默认选择下一步即可

>示例图如下:

![选择虚拟硬盘](/img/linux-fdisk/1.png)

![选择虚拟硬盘](/img/linux-fdisk/2.png)

![选择虚拟硬盘](/img/linux-fdisk/3.png)

![选择虚拟硬盘](/img/linux-fdisk/4.png)

- #### 手动分区  
>虚拟机开机  

1. 命令查询硬盘是否被识别了
`fdisk -l` 
>如下图
![查看硬盘是否被识别](/img/linux-fdisk/5.png)  

2. 使用fdisk命令分区  
`fdisk /dev/sdb`
![执行分区命令](/img/linux-fdisk/6.png) 
 
3. 常用命令注解  
a 设置可引导标记  
b 编辑bsd磁盘标签  
c 设置DOS操作系统兼容标记  
d 删除一个分区**(常用)**  
l 显示已知的文件系统类型. 82为swap分区,83为标准分区**(常用)**  
m 显示帮助  
n 新建分区**(常用)**  
o 建立空白DOS分区表  
p 显示分区列表  
q 不保存退出
s 新建空白SUN磁盘标签  
t 改变一个分区的系统ID**(常用)**  
u 改变显示记录单位  
v 验证分区表  
w 保存退出  
x 附加功能

4. 按n新建分区 >如下图
![新建分区](/img/linux-fdisk/7.png)  
p 主分区(主分区最多只能有4个)  
e 扩展分区(扩展分区+主分区只能有4个,扩展分区只能有1个,逻辑分区只能在扩展分区建立之后才能建立)

5. 建立主分区(自己分配选择的大小,ID号最好默认1开始逐个添加) >如下图
![新建主分区](/img/linux-fdisk/8.png) 

6. 建立一个扩展分区
![新建扩展分区](/img/linux-fdisk/9.png)

7. 有了扩展分区之后,在新建分区就可以出现逻辑分区l
![新建逻辑分区](/img/linux-fdisk/10.png)

8. w 保存退出,假如被占用保存失败,可以重启,但是重启麻烦,可以使用命令`partprobe` 重新读取分区表信息

- #### 格式化分区
1. 格式化分区(我们刚刚建的主分区及逻辑分区)扩展分区不能格式化,也不能写入数据  
`mkfs -t ext4 /dev/sdb1`  
`mkfs -t ext4 /dev/sdb5`  
![格式化分区](/img/linux-fdisk/11.png)

- #### 手动挂载
1. 挂载需要空的文件夹,先建立  
`mkdir /disk1`
`mkdir /disk5`
2. 执行挂载  
`mount /dev/sdb1 /disk1`  
`mount /dev/sdb5 /disk5`  
3. 查看下是否挂载成功  
`mount` 或者 `df`
![挂载mount](/img/linux-fdisk/12.png)
![查看挂载df](/img/linux-fdisk/13.png)

- # 总结  
这种挂载方式重启之后需要每次重新执行挂载命令  
下一篇我们讲解自动挂载