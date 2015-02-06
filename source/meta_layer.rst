Meta Layer
==========

A Yocto/OpenEmbedded meta-layer is a directory that contains recipes, configuration files, patches, etc., all needed by
*Bitbake* to properly "see" and build a BSP, a distrubution, a (set of) package(s), whatever.
**@meta-layer@** is a meta-layer which defines the BSP for Xilinx devices, @board@ included. 
You can get it with *git*:

.. host::

 | git clone git://git.yoctoproject.org/meta-xilinx.git
 | cd meta-xilinx/
 | git checkout cb7329a596a5ab2d1392c1962f9975eeef8e4576

Please, refer to the *README* file contained inside the meta-layer directory.

The machine name corresponding to @board@ is **@machine-name@**.
