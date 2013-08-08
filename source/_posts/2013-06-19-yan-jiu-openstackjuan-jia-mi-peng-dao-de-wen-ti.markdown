---
layout: post
title: "研究OpenStack卷加密碰到的问题"
date: 2013-06-19 20:23
comments: true
categories: OpenStack
---
### OpenStack环境搭建
这一步有两个选择:


1.	一种就是正统的真实搭建，所有都按生产环境来部署，费时费力。
2.	还有一种就是官方推荐的一键安装[***DevStack***](http://devstack.org)，直接安装最新的版本，体验最新的特性。

至于如何选择，就看需求。当时我选择了DevStack，因为本身只是去实现个功能，这个就完全满足了。安装有点费时，默认会安装在 /opt/stack/ 下面，里面还有个devstack的目录，一般在这里启动，停止DevStack。官方没有自带重启 DevStack 的各种服务，不过高手自在民间，于是有外国有人写了重启的脚本。[***Restarting DevStack***](http://www.scalegrid.net/blog/?p=52)。
所有的都OK了，然后访问服务器地址，就能看到Horizon了，账号admin，密码在服务器启动的时候，最后的地方会有提示，当然也就是你安装DevStack的时候输入的。

### 卷加密
环境OK了，接下来研究卷加密。google了下LVM加密相关的内容，发现了Linux下面用[***cryptsetup***](https://code.google.com/p/cryptsetup/)就能够实现卷加密的功能，这个可以通过apt-get来获取，只有几百KB。它把LVM加密过后必须要输入正确的密码才能mount使用，否者就无法挂载，自然也就不能查看里面的内容。操作的对象为LVM，所以需要事先创建好LVM，[***如何创建LVM看这里***](http://space.itpub.net/82392/viewspace-166638)。

cryptsetup LUKS加密步骤如下(假设LVM名为:lvm1, VG名为:vgtest):
```bash
cryptsetup luksFormat /dev/vgtest/lvm1
#回车后，会要求输入大写的YES，然后就输入密码
```
解密，并使用：
```bash
cryptsetup luksOpen /dev/vgtest/lvm1 encryptLVM
#回车后，要求输入刚才加密时设置的密码，正确则会创建一个Device-Mapper文件，/dev/mapper/encryptLVM，错误则无法生成，也就无法挂载使用。
	
#格式化系统，并挂载使用
mkfs.ext3 /dev/mapper/encryptLVM
mkdir /mnt/crypt
mount /dev/mapper/encryptLVM /mnt/crypt
```

卸载设备:
```bash
umount /mnt/crypt
cryptsetup luksClose /dev/mapper/encryptLVM
```

如果要再使用的话，只需要再luksOpen，然后mount就可以了，不需要再luksFormat了。

### 整合到DevStack
加密和解密过程发生在LVM的attach和detach过程中，所以只需要在/nova/virt/libvirt/volume.py中的相关部分加上cryptsetup的加密，解密和关闭操作就可以实现LVM加密的功能，这部分就看个人怎么实现了。具体的实现代码可参考[Add encryption support for volumes](https://review.openstack.org/#/c/30976/)。


---
