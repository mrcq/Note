# PinMap

1. 穿戴IC，包含3306、3310、3311、3312，都是纯DDIC
2. IC pin分为ILB部分和OLB部分，先是ILB然后是OLB，ILB是fpc bonding即IC输入，OLB是IC bonding即IC输出

## 工作流程

1. 确认ILB和OLB部分IC的pin顺序正确（IC pin参考GDS的pad_location表)
2. 

## ILB（3312）

如下参考对应IC的datasheet

| No   | Name           | Description                                                  | Remark                                                       |
| ---- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | DUMMY_R1       | - These pins are used source dummy.<br/>- If not used, please keep these pins floating. | open                                                         |
| 2    | DUMMY_R1       | - These pins are used source dummy.<br/>- If not used, please keep these pins floating. | open                                                         |
| 3    | ANALOG_TEST[1] | - Test pin, must be left open.<br/>- ANALOG_TEST1 (AVDD --- VSSA)<br/>- ANALOG_TEST2 (VSSA --- VCL) | 测试pin，open                                                |
| 4    | VGLR_2         | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | OLB中如没用到，short165、166即可，不用拉出来                 |
| 5    | VGLR           | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | short 163、164，拉出来接到FPC的VGLR                          |
| 6    | VGHR           | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | short 161、162，拉出来接到FPC的VGHR                          |
| 7    | VGHR_2         | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | OLB中如没用到，short159、160即可，不用拉出来                 |
| 8    | VREFP5         | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization. | short 9，拉出来接到FPC的VREFP5                               |
| 9    | VREFP5         | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization. | 见8                                                          |
| 10   | VREFN5_2       | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization | 参考电压，看客户需求，可以不拉出，不用short                  |
| 11   | VREFN5_2       | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization | 见10                                                         |
| 12   | VREFN5         | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization. | short 13，拉出来接到FPC的VREFN5                              |
| 13   | VREFN5         | - LDO output used for OLED panel display.<br/>- Connect a capacitor for stabilization. | 见12                                                         |
| 14   | BVP3D          | - Positive output voltage generated from AVDD. LDO output used for OLED<br/>panel display. Connect a capacitor for stabilization. When not in use,please open this pin.<br/>- Connect a capacitor for stabilization. | short 15，open或接OVDD_INT                                   |
| 15   | BVP3D          | - Positive output voltage generated from AVDD. LDO output used for OLED<br/>panel display. Connect a capacitor for stabilization. When not in use,please open this pin.<br/>- Connect a capacitor for stabilization. | 见14                                                         |
| 16   | BVN3D          | - Negative output voltage generated from VCL. LDO output used for OLED<br/>panel display Connect a capacitor for stabilization. When not in use, please open this pin.<br/>- Connect a capacitor for stabilization. | short 17，open或接OVSS_INT                                   |
| 17   | BVN3D          | - Negative output voltage generated from VCL. LDO output used for OLED<br/>panel display Connect a capacitor for stabilization. When not in use, please open this pin.<br/>- Connect a capacitor for stabilization. | 见16                                                         |
| 18   | VCL            | - Output voltage from step-up circuit 4, generated from VCIC.<br/>- Connect a capacitor for stabilization. | short 19、142\~144，拉出来接到FPC的VCL                       |
| 19   | VCL            | - Output voltage from step-up circuit 4, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见18                                                         |
| 20   | AVDD           | - Output voltage from step-up circuit 1, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 显示source供电，short 21、135\~137，拉出来接到FPC的AVDD      |
| 21   | AVDD           | - Output voltage from step-up circuit 1, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见20                                                         |
| 22   | VREF           | - LDO output for internal reference voltage.<br/>- Connect capacitor for stabilization. | 拉出来接到FPC的VREF                                          |
| 23   | VGSP           | - Output voltage generated from AVDD. LDO output for positive gamma low voltage generator. | 拉出来接到FPC的VGSP                                          |
| 24   | VGMP           | - Output voltage generated from AVDD. LDO output for positive gamma high voltage generator. | 拉出来接到FPC的VGMP                                          |
| 25   | ANALOG_TEST[2] | - Test pin, must be left open.<br/>- ANALOG_TEST1 (AVDD --- VSSA)<br/>- ANALOG_TEST2 (VSSA --- VCL) | 测试pin，open                                                |
| 26   | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- **VCIC, VCIA, VCIR should be the same input voltage level to system VCI**. | 模拟供电，VCIC, VCIA, VCIR电压要相同，short 27\~29、126\~134，拉出来接到FPC的VCI |
| 27   | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 28   | VCIA           | - Power supply to analog power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIA= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 29   | VCIA           | - Power supply to analog power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIA= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 30   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 模拟地，short所有地 31\~35、87、92、97、102\~113，拉出来接到FPC的AGND |
| 31   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 32   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 33   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 34   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 35   | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 36   | VSSI           | - Ground for I/O except MIPI interface.<br/>- In case of COF, connect VSSI to system VSS on the FPC to prevent noise. | IO地，short44、50、56、62、79\~82，拉出来接到FPC的DGND       |
| 37   | OSC_AOD_IN     | - The oscillator input of self-clock function for AOD mode. (crystal oscillator=<br/>32.768kHz)<br/>- If not used, please connect to VSSI. | 接WATCH_OSC_IN或者VSSI，不用外部OSC就接到VSSI                |
| 38   | ERR            | - Output pin used to monitor display driver state and error status, If not used,<br/>please open this pin. | debug信号，看客户需要，可以open                              |
| 39   | TE1            | - Tearing effect output pin to synchronize MCU to frame writing, activated by S/W<br/>command.<br/>- When this pin is not activated, this pin is output low.<br/>- If not used, please open this pin. | 拉到TE1或open                                                |
| 40   | SWIRE          | - Swire protocol setting pin of Power IC, If not used, please open this pin. | 拉到swire或open                                              |
| 41   | TE             | - Tearing effect output pin to synchronize MCU to frame writing, activated by S/W<br/>command.<br/>- When this pin is not activated, this pin is output low.<br/>- If not used, please open this pin. | 拉到TE                                                       |
| 42   | RESX           | - This signal will reset the device and must be applied to properly initialize the chip.<br/>Signal is active low.<br/>- There is no internal pull high resistor for this pin. | 拉到RESX，low使能，内部没上拉，需要外部上拉                  |
| 43   | SDO            | - Serial output signal in SPI I/F. The data is output on the rising/falling edge<br/>of the SCL signal. If the host places the SDI line into high-impedance state during the read interval, the SDI and SDO can be tied together.<br/>- If not used, please open this pin. | Serial Data Out，SPI的SO，拉到SDO或open，如果主机在读数据时将SDI设置成高阻态，可以将SDI和SDO焊一起 |
| 44   | VSSI           | - Ground for I/O except MIPI interface.<br/>- In case of COF, connect VSSI to system VSS on the FPC to prevent noise. | 见36                                                         |
| 45   | SDI_RDX        | - SDI: Serial input signal in SPI I/F. The data is input on the rising edge of the<br/>SCL signal.<br/>- RDX: Reads strobe signal to write data when RDX is “Low” in 80-series<br/>MPU interface.<br/>- If not used, please open this pin. | SPI的输入SI或80系的读使能，接到SDI_RDX或open                 |
| 46   | DCX            | - Display data / command selection in 80-series MPU I/F and 4-wire SPI I/F.<br/>D/CX = ”0” : Command<br/>D/CX = ”1” : Display data or Parameter<br/>- If not used, please connect to VSSI. | SPI的控制线（数据/命令）或80系的RS（同DCX），接到DCX或open   |
| 47   | WRX_SCL        | - WRX: Writes strobe signal to write data when WRX is “Low” in 80-series<br/>MPU I/F.<br/>- SCL: A synchronous clock signal in SPI I/F.<br/>- If not used, please connect to VSSI. | SPI的CLK或80系的写使能，接到WRX_SCL或open                    |
| 48   | CSX            | - Chip select input pin (“Low” enable) in 80-series MPU I/F and SPI I/F.<br/>- If not used, please connect to VDDI. | SPI和80系的片选CS，接到CSX或open                             |
| 49   | D[0]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线，接D[0]，QSPI需要D[0]和D[1] |
| 50   | VSSI           | - Ground for I/O except MIPI interface.<br/>- In case of COF, connect VSSI to system VSS on the FPC to prevent noise. | 见36                                                         |
| 51   | D[1]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线，接D[1]，QSPI需要D[0]和D[1] |
| 52   | D[2]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 53   | D[3]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 54   | D[4]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 55   | D[5]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 56   | VSSI           | - Ground for I/O except MIPI interface.<br/>- In case of COF, connect VSSI to system VSS on the FPC to prevent noise. | 见36                                                         |
| 57   | D[6]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 58   | D[7]           | - 8-bit bi-directional data bus for 80-series MPU I/F and 8-bit input data bus<br/>for RGB I/F.<br/>- These pins are not used for SPI, MIPI, please open this pin. | SPI/MIPI open，80系或RGB接口的data线                         |
| 59   | TEST[1]        | - Test pin, must be left open.                               | open                                                         |
| 60   | EXTCLK         | - Test pin, must be left open.                               | open                                                         |
| 61   | TEST[2]        | - Test pin, must be left open.                               | open                                                         |
| 62   | VSSI           | - Ground for I/O except MIPI interface.<br/>- In case of COF, connect VSSI to system VSS on the FPC to prevent noise. | 见36                                                         |
| 63   | TEST[3]        | - Test pin, must be left open.                               | open                                                         |
| 64   | IM[1]          | - Interface type selection. The connections of IM[1:0] which not shown in table are invalid.<br />![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260302170920671.png) | 选择pin，接IM1                                               |
| 65   | IM[0]          | - Interface type selection. The connections of IM[1:0] which not shown in table are<br/>invalid. | 选择pin，接IM0                                               |
| 66   | MIPI_SEL       |                                                              | 没有HSSI2时，short DGND                                      |
| 67   | EXT_DVDD_EN    |                                                              | 2power时short DGND，3power拉高（常见2power）                 |
| 68   | PSWAP          | - Input pin to select HSSI_D0/D1 data lane sequence and polarity in high speed interface only.<br />- If not used, please connect to VSSI.<br />![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260302171515716.png) | 当 `PSWAP=0` 时，保持原始极性（如 D0+ 对应 D0+）。 当 `PSWAP=1` 时，翻转极性（如 D0+ 变为 D0-）。<br />open或者拉出来接到FPC的PSWAP |
| 69   | VDDI           | - Power supply to I/O.<br/>- VDDI= 1.65V ~ 3.3V.             | IO供电，short所有地 70\~74，拉出来接到FPC的VDDIO             |
| 70   | VDDI           | - Power supply to I/O.<br/>- VDDI= 1.65V ~ 3.3V.             | 见69                                                         |
| 71   | VDDI           | - Power supply to I/O.<br/>- VDDI= 1.65V ~ 3.3V.             | 见69                                                         |
| 72   | VCC            | - Power supply for DVDD regulator.<br/>- In case of COF, connect VCC to VDDI on the FPC. | 数字供电，short 73\~74，COF short VDDI                       |
| 73   | VCC            | - Power supply for DVDD regulator.<br/>- In case of COF, connect VCC to VDDI on the FPC. | 见72                                                         |
| 74   | VCC            | - Power supply for DVDD regulator.<br/>- In case of COF, connect VCC to VDDI on the FPC. | 见72                                                         |
| 75   | DVDD           | - LDO output for logic system power.<br/>- Connect a capacitor for stabilization. | 实际数字用电，需要外部电路接电容稳压，short76~78，拉出来接到FPC的DVDD |
| 76   | DVDD           | - LDO output for logic system power.<br/>- Connect a capacitor for stabilization. | 见75                                                         |
| 77   | DVDD           | - LDO output for logic system power.<br/>- Connect a capacitor for stabilization. | 见75                                                         |
| 78   | DVDD           | - LDO output for logic system power.<br/>- Connect a capacitor for stabilization. | 见75                                                         |
| 79   | VSSD           | - Ground for internal digital system. VSSD=0V.<br/>- In case of COF, connect VSSD to system VSS on the FPC to prevent noise. | 见36                                                         |
| 80   | VSSD           | - Ground for internal digital system. VSSD=0V.<br/>- In case of COF, connect VSSD to system VSS on the FPC to prevent noise. | 见36                                                         |
| 81   | VSSD           | - Ground for internal digital system. VSSD=0V.<br/>- In case of COF, connect VSSD to system VSS on the FPC to prevent noise. | 见36                                                         |
| 82   | VSSD           | - Ground for internal digital system. VSSD=0V.<br/>- In case of COF, connect VSSD to system VSS on the FPC to prevent noise. | 见36                                                         |
| 83   | HSSI2_D0_P     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 84   | HSSI2_D0_P     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 85   | HSSI2_D0_N     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 86   | HSSI2_D0_N     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 87   | HVSSAM         | - Ground for MIPI interface.<br/>- In case of COF, connect HVSSAM to system VSS on the FPC to prevent noise. | MIPI地，short到VSS                                           |
| 88   | HSSI2_CLK_P    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 89   | HSSI2_CLK_P    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 90   | HSSI2_CLK_N    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 91   | HSSI2_CLK_N    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 看客户需求，不需要则，short到HVSSAM                          |
| 92   | HVSSAM         | - Ground for MIPI interface.<br/>- In case of COF, connect HVSSAM to system VSS on the FPC to prevent noise. | 见87                                                         |
| 93   | HSSI1_CLK_P    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | short 94，接到MIPI1_CLK_P或open                              |
| 94   | HSSI1_CLK_P    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 见93                                                         |
| 95   | HSSI1_CLK_N    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | short 96，接到MIPI1_CLK_N或open                              |
| 96   | HSSI1_CLK_N    | - DSI CLK+/- differential clock signals.<br/>- HSSI_CLK_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_CLK_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 见95                                                         |
| 97   | HVSSAM         | - Ground for MIPI interface.<br/>- In case of COF, connect HVSSAM to system VSS on the FPC to prevent noise. | 见87                                                         |
| 98   | HSSI1_D0_P     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | short 99，接到HSSI1_D0_P或open                               |
| 99   | HSSI1_D0_P     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 见98                                                         |
| 100  | HSSI1_D0_N     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | short 101，接到HSSI1_D0_N或open                              |
| 101  | HSSI1_D0_N     | - DSI D0+/- differential clock signals.<br/>- HSSI_D0_P/N are differential signals. To be ensure the trace length is<br/>shortest so that COF and FPC resistance is less than 10 Ohm.<br/>- For MIPI I/F, please pull HSSI_D0_P/N to LP-00 or VSSI when enter deep<br/>standby mode.<br/>- If not used, please connect these pins to HVSSAM. | 见100                                                        |
| 102  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 103  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 104  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 105  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 106  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 107  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 108  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见30                                                         |
| 109  | VSSC           | - Ground for DC/DC converter. VSSC=0V.                       | DC/DC地，short到AGND                                         |
| 110  | VSSC           | - Ground for DC/DC converter. VSSC=0V.                       | 见109                                                        |
| 111  | VSSC           | - Ground for DC/DC converter. VSSC=0V.                       | 见109                                                        |
| 112  | VSSC           | - Ground for DC/DC converter. VSSC=0V.                       | 见109                                                        |
| 113  | VSSC           | - Ground for DC/DC converter. VSSC=0V.                       | 见109                                                        |
| 114  | C11P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | short 115\~116，连接到C11P，或open<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 115  | C11P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见114                                                        |
| 116  | C11P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见114                                                        |
| 117  | C11N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | short 118\~119，连接到C11N，或open<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 118  | C11N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见117                                                        |
| 119  | C11N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见117                                                        |
| 120  | C12P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | short 121\~122，连接到C12P，或open<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 121  | C12P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见120                                                        |
| 122  | C12P           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见120                                                        |
| 123  | C12N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | short 124\~125，连接到C12N，或open<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 124  | C12N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见123                                                        |
| 125  | C12N           | - Capacitor connection pins for the step-up circuit which generate AVDD.<br/>- Connect capacitor as requirement. When not in used, please open these pins. | 见123                                                        |
| 126  | VCIC           | - Power supply to DCDC power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIC= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 127  | VCIC           | - Power supply to DCDC power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIC= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 128  | VCIC           | - Power supply to DCDC power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIC= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 129  | VCIC           | - Power supply to DCDC power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIC= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 130  | VCIC           | - Power supply to DCDC power for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIC= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 131  | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 132  | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 133  | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 134  | VCIR           | - Power supply to LDO for DDIC use.<br/>- It can be supported by “external PMIC”.<br/>- VCIR= 2.7V ~ 3.6V.<br/>- VCIC, VCIA, VCIR should be the same input voltage level to system VCI. | 见26                                                         |
| 135  | AVDD           | - Output voltage from step-up circuit 1, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见20                                                         |
| 136  | AVDD           | - Output voltage from step-up circuit 1, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见20                                                         |
| 137  | AVDD           | - Output voltage from step-up circuit 1, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见20                                                         |
| 138  | C41P           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | short 139，连接到C41P<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 139  | C41P           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | 见138                                                        |
| 140  | C41N           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | short 140，连接到C41N<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 141  | C41N           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | 见140                                                        |
| 142  | VCL            | - Output voltage from step-up circuit 4, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见18                                                         |
| 143  | VCL            | - Output voltage from step-up circuit 4, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见18                                                         |
| 144  | VCL            | - Output voltage from step-up circuit 4, generated from VCIC.<br/>- Connect a capacitor for stabilization. | 见18                                                         |
| 145  | C42P           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | short 146，连接到C42P<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 146  | C42P           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | 见145                                                        |
| 147  | C42N           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | short 148，连接到C42N<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 148  | C42N           | - Capacitor connection pins for the step-up circuit which generate VCL.<br/>- Connect capacitor as requirement. | 见147                                                        |
| 149  | C21P           | - Capacitor connection pins for the step-up circuit which generate VGH.<br/>- Connect capacitor as requirement. | short 150，连接到C21P<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 150  | C21P           | - Capacitor connection pins for the step-up circuit which generate VGH.<br/>- Connect capacitor as requirement. | 见149                                                        |
| 151  | C21N           | - Capacitor connection pins for the step-up circuit which generate VGH.<br/>- Connect capacitor as requirement. | short 152，连接到C21N<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 152  | C21N           | - Capacitor connection pins for the step-up circuit which generate VGH.<br/>- Connect capacitor as requirement. | 见151                                                        |
| 153  | C31N           | - Capacitor connection pins for the step-up circuit which generate VGL.<br/>- Connect capacitor as requirement. | short 154，连接到C31N<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 154  | C31N           | - Capacitor connection pins for the step-up circuit which generate VGL.<br/>- Connect capacitor as requirement. | 见153                                                        |
| 155  | C31P           | - Capacitor connection pins for the step-up circuit which generate VGL.<br/>- Connect capacitor as requirement. | short 156，连接到C31P<br />因兼容友商命名可能不叫对应电容No，PN成对，且原理图上规格符合即可 |
| 156  | C31P           | - Capacitor connection pins for the step-up circuit which generate VGL.<br/>- Connect capacitor as requirement. | 见155                                                        |
| 157  | VGH            | - Output voltage from step-up circuit 2.<br/>- Connect a capacitor for stabilization. | short 158，连接到VGH                                         |
| 158  | VGH            | - Output voltage from step-up circuit 2.<br/>- Connect a capacitor for stabilization. | 见157                                                        |
| 159  | VGHR_2         | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见7                                                          |
| 160  | VGHR_2         | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见7                                                          |
| 161  | VGHR           | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见6                                                          |
| 162  | VGHR           | - Output voltage generated from VGH. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见6                                                          |
| 163  | VGLR           | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见5                                                          |
| 164  | VGLR           | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见5                                                          |
| 165  | VGLR_2         | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见4                                                          |
| 166  | VGLR_2         | - Output voltage generated from VGL. LDO output used for panel voltage.<br/>- Connect a capacitor for stabilization.<br/>- When not in use, please open this pin. | 见4                                                          |
| 167  | VGL            | - Output voltage from step-up circuit 3.<br/>- Connect a capacitor for stabilization. | short 168，连接到VGH                                         |
| 168  | VGL            | - Output voltage from step-up circuit 3.<br/>- Connect a capacitor for stabilization. | 见167                                                        |
| 169  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | short 170，连接到AGND                                        |
| 170  | VSSA           | - Ground for analog system. VSSA=0V.<br/>- In case of COF, connect VSSA to VSS on the FPC to prevent noise. | 见169                                                        |
| 171  | MTP_PWR        | - MTP programming power supply pin. (8.25V typical)<br/>- Must be left open or connected to VSSD in normal condition. | short 172\~174，连接到MTP                                    |
| 172  | MTP_PWR        | - MTP programming power supply pin. (8.25V typical)<br/>- Must be left open or connected to VSSD in normal condition. | 见171                                                        |
| 173  | MTP_PWR        | - MTP programming power supply pin. (8.25V typical)<br/>- Must be left open or connected to VSSD in normal condition. | 见171                                                        |
| 174  | MTP_PWR        | - MTP programming power supply pin. (8.25V typical)<br/>- Must be left open or connected to VSSD in normal condition. | 见171                                                        |
| 175  | DUMMY_R2       |                                                              |                                                              |
| 176  | DUMMY_R2       |                                                              |                                                              |

