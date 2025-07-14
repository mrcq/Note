# arm程序编译以及执行

## arm程序编译

sudo apt install gcc-arm-linux-gnueabihf

sudo arm-linux-gnueabihf-gcc  -static hello.c -o hello

> ### **`-static`**
>
> - 目标系统缺少动态库或库版本不兼容。
> - 需要简化部署流程。
> - 对启动速度有较高要求，且文件体积和内存占用不是主要问题。

## arm程序执行

qemu-arm hello

## arm程序反汇编

arm-linux-gnueabihf-objdump
