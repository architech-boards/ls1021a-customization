Root FS
=======

An SD card image provides the full system to boot with U-Boot and kernel. To flash an SD card image, run the following
command:

.. host::

 | sudo dd if=core-image-minimal-@board-alias@.sdcard of=/dev/sd<partition> bs=1M


.. important::

 sudo password is **@user-password@**

Make sure everything has been written on the SD card:

.. host::

 | sync

and unmount the SD card from your system.

Extra information on how to customize the file system and the different packages available from Freescale, 
can be found on the `IMXLINUX: Embedded Linux for i.MX Applications Processors <http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=IMXLINUX&fsrch=1>`_
