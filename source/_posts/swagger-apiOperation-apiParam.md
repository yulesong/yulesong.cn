---
title: Swagger注解：@ApiOperation与@ApiImplicitParams使用
date: 2019-05-20 09:45:33
tags: Swagger注解说明
---
## @ApiOperation和@ApiImplicitParams

@ApiOperation不是spring自带的注解是swagger里的 
com.wordnik.swagger.annotations.ApiOperation;

<!--more-->

@ApiImplicitParams：用在请求的方法上，包含一组参数说明
     @ApiImplicitParams：用在请求的方法上，包含一组参数说明
     @ApiImplicitParam：用在 @ApiImplicitParams 注解中，指定一个请求参数的配置信息       
        name：参数名
        value：参数的汉字说明、解释
        required：参数是否必须传
        paramType：参数放在哪个地方
            · header --> 请求参数的获取：@RequestHeader
            · query --> 请求参数的获取：@RequestParam
            · path（用于restful接口）--> 请求参数的获取：@PathVariable
            · body（不常用）
            · form（不常用）    
        dataType：参数类型，默认String，其它值dataType="Integer"       
        defaultValue：参数的默认值

``` bash
@ApiImplicitParams({
    @ApiImplicitParam(name="mobile",value="手机号",required=true,paramType="form"),
    @ApiImplicitParam(name="password",value="密码",required=true,paramType="form"),
    @ApiImplicitParam(name="age",value="年龄",required=true,paramType="form",dataType="Integer")
})
```

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
