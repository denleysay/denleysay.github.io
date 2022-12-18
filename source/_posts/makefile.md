---
title:  "makefile使用"
date:   2009-12-29 22:14
categories: 技术
---

makefile，至今还能存在，为什么呢？

<!-- More -->

# linux下动态库生成
* 生成libxxx.a  
  Makefile.am(lib_LIBRARIES=libxxx.a libxxx_a_SOURCES=xxx.cpp)
* 生成libxxx.so  
  Makefile.am(bin_PROGRAMS=libxxx.so libxxx_so_SOURCES=xxx.cpp libxxx_so_LDFLAGS= -fPIC -shared)
* 生成libxxx.la  
  configure.in中增加:AC_PROG_LIBTOOL  
  运行libtoolize --copy --force(-f -c)  
  Makefile.am(lib_LTLIBRARIES=libxxx.la libxxx_la_SOURCES=xxx.cpp)

# 参考
* [阅读原文](http://www.cppblog.com/ietj/archive/2009/12/29/104409.html)
