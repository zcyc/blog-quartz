---
title: "CefSharp Video and Audio Playback"
---


由于 chromium 没有版权，所以不能播放 mpeg/h264/h265 格式的文件。可以使用 ogv 格式，也可以自己编译 CEF。
CefSharp 允许自动播放视频，但不能自动播放音频。自动播放视频也必须静音才行，还要给 video 元素设置 autoplay 属性。
