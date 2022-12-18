---
title:  "安装Google Play"
date:   2017-10-05 9:00:00
categories: 杂项
tags: [Firewall]
---

为了能在手机上安装Google play,不得不打开ROOT权限，以下是本人在SM-G5510手机上的操作过程。

因为不是进行刷机，自认为变砖的可能性没有，也就对可能造成的后果没有必要的准备，最后证明是错的

<!-- More -->

# 安装步骤
以下涉及到的软件都是在手机自带应用商店中下载安装的；

1. 安装“谷歌安装器”；安装一切顺利，google play也有了，但打开后出现”启动停止“问题；
2. 网上说可能是没”打开ROOT权限",找了一堆的安装后都不成功，好象都是没有针对此手机型号的；
3. 找到相匹配的呢，又是注册，又是收费￥21；
4. 最后选择QQ联系收费￥5的，收到一个压缩包`SM-G5510ZCU1APK1_ROOT_2.78.rar`；
5. 线上指导安装CROM service，OK；
6. 关机，按`声音下键+HOME键+电源键`键开机，连上电脑更新，不OK，没驱动；
7. 安装驱动精灵后安装驱动，OK;
8. 电脑上通过`odin`安装`AP`;
8. 重启手机后提示输入混合密码，原因是没有”关闭锁屏功能“；
9. 已经进不了手机，只好关机后按`声音上键+HOME键+电源键`键进行双清；
10. 因理解错误，执行了`swipe cache`后还执行了`swipe data`, 导致全部数据丢失；
11. 开机后一切顺利，但还是会出现`启动停止`的问题；
12. 将已经安装的`google play`相关的卸载，重新安装“谷歌安装器”，不OK;
13. 继续卸载后安装“GO谷歌安装器”，OK;
14. 因没有翻墙,`google play`总是显示“正在核查信息”；
14. 当然直接VPN就可以了，以下我是通过修改hosts实现的；
15. 商店中下载`文件管理器`之类的，不能修改；
16. 网上下载`root explorer`最新版本4.8.5，还是不能修改；
17. 网上下载`root explorer`版本3.3.7，OK;
18. 将网上下载的hosts文件覆盖/etc/hosts，重启手机，OK
19. 安装superuser

注：做`双清`时`电源键`是确认键，`声音键`是上下选择键

# 正确步骤
1. 手机连接到电脑，安装好驱动;
2. 备份数据，关闭手机的锁屏功能，手势密码；
3. 安装CROM service;
4. 关机后按`声音下键+HOME键+电源键`键开机；
5. 电脑上通过`odin`安装`AP`;
6. 一切顺利的话转下一步，不顺利则将已经安装的`google play`相关的卸载，重新安装“谷歌安装器”；
8. VPN翻墙；
7. 安装superuser

# 文件传输
针对目前的手机SM-G5510与Mac Air之间的文件传输
1. 安装Mac版本的[S换机助手](http://www.samsung.com/cn/apps/smart-switch/);
2. 安装后打开软件，单击`Galaxy On5 2016青春版`右边的下拉按钮，再单击`内部存储器`的右边文件夹;
3. 在显示的目录树中找到目标文件，拖动到Mac Air下，就完成了文件的传输.

# 翻墙工具
 * [目前使用的VPN](https://cloud.digitalocean.com)
 * [Docker + DigitalOcean + Shadowsocks 5分钟科学](http://blog.sina.com.cn/s/blog_6d95ca170102x71b.html)
 * 各平台下的[ShadowSocks客户端下载](https://blog.csdn.net/weixin_41557898/article/details/81083114)
 * 可以考虑的[Nord VPN](https://join.nord-for-china.com/zh/order/?coupon=3yholiday): $99/3年
 * [web hosts](https://laod.org/hosts/2016-google-hosts.html)
 * [github hosts](https://github.com/racaljk/hosts)
 * 不能翻的情况下可以使用biying,bilibili找资料，视频
