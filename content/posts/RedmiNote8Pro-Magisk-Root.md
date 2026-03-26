---
date: '2026-03-26T14:13:53+08:00'
draft: false
title: 'RedmiNote8Pro通过Magisk刷Root'
---

在Redmi Note 8 Pro设备上刷Magisk、刷Root的过程中遇到了一些问题，查找了很多资料，最终成功刷入Magisk和Root。本篇涉及以下问题：

> 1. 下载Magisk后，发现手机上没有Ramdisk。能否正常安装Magisk？
> 2. 在手机的fastboot模式下刷入Magisk修改后的镜像，手机无法进入系统，循环启动无法开机，怎么解决？



# 1. 解锁BL锁，刷机

想要刷Root，解锁BL锁（Boot Loader锁）是必需的步骤。解锁了BL锁后，还可以根据个人需求或者喜好，刷入不同的MIUI系统。下面是解锁BL锁、刷机时参考的两篇文章：

> 链接1，https://zhuanlan.zhihu.com/p/408114647
>
> 链接2，https://zhuanlan.zhihu.com/p/499270772

其中链接1的文章介绍了MIUI系统不同地区版本的区别。我最终安装了MIUI 11.03，国内发行稳定版。



# 2. 安装Magisk

需要在电脑上提前安装好Android SDK工具，后续需要使用到其中的fastboot工具。[官网链接](https://developer.android.com/tools/releases/platform-tools?hl=zh-cn)在这里。

在[Magisk官网](https://github.com/topjohnwu/Magisk/releases)上下载最新版的apk安装包，安装到手机上。正常情况下，后续的安装步骤很简单。下面是两个参考链接：

> 官方安装指导：https://topjohnwu.github.io/Magisk/install.html
>
> 其它安装指导：https://miuiver.com/how-to-root-xiaomi-phone/

很不幸，我从这里开始，就走上了和这些博客不同的坎坷安装道路。

遇到的问题1，我的Redmi设备安装了Magisk后，识别不到Ramdisk。查找资料后，我觉得按照正常安装流程莽上去干就是了。不行再换种方法。

遇到的问题2，我的Redmi设备刷入Magisk修补的镜像后，无法进入MIUI系统，循环启动无法正常开机。补充说明，遇到循环重启的情况，手机进入fastboot模式，重新刷机、刷入新的系统，就能回归正常情况。但这也意味着恢复出厂设置，安装了一半的Magisk也被清除了。

之所以遇到问题2，会不会因为问题1没有Ramdisk的原因呢？排除后发现不是。只要修改了镜像，就不能开机，更别说安装Magisk了。那咋整？查阅了资料后，我找到了以下解决方案：

> + 方案1：安装MIUI其它地区的版本，比如国际版、欧洲版等。国行版的MIUI系统似乎有一定特殊性，其它地区的版本不容易遇到循环启动的问题。
>
> + 方案2：正常安装流程是在fastboot模式下刷入Magisk修补的镜像，就能完成安装。但是部分小米、红米手机比较特殊，需要完成“刷入镜像-清除数据-再次刷入镜像”三个步骤，才能安装Magisk。
>
> + 链接：https://github.com/topjohnwu/Magisk/issues/2008。两个方案都在其中被提到。

我尝试了方案2，验证了方案2的可行性。手机恢复正常开机，安装Magisk的apk，修补MIUI ROM的`boot.img`文件，把修改后的boot镜像保存在电脑上。手机进入fastboot模式，在电脑的命令行下依次输入以下命令：

```
fastboot flash boot /path/to/magisk_patched_[random_strings].img
fastboot -w
fastboot flash boot /path/to/magisk_patched_[random_strings].img
```

其中的`/path/to/magisk_patched_[random_strings].img`是修补后的`boot.img`文件的路径+文件名。

输入完命令后，重启手机。



# 3. 完结撒花

手机重启后，Magisk的版本莫名其妙变成了1.0（最初版，图标也很简陋）。覆盖安装最新版。打开Magisk，可以看到已经取得root权限。安装完成。
