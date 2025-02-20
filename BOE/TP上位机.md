# TP上位机

## 模组卡控百分比

模组卡控自动计算方式：

- 上限：百分比×(目标值+step)
- 下限：百分比×(目标值-step)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131750174.jpg)

## 寄存器读写

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131753475.png)

- 点击寄存器可以打开寄存器窗口
- 已经加载固件使用Xdata、未加载固件使用Prog、DRW两种情况都可以
- 在蓝色框处输入寄存器地址可以直接跳转，可通过下方 Read Reg读取寄存器数据，也可以直接对相应寄存器位置输入数据
- 每个数据格为1字节大小（2个16进制字符）

