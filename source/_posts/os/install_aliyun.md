---
layout: post
title: 阿里云系统安装
date: 2022-08-11 15:40
categories:
  - 操作系统
---

针对mac与阿里云上的ubuntu远程操作
<!-- More -->

1. 阿里云服务器重置: 已安装python3;
2. 系统初始化
    * 执行[ubuntu系统安装](/coder/install_ubuntu)下的`docker环境安装`小节
    * [git环境安装](https://app.yinxiang.com/shard/s26/nl/5132076/76807934-164f-4bdd-8280-22c673fc2089/)

3. ssh远程登陆到阿里云，可参考附录2;
    ```bash
    ssh-keygen -t rsa -C ${REMOTE_USERNAME}@justtodo.com
    ssh-copy-id -i ~/.ssh/id_rsa.pub ${REMOTE_USERNAME}@${REMOTE_IP}
    ssh ${REMOTE_USERNAME}@${REMOTE_IP}
    ```

4. 迁出源码，执行
    ```bash
    git clone https://github.com/argusai/argusai_web.git
    cd argusai_web
    sudo chmod a+x main.py
    pip3 install bottle
    python3 main.py
    ```

5. 外网访问`${REMOTE_IP}:8080`时没有反应，可参考[阿里云服务器公网ip无法访问解决办法](https://developer.aliyun.com/article/87135)或[阿里云公网访问不了，域名不加端口号访问](https://blog.csdn.net/a419419/article/details/85047110)

6. 后台运行
    * 执行以下步骤后，按CTRL+A D就后台运行；
    ```bash
    sudo apt-get install screen
    screen -S vpn
    cd argusai_web
    python3 main.py
    ```
    * 执行`screen -r`就返回前台运行

# 附录
1. 因mac下没有文件ssh-copy-id，在`/usr/bin/`目录下新建此文件

    ```bash
    #!/bin/sh

    # Shell script to install your identity.pub on a remote machine
    # Takes the remote machine name as an argument.
    # Obviously, the remote machine must accept password authentication,
    # or one of the other keys in your ssh-agent, for this to work.


    ID_FILE="${HOME}/.ssh/identity.pub"


    if [ "-i" = "$1" ]; then
      shift
      # check if we have 2 parameters left, if so the first is the new ID file
      if [ -n "$2" ]; then
        if expr "$1" : ".*\.pub" ; then
          ID_FILE="$1"
        else
          ID_FILE="$1.pub"
        fi
        shift         # and this should leave $1 as the target name
      fi
    else
      if [ x$SSH_AUTH_SOCK != x ] ; then
        GET_ID="$GET_ID ssh-add -L"
      fi
    fi


    if [ -z "`eval $GET_ID`" ] && [ -r "${ID_FILE}" ] ; then
      GET_ID="cat ${ID_FILE}"
    fi


    if [ -z "`eval $GET_ID`" ]; then
      echo "$0: ERROR: No identities found" >&2
      exit 1
    fi


    if [ "$#" -lt 1 ] || [ "$1" = "-h" ] || [ "$1" = "--help" ]; then
      echo "Usage: $0 [-i [identity_file]] [user@]machine" >&2
      exit 1
    fi


    { eval "$GET_ID" ; } | ssh $1 "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys" || exit 1


    cat <<EOF
    Now try logging into the machine, with "ssh '$1'", and check in:


      .ssh/authorized_keys


    to make sure we haven't added extra keys that you weren't expecting.


    EOF
    ```
