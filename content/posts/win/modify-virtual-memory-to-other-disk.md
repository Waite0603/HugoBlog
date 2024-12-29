---
title: "Win11 改虚拟内存到C盘之外的盘"
date: 2023-09-03T16:04:23+08:00
categories: ["Win"]
tags: ["Win"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---



> 参考 https://answers.microsoft.com/zh-hans/windows/forum/all/win11%E6%97%A0%E6%B3%95%E6%8A%8A%E8%99%9A%E6%8B%9F/50381a46-ce77-40f5-8bde-a9d01b361e6c

> 解决: WIN11无法把虚拟内存更改到其他盘，改完后重启显示由于启动页面文件配置的问题,然后虚拟内存又回c盘了

## 分页文件更改

1. 此电脑 -> 右键属性 -> 高级系统设置
2. 选择高级 -> 性能(设置) -> 高级 -> 虚拟内存 -> 更改

![](https://qiniu.waite.wang/20230903163805.png)

3. 取消勾选“自动管理所有驱动器的分页文件大小"
4. C -> 无分页文件 -> 设置
5. D -> 自定义大小 -> 建议最小在 1.5 倍数以上
6. 输入虚拟内存大小后请务在设置的虚拟内存大小右侧点击“设置”后再点击“确定”/“应用”，否则虚拟内存配量将不生效。
7. 重启

![](https://qiniu.waite.wang/20230903164048.png)

## 提示以及解决方案

> 通过修该键值解决问题一般是因为当前开启了Bitlocker，所以对于虚拟内存的修改做出了一些限制。 解决此限制

1. win键+R， 输入：regedit 打开注册表编辑器
2. 找到：HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Session Manager\Memory Management
3. 然后PagefileOnOsVolume和ExistingPageFiles，将PagefileOnOsVolume 的值由默认的1改为0，ExistingPageFiles里面的C:改为你要更换的盘符名。
  