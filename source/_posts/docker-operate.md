---
title: docker-operate
date: 2020-01-04 14:53:27
tags: kubernates
---


## 停止所有的container

```
docker stop $(docker ps -q)

```


## 清除 所有的docker container

```
docker rm $(docker ps -a -q)

```

##  清除 所有的磁盘挂载

```
docker volume prune
```