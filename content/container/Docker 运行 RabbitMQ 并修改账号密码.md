---
title: "Docker 运行 RabbitMQ 并修改账号密码"
---


# 运行

```shell
# 前台启动一个用完就删的 RabiitMQ
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
# 后台启动一个用完不删的 RabiitMQ
docker run -it -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

# 修改帐号密码

```shell
# 比如重置 rabbitmq 的帐号密码，如果你是按照官方的教程安装的，那么容器名称就是 rabbitmq，那么执行下边的命令就可以

docker exec rabbitmq rabbitmqctl add_user newadmin newpassword

docker exec rabbitmq rabbitmqctl set_user_tags newadmin administrator

docker exec rabbitmq rabbitmqctl set_permissions -p / newadmin "." "." ".*"
```
