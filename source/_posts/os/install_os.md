---
layout: post
title: 通用系统安装
date: 2022-08-25 22:45
categories:
  - 操作系统
---

各系统安装的通用部分
<!-- More -->

* `安装前一定要备份`
* 安装[VIM](https://www.vim.org/download.php#pc)，并配置好;
* 安装mtux，并配置好；
* 安装[git](https://git-scm.com/download/win)/[git GUI](https://tortoisegit.org/download/)及汉化包，并配置好git;
* 重要软件的配置通过git管理
* [git汉化](https://blog.justtodo.com/tool/git_setup/): 插件
* 虚拟机与宿主机通过共享文件夹互访
* [破解app下载](https://filecr.com/)：Mac/Window/Android

# git设置
```bash
git config --global user.name "Denley Hsiao"
git config --global user.email denley@justtodo.com
git config --global core.editor vim
git config --global http.postBuffer 524288000
git config --global https.postBuffer 524288000
```

# github设置
对仓库`git@github.com:xxx/yyy`写操作时无需密码验证, 可参考[windows下github的ssh配置](https://www.jianshu.com/p/9317a927e844)
