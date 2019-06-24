---
title: nodejs模块——fs模块 WriteFile写入文件
date: 2019-06-20 10:50:52
categories: 
- nodejs模块——fs模块 WriteFile写入文件
tags: 
- nodejs
---

## WriteFile写入文件
使用fs.writeFile(filename,data,[options],callback)写入内容到文件。

<!-- more -->

参数说明：

*   filename String 文件名
*   data String|buffer
*   option Object
*       encoding String |nulldefault='utf-8'
*       mode Number default=438(aka 0666 in Octal)
*   flag 
*       Stringdefault='w'
*   callback Function

## 新建文件  readfile.js  output.txt
![](/img/nodejs-writefile1.png)



## 代码：readfile.js
``` bash
var fs = require('fs'); // 引入fs模块
 
// 写入文件内容（如果文件不存在会创建一个文件）
// 传递了追加参数 { 'flag': 'a' }
fs.writeFile('./output.txt', 'HelloWorld', { 'flag': 'a' }, function(err) {
    if (err) {
        throw err;
    }
 
    console.log('Hello.');
 
    // 写入成功后读取测试
    fs.readFile('./output.txt', 'utf-8', function(err, data) {
        if (err) {
            throw err;
        }
        console.log(data);
    });
});
```
## 输出 
打开cmd窗口，执行node readfile.js命令  

![](/img/nodejs-writefile2.png)

## 查看结果

![](/img/nodejs-writefile3.png)

