---
layout: post
title: ubuntu系统安装
date: 2022-08-11 15:35
categories:
  - 操作系统
---

针对本人Ubuntu系统下的安装
<!-- More -->

# docker环境安装
* 安装[docker](https://docs.docker.com/install/)和[docker-compose](https://docs.docker.com/compose/install/)
```bash
# root用户下
apt-get update -y
adduser ${REMOTE_USERNAME}
echo '${REMOTE_USERNAME} ALL=(ALL:ALL) ALL' | sudo EDITOR='tee -a' visudo
usermod -aG docker ${REMOTE_USERNAME}

# ${REMOTE_USERNAME}用户下安装docker
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

# 安装docker-compose 参考自
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

* 删除满足条件的文件: `find . -name '*_test*' -exec rm {} \;`

# PostgreSQL
* 配置文件：sudo vi /etc/postgresql/9.3/main/pg_hba.conf
* 服务重启：sudo service postgresql restart

# 下载地址
[官方](http://releases.ubuntu.com/)

# 查看版本号
1. cat /etc/issue
2. cat /etc/lsb-release
3. uname -a

# 设置网络代理
1. 设置网络
	* 全局设置: [详见配置文件设置](http://www.2cto.com/os/201204/127131.html)
	* 当前用户设置: `ifconfig eth0 210.34.6.89 netmask 255.255.255.128 broadcast 210.34.6.127`
2. 设置代理

# 设置环境变量
1. [全局设置](http://jingyan.baidu.com/article/db55b609a3f6274ba30a2fb8.html):`gedit /etc/environment`---->`source /etc/environment`
2. 当前用户设置: `gedit ~/.bashrc`---->`source ~/.bashrc`

# 重置root用户密码
1. 任意用户登陆进入命令行;
2. 执行`sudo passwd`即可重置密码

# 修改源并升级更新
1. 备份源: `sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup`
2. 修改源: `sudo gedit /etc/apt/sources.list`,可参考[Ubuntu 10.04 (Lucid) 更新源](http://www.cnblogs.com/dolphin0520/archive/2013/03/15/2960907.html)/[Ubuntu各版本下的更新源](http://pan.baidu.com/share/link?shareid=1362990598&uk=4278685087)
3. 保存编辑好的文件，执行以下命令更新: `sudo apt-get update`

# 命令
* [shyaml](https://linuxtoy.org/archives/shyaml.html):shell下的yaml解析器
* [jq](https://linuxtoy.org/archives/jq.html): shell下的json解析器
* tree命令
```bash
find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
```
* pause命令
```bash
function pause(){
  read -n 1 -p "$*" INP
  if [ '$INP' != '' ] ; then
    echo -ne '\b \n'
  fi
}
```

# 免密登陆
参考自[免密登陆](https://www.cnblogs.com/konrad/p/6901273.html)

1. 通过`ifconfig`获取主机B(ubuntu虚拟机)的IP地址${B_IP};
2. 在主机A生产密钥对: `ssh-keygen -t rsa -C denley@justtodo.com`， 会在.ssh目录下产生密钥文件
3. 将此密钥传输到主机B，`ssh-copy-id -i ~/.ssh/id_rsa.pub ${B_USER}@${B_IP}`，这样主机A就可以免密访问主机B了

# 安装
## 系统
1. 通过老毛桃制作启动U盘，将Ubuntu官网下载的ubuntu 16.04 amd64版本COPY到U盘的LMT目录下；
2. 开机后按DEL进入BIOS设置：Advanced/Miscellaneous Configuration/OS Selection选择Windows 8.X，再将系统设置为U盘启动按F4选YES保存；
3. 用户名/密码设置为: argusai/1..8，选择免密登陆

## 软件
* 为加快速度可选择设置更新源：aliyun或tsing
1. 设置更新源：进入Software&Updates设为mirrors.aliyun.com
```bash
sudo apt-get update -y
sudo apt-get install cutecom watchdog git -y
sudo cutecom
```
2. 安装Teamviewer: 使用U盘中的指定版本，安装完后设置密码1..8,并设置成“随系统一同启动"te保存好ID

# 看门狗
## 步骤
```bash
# 安装
sudo apt-get install watchdog -y

# 打开
sudo ln -s  /lib/systemd/system/watchdog.service /etc/systemd/system/multi-user.target.wants/watchdog.service
sudo systemctl enable watchdog.service
sudo systemctl start watchdog.service

# 修改参数
sudo vim /etc/watchdog.conf
sudo systemctl restart watchdog.service

# 查看状态
sudo systemctl status watchdog.service
```

# 参考
1. 梦想家[Watchdog](https://datahunter.org/watchdog)
2. [如何设置看门狗](https://docs.khadas.com/zh-cn/vim3/HowToSetupWatchdog.html)
3. [一个软件实现的Linux看门狗—soft_wdt](https://docs.khadas.com/zh-cn/vim3/HowToSetupWatchdog.html)
4. [Linux 软件看门狗 watchdog 喂狗](https://blog.csdn.net/u013932687/article/details/73274178)
5. [Ubuntu下看门狗程序](https://blog.csdn.net/qq_35571554/article/details/82763977)