## PS

- **SPI**

	- **DCX （Data/Command）：** 这是标准 SPI 里没有的。它的作用是区分**这是数据还是命令**。
		- 高电平：表示 SDA 上传的是**显示数据**（比如像素颜色）。
		- 低电平：表示 SDA 上传的是**控制命令**（比如设置亮度、调整光标）。
	- **CSX （Chip Select）：** 标准 SPI 里的**CS（片选）**，低电平有效，告诉屏幕“我要跟你说话了”。
	- **SCL （Serial Clock）：** 标准 SPI 里的**CLK（时钟）**。
	- **SDA （Serial Data）：** 标准 SPI 里的**MOSI**（主出从进）。注意这里**没有 MISO**，因为屏幕通常只需要接收数据（比如显示图片），很少需要向主控发送大量数据。

	> - **4-wire 模式：**4 根线全用（DCX， CSX， SCL， SDA）。这是最常用的模式，因为 DCX 单独一根线，传输效率高。
	> - **3-wire 模式：** 在这种模式下，DCX 的功能被整合进了 SDA 的数据流中。通常是在发送每个字节之前，先用 1 位（bit）来表示是命令还是数据。这样虽然节省了一根线（少用了 DCX），但传输速度会稍慢一些。

- **80 系列并行接口（可忽略）**

	- 80 系列接口是一种类似内存总线的并行接口，通常包含：**CS（片选）、RS（寄存器选择，相当于 DCX）、RD（读使能）、WR（写使能）** 以及 8 位或 16 位并行数据线。

