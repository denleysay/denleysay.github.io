---
layout: post
title: 树莓派系统安装
date: 2022-08-12 09:58
categories:
  - 操作系统
---

针对本人MAC系统下的安装
<!-- More -->

* 确保加密狗驱动已经安装;
* 激光雷达(主副)接入IP正常：192.168.0.10/11
* 远程控制电脑，打开rviz时如果被控电脑没接显示屏就打不开，这是因为rviz软件打开时需要使用屏幕参数。解决方法：在电脑上插一根HDMI转VGA的线即可

# ROS
* 运行如下命令失败时应先执行apt-get update -y
```bash
rosdep install --from-paths src --ignore-src --rosdistro=kinetic -y
```
* rosdep命令--from-paths可以是源代码的src目录，也可以是安装包的share目录：其中都有package.xml文件
* source /opt/ros/argusai/setup.bash后还是找不到工作空间，刷新一下环境：rospack profile
* launch文件[使用robot_upstart自启动](http://www.bubuko.com/infodetail-2614884.html)：目前问题是执行sudo systemctl daemon-reload && sudo systemctl start mine_job时：Failed to connect to bus: No such file or directory
* 出现问题请见参考下的1/2/3链接

# 远程工具
Teamviewer
* [服务器版本](https://pan.baidu.com/s/1O9RxtLc0fhmPhzhjM0xEwA):8yr6已破解，安装完后设置为“随系统启动”,不要升级;
* [客户端版本](https://pan.baidu.com/s/1A0DxukgZot5BZdtRLyikww):8bi7要一致

# 树莓派桌面版
推荐使用厂家预安装的系统
* 设置用户argusai，初始密码为: 1...8
* 选择自动登陆，其它使用默认设置
* 进入目录/usr/share/applications/拖动argusai图标到桌面生成快捷方式;
* 将终端pin到桌面快捷方式中;
* 删除多余的快捷方式：右键Unpin
* 第一次看不到wifi网络连接时，重启机器即可；
* 执行：
```bash
  # 所有操作开始前都应执行
  sudo apt-get update -y

  sudo apt-get install wget -y
  wget -O ros.sh raw.github.com ......
  sudo chmod +x ros.sh
  ./ros.sh

  # 所有操作完成后都应执行
  sudo apt-get autoremove clean
  sudo rm -rf /var/lib/apt/lists/*
```
* 执行脚本：`sudo argusai_deploy/scripts/ubuntu.sh`
* sudoers.sh脚本最后部分不起作用，手动加入argusai ALL=NOPASSWD:ALL到/etc/sudoers文件中；
* 雷达IP是否设置为: 192.168.2.2，设置详见[机智人雷达](https://app.yinxiang.com/notelink/Login.action?targetUrl=%2Fshard%2Fs26%2Fnl%2F5132076%2Ff08cc29a-96fe-4dae-a69c-cf384bd7a5cb%2F)
* 设置显示样式: 系统-->控制中心-->Mate Tweak-->界面-->将Ubuntu Mate改为Mutiny;
* 开机启动方式: 系统-->控制中心-->启动应用程序-->Add
```bash
mate-terminal -x /home/argusai/argusai/share/argusai/scripts/startup.sh
```
* 在使用gpio -v之前，先要安装wiringPi:
```bash
git clone https://github.com/WiringPi/WiringPi
cd wiringpi
./build
```
* 编译源码时要运行测试时，将misc/build.sh的末尾改为--catkin-make-args run_tests -j1

# 树莓派服务器版
ubuntu mate 18.04.2
## 准备
* 下载: [16.04](https://www.finnie.org/software/raspberrypi/ubuntu-rpi3/ubuntu-16.04-preinstalled-server-armhf+raspi3.img.xz)/[18.04](http://cdimage.ubuntu.com/ubuntu/releases/bionic/release/ubuntu-18.04.2-preinstalled-server-armhf+raspi3.img.xz)
* 有线网络畅通
## 安装
* [修改用户名和密码](https://blog.csdn.net/qq_40606798/article/details/82626528): 缺省为ubuntu/ubuntu
| 用户名 | 密码 | 备注 |
| --- | --- | --- |
| root | 3,3 | |
| argusai | s.a.. | |

* 脚本
```bash
# 修正时间(Asian/Shanghai)
sudo dpkg-reconfigure tzdata

# 修改root密码
sudo passwd root

# 更新
sudo apt-get update -y && sudo apt-get upgrade -y

# 安装
## gpu_mem=16
sudo vim /boot/firmware/config.txt
```

* 处启动
```bash
sudo apt-get install ros-kinetic-robot-upstart
rosrun robot_upstart install --symlink argusai/launch/argusai.launch

# 重启才会生效
sudo reboot
```

* 免密登陆
  1. 获取主机B的IP地址${B_IP};
  2. 在主机A生产密钥对: `ssh-keygen -t rsa -C denley@justtodo.com`， 会在.ssh目录下产生密钥文件
  3. 将此密钥传输到主机B，`ssh-copy-id -i ~/.ssh/id_rsa.pub ${B_USER}@${B_IP}`，这样主机A就可以免密访问主机B了

* gpio访问权限 参考[wiringPiSetup: Try running with sudo](https://raspberrypi.stackexchange.com/questions/40105/access-gpio-pins-without-root-no-access-to-dev-mem-try-running-as-root)
```bash
sudo chown root.argusai /dev/gpiomem
sudo chmod g+rw /dev/gpiomem
```

# 工控机
推荐使用ubuntu 16.04.5版本
* 制作[ubuntu启动盘](https://www.jianshu.com/p/187c4ab01add)
* 按DEL键进入BIOS设置上电自启，U盘启动安装
* 改用[国内Ubuntu更新源](https://www.jianshu.com/p/7690bfbcee72):亦可直接选择aliyun
* 改用[国内ROS更新源](http://wiki.ros.org/ROS/Installation/UbuntuMirrors)/[更新方法](https://www.cnblogs.com/letisl/p/11815191.html)：亦可手动添加
```bash
deb http://ros.exbot.net/rospackage/ros/ubuntu/ trusty main
```
* google chrome安装
```bash
sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt-get update -y
sudo apt-get install google-chrome-stable -y
```

* 安装teamviewer并设置成“随系统一同启动"，安装失败的话改为执行：`sudo apt-get install ./teamviewer-xxxx.deb`，
* 安装[linuxbrew](https://docs.brew.sh/Homebrew-on-Linux)
* 安装[RoboWare Studio开发工具](http://ww1.roboware.me/#/Download)
* 开机启动设置: 搜索中输入`session`后选择启动应用程序
```bash
gnome-terminal -x /home/argusai/argusai/share/argusai/scripts/startup.sh
```
* 分屏终端工具: terminator
* [ubuntu系统汉化](https://blog.csdn.net/qq_19339041/article/details/80058575)
* 安装shell: [zsh/oh-my-zsh](https://www.cnblogs.com/EasonJim/p/7863099.html)
* 安装终端增强[oh-my-zsh](https://ohmyz.sh/)：
```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

* 运行时文件存在但还是出现`No such file or directory`错误的[解决方法](https://www.cnblogs.com/rohens-hbg/p/4763378.html)
* 无法获得锁 /var/lib/dpkg/lock -open （11：资源暂时不可用）的[解决方法](https://blog.51cto.com/dreamylights/1287073)
```bash
sudo rm  /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```
* 编译源码： `INSTALL_PATH=/home/jygk/argusai OS=ABC ./src/argusai/scripts/misc/build.sh`

# 参考
1. [GPG error](https://blog.csdn.net/zhuiqiuzhuoyue583/article/details/90597499)
2. [Error in `appstreamcli'](https://blog.csdn.net/AAA123524457/article/details/96484576)
3. [cannot download default sources list from](https://blog.csdn.net/u013468614/article/details/102917569)
4. [树莓派打开GPIO的TTL串口](https://blog.csdn.net/u013451404/article/details/80385496)
5. [通过USB转TTL来登陆树莓派](https://jingyan.baidu.com/article/14bd256e7afb78bb6c261246.html)
6. [树莓派raspberry pi 安装远程工具teamviewer](https://blog.csdn.net/shaopengf/article/details/75072907)
7. [各版本服务器版](https://wiki.ubuntu.com/ARM/RaspberryPi)
8. [RaspberryPi 3安装ubuntu server](https://blog.csdn.net/kevin3683/article/details/52202425)
9. [Raspberry Pi 3B+ Ubuntu Server 18.04.2 Installation Guide](https://jamesachambers.com/raspberry-pi-ubuntu-server-18-04-2-installation-guide/)
10.[ROS程序开机自启动](http://blog.sina.com.cn/s/blog_602f87700102wqy9.html)
11.[Ubuntu Server 18.04安装并配置wifi](https://www.cnblogs.com/free-ys/p/10162388.html)
12.[ROS主从机设置](https://www.jianshu.com/p/5ff453cb994e)
13,[免密登陆](https://www.cnblogs.com/konrad/p/6901273.html)
14.[各主板定时开机设置](https://wenku.baidu.com/view/66c6604bc850ad02de80416d.html)
15.[让树莓派永不失联](https://blog.csdn.net/kxwinxp/article/details/78370980): Screen
