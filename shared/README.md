pi-OS for ENEE447
---------
pi-OS is based on open-source project Circle(https://github.com/rsta2/circle).
The assembly in this distribution is ARM32. It fits Raspberry Pi 2 and 3.
Circle has implemented some basic functions for a OS, such as hardware configuration and I/O driver. It  contains many ARM assembly codes, for configuring interuption, memory, mailbox. These codes are hard to "design", since the documentation for low-level programming on BCM283* chip is very limited. Many codes in Circle are imported from Linux kernel, where BroadCom put their chip driver. 

pi-OS is implemented in ARM32 assembly, C, C++. When this program is running, it first does some configuration, then logger function can be uesd. We can print information on the screen(through HDMI cable). Its running routine begins from _start, after some configuration steps it goes into main, where we can begin our OS implementation. 


Building
--------

For Raspberry Pi 2/3 you need a toolchain with Cortex-A7/-A53 support. [This one](https://github.com/raspberrypi/tools/tree/master/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf) should work for all of these.

First edit the file *Rules.mk* and set the Raspberry Pi version (*RASPPI*, 1, 2 or 3) and the *PREFIX* of your toolchain commands. Alternatively you can create a *Config.mk* file (which is ignored by git) and set the Raspberry Pi version and the *PREFIX* variable to the prefix of your compiler like this (don't forget the dash at the end):

`RASPPI = 3`  
`PREFIX = arm-none-eabi-`

The following table gives support for selecting the right *RASPPI* value:

| RASPPI | Target      | Models                   | Optimized for |
| ------ | ----------- | ------------------------ | ------------- |
|      1 | kernel.img  | A, B, A+, B+, Zero, (CM) | ARM1176JZF-S  |
|      2 | kernel7.img | 2, 3                     | ARMv7-A       |
|      3 | kernel7.img | 3                        | Cortex-A53    |

For a binary distribution you should do one build with *RASPPI = 1* and one with *RASPPI = 2* and include the created files *kernel.img* and *kernel7.img*.

Then go to the build root of Circle and do:

`./makeall clean`  
`./makeall`

The ready build *kernel.img* file should be in top directory. 




Installation
------------

Copy the Raspberry Pi firmware (from boot/ directory, do *make* there to get them) files along with the kernel.img (from sample/ subdirectory) to a SD(HC) card with FAT file system. Put the SD(HC) card into the Raspberry Pi. So all you need is three files, bootcode.bin, start.elf and kernel7.img. The first two files are from BroadCom and they're not open-source. kernel7.img is your OS image. Connect the Pi to a monitor with HDMI. Power on, and you can see some information on the screen.

Note that the file *kernel.img* has been renamed to *kernel7.img* for the Raspberry Pi 2/3.

Directories
-----------

* include: The common header files, most class headers are in the include/circle/ subdirectory.
* lib: The Circle class implementation and support files (other libraries are in subdirectories of lib/).