## Checklist

### C1：是否确认好分辨率

分辨率决定了source的连接方式，在画pinmap前一定要确认好实际分辨率

- 若客户panel上提供source pin数大于所需pin数，建议删减，降低功耗

- 若支持RGB数大于客户所需RGB数（eg：MUX6，提供454RGB，需要450RGB），要将部分source进行，pass through，通常选取一边而不是两边同时，从偶数标号开始

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306200735816.png)

- 390RGB和454RGB从source pin数为奇数，所以会dummy1或240

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306201023046.png)

### C2：IC OLB端VSR信号建议间隔分开，尤其是EOA/GOA信号

### C3：VSR1-10会重复，要检查是否重复使用，建议输出测试信号使用VSR11-16

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306201616383.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306201653981.png)

### C4：不需要和客户标注的VSR编号保持一致

### C5：两个IM信号要确认拉出，可通过配置这两个信号，支持所有数据传输协议

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306202306930.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260306202326123.png)

## QA

### Q1：I/O Type中 CP Output、Analog Output、LDO Output有什么区别

| 类型              | 产生方式     | 主要作用                | 输出质量 | 典型引脚       |
| ----------------- | ------------ | ----------------------- | -------- | -------------- |
| **CP Output**     | 电容开关升压 | 提供系统主功率 (大电流) | 粗糙     | `AVDD`, `VCL`  |
| **Analog Output** | 内部电路引出 | 连接飞跨电容，辅助升压  | N/A      | `C11P`, `C11N` |
| **LDO Output**    | 线性稳压     | 提供面板精密偏置电压    | 纯净     | `VGHR`, `VGLR` |

