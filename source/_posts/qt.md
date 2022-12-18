---
title:  "QT使用"
date:   2016-07-19 9:00:00
categories: 
- 技术
- C++
tags: [Third-Party]
---

QT，你用它做过什么？

<!-- More -->

* 绕开QT官方的注册进行[下载](http://download.qt.io/official_releases/)
* 可以自定义安装QT, 不安装android相关
* 通过vs2017安装qt的addin：通过“工具和扩展”菜单搜索"QT"
* 通过”Qt VS Tools“=>"Qt Options"菜单设置qt的安装位置; 如项目是从另一系统迁移来的，也要设置”Qt Project Options”=>"Properties"=>"Version"

# 链接问题
VS2008下的QT App在包含Q_OBJECT宏的头文件时，会提示链接错误

原因是在处理Q_OBJECT宏的方式上,Qt其实是在qmake生成的makefile里面调用了Qt\bin\moc.exe，而VS2008不会，

做如下配置：假设对包含Q_OBJECT宏的aa.h文件进行处理,生成一个moc_aa.cpp
  1. 点击aa.h右键在属性里面加一个Custom Build Step；
  2. 选取所有配置
  3. Custom Build Step->General->Command Line里面加上
  $(QTDIR)\bin\moc.exe $(InputPath) -o$(InputDir)\moc_$(InputName).cpp
  4. Custom Build Step->General->Outputs里面加上moc_$(InputName).cpp