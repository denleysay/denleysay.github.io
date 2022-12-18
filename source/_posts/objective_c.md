---
title: Objective-C学习笔记
date: 2009-08-25 08:39:00
categories: 技术
tags: Objective-C
---

目前好象只有Apple使用Objective-C作为其支持的语言吧，那它与C++相比到底如何呢？

<!-- More -->

与C++的不同之处有：
* O-C中所有的类都必须继承自NSObject。
* O-C中所有对象都是指针的形式。
* O-C用self代替this。
* O-C使用id代替void*。
* O-C使用nil表示NULL
* O-Ck只支持单继承。
* O-C使用YES/NO表示TRUE/FALSE
* O-C使用#import代替#include
* O-C中用消息表示类的方法，并采用[aInstance method:argv]调用形式。
* O-C支持反射机制
* O-C支持Dynamic Typing, Dynamic Binding和Dynamic Loading。

与C++的相同之处有：
* 与C共享的部分一致。
* 可以使用assert(BOOL), 一般用NSCParameterAssert(BOOL)代替。

O-C中的命名前缀说明：
* NS-：NextStep
* CF-：Core Foundation
* CA-：Core Animation
* CG-：Core Graphics
* UI-：User Interface

O-C中的消息特殊性：  
调用消息的类可以不知道如何响应这个消息。如果它不知道如何处理这个消息，它会自动的将这个消息转给其他的类，比如它的父类。
调用消息的类可以是nil。在C++中，在使用类方法之前，我们都需要检查对象是否为空，所以在实现析构函数的时候，常会有如下的代码，如if (var) { delete var; } 但是在objective c中，我们就可以直接写[var release]; 即使var == nil, 也不会有问题。
O-C中的函数声明格式有：
```
-/+ (return type) function_name;

-/+ (return type) function_name : (parameter type) parameter;

-/+ (return type) function_name : (parameter type) parameter1 otherParameter : (parameter_type) parameter2
```

以上参数说明:-表示一般函数，+表示静态函数。otherParameter是参数的别名(第一个参数的别名省略),在函数调用时方便指定。

O-C中的构造/析构函数
* O-C中的init()/release()对应于C++的构造/析构函数。alloc()/dealloc()也就对应于C++的new和delete,其中的dealloc()由于引用计数的自动调用而不用手动调用。
* O-C中父类的init()/release()函数需要子类的手动调用。而且每次都必须调用。不同于C++的自动调用。
* 构造函数(- (id) init)调用形如：CSample* pSample=[CSample alloc] init];其中alloc(+ (id) alloc)是继承来的static函数，init是继承来的一般函数，如重写一般函数时，则相当于C++的覆盖(不带参数)或重载(带参数)。
* 析构函数(- (void) release)将引用计数减1，当=0时父类的release()会自动调用dealloc(- (void) dealloc);

当O-C没有数据成员时，可省略{},建议保留。

继承下来的方法，如：-(id) init可以头文件中省略，建议保留

0-C中只有数据成员的访问限制，没有方法的访问限制。

同C++一样，数据成员有三种访问限制public, protected, private，缺省是protected。

示例：
```
@interface AccessExample: NSObject { 
@public 
int publicVar; 
@protected 
int protectedVar; 
@private 
int privateVar; 
} 
@end
```

方法的访问限制可通过Category实现

示例：
```
@interface MyClass

- (void) sayHello {

NSLog(@"Hello"); 

} 

@end 

@interface MyClass(Private)

- (void) kissGoodbye;

@end 
```

O-C中没有类的静态变量，只有全局变量

O-C中的数组NSArray可以保存不同类型的数据。

O-C也支持run-time时的类类型检查
```
- (BOOL) isKindOfClass: classObj 
用于判断该对象是否属于某个类或者它的子类

- (BOOL) isMemberOfClass: classObj 
用于判断该对象是否属于某个类（这里不包括子类)

- (BOOL) respondsToSelector: selector 
用于判断该对象是否能响应某个消息。这里，我们可以将@selector后面带的参数理解为C++中的函数指针。 
注意：1）不要忘了@ 2）@selector后面用的是()，而不是［］。3）要在消息名称后面跟：，无论这个消息是否带参数。如：[pSquare respondsToSelector:@selector(Set: andHeight:)]。

+ (BOOL) instancesRespondToSelector: selector 
用于判断该类是否能响应某个消息。这是一个静态函数。

-(id) performSelector: selector ：调用对象的selector方法。
```

conformsToProtocol 类似于respondsToSelector ，用于动态检查某个对象是否遵守某个协议。

Category：在没有源代码的情况下，为一个已经存在的类添加一些新的功能

只能添加新的方法，不能添加新的数据成员

Category 的名字必须是唯一的

Protocol：相当于C++中的纯虚类

形如：`@interface MyDate: NSObject <Printing> { } @end`  
使用：`MyDate * dat = [[MyDate alloc] init]; id<Printing> var = dat; [var print]`  
说明：我们首先声明了Printing 协议，任何遵守这个协议的类，都必须实现print 方法。在Objective C中，我们通过<>来表示遵守某个协议。当某个类声明要遵守某个协议之后，它就必须在.m文件中实现这个协议中的所有方法。使用id<Printing> 作为类型，而不是象C++中的Printing* var。
IBOutlet, IBAction: 这两个东西其实在语法中没有太大的作用。如果你希望在Interface Builder中能看到这个控件对象，那么在定义的时候前面加上IBOutlet，在IB里就能看到这个对象的outlet，如果你希望在Interface Builder里控制某个对象执行某些动作，就在方法前面加上(IBAction)。
尽量避免在一行语句中进行两层以上的嵌套
消息转发：`- (void) forwardInvocation: (NSInvocation*)anInvocation;`

# 参考
* [阅读原文](http://www.cppblog.com/ietj/archive/2009/08/25/94346.html)
* [iPhone开发学习笔记](http://blog.csdn.net/huanglx1984/archive/2009/07/06/4325377.aspx)