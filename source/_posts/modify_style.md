---
title: ModifyStyle调用不起作用
date: 2011-12-08 09:50:57
categories: 
- 技术
- C++
---

在使用WTL做CListBox子类化时，使用 ModifyStyle(0, LBS_OWNERDRAWFIXED)不起作用;

<!-- More -->

# 原因
并不是所有的风格都可以动态利用ModifyStyle/ModifyStyleEx()函数增加和去除，有些风格比如 LBS_HASSTRINGS | LBS_OWNERDRAWFIXED| LBS_OWNERDRAWVARIABLE 就只能在创建窗口的时候指定（其后再增加是无效的），也就是说你只能创建的之前指定。

# 解决
自己动态创建控件，或在待子类化的ListBox控件中指定属性（如LBS_HASSTRINGS | LBS_OWNERDRAWFIXED| LBS_OWNERDRAWVARIABLE ）

# 参考
* code   project中的解释

>  
It   is   not   possible   to   change   these   styles   at   runtime   even   though   ModifyStyle()   may   give   the   impression   it   does.   If   you   want   turn   the   Sort   style   on   and   off   for   example   it   is   best   to   construct   the   List   box   by   calling   new   and   Create   then   deleting   it   and   creating   a   new   one   when   the   style   is   to   be   changed.   Alternatively   you   can   have   2   List   box   superimposed   and   hide   the   one   with   the   incorrect   style. 

也就是说,无办法动态改变那些只能在创建时设置地style

* ListBox的样式说明

| 风格取值 |描述 |
|---|---|
| LBS_EXTENDEDSEL	 |能通过Shift键或者鼠标进行多选 |
| LBS_HASSTRINGS	 |可以用GetText函数得到列表框里选项的字符串 |
| LBS_MULTICOLUMN	 |指定列表框以多列形式显示内容 |
| LBS_MULTIPLESEL	 |用户可以选择多个项 |
| LBS_NOINTEGRALHEIGHT	 |当应用程序创建列表框时，列表框的大小由系统指定 |
| LBS_NOREDRAW	 |列表框不响应用户的修改，可以通过发送WM_REDRAW 来取消该设置 |
| LBS_NOTIFY	 |让主窗口接收列表框的任何改变的消息 |
| LBS_OWNERDRAWFIXED	 |主窗口负责列表框的重画，列表框里每项的高度相同(因此不会有WM_MeasureItem消息) |
| LBS_0WNERDRAWVARIABLE	 |主窗口负责列表框的重画，列表框里每项的高度可以变化 |
| LBS_SORT	 |按各项名称的字母排序 |
| LBS_STANDARD	 |等同于LBS_SORT和LBS_NOTIFY |
| LBS_USETABSTOPS	 |允许用户使用Tab键在各项中切换 |
| LBS_WANTKEYBOARDINPUT	 |输入焦点在列表框时，任何键盘输入都会向父窗口发送WM_VKEYTOITEM或者WM_CHARTOITEM消息 |
| LBS_DISABLENOSCROLI	 |当列表框的项不够时，垂直滚动条禁活：没有该属性时，滚动条隐藏 |

* [查看原文](http://blog.csdn.net/ietj/article/details/7052257)