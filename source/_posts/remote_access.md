---
title:  "远程访问"
date:   2018-06-07 22:58:00
categories: 工具
---

对远程访问的一点总结

<!-- More -->

# 访问远程主机
以下修改参数都是针对"/etc/ssh/sshd_config"文件，执行"service sshd restart"后修改才生效
* 允许远程访问，修改如下参数
```
PermitRootLogin yes
PasswordAuthentication yes
```

* 若想无需身份验证过程，通过[ssh-copy-id](https://gist.github.com/denleyhsiao/411ac2829bfbe34f24a41088387f6dbe)上传本地密钥文件(~/.ssh/id_rsa.pub)
```
ssh-copy-id -i ~/.ssh/id_rsa.pub <user_name>@<remote_ip>
```

* 为安全考虑，可修改端口号："port 822"
* 访问远程
```
# 远程登陆
ssh -p 822 <user_name>@<remote_ip>
# 远程传输
scp -P 822 <user_name>@<remote_ip>:<remote_file> <local_path>
```

# 访问远程受限服务
## 通过本地访问
原理：将本地的密钥文件内容注册到远程受限服务中
1. 如果密钥文件不存在，先生成: "ssh-keygen -t rsa -C denley@justtodo.com"，一路回车即可，如设密码，在初次连接时，则需要输入此密码
3. 将密钥文件中的内容注册到受限服务中，以下为各服务注册位置
  - gitlab: "设置 - SSH 密钥区域"
  - github: ”Settings - SSH and GPG keys”
4. 测试
```
# 其中<remote_domain>为受限服务相应的访问域名
ssh -T git@<remote_domain>
```

## 通过远程应用访问
原理：远程应用通过持有远程受限服务提供的TOKEN进行访问

以下操作参考自[阮一峰的Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)和[使用 Travis CI 自动更新 GitHub Pages](https://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)
1. 远程受限服务生成TOKEN，以下为服务生成TOKEN位置
  - github: “Settings - Developer settings - Personal access tokens”
2. 为安全起见, 远程应用将不直接使用此TOKEN值
  - travis-ci:

  ```
  # 方式1： 通过“More Options - Settings”将此值直接保存为环境变量<GITHUB_TOKEN>

  # 方式2： 鉴于”方式1“也存在第三方应用泄漏此TOKEN的风险，只公开环境变量加密后的值，即这里的输出内容<SECURE>
  gem install travis
  travis encrypt GITHUB_TOKEN=<TOKEN>
  ```
3. 远程应用使用此环境变量
以travis-ci自动更新github pages为例，在".travis.yml"中使用此环境变量

```
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN
  local-dir: public
  repo: denleyhsiao/denleyhsiao.github.io
  target-branch: master
  fqdn: blog.justtodo.com
  project-name: denleyhsiao.github.io
  committer-from-gh: true
  keep-history: false
  on:
    branch: blog

# 方式2新加的
env:
  global:
    - secure: <SECURE>
```

# 工具
* [mac连接到window](https://blog.csdn.net/ytangdigl/article/details/78941783)，好象中国区[官网](https://github.com/fitztrev/shuttle/releases)不能下载
* [shuttle](https://github.com/fitztrev/shuttle/releases): 简易的SSH快捷菜单