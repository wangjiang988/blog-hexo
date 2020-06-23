---
title: centos7 virtualbox 安装后的静态IP设置
date: 2020-06-23 13:10:17
tags: 
    - centos
    - 运维
categories:
    - [it]
    - [devops]    
---

## 说明

virtualbox 安装虚拟机centos7后，设置静态IP过程

## 步骤

1. 安装centos

略

2. 关掉虚拟机 设置虚拟机

在virtualbox中设置两个网卡，第一个网卡设置net， 第二个网卡设置桥接

3. 开启虚拟机

进入 /etc/sysconfig/network-scripts/

    cp ifcfg-enp0s3 ifcfg-enp0s8
    vi ifcfg-enp0s8

生成新uuid

    uuidgen

生成的新uuid 替换掉ifcfg-enp0s8 里边的uuid

新的配置文件如下


    TYPE="Ethernet"
    PROXY_METHOD="none"
    BROWSER_ONLY="no"
    #BOOTPROTO="dhcp"
    DEFROUTE="yes"
    IPV4_FAILURE_FATAL="no"
    IPV6INIT="yes"
    IPV6_AUTOCONF="yes"
    IPV6_DEFROUTE="yes"
    IPV6_FAILURE_FATAL="no"
    IPV6_ADDR_GEN_MODE="stable-privacy"
    NAME="enp0s8"
    UUID="61e1757f-8d77-4c56-a9d4-8792deac1bdb"
    DEVICE="enp0s8"
    #ONBOOT="yes"

    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.1.190

结束
