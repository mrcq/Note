# TDDI参数计算

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250722151617408.png)

## 每一帧扫描的总时间：

60Hz：1frame = 1/60; 

90Hz：1frame = 1/90;  

### 每一行的时间计算：

LineTime(LV) = 1frame /(VBP+VFP+VACT)

LineTime(LH-EXT) = frame/(VBP+VFP+VACT)

> 外部LineTime时间

LineTime(LH-INT) = (1frame-TPS_total)/(Vbp+Vfp+Vact) ; 

> 内部LineTime时间，要扣除TP所用时间

### MIPI速度计算： 

lane_speed = Vtotal\*Htotal\*24\*framerate/lane_number; 

> 每个像素有红绿蓝三个子像素，每个子像素需要8个bit，3*8=24
>
> 单通道MIPI理论上限1.5G，IC上限1.2G

### OSC时钟周期：

Tosc = 1/OSC ; 

Ttcon = 2(4) * Tosc ;