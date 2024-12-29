---
title: "gcc 安装 + vscode 搭建"
date: 2023-08-22T16:04:23+08:00
categories: ["Win"]
tags: ["Env"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

## 安装 MinGW

MinGW（Minimalist GNU for Windows）, 是一个适用于微软windows应用程序的极简洁的开发环境。MinGW提供了一个完整的开源编程工具集，适用于原生MS-Windows应用程序的开发，并且不依赖于任何第三方C运行时DLL。MinGW主要供在MS-Windows平台上工作的开发人员使用，但也可跨平台使用。

### 安装

- [https://sourceforge.net/projects/mingw-w64/files/](https://sourceforge.net/projects/mingw-w64/files/)

![_20220628223854](https://qiniu.waite.wang/202309221423490.png)

- 解压 配置 环境变量 到 bin 目录

### 验证

- gcc -v 查看版本号

- 新建文件 test.c

```
#include <stdio.h>

int main()
{
    printf("hello world\n");
    return 0;
}
```

- gcc test.c -o test

- test.exe

![_20220628224408](https://qiniu.waite.wang/202309221423040.png)

## VsCode 配置

### 插件安装

- C/C++

- Code Runner

### 设置

- code runner 右键 扩展设置 勾选以下

![c82830bc0eb14ece8ba7157c83e2411d](https://qiniu.waite.wang/202309221424164.png)

- 打开项目 在打开的文件夹中新建一个名为“.vscode”的子文件夹

- 选中“.vscode”子文件夹，新增三个配置文件”c\_cpp\_propertise.json“、”launch.json“、”tasks.json“

- 以下 C:/mingw64/ 替换为 自己路径

- c\_cpp\_propertise.json

```
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceRoot}",
                "C:/mingw64/include/**",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "__GNUC__=6",
                "__cdecl=__attribute__((__cdecl__))"
            ],
            "intelliSenseMode": "msvc-x64",
            "browse": {
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": "",
                "path": [
                    "${workspaceRoot}",
                    "C:/mingw64/include/**",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed",
                    "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
                ]
            }
        }
    ],
    "version": 4
}
```

- launch.json

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示            
            "type": "cppdbg", // 配置类型，这里只能为cppdbg           
            "request": "launch", //请求配置类型，可以为launch（启动）或attach（附加）          
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
            "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可            
            "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false            
            "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录workspaceRoot已被弃用，现改为workspaceFolder            
            "environment": [],
            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台           
            "MIMode": "gdb",
            "miDebuggerPath": "C:/mingw64/bin/gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应  
            "preLaunchTask": "gcc", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc      
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": false
                }
            ]
        }
    ]
}
```

- tasks.json

```
{
    "version": "2.0.0",
    "command": "gcc", // 注意对应  
    "args": [
        "-g",
        "${file}",
        "-o",
        "${fileBasenameNoExtension}.exe"
    ], // 编译命令参数
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": [
            "relative",
            "${workspaceFolder}"
        ],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}
```

## 运行

- 新建 hello.c

```
#include <stdio.h>

int main(void){
    printf("hello world! I\' m VSCode\n");
    return 0;
}
```

- 右键 Run Code

[](http://49.234.55.187:8090/archives/gcc%E5%AE%89%E8%A3%85vscode%E6%90%AD%E5%BB%BA)
