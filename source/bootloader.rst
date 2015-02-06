U-boot
======

The bootloader used by @board@ is **u-boot**. 
If you want to browse/modify the sources first you have to get them. There are two viable
ways to do that:

* if you already built @board@'s bootloader with *Bitbake*, then you already have them on your (virtual) disk, otherwise

* you can download and patch them.

*Bitbake* will place *u-boot* sources under:

.. host::

 | /path/to/build/tmp/work/zedboard_zynq7-poky-linux-gnueabi/u-boot-xlnx/v2013.01-xilinx+gitAUTOINC+20a6cdd301-r1/git

this means that within the virtual machine you will find them under:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/work/zedboard_zynq7-poky-linux-gnueabi/u-boot-xlnx/v2013.01-xilinx+gitAUTOINC+20a6cdd301-r1/git


We suggest you to **don't work under Bitbake build directory**, you will pay a speed penalty
and you can have troubles syncronizing the all thing. Just copy the sources some place else
and do what you have to do.

If you didn't build them already with *Bitbake*, or you just want to make every step by hand,
you can always get the sources from the Internet by cloning the proper repository and checking
out the proper commit:

.. host::

 | cd ~/Documents
 | git clone git://github.com/Xilinx/u-boot-xlnx.git
 | cd u-boot-xlnx
 | git checkout 20a6cdd301941b97961c9c5425b5fbb771321aac

and by properly patching the sources:

.. host::

 | cd ~/Documents
 | git clone git://git.yoctoproject.org/meta-xilinx.git
 | cd meta-xilinx/
 | git checkout cb7329a596a5ab2d1392c1962f9975eeef8e4576
 | cd ..
 | patch -p1 -d u-boot-xlnx/ < meta-xilinx/recipes-bsp/u-boot/u-boot-xlnx/*

Suppose you modified something and you want to recompile the sources to test your patches, well,
you need a cross-toolchain (see :ref:`manual_compilation_label` Section). If you are not working
with the virtual machine, the most comfortable way to get the toolchain is to ask *Bitbake* for it:

.. host::

 | bitbake meta-toolchain

When *Bitbake* finishes, you will find an install script under directory:

.. host::

 path/to/build/tmp/deploy/sdk/

Install the script, and you will get under the installation directory a script to source to get your
environment almost in place for compiling. The name of the script is:

.. host::

 | environment-setup-armv7a-vfp-neon-poky-linux-gnueabi

Anyway, the environment is not quite right for compiling the bootloader and the Linux kernel, you need
to unset a few variables:

.. host::

 | unset CFLAGS CPPFLAGS CXXFLAGS LDFLAGS

Inside the virtual machine, the toolchain is already installed under:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/toolchain

In the very same directory there is a file, **environment-nofs**, that you can source that takes care of the
environment for you when you want to compile the bootloader or the kernel

.. host::

 | source /home/@user@/architech_sdk/architech/@board-alias@/toolchain/environment-nofs


Ok, now you a have working environment to compile *u-boot*, just do:

.. host::

 | cd ~/Documents/u-boot-xlnx/
 | make mrproper
 | make zynq_zed_config
 | make [-j parallelism factor] all

if you omit *-j* parameter, *make* will run one task after the other, if you specify it *make* will parallelize
the tasks execution while respecting the dependencies between them.
Generally, you will place a value for *-j* parameter corresponding to the double of your processor's cores number,
for example, on a quad core machine you will place *-j 8*.

Once the build process is complete, you will find **u-boot** file in your sources directory, that's your binary.
However, *u-boot* file alone is not able to boot the board, you are going to need a **First Stage Bootloader** and
a **Bitstream** to make the board properly boot.

