---
title: "Nginx: Allow iframe"
---


# 配置允许同源 iframe 和指定域名 iframe

```
add_header Content-Security-Policy "frame-ancestors 'self' *.test.com *.test.cn";
```

# 配置允许跨站 Set-Cookie
[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie)