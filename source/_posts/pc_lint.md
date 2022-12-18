---
title:  PC-Lint使用
date:   2009-11-10 15:29
categories: 工具
tags: [C++]
---

PC-Lint是一款针对C/C++的代码静态分析工具，可以把它看作一种更加严格的编译器，不仅可以检查出一般的语法错误(当前编译器所作的工作)，还可以检查出那些符合语法要求但不易发现的潜在错误，如：

* else对if的就近匹配原则
* 优先级导致的错误
* 以0开头的数字(作为8进制处理)
* &&, ||, ==等的漏写

C/C++语言的灵活性带来了代码效率的提升，但也因其灵活性而带来了代码编写的随意性，另外C/C++编译器不进行强制类型检查，也带来了代码编写的隐患。PC-Lint识别并报告C/C++语言中的编程陷阱和格式缺陷的发生。它进行程序的全局分析，能识别没有被适当检验的数组下标，报告未被初始化的变量，警告使用空指针，冗余的代码等等。软件除错是软件项目开发成本和延误的主要因素。根据发现错误时间与成本成指数级的关系，PC-Lint能够帮你在程序动态测试之前发现编码错误。这样消除错误的成本更低。

<!-- More -->

正因为其重要性,在很多专业级的软件公司,如Microsoft中，PC-Lint检查是无错误无警告是代码首先要过的第一关。 本文将就PC-Lint在VC6上的使用作一简单的介绍：

# 下载与安装 
[官方网址](http://www.gimpel.com/)，但PC-Lint是一款共享软件，因此只能通过其它途径下载使用了，本人使用的是PC-Lint8.0w版本。

# 配置 
文件解压后可以看到如下文件：(解压目录为E:\Work\DevLib\pclint,以下将用$(PC-Lint)代替)  
![](/assets/images/PCLint-Directory.JPG)

可以使用Config.exe的向导功能配置个针对自个环境的lnt文件，我这里是直接编辑文本文件std.lnt，保存在$(PC-Lint)目录下，其中VC6的安装目录为：`d:\Program Files\Microsoft Visual Studio\VC98。std.lnt`内容如下： 

```
au-sm.lnt 
co-msc60.lnt 
env-vc6.lnt 
lib-mfc.lnt 
lib-stl.lnt 
lib-w32.lnt 
lib-wnt.lnt 
lib-atl.lnt 
options.lnt -si4 -sp4 

-i"d:\Program Files\Microsoft Visual Studio\VC98\Include" 
-i"d:\Program Files\Microsoft Visual Studio\VC98\atl\include" 
-i"d:\Program Files\Microsoft Visual Studio\VC98\MFC\include"
```

在std.lnt中的options.lnt属于新增文件，用于增删某些反映全局编译信息的选项。如"-e783"，用于关闭警告信息：当文件不是以空行结束时。
整合到IDE 
打开VC6菜单上的工具--->定制部分，在工具选项卡中增加PC-Lint项，参数设置如下图所示：  
添加快捷键 
打开工具菜单，首先看PC-Lint位于工具组中的很几项（我这里是第8项）,然后点击定制--->键盘，在"分类"中选择工具，在命令中选择"UserTool8"，将光标移到新建快捷键中，此时在键盘上同时按下"CTRL+F12"键(注意看提示是否此快捷键已经使用)，点"OK"，就可以用CTRL+F12执行PC-LINT了 
使用 
设置完成后，在菜单的工具栏中就有了PC-Lint项了，对当前打开的C/C++文件，执行此项操作就可以在Output窗口中输出执行信息了，如下图所示：  
一切正常的话, return code为0,本文这里出现2个错误是由于使用了命名空间std,如不使用则正常，目前也不知道什么原因，main.cpp代码如下： 

```
#include <iostream> 
int main(int argc, char* argv[]) 
{ 
(void)argc; 
(void)argv; 
std::cout<<"Hello, World!\n"; 
return 0; 
} 
```
# 参考 
* [如何在Source Insight中配置PC-Lint](http://blog.csdn.net/minico/article/details/1775554)
* 在IDE中PC-Lint整个项目文件
* PCLint 在VC6.0下的配置使用
* [阅读原文](http://www.cppblog.com/ietj/archive/2009/11/10/100620.html)