---
layout:     post
title:      "在Android中应该如何使用Artifactory本地化仓库"
subtitle:   " \"Gradle、Maven本地化仓库加快下载类库速度\""
date:       2019-08-24
author:     "yubinWong"
header-img: "img/head-bg/artifactory-head.png"
catalog:    true
tags:
    - Android
    - artifactory
---

## 本篇主要讲解如何加载本地仓库，避免翻墙的痛苦，还可以自己编写类库，在公司内部使用

> 我们可以在本地仓库放入我们每次需要编译需要使用的Gradle，每次升级版本避免每个工程师都重复下载一次，可以直接从本地局域网下载速度会快很多

### 首先打开[Artifactory](http://localhost:8081/artifactory/),并登入

![1](/img/android-artifactory-apply/1.png)

### 我们先来看下如何上传Gradle到本地仓库

#### Android Gradle下载及放置位置

![2](/img/android-artifactory-apply/2.png)

- 我们需要做的就是找到gradle压缩包，放置到我们的Artifactory，把gradle下载地址更换成我们的本地下载地址
- 进入到本机的gradle缓存位置，找到你想要上传到仓库的zip

    ```bash
    # 进入到gradle缓存位置
    cd .gradle/wrapper/dists/
    ```

- 或者我们可以直接从[gradle官网](http://services.gradle.org/distributions/)下载自己想要的版本后，上传到仓库

- 我们先在Artifactory创建我们想要的本地仓库名称

    ![3](/img/android-artifactory-apply/3.png)
    ![4](/img/android-artifactory-apply/4.png)
    ![5](/img/android-artifactory-apply/5.png)

- 直接上传gradle-all.zip即可，我们上传前，直接先修改下配置**Admin->General Configuration->File Upload Max Size**，默认是100MB，无法满足我们的需求

- 第一次使用Artifactory，会遇到**401**无授权错误

    ![6](/img/android-artifactory-apply/6.png)

- 我们需要修改下默认设置，**Admin->General Security Settings->Allow Anonymous Access**，还可以针对仓库针对性设置权限**Permissions**，设置之后我们就可以正常下载到gradle文件了,设置权限后假如还是不可以正常使用，可以重启下Artifactory。

#### Android 中我们常用的 google()、 jcenter() 如何从我们本地仓库下载

- 都知道我们版本仓库控制[google](https://dl.google.com/dl/android/maven2/)、[jcenter](https://jcenter.bintray.com/)是从海外下载，我们一般会在之前设置[阿里的仓库地址](https://maven.aliyun.com/mvn/view)，我们有了本地仓库，我们就可以自己仓库的为主，先从我们仓库下载

- 创建remote仓库，放入阿里的仓库地址，直接引用到项目中使用。

    ![7](/img/android-artifactory-apply/7.png)
    ![8](/img/android-artifactory-apply/8.png)
