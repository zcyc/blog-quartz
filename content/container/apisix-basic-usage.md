---
title: "Basic Usage of Apisix"
---


对于入门来说，使用成套组件，建议使用官方的 helm 源，可以减少配置错误和兼容性问题，比如：apisix 与 etcd 版本不兼容。

apisix 包括 apisix gateway、apisix ingress controller、apisix dashboard。由于我的环境上已经安装了 nginx ingress controller，所以不安装 apisix ingress controller 了，只安装了apisix gateway与dashboard。

```shell
helm repo add apisix https://charts.apiseven.com
helm repo update
helm install apisix apisix/apisix --create-namespace  --namespace apisix
helm install apisix-dashboard apisix/apisix-dashboard --namespace apisix
```

apisix 会安装在 apisix namespace 下，默认会起一个三节点的 etcd。

![Untitled](/static/apisix-1.png)

默认的dashboard的service是nodeport类型，这里改成cluster ip，用ingress映射对外访问，在自己本机host设置一个域名。

![Untitled](/static/apisix-2.png)

![Untitled](/static/apisix-3.png)

```text
172.18.1.10 apisix-dashboard.com
```

输入域名，进来dashboard的界面。

![Untitled](/static/apisix-4.png)

## 创建内部k8s服务发现的上游

假如我要在集群外通过某个 URI 访问某个 service 的接口，如:
![Untitled](/static/apisix-5.png)

在 ingress 添加一条规则，test.api.com 的所有流量转发到 apisix gateway。
![Untitled](/static/apisix-6.png)

接下来在 apisix dashboard 中配置一个上游(upstream)。
![Untitled](/static/apisix-7.png)

这是一个简单的示例，将一个在 hcare namespace 的 service hcare-user-svc 作为 upstream。
![Untitled](/static/apisix-8.png)

这样的主机名其实是使用了 k8s 内部的服务发现，apisix 提供了另外一种更优雅使用 k8s 内部服务发现的方式：
![Untitled](/static/apisix-9.png)

关于配置 apisix 中的 k8s 服务发现的文档：
[Kubernetes | Apache APISIX® -- Cloud-Native API Gateway](https://apisix.apache.org/zh/docs/apisix/discovery/kubernetes/)

其实 apisix 就是调用了 k8s 内的 api-server，获取某个 service 的 endpoint，直接通过 endpoint 地址访问，这样更符合服务发现的方式。

但是这样访问 k8s 内的 api-server，需要一个比较高权限的账号，文档内也提到了这个：
![Untitled](/static/apisix-10.png)

然后获取这个 ServiceAccount 的权限：
![Untitled](/static/apisix-11.png)

然后在 apisix 的 configmap 里配置这个 token:
![Untitled](/static/apisix-12.png)

这样子我们就可以直接用 apisix 给我们提供的 k8s 内部服务发现了。
![Untitled](/static/apisix-13.png)

服务名称填写的规范在这里：
![Untitled](/static/apisix-14.png)

## 创建一个路由

![Untitled](/static/apisix-15.png)
![Untitled](/static/apisix-16.png)

这样子就可以通过这个地址的路由访问到服务了。
