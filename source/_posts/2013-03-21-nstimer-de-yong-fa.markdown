---
layout: post
title: "NSTimer 不准时的处理"
date: 2013-03-21 15:45
comments: true
categories: iOS
---
NSTimer作为iOS开发中的定时器，用处广泛，最近在使用它作为倒计时时，出了点小状况，如果使用如下方法初始化：
```objc
NSTimer scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:
```
如果我在拖动UIScrollView并且不放的时候，Timer并不会被触发，1s的间隔，我猜测可能是Timer附着在主线程上，也就是UI线程，如果主线程需要做UI重绘等大量计算或者被阻塞住，那么Timer的计时准确率就会变低，什么时候来都不确定，只有主线程空闲了，才会准确到达（任然有50-100毫秒误差），后来查阅了文档，果然是说Timer附着在NSRunLoop里，所以要保证Timer的触发，就应该换到其他NSRunLoop去，用下面方法就可以解决：
```objc
yourTimer = [NSTimer timerWithTimeInterval:1.0f target:self selector:@selector(updateTime) userInfo:nil repeats:YES];;
[[NSRunLoop currentRunLoop] addTimer:yourTimer forMode:NSRunLoopCommonModes];
```
具体参数信息就需要自己查文档了。

---
