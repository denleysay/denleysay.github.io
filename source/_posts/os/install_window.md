---
layout: post
title:  windows系统安装
date:   2022-04-29 16:49:08
categories:
  - 操作系统
---

针对本人[MAC下的虚拟机](/software/mac/vmware_fusion/vmware_fusion_11.dmg)win7系统
<!-- More -->

* 先参考[通用系统安装](/coder/install_os/)
* OS下载：[个人](https://msdn.itellyou.cn/)或者[微软下载中心](https://www.microsoft.com/zh-cn/software-download)，注意`微软下载中心的”Oct 2018"版本有问题，应选”April 2018“`
* 使用[老毛桃](https://www.laomaotao.net/)制作windows启动盘
* 运行win7激活工具

# 软件
优先使用绿色软件

## 必选
* [驱动精灵万能网卡版](http://www.drivergenius.com/)

## 可选
* 烧录工具：[stc-isp-15xx-v6.86S](/software/win7/stc-isp-15xx-v6.86S.zip)
* SD卡格式化：[SDFormatter](/software/win7/SDFormatter.zip)
* SD卡备份与恢复：[Win32DiskImager](/software/win7/Win32DiskImager.zip)
```shell
恢复: 选择指定image文件后Write
备份: 选择指定image文件(虚拟机中可使用映射)后Read，但只能手动输入保存文件路径，如：C:\RaspberryPi.img
```
* everything
* win10截图：snippingtool
* windows下的包管理工具：[scoop](https://sspai.com/post/52496)、[baulk](https://github.com/baulk/baulk)
* Total Commander
* autohotkey

# 虚拟机
## v1
存放[位置](/software/win7/win7_64_v1)

* 正版win7 64位旗舰版
* 已安装TeamViewer
* 已映射下载/work快捷方式

## v2
存放[位置](/software/win7/win7_64_v2)，在v1基础上，安装了STC开发环境[keil5](/software/win7/keil)

* 执行MDKxxx.exe安装IDE,缺省安装了ARM;
* 手动安装c51，期间出现的所有提示都选skip;
* 运行注册机
```shell
1. 右键以管理员身份打开IDE的桌面快捷方式;
2. 打开“File”的“License Management”，拷贝其CID编号;
3. 打开注册机keygen，粘贴CID编号;
4. target”选择arm，点击“Generate”，复制结果到“License Management”中，点“Add Lic”，激活ARM
5. 同样操作激活C51.
```
