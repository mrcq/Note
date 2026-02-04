## code调试实践

## 准备工作

- 找一份同IC code作为初版code，确认项目的分辨率、帧率、longV/longH、3Power电压、VGHO/VGLO电压、VGMP/VGMN电压

- 初版code如下ICNL9916-02_BOE6467_720X1560_120hz_Vmode_1450us_V07_20230803

	```shell
	#=========================================================
	# System board initial
	#=========================================================
	# DISPOFF
	R28
	delay 20
	mipi.video.disable
	delay 20
	
	#==============================================================================
	# Setting data lanes and data rate
	# Ex. mipi.dsi [lane 1/2/3/4] [data rate Mbps/lane] [nonburst-pulse/nonburst-event/nonburst/burst]/EOT
	#==============================================================================
	mipi.dsi 4 1160 burst//120Hz
	delay 100
	#==============================================================================
	# Setting data lanes and data rate
	# Ex. mipi.dsi.datatype [Generic/DCS]
	#==============================================================================
	mipi.dsi.datatype DCS
	//mipi.dsi.datatype Generic
	#====================================================================================
	# Set LCM Info.
	# Ex. mipi.video [V_line] [H_pixel] [Frame rate] [VBP] [VFP] [HBP] [HFP] [VSA] [HSA]
	# Ex. mipi.video.pixelformat [16bpp/18bpp/18bpp-loosely/24bpp/vesa-dsc/16YCbCr]
	# Ex. mipi.video.interval [Blanking/LP]
	#====================================================================================
	//Hmode
	mipi.video 1560 720 120 32 350 48 48 4 4 //for 120Hz V mode
	
	mipi.video.pixelformat 24bpp
	mipi.video.interval Blanking
	#=========================================================
	# Select Low Power mode
	# Ex. mipi.lpmode.on
	# Ex. mipi.lpmode.datarate [Mbps]
	#========================================================= 
	mipi.lpmode.on
	mipi.lpmode.datarate 8
	#====================================================================================
	# Set LCM Hardware Reset.
	# Ex. HWRESET [pulse width msec] [gpio number] [IOVCC/VDDIO]
	#====================================================================================
	HWRESET 10 1 IOVCC
	
	#=========================================================
	# initial code start
	#=========================================================
	RF0 5A 59
	RF1 A5 A6
	//RF0 B4 4B
	
	//OSC SETTING
	RD4 31 // OSCTCON_ADJ = 001
	//RD5 01 7C // Auto Trim
	
	//TCON
	RC1 10 20 5E 5E 04 30 30 04 18 06 12 70 31 31 07 11 84 4C 00 93 
	//RC2 00 05 01 10 00 00 80// TE
	RC2 00// TE
	//SRCCON
	RC4 04 33 B8 40 00 BC 00 E0 00 00 00 00 00 00 00 00 00 04 E0 20 F0 00 00 00 00 00 00 00 00 00 00 00
	//HSCAN
	RC5 03 24 96 C8 3E 00 07 02 12 0C 00 32 3F 08 01 3F 08 21 D6 00 00 00 01 14 38 7F 00 00 00 00 00 00
	//HSCAN
	RC6 5E 1F 15 2B 2B 28 3F 03 16 16 00 01 40 00 98 98 60 80 0C 00 00 00 00 00 00 00 00 00 00 00 00 00
	
	///GSP
	RB2 64 88 10 10 95 00 0D 82 88 52 37 52 37 00 00 00 00 79 79 65 4A 00 00 00 00 55 55 10 20 11 00 00
	
	//GCK GOE-1.2us
	RB3 F7 09 01 09 89 00 00 00 4B 00 00 00 65 4A 0C 00 00 03 00 00 0C 00 00 03 00 00 3F FF FF FF 00 00
	RB3 F7 09 01 09 89 00 7F 00 4B 00 00 00 65 4A 0C 00 00 03 00 00 0C 00 00 03 00 00 3F FF FF FF 00 00// clk EQ max
	
	
	
	//GG_MUX
	RB4 F7 09 01 09 89 64 00 00 65 00 00 00 65 4A 0C 00 00 03 00 00 0C 00 00 03 00 00 3F FF FF FF 00 00
	//updata-518
	RB5 12 16 09 16 0A 26 26 91 A2 33 44 00 26 00 55 3C 02 08 20 30 00 12 20 12 00 00 00 00 00 00 00 00
	//CGOUTL
	RB6 00 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 32 C1 02 02 02 02 02 02 FF FF FC 00 00 00 3C 00 00 00
	//CGOUTR
	RB7 00 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 32 C1 02 02 02 02 02 02 FF FF FC 00 00 00 3C 00 00 00
	
	
	//back scan
	//R36 03
	//RB6 02 32 F3 00 1B 19 17 15 13 11 0F 0D 07 05 00 C1 00 00 00 00 00 00 7F FF 00 00 00 00 00 00 00 00
	//RB7 02 32 F3 00 1A 18 16 14 12 10 0E 0C 06 04 00 C1 00 00 00 00 00 00 7F FF 00 00 00 00 00 00 00 00
	
	//GG_MUX
	RB8 3F FF FF FF 00 00 3F FF FF FF 00 00 00 00 00
	
	//RB9 00 15 55 55 55 00 00 15 55 55 55 00 00 0C 00 00 03 00 00 0C 00 00 03 00 00 00 00 00 00 00 00 00
	RB9 00 15 55 55 55 00 00 15 55 55 55 00 00 0C 00 00 00 00 00 0C 00 00 00 00 00 00 00 00 00 00 00 00
	RBB 01 02 03 0A 04 13 14 40 
	
	RBC 00 00 00 00 04 00 FF F0 0B 33 50 5B 33 33 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00//VCOM end
	
	//GAS
	RBD A1 A2 52 2E 00 8F
	//OTP VGH
	//VDDD 1.27V
	RBE 0C 88 43 38 33 00 00 38 00 B2 AF B2 AF 00 00 33 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	//POWERTPS
	RBF 0C 19 0C 19 00 11 04 18 50  //9.22
	//VGHO/VGLO=+/-14V
	RCA 00 40 00 19 46 94 41 8F 44 44 50 50 5A 5A 64 64 32 32 11 00 01 01 0A 06 22 00 05 00 00 5D 5D 04
	//VGH 15V VGL-15V 
	RFB 54 54
	
	
	//VCOM SETTING-1.7V
	//R89 1C CE 23 11 00 90 90 11
	
	//Gamma VGMP5.3V &VGMN -5.7V_one gamma 0606
	RC7 76 54 32 22 23 45 67 76 10 76 54 32 22 23 45 67 76 10 31 00 01 FF FF 40 FC 24 41 00 00 00 00 00
	R80 FF FA F2 EB E5 E0 DC D8 D4 C9 C0 B2 A5 9A 90 87 7F 7E 74 69 5D 50 40 2D 23 18 15 11 0E 0C 09 05
	R81 FF FA F2 EB E5 E0 DC D8 D4 C9 C0 B2 A5 9A 90 87 7F 7E 74 69 5D 50 40 2D 23 18 15 11 0E 0C 09 05
	R82 FF FA F2 EB E5 E0 DC D8 D4 C9 C0 B2 A5 9A 90 87 7F 7E 74 69 5D 50 40 2D 23 18 15 11 0E 0C 09 05
	R83 01 00 00 00 00 00 00 02 00 02 00 02 00
	R84 E0 B5 D0 5B B1 AD 57 BB C0 E0 B5 D0 5B B1 AD 57 BB C0 E0 B5 D0 5B B1 AD 57 BB C0
	RC8 46 00 49 40 
	
	//bist code
	RC0 40 93 00 1F
	
	//PASSWORD OFF
	//RF1 5A 59
	//RF0 A5 A6   
	
	R35 00 00
	// SLPOT + DISPON
	R11 00 00
	delay 120
	R29 00 00
	delay 10
	R6D 02 00
	
	//// Sleep In + Display Off
	//R6D 25          
	//R28 00 00
	//delay 10
	//R10 00 00
	//delay 120
	
	#=========================================================
	# Set first display image
	#=========================================================
	//image.fromfile "D:\Pictures\image.bmp"
	
	#=========================================================
	# Enter Video Mode
	#=========================================================
	mipi.video.enable
	```

