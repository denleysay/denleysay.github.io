---
layout: post
title: MAC系统安装
date: 2022-08-12 09:24
categories:
  - 操作系统
---

针对本人MAC系统下的安装
<!-- More -->

个人机器要在mac中安装windows双系统, 目前只支持windows 10
* bootcamp菜单栏中下载windows支持软件，以便安装window下的所有驱动: `有时安装后会显示有此支持软件的安装盘`
* 原来已经安装了双系统：
  * 进入"磁盘工具”，选中”windows"所在的盘，执行“抹掉"
  * 运行”启动转换助理“bootcamp，按提示操作

# 准备
* 确定系统已更新：“About this Mac"
    > 1) 确定操作系统已更新: Monterey Verssion 12.6.2;
    > 2) 确定软件已更新：”Software Update...“；
* 对自带的git进行设置: http.postBuffer/https.postBuffer, 否则下一步的brew安装会出现错误，可参考git设置
* [brew安装](https://brew.sh/index_zh-cn)
```bash
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

  # 更换源
  cd "$(brew --repo)"
  git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
  brew update
```
# 安装
## 自动安装
```bash 
  # 安装最新版git：2.39.0，重启终端生效
  brew install git --with-gettext
  # Monterey已缺省安装
  brew install zsh
  
  # Console，其中的docker是客户端
  brew install boost autojump docker docker-compose docker-completion docker-compose-completion wget you-get
  brew install --HEAD https://gist.githubusercontent.com/Kronuz/96ac10fbd8472eb1e7566d740c4034f8/raw/gtest.rb
  brew link docker docker-compose --overwrite
  brew install enca  # 批量文件格式转换工具，使用方法：enca -L zh_CN -x UTF-8 *

  # GUI
  # cmake aliwangwang thunder mounty qq wechat wechatwebdevtools shuttle textmate v2rayU
  # the-unarchiver obs xmind-zen postgres pgadmin4 sourcetree homebrew/cask-versions/docker-edge virtualbox vagrant vagrant-manager chrome-remote-desktop-host
  brew install --cask yinxiangbiji google-chrome iterm2 github teamviewer vlc baidunetdisk tencent-meeting
  brew cleanup
```

## 下载安装
* [alfred](https://app.yinxiang.com/shard/s26/nl/5132076/dad80f02-27a5-447e-a0d2-e7cee1978785/) 
* unclutter: 剪切板、处理中文件和临时笔记三合一管理
* 以上可以通过brew cask安装，但是要付费使用
* [sogou拼音](https://shurufa.sogou.com/): 可以brew cask sogouinput安装，但在Monterey上无法使用
* [sogou五笔](https://pinyin.sogou.com/mac/)
* [oh-my-zsh](https://ohmyz.sh/)
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
* [docker服务端](https://docs.docker.com/desktop/install/mac-install/): 设置国内镜像来加速http://f1361db2.m.daocloud.io
* [OpenInTerminal](https://github.com/Ji4n1ng/OpenInTerminal/blob/master/README-zh.md)


# 设置
## 系统设置
* 执行app_config下的脚本
* 下载软件时不再安全提示：通过执行`sudo spctl --master-disable`显示”Anywhere“，再选中此项
![Anywhere](/assets/images/mac/Anywhere.png)
* 鼠标滚动不选中自然
![ScrollMouse](/assets/images/mac/ScrollMouse.png)
* FN键起作用
![FnKey](/assets/images/mac/FnKey.png)
* 显示文件后缀
![ShowFileExt](/assets/images/mac/ShowFileExt.png)

## Alfred设置
快捷键冲突：先取消spotlight设置

## GoogleChrome设置
在插件应用商店中下载
* Evernote Web Clipper
* Clearly Reader
* Ghelper: 登陆才生效
* GoFullPage
* Language Reactor
* Google Voice
* Vimium

## github设置
参考系统安装的通用部分

## zsh设置
* [安装自动提示](https://www.jianshu.com/p/0d265d9f914b)
```bash
# if use brew install
brew install zsh-autosuggestions
echo "source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

* 安装语法高亮
```bash
  # if use brew install
  brew install zsh-syntax-highlighting
  echo "source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc

  # use git install
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
  echo "source \$ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

## Textmate设置
* 下载文件后重启
```bash
cd ~/Library/Application\ Support/TextMate/Managed/Bundles/
# 高亮显示空格
git clone https://github.com/mads379/Whitespace.tmbundle.git

# 自动去掉行尾空格
git clone https://github.com/bomberstudios/Strip-Whitespace-On-Save.tmbundle.git
# 自动生成尾行
git clone https://github.com/hajder/Ensure-New-Line-at-the-EOF.tmbundle.git
```
* 设置textmate的tab: ~/.tm_properties
```bash
softWrap = true
tabSize = 2
softTabs = true
```

## 其它设置
* sogouinput需要按提示进入目录安装
* google需要登陆才能同步收藏夹
* [docker加速配置](https://www.daocloud.io/mirror)

# 问题
* [苹果官网问题咨询](https://getsupport.apple.com/?caller=home&PRKEYS=)
* 出现[此文所述错误](https://www.jianshu.com/p/7d055bebab46)，使用brew install brew-cask-completion代替
* 安装时如有卡住(更新需要翻墙)，按CTRL + C,不影响安装

# 参考
部分可以通过brew安装
* [MicrosoftRemoteDesktop](https://blog.csdn.net/ytangdigl/article/details/78941783): 远程桌面登录到windows
* [shuttle](https://github.com/fitztrev/shuttle/releases): 简易SSH快捷菜单
* Mounty
  1. 使用Mounty出现如下错误的[解决办法](http://yangl.net/2017/05/15/mounty_error/)：在windows下执行chkdsk /f
`The volume UNTITLEDis not re-mountable in read/write mode .Probably it was not clean unmounted before.`
  2. 外接硬盘文件为灰色时执行：xattr -d com.apple.FinderInfo *
* [DictUnifier](https://www.jianshu.com/p/c57be986589b): 词典转换工具
推荐词典: 朗道英汉字典5.0
* [USB转串口驱动程序](https://www.prolific.com.tw/US/index.aspx), 注意Mac OS High Sierra 10.13上安装时[Blocked kernel extension的问题](https://developer.apple.com/library/archive/technotes/tn2459/_index.html)
* MAC下网口转USB： [确定网卡芯片型号](https://sspai.com/post/41120?_t=1576887829), 然后通过型号找到绿联mac驱动[Type-C千兆网卡AX88179芯片下载 ](https://www.lulian.cn/download/6-cn.html)
* 比Eclipse容易上手的Java IDE **IntelliJ-IDEA**：[下载](https://www.jetbrains.com/idea/download/)/[激活](http://idea.javatiku.cn/)
* 免费的VPN工具：**PigchaClient**

**粗体表示此软件在百度网盘OS/mac上有**

* [苹果Mcbook Air A1466更换硬盘教程](https://mp.weixin.qq.com/s/kd1NHUfxfrFFip7a_S-fSQ)

* brew使用指南
```bash
 brew info         # 显示软件信息（包括安装参数说明)
 brew update       # 更新brew
 brew outdated     # 检查哪些软件需要更新
 brew upgrade      # 更新所有软件
 brew cleanup      # 清理不需要的版本极其安装包缓存
 brew install caskroom/cask/brew-cask  # 出现错误参考1
 brew cask search  # 显示所有可安装软件
```
