.. _qt_framework_label:

Qt cross-toolchain
==================

The Qt Framework used by this SDK is composed of libraries for your host machine and your target.
To compile the libraries for *x86* you only need your distribution toolchain, while to compile the
libraries for @board@ board you need the proper cross-toolchain (see Chapter :ref:`manual_compilation_label`
for further information on how to get it).

First of all you need to compile the cross-toolchain with Yocto: 

.. host::

 | bitbake meta-toolchain-qte

The recipe builds **poky-glibc-i686-meta-toolchain-qte-armv7a-vfp-neon-toolchain-qte-1.7.1.sh** installation script.
You should find the installation script in */home/architech/architech_sdk/architech/zedboard/yocto/build/tmp/deploy/sdk*.
The cross-toolchain allows to compile a Qt embedded 4.8.5 application.

To install the toolchain run the following commands:

.. host::

 | sudo ./poky-glibc-i686-meta-toolchain-qte-armv7a-vfp-neon-toolchain-qte-1.7.1.sh

The installation script will ask to select an installation path.

.. host::
 | sudo chown -R architech:architech ~/path/to/toolchain/installed

Before to run Qt creator you must set the environment variables:

.. host::

 | source /opt/poky/1.7.1/environment-setup-armv7a-vfp-neon-poky-linux-gnueabi
 | source /opt/poky/1.7.1/sysroots/i686-pokysdk-linux/environment-setup.d/qtopia.sh

.. note::

 | qtopia.sh is used to allow the compilation for the **qt4e-demo-image**
