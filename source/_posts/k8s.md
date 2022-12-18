---
title:  "k8s总结"
date:   2018-05-27 9:58:00
categories:
- 工具
- Docker
---

k8s是基于docker下的DevOps工具

<!-- More -->

# 学习方法

## docker
首先从其基础docker学习开始
* 从简单开始，边ß看边动手
* 难点在网络、端口的映射，容器卷的使用

## kubernetes
* 坚持啃官网，特别留意是否有没有[中文官网](https://k8smeetup.github.io/)
* 先简要了解:  [写给孩子看的Kubernetes 动画指南](https://www.bilibili.com/video/av10087636/)， [docker用户过渡到kubectl命令行指南](https://jimmysong.io/kubernetes-handbook/guide/docker-cli-to-kubectl.html)
* [每天5分钟玩转 Kubernetes](http://www.cnblogs.com/CloudMan6/tag/Kubernetes/)，微信公众号同步：CloudMan
* 单机版minikube开始，安装前要翻墙，包括终端下的，也是边看边动手
* 集群学习前先不要自己搭建，太难搭成功，先试验 [在线玩转k8s](https://labs.play-with-k8s.com/)
* 上一步骤的缺点是没有外网IP，此部分可以在GKE上试验
* 啃精典：Kubernetes权威指南：从Docker到Kubernetes实践全接触（第2版)/ [IBM在线微课堂: Kubernetes系列](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/W30b0c771924e_49d2_b3b7_88a2a2bc2e43/page/Kubernetes%E7%B3%BB%E5%88%97)
* 自己实战，先搭建集群的实验环境，再从简单的项目迁移开始

## 他人建议
* 多读、多看、多听。
Retriever Communications 首席技术官 Nic Grange 推荐 Kelsey Hightower 提供的学习资料；此外，Hightower、Beda、Brendan Burns（Kubernetes 创始人之一）联合创作的《Kubernetes：Upand Running》也值得一看；另外，Red Hat 的《Kubernetes Guide》 ，包含了 Kubenetes 词汇表以及 Kubernetes 如何融入企业 IT 架构，推荐阅读。

* 有计划地理解概念性知识。
Pepperdata 高级架构师 Kimoon Kim 表示：“Kubernetes 有许多不同的结构，新用户很容易迷路。因此，要先从 Kubernetes Pod 开始，再结合 Kubernetes 集群进行学习。CYBRIC 首席技术官兼联合创始人 Mike Kail 表示：“开始使用像 Kubernetes 这样的新兴技术的最好方法是构建框架，然后以合乎逻辑的方式进行下去，而不是一口气吃成胖子。 Kail 将其分解为：Kubernetes 构建 Blocks（如 Pod），服务（如 ClusterIP），网络，数据卷管理以及服务发现/负载均衡。

* 用 Kubernetes “试驾”。
一旦你对 Kubernetes 核心概念有了较好的理解，那么，你就可以从一个简单的应用程序部署开始了解集群。Nic Grange 认为：“让 Kubernetes 自己启动和运行是令用户和团队感到最具挑战性的部分。通过类似 Minikube 这样的开源项目，首先学习如何托管 Kubernetes 部署和管理应用程序，然后回过头去学习如何构建和管理自己的集群受益会更大。

* 计划使用 Kubernetes 非关键负载。
一旦你接受了这些概念，你就可以开始计划如何使用（Kubernetes）非关键的工作负载了。因为在部署更重要的工作负载时，这个过程能帮助你学习、犯错、获得自信。一个成功的学习策略需要反复试验：通过预判早期的失误，即可限制它们对底层应用程序的影响。这点在“测试和学习模式”发展到“管理生产中的容器”模式的过程中，以及当您用编排平台（如 OpenShift）以可扩展的方式管理容器时，显得尤为重要。

* 从概念学习向深入学习递进，确保学习进度呈现曲线而非直线。
部分实践学习应该在日常使用中边练边学，然后逐步掌握概念，深入了解该平台。一旦你建立了初步的应用程序，并逐渐适应，就可以开始深入研究 Kubernetes 的所有“魔法”了。 例如它是如何导流到服务的？当 Pod 移动时，它是如何使用并管理 PV 的？以及为了保护 Pods，哪些需求选项是可用的？这将对您有效地使用托管的 Kubernetes，甚至建立自己的私有 Kubernetes 集群非常有利。

# minikube安装
Mac air下
1. 安装virtualbox
2. kubectl 安装: `curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/`
3. minikube安装：`curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v0.24.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/`
4. 启动：`minikube start --registry-mirror=https://registry.docker-cn.com`
5. 运行: `minikube dashboard`

# 环境搭建
master和node节点都需要的操作，详见: [部署 k8s Cluster](http://www.cnblogs.com/CloudMan6/p/8269620.html)

```bash
apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
```

## master
```bash
# 一定要记下输出内容，替换后续的部分操作
kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr=10.244.0.0/16

# 非root用户下
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

## node
`kubeadm join 10.128.0.2:6443 --token ldoxl7.bsfdzfyfunvd0v4q --discovery-token-ca-cert-hash sha256:b846e9c7acf4d83d18850623e0186f8ee97d4f0c483af19410a1cea427872228`

# 参考
* [Kubernetes 学习笔记之 MiniKube 安装](https://ehlxr.me/2018/01/12/kubernetes-minikube-installation/)
* [mac安装kubernetes并运行echoserver](https://www.jianshu.com/p/a42eeb66a19c)
* [Standardized CI System Blog](https://github.com/IBM/cloud-journeys/issues/1)
* [使用Minikube搭建Nginx集群](https://github.com/johnnian/Blog/issues/32)
* [Ubuntu 16.04下kubeadm安装Kubernetes](http://blog.csdn.net/yan234280533/article/details/75136630)
* [Kubernetes1.9 在ubuntu16.4 安装](http://blog.csdn.net/russle/article/details/78841937)
* [利用Minikube来部署一个nodejs应用](http://blog.gezhiqiang.com/2017/08/04/minikube/)
* [如何开启kubernetes之旅](http://dockone.io/article/2523)
* 从[GitLab-sample](https://github.com/IBM/Kubernetes-container-service-GitLab-sample)中学习如何让gitlab上k8s
* [k8s的存储管理](http://v.youku.com/v_show/id_XMzE4NzU1OTA3Ng==.html): 看完成视频，再重看实践示例
* [gitlab在minikube中的实践](https://github.com/IBM/Kubernetes-container-service-GitLab-sample/blob/master/README-cn.md)