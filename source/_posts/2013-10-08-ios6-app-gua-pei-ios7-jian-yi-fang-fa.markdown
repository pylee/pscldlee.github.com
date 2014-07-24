---
layout: post
title: "iOS6 APP 适配 iOS7 简易方法"
date: 2013-10-08 11:21
comments: true
categories: iOS
---
最近需要适配一个iOS6 APP到iOS7下，发现View会顶到最上面。查了下发现，因为iOS7默认开启了导航栏的半透明效果，能够看见View从它下面滑过，这样的话会导致原有布局正常的APP发生界面尺寸错乱，整体上移，底部留白。
通过如下设置可以关掉这个效果，让布局同iOS6一致。

```objc
self.navigationController.navigationBar.translucent = NO;
```

---
