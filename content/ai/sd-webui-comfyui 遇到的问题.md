---
title: "sd-webui-comfyui 遇到的问题"
---


# 页面空白、黑屏

[https://github.com/ModelSurge/sd-webui-comfyui/issues/184](https://github.com/ModelSurge/sd-webui-comfyui/issues/184)
通过绘世启动器安装插件会自动使用 gitcode 镜像加速，此插件的镜像代码太旧了，导致配置中的启动端口不生效。更新为 github 最新代码即可。
如果你使用 comfyui，建议直接使用 comfyui 整合包，不要用 sd-webui-comfyui。
