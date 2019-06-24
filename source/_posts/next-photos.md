---
title: Hexo NexT主题内添加相册功能
date: 2019-06-24 15:50:52
categories: 
- Hexo NexT主题内添加相册功能
tags: 
- Next
---

给博客添加一个相册页面，以展示自己拍摄的一些照片 (≖ᴗ≖)✧

## _config.yml
首先新建hexo new page photos相册页面，将会在source/下创建photos/index.md，在其中添加type: photos

之后在主题_config.yml文件中对应位置menu里添加Photos: /photos/ || image ，这样生成后就能在页面的对应页面选项中有该相册Tab。

<!--more-->

## json
在博客根目录下新建uploadPhotos文件夹，里面将会存放照片以及相关js文件。

新建uploadPhotos/Images/文件夹，然后在其中存放需要在页面中展示的照片集（后续需将该文件夹上传至GitHub相册库，若没有对应库，可新建，并上传图片）。

新建uploadPhotos/tool.js文件，里面内容如下，主要功能是访问照片文件夹，获取每张照片的size和name，并生成对应的json文件：

命令：Git Bash中键入 node tool.js生成json
注：若出现Error: Cannot find module 'image-size'问题，请在Git Bash中键入对应命令npm install image-size进行安装。

``` bash
"use strict";
    const fs = require("fs");
    const sizeOf = require('image-size');
    const path = "Images";
    const output = "../themes/next/source/photos/photoslist.json";
    var dimensions;
    fs.readdir(path, function (err, files) {
        if (err) {
            return;
        }
        let arr = [];
        (function iterator(index) {
            if (index == files.length) {
                fs.writeFile(output, JSON.stringify(arr, null, "\t"));
                return;
            }
            fs.stat(path + "/" + files[index], function (err, stats) {
                if (err) {
                    return;
                }
                if (stats.isFile()) {
                    dimensions = sizeOf(path + "/" + files[index]);
                    console.log(dimensions.width, dimensions.height);
                    arr.push(dimensions.width + '.' + dimensions.height + ' ' + files[index]);
                }
                iterator(index + 1);
            })
        }(0));
    });
```

json文件样例如下：

``` bash
[
	"4032.3024 IMG_0391.JPG",
	"12500.3874 IMG_0404.JPG",
	"4032.3024 IMG_0416.JPG",
	"4032.3024 IMG_0424.JPG",
	"3024.4032 IMG_0427.JPG",
	"4032.3024 IMG_0429.JPG",
	"4032.3024 IMG_0430.JPG"
]
```

## photo.js
新建themes/next/source/photos/photo.js文件，其中photos文件夹是自己创建的。

photos.js内容如下，主要功能是访问json文件内容，遍历每行数据，并在页面对应位置上放置代码，展示图片（其中图片链接为自个GitHub相册库中图片的链接）：

``` bash
photo ={
    page: 1,
    offset: 20,
    init: function () {
        var that = this;
        $.getJSON("/photos/photoslist.json", function (data) {
            that.render(that.page, data);
            //that.scroll(data);
        });
    },
    render: function (page, data) {
        var begin = (page - 1) * this.offset;
        var end = page * this.offset;
        if (begin >= data.length) return;
        var html, imgNameWithPattern, imgName, imageSize, imageX, imageY, li = "";
        for (var i = begin; i < end && i < data.length; i++) {
           imgNameWithPattern = data[i].split(' ')[1];
           imgName = imgNameWithPattern.split('.')[0]
           imageSize = data[i].split(' ')[0];
           imageX = imageSize.split('.')[0];
           imageY = imageSize.split('.')[1];
            li += '<div class="card" style="width:330px">' +
                    '<div class="ImageInCard" style="height:'+ 330 * imageY / imageX + 'px">' +
                      '<a data-fancybox="gallery" href="https://github.com/asdfv1929/BlogPhotos/blob/master/Images/' + imgNameWithPattern + '?raw=true" data-caption="' + imgName + '">' +
                        '<img src="https://github.com/asdfv1929/BlogPhotos/blob/master/Images/' + imgNameWithPattern + '?raw=true"/>' +
                      '</a>' +
                    '</div>' +
                    // '<div class="TextInCard">' + imgName + '</div>' +
                  '</div>'
        }
        $(".ImageGrid").append(li);
        $(".ImageGrid").lazyload();
        this.minigrid();
    },
    minigrid: function() {
        var grid = new Minigrid({
            container: '.ImageGrid',
            item: '.card',
            gutter: 12
        });
        grid.mount();
        $(window).resize(function() {
           grid.mount();
        });
    }
}
photo.init();
```
## photos.swig

