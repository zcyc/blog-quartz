---
title: "Integrating Apisix with JWT and Casbin"
---


现在我有两个用户:

| user_name | role name    |
| ---
title: "Integrating Apisix with JWT and Casbin"
---
---
title: "Integrating Apisix with JWT and Casbin"
---
---
title: "Integrating Apisix with JWT and Casbin"
---
---
title: "Integrating Apisix with JWT and Casbin"
---
负值新 header
ctx.headers["Token-Customer"] = consumer.consumer_name
```

将修改完的 jwt-auth 插件通过 configmap 挂载到 apisix 中，depolyment 如下：
![Untitled](/static/apisix-jwt-casbin-4.png)
![Untitled](/static/apisix-jwt-casbin-5.png)

configmap:
![Untitled](/static/apisix-jwt-casbin-6.png)

在 apisix 的配置文件里添加如下内容：

```yml
enable_debug: true
enable_dev_mode: true
extra_lua_path: "/usr/local/apisix/my/?.lua"
```

这样自定义的 jwt-auth 就覆盖了原 jwt-auth。

## 构建token

创建一个满足 jwt-auth 要求的 token，至少要有 key 和 exp

![Untitled](/static/apisix-jwt-casbin-8.png)

jwt-auth 在解析 token 的时候，拿到了这个 key 对应的 customer，然后将 customer 放入上下文的 header 中，接下来的 authz-casbin 从 header 拿出用户，通过 Casbin 鉴权，然后放行。

![Untitled](/static/apisix-jwt-casbin-9.png)
