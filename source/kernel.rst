Linux Kernel
============

Like we saw for the :ref:`bootloader <bsp_bootloader_label>`, the first thing you need is: sources.
Get them from *Bitbake* build directory (if you built the kernel with it) or get them from the Internet.

*Bitbake* will place the sources under directory:

.. host::

 | /path/to/build/tmp/work/zedboard_zynq7-poky-linux-gnueabi/linux-xlnx/3.8-xilinx+gitf4ff79d44a966ebea6229213816d17eb472b303e-r1/git


If you are working with the virtual machine, you will find them under directory:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/work/zedboard_zynq7-poky-linux-gnueabi/linux-xlnx/3.8-xilinx+gitf4ff79d44a966ebea6229213816d17eb472b303e-r1/git


We suggest you to **don't work under Bitbake build directory**, you will pay a speed penalty and you could
have troubles syncronizing the all thing. Just copy them some place else and do what you have to do.

If you didn't build them already with *Bitbake* or you just want to do make every step by hand, you can
always get them from the Internet by cloning the proper repository and checking out the proper hash commit:

.. host::

 | cd ~/Documents
 | git clone git://github.com/Xilinx/linux-xlnx
 | cd linux-xlnx
 | git checkout f4ff79d44a966ebea6229213816d17eb472b303e

and by properly patching the sources:

.. host::

 | cd ~/Documents
 | git clone git://git.yoctoproject.org/meta-xilinx.git
 | cd meta-xilinx/
 | git checkout cb7329a596a5ab2d1392c1962f9975eeef8e4576
 | cd ..
 | patch -p1 -d linux-xlnx/ < meta-xilix/recipes-kernel/linux/linux-xlnx/libtraceevent-Remove-hard-coded-include-to-usr-local.patch
 | cp /home/@user@/architech_sdk/architech/@board-alias@/yocto/meta-xilinx/conf/machine/boards/common/zynq_defconfig_3.8.cfg ~/linux-xlnx/.config

Source the script to load the proper evironment for the cross-toolchain (see :ref:`manual_compilation_label`
Section) and you are ready to customize the kernel:

.. host::

 | cd ~/Documents/linux-xlnx
 | make menuconfig

and to compile it:

.. host::

 make -j <2 * number of processor's cores> uImage

By the end of the build process you will get **uImage** under *arch/arm/boot*.

.. host::

 ~/Documents/linux-xlnx/arch/arm/boot/uImage

Enjoy!
