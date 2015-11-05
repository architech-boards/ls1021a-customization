
Bitbake
=======

**Bitbake** is the most important and powerful tool available inside Yocto/OpenEmbedded.
It takes as input configuration files and recipes and produces what it is asked for, that is,
it can build a package, the Linux kernel, the bootloader, an entire operating system from
scratch, etc.

A **recipe** (*.bb* file) is a collection of metadata used by BitBake to set **variables** or define
additional build-time **tasks**. By means of *variables*, a recipe can specify, for example,
where to get the sources, which build process to use, the license of the package, an so
on. There is a set of predefined *tasks* (the fetch task for example fetches the sources
from the network, from a repository or from the local machine, than the sources are cached
for later reuses) that executed one after the other get the job done, but a recipe can always
add custom ones or override/modify existing ones. The most fine-graned operation that Bitbake
can execute is, in fact, a single task.

Environment
-----------

To properly run Bitbake, the first thing you need to do is setup the shell environment.
Luckily, there is a script that takes care of it, all you need to do is:

.. host::

 | source /path/to/oe-init-build-env /path/to/build/directory

Inside the virtual machine, you can find *oe-init-build-env* script inside:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/poky

If you omit the build directory path, a directory named **build** will be created under your 
current working directory.

By default, with the SDK, the script is used like this:

.. host::

 | source /home/@user@/architech_sdk/architech/@board-alias@/yocto/poky/oe-init-build-env

Your current working directory changes to such a directory and you can customize configurations
files (that the environment script put in place for you when creating the directory), run Bitbake
to build whatever pops to your mind as well run hob.
If you specify a custom directory, the script will setup all you need inside that directory
and will change your current working directory to that specific directory.

.. important::

 The build directory contains all the caches, builds output, temporary files, log files, file system images... everything!

The default build directory for @board@ is located under:

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/build

and the splash screen has a facility (a button located under @board@'s page) that can take you
there with the right environment already in place so you are productive right away.

.. important::

 | If you don't use the default build directory you need setup the local.conf file. See the paragraph below.

Configuration files
-------------------

Configuration files are used by Bitbake to define variables value, preferences, etc..., there are
a lot of them. At the beginning you should just worry about two of them, both located under *conf*
directory inside your build directory, we are talking about **local.conf** and **bblayers.conf**.

*local.conf* contains your customizations for the build process, the most important variables you
should be interested about are: **MACHINE**, **DISTRO**, **BB_NUMBER_THREADS** and **PARALLEL_MAKE**.
*MACHINE* defines the target machine you want compile against. The proper value for @board@ is 
@machine-name@:

.. host::

 | MACHINE ??= "@machine-name@"

*DISTRO* let you choose which distribution to use to build the root file systems for the board. The
default distribution to use with the board is:

.. host::

 | DISTRO ?= "@distro-name@"

*BB_NUMBER_THREADS* and *PARALLEL_MAKE* can help you speed up the build process. *BB_NUMBER_THREADS*
is used to tell Bitbake how many tasks can be executed at the same time, while *PARALLEL_MAKE* contains
the **-j** option to give to *make* program when issued. Both *BB_NUMBER_THREADS* and *PARALLEL_MAKE*
are related to the number of processors of your (virtual) machine, and should be set with a number
that is two times the number of processors on your (virtual) machine. If for example, your (virtual)
machine has/sees four cores, then you should set those variables like this:

.. host::

 | BB_NUMBER_THREADS ?= "8"
 | PARALLEL_MAKE ?= "-j 8"

*bblayers.conf* is used to tell Bitbake which meta-layers to take into account when parsing/looking for
recipes, machine, distributions, configuration files, bbclasses, and so on. The most  important variable contained inside *bblayers.conf* is **BBLAYERS**, it's the variable where the actual meta-layers layout get specified.

.. host::

 | BBLAYERS ?= " \
 |   /home/architech/architech_sdk/architech/ls1021atwr/yocto/poky/meta \
 |   /home/architech/architech_sdk/architech/ls1021atwr/yocto/poky/meta-yocto \
 |   /home/architech/architech_sdk/architech/ls1021atwr/yocto/poky/meta-yocto-bsp \
 | "

You should add those lines to **BBLAYERS**

.. host::

 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-fsl-arm \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-fsl-networking \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-fsl-toolchain \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-virtualization \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-oe/meta-oe \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-oe/meta-networking \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-oe/meta-perl \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-oe/toolchain-layer \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-java \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-linaro/meta-linaro-toolchain \
 | /home/architech/architech_sdk/architech/ls1021atwr/yocto/meta-security \


All the variables value we just spoke about are taken care of by Architech installation scripts.

Command line
------------

With your shell setup with the proper environment and your configuration files customized according to your
board and your will, you are ready to use Bitbake.
The first suggestion is to run:

.. host::

 | bitbake -h

Bitbake will show you all the options it can be run with.
During normal activity you will need to simply run a command like:

.. host::

 | bitbake <recipe name>

for example:

.. host::

 | bitbake @quickstart-image@

Such a command will build bootloader, Linux kernel and a root file system.
*@quickstart-image@* tells Bitbake to execute whatever recipe

.. host::

 | /home/@user@/architech_sdk/architech/@board-alias@/yocto/poky/meta/recipes-extended/images/@quickstart-image@.bb

you just place the name of the recipe without the extension *.bb*.

Of course, there are times when you want more control over Bitbake, for example, you want to execute just one task
like recompiling the Linux kernel, no matter what. That action can be achieved with:

.. host::
    
 | bitbake -c compile -f virtual/kernel

where *-c compile* states the you want to execute the *do_compile* task and *-f* forces Bitbake
to execute the command even if it thinks that there are no modifications and hence there is no need to 
to execute the same command again.

Another useful option is *-e* which gets Bitbake to print the environment state for the command you ran.

The last option we want to introduce is *-D*, which can be in fact repeated more than once and asks Bitbake
to emit debug print. The amount of debug output you get depend on many times you repeated the option.

Of course, there are other options, but the ones introduced here should give you an head start.
