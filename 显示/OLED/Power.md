# POWER

## 电压描述（datasheet）

### Power Input

| 符号 (Symbol) | I/O 类型 | 描述 (Description)                                           |
| ------------- | -------- | ------------------------------------------------------------ |
| **VCIC**      | 电源输入 | - DDIC 内部 DCDC 转换器的供电电源。 - 可由“外部 PMIC”供电。 - 电压范围：2.7V ~ 3.6V。 - VCIC, VCIA, VCIR 的输入电压应与系统 VCI 保持一致。 |
| **VCIA**      | 电源输入 | - DDIC 内部模拟电路的供电电源。 - 可由“外部 PMIC”供电。 - 电压范围：2.7V ~ 3.6V。 - VCIC, VCIA, VCIR 的输入电压应与系统 VCI 保持一致。 |
| **VCIR**      | 电源输入 | - DDIC 内部 LDO 的供电电源。 - 可由“外部 PMIC”供电。 - 电压范围：2.7V ~ 3.6V。 - VCIC, VCIA, VCIR 的输入电压应与系统 VCI 保持一致。 |
| **VDDI**      | 电源输入 | - I/O 接口供电电源。 - 电压范围：1.65V ~ 3.3V。              |
| **VCC**       | 电源输入 | - DVDD 稳压器的供电电源。 - 在 COF 封装情况下，将 VCC 连接至 FPC 上的 VDDI。 |
| **VSSC**      | 电源地   | - DCDC 转换器接地。VSSC = 0V。                               |
| **VSSA**      | 电源地   | - 模拟系统接地。VSSA = 0V。 - 在 COF 封装情况下，将 VSSA 连接至 FPC 上的 VSS 以防止噪声。 |
| **HVSSAM**    | 电源地   | - MIPI 接口接地。 - 在 COF 封装情况下，将 HVSSAM 连接至 FPC 上的系统 VSS 以防止噪声。 |
| **VSSI**      | 电源地   | - 除 MIPI 接口以外的 I/O 接地。 - 在 COF 封装情况下，将 VSSI 连接至 FPC 上的系统 VSS 以防止噪声。 |
| **VSSD**      | 电源地   | - 内部数字系统接地。VSSD = 0V。 - 在 COF 封装情况下，将 VSSD 连接至 FPC 上的系统 VSS 以防止噪声。 |
| **MTP_PWR**   | 电源输入 | - MTP 编程（烧录）电源引脚（典型值 6.0V）。                  |

###  DC/DC

