---
title: windows下使用cmake+vscode实现多文件多目录编译和调试（1）
date: 2021-08-02 16:34:11
tags: CMake
categories: c++
keywords: cmake
author: hlss

---

<b>前言</b>：CMake用来编写makefile的工具，因为win下编写c++程序习惯使用的是vscode,所以写了这篇。食用之前先确保你的cmake没有问题，vscode可以选择安装两个插件 `cmake`和 `cmake tools`

如果对文中cmake指令

<!--more-->

---

##### 1.创建示例

这里使用一个简单示例，实现打印 `hello hello`，以及交换 `a = 80,b = 20`两个量的值。

- 创建一个文件夹，在vscode中创建以下目录结构

![test2目录结构](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/test2%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)



- 接下来编辑`main.cpp`的内容：

```c++
#include<iostream>
#include"swap.h"
#include"hello.h"
using namespace std;

int main()
{
    int a = 80;
    int b = 20;

    swap(&a, &b);	#swap交换函数
    cout << "after swap: " << endl;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    hello();	#hello函数输出'hello hello'
}
```

- 在 `inc`文件夹中加入两个要用到的头文件 `swap.h`, `hello.h`

swap.h内容：

```c++
void swap(int *p1, int *p2);
```

hello.h内容：

```c++
void hello();
```

- 下面编写 `swap.cpp`和 `hello.cpp`内容

swap.cpp

```c++
#include<iostream>
#include"swap.h"
using namespace std;

void swap(int *p1, int *p2)\
{
    int temp = 0;
    temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}
```

hello.cpp

```c++
#include<iostream>
#include"hello.h"
using namespace std;

void hello()
{
    cout << "hello hello" << endl;
}
```

##### 2.g++编译验证程序正确性

![程序验证](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/%E7%A8%8B%E5%BA%8F%E9%AA%8C%E8%AF%81.png)

程序没问题，继续

##### 3.cmake使用简介

使用cmake生成makefile一般有 `内部构建` 和 `外部构建`两种方式，内部构建生成的文件全在根目录下，影响我们的操作，所以一般采用外部构建的方法

外部构建分下面几步：

- 在根目录下建立 `build`文件夹
- 编写`CMakeLists.txt`确定构建方式
- 执行`cmake`命令构建
- 执行`mingw32-make.exe`生成可执行文件，如果是Linux则是`make`。注意你的mingw是32还是64，可以在你的mingw安装目录下的bin文件夹下查看

另外注意cmake的变量是要区分大小写的，例如 `set(EXECUTABLE_OUTPUT_PATH ../EXE)`,其中set大小写均可，但是后面的变量 `EXECUTABLE_OUTPUT_PATH`却只能大写

##### 4.实际操作

因为是多文件多目录的基础cmake使用演示，所以需要创建3个 `CMakeLists.txt`

- 在根目录下创建`CMakeLists.txt`,内容如下

```cmake
#指定cmake最低版本为3.0
cmake_minimum_required(VERSION 3.0)

#指定项目名称
project(swap_hello)

#包含头文件路径,这里使用相对路径
include_directories(inc)

#添加子文件夹以便于构建和编译时的搜索
add_subdirectory(swap)
add_subdirectory(hello)

#添加编译参数
add_compile_options(-g -Wall -std=c++11)

#指定可执行文件生成路径,这里也是采用相对路径，是相对于外部构建文件夹build的路径
#这里指定生成在根目录下的EXE文件夹
set(EXECUTABLE_OUTPUT_PATH ../EXE)

#生成可执行文件
add_executable(swap_hello main.cpp)

#链接库
target_link_libraries(swap_hello swap_lib hello_lib)
```

- 在swap文件夹下创建 `CMakeLists.txt`

```cmake
#包含该目录下所有源文件
aux_source_directory(. swap_SRC)
#生成静态库
add_library(swap_lib STATIC ${swap_SRC})
```

- 在hello文件夹下创建 `CMakeLists.txt`

```cmake
#包含该目录下所有源文件
aux_source_directory(. hello_SRC)
#生成静态库
add_library(hello_lib STATIC ${hello_SRC})
```

##### 5.构建项目并编译生成可执行文件

###### 5.1 生成build文件夹，并执行cmake进行构建

- 如果有安装cmake插件，在根目录的CMakeLists.txt界面下，直接`ctrl+shift+P`,搜索 `cmake:configure`,确认选择`GDB`,再确认选择`gcc`,vscode会自动创建`build`文件夹以及执行`cmake`命令

<b>手动步骤</b>：

- 在vscode终端新建build文件夹，进入build文件夹目录，执行cmake指令

  ```cmake
  mkdir build	
  cd ./build
  cmake ..	//这里是以根目录的CMakeLists.txt为构建对象，而它在build的上层目录（根目录）
  ```

成功构建之后，在根目录下会出现一个 `build`和 `EXE`文件夹，并且build文件夹下会有一个构建成功的makefile文件

![build-EXE](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/build-EXE-16281684943941.png)

###### 5.2 make编译

依旧在build目录下，执行 `mingw32-make.exe`

![make编译](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/make%E7%BC%96%E8%AF%91.png)

成功后根目录下的EXE文件会出现我们之前定义名为 `swap_hello.exe`的可执行文件

现在我们来运行验证一下          ![程序验证2](../images/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95/%E7%A8%8B%E5%BA%8F%E9%AA%8C%E8%AF%812.png)

成功。

##### 6.总结

文中介绍了如何利用cmake构建项目，编译成功的基本方法。

建议对编写CMakeLists.txt的常用变量，以及常用指令进行了解，文中仅仅代表了基本的编写方法，cmake更多的东西（如静、动态库，宏，导入外部库等）有待自己在实践中发掘

> 我将在[下一篇](https://hlss-lxh.github.io/2021/08/05/windows%E4%B8%8B%E4%BD%BF%E7%94%A8cmake-vscode%E5%AE%9E%E7%8E%B0%E5%A4%9A%E6%96%87%E4%BB%B6%E5%A4%9A%E7%9B%AE%E5%BD%95%E7%BC%96%E8%AF%91%E5%92%8C%E8%B0%83%E8%AF%95%EF%BC%882%EF%BC%89/#more)中讲述如何基于cmake构建的项目使用vscode进行调试

