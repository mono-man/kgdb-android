# kgdb-android

Kernel patches to get KGDB working on the Nexus 6.

For background, please see associated blog post at http://www.contextis.com/resources/blog/kgdb-kernel-debugging-android

1. Download and build the stock Nexus 6 kernel (kernel/msm) using instructions from https://source.android.com/source/building.html
2. Download this directory structure into the root of your kernel source (kernel/msm/) including the .config file.
3. Re-build your kernel source.
4. Create your boot image, passing console arguments e.g. ```abootimg -u boot.img -k zImage-dtb -c 'console=ttyHSL0,115200,n8 kgdboc=ttyHSL0,115200 kgdbretry=4'```
5. Plug in your debug cable (see blog)
6. Boot your image e.g. ```fastboot boot boot.img```
7. Open a shell (adb shell), su to root, then type:


    echo -n g > /proc/sysrq-trigger


8. Hit enter
9. On your host machine fire up GDB:


    arm-eabi-gdb ./vmlinux
    (gdb) set remoteflow off
    (gdb) set remotebaud 115200
    (gdb) target remote /dev/ttyUSB0


You should hit the KGDB breakpoint and be able to continue, examine memory, etc.
