---
title:  "Jekyll使用"
date:   2017-10-10 9:00:00
categories: 工具
---

一直在找合适的主题，也觉得[掌心中的demo][zhangxin]主题效果不错，但最终决定使用kasper，还是因为看了Rei的[简单就好][simple-is-better]

<!-- More -->

不过初次使用kasper，就遇到了下面的问题：

# 启动错误
```
Incremental build: disabled. Enable with --incremental
Generating...
 Deprecation: Collection#map should be called on the #docs array directly.
              Called by /Users/xiao_dli/work/kasper/_plugins/rssgenerator.rb:46:in `block in generate'.
 Deprecation: Collection#count should be called on the #docs array directly.
              Called by /Users/xiao_dli/work/kasper/_plugins/rssgenerator.rb:49:in `rescue in block in generate'.
 Deprecation: Collection#reverse should be called on the #docs array directly.
              Called by /Users/xiao_dli/work/kasper/_plugins/rssgenerator.rb:51:in `block in generate'.
 Deprecation: Document#title is now a key in the #data hash.
              Called by /Users/xiao_dli/work/kasper/_plugins/rssgenerator.rb:53:in `block (3 levels) in generate'.
 Deprecation: Document#excerpt is now a key in the #data hash.
              Called by /Users/xiao_dli/work/kasper/_plugins/rssgenerator.rb:55:in `block (3 levels) in generate'.
```

最后发现原来是不能使用jekyll新的版本3.0.0（3.0.1也不行），通过以下方式解决

```
gem uninstall jekyll
gem install jekyll -v '<3.0.0'
```

# 路由失败
个人Blog网站上通过首页无法进入具体的文章页面，解决方法是：
* _config.yml中"baseurl: /"的":”后面只能是一个空格而不是一个tab
* index.html中: href中删除"site.baseurl"部分.

# 参考
* [采用Jekyll + github + pygments构建个人博客的最终说明](http://www.jianshu.com/p/609e1197754c)
* [Jekyll指南](http://jekyll.bootcss.com/docs/home/)
* [Jekyll变量 和 Jekyll模板语法教程](http://higrid.net/c-art-jeklly_template_data.htm)
* [Jekyll 语法简单笔记](http://ju.outofmemory.cn/entry/98471)
* [Jekyll themes](http://www.zhanxin.info/themes.html)