- 实践项目信息

	- 分辨率：720×1612
	- 90hz longV
	- 3Power：±6V 1.8V（VSN/VSP/IOVCC)
	- VGHO/VGLO：±14V
	- VGMP：5.36V、VGMN：-5.7V

## 调整分辨率和帧率

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820164148699.png)

- ICNL9916_INIT_Check_List推荐90hz longV RC1参数如下

	```shell
	Long V 90Hz        ：RC1 10 20 20 2C 04 30 30 04
	//Long V 90Hz      : VBP/VFP/HBP/HFP/VSA/HSA: 32 300 48 48 4 4
	```

	修改 RC1寄存器的VBP/VFP/HBP/HFP/VSA/HSA相关参数

	```shell
	RC1 10 20 5E 5E 04 30 30 04 18 06 12 70 31 31 07 11 84 4C 00 93 //旧
	RC1 [10 20 20 2C 04 30 30 04] 18 06 12 70 31 31 07 11 84 4C 00 93 //新
	```

- 修改RC1寄存器的分辨率

  1612 == 64C(0x)

  ```shell
  RC1 10 20 20 2C 04 30 30 04 18 06 12 70 31 31 07 11 84 4C 00 93 //旧
  RC1 10 20 20 2C 04 30 30 04 [4C] 0[6] 12 70 [3]1 31 07 11 84 4C 00 93 //新
  ```

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820164357828.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820164428253.png)

