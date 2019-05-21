---
title: SpringBoot项目在IntelliJ IDEA中实现热部署
date: 2019-05-21 11:33:41
categories: 
- SpringBoot项目在IntelliJ IDEA中实现热部署
tags: 
- SpringBoot
---
## SpringBoot项目在IntelliJ IDEA中实现热部署

## 方式一、 开启idea自动make功能
1.在IDEA界面按CTRL+SHIFT＋Ａ->查找 make project automatically ->选中如图：

![](/img/idea-springboot1.png)

2.CTRL+SHIFT+A–>查找Registry–>找到并勾选compiler.automake.allow.when.app.running如图：

![](/img/idea-springboot2.png)

3.重启IDEA

## 方式二、引入spring-boot-devtools依赖

spring-boot-devtools是一个为开发者服务的一个模块，其中最重要的功能就是自动应用代码更改到最新的App上面去。

``` bash
原理是在发现代码有更改之后，重新启动应用，但是速度比手动停止后再启动更快。其深层原理是使用了两个ClassLoader，一个Classloader加载那些不会改变的类(第三方Jar包),另一个ClassLoader加载会更改的类，称为restart ClassLoader,这样在有代码更改的时候，原来的restartClassLoader被丢弃，重新创建一个restart ClassLoader，由于需要加载的类相比较少，所以实现了较快的重启时间。 
即devtools会自动监听classpath下的文件变动，并且会立即重启应用（发生在保存时机）
```

1.添加依赖

``` bash
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

2.在依赖中开启热部署

``` bash
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>//该配置必须
            </configuration>
        </plugin>
    </plugins>
</build>
```

接下来测试一下：
1.修改类–>保存：应用会重启
2.修改配置文件–>保存：应用会重启
3.修改页面–>保存：应用会重启，页面会刷新（原理是将spring.thymeleaf.cache设为false）
至此你的IDEA就可以愉快的修改代码了，修改后可以及时的看到效果，无须重启和清除浏览器缓存
