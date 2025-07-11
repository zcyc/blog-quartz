---
title: "复制过来的 ssh 密钥无法使用"
---


不同系统之间复制 ssh 密钥需要赋予正确的权限，以 mac 为例：

```bash
chmod 0600 id_rsa
```
