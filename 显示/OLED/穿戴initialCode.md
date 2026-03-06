# 穿戴initialCode（3312_GVO2.59）

## 分辨率

### 客户信息

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303164742195.png)

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303165241844.png)

### Code

- 480\*466改成400\*672

- 修改列source（480->400）

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303155457170.png)

	```txt
	// source=01(dec),R01=09(hex)
	0000 1001(bin)
	// source=13,R01=69
	0110 1001
	// source=17,R01=89
	1000 1001
	// source=18,R01=91
	1001 0001
	// source=20,R01=A1
	1010 0001
	
	18(10010)—>13(1101)
	左高位右低位
	```

- 修改行gate（466->672）

	```
	// source=13,gate=466
	R01 69		//Source number
	R02 D2		//Total H number
	R61 01		//AFC_BASE_NUM
	R62 E1
	
	69(011010 01)
	D2(1101 0010)
	R01+R02：gate=466(dec)(01 1101 0010)
			
	// source=13,gate=672
	R01 6A		//Source number
	R02 A0		//Total H number
	R61 02		//AFC_BASE_NUM
	R62 AF
	
	6A(011010 10)
	A0(1010 0000)
	R01+R02：gate=672(dec)(10 1010 0000)		
	```

## Frame rate/HS/Porch

### 客户信息

-  Frame rate：60Hz

- Hs：1/60/(672+16)≈24.2us

- AOD：AOD默认15Hz，客户有需要再提供修改

- Blank（VBP+VHP）:16

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303165016368.png)

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303171440700.png)

### Code

- VBP：8，VHP：8改成VBP：10，VFP：6

	- Normal场景Porch设定

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303180453058.png)

	- AOD（idle）场景Porch设定

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303180610764.png)

	- 额外VBP

		![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303181457035.png)

	```
	//R59 03
	0000 0011
	
	// BP：8，VHP：8
	R5B 08		//VFP Normal
	R5C 08		//VBP Normal
	R72 08		//VFP Idle
	R73 08		//VBP Idle
	R13 08			
	
	// VBP：10，VFP：6
	R5B 06		//VFP Normal
	R5C 0A		//VBP Normal
	R72 06		//VFP Idle
	R73 0A		//VBP Idle
	R13 0A		
	```

- Hs：34.5改成Hs：24.2

	- T1A：normal Hs osc计数

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303182137627.png)

	- T1A：AOD Hs osc计数

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303184949514.png)

	Hs的取值影响T1A的设定，T1A = Hs \* Freq

	```
	// Hs：34.5
	R59 03		//Normal line time
	R5A 3C		
	R70 03		//Idle line time
	R71 3C		
	33C(0X) = 828 ≈ 24.2 * 24 
	
	// Hs：24.2
	R59 02		//Normal line time
	R5A 44		
	R70 02		//Idle line time
	R71 44	
	244(0x) = 580 ≈ 24.2 * 24 
	```
	
- AOD：30HZ改成15HZ

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303185604445.png)
	$$
	AFC\_BASE\_NUM\_B=
	V 
	total
	​
	 ×(
	AOD 频率/
	Normal 频率
	​
	 −1)−1
	$$
	

	```
	// AOD 30HZ
	R61 02		//AFC_BASE_NUM
	R62 AF	
	2AF(0x) = 688*(60/30-1)-1 = 687
	
	// AOD 15HZ
	R61 08		//AFC_BASE_NUM
	R62 0F		
	80F(0x) = 688*(60/15-1)-1 = 2063
	```


## OSC

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303211112508.png)

分辨率\*帧率要大于OSC频率，如该项目400\*672\*60=16 128 000，所以OSC不能降规到16M

## 客户规定输出电压（VGLR/VGHR/VREFN/ELVDD/ELVSS）

### 客户信息

- VGLR：-6V
- VGHR：6V
- VREFN：-2.4V
- ELVDD：3.5V（这里设置的是AOD内电电压，AOD外电电压后面也要设置）
- ELVSS：-3.5V（这里设置的是AOD内电电压，AOD外电电压后面也要设置）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304153212900.png)

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304153631683.png)

### Code

- **VGHR**

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304154908192.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304161113236.png)

