# 虚拟机Ubuntu版本
```bash
$ uname -a
Linux ubuntu 5.4.0-150-generic #167~18.04.1-Ubuntu SMP Wed May 24 00:51:42 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux

# 清空回收站
sudo rm -rf ~/.local/share/Trash/files/*
```

# 其他指令
```bash
# 获取IP
sudo dhclient ens33
```

# Xilinx工具相关指令
```bash
# Y2K22补丁
python3 y2k22_patch/patch.py

# Source
source /opt/Xilinx/Vivado/2020.1/settings64.sh
source /opt/Xilinx/Vitis/2020.1/settings64.sh
source /opt/Xilinx/petalinux/2020.1/settings.sh

source /opt/Xilinx/Vivado/2022.2/settings64.sh
source /opt/Xilinx/Vitis/2022.2/settings64.sh
source /opt/Xilinx/petalinux/2022.2/settings.sh

# 调整JVM内存堆栈大小
vivado -stack 2000 *.xpr 

# SDKmain
source /opt/petalinux/2020.1/environment-setup-aarch64-xilinx-linux
source /opt/petalinux/2022.2/environment-setup-cortexa72-cortexa53-xilinx-linux

# Petalinux
petalinux-config -c rootfs
petalinux-config --get-hw-description ../hardware
petalinux-build --sdk
petalinux-build -c device-tree -x cleanall
petalinux-build -c device-tree
petalinux-build -c <app_name> -x do_clean
petalinux-build -c <app_name>
petalinux-build -x mrproper -f
petalinux-build 
petalinux-package --boot --fsbl --fpga --u-boot --force
petalinux-package --wic --bootfiles "boot.scr,BOOT.BIN,image.ub"
petalinux-build -s
petalinux-package --sysroot

# Modules
petalinux-create -t modules --name aaa --enable
petalinux-create -t apps --template install -n <app_name> --enable
petalinux-build -c <app_name>

# BSP
petalinux-package --bsp -p ./petalinux/ --output my_bsp.bsp
petalinux-create -t project -s ./my_bsp.bsp -n petalinux_prj_name

# SD rootfs
sudo tar -zxvf ./images/linux/rootfs.tar.gz -C /media/keyneko/EXT
sudo sync

# Petalinu构建
petalinux-create --type project --template zynqMP --name axu2cg_petalinux

cd axu2cg_petalinux

petalinux-config --get-hw-description=../../axu2cg_hardware_platform/

/home/keyneko/Downloads/2022.2/u-boot-xlnx-master
/home/keyneko/Downloads/2022.2/linux-xlnx-master
file:///home/keyneko/Downloads/2022.2/downloads
/home/keyneko/Downloads/2022.2/aarch64


## -----------------------------
## Vitis flow
# createdts -hw ../axu2cg_hardware_platform/system_wrapper.xsa -zocl  -platform-name mydevice -git-branch xlnx_rel_v2022.2 -dtsi system-user.dtsi -compile
# 
# createdts -update -hw newdesign.xsa
# createdts -update -hw ../axu2cg_hardware_platform/system_wrapper.xsa -dtsi system-user.dtsi

petalinux-config --get-hw-description ../../axu2cg_hardware_platform/


mkdir pfm 
mkdir pfm/boot
mkdir pfm/sd_dir
mkdir pfm/sw_comp 

cp axu2cg_fsbl/zynqmp_fsbl/fsbl_a53.elf pfm/boot/fsbl.elf
cp axu2cg_fsbl/zynqmp_pmufw/pmufw.elf pfm/boot/

cp petalinux/images/linux/zynqmp_fsbl.elf pfm/boot/fsbl.elf
cp petalinux/images/linux/pmufw.elf pfm/boot/
cp petalinux/images/linux/bl31.elf pfm/boot/
cp petalinux/images/linux/u-boot.elf pfm/boot/
cp petalinux/images/linux/boot.scr pfm/sd_dir/
cp petalinux/images/linux/rootfs.ext4 pfm/sw_comp
cp petalinux/images/linux/Image pfm/sw_comp
cp petalinux/images/linux/system.dtb pfm/boot/
cp petalinux/images/linux/system.dtb pfm/



# DPU Vitis编译流程(gui)
# Sysroot
/opt/petalinux/2022.2/sysroots/cortexa72-cortexa53-xilinx-linux

# Root FS
/home/keyneko/Documents/AXU2CG/2022.2/WorkSpace/axu2cg_software_platform/pfm/sw_comp/rootfs.ext4

# Kernel image
/home/keyneko/Documents/AXU2CG/2022.2/WorkSpace/axu2cg_software_platform/pfm/sw_comp/Image

# Package options
--package.sd_dir=../../dpu_trd/src/app

# # DPU Vitis编译流程(cmd)
# export TRD_HOME=/home/keyneko/Documents/AXU2CG/2022.2/DPUCZDX8G_VAI_v3.0
# 
# cd $TRD_HOME/prj/Vitis
# 
# export EDGE_COMMON_SW=/home/keyneko/Documents/AXU2CG/2022.2/WorkSpace/# axu2cg_software_platform/xilinx-zynqmp-common-v2022.2
# 
# export SDX_PLATFORM=/home/keyneko/Documents/AXU2CG/2022.2/WorkSpace/# axu2cg_software_platform/axu2cg_custom/export/axu2cg_custom/axu2cg_custom.xpfm
# 
# make all KERNEL=DPU

env LD_LIBRARY_PATH=samples/lib XLNX_VART_FIRMWARE=/run/media/mmcblk0p1/dpu.xclbin samples/bin/resnet50 strawberry.jpg

```



# ZynqMP目标板相关指令
```bash
mount -t nfs -o nolock 192.168.1.151:/home/keyneko/Work /mnt

# 选择显示设备
export DISPLAY=:0.0

echo 7 4 1 7 > /proc/sys/kernel/printk

resize-part /dev/mmcblk0p2

# app
cd /lib/modules/5.4.0/extra/
depmod
modprobe alinx-led.ko
insmod alinx-led.ko
lsmod
# mknod /dev/alinx-led c 200 0
ls /dev/alinx-led
./ps-led_test /dev/alinx-led on
rmmod alinx-led.ko
```
