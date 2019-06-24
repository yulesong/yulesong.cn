---
title: Hexo NexT主题中添加网页标题崩溃欺骗搞怪特效
date: 2019-05-24 15:50:52
categories: 
- Hexo NexT主题中添加网页标题崩溃欺骗搞怪特效
tags: 
- Next
---

给网页title添加一些搞怪特效

<!-- more -->

## crash_cheat.js
在next\source\js\src文件夹下创建crash_cheat.js，添加代码：

``` bash
<!--崩溃欺骗-->
 var OriginTitle = document.title;
 var titleTime;
 document.addEventListener('visibilitychange', function () {
     if (document.hidden) {
         $('[rel="icon"]').attr('href', "/img/TEP.ico");
         document.title = '╭(°A°`)╮ 页面崩溃啦 ~';
         clearTimeout(titleTime);
     }
     else {
         $('[rel="icon"]').attr('href', "/favicon.ico");
         document.title = '(ฅ>ω<*ฅ) 噫又好了~' + OriginTitle;
         titleTime = setTimeout(function () {
             document.title = OriginTitle;
         }, 2000);
     }
 });
```

## 引用
在next\layout\_layout.swig文件中，添加引用（注：在swig末尾添加）：

``` bash
<!--崩溃欺骗-->
<script type="text/javascript" src="/js/src/crash_cheat.js"></script>

```
