﻿---
layout: post
title:  have no enough inode
date:   2018-02-27 15:15:58
categories: Linux
tags: Linux
---


今天遇到inode不够的问题了，网上大多数文章都是让删除文件。我这是在盘里拷贝图片，多是必须的呀。咋办。
我使用docker挂载ceph镜像inode只有6.3M，删除是不可能了。我的处理方法如下。。。。
1. 卸载盘</br>
- umount /dev/rbd0
2. 建立文件系统，指定inode节点数  （-N 指定inode大小）</br>
- mkfs.ext4 /dev/rbd6 -N 128000000
3. 挂回到原来的位置</br>
4. 查看修改后的inode参数</br>  
- dumpe2fs -h /dev/sda6 | grep node  
