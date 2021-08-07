---
title: windows下使用cmake+vscode实现多文件多目录编译和调试（2）
author: hlss
date: 2021-08-05 21:25:00
tags: CMake
categories: c++
keywords: cmake

---

<b>前言：</b>[上一篇](https://hlss-lxh.github.io/2021/08/02/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/#more)文章中描述了使用cmake在windows平台下编译一个项目的基本方法，这里继续在上一篇的基础是叙述如何在vscode中进行调试

<!--more-->

---

##### 1.开始调试

快捷键 `F5`启动调试，此时会出现报错信息，叉掉。根目录下会出现 `.vscode`文件夹，里面包含了 `tasks.json`和 `launch.json`两个文件             ![vscode](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95%EF%BC%882%EF%BC%89/.vscode.png)

接下来主要就是对着两个文件的修改

##### 2.修改tasks.json和launch.json

tasks.json需要替换重写，如下

当然也可以自己编写json，然后加上新建build文件夹的步骤，实现编写完CMakeLists.txt之后的一系列操作

```json
{
    "version": "2.0.0",
    "options": {
        "cwd": "${workspaceFolder}/build/"   //进入build子目录
    },
    "tasks":[
       //任务一：执行cmake..
        {
            "label": "cmake",  //定义一个标签进行标记
            "type": "shell",
            "command": "cmake",
            "args": [
                ".."
            ]
        },
        //任务二：执行mingw32-make.exe
        {
            "label": "make",
            "type": "shell",
            "group":{
                "kind":"build",
                "isDefault":true
            },
            "command": "mingw32-make.exe",
            "args": []
        },
        //任务三：使前两个任务按顺序执行
        {
            "label": "build cpp project",   //注意与launch.json中的preLaunchTask对应
            "dependsOrder": "sequence",     //按顺序执行依赖项
            "dependsOn":[
                "cmake",
                "make"
            ]
        }
    ]
}
```

然后是launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++.exe - 生成和调试活动文件",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}\\EXE\\swap_hello.exe",   //在CMakeLists.txt中指定的exe文件路径
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",    //表示当前workspace文件夹路径，即 TEST2/
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\MinGW\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build cpp project"    //注意与tasks.json中的任务三的label值对应
        }
    ]
}
```

##### 3.验证

现在直接`F5`运行调试，运行结果与第一篇中手动结果一致

![验证](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95%EF%BC%882%EF%BC%89/%E9%AA%8C%E8%AF%81-16283069119252.png)

![结果](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95%EF%BC%882%EF%BC%89/%E7%BB%93%E6%9E%9C.png)

这之后就可以把vscode当IDE使用了，断点调试什么的都是信手拈来

##### 4.总结

对于不明白的地方多去探索，才会有自己的理解和感悟。

例如tasks.json我开始也没太明白，后来模仿别人写的，在百度和官方找解释，慢慢也就明白了

总之，实践是最好的老师

> [windows下使用cmake+vscode实现多文件多目录编译和调试（1）](https://hlss-lxh.github.io/2021/08/02/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/#more)
