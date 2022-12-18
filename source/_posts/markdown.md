---
title:  "Markdown解析器"
date:   2018-01-15 21:02:00
categories: 
- 工具
- Markdown
---

> 怎样才能做到一份Markdown文本，多处平台的引擎渲染后效果都一样呢？

目前主流的渲染引擎(后续统称为解析器)有
* Rediscount
* Redcarpet
* BlueCloth
* kramdown：[语法](https://kramdown.gettalong.org/syntax.html)

跟C++的编译器类似，各解析器对Markdown的语法支持也不一致，

准确的说，Github，包括Gitbook使用的Markdown都是在kramdown的基础上的扩展版本：<abbr title="GitHub Flavored Markdown">GFW</abbr>

最近，基于三个平台上中使用的解析器不同而导致渲染后的效果偶有差别，甚是困惑

现将问题罗列于此，以备高手帮忙解惑

<!-- More -->

# 问题

## 个人网站

所托管的Github Pages只支持kramdown，因此建议统一使用

采用的是Ruby on Rails框架，使用了`gem kramdown`，以下是内容转换函数

```ruby
  def markdown(content)
    return '' unless content.present?
    @options ||= {
        input: "GFM",
        syntax_highlighter: "rouge",
        syntax_highlighter_opts: {
            span: { disable: true },
            block: {
                inline_theme: "github",
                wrap: true,
                line_numbers: true
            }
        }
    }
    Kramdown::Document.new(content, @options).to_html.html_safe
  end
```

## 个人博客

采用了hexo框架，在light_cn布局中使用了`hexo-renderer-markdown-it`的Markdown渲染引擎，以下是相关配置

```
markdown:
  render:
    html: true
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
	- markdown-it-checkbox
  anchors:
    level: 1
    permalinkSymbol: ''
```

## 电子书

采用Gitbook框架缺省的Markdown渲染引擎，具体我也不知道是啥，[这里](https://chrisniael.gitbooks.io/gitbook-documentation/content/format/markdown.html)是gitbook支持的语法
   
# 参考
* [GFM语法](http://www.renmaomin.com/gfm-doc/)
* [Markdown入门指南](http://www.jianshu.com/p/468f111d8fd3)    
* [kramdown和markdown较大的差异比较](http://gohom.win/2015/11/06/Kramdown-note/)
* [KRAMDOWN学习笔记](http://caunion.me/study-notes-of-kramdown/)
* [Markdown 语法说明(中文版)](http://wowubuntu.com/markdown/)
* [Markdown的各种扩展](https://segmentfault.com/a/1190000000601562)
* [Markdown实用工具集](http://www.open-open.com/news/view/1be6464) 
* pandoc工具
