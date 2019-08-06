---
layout: post
title: "QEMU 基本技巧"
subtitle: 'QEMU basic skills'
author: "Tao Xu"
header-style: text
tags:
  - Linux
  - Virtualization
  - QEMU
---

## Linux 环境下源码安装 QEMU
### 下载地址
```sh
git clone https://git.qemu.org/git/qemu.git
```
### 安装依赖包
#### For Ubuntu (Debian based distributions)
```sh
sudo apt-get install git libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev
```
#### For RHEL， CentOS and Fedora
```sh
yum install git glib2-devel libfdt-devel pixman-devel zlib-devel
```
#### 推荐的额外安装包
```sh
sudo apt-get install git-email
sudo apt-get install libaio-dev libbluetooth-dev libbrlapi-dev libbz2-dev
sudo apt-get install libcap-dev libcap-ng-dev libcurl4-gnutls-dev libgtk-3-dev
sudo apt-get install libibverbs-dev libjpeg8-dev libncurses5-dev libnuma-dev
sudo apt-get install librbd-dev librdmacm-dev
sudo apt-get install libsasl2-dev libsdl1.2-dev libseccomp-dev libsnappy-dev libssh2-1-dev
sudo apt-get install libvde-dev libvdeplug-dev libvte-2.90-dev libxen-dev liblzo2-dev
sudo apt-get install valgrind xfslibs-dev
```
#### 新版 Ubuntu
```sh
sudo apt-get install libnfs-dev libiscsi-dev
```
#### 新版 RHEL Centos
```sh
sudo yum install libaio-devel libcap-devel libiscsi-devel
```
### 编译源码
全目标平台编译
```sh
cd qemu
mkdir -p bin
cd bin
../configure
make -j12
```
x86 目标平台
```sh
./configure --target-list=x86_64-softmmu
```
## QEMU 常用启动参数 （x86 平台）
x86 平台的可执行程序位置为 /x86_64-softmmu/qemu-system-x86_64
### CPU 参数
```sh
-cpu host -smp 4 \
```
表示使用 4 个核心，Host passthrough 模式，最大化接近 host。或者
```sh
-cpu Skylake-Server -smp 4 \
```
表示使用 4 个核心，Skylake-Server CPU model 模式
关于 QEMU CPU Model 请参考
[QEMU CPU Model](2019-08-07-qemu-cpu-model.md)
### 内存参数
```sh
-m 3G,slots=4,maxmem=32G \
```
表示 Guest 具有 3G 内存，有 4 个插槽，每个插槽最大 32G。
注：只有定义了插槽才能使用 hot-plug
### 虚拟化技术
```sh
-enable-kvm \
```
表示使用 KVM
```sh
-bios OVMF.fd \
```
Guest 是 UEFI 镜像的时候必须指定 -bios
```sh
-hda XXX.img \
```
指定 Guest 镜像
```sh
-vga virtio \
```
指定显示模式
```sh
-qmp tcp:localhost:4444,server,nowait \
```
```sh
telnet localhost 4444
```
制定 QMP 接口，启动之后可以使用 telnet 连接 QMP
```sh
-serial mon:stdio \
```
将串口输出打印到当前窗口，在内核debug中非常重要
```sh
-net nic,model=virtio \
-net user,hostfwd=tcp::10022-:22
```
```sh
ssh -p 10022 username@localhost
```
设置网络和端口转发，可使用 ssh 连接 Guest