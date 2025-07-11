---
title: "Nginx 部署过程中的问题"
---


```shell
# 测试当前配置文件，可以根据错误修改 nginx 配置文件
nginx -t

# 重载配置文件
nginx -s reload

# /etc/nginx/sites-available 中的文件直接复制到 /etc/nginx/sites-enabled 中，软连接会因为权限报错
cp sites-available/*.conf sites-enabled/

# 目录一般在
/etc/nginx/sites-enabled

# 网站目录在
/var/www/

# java程序目录在
/opt/
```
