---
title:  "微信开发"
date:   2017-09-17 9:00:00
categories: 技术
tags: [WeChat]
---

微信开发，你参加了吗？

<!-- More -->

# 小程序
* WXSS中导入wxss文件时，只能是app内部文件，而且只能形如：`@import "/assets/stylesheets/weui.wxss"`，不能形如：`@import url("/assets/stylesheets/weui.wxss")`

## 使用图标库
1. 下载[font-awesome字体包](http://fontawesome.io/);
2. 打开[Transfonter网站](https://transfonter.org/)，上传字体`fonts/fontawesome-webfont.ttf`（理论其它文件格式也可以转换，并未尝试），选择base64编码，convert后下载;
3. 下载得到的包中有stylesheet.css文件，打开后与`css/font-awesome.css`中的内容合并到`font-awesome.wxss`新文件中，将此文件import到微信小程序的app.wxss文件中就可以了;
   因stylesheet.css文件中已有`@font-face`节，`css/font-awesome.css`最上面相应此节就不需要了
4. 使用如下:

```
<i class="fa fa-spinner fa-spin fa-3x fa-fw"></i>
<span class="sr-only">Loading...</span>

<i class="fa fa-circle-o-notch fa-spin fa-3x fa-fw"></i>
<span class="sr-only">Loading...</span>

<i class="fa fa-refresh fa-spin fa-3x fa-fw"></i>
<span class="sr-only">Loading...</span>

<i class="fa fa-cog fa-spin fa-3x fa-fw"></i>
<span class="sr-only">Loading...</span>

<i class="fa fa-spinner fa-pulse fa-3x fa-fw"></i>
<span class="sr-only">Loading...</span>

```
# 公众号
## 准则
1. 订阅号优先
2. 怎么写才适合订阅号这一表现形式
3. 保证一星期内至少2篇

## 注意事项
1. 不发今天才创建的主题
2. 手机浏览整体效果
3. 链接提示是否存在
4. 检查链接有效性
5. 是否有更好的图片表示主题
6. 不要轻易删除
7. 删除再群发时不要推给所有人
8. 检查整体合理性
9. 下午4：00发

# 服务号
* JS接口安全域名： domain_name.com
* 网页授权回调页面域名: www.domain_name.com
* 网页回调网址redirect_uri: urlencode(http://www.domain_name.com)转换  

# 参考
* [微信小程序使用font-awesome图标库](http://blog.csdn.net/abczdefg/article/details/54407526)