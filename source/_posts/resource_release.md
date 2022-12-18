---
title: C++ 资源释放
date: 2009-10-23 14:50:00
categories: 
- 技术
- C++
---

在Windows编程中，GDI资源的泄露一直是需要引起C++程序员的高度关注，一不小心，就会在函数的中途正常退出或者中途抛出异常退出的地方遗忘掉释放前面申请的资源。本人也曾多次碰到这种问题，查阅了网上的资料，总是不能得到满意的解决。最近看了下boost中的库，才略有收获，也算是抛砖引玉吧。

<!-- More -->

要想解决上面的问题，就必须实现资源的自动释放，类的析构函数正好可以满足此要求，就象标准库中智能指针就是这么实现的，但问题在于我们的参数个数，参数类型的不确定性。虽然重载和模板可以解决此问题(这也是我在网上看到的解决方法)，但模板类的参数不具备自动推导能力（经传入成员函数参数值推导出模板参数类型），而且过多的模板偏特化也不是我所擅长的，最主要是代码的移植性无法保证。

本文主要利用的boost中的bind库，觉得仿函数的功能跟自己当前的需求不远了，因为它们的共同点有:

1. 可以接收任意多个模板参数(没有具体验证，至少是9个吧),
2. 可以利用函数对模板参数类型的推导能力，省去了参数类型的指定。

唯一不同的是bind后的仿函数是立即执行，不能具有类的析构函数自动执行的优点。目前需要解决的问题是推迟执行期，也既把operator()函数移到析构函数中执行，这就需要保存boost::bind(....)返回的对象，通过类的构造函数去保存，然后在析构函数中执行operator()就可以了。

思路是出来了，但问题是boost::bind(...)函数返回的类型不确定，对象通过类模板是可以保存，但类没有自动推导能力，还是无法实现，这里我就利用了boost::any的原理，正好解决了此问题，而且它也可以用于函数的延迟执行。详见以下使用方法：

1. 实现类似于boost:;any的类，主要完成资源的自动释放。实现如下:

```cpp
//SrcRelease.h头文件
#ifndef _SRCRELEASE_INC_
#define _SRCRELEASE_INC_

class CSrcRelease
{
public: 
    template<typename T>
    CSrcRelease(const T & value)
        : m_pHelder(new Helder<T>(value))
    {
    }

    ~CSrcRelease()
    {
        delete m_pHelder;
    }

private: 
    class IHelder
    {
    public:
        virtual ~IHelder() {}
    };

    template<typename T>
    class Helder : public IHelder
    {
    public: 
        Helder(const T & value)
            : held(value)
        {
        }
        ~Helder() 
        {
            held();
        }

    public: // representation
        T held;
    };

    IHelder* m_pHelder;
};

#endif //_SRCRELEASE_INC_ 
```

2. 下载boost库，因为只用到了boost::bind库，所以无需编译. 将头文件目录加入vs2005中。
3. 客户端调用

```cpp
//main.cpp
#include "SrcRelease.h"
#include <iostream>
#include <Windows.h>
#include <boost/bind.hpp>
#include <cassert>

void _stdcall InvokeStr(const char* szValue)
{
    std::cout<<szValue<<std::endl;
}

bool _stdcall InvokeStr(const char* szValue, int a, int b)
{
    std::cout<<szValue<<"\ta: "<<a<<"\tb: "<<b<<std::endl;
    return true;
}

int main()
{
    //由于API都是_stdcall调用，而vs2005环境都是默认_cdecl，所以需要修改vs2005环境
    HBITMAP hBitmap=reinterpret_cast<HBITMAP>(LoadImage(NULL, L"test.bmp", IMAGE_BITMAP, 0, 0, LR_LOADFROMFILE));
    assert(hBitmap!=NULL);
    CSrcRelease aBitmapRelease(boost::bind(&DeleteObject, hBitmap));

    std::cout<<"Invoke Outer Before"<<std::endl;
    CSrcRelease aRelease(boost::bind(&InvokeStr, "Invoke Outer After", 8, 5));

    {
        std::cout<<"Invoke Inner Before"<<std::endl;
        CSrcRelease aRelease(boost::bind(&InvokeStr, "Invoke Inner After"));
        std::cout<<"Invoke Inner Middle"<<std::endl;
    }

    std::cout<<"Invoke Outer Middle"<<std::endl;
    return 0;
} 
```
以上代码在winxp+vs2005下测试通过，如有疑问，欢迎联系: ietj@mail.21cn.com


# 参考
* [阅读原文](http://www.cppblog.com/ietj/archive/2009/10/23/99287.html)
