U-boot
======

Get sources
-----------

The bootloader used by @board@ is **u-boot**. 
If you want to browse/modify the sources first you have to get them, if you already built @board@'s bootloader with *Bitbake*, then you already have them on your (virtual) disk:

*Bitbake* will place *u-boot* sources under:

.. host::

 | /path/to/yocto/build_ls1021atwr_release/tmp/work/ls1021atwr-fsl-linux-gnueabi/u-boot-ls1/2014.07-r0/git


this means that within the virtual machine you will find them under:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build_ls1021atwr_release/tmp/work/ls1021atwr-fsl-linux-gnueabi/u-boot-ls1/2014.07-r0/git


We suggest you to **don't work under Bitbake build directory**, you will pay a speed penalty
and you can have troubles syncronizing the all thing. Just copy the sources some place else
and do what you have to do.

