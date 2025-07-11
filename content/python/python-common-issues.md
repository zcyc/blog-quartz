---
title: "Common Issues in Python"
---


# site-packages 不可写

报错 Defaulting to user installation because normal site-packages is not writeable
通过 target 指定目录，如：

```
pip install --target=C:\Users\z\pip\site-packages pytest-playwright
```
