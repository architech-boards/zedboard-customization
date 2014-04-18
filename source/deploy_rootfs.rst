.. warning::

 The following instruction will make you overwrite your SD card content, it will be lost forever!
 If you have important data on it, make sure you do a backup of your data on the SD card before
 catching up with the next steps.

Create two partitions on the SD card you mean to use to boot the board. The first
one has to be a *FAT16* (name it **boot**), 64MB will be more than enough. Create the second
partition as an *EXT2* (name it **rootfs**), make it big enough to fill the free space on the
disk size.

Run this command:

.. host::

 | mkdir -p /home/@user@/Documents/@board-alias@

to create the directory that will be used to save a few files you need to download from the
Internet:


* `Download file BOOT.BIN <_static/BOOT.BIN>`_

* `Download devicetree.dtb <_static/devicetree.dtb>`_

* `Download file uEnv.txt <_static/uEnv.txt>`_

Now, we assume that the first partition of the SD card gets mounted (in your SDK virtual machine)
under:

.. host::

 | /media/boot

while the second partition gets mounted under:

.. host::

 | /media/rootfs

.. warning::

 If that's not the case for your configuration, please find out which are the proper mounting points
 for those two partitions on your system and replace them in the following instructions.

Ok then, we can finally deploy bootloader and kernel on the first partition of the SD card:

.. host::

 | cp /home/@user@/Documents/@board-alias@/BOOT.BIN /media/boot/
 | cp /home/@user@/Documents/@board-alias@/uEnv.txt /media/boot/
 | cp /home/@user@/Documents/@board-alias@/devicetree.dtb /media/boot/
 | cp /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/deploy/images/@machine-name@/uImage /media/boot/

and the root file system on the second partition of the SD card:

.. host::

 | sudo rm -rf /media/rootfs/* 
 | sudo tar @quickstart-image-tar-options@ /home/@user@/architech_sdk/architech/@board-alias@/yocto/build/tmp/deploy/images/@machine-name@/@quickstart-image@-@machine-name@.@quickstart-image-extension@ -C /media/rootfs/

.. important::

 sudo password is **@user-password@**

Make sure everything has been written on the SD card:

.. host::

 | sync

and unmount the SD card from your system.

