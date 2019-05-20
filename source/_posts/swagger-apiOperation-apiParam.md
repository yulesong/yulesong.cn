---
title: Swagger注解：@ApiOperation与ApiParam使用
date: 2019-05-20 09:45:33
tags: Swagger注解说明
---
## @ApiOperation和@ApiParam

@ApiOperation不是spring自带的注解是swagger里的 
com.wordnik.swagger.annotations.ApiOperation;

<!--more-->

@ApiOperation和@ApiParam为添加的API相关注解，个参数说明如下： 
@ApiOperation(value = “接口说明”, httpMethod = “接口请求方式”, response = “接口返回参数类型”, notes = “接口发布说明”；其他参数可参考源码； 
@ApiParam(required = “是否必须参数”, name = “参数名称”, value = “参数具体描述”

## 实体注解 @ApiModel和@ApiModelProperty  

``` bash
@ApiModel("用户")
public class User {
    @ApiModelProperty("用户ID")
    private long id;
    @ApiModelProperty("用户名")
    private String name;
    @ApiModelProperty("生日")
    private Date birth;
}
```
|  注解 | 作用域 |  说明 |
|:-----|-----:|:-----:|
|ApiModel	 |  实体类名  |   描述实体  |
|ApiModelProperty  |  实体属性  |   描述属性  |

实际项目中非常需要写文档，提高Java服务端和Web前端以及移动端的对接效率。

Swagger是当前最好用的Restful API文档生成的开源项目，通过swagger-spring项目

实现了与SpingMVC框架的无缝集成功能，方便生成spring restful风格的接口文档，

同时swagger-ui还可以测试spring restful风格的接口功能。
