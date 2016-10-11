---
layout: post
title: openwrt build for asus rt-n13u b1
---

##env & prepare 
ubuntu 16.04 64bit
```
sudo apt-get update
sudo apt-get install build-essential subversion git-core libncurses5-dev zlib1g-dev gawk flex quilt libssl-dev xsltproc libxml-parser-perl mercurial bzr ecj cvs unzip
sudo apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext libssl-dev
```
##下载代码
非常慢，非常慢
```
git clone git://git.openwrt.org/15.05/openwrt.git
```
##更新
全程联网，也非常慢，非常慢
{% highlight ruby %}
cd openwrt
./script/feeds update -a
./script/feeds install -a
{% endhighlight %}
##配置
```
make defconfig
make prereq
make menuconfig
```
配置选项
```
Target System (Ralink RT288x/RT3xxx)  --->  
Subtarget (RT3x5x/RT5350 based boards)  --->  
Target Profile (Asus RT-N13U)  --->   
LuCI  --->
    1. Collections  ---> 
        -*- luci 
```
其它配置根据需要选择

##编译
编译过程需要下载各种包，联网，非常慢，非常慢
```
make -j1 V=s
```
编译完成后会在openwrt/bin/ramips下产生：
```
openwrt-ramips-rt305x-rt-n13u-squashfs-sysupgrade.bin
```
##升级
webUI升级不介绍了，这里说通过ssh升级，因为第一次升级的img没luci，没法通过webui升级。
openwrt默认开启ssh，账号root，密码admin
###ssh链接与下载
```
ssh root@192.168.8.1
scp ./openwrt-ramips-rt305x-rt-n13u-squashfs-sysupgrade.bin root@192.168.8.1:/tmp/
```
###升级
在ssh中执行
```
sysupgrade -v openwrt-ramips-rt305x-rt-n13u-squashfs-sysupgrade.bin
```

参考链接  
https://wiki.openwrt.org/zh-cn/doc/howto/buildroot.exigence  
https://wiki.openwrt.org/zh-cn/doc/howto/build
