
.. _bsp_kernel_label:

Linux Kernel
============

Like we saw for the :ref:`bootloader <bsp_bootloader_label>`, the first thing you need is: sources.
Get them from *Bitbake* build directory (if you built the kernel with it) or get them from the Internet.

*Bitbake* will place the sources under directory:

.. host::

 | /path/to/build/tmp/work/zedboard-poky-linux-gnueabi/linux-xlnx/3.17-xilinx+gitAUTOINC+7b042ef9ea-r0

If you are working with the virtual machine, you will find them under directory:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/work/zedboard-poky-linux-gnueabi/linux-xlnx/3.17-xilinx+gitAUTOINC+7b042ef9ea-r0

We suggest you to **don't work under Bitbake build directory**, you will pay a speed penalty and you could
have troubles syncronizing the all thing. Just copy them some place else and do what you have to do.

If you didn't build them already with *Bitbake* or you just want to do make every step by hand, you can
always get them from the Internet by cloning the proper repository and checking out the proper hash commit:

.. host::

 | cd ~/Documents
 | git clone -b xlnx_3.17 git://github.com/Xilinx/linux-xlnx.git 
 | cd linux-xlnx
 | git checkout 7b042ef9ea5cc359a22110c75342f8e28c9cdff1

and by properly patching the sources:

.. host::

 | cd ~/Documents
 | git clone git://git.yoctoproject.org/meta-xilinx.git
 | cd meta-xilinx/
 | git checkout 7f759048bb0aeef3c0b3938be81d2bcade7acb7e
 | cd ..
 | git clone -b dizzy https://github.com/architech-boards/meta-zedboard.git
 | patch -p1 -d linux-xlnx/ < meta-xilix/recipes-kernel/linux/linux-xlnx/3.17/tty-xuartps-Fix-RX-hang-and-TX-corruption-in-set_termios.patch
 | patch -p1 -d linux-xlnx/ < meta-zedboard/recipes-kernel/linux/linux-xlnx/3.17/0001-Updated-the-TI-Wilink8-driver-to-R8.5.patch
 | patch -p1 -d linux-xlnx/ < meta-zedboard/recipes-kernel/linux/linux-xlnx/3.17/0002-Patching-kernel-to-adapt-TI-Wilink8-driver.patch
 | patch -p1 -d linux-xlnx/ < meta-zedboard/recipes-kernel/linux/linux-xlnx/3.17/0003-Fixed-TI-Wilink8-driver-with-kernel-structure.patch
 | cp meta-zedboard/recipes-kernel/linux/linux-xlnx/3.17/defconfig linux-xlnx/.config


add the copy of devicetree dts:

.. host::

 | cp meta-zedboard/conf/machine/boards/@machine-name@/zedboard.dtsi linux-xlnx/arch/arm/boot/dts
 | cp meta-zedboard/conf/machine/boards/@machine-name@/zedboard-mmcblk0p2.dts linux-xlnx/arch/arm/boot/dts
 | cp meta-zedboard/conf/machine/boards/@machine-name@/zedboard-ram.dts linux-xlnx/arch/arm/boot/dts


Source the script to load the proper evironment for the cross-toolchain (see :ref:`manual_compilation_label`
Section) and you are ready to customize the kernel:

.. host::

 | source /home/@user@/architech_sdk/architech/@board-alias@/toolchain/environment-nofs
 | cd ~/Documents/linux-xlnx
 | make

and compile the rigth device tree depending if you need to work only on the RAM or also with an MMC:

.. host::

 | make zedboard-ram.dtb
 | or
 | make zedboard-mmcblk0p2.dtb

you can find the .dtb files in *arch/arm/boot/dts*

.. note::

 | if you need to add some custom properties to the kernel compile with:
 | make menuconfig

and to compile it:

.. host::

 | make -j <2 * number of processor's cores> uImage UIMAGE_LOADADDR=0x8000

By the end of the build process you will get **uImage** under *arch/arm/boot*.

.. host::

 | ~/Documents/linux-xlnx/arch/arm/boot/uImage

Enjoy!
