---
title: "Using Ant Landing Page in the Latest Version of Umi"
---


```````bash
# 首先初始化一个 umi 项目
mkdir myapp && cd myapp
yarn create @umijs/umi-app
yarn
```把 ant landing 下载的文件夹复制到 src 里边
比如你下载下来是 Home 文件夹，到 src/index/index.tsx 中引入```typescript
- import styles from './index.css';
+ import Home from '../Home';

export default function() {
  return (
-   <div className={styles.normal}>
-     <h1>Page index</h1>
-   </div>
+   <Home />
  );
}
``````bash
# 启动项目
yarn start
```````