```
// VGHR1 Normal 6，AOD 6
R5E 0F		// VCL_RT_B,VCL_SEL_B
R5F 0F		// HBM mode VREFN

// VGHR1 Normal 6，AOD 6
R5E 14		// VCL_RT_B,VCL_SEL_B
R5F 14		// HBM mode VREFN

R5E:Normal VGHR
0F=0C(5.2)+3(F-C)*0.1=5.5
14=0C(5.2)+8()*0.1=6
R5F:AOD VGHR
```

- **VREFN**

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304161938168.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304162013500.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304162047671.png)

```
// 2.5 3.5 2.5
R21 AE		
R2D AE		
R32 C2		

// 2.4 2.4 2.4
R21 AC		
R2D AC		
R32 AC

//AE
1 0101110
//2E
101110

2.5：0.6(08)+38(2E-08)*0.05=2.5
2.4：0.6(08)+36(2C-08)*0.05=2.4
3.5：0.6(08)+58(2C-08)*0.05=3.5
```

## 内部电压

### 电压卡控（RT尽量设小，省功耗）

- **VCL**

	VCL小于等于VREFN和ELVSS即可，因为这两路电由VCL LDO生成

	RT=0时，不代表VCL为-3.5V，要看VCI实际电压，由-VCI决定，穿戴VCI客户一般提供两中3.3V和2.8V
	档位越低越好，0档位即可满足

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304184625608.png)

- **VGH&AVDD**

  VGHR1要求6V，VGHR由VGH LDO生成，VGH要大于等于VGHR

  先看VGH RT=0是否可满足，VGH RT=0时，VGH=AVDD，VGH由AVDD charge pump生成
  AVDD需大于等于6，AVDD RT=0时刚好可满足（VCI=3.3V，3.3+3.3 —climp—>6V）

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304185036299.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304185852189.png)

- **VGL**

	VGL需要小于等于VGLR，因为VGLR由VGL经由LDO生成
	VCI为3.3V时，RT=1可满足需求（-3.3V-3.3V —climp—> -6V）
	为2.8V时RT1无法满足

	![image-20260304190107482](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304190107581.png)

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304184151842.png)

### Code

- **VCL**

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304191411597.png)

	```
	// VCL HBM RT=2
	R11 18		// ELVDD for IDLE mode
	//18，VCL RT 10=2
	0001 1000
	
	// VCL HBM RT=0
	R11 10		// ELVDD for IDLE mode
	//10，VCL RT 00=0
	0001 0000
	```

- **VGH**

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304193925583.png)

	![image-20260304192116318](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304192116403.png)

	```
	//	VGH Nor Power_6.4 RT_3,AOD Power_6.4 RT_3
	R06 0C		// VGLR_B
	R08 03		// VGL_RT_B
	R0E 0C		// Nor mode VREFN
	R0F 03		// Idle mode VREFN
	// 0C：VGH_SEL_A=4+12(C)*0.2=6.4
	
	//	VGH Nor Power_6 RT_0,AOD Power_6 RT_0
	R06 0A		// VGLR_B
	R08 00		// VGL_RT_B
	R0E 0A		// Nor mode VREFN
	R0F 00		// Idle mode VREFN
	// 0A：VGH_SEL_A=4+10(A)*0.2=6
	```

## AOD 内电ELVDD/ELVSS默认输出

### 计算表格

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304202822429.png)

> 修改表格参数，左侧Code会联动改变，可通过观察左侧改变项，判断该项含义

- ELVDD  output

	AVDD为6，ELVDD为3.3，不相同无法复用AVDD，选0

- ELVSS  output

	ELVSS为-3.3，VCL已配置为-VCI，穿戴VCI客户一般提供两中3.3V和2.8V，该项目为3.3V，故ELVSS可以复用VCL，选1

- ELVDDPWR_B（ELVDD源头，为了省功耗）
	内电提供电压要大于外电ELVDD，因为由内电LDO后输出
	
	对称电压 ELVDD 3.3V时，可以用VCI 3.3V 提供，复用VCI
	
	当ELVDD ELVSS非对称电压时 eg：4.6V -2.4V，VCI小于外电ELVDD，需要选0，否则VCI外供4.6V ELVDD进AOD后可能显示闪屏异常，电压倒灌
	
	因为ELVDD为3.3，选1

> 因和initial code table配置一致，故无改动，该项在细节CMD文档中不存在，需查Register Table

