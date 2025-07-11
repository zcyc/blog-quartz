---
title: "macOS L2TP/IPSec No Response"
---


```shell
#由于国内运营商对 PPTP 封锁，所以自己本地组网必须使用 L2TP，和各大运营商确认过，L2TP 是不封锁的。
sudo vim /etc/ppp/options

#然后在文件中输入
plugin L2TP.ppp

```
