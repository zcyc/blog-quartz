---
title: "Go Reverse Proxy"
---


# 直接代理

```go
func (c *cGateway) ReverseProxy(r *ghttp.Request) {
	serverName := r.GetRouter("REQUEST_SERVER").String()
	serverUri, _ := g.Cfg().Get(r.Context(), fmt.Sprintf("serverUri.%s", serverName))
	target, _ := url.Parse(serverUri.String())
	proxy := &httputil.ReverseProxy{
		Director: func(req *http.Request) {
			req.URL.Scheme = target.Scheme
			req.URL.Host = target.Host
		}
    }
	proxy.ServeHTTP(r.Response.Writer, r.Request)
	return
}
```

# 修改请求路径

```go
func (c *cGateway) ReverseProxy(r *ghttp.Request) {
    serverName := r.GetRouter("REQUEST_SERVER").String()
    path := r.URL.Path
    subPath := path[len(serverName)+1:]
    serverUri, _ := g.Cfg().Get(r.Context(), fmt.Sprintf("serverUri.%s", serverName))
    requestUrl := fmt.Sprintf("%s%s", serverUri, subPath)
    target, _ := url.Parse(requestUrl)
	proxy := &httputil.ReverseProxy{
		Director: func(req *http.Request) {
			req.URL.Scheme = target.Scheme
			req.URL.Host = target.Host
            req.URL.Path = subPath  // 如果请求 path 和代理的 path 不同，需要修改请求 path
		}
    }
    proxy.ServeHTTP(r.Response.Writer, r.Request)
    return
}
```
