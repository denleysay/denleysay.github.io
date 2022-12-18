---
title:  "Nginx使用"
date:   2017-07-09 9:00:00
categories: 工具
tags: [Nginx]
---

以下是自己在ubutu上的一次二级域名配置总结

<!-- More -->

# 卸载
* [ubuntu14.04彻底删除nginx](http://blog.csdn.net/u010571844/article/details/50819704)

# 安装
* 对入门者不建议使用源码安装，会有太多的坑;
* 为确保安装的版本相对较新，先执行：`sudo apt-get update`;
* 执行安装：`sudo apt-get install nginx`。

目前安装的版本是1.11.9。

# 生成证书
* 通过阿里云申请免费证书: [申请服务器与域名](http://www.ifanr.com/minapp/779677)

# 配置
* 在`/etc/nginx/nginx.conf`中增加：`include /home/denley/backup/nginx/conf/*.conf;`;
* 在上述的conf目录下增加新规则www.conf/app.conf;（指定证书所在路径）

# 参考
* [nginx使用ssl模块配置HTTPS支持](https://www.centos.bz/2011/12/nginx-ssl-https-support/)
* [nginx服务器http重定向到https的正确写法](http://www.tuicool.com/articles/3UJNfmi)
* 配置文件: sudo vi /etc/nginx/sites-available/default
* 服务重启: sudo /etc/init.d/nginx restart
