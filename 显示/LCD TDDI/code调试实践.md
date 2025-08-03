# code调试实践\_ICNL9916X_BOE6.78_720X1576

## DISPOFF

```shell
#=========================================================
# System board initial
#=========================================================
# DISPOFF
R28
delay 20
mipi.video.disable
delay 20
```

## MIPI

```shell
#==============================================================================

# Setting data lanes and data rate
# Ex. mipi.dsi [lane 1/2/3/4] [data rate Mbps/lane] [nonburst-pulse/nonburst-event/nonburst/burst]/EOT
#==============================================================================
mipi.dsi 4 1200 nonburst-event 

delay 100

#==============================================================================
# Setting data lanes and data rate
# Ex. mipi.dsi.datatype [Generic/DCS]
#==============================================================================
mipi.dsi.datatype DCS
```

1. MIPI lane在BOE邀请表中有说明![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250731152824475.png)

2. MIPI速率计算公式为：(HAS+HBP+HFP+HACT)\*(VSA+VBP+VFP+VACT)\*FrameRate\*24/Lane，如是常规分辨率，可参考默认参数

	> Long V 120Hz        ：RC1 10 20 20 4A 04 30 30 04
	>  //Long V 120Hz      : VBP/VFP/HBP/HFP/VSA/HSA: 32 330 48 48 4 4

## LCM Info

```shell
#====================================================================================
# Set LCM Info.
# Ex. mipi.video [V_line] [H_pixel] [Frame rate] [VBP] [VFP] [HBP] [HFP] [VSA] [HSA]
# Ex. mipi.video.pixelformat [16bpp/18bpp/18bpp-loosely/24bpp/vesa-dsc/16YCbCr]
# Ex. mipi.video.interval [Blanking/LP]
#====================================================================================

mipi.video 1576 720 120 32 300 48 48 4 4 //120HZ
//mipi.video 1600 720 90 32 936 48 48 4 4 //90HZ
//mipi.video 1600 720 60 32 2210 48 48 4 4 //60HZ
//mipi.video 1600 720 45 32 2850 48 48 4 4 //45HZ

mipi.video.pixelformat 24bpp
mipi.video.interval Blanking
```

## Low Power mode

```shell
#=========================================================                                
# Select Low Power mode                                
# Ex. mipi.lpmode.on                                
# Ex. mipi.lpmode.datarate [Mbps]                                
#=========================================================                                 
mipi.lpmode.on                                
mipi.lpmode.datarate 8     
```

## Hardware Reset

```shell
#====================================================================================
# Set LCM Hardware Reset.
# Ex. HWRESET [pulse width msec] [gpio number] [IOVCC/VDDIO]
#====================================================================================
HWRESET 10 0 IOVCC
```

## initial code

### Password

```shell
//=========================================================
//initial code start
//=========================================================
//PW
RF0 99 16 18
```

每个IC不同，checklist中有描述

> PassWord选择：
> 品牌：RF0 99 16 18  
> 返修：RF0 88 16 18 
> OTP：RF0 B4 4B 00

### BIST_mode

```shell
//RF0 B4 4B 00
RC0 40 93 FF FF FF 3F FF 00 FF 00 CC 58 01 23 45 67 89 AB FF FF
```

### TCON

```shell
//TCON
RC1 01 20 2C 04 30 30 04 28 06 22 40 35 05 07 11 84 4C 00 E3 00
```



## 确定panel驱动电压

需要先设定好IOVCC/VSP/VSN/VCOM/VGH/VGL的电压

## GOA

## VCOM

## GAMMA

## QA

1. DISPOFF每个IC都要下吗？9922C没看到有下；DISPOFF的作用是什么？

2. MIPI nonburst-pulse/nonburst-event/nonburst/burst如何选择？Generic/DCS如何选择？

3. LCM info VFP为什么是300而不是推荐的330？是有压缩用vesa-dsc无压缩用24bpp吗？什么时候用LP？

4. lpmode 默认8Mbps吗？还是客户给值？

5. Hardware Reset每个IC都是固定的吗？从哪确认不同IC的指令？

6. 9916 IC RF0 5A 59和RF1 A5 A6是做什么用的？

	> RF0 99 16
	>
	> RF0 5A 59
	> RF1 A5 A6

7. 

