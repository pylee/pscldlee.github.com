---
layout: post
title: "调用python-novaclient遇到的问题"
date: 2013-11-19 12:05
comments: true
categories: OpenStack 
---
在做私有应用平台的过程中，需要对nova操作，这里选择直接调用python-novaclient的python api。o(╯□╰)o的是，python api没有文档，只能一个一个的翻源文件，好在代码非常规范，注释写的非常详细，这里就感受到了具有良好的编码习惯是件多么让后来人敬佩。在调用这些api的时候遇到了一些问题，这里集中记录说明：

1. `Error: No nw_info cache associated with instance (HTTP 400)`
> 这个原因是获取不到instance的fixed_ip的信息，也就是instance还没初始化好，我实在给instance添加floating_ip的时候遇到的这个问题。
> [参考引用](http://www.gossamer-threads.com/lists/openstack/dev/23837)

---