| 符号 (Symbol)                 | I/O 类型     | 描述 (Description)                                           |
| ----------------------------- | ------------ | ------------------------------------------------------------ |
| **AVDD**                      | **CP 输出**  | - 由升压电路 1 从 VCIC 产生的输出电压。 - 需连接电容以稳定电压。 |
| **VCL**                       | **CP 输出**  | - 由升压电路 4 从 VCIC 产生的输出电压。 - 需连接电容以稳定电压。 |
| **VGH**                       | **CP 输出**  | - 由升压电路 2 产生的输出电压。 - 需连接电容以稳定电压。     |
| **VGL**                       | **CP 输出**  | - 由升压电路 3 产生的输出电压。 - 需连接电容以稳定电压。     |
| **C11P, C11N** **C12P, C12N** | **模拟输出** | - 用于产生 AVDD 的升压电路的电容连接引脚。 - 根据需要连接电容；不使用时，请保持引脚悬空。 |
| **C41P, C41N** **C42P, C42N** | **模拟输出** | - 用于产生 VCL 的升压电路的电容连接引脚。 - 根据需要连接电容。 |
| **C21P, C21N**                | **模拟输出** | - 用于产生 VGH 的升压电路的电容连接引脚。 - 根据需要连接电容。 |
| **C31P, C31N**                | **模拟输出** | - 用于产生 VGL 的升压电路的电容连接引脚。 - 根据需要连接电容。 |
| **VGHR1/VGHR2**               | **LDO 输出** | - 从 VGH 产生的输出电压。用于面板电压的 LDO 输出。 - 需连接电容以稳定电压。 - 不使用时，请保持引脚悬空。 |
| **VGLR1/VGLR2**               | **LDO 输出** | - 从 VGL 产生的输出电压。用于面板电压的 LDO 输出。 - 需连接电容以稳定电压。 - 不使用时，请保持引脚悬空。 |
| **VGMP**                      | **LDO 输出** | - 从 AVDD 产生的输出电压。用于正伽马（Gamma）高电压发生器的 LDO 输出。 |
| **VGSP**                      | **LDO 输出** | - 从 AVDD 产生的输出电压。用于正伽马（Gamma）低电压发生器的 LDO 输出。 |
| **VREF**                      | **LDO 输出** | - 内部参考电压的 LDO 输出。 - 需连接电容以稳定电压。         |
| **DVDD**                      | **LDO 输出** | - 逻辑系统电源的 LDO 输出。 - 需连接电容以稳定电压。         |
| **VREFP5**                    | **LDO 输出** | - 用于 OLED 面板显示的 LDO 输出。 - 需连接电容以稳定电压。   |
| **VREFN5/** **VREFN5_2**      | **LDO 输出** | - 用于 OLED 面板显示的 LDO 输出。 - 需连接电容以稳定电压。   |
| **BVP3D** **(I_ELVDD)**       | **LDO 输出** | - 从 AVDD 产生的正输出电压。用于 OLED 面板显示的 LDO 输出。 - 需连接电容以稳定电压。不使用时，请保持引脚悬空。 |
| **BVN3D** **(I_ELVSS)**       | **LDO 输出** | - 从 VCL 产生的负输出电压。用于 OLED 面板显示的 LDO 输出。 - 需连接电容以稳定电压。不使用时，请保持引脚悬空。 |
| **VREFX**                     | **LDO 输出** | - VREFN5 或 VREFP5 的输出引脚。                              |



## 电压生成方式

| 特性          | DCDC (电感式开关电源)                 | LDO (低压差线性稳压器)                         |
| ------------- | ------------------------------------- | ---------------------------------------------- |
| **效率**      | **高**（通常 >85%），适合大电流场景。 | **低**，压差越大效率越低，能量以热量形式损耗。 |
| **纹波/噪声** | **较大**，因为有高速开关切换。        | **极小**，输出非常平滑。                       |
| **主要用途**  | 产生正负高压（VSP/VSN）作为母电压。   | 产生像 **VGLR/VGHR** 这样的精密基准电压。      |

## Timing

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303225714353.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260303225955382.png)

## 电压生成关系

- VCI（模拟）和VDDIO（数字）是输入电压
- ELVSS、ELVDD在非AOD模式下是PMIC供电，AOD可以由DDIC供电
- AVDD是VCI charge pump生成
- VREFP是给NMOS用的，PMOS不用

## AVDD

C11和C12为两个升压电容，CB20为稳压电容

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304111447671.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304111546675.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304111609053.png)

- 2VCI
	- 充电：1，2闭合，a-b=VCI
	- 升压：1，2断开，6，7闭合，b=VCI，a-b=VCI，a=2VCI
- 2VCI+VDDI
	- 充电：1，2，4，5闭合，a-b=VCI，c-d=VDDI
	- 升压：1，2，4，5断开，6，8，9闭合，d=VCI，c-d=VDDI，c=b=VDDI+VCI，a=b+VCI=2VCI+VDDI
- 3VCI
	- 充电：1，2，4，5闭合，a-b=VCI，c-d=VCI
	- 升压：1，2，3，5断开，6，8，9闭合，d=VCI，c-d=VCI，c=b=VCI+VCI，a=b+VCI=3VCI

> **VSSD**  电源地  - 内部数字系统接地。VSSD = 0V。

## QA

### OP1如何关闭AVDD输入？DVDD和DVDDP为什么可以输入，不是VDDI LDO生成DVDD

### VREF和VREFP5、VREFN5区别是什么

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304111000286.png)

VREF是内部参考电压

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304111031991.png)

VREFN5是输出给Panel的

### BVP3D和BVN3D是什么

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304112748683.png)

AOD模式下ELVSS、ELVDD供电

### 如何切换模式，normal、AOD、HBM

- 进AOD
	- RFE 00
	- R39
- 退AOD
	- RFE 00
	- R38
- 进HBM
	- RFE 00
	- R66 02
- 退HBM
	- RFE 00
	- R66 00

## 穿戴IC一般是2power供电，如何关闭OP1的AVDD输入，以及DVDD和DVDDP

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260304140002692.png)

不用操作，一般都在代码里面写死了的,OP1一般没做电压部分的操作