## Swire

### 客户信息

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304215459122.png)

### 计算表格

- 客户要求不同模式VDD设成一致，ELVDD：3.5V，ELVSS：-3.5V

- 根据客户提供对应PMIC的Datasheet，ELVDD 3.5V需打32个pulse，ELVSS -3.5V需打49个pulse

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304220039700.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304215747891.png)

### Code

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304220853229.png)

```
//ELVDD：32pulse，ELVSS：pulse
RC9 31		//Normal ELVSS
R96 20		//Normal ELVDD     
RAB 31		// HBM ELVSS
RA7 20		// idle ELVDD         
RA9 31		// idle ELVSS

//31(0x)=49(dec),20(0x)=32(dec)
```

## QA

### Q1：改分辨率时，修改gate数时为什么会同步改动R61、R62？

​	因为R61、R62是AFC_BASE_NUM_B，要参考Vtotal，所以改gate会影响到，详见Q5。

### Q2：穿戴是因为扫描频率低，所以VBP和VFP自己定就好吗？看到客户没有提供source timing没法计数，一般设定如何？

​	如果客户未给出，自定即可，VBP和VFP都要是偶数，建议VBP大于VFP，对建立扫描更好。

### Q3：R13额外VBP是什么，为什么默认follow VBP设定值？

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305143249668.png)

在模式切换时，如果swire电压不一样，需要加大VBP，如果跨压不大则可以跟VBF保持一致（不加大VBF）。
swire通知PMIC需要时间，通过加大VBP的方式，一般增加1/2帧或1/4帧，具体时间设定要为GOA clk数的倍数（比如GOA 2CLK，则为2clk时间倍数）。
如果切模式后ELVDD、ELVSS跨压大，VBP时间不够，可能会闪一下

### Q4：什么是T1A，为什么Hs的改动影响T1A的设定？

-  **T1A 是什么**
	 T1A 是 DDIC 内部定义的 **Line-time（行时间）计数器**。它代表显示屏扫描一行（Horizontal Line）所占据的系统时钟周期总数。
- **T1A 的作用**
	- **定义行周期**：作为 IC 内部的时序基准，决定了每一行扫描的物理持续时间。
	- **同步触发**：当计数器达到 T1A 设定值时，触发内部 Hsync 信号，控制 Source 输出切换和 Gate 移位。

- **为什么是 T1A = Time \* Freq**

	- **单位转换**：寄存器只能存储“次数”，无法直接存储“秒数”。

	- **物理计数**：内部时钟（如 24MHz）每秒跳动 24×106 次。要确定一行的长度，必须计算在这段 *Hs* 时间内，时钟总共跳动了多少次。

	- **计算逻辑**：频率（次/秒）乘以时间（秒）得出总次数。*Hs* 时间越长，计数器需要数到的数值（T1A）就越大。

### Q5：什么是AFC_BASE_NUM_B？

- **AFC_BASE_NUM_B 是什么** ？

	**AFC_BASE_NUM_B** 是在 **Idle Mode (AOD)** 下用于降低帧频的 **自动帧率控制（AFC）基础计数值**。它决定了为了达到目标低频率，需要在原始帧周期中额外插入多少个基准单位（H-sync 行）。

- **AFC_BASE_NUM_B 作用**

	**降低功耗**，通过在帧与帧之间插入额外的空闲时间，降低屏幕刷新率，从而减少 IC 的活动功耗。

- **为什么公式是**
	$$
	AFC\_BASE\_NUM\_B=
	V 
	total
	​
	 ×(
	AOD 频率/
	Normal 频率
	​
	 −1)−1
	$$
	这个公式的核心逻辑是计算 “需要额外补偿的倍数”。

	- AOD 频率Normal 频率：计算当前帧周期相对于标准帧周期需要放大的 总倍数。例如从 60Hz 降到 15Hz，倍数就是 4。

	- **第一个 “-1”**：减去原始的那 1 帧，得到需要 **额外增加** 的总长度倍数（即 3 帧的长度）。
	- **乘以Vtotal ：**将“帧倍数”转换为具体的 **行数（Line）**。因为在 V-mode 下，AFC 是以“行”为单位进行插入的。

	- **第二个 “-1”**：通常在硬件逻辑中，计数器是从 0 开始计数或者存在一个基础的偏移量（Offset），因此在设定寄存器值时需要减去 1 个基准单位。

