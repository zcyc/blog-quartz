---
title: "macOS 常用命令备忘"
---


# 一键更新 brew

```bash
brew update && brew upgrade && brew upgrade --greedy
```

# 隐藏桌面

```bash
defaults write com.apple.finder CreateDesktop -bool false && killall Finder
```

# 显示桌面

```bash
defaults write com.apple.finder CreateDesktop -bool true && killall Finder
```
