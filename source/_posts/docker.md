---
title:  "Docker使用"
date:   2017-12-04 9:00:00
categories: 
- 工具
- Docker
---

docker, 新一代“虚拟机”

<!-- More -->

# 安装
* 在ubuntu下安装docker： `curl -s https://get.docker.io/ubuntu/ | sudo sh`
* 安装docker-compose: 

```bash
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/bin/docker -v $(pwd):/app docker-compose -f /app/docker-compose.yml up -d
```

# 注意
在将image push到自己仓库时，记得先进行登陆操作：  

```bash
docker login
docker tag ubuntu denley/ubuntu:ping
docker push denley/ubuntu:ping
```

# 参考
* [一步步简单地操作docker](https://www.2cto.com/kf/201702/593991.html)
* [Docker---从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/)
* [Docker Compose in 12 Minutes](https://www.youtube.com/watch?v=Qw9zlE3t8Ko)