---
layout: post
title:  "写在前面"
date:   2007-12-23 11:30
---

欢迎来到晓月刀的博客！！！

<!-- More -->

在这里，我主要会放些个人文章，比如工作总结，生活感悟之类的

下面还是用coder跟大家打个招呼吧

* c/c++方式

```c
#include <iostream>
#include <cassert>

std::string capitalize(const char* value)
{
    assert(value != NULL && strlen(value) != 0);
    std::string result = value;
    result[0] = toupper(result[0]);
    return result;
}

void hello(const char* name)
{
    std::cout << capitalize(name).c_str() << ", welcome to xiao_dli's blog!";
}

int main(int argc, const char* argv[])
{
    hello("everyone");
    return 0;
}
```
* Ruby方式

```ruby
def hello(name)
  puts "#{name.capitalize}, welcome to xiao_dli's blog!"
end

hello("everyone")
#=> Everyone, welcome to xiao_dli's blog!
```

# 参考
* CppBlog上发布的第一篇博客: [新的起点,新的开始](http://www.cppblog.com/ietj/archive/2007/12/23/39341.html)
> 终于在这个圣诞前夕,我的第一个个人 Blog开通了,很高兴有了自己的一片新天地.
> 在这里,将记录我个人生活与技术的点点滴滴,也将印证我的个人历程.
> 希望能够一路走好!我也坚信这一目标能够实现!

* 为前置此文件，本文使用了第一篇博客的发布时间，真实时间其实是`2015-11-17 12:00:05`