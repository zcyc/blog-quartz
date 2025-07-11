---
title: "Packet Sniffing on macOS and for iOS Devices"
---


# 环境

macOS 11.5.1 
iOS 14.7.1

# charles

## 按照官方教程步骤安装ssl证书

需要安装并且信任

## 然后设置ssl proxy

ip和port都填\*就行了

### 注意

如果只抓手机的话把 mac proxy 关掉，否则太多包影响查看

# wireshark

## 载入调试模块

```bash
sudo launchctl load -w /Library/Apple/System/Library/LaunchDaemons/com.apple.rpmuxd.plist
```

## 连接设备

rvictl -s 00008101-**\*\***\***\*\***

### 注意

首次连接需要重启系统载入调试模块
