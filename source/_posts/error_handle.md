---
title: C++错误处理
date: 2008-03-01 23:28:11
categories: 
- 技术
- C++
---

在2003年9月的JAOO会议上，Bill Venners对Bjarne Stroustrup采访时，BS说：
>  
C++的一个问题是有太多的库，但是没有一个大卖场有所有这些库。C++不是对GUI支持不好。C++有GUI库。问题是C++有25个GUI库。有些 GUI库不错。但是人们说：“可是C++没有标准GUI库。”Python不一样。人们知道到哪里去找Python的GUI库。所以C++的问题是有太多库，但是所有的库市场宣传都做的不够（It's a problem of riches, plurality, and a lack of marketing）。搞C++的看来都很穷，没有钱来提供一个可以找到所有库的大卖场。

我想，这样的局面也导致了目前C++对错误没有一个统一的处理方式，不象JAVA可以统一采用异常的错误处理方式（我曾看过经JMS重写的CMS--ActiveMQ CMS，里面一律采用异常，包括构造/析构函数），因为它需要兼顾C/C++/SDK中各自不同的错误处理方式。 

<!-- More -->

* 不应该发生的错误如果发生了，那就是程序员的过错，此时就该使用ASSERT断言了。  
  断言采用如下形式:
```
unsigned int Employee::GetID()
{
   assert(m_nID!=0 && "Employed ID is invalid(must be nonzero");
   return m_nID;
}
```

* 不能用抛出异常来代替断言

* 核心模块(调用第三方API的模块)的信息一律通过异常抛出，这样可以知道错误发生的具体位置;
* 一个函数只应该有一个try/catch块;
* try/catch不要从函数头开始一直到函数尾，要求包含的语句尽可能的少(变量定义就可以不包含，因为构造/析构函数不应该势出异常;
* 错误码应该分类，每类错误最好定义一个起始值，后面的错误码依次在其上+1;
* 可以抛出int类型的异常，以便第三方调用时可以封装成API(因为他们都是通过返回值来确定调用正确与否); 

# 错误定位
主要针对Linux环境下的错误定位
这段时间，由于做的是Linux环境下的C++程序移植工作，总会碰到"段错误/已放弃"之类的问题，可以通过以下方式得到程序退出时的调用堆栈信息，方便错误的定位

打开生成core.xxx文件开关：设置ulimit -c unlimited，也可在环境变量中设置，以避免每次打开终端时都要进行设置；
执行linux下debug版本的应用程序: ./AppName；
程序出现"段错误/已放弃"而退出时，会在当前运行目录下产生 core.xxx文件(其中xxx是一串数字)；
使用gdb运行core.xxx文件: gdb ./AppName core.xxx；
在gdb>下执行info stack查看最后的堆栈,从堆栈中得到最后退出时的信息。
更多操作详情查看: Linux下发生段错误时如何产生core文件

* [阅读原文](http://www.cppblog.com/ietj/archive/2010/06/09/117488.html)

# 参考
* [考虑错误情况](http://msdn2.microsoft.com/zh-cn/library/ms809695.aspx)
* [错误处理(Error-Handling)：为何、何时、如何](http://blog.csdn.net/pongba/archive/2007/10/08/1815742.aspx)
* 《契约式设计的理解及其在c/c++中的应用》
* Robert Schmidt《C与C++中的异常处理》
* [阅读原文](http://www.cppblog.com/ietj/archive/2008/03/01/43542.html)