# code调试实践\_ICNL9916_BOE6.56_720×1612

## DISPOFF

```shell
R28
delay 20
mipi.video.disable
delay 20
```

## MIPI

```
mipi.dsi 4 875 burst//90Hz
delay 100
mipi.dsi.datatype DCS

mipi.video 1612 720 90 32 300 48 48 4 4 //for 90Hz V mode

mipi.video.pixelformat 24bpp
mipi.video.interval Blanking

mipi.lpmode.on
mipi.lpmode.datarate 8
```

## Hardware Reset

```shell
HWRESET 10 1 IOVCC
```

## PASSWORD

```shell
RF0 5A 59
RF1 A5 A6 
```

## OSC

```shell
RD4 31
```

## TCON

```shell
RC1 10 20 00 2C 04 30 30 04 4C 06 22 70 31 24 07 91 84 4C 00 93
```

- VFP：300 == 12C(0x)

- VBP：32 == 20(0x)

- VSA/HSA：4 == 4(0x)

- HBP/HFP：48 == 30(0x)

- NL(V_line)：1612 == 64C(0x)

- 正扫时: RC1-10th = 0x ; (确保 GS_EOR=0;BGR_EOR=0;SS_EOR=0)。

- BLK_I2O：开机插黑，2帧（详见客户提供Power on时序）

- BLK_O2I：关机插黑，2帧（详见客户提供Power off时序）

- Parameter12，默认70

- Parameter13，默认3X

- Gon1、Gon2（Goff1、Goff2）：1和2自由分配，根据客户Power on/off时序要求，例1帧或至少1帧（总和）

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820143634825.png)

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820143747165.png)

- CHECK_LIST：建议RC1_14th_bit4=0，LVB_HS_OPT=0

- Parameter14，TCON_SYNC_SEL 默认1

- Parameter15，默认07

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820110547182.png)
