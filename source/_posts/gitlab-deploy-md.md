---
title: gitlab-deploy.md
date: 2020-01-03 22:49:16
tags: kubernates
---

<img src="https://lin19999.oss-cn-beijing.aliyuncs.com/gitlab.svg" width = "400" height = "400" alt="图片名称" align=left />


<img src="https://lin19999.oss-cn-beijing.aliyuncs.com/docker.png" width = "400" height = "400" alt="图片名称" align=right />


## gitlab 部署

```
sudo docker run --detach \
  --hostname 47.110.136.181 \
  --publish 443:443 --publish 80:80 --publish 23:22 \
  --name gitlab \
  --restart always \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest

```

## 部署太慢可以先拉去镜像

dockerhub.azk8s.cn/library/gitlab/gitlab-ce


