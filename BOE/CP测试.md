# CP测试

## 准备

- 特殊版本TP上位机
- FPC上GPIO1接口
- 串口工具
- 对应测试项TP CP固件

## 过程

1. 从TP AE处获得CP测试用TP上位机和CP固件

2. 和BOE确认FPC Layout，确认GPIO1位置（具体GPIO端口看CP固件配置）

3. 点亮显示和TP，USB串口工具一端接地一端接GPIO（下TP固件后没数据为正常现象）

4. 调整串口波特率为115200，查看串口循环打印的TP log

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131523515.png)