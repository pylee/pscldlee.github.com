---
layout: post
title: "如何安装pip"
date: 2013-08-08 14:27
comments: true
categories: Python 
---
pip是一个专门用来安装Python包的管理的工具，比如用来安装django，用法就想apt-get一样，非常的方便。
工具的安装方法如下:
```objc
先安装Setuptools
wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
python ez_setup.py --user

在安装pip
curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
[sudo] python get-pip.py
```

---
