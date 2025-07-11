---
title: "Issues with Using Antd and Tailwind CSS Together"
---


_由于svg的对齐方式不同，所以需要覆盖 tailwind 的 svg样式_
_/_ ./styles/global.css _/
_@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
    svg {
        vertical-align: initial;
    }
}

或者将 tailwind.config.js 改成
module.exports = {
  \_// 关闭全局基础样式，防止和其他样式库冲突
 // corePlugins: {
 //   preflight: false,
 // },
 _purge: \['./pages/**/\*.js', './components/**/\*.js'],
  darkMode: false, \_// or 'media' or 'class'
 _theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: \[],
}
再或者 参考 <https://ant.design/docs/react/customize-theme-cn> 关闭antd的全局样式
