---
title: "Windows11家庭中文版系统安装Docker"
date: 2023-09-24T16:04:23+08:00
categories: ["Win"]
tags: ["Win", "Docker"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

---
title: Windows11家庭中文版系统安装Docker
id: a293d46a-9b4e-4e42-8bbc-0bcec5c21662
date: 2023-09-24 22:29:15
auther: admin
cover: 
## 安装到 D 盘(软链接)

```bash
docker 默认安装路劲为 C:\Program Files\Docker
docker 虚拟磁盘默认安装路劲为 C:\Users\<YourName>\AppData\Local\Docker
```

1. 用 **管理员身份打开cmd窗口**
2. 执行如下命令

```bash
mklink /j "C:\Program Files\Docker" "D:\Program Files\Docker"
```

3. 已经安装Docker，需要重新再安装一次。
4. 安装后C盘下的Docker文件就只是一个软链接了，映射的真实路径在D盘Docker文件夹下(注意: 软链接后需要在相应目录创建文件夹, 不然后续安装会报错)

## Hyper-Vr

> 查看自己的系统, 如果你的系统跟我一样是window11家庭[中文版](https://so.csdn.net/so/search?q=中文版&spm=1001.2101.3001.7020)，则会找不到Hyper-Vr，这时则需要自己创建，讲下述代码复制在txt文本里，并重命名为Hyper.cmd，右键以管理员方式运行，最后输入“Y”重启电脑后即可。

```bash
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```

1. 找到控制面板--程序--程序和功能--启用或关闭windows功能（或者电脑直接搜索启用或关闭windows功能） ，勾选Hyper-V。（如果找不到 Hyper-V，请尝试以上步骤）

![image-20230924221716928](https://qiniu.waite.wang/202309242217878.png)

2. 也可以输入下述命令在Windows 操作系统中启用 Microsoft Hyper-V 虚拟化技术。 

```bash
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
```

3. 然后输入下述命令，设置 Windows 操作系统中的 Hyper-V 启动类型。具体来说，它会将 Hypervisor 的启动类型设置为 "auto"，这意味着在系统启动时自动启动 Hyper-V。(非强制)

```bash
bcdedit /set hypervisorlaunchtype auto
```

## 安装 Docker

1. 官网下载docker文件[Get Started | Docker](https://www.docker.com/get-started/)，选择download for windows下载。 

2. 双击打开下载好的文件Docker Desktop Installer.exe，add shortcut to desktop选择√代表同意添加快捷键到桌面，如果不勾选就说明不创建快捷键，大家根据自己需求选择就行。之后点击🆗
3.  等待一会后会出现下图所示，1说明安装成功，2说明必须重启电脑才能成功安装，3代表关闭这个界面重启电脑

![](https://qiniu.waite.wang/202309242221166.png)

4. 如果重启电脑后又出现下图报错：

![image-20230924222140258](https://qiniu.waite.wang/202309242221806.png)

5. 说明系统的WSL版本太旧，需要更新，按照提示在终端中输入下述代码等待更新即可。（终端最好以右键点击以管理员身份运行打开）

```bash
wsl --update
```

![image-20230924222228138](https://qiniu.waite.wang/202309242222498.png)

6. 接着继续输入docker --version检测[docker安装](https://so.csdn.net/so/search?q=docker安装&spm=1001.2101.3001.7020)的版本，出现如下图说明已经安装docker。

![image-20230924222346598](https://qiniu.waite.wang/202309242223710.png)

7. 继续输入docker run hello-world，出现下图说明docker安装成功，且可以执行docker命令。

![image-20230924222425682](https://qiniu.waite.wang/202309242224058.png)

> 这里有可能报错，当你在终端输入 docker run hello-world，出现的结果反而是下面错误的结果：
>
> ```bash
> error during connect: In the default daemon configuration on Windows, the docker client must be run with elevated privileges to connect.
> ```
> 这里是说权限不够，解决方法也很简单，退出 Docker 后，右键点击桌面 docker 图标，选择以管理员身份运行程序，这时候再重新打开终端输入 docker run hello-world，就会出现 正确的结果了。

8. 这时在打开桌面docker快捷键就不会报错了，见下图，此时还能看到hello-world的镜像。

![image-20230924222715964](https://qiniu.waite.wang/202309242227032.png)

## 一些报错.

> 参考 https://forums.docker.com/t/solved-docker-failed-to-start-docker-desktop-for-windows/106976/16

>Docker: Error response from daemon: Ports are not available 端口没被占用，却显示被占用

`docker: Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:10911 -> 0.0.0.0:0: listen tcp 0.0.0.0:10911: bind: An attempt was made to access a socket in a way forbidden by its access permissions.`

1. 看错误信息说是端口被占用了，那咱就用 `netstat -aon | findstr`, 结果发现Docker报错所指向的端口并没有被占用，又遇到了奇怪问题。
2. 解决: 其实这是Windows中的一个小问题，只需要重启NAT网络就可以解决了，执行如下两条命令：

```
net stop winnat
net start winnat
```