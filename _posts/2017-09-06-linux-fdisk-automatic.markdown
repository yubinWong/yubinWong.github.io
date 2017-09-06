---
layout:     post
title:      "fdisk分区自动挂载"
subtitle:   " \"自动挂载\""
date:       2017-09-06
author:     "yubinWong"
header-img: "img/head-bg/git-linux.jpg"
catalog:    true
tags:
    - Linux
    - fdisk
---

>fdisk分区自动挂载,fdisk分区方法可以查看上篇文章

## 分区自动挂载
> 原理其实就是写入/etc/fstab文件,开机之后自动执行配置文件

- #### 理解/etc/fstab文件配置
1. 首先打开这个文件我们查看下本身内容  
`vi /etc/fstab`   或者   `vim /etc/fstab`  
![查看fstab](/img/linux-fdisk-automatic/1.png)  

## 介绍下fstab配置
> 文件配置每一行属于一个配置,每个配置分成6块,下面详细介绍每一块的含义  

- 第一块:分区设备名或者**UUID**<UUID是通用的唯一标识码>建议使用UUID作为配置,分区设备名假如因为您重新设置分区顺序或者添加分区之后导致不一致的话,需要再次编辑,造成一些麻烦!  

查看方法UUID `dumpe2fs 分区文件名` 或者 `dumpe2fs -h 分区文件名`只查看当前文件
![查看UUID](/img/linux-fdisk-automatic/2.png)  

- 第二块:挂载点,也就是说需要挂载到哪个目录,这个目录**必须存在**,当然比如在一些系统里面会帮您自动创建(例如:centos7),还是建议大家自己先确保文件夹存在
- 第三块:文件系统名称,看当时格式化的时候选择的是哪种类型
- 第四块:挂载参数,我们可以选择默认的
- 第五块:指定分区是否备份,0:不备份,1:每天备份,2:不定期备份,自动备份到该分区的lost+found目录下
- 第六块:指定分区是否被fsck检测,0:不检测,其他数字代表优先级,1的优先级大于2 代表系统先检测哪个,设置优先级一般我们不设置1,1的优先级最高,一般只有根分区设为1

## 配置fstab
1. 示例图如下:
![配置fstab](/img/linux-fdisk-automatic/3.png)  

2. 配置好后,保存退出,每次开机,都会自动挂载您设置的分区
3. 为了保险我们先要检测我们设置的是否正确,可以执行`mount -a`命令<fstab文件重新挂载	>,判断我们是否配置正确

## fstab配置错误,解决方案
1. 演示配置错误,把
![配置fstab](/img/linux-fdisk-automatic/4.png)  
2. 重启Linux
3. 出现错误,系统提示说下root密码,或者按Ctrl+D重新启动
![配置fstab](/img/linux-fdisk-automatic/5.png)  
4. 我们输入root管理员密码,然后我们就可以正常操作修改fstab
5. 假如vim提示是只读文件,即使强制保存也没办法的时候,是因为我们挂载的时候是只读权限,需要重新挂载成可以读写就可以正常操作啦
6. 执行命令 `mount -o remount,rw /` 重新挂载读写权限,修改正确的fstab

- # 总结  
修改fstab之后一定要执行`mount -a`查看是否编辑正确,因为上面的修复方法只能修复部分问题噢~