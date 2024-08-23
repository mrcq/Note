# arm程序编译以及执行

## arm程序编译

sudo apt install gcc-arm-linux-gnueabihf

sudo arm-linux-gnueabihf-gcc  -static hello.c -o hello

## arm程序执行

qemu-arm hello

## arm程序反汇编

arm-linux-gnueabihf-objdump
