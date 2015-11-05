Root FS
=======

An SD card image provides the full system to boot with U-Boot and kernel. To flash an SD card image, run the following
commands (after have formatted it in FAT32):

.. host::

 | cp tmp/deploy/images/ls1021atwr/fsl-image-core-ls1021atwr.ext2.gz.u-boot /path/your/sd
 | cp tmp/deploy/images/ls1021atwr/uImage /path/your/sd
 | cp tmp/deploy/images/ls1021atwr/uImage-ls1021a-twr.dtb /path/your/sd

.. important::

 sudo password is **@user-password@**

Make sure everything has been written on the SD card:

.. host::

 | sync

and unmount the SD card from your system.

Extra information on how to customize the file system and the different packages available from Freescale, 
can be found on the `IMXLINUX: Embedded Linux for i.MX Applications Processors <http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=IMXLINUX&fsrch=1>`_
