---
title:  "Vagrant使用"
date:   2017-07-27 9:00:00
categories: 
- 工具
- Vagrant
---

Vagrant是一个基于Ruby的工具，用于创建和部署虚拟化开发环境。它使用Oracle的开源VirtualBox虚拟化系统，使用 Chef创建自动化虚拟环境。

<!-- More -->

# 前提
* [Oracle VirtualBox下载](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)并安装

# 安装
* [Vargant下载](https://www.vagrantup.com/)并安装(安装后会提示重启系统)

# 说明
* 以下操作都是在windows命令行下操作;
* 使用$VBox代表当前工作目录(如:F:\Work\VBox);
* 考虑到网络下载问题，建议先从网络下载后再采用本地方式初始化;
* 本地最好放在$VBox目录下，本人测试时使用`G:\Software\OS\lucid32.box`进行本地初始化不成功(可能对路径支持有bug),错误如下:
>  F:\Work\VBox>vagrant box add base G:\Software\OS\lucid32.box
==> box: Adding box 'base' (v0) for provider:
    box: Downloading: file://G:/Software/OS/lucid32.box
    box:
An error occurred while downloading the remote file. The error
message, if any, is reproduced below. Please fix this error and try
again.
Couldn't open file /Software/OS/lucid32.box.

# 使用
1. 初始化VBox
	* 本地方式： `vagrant box add base lucid32.box`;
	* 网络方式： `vagrant box add base http://files.vagrantup.com/lucid32.box`
2. 初始化配置: `vagrant init`,执行后会生成配置文件Vagrantfile,打开修改配置即可，可参考[配置修改](http://www.kissthink.com/archive/v-a-g-r-a-n-t-shi-yong-jian-jie.html)
3. 加载: `vagrant up`
4. 连接: `vagrant ssh`, 缺省用户名/密码都是vagrant

# 设置桥接
* 方式1: config.vm.network "public_network", :bridge => 'en1: Wi-Fi (AirPort)'[详见](http://www.tuicool.com/articles/v6ZnUzm)
* 方式2: config.vm.network :public_network [详见](https://blog.khsing.net/2013/07/vagrant.html)

# VeeWee  
* 查看支持的模板: `veewee vbox templates`;  
* 根据支持的模板生成: `veewee vbox define '<box_name>' 'windows-7-enterprise-i386'`;  
* 修改配置文件(./definitions/<box_name>/define.rb), 修改`:iso_file`内容为实际操作系统iso文件名(此文件放在./iso目录下);
* 安装: `veewee vbox build <box_name>`
* 生成box: `veewee vbox export <box_name>`

# 参考
* [如何制作一个vagrant的base box](http://blog.163.com/ly_89/blog/static/186902299201412125252320/)
* [用veewee创建vagrant的虚拟机](http://www.larrycaiyu.com/2011/11/04/veewee-create-vm.html)
* [veewee使用入门](https://github.com/larrycai/blog/blob/master/cn/veewee_create_vm.mkd)
* [使用Veewee](http://v2ex.com/t/90141)
* [已有的box库](http://www.vagrantbox.es/)
* [使用Vagrant在Windows下部署开发环境](http://blog.smdcn.net/article/1308.html)