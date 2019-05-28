[TOC]

# 1. 修改 u-boot 的设备树

修改文件 **arch/arm/dts/zynq-zed.dts**

```
/dts-v1/;
#include "zynq-7000.dtsi"

/ {
        model = "Zynq Zed Development Board";
        compatible = "xlnx,zynq-zed", "xlnx,zynq-7000";

        aliases {
                ethernet0 = &gem0;
                serial0 = &uart1;
                spi0 = &qspi;
        };

        memory {
                device_type = "memory";
                reg = <0x0 0x18000000>;
        };

        chosen {
                bootargs = "earlyprintk";
                linux,stdout-path = &uart1;
                stdout-path = &uart1;
        };

        usb_phy0: phy0 {
                compatible = "usb-nop-xceiv";
                #phy-cells = <0>;
        };
};

```

# 2. 编译 u-boot

```
$ make distclean

$ make ARCH=arm CROSS_COMPILE=arm-xilinx-linux-gnueabi- zynq_zed_config 

$ make ARCH=arm CROSS_COMPILE=arm-xilinx-linux-gnueabi-


```

生成 u-boot，重命名为u-boot.elf; 使用FPGA的fsbl、bit文件和u-boot.elf 制作成BOOT.bin; 将BOOT.bin 烧录到板卡。

# 3. 查看板卡内存

```
root@linux:/tmp# cat /proc/meminfo 
MemTotal:         333796 kB
MemFree:          169072 kB
MemAvailable:     201748 kB
Buffers:             916 kB
Cached:            45920 kB

```
