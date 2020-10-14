---

title: maven 模块化目录结构，jar或war包打包到统一父路径
date: 2020-10-14 22:05:00
tags:
    - java
    - maven
categories:
    - [java]
    - [maven]
---

## 说明

maven 模块化目录结构，jar或war包打包到统一父路径的pom.xml设置

 <!-- more -->

### 项目结构



<img src="./demo/image-20201012093409146.png" alt="image-20201012093409146" style="zoom:25%;" />

将druid-test的包打到父级目录下



### 打包代码



> Druid-test的pom.xml配置部分如下



```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals>
                <goal>copy</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                        <type>${project.packaging}</type>
                        <overWrite>true</overWrite>
                        <outputDirectory>${project.parent.basedir}/jars</outputDirectory>
                        <destFileName>druid-test.jar</destFileName>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```



关于 maven-dependency-plugin 的讲解如下

goal 讲解：

1、analyze：分析项目依赖，确定哪些是已使用已声明的，哪些是已使用未声明的，哪些是未使用已声明的

2、analyze-dep-mgt：分析项目依赖，列出已解析的依赖项与dependencyManagement中定义的依赖项不匹配的部分

3、analyze-report：生成项目报告

4、analyze-duplicate：分析pom.xml中的`<dependencies/>`和`<dependencyManagement/>标记，确定重复声明的依赖项`

5、build-classpath：告诉Maven以类路径格式从本地存储库输出依赖项的路径

6、copy：将配置在插件中的jar包复制到指定位置

7、copy-dependencies：将项目pom文件中定义的所有依赖及其传递依赖复制到指定位置

8、display-ancestors：显示项目所有祖先pom

8、get：从远程库中解析出一个构件

9、go-offline：让maven解决该项目所依赖的所有内容，以准备脱机

10、list：resolve的别名，列出项目的所有依赖项

11、list-repositores：显示项目所有依赖项目，然后列出使用的存储库

12、properties：包含文件系统上的工件的每个项目依赖项设置一个属性

13、purge-local-repository：清除本地存储库中的依赖

14、resolve：告诉Maven解析所有依赖项并显示版本。**JAVA 9注意：** *在使用Java 9运行时将显示模块名称。*

15、resolve-plugins：告诉Maven解决插件及其依赖项

16、sources：告诉Maven解析所有依赖项及其源附件、并显示版本

17、tree：显示该项目的依赖关系树

18、unpack：解压缩

19、unpack-dependencies：与copy-dependencies功能一致，只是会解压





参考文章：

 https://www.cnblogs.com/hqzmss/p/9837396.html

https://www.cnblogs.com/selier/p/9510326.html



### 常见问题

> 1.  maven-dependency-plugin 复制包失败，复制出来的包小了很多


```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>3.1.1</version>
    <executions>
        <execution>
            <id>copy</id>
            <phase>package</phase>
            <goals>
                <goal>copy</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                        <type>${project.packaging}</type>
                        <overWrite>true</overWrite>
                        <outputDirectory>../../target/</outputDirectory>
                        <destFileName>cs-onekey-selftest-java.jar</destFileName>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```



> 解决方案：

该问题已解决  原因是plugin的加载顺序

把打包的和复制的顺序调成如下即可

```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <!-- 指定该Main Class为全局的唯一入口 -->
        <mainClass>com.wangjiang.k8s.jvm.test.StarterApplication</mainClass>
        <layout>ZIP</layout>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal><!--可以把依赖的包都打包到生成的Jar包中-->
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>3.1.1</version>
    <executions>
        <execution>
            <id>copy</id>
            <phase>package</phase>
            <goals>
                <goal>copy</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                        <type>${project.packaging}</type>
                        <overWrite>true</overWrite>
                        <outputDirectory>../../target/</outputDirectory>
                        <destFileName>cs-onekey-selftest-java.jar</destFileName>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```



