---
title: dockerzie学习笔记
date: 2020-09-27 13:35:32
tags: 
    - docker
categories:
    - [docker]
---

## 说明
----
dockerize 是一个小型的 Golang 应用，是一个实用程序，用于简化在 Docker 容器中运行的应用程序，可以简化以下容器管理的过程。

    1. 跟踪任意的日志文件到 STDOUT 和 STDERR。
    2. 在启动时使用模板生成配置文件和容器环境变量。
    3. 在启动主进程之前，等待（探测 ）使用 TCP、HTTP、Unix 的其他服务可用。

dockerize 的典型用例是，当有一个应用程序具有一个或多个配置文件，并且希望使用环境变量控制某些值时。

## 用例
-----

详情见

    https://gitee.com/leoss/vbox/tree/master/centos7  这篇文章`测试dockerize`部分和代码



## 测试dockerize  

参考文档：
  
https://segmentfault.com/a/1190000000728440

测试 dockerize 启动 nginx

路径 source/docker-learn


### 用例1：日志文件内容输出到控制台
 

> docker启动

    `docker logs 无输出`

    docker run -itd -p 80:80 dockerize-test:latest

    将日志文件输出到控制台
    docker run -itd -p 80:80 dockerize-test:dev-output


注：
 查看上一行命令是否执行成功

    if [ $? -ne 0 ]; then
        echo "build unsuccessful"
        exit;
    fi

### 用例2: 在启动时使用模板生成配置文件和容器环境变量

1. 修改Dockerfile中最后一行为：


        CMD dockerize -template /app/source/docker-learn/docker/nginx.tpl:/etc/nginx/nginx.conf -stdout /var/log/nginx/access.log -stderr /var/log/nginx/error.log nginx


2. nginx.tpl中有模板变量：

        # 指定当前启动环境作者
        # {{ .Env.author }}

3. 开始构建版本

    dev-tpl

4. 创建容器

        带上参数 -e "author=wangjiang"

        docker run -itd -p 80:80 --name nginx -e "author=wangjiang" dockerize-test:dev-tpl

5. 进入到容器中 /etc/nginx/nginx.conf

显示  

        # 指定当前启动环境作者
        # wangjiang

### 用例3：等待其他依赖项

使用 docker compose 等工具依赖其他链接容器中的服务是很常见的，但是通常依赖链接是不够的，虽然容器本身可能已经启动，但其中的服务可能还没有准备好。

dockerize 能够在启动应用程序之前在指定协议（FILE、TCP、TCP4、TCP6、HTTP、HTTPS 和 Unix）上等待服务。

[root@localhost ~]# dockerize -wait tcp://db:5432 -wait http://web:80 -wait file:///tmp/generated-file

可以使用 -timeout 参数（默认值 10 秒）指定等待服务可用的时间。如果超时，并且服务仍然不可用，则进程退出，状态代码为 1。

[root@localhost ~]# dockerize -wait tcp://db:5432 -wait http://web:80 -timeout 10s





