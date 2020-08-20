---
title: mongodb docker 部署-非集群
date: 2020-07-08 14:16:37
summary: MongoDB docker 非集群安装
tags:
  - 非集群
  - mongo
  - docker 部署
categories: 数据库
---

# Docker 部署 MongoDB

### 1. 拉取 docker 镜像

```
docker pull mongo:3.4
```

### 2. 运行

```
docker run -d --name mongodb --volume /root/docker/local/mongodata:/data/db -p 27017:27017 mongo:3.4 --auth
```

### 3. 进入 mongo

```
docker exec -it mongodb mongo
```

### 4. 创建数据库帐号

```
use admin;db.createUser({ user: 'root', pwd: '123', roles: [ { role: "root", db: "admin" } ] });
```

### 5. 安装 mongo-express 可视化工具

```
docker run -d --name mongo-express -p 8081:8081 --link mongodb:mongo --env ME_C
```