新建themes/next/layout/photos.swig文件，其内容模仿tag.swig中内容（直接复制粘贴），然后将其中的tag全部替换为photos。

## photos.css
.ImageGrid {width: 100%;max-width: 1040px;margin: 0 auto; text-align: center;}
.card {overflow: hidden;transition: .3s ease-in-out; border-radius: 8px; background-color: #ddd;}
.ImageInCard {}
.ImageInCard img {padding: 0 0 0 0;}
.TextInCard {line-height: 54px; background-color: #ffffff; font-size: 24px;}

## page.swig
修改themes/next/layout/page.swig文件，添加下面代码中中间page.type === "photos"那两行。

``` bash
{% block title %}{#
#}{% set page_title_suffix = ' | ' + config.title %}{#
#}{% if page.type === "categories" and not page.title %}{#
  #}{{ __('title.category') + page_title_suffix }}{#
#}{% elif page.type === "tags" and not page.title %}{#
  #}{{ __('title.tag') + page_title_suffix }}{#
#}{% elif page.type === "photos" and not page.title %}{#
  #}{{ __('title.photos') + page_title_suffix }}{#
#}{% else %}{#
  #}{{ page.title + page_title_suffix }}{#
#}{% endif %}{#
#}{% endblock %}
```

依旧是该文件中，添加page.type === "photos"那两行：

``` bash 
{% elif page.type === 'categories' %}
  <div class="category-all-page">
    <div class="category-all-title">
        {{ _p('counter.categories', site.categories.length) }}
    </div>
    <div class="category-all">
      {{ list_categories() }}
    </div>
  </div>
{% elif page.type === 'photos' %}
    <div class="ImageGrid"></div>
{% else %}
  {{ page.content }}
{% endif %}
```

## _config.yml
在主题配置文件_config.yml中相关部分，进行相关的配置（lazyload、fancybox）：
首先确保_config.yml中Fancybox部分是否开启：

``` bash 
# Fancybox
fancybox: true
```

之后亦在该文件_config.yml的末尾部分，找到

``` bash 
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib
```

在这个vendors的选项下面，有对应的fancybox、lazyload配置区域，填写相应的URL即可：

``` bash 
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib
  # Internal version: 2.1.3
  jquery:
  # Internal version: 2.1.5
  # See: http://fancyapps.com/fancybox/
  fancybox: https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.3.5/jquery.fancybox.min.js
  fancybox_css: https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.3.5/jquery.fancybox.min.css
  # Internal version: 1.0.6
  # See: https://github.com/ftlabs/fastclick
  fastclick:
  # Internal version: 1.9.7
  # See: https://github.com/tuupola/jquery_lazyload
  lazyload: https://cdn.jsdelivr.net/npm/lazyload@2.0.0-beta.2/lazyload.js
```

之后在_layout.swig中相关信息配置：

``` bash
<script type="text/javascript" src="https://unpkg.com/minigrid@3.1.1/dist/minigrid.min.js"></script>
<link rel="stylesheet" href="/photos/photos.css">
<script type="text/javascript" src="/photos/photo.js"></script>
```

重新生成访问，查看成果。

## 总结
以上就是添加相册功能大概流程，因为步骤比较多，且是通过后期回忆步骤进行记录，所以可能存在些许问题，还请原谅，并请把出现的问题在本文下面的评论中点出，我会进行修改。

后续的实现：

*   将照片上传至GitHub相册库时，由于照片分辨率较高，其都达到了两三M以上，上传速度较慢，导致上传进度缓慢。后期想通过代码将照片进行压缩后再上传至相册库。

*   相册展示整个操作流程为：先上传照片到git库，再生成json文件，之后便是正常的clean、g、d，后期想把压缩、上传照片和生成json文件整合到一起。
    
*   目前的照片展示都是所有照片一整块放一起进行瀑布流显示，后期想将照片根据其旅游场景或类别、时间不同进行分类至对应文件夹，并根据类别或时间线显式展示出不同文件夹下的照片。

原文链接:(https://asdfv1929.github.io/2018/05/26/next-add-photos/#more)