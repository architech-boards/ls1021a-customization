Installation
============

Architech's Yocto based SDK is built on top of **@ubuntu-version@**, hence all the scripts
provided are proven to work on such a system.


If you wish to use another distribution/version you might need to change some script
option and/or modify the scripts yourself, remember that you won't get any support in
doing so.

Install a clone of the virtual machine inside your native machine
-----------------------------------------------------------------

To install the same tools you get inside the virtual machine on your native machine
you need to download and run a system wide installation script:

.. host::

 | git clone -b 2.4.0 https://github.com/architech-boards/machine_installer.git
 | cd machine_installer
 | ./machine_install -g -p

where *-g* option asks the script to install and configure a few graphic customization,
while *-p* option asks the script to install the required packages on the machine.
If you want to install the toolchain on a machine not equal to @ubuntu-version@ then
you may want to read the script, install the required packages by hand, and run it without
options. You might need to recompile the Qt application used to render the splashscreen.

At the end of the installation process, you will get the same tools installed within 
the virtual machine, that is, all the tools necessary to work with Architech's boards.

Install just one board
----------------------

If you don't want to install the tools for all the boards, you can install just the subset
of tools related to @board@:

1) Install the following packages, it can require for a while:

.. host::

 | sudo apt-get update
 | sudo apt-get --yes --force-yes install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm vim curl u-boot-tools libqtwebkit4 qt4-dev-tools texi2html subversion apache2 autoconf vim-common uuid-dev iasl default-jre libncurses5-dev > /dev/null

2) Install repo tool, if you already have it go to the next step

.. host::

 | mkdir -p ~/bin
 | sudo apt-get install curl
 | curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
 | chmod a+x ~/bin/repo

3) Make sure directory *~/bin* is included in your *PATH* variable by printing its content

.. host::

 | echo $PATH

4) If *~/bin* directory is not included, add this line to your *~/.bashrc*

.. host::

 | export PATH="$PATH:${HOME}/bin"

5) Install and setup git:

.. host::

  | sudo apt-get install git-core
  | git config --global user.name "Architech User"
  | git config --global user.email "your@mail.org"
  | git config --global color.ui "auto"

6) Finally install the board sdk:

.. host::

 | mkdir @board@
 | cd @board@
 | git clone -b @yocto-version@ https://github.com/architech-boards/@board-splashscreen@.git
 | mv @board-splashscreen@ splashscreen
 | cd splashscreen
 | ./run_install

before build an image with bitbake open the file */your/path/@board@/yocto/build/conf/local.conf* and edit these variables:

.. host::

  | DL_DIR = "/home/downloads"
  | SSTATE_DIR = "/home/sstate-cache"

and change them in:

.. host::

  | DL_DIR ?= "${TOPDIR}/downloads"
  | SSTATE_DIR ?= "${TOPDIR}/sstate-cache"