### Q6：计算表格中未被使用的CGOUT如CGOUT_L2、CGOUT_L4、CGOUT_L6等，是要初始化成VGH吗？

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303200150704.png)

是的

### Q7：关于OSC选择，为什么计算公式不\*8(1/3压缩)或\*24?

OSC要考虑数据传输过程，即MIPI阶段数据量，以B为单位，因为1/3压缩所以数据量为分辨率\*帧率和OSC做对比，如果16M符合可以考虑降规

### Q8：关于电压，DDIC要求VCI提供3.3V，VDDIO提供1.8V，VCL RT=0时，VCL=-VCI为-3.3V，若客户ELVSS需要-3.4V，VCL RT=0能满足需求嘛？

**TODO实际量测**

### Q9：为什么不需要考虑HBM模式的AVDD和VGH?

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304192232632.png)

设计上复用Normal，没法设置，也不需要设置

### Q10：T1A=Hs\*osc频率，但Hs存在小数，T1A是否取整？

分辨率：400*672，Porch=16，Vtotal=688，频率60Hz，osc 24MHz，Hs≈24.2

- 不考虑过多小数：T1A=Hs*osc=580.8=580
- 不考虑过多小数，向上取整：580.8=581
- 考虑小数，向上取整：1/60/688*24≈581.39=582

实际量测时符合spec即可，默认是不考虑小数即580

### Q11：为什么CMD文档中很多存在多组相同地址寄存器，并且部分寄存器查不到？

- OLED CMD 分组，参考每段code的RFE值，例如RFE e0

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304204504657.png)

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304204534125.png)

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304204620891.png)

- 如果查找CMD文档对应组的CMD List仍无对应改动寄存器，则该寄存器被隐藏，需查register table

### Q12：如何确认项目AOD模式的ELVDD和ELVSS是内部供电还是外部供电？

​	绝大部分项目AOD ELVDD和ELVSS都是用外电，内电没调gamma或特别亮时会把IC拉跨进AOD就黑屏了（IC带载能力不够）
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305145718387.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305145817365.png)

### Q13：如何理解客户这个电压规格？

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304213531290.png)

### Q14：swire VCI pluse是什么意思，怎么配置？

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304224111232.png)

### Q15：若OSC有两个规格：31M和22M，使用31M二分频和22M不分频，综合看哪种功耗低？

在满足规格的情况下，osc频率越低越好，定好osc频率后，在满足规格的情况下分频越多越好

### Q16：从客户提供的什么文档，可知项目使用的通信协议是MIPI或是其他

### Q17：Vtotal为什么要是ECK周期(4)的整数倍，即如果客户给定Vtotal不是4的整数倍，要加大Porch

OLED ECK和GCK在Porch区是不停的，一直在打

如果Vtotal行数不是ECK周期的倍数，则后面打的GOA ECK不是完整的周期，即使不是ECK周期倍数可能也没问题，但从时序上将配成ECK周期倍数较好

ECK周期一般为4，计算方式是38.4+58.4/24.2(Hs)=4

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305102353138.png)
由下图可见EOA的ECK一个周期等于ECK1完整的低电平+完整的高电平（红色绿色框时间相同）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305103757941.png)

ECK周期一般等同于GCK周期或为GCK周期的整数倍，所以是ECK周期倍数也等同于GCK周期倍数

### Q18：RT=0，VCI=3.3V，确认AVDD正常模式是否被climp到6V，还是能给到6.6V

**待实验**

### Q19：关于VGH、VGHR、VGL、VGLR设置的考量

这四路电压都可以分别设置，常规情况下，VGH和VGHR保持一致，VGL和VGLR保持一致，为了省功耗

### Q20：电压设定的RT档位，是越低越好吗？

不是，RT档位在能符合电压需求的情况下，越接近目标电压越好，提高LDO效率省功耗

### Q21：pump CLK 是什么？如何设定？

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305135408034.png)

- pump CLK H-Freq/16：行频的1/16
- pump CLK 设置的过大功耗高，设置的小驱动力不够导致闪频等显异
- 同时要兼顾EMI影响，该组参数一般保持不变，为量产经验参数，无干扰、视效问题

### Q22：如何通过code确认当前是MIPI还是SPI通信









