---
title: 微服务图片存储问题解决
date: 2020-06-23 13:39:34
tags:
    - java
    - 微服务
categories:
    - [it]
    - [work]
---

## 说明

> 微服务的图片保存问题，如果不用fastdfs这种方案，那么需要本地存储


在docker swarm 部署方案中

有文件上传的那个微服务做路径映射，且该微服务的节点需要放到同一台机器上

通过docker node 给节点打标签解决

> 如下

    1. 添加label

    docker node update --label-add [标签] [节点名]
    docker node update --label-rm [标签] [节点名]
    docker node inspect [节点名]

    2. 编排脚本 docker-compose.yml中

    （略...）
        deploy:
        placement:
            constraints: [node.labels.标签==值]
    （略...）



然后最终的微服务配置文件如下

    version: "3.2"

    services:
    
    ....
    
    # 改服务有文件上传
    platform:
        image: registry.cn-hangzhou.aliyuncs.com/w-cloud/platform-microservice:docker-wj-20200622-v1
        networks:
        - cloud
        environment:
        - mysqlIp=192.168.1.115 # 参数可以被application.yml 中使用 使用方法为 ${mysqlIp}
        - mysqlPort=3306 # 参数可以被application.yml 中使用 使用方法为 ${mysqlPort}
        volumes:
        - ./uploads/:/uploads/
        deploy:
        mode: replicated
        replicas: 2
        placement:
            constraints: [node.labels.func==platform] # 指定服务运行位置
        
        ....

    networks:
    cloud:

