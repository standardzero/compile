[TOC]

# 1. zynq 烧录

## 1.1 BOOT.bin 制作

* 根据逻辑的hdf文件在SDK上新建一个名为`xxx_hw`硬件工程，会生成一个`ps7_init.ctl`文件和`xxx.bit`，`ps7_init.ctl`文件jtag烧录时候需要用到。
* 根据硬件工程生成一个fsbl工程，工程名字为`xxx_fsbl`, 编译后生成一个`xxx_fsbl.elf`
* 编码uboot源码，生成u-boot，将u-boot重命名为u-boot.elf

* 使用SDK将`xxx_fsbl.elf` 、`xxx.bit`和`u-boot.elf` 生成 `BOOT.bin`

## 1.2 烧录

如下命令都在SDK的`XMD Console`窗口下操作

```
//连接arm
$ connect arm hw

//相关初始化，需要使用到 ps7_init.tcl
$ source ps7_init.tcl;ps7_init;init_user;rst -processor

//将BOOT.bin down到内存0x1000000
$ dow -data BOOT.bin 0x1000000

//down u-boot.elf
dow u-boot.elf

//输完con后，需迅速切到串口界面敲回车
con

//串口界面操作u-boot 命令烧录
//擦除flash
$ nand erase 0x0 0x300000

//将down到DDR的内容写入到flash
$ nand write 0x1000000 0x0 0x300000

//回到SDK 断开连接
disconnect 64

```