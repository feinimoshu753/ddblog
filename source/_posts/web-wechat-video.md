---
title: video标签在ios微信中的一个问题
date: 2017-03-22 07:23:10
tags: 前端
---
ios微信video标签设置preload为none，点击自定义播放按钮调用video.play()方法仅触发了play事件，没有进行视频播放（未进入playing、loadedData等事件）。补充：safari浏览器正常。

<!-- more -->

解决方法：将preload改为auto就可以正常播放，原因未知。
