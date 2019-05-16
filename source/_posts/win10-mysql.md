---
title: win10-mysql
date: 2019-05-16 09:51:11
tags: mysql
---

## 一、下载mysql

1.在浏览器里打开mysql的官网(http://www.mysql.com/)
2.进入页面顶部的"Downloads"

<!--more-->

![](/img/mysql1.png)

3.打开页面底部的“Community(GPL) Downloads”

![](/img/mysql2.png)

4.在页面中间的位置找到我们windows上要用的下载页面“MySQL on Windows(Installer & Tools)”

![](/img/mysql3.png)

5.选择第一项"MySQL Installer”

![](/img/mysql4.png)

6.页面底端找到下载入口“Windows (x86,32-bit), MSI Installer”，点击Download按钮开始下载，共381.4M 注意：MSI格式是指windows的安装程序，下载后直接双击就能进入安装向导的那种，区别于对文件进行解压的安装方式；

![](/img/mysql5.png)

7.这个页面告诉询问你是否登录，告诉你登录之后有哪些好处，我们不登录，点击页面底部的“No thanks, just start my download.”按钮进入下载页面

![](/img/mysql6.png)

8.开始下载，等待下载完成（由于直接下载速度太慢，之后我用迅雷下载完成的）

![](/img/mysql7.png)

9.下载完成

![](/img/mysql8.png)

## 二、安装mysql

1.双击下载好的mysql安装文件“mysql-installer-community-5.7.14.0.msi”打开安装程序，打开后需要稍等一下

![](/img/mysql9.png)

2.选择安装类型（根据个人需要）

![](/img/mysql10.png)

3.我只需要安装mysql server，所以选择最后一项“Custom”，选择Custom之后左边的安装流程和右边的描述文字会改变，然后点击"Next"按钮继续

![](/img/mysql11.png)

4.在这里我们需要从安装程序提供的可安装的产品（Products）中选择我们需要的mysql server

![](/img/mysql12.png)

我们展开Available Products里的第一项“MySQL Servers”，依次展开其子结点，直到其终端结点，我的操作是64位的，所以选中“MySQL Server 5.7.14 - X64

![](/img/mysql13.png)

然后点击绿色的向右箭头，将当前Product移动需要安装的列表，然后在右边展开“MySQL Server 5.7.14 - X64”项，取消“Development Components”的勾选（因为我们只需要安装mysql server），之后点击“Next”按钮进入下一步

![](/img/mysql14.png)

5.点击“Execute”（执行）开始安装，安装过程中会显示安装的Progress（进度），等待安装完成后Status会显示Complete，mysql图标前会出现一个绿色的勾，然后点击“Next”按钮进入产品配置界面

![](/img/mysql15.png)

![](/img/mysql16.png)

6.点击“Next”按钮进入MySQL Server 的配置

![](/img/mysql17.png)

Config Type选择“Development Machine”，选择此项将使用较小的内容来运行我们的mysql server，对应小型软件、学习是完全够用的。之后“Next”

![](/img/mysql18.png)

在Root Account Password设置数据库root账号的密码，我填的是123456所以程序提醒我密码强度为弱，我们需要牢记这个密码，然后点击“Next”

![](/img/mysql19.png)

这里可以设置mysql server的名称和是否开机启动，我把名称改为了“MySQLZzz1”，取消了开机启动，其它的没改，点击“Next”

![](/img/mysql20.png)

点击“Next”

![](/img/mysql21.png)

此界面将之前设置的配置内容应用到我们的mysql server，点击“Execute”，等待完成就可以了

![](/img/mysql22.png)

配置完成，点击“Finish”完成配置环节

![](/img/mysql23.png)

7.配置完成后将回到安装程序，我们点击“Next”继续

![](/img/mysql24.png)
 
提示我们安装完成，点击“Finish”

![](/img/mysql25.png)

五、测试是否安装成功我们使用MySQL管理软件（Navicat for MySQL）进行连接测试，确保mysql已经可以使用：

这里我提供sqlNavicat怎么安装：就是傻瓜安装方法，安装好之后你们会遇到要求注册，直接解压我给你们的破解软件点击一下，在重新打开Navicat就ok拉：

Navicat安装百度云地址：http://pan.baidu.com/s/1eSb7qpW

破解Navicat软件百度云地址：http://pan.baidu.com/s/1kVf0QQr

1.打开Navicat for MySQL

2.新建一个连接，填写连接信息：连接名称：用于区分不同的连接，自己命名即可主机名：localhost端口：3306用户名：root密码：123456（之前配置mysql的时候填写的密码）

![](/img/mysql26.png)

3.点击“连接测试”按钮，弹出连接成功对话框即表示mysql server已开启

4.之后就是Navicat for MySQL软件的使用另：我们也可以在cmd里，再次输入“net start mysqlzzz1”，若提示“请求的服务已经启动。”表示mysql server已正常启动；


至此，mysql server在windows 10 64位上就安装完成了。



参考链接：https://www.jianshu.com/p/1aba608b21c5
