---
title: "Tomcat + Idea 配置及使用"
date: 2022-10-22T16:04:23+08:00
categories: ["Env"]
tags: ["Jsp"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---


## Jdk 安装

![](https://qiniu.waite.wang/0_0_13.png)

## Tomcat

> 注意 Tomcat 版本与 Jdk 匹配

![](https://qiniu.waite.wang/20230906133609.png)

官网地址：https://tomcat.apache.org/

我这里选择的是Tomcat9.0版本，大家可以选择自己需要的版本

![](https://qiniu.waite.wang/20230906133736.png)

将下载好的zip文件解压到指定文件夹，例如：D:\software\tomcat90，目录不要有中文（建议不要解压到C盘 ）

### 配置Tomcat环境变量

Tomcat的环境变量配置跟JDK的环境变量配置几乎一样，只是修改变量名称和对应的路径，具体操作如下:

1. 右击此电脑--属性
2. 单击高级系统设置--高级环境变量(N)
3. 在系统变量下面找到新建，填写变量名：CATALINA_HOME，变量值：D:\software\tomcat90\apache-tomcat-9.0.74（即Tomcat的安装路径）
4. 在系统变量找到变量名Path--单击Path--编辑--新建--输入"%CATALINA_HOME%\bin"--确定--确定--确定

### 测试环境变量是否配置成功

1. 点击屏幕左下角的“开始”按钮--搜索命令提示符--右键以管理员身份运行（以管理员身份进入防止权限问题的错误）--输入cmd回车进入命令行窗口--进入Tomcat的bin目录，输入`service.bat install` [服务名,默认为配置文件中名称Tomcat9] 进行window服务安装，我这个已经安装完了，window服务已经存在了所以显示error，如果出现 The service 'Tomcat9' has been installed.则表明安装服务成功！
2. 在目录下 管理员 cmd 运行 `startup.bat`

![](https://qiniu.waite.wang/20230906134140.png)

3. 以上页面表示运行成功, 浏览器打开 127.0.0.1:8080 即可看到 Tomcat 起始页面

### 改端口

1. 找到tomcat目录/conf/server.xml
2. 选择以记事本打开，把8080改为你想修改的端口号，这里选择修改为8，修改后保存
![](https://qiniu.waite.wang/20210516171647863.png)

3. 注意：
1）修改的端口一定不能被占用
2）修改完成后，进入bin目录，先启动shutdown.bat，再启动startup.bat 重启tomcat服务器

## Idea 配置

1.先新建一个正常的java项目

![](https://qiniu.waite.wang/aa9d56ff60b34a3ba48104f17d87b2f1.png)

 2.运行下拉框找到编辑配置，点击

![](https://qiniu.waite.wang/9f44c37140c04400ab7ea4496131220d.png)

 3.找到tomcat服务器，选择本地那个

![](https://qiniu.waite.wang/519f8f1157334cf8b3f23816c7da142a.png)

 4.配置你的tomcat文件的路径，在tomcat项目启动完成后打开浏览器，这里选择你电脑上安装的一款浏览器即可，点击确定

![](https://qiniu.waite.wang/4b58f61a25204d87be35f250bcdb60f7.png)


5.右键点击项目，选择添加框架支持，点击

![](https://qiniu.waite.wang/97f463754fa143589fa02447b1c47815.png)

6.勾选web应用程序与创建web.xml，点击确定

![](https://qiniu.waite.wang/1d245f043d1743dfa4ac7dc28de05fe3.png)

 7.找到运行箭头左边的tomcat图标，点击，选择编辑配置，点击

![](https://qiniu.waite.wang/6dd05440497c4f9d9bb27d44c63cd14e.png)

8. 点击部署

![](https://qiniu.waite.wang/a9c28d16cc0e4ea3b95ce30939fb82df.png)

9. 添加工件，点击 

![](https://qiniu.waite.wang/c35d408495424a419bc03a2006985ad1.png)

10. 选择你的项目，上下文那一栏填写一个/即可 

![](https://qiniu.waite.wang/7d41fbb1bce7466d933afd316ec3c7fb.png)

11. 运行左边的tomcat图标没有❌时，就代表配置成功，编辑你的项目文件，点击运行即可 

![](https://qiniu.waite.wang/426b0ec5020948dd8669749de26e87a2.png)

12. 运行成功会自动打开你自己设置的默认浏览器

> 建议开启以下功能, 这样不用每次都要重新部署

![](https://qiniu.waite.wang/202309141018297.png)

## 设置JSP代码自动补全

+ File -> Project Structure，打开项目设置页面；选择到“Dependencies”便签 -> 点击“+”-> 2.Librarys：
![](https://qiniu.waite.wang/202310121210567.webp)

+ 在Choose Libraries页面，选择“Tomcat”:

+ 将Tomcat的Scope设置为Provinced，然后保存设置：

![](https://qiniu.waite.wang/202310121211983.webp)