- 调整MIPI 分辨率、帧率、速率

  ```shell
  # Ex. mipi.video [V_line] [H_pixel] [Frame rate] [VBP] [VFP] [HBP] [HFP] [VSA] [HSA]
  mipi.video 1560 720 120 32 350 48 48 4 4 //旧
  mipi.video 1612 720 90 32 300 48 48 4 4 //新
  
  # Ex. mipi.dsi [lane 1/2/3/4] [data rate Mbps/lane] [nonburst-pulse/nonburst-event/nonburst/burst]/EOT
  mipi.dsi 4 1160 burst //旧
  mipi.dsi 4 875 burst //新
  ```

  MIPI速率可通过Check_List中工具计算，大于计算出的MIPI速率，上调至可点亮即可

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820171230218.png)

## 调整为正扫

LCD扫描一般分正向扫面和反向扫描，分别针对正装和倒装结构（如下）：
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250820172753368.png)

根据Check_List，正扫时: R36=00; RC1-10th = 0x ; (确保 GS_EOR=0;BGR_EOR=0;SS_EOR=0)

```shell
RC1 10 20 20 2C 04 30 30 04 4C 06 12 70 31 31 07 11 84 4C 00 93 //旧
RC1 10 20 20 2C 04 30 30 04 4C [0]6 12 70 31 31 07 11 84 4C 00 93 //新
```

R36在code中未初始化，默认00（可下 R36 #1 指令读取确认）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821093315795.png)

## 确认TPS时间

TPS是指Touch工作的时间片，其它称呼包括”坑”，Touch Term等

将TE pin通过指令配成TPS输出，确认当前TP时间是否足够，如不足则加大VFP（longV）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821111356025.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821111432499.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821111746425.png)

根据对应IC要求（有spec表格），9916 90hz longV TPS大于1.6ms即可，当前符合要求

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821111950131.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821112507385.png)

## 确认OSC

将TE pin通过指令（RC2 66）配成OSC输出，根据Check_List 9916 60/90hz trim 50M，128分频
$$
fout=50MHz/128=0.390625MHz=390.625kHz
$$
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821113046846.png)

当前输出OSC符合预期

## 确认line time（1H）

将TE pin通过指令（RC2 22）配成Hsync输出，量测1H时间，方便后面进行调整，经测量1H时间为5.73微秒

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250822095203019.png)

## 设置电压

### 3Power

通过设置OP1，调整3Power电压

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821132601542.png)

### VGLO/VGHO

调整VGLO和VGHO倍率选择 ，目标VGHO/VGLO：±14V
2h：6V\*2+1.8V=13.8V
3h：6V\*2+6V=18V（满足需求）
4h：6V\*2+6V+1.8V=19.8V（没有必要，徒增功耗）

```shell
RBC 00 00 00 00 04 00 FF F0 0B 33 50 5B 33 33 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 //旧
RBC 00 00 00 00 04 00 FF F0 0B [33] 50 5B 33 33 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 //新
```

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821132752314.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821141805938.png)

### VGL/VGH

调整VGL和VGH电压选择 ，步进0.1V

```shell
RFB 54 54 //旧
RFB 4A 4A //新
```

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821143228206.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821145154782.png)

### VGMP/VGMN

调整VGMP和VGMN电压选择 ，VGMP：5.36V、VGMN：-5.7V，步进0.01V

