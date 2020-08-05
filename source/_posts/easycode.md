---
title: easycode介绍及使用
date: 2020-05-11 20:53:33
tags:
    - easycode
categories:
    - [it]
    - [work]
---

## 说明

easycode 是我业余时间开发的一款代码生成工具。

一开始主要是想看看vue-cli的脚手架写法，后来我发现node非常适合写这种项目的脚手架，且网上教程也很多，于是便萌生了写下一款代替CV大法的工具。

<!-- more -->
## 项目地址

npmjs : https://www.npmjs.com/package/@wangjiang988/easycode

github: https://github.com/wangjiang988/easycode

## 使用说明

### 1. 初始化配置文件 easycode.yml

    easy config init

![image](/img/easycode/easy_init_2.png)

执行成功后，在当前目录下回生成

    .
    ..
    easycode.yml
    templates/
    
    项目其他文件

### 2. 重新加载文件 

    easy config refresh

### 3. 代码生成

    easy make {name} 

这里的name是我们的easycode.yml中的模板名：

> 单文件模板名

![image](/img/easycode/easycode_yml_name_desc.png)

> 文件组模板名

![image](/img/easycode/easycode_group_name_desc.png)

### 4. 模板制作

以项目里边默认跟的一个模板demo.tpl为例：

**`demo.tpl 如下：`** 

    /**
    *
    * ***** 模板定义                         *****
    * @name demo 
    * @filename demo 即将生成的目标名称 可含有变量 如:<{root.model_name}>demo
    * @description 测试模板
    * @ext js 
    * @targetPath  @projectRoot/src
    * ***** 模板内变量: 变量名称|默认值|变量描述 *****
    * @variable    func_name|fabulous|方法名称
    * @variable    desc|默认模板文件|待生成文件描述
    * ****** 全局常用变量                     *****
    * projectRoot     当前命令行所在路径，通常是项目路径，也是有配置文件easycode.yaml的路径
    * now_datetime    当前系统时间 格式为YYYY-mm-dd HH:MM:ss
    * now_date        当前日期 格式为YYYY-mm-dd
    * 还有其他配置文件中的根属性和metadata中定义的属性都可以在模板中使用
    * 全局变量通过加root来定义
    * 如： root.now_date, root.author root.filename_withno_ext(没有后缀名的文件名)) 等等
    */

    /**
    * @author <{root.author}>
    * @date_time <{root.now_date}>
    * @description <{desc}>
    */

    function <{func_name}> () {
        let author = '<{root.author}>'
        console.log('作者是:' + author)
    }

    <{func_name}>()

** 上面代码共有两块注释 第一块为模板定义注释, 第二块则显然就是代码注释 ** 

**`我们讲解第一块代码注释`**

> @name  模板名

easy config refresh后，改字段将作为模板名放到easycode.yml模板块那里

> @filename 文件名

该注释将会觉得该模板生成时默认的文件名，支持变量定义

> @description 模板描述

支持变量定义

> @ext 模板对应的文件后缀名 

生成的文件将会是该后缀名的文件

> @targetPath 文件将生成的目标路径


> @varibale 模板内变量定义

定义规则 {变量名称}|{默认值}|{变量描述}

在生成文件时，将会在命令行交互定义模板内的变量

`其他的行 不重要 都可以删`

> 模板的变量符号及使用

    <{ }>

    做过web开发的都应该懂的。这个就是个变量占位符，替换变量用的

> 全局变量

    常用的全局变量（全局变量通过加root来定义）
    root.now_date
    root.author
    root.filename_withno_ext
    这些都是在eayscode.yaml中定义的属性
    而当前页面定义的属性，如demo.tpl定义的desc变量 直接使用<{desc}>来使用

> 模板组

*模板组的文件，是以文件夹为单位的。里边包含一个group.yml配置文件，内容格式如下*

    ## 模板组名
    name: simple-curd    
    ## 模板组描述
    description: 简单增删盖查-不继承BaseEntity,没有controller 
    ## mysql 连接 用来生成 如java中的实体类, 如自带模板组的simple-curd中的 Entity.tpl中会用到
    ## 查询对应表中的字段来生成实体类
    mysql:
      host: '127.0.0.1'
      port: '33060'
      user: 'root'
      password: '123456'
      database: 'db_name'
    # 公共变量名称: 默认名称，用来命令行交互，交互生成的字段都是以 root.group.xx 为变量在模板文件中可以使用
    public_variables: 
      - type: 'input'
        name: 'table_name'  // 这里就是 root.group.table_name
        message: '表名'
        default: 'prefix_tablename'
      - type: 'input'
        name: 'model'      // 这里就是 root.group.model
        message: '模型名'
        default: 'User'
      - type: 'input'
        name: 'model_lower'
        message: '模型名小写'
        default: 'user'
      - type: 'input'
        name: 'model_desc'
        message: '模型名描述'
        default: '用户'

## easycode.yml 配置文件讲解

**`格式如下`**

    name: 柠檬报价
    description: 一个小而美的报价系统
    author: wangjiang
    version: 1.0.0
    project_name: lemon-price
    metadata:
      basePackagePath: com/wangjianng/cloud
      basePackage: com.wangjianng.cloud
    pathMetadata:
      project: lemon
    tplGit:
      path: https://github.com/wangjiang988/easytpl_java.git
      branch: master
    templates:
      singles:
        - name: AdminController
          description: 控制接口类
          filePath: curd/Controller.tpl
        - name: Entity
          description: 实体类
          filePath: curd/Entity.tpl
        - name: Mapper
          description: mapper类
          filePath: curd/Mapper.tpl
        - name: Mapper
          description: MapperXml文件
          filePath: curd/MapperXml.tpl
        - name: Service
          description: 服务接口类
          filePath: curd/Service.tpl
        - name: ServiceImpl
          description: 服务实现类
          filePath: curd/ServiceImpl.tpl
        - name: demo
          description: 测试模板
          filePath: demo/demo.tpl
        - name: class
          description: java通用class
          filePath: java/class.tpl
      groups:
        - name: curd
          rootPath: curd
          description: 增删盖查
          children:
            - name: AdminController
              description: 控制接口类
              filePath: curd/Controller.tpl
            - name: Entity
              description: 实体类
              filePath: curd/Entity.tpl
            - name: Mapper
              description: mapper类
              filePath: curd/Mapper.tpl
            - name: Mapper
              description: MapperXml文件
              filePath: curd/MapperXml.tpl
            - name: Service
              description: 服务接口类
              filePath: curd/Service.tpl
            - name: ServiceImpl
              description: 服务实现类
              filePath: curd/ServiceImpl.tpl

> 其中 tplGit 指向的是你自己制作的模块库

针对不同的项目，你可以使用不同的模板库，在easy config init 或refresh时 会自动把代码库下载到当前路径下的templates文件夹内作为模板使用


## 其他 
  待补充


