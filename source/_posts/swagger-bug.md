---
title: springboot集成swgger2错误解决（No enum constant org.springframework.web.bind.annotation.RequestMethod.Get）
date: 2019-05-20 16:29:27
categories:
- springboot集成swgger2错误解决
tags: 
- swgger2
---

## springboot在集成swagger2启动时遇到如下错误：

``` bash
java.lang.IllegalArgumentException: No enum constant org.springframework.web.bind.annotation.RequestMethod.Get
	at java.lang.Enum.valueOf(Enum.java:238)
	at org.springframework.web.bind.annotation.RequestMethod.valueOf(RequestMethod.java:35)
	at springfox.documentation.swagger.readers.operation.OperationHttpMethodReader.apply(OperationHttpMethodReader.java:49)
	at springfox.documentation.spring.web.plugins.DocumentationPluginsManager.operation(DocumentationPluginsManager.java:120)
	at springfox.documentation.spring.web.readers.operation.ApiOperationReader.read(ApiOperationReader.java:73)
```
<!--more-->

``` bash
	at springfox.documentation.spring.web.scanners.CachingOperationReader$1.load(CachingOperationReader.java:50)
	at springfox.documentation.spring.web.scanners.CachingOperationReader$1.load(CachingOperationReader.java:48)
	at com.google.common.cache.LocalCache$LoadingValueReference.loadFuture(LocalCache.java:3527)
	at com.google.common.cache.LocalCache$Segment.loadSync(LocalCache.java:2319)
	at com.google.common.cache.LocalCache$Segment.lockedGetOrLoad(LocalCache.java:2282)
	at com.google.common.cache.LocalCache$Segment.get(LocalCache.java:2197)
	at com.google.common.cache.LocalCache.get(LocalCache.java:3937)
	at com.google.common.cache.LocalCache.getOrLoad(LocalCache.java:3941)
	at com.google.common.cache.LocalCache$LocalLoadingCache.get(LocalCache.java:4824)
	at com.google.common.cache.LocalCache$LocalLoadingCache.getUnchecked(LocalCache.java:4830)
	at springfox.documentation.spring.web.scanners.CachingOperationReader.read(CachingOperationReader.java:57)
	at springfox.documentation.spring.web.scanners.ApiDescriptionReader.read(ApiDescriptionReader.java:66)
	at springfox.documentation.spring.web.scanners.ApiListingScanner.scan(ApiListingScanner.java:89)
	at springfox.documentation.spring.web.scanners.ApiDocumentationScanner.scan(ApiDocumentationScanner.java:71)
    at springfox.documentation.spring.web.plugins.DocumentationPluginsBootstrapper.scanDocumentation(DocumentationPluginsBootstrapper.java:95)
	at springfox.documentation.spring.web.plugins.DocumentationPluginsBootstrapper.start(DocumentationPluginsBootstrapper.java:154)
	at org.springframework.context.support.DefaultLifecycleProcessor.doStart(DefaultLifecycleProcessor.java:182)
	at org.springframework.context.support.DefaultLifecycleProcessor.access$200(DefaultLifecycleProcessor.java:53)
	at org.springframework.context.support.DefaultLifecycleProcessor$LifecycleGroup.start(DefaultLifecycleProcessor.java:360)
	at org.springframework.context.support.DefaultLifecycleProcessor.startBeans(DefaultLifecycleProcessor.java:158)
	at org.springframework.context.support.DefaultLifecycleProcessor.onRefresh(DefaultLifecycleProcessor.java:122)
	at org.springframework.context.support.AbstractApplicationContext.finishRefresh(AbstractApplicationContext.java:879)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.finishRefresh(ServletWebServerApplicationContext.java:161)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:549)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:140)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:775)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:397)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:316)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1260)
    at org.springframework.boot.SpringApplication.run(SpringApplication.java:1248)
	at com.wx.wlcx.WlcxApplication.main(WlcxApplication.java:14)
```

在controller 中用的是注解@GetMapping,在集成swagger2之前可以正常访问， 那问题就一定处在swagger2相关的code 中， 于是检查在code 中哪里用到了httpmethod, 发现了问题所在：

``` bash
public enum RequestMethod {
    GET,
    HEAD,
    POST,
    PUT,
    PATCH,
    DELETE,
    OPTIONS,
    TRACE;

    private RequestMethod() {
    }
}
```

枚举类中method 方法都是大写， 而我的代码中将method 写成了：

``` bash
//api 具体描述
@ApiOperation(value = "根据id查询公司具体信息", notes = "查询公司所有信息及送货地址", tags = {"公司查询"}, httpMethod = "Get")
```

这导致swagger 根据httpMethod 去获取enum 类的类型时匹配不到， 于是将
httpMethod = “Get” 中的"Get" 改成"GET"即可。

``` bash
@ApiOperation(value = "根据id查询公司具体信息", notes = "查询公司所有信息及送货地址", tags = {"公司查询"}, httpMethod = "GET")
```