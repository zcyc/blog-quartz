---
title: ".gitignore Not Working & Previously Committed Files Cannot Be Ignored"
---


```bash
// 一般情况下执行下边的语句，这个会把目录下所有文件从版本控制移除，你只需要重新将需要的文件添加到版本控制就行。
git rm -r --cached . [这里的 . 你可以替换成你本地的目录或者文件也可以]

// 如果你执行上边的不管用，请执行下边的。这个会强制把目录下所有文件从版本控制移除。不过你放心，不会影响文件，只是移除版本管理。
git rm -r -f --cached . [这里的 . 你可以替换成你本地的目录或者文件也可以]

// 上边这句话执行完基本上你只要 git add * 就可以了，因为这时候 gitignore 已经生效了。
```
