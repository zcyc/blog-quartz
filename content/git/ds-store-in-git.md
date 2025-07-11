---
title: ".DS_Store Files Accidentally Committed to Git"
---


.DS_Store 是 Mac OS 保存文件夹的自定义属性的隐藏文件，如文件的图标位置或背景色，相当于 Windows 的 desktop.ini。

# 禁止.DS_store 生成

打开 “终端” ，复制黏贴下面的命令，回车执行，重启 Mac 即可生效。

```bash
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

# 恢复.DS_store 生成：

```bash
defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```

# 最好的解决方案

实测以上方案不生效，所以最好的解决方案就是根据不同需求处理

## 需要 git 版本控制的文件夹，直接在 .git_ignore 中添加忽略

## 也可以在 .git_ignore_global 中添加，所有 git 项目都会忽略

## 压缩包中忽略可以在压缩软件中添加
