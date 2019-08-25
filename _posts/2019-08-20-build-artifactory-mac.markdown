---
layout:     post
title:      "mac搭建本地化仓库artifactory"
subtitle:   " \"Gradle、Maven 本地化仓库Artifactory\""
date:       2019-08-20
author:     "yubinWong"
header-img: "img/head-bg/artifactory-head.png"
catalog:    true
tags:
    - Android
    - artifactory
---

## 安装和运行 Artifactory，需要先安装JDK 8

```bash
# 查看本地Java版本
java -version
```

### 配置 JAVA_HOME

vim打开配置文件  **.bash_profile**，配置如下环境

```bash
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_161.jdk/Contents/Home/
CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=$JAVA_HOME/bin:$PATH:
export JAVA_HOME
export CLASSPATH
export PATH
```

## mac系统不支持服务安装，只能自己手动安装

1. [下载 Artifactory](https://jfrog.com/open-source/#artifactory)，解压到自己想要安装的位置

2. 找到安装位置进入到**bin**目录
3. 配置Artifactory启动文件，**artifactory.default**
4. 具体配置如下，改成自己实际安装位置即可：

    ```bash
    #!/bin/sh

    #Default values
    export ARTIFACTORY_HOME=/Applications/artifactory-oss-6.12.0
    export ARTIFACTORY_USER=artifactory
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_161.jdk/Contents/Home
    export START_LOCAL_REPLICATOR=true

    #export START_LOCAL_MDS=true
    #export START_LOCAL_ROUTER=true

    export TOMCAT_HOME=$ARTIFACTORY_HOME/tomcat
    export ARTIFACTORY_PID=$ARTIFACTORY_HOME/run/artifactory.pid

    export JAVA_OPTIONS="-server -Xms512m -Xmx4g -Xss256k -XX:+UseG1GC -XX:OnOutOfMemoryError=\"kill -9 %p\""
    export JAVA_OPTIONS="$JAVA_OPTIONS -Djruby.compile.invokedynamic=false -Dfile.encoding=UTF8 -Dartdist=zip -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true -Djava.security.egd=file:/dev/./urandom"
    # Timeout waiting for artifactory to start
    # START_TMO=60
    ```

5. 在**bin**目录下，有安装脚本，执行**installService.sh**
6. 安装好后，接下来直接运行可以访问使用

    ```bash
    # 直接运行
    sudo ./artifactory.sh
    # 以服务的方式运行
    sudo ./artifactoryctl start
    # 显示当前服务状态
    sudo ./artifactoryctl check
    # 停止服务
    sudo ./artifactoryctl stop
    ```

7. 浏览器打开[http://localhost:8081/artifactory](http://localhost:8081/artifactory),可以直接访问本地仓库，默认账户：**admin**，密码：**password**
8. 本地化仓库打开如下图：
![图](/img/build-artifactory-mac/artifactory_start.png)

## 其他系统安装Artifactory，可以直接参考[官网文档](https://www.jfrog.com/confluence/display/RTF/Installing+on+Linux+Solaris+or+Mac+OS#InstallingonLinuxSolarisorMacOS-SettingJavaMemoryParameters)