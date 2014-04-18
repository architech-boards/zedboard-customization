Root FS
=======

The final root file system will be packaged as a *.tar.gz* file that, at the
end of the build process, *Bitbake* will let you find it under directory:

.. host::

 | /path/to/yocto/build/tmp/deploy/images/@machine-name@/

this means that within the SDK the actual path of the directory is:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/deploy/images/@machine-name@/

To deploy the root file system, you are going to need an SD card with two
partitions on it.

The first partition must be formatted as **FAT16**, its size must be sufficient
to contain all the following files (64MB are more than enough):

1. **UBOOT.BIN**, read directly by the processor at boot, containing the *first stage bootloader*, the  *bitstream* (optional), and *u-boot*

2. **uEnv.txt**, the bootscript with customizations

3. **uImage**, the Linux kernel 

4. **devicetree.dtb**, the device tree binary file

To have a better understanding of those components and how to boot the board please refer 
to :ref:`boot_label` Section.

The second partition, our root file system partition, can be formatted as **EXT2**.

We assume that the second partition of the SD card gets mounted (in your SDK virtual machine)
under:

.. host::

 | /media/rootfs

.. warning::

 If that's not the case for your configuration, please find out what is the proper mounting point
 for such a partition on your system and replace it in the following instructions.

Untar the file corresponding to your root file system inside such a partition:

.. host::

 | sudo rm -rf /media/rootfs/* 
 | sudo tar @quickstart-image-tar-options@ /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/deploy/images/@machine-name@/<image>-@machine-name@.@quickstart-image-extension@ -C /media/rootfs/

where *<image>* is the name of the recipe you used to build your root file system.
For example, if you built @quickstart-image@ with :ref:`Bitbake <bitbake_label>`,
then the name of the tarball will be *@quickstart-image@-@machine-name@.@quickstart-image-extension@*

.. important::

 sudo password is **@user-password@**