### Q2：VGLR_2、VGHR_2、VREFN5_2、HSSI2_D0_P，是否使用是看客户是否需求，但如果使用，有什么好处
这些看客户需求，根据协议来的，客户知道是否需要

### Q3：ERR这个监控PIN，是看客户是否有需求嘛？建议拉出？
ERR、TE、TE1作用都是一样的，可以debug用，TE1和ERR建议至少拉一个，但如果都不想拉也可以

### Q4：怎么判断是否需要客户拉出OSC_AOD_IN到WATCH_OSC_IN，如果FPC没WATCH_OSC_IN？
AOD时看是否提供外部OSC，若有则拉出

### Q5：VGSP、VGMP没有拉出来，取而代之是右侧加个测点？
客户如果pin紧张，不想拉则需要加测点，用以监测

### Q6：TE和TE1的区别是什么，TE1具体用来作什么调试的？（当前有引出TE）

TE TE1均可以用来debug，但是如果只拉一个TE时，debug时无法同时输出TE和所需的debug信号，因此建议外拉TE1

### Q7：ILB中，一些只short未拉出的pin，作用是什么？

如下图所示，就是TP_L_4和TP_L_3短接但未引出，这种是测Bonding用的，参考下面标注方法即可
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305220413096.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305220531716.png)

