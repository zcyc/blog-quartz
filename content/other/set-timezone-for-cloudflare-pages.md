---
title: "Set Timezone for Cloudflare Pages"
---


hugo、astro 等静态站点工具通过 Cloudflare Pages 编译时使用 0 时区 format 时间，导致显示时间比实际时间早 8 个小时，需要手动设置环境变量 TZ。

![Untitled](/static/cloudflare-1.jpg)

[点我查看常用时区](https://help.aliyun.com/zh/maxcompute/user-guide/time-zones)
