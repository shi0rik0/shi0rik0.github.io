---
title: "BT种子文件和磁力链的区别"
date: 2024-06-16T21:54:50+08:00
draft: false
---

在BT分享网站上，经常可以看见一个资源有两种下载方式：BT种子文件或者磁力链。使用这两种方式有什么区别呢？先下结论：磁力链只存储了BT种子文件部分内容的hash值。使用磁力链下载文件的时候，要首先从拥有资源的节点上根据hash值获得BT种子文件，然后才能下载。因此可以说磁力链只是一个中间过程，BT种子文件才是最终的结果。

接下来将首先介绍使用BT协议下载文件的时候需要的信息，然后将分别介绍BT种子文件和磁力链分别存储了哪些信息。

## BT协议需要的信息

要让BT协议工作，需要两个最关键的信息。第一个是tracker服务器的地址。第二个是文件名及其hash值。Tracker服务器的作用是让需要下载资源的节点能够发现拥有资源的节点。在这两个信息中，文件名及其hash值显然是更重要的，因为没有这个信息，我们就无法确定要下载的文件。至于tracker服务器地址这个信息其实并不是那么重要，因为tracker服务器并不是唯一且固定的，知名的tracker服务器列表在互联网上都可以查到，所以我们直接让BT客户端使用互联网上的tracker列表就行。

## BT种子文件

BT种子文件本质上是一个使用某种特定编码的字典。这里的字典是指能够存储键值对的数据结构。在这个字典中有两个关键的字段：announce和info。annouce字段中存储了tracker服务器的地址，info字段中则存储了文件名及其hash值。

## 磁力链

磁力链本质上也是存储了若干键值对，其格式如下：
```
magnet:?{key1}={value1}&{key2}={value2}&{key3}={value3}...
```

常见的key有两个：tr、dn和xt。tr用于指定tracker服务器地址。dn用于指定该资源的名称，这个名称仅仅只是为了方便用户识别，理论上可以随意指定。xt则存储了BT种子文件中info字段的hash值。通过这个hash值，就可以在拥有该资源的节点上获取对应的BT种子。但很显然，如果找不到拥有该资源的节点，就无法通过磁力链获取文件的具体信息。

## 参考资料

1. https://en.wikipedia.org/wiki/Torrent_file
2. https://en.wikipedia.org/wiki/Magnet_URI_scheme