### Q8：OLB中需要留哪些测试点？

需要留两个GOA测点（任意未用到的VSR上下各一个），两个Source测点（最外侧，参考下图）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305220734119.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305220755358.png)

GOA测点一般客户都有要求拉到FPC上，拉出即可

OLB上还有一些客户要求的pin需要拉出，一般命名为XX_out，如下图

- Scan_out

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305221650992.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305221715012.png)

- EM_OUT

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305221806913.png)

### Q9：LIB下方169 VSSA为什么需要单独拉出到地，上方的不需要

因为这俩pin在IC内部是弱拉地，因此建议外接GND，以保证满足设计需求。

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305222439997.png)

如下这种内部short，并外接GND，所以不用单独拉到地
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305222713017.png)

### Q10：pin删减建议——DSWAP

- 1P1L
	- **在 1P1L 模式下：** 只有一对 Data 线（D0P/N）。因为只有一条通道，不存在“谁先谁后”或者“谁接错了谁”的问题。
	- **结论：** 在这种配置下，DDIC 内部的 Data Lane Swap 逻辑通常是失效或无意义的。
- 1P2L
	- 在 1-Port 2-Lane 配置中，MIPI 协议规定 **Lane 0 (D0)** 负责传输所有的低功耗指令（LP 模式，如读取寄存器、初始化设置）。
	- **如果接错：** 如果你的 SOC D0 接到了 IC 的 D1 引脚，但 DSWAP 没开（没接高），芯片会尝试从 D1 接收初始化代码。由于 D1 不具备 LP 接收功能（或协议不匹配），屏幕会**彻底无法初始化**，表现为黑屏，且读不到芯片 ID。
	- 但如果接线没问题，DSWAP 是接地，若空间不足可删减该pin

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260305223038380.png)