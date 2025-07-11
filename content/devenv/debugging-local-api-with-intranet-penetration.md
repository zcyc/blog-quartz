---
title: "Debugging Local APIs with Intranet Penetration"
---


最近调试 GitHub Webhook 需要公网地址，所以试用了各种内网穿透服务，以下是体验比较好的。

# [smee](https://smee.io)

## 获取隧道地址
[点此链接获取隧道地址](https://smee.io/new)

## 启动隧道
注意：将 https://smee.io/xxx 改为获取到的隧道地址
```bash
npx smee-client --url https://smee.io/xxx --path /integration/webhook/github --port 8080
```

# [Hookdeck CLI](https://github.com/hookdeck/hookdeck-cli)

## 登录
```bash
npx hookdeck-cli login
```

## 启动隧道
```bash
npx hookdeck-cli listen 8080 github --path /integration/webhook/github
```

# [ngrok](https://dashboard.ngrok.com/)

## 安装 cli
```bash
brew install ngrok/ngrok/ngrok
```

## 写入 token
登陆后执行页面显示的命令，示例：
```bash
ngrok config add-authtoken XXX
```

## 启动隧道
```bash
ngrok http http://localhost:8080
```

# [dev tunnel](https://learn.microsoft.com/en-us/azure/developer/dev-tunnels/get-started?tabs=macos#install)

## 安装 cli
使用 brew 安装启动时会提示版本过旧
```bash
curl -sL https://aka.ms/DevTunnelCliInstall | bash
```

## 登录
-g 是 GitHub 登录，否则是微软登录
```bash
devtunnel user login -g
```

## 启动隧道
```bash
devtunnel host -p 8080
```

# [Localtunnel](https://theboroer.github.io/localtunnel-www/)

## 启动隧道
```bash
npx localtunnel --port 8000
```

## 获取隧道密码
以下命令二选一
```bash
curl https://loca.lt/mytunnelpassword
wget -q -O - https://loca.lt/mytunnelpassword
```

# [Zero Trust](https://one.dash.cloudflare.com/)

## 启动隧道
1. 进入 `Networks` - `Tunnels`
2. 点击 Create a tunnel
2. 选择 Cloudflared
3. 填写 Tunnel name
4. 点击 Save tunnel

## 安装并运行 connector
1. 执行页面显示的命令，示例：
```bash
brew install cloudflared && 
sudo cloudflared service install XXX
```
2. 安装完成后点击 Next

## 配置路由
1. 选择 `Public Hostnames`
2. 填写 Subdomain
3. 选择 Domain
4. 填写 Path
5. 选择 Type
6. 填写 URL，不要写 http:// 和 https://
3. 点击 Save tunnel
4. 测试路由能否正常访问
