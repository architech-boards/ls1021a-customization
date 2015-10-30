.. warning::

 The following instruction will make you overwrite your SD card content, it will be lost forever!
 If you have important data on it, make sure you do a backup of your data on the SD card before
 catching up with the next steps.

1. Format the SD card in **FAT32**, for example using **gparted**

2. Copy the following files:

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

