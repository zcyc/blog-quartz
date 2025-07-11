---
title: "Flutter 遇到的问题"
---


# 进程锁

当你遇到 Waiting for another flutter command to release the startup lock

```shell
# Mac or Linux
killall -9 dart

# Windows
taskkill /F /IM dart.exe
```