```shell
RC7 76 54 32 22 23 45 67 76 10 76 54 32 22 23 45 67 76 10 31 00 01 FF FF 40 FC 24 41 00 00 00 00 00 //旧
RC7 76 54 32 22 23 45 67 76 10 76 54 32 22 23 45 67 76 10 31 00 01 FF FF 40 [00 22] 4[3] 00 02 00 00 00 //新
```

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821145624387.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821145739865.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821151908348.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821151938780.png)

### 参考（9911C）

| **ITEM**            | Power Domain（default） | CTRL Registers                                | Description                                                 |
| ------------------- | ----------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| VGH/VGHO  (pump)    | 7V~19.6V（12V）         | VGH_RT[2:0]、VGH_S[6:0]                       | VGH_RT:倍率选择 ，VGH_S:电压选择 ，步进0.1V                 |
| VGL  /VGLO(pump)    | -5V~-15.5V（-12V）      | VGL_RT[2:0]、VGL_S[6:0]                       | VGL_RT:倍率选择 ，VGL_S:电压选择，步进0.1V                  |
| VCL  (LDO)          | -3.4V~-4.9V（-3.8V）    | VCL_S[3:0]                                    | VCL_S:电压选择，步进0.1V                                    |
| VDDD  (LDO)         | 1V~1.35V（1.3V）        | VDDDL_S[2:0]、VDDDR_S[2:0]、VDDD_SLPIN_S[2:0] | VDDDL_S、VDDDR_S  and VDDD_SLPIN_S：VDDD 电压选择，步进0.1V |
| VGMP  (LDO)         | 3V~6V（4.5V）           | VGMP_S[7:0]                                   | VGMP_S:VGMP电压选择  ,步进0.02V                             |
| VGMN  (LDO)         | -3~-6V（-4.5V）         | VGMN_S[7:0]                                   | VGMN_S:VGMN电压选择,步进0.02V                               |
| VCOM  (LDO)         | 0V~-3.705V（-1.005V）   | VCOM_FWS[8:0]/VCOM_BWS[8:0]                   | VCOM_FWS正扫电压选择，VCOM_BWS反扫电压选择，步进0.015V      |
| VSP(Power  input)   | 3.4V~6.5V               | 调整点屏治具                                  | 输入电压                                                    |
| VSN(Power  input)   | -3.4V~-6.5V             | 调整点屏治具                                  | 输入电压                                                    |
| IOVCC(Power  input) | 1.65V~1.95V             | 调整点屏治具                                  | 输入电压                                                    |

## GOA

### CGOUT MAPPING

根据客户提供的OLB表格配置CHOUT MAPPING

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821152617567.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821152723849.png)

- 未用到的CGOUT可以配置成HIZ：0x82

	```shell
	//CGOUTL
	RB6 00 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 32 C1 02 02 02 02 02 02 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB6 [82] 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 [82 82 82 82 82 82 82 82] FF FF FC 00 00 00 3C 00 00 00 //新
	//CGOUTR
	RB7 00 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 32 C1 02 02 02 02 02 02 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB7 [82] 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 [82 82 82 82 82 82 82 82] FF FF FC 00 00 00 3C 00 00 00 //新
	```

- 设置CGOUT TP工作时状态（GOUTL_TPSxx），根据客户给的Display Timing图中Blanking区波形决定（一般为VGL/VGH），该项目只有STV0在Blanking时为VGH，其余使能cgout均为VGL

	```shell
	//CGOUTL
	RB6 82 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB6 82 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //新
	//CGOUTR
	RB7 82 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB7 82 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //新
	```

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821164010808.png)

- 设置CGOUT功能

	- STV0：特殊信号，每帧结尾时扫描一次，并与下一帧开始时间隔1H，可使能为GCL

		![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821172916259.png)

	- VGL：设置成VGL即可

	- STV1-4：根据波形，配置成GSP1~GSP4
	
	- dsd
	
	```shell
	//CGOUTL
	RB6 82 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB6 82 00 F3 00 14 16 18 1A 0C 0E 10 12 04 06 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //新
	//CGOUTR
	RB7 82 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //旧
	RB7 82 00 F3 00 15 17 19 1B 0D 0F 11 13 05 07 82 82 82 82 82 82 82 82 FF FF FC 00 00 00 3C 00 00 00 //新
	```
	
- 寄存器参考

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821153403657.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821155944464.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821160005099.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20250821160036869.png)
