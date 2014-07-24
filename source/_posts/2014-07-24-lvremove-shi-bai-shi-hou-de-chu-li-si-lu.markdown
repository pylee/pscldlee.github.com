---
layout: post
title: "当lvremove失败时的解决思路"
date: 2014-07-24 11:16
comments: true
categories: openstack 
---
在删除逻辑卷的时候，遇到了如下问题：

```
# lvremove /dev/cinder-volumes/volume-cf5565aa-291c-49ba-a75c-6842d4655d34
Can't remove open logical volume "volume-cf5565aa-291c-49ba-a75c-6842d4655d34"
```

在serverfault.com找到如下解决方案:

```
# fuser /dev/cinder-volumes/volume-cf5565aa-291c-49ba-a75c-6842d4655d34
/dev/dm-14:           1749

# lsof | grep /dev/dm-14
tgtd       1749       root   19u      BLK             252,14         0t0       8546 /dev/dm-14

# service tgt stop
tgt stop/waiting

# lvremove /dev/cinder-volumes/volume-cf5565aa-291c-49ba-a75c-6842d4655d34 
Do you really want to remove active logical volume volume-cf5565aa-291c-49ba-a75c-6842d4655d34? [y/n]: y
  Logical volume "volume-cf5565aa-291c-49ba-a75c-6842d4655d34" successfully removed
```

[参考引用](http://serverfault.com/questions/266697/cant-remove-open-logical-volume)

---
