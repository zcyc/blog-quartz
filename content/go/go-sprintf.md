---
title: "Go Sprintf %%s%%"
---


要用 %%%s%%，不能用 \ 转义。

```go
fmt.Sprintf("%%%s%%", "a")
// 最后的效果就是 %a%
```
