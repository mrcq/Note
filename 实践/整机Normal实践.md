# 整机Normal实践

[TOC]

## INFO

- 硬件设备：ICNL9916 麦博韦尔-TCL-civic德智欣_INX6.56,V60hz

- Git Branch：origin/master_ICNL9916

- Cneg归一化工具：ICNL9916_CNEG_Auto_Setting_V1.2_20230712.xlsx

- touch工具：touch_viewer_9916、chipone-touch-v1.2.5.apk

- Diff 调整系数生成工具：ICNL9916 全屏 Diff 调整系数生成工具.xlsx

- AutoCheckTool：AutoCheckTool_V3.7.2

<mark>Noraml 调屏的前提是 RX Mapping 调整和 Sensor 补偿</mark>

## 参数配置

<mark>以下参数在调屏前根据设备信息或客户需求进行确认和配置</mark>

### Config.h

- 唤醒方式

  ```c
  #define WAKE_UP_SRC                            WAKE_UP_SPI
  ```

- Firmware 版本号

  ```c
  #define FIRMWARE_VER                            LIB_CODE_VER|0x02
  ```

- V mode 显示帧率模式使能

  ```c
  #define  DDI_FR_MODE               FIXED_60HZ
  ```

- TP 均匀报点功能设置

  ```c
  #define TP_RATE_AVG                             ON //OFF
  ```

- 中断模式和相关设置

  ```c
  #define INT_MODE                                INT_MODE_LO
  #define INT_KEEP_TIME                           3000
  ```

### TouchCommon.h

- Panel Type
  
  ```c
  #define PANEL_INX_6_56          1  
  
  #ifndef PANEL_TYPE
      #define PANEL_TYPE          1 //15
  ```
  
### Panel_01.h

- 扫描方式
  
  ```c
  #define SCAN_MODE                       SCAN_V
  ```
  
- OSC_CLK
  
  ```c
  #define OSC_CLK                         64000000UL
  ```
  
- 扫描频率 <mark>一般沿用之前的质数值，没有影响到功能（比如电流音问题，Noise杂讯太大）的时候，默认不变化</mark>
  
  ```c
  #define SCAN_FREQ                       79
  ```
  
- V mode 报点率翻倍

  ```c
  #define  VMODE_DOUBLE_RATE_EN       ON
  ```
  
- 模拟参数 <mark>以下为模拟参数，通常无需修改。如要修改请询问研发</mark>

  ```c
  // 反馈电容调整
  #define  RX_CFB_ADJ                     0x16
  // VSTIM 幅度调整
  #define  LEVEL_VSTIM                    0x3D
  ```

- 面板配置 <mark>这个版本的公版 PANEL_INX_6_56 面板配置不正确</mark>

  ```c
  #define PANEL_SCALE                     PANEL_SCALE_21_9 //PANEL_SCALE_19_9
  #define PANEL_SIZ                       PANEL_SIZ_6_56 //PANEL_SIZ_6_22
  
  //Constant.h
  + #define PANEL_SIZ_6_56                              66
  + #define PANEL_SCALE_21_9                            219    
  ```

- 最大手指数

  ```c
  #define MAX_SUPP_TCH_NUM                10
  ```

- 采样系数使能

  <mark>FIR_EN 为 OFF 时 FIR 未生效，此时 FIR 系数默认值为 1024,可修改 FIR_COEF_VALUE 改变系
  数值，该值放大或缩小 1 倍 RawData 放大缩小 1 倍，一般不修改 FIR_COEF_VALUE，如若修改
  请联系固件。当 FIR_EN 为 ON 时打开 FIR，固件会根据 CycleNum 自动配置汉宁窗系数。</mark>

  ```c
  #define FIR_EN  ON
  ```

- 分辨率

  ```c
  #define RES_X                           720
  #define RES_Y                           1612
  ```

- X/Y SWAP

  ```c
  #define SWAP_X                          ON       
  #define SWAP_Y                          ON //16一般OFF，这个公版有问题，修改RX Mapping调整后会改OFF
  #define SWAP_XY                         OFF
  ```

## TPS

1. 调整相关参数

   通过调整 CYCLE_NUM 和 SETUP_TIME_SHARE、SETUP_TIME_ALONE 测得 Normal 模式下 SCAN_FREQ 为 79 时 CYCLE_NUM 最大为11，TPS 预留时间约为1.68ms

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/DSTSS11.bmp)

2. 设置 CYCLE_NUM 为10，防止 TP Break

   ```c
   #define CYCLE_NUM                   10 //7
   ```

3. 设置 SETUP_TIME_SHARE、SETUP_TIME_ALONE

   <mark>一般默认设置为25，若 TPS 时间足够，可以适当增加（100us以内），主要目的为了减小重载画面时对第一个 Term 的干扰影响</mark>

   ```c
       #define SETUP_TIME_SHARE                  40 //5
       #define SETUP_TIME_ALONE                40 //25
   ```

## 设置目标值

1. 关闭干扰功能

   <mark>关闭 Normal 下自动补偿、MNT 功能（主要为了预防切换到 MNT 后，原本保存 CNEG 的值会被填0）、跳频功能（预防跳频之后相关参数值变化）</mark>

   ```c
   #define CNEG_EN                             OFF //ON
   #define MNT_MODE_EN                     OFF
   #define FREQ_HOP_EN                             OFF //ON
   ```

2. 清空补偿电容初始化数据

   <mark>补偿电容初始化数据：防止由于屏内寄生电容导致原始数据饱和</mark>

   ```c
   #define NCAP_L                          0x0 //0x39
   #define NCAP_R                          0x0 //0x38
   
   #define CNEG_L                          {NCAP_L, NCAP_L , NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L, NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L, NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L, NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L, NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L, NCAP_L, NCAP_L, NCAP_L,\
           NCAP_L, NCAP_L\
       }
   #define   CNEG_R                       {NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R, NCAP_R, NCAP_R, NCAP_R,\
           NCAP_R, NCAP_R\
       }
   ```

3. 设置相关参数 <mark>默认值或常见值，视情况调整</mark>

   ```c
   //SHIFT_NUM决定了Rawdata的大小,增加SHIFT_NUM会导致noise同步增加，因此并不会增加SNR
   #define SHIFT_NUM                   0xB //0xC
   ```

4. 编译烧录固件，读取 Rawdata 获取到饱和值4087，设置 RAW_DEST_VALUE 为饱和值一半2000

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230811162526486.png)

   ```c
   #if(SCAN_MODE == SCAN_H)
       #define RAW_DEST_VALUE                  1500
   #else
       #define RAW_DEST_VALUE                  2000 //1700
   #endif
   ```


## Cneg归一化

1. 选取第一份 NCAP_L、NCAP_R 使 Rawdata 整体的值在目标值附近，没有饱和值，将 Rawdata 填入Cneg归一化工具

   ```c
   #define NCAP_L                          0x3c //0x0
   #define NCAP_R                          0x3c //0x0
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230811164011831.png)

2. 选取和第一份差值为3的 NCAP_L、NCAP_R 使 Rawdata 较为平均，没有饱和值，将 Rawdata 填入Cneg归一化工具

   ```c
   #define NCAP_L                          0x39 //0x3c
   #define NCAP_R                          0x39 //0x3c
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230811170129263.png)

   根据第一份和第二份 Rawdata AVG 的差值，设置 CEG_STEP 为 AVG差值/3=108

   ```c
   #define CNEG_STEP                       108 //130
   ```

3. 将Cneg归一化工具生成的 CNEG_L、CNEG_R 填入固件，开启 Normal 下自动补偿，编译烧录固件

   <mark>确认 Rawdata 在目标值附近 ±1 CNEG_STEP 并且 Cneg 居中（40,90）</mark>

   ```c
   #define CNEG_EN                             ON //OFF
   
   #define CNEG_L                           {NCAP_L-6, NCAP_L-4, NCAP_L+2, NCAP_L-6, NCAP_L-1,\
       NCAP_L+0, NCAP_L-3, NCAP_L+1, NCAP_L+0, NCAP_L-3,\
       NCAP_L-1, NCAP_L+1, NCAP_L+4, NCAP_L+1, NCAP_L+3,\
       NCAP_L+1, NCAP_L+3, NCAP_L+3, NCAP_L+2, NCAP_L+1,\
       NCAP_L+2, NCAP_L+0, NCAP_L+3, NCAP_L+4, NCAP_L+7,\
       NCAP_L+7, NCAP_L+6, NCAP_L+3, NCAP_L+7, NCAP_L+6,\
       NCAP_L+5, NCAP_L+3\
       }
   
   #define   CNEG_R                        {NCAP_R-2, NCAP_R-4, NCAP_R+1, NCAP_R-5, NCAP_R-4,\
       NCAP_R-2, NCAP_R-4, NCAP_R-5, NCAP_R-2, NCAP_R-5,\
       NCAP_R+2, NCAP_R-1, NCAP_R-3, NCAP_R-2, NCAP_R+0,\
       NCAP_R+0, NCAP_R-1, NCAP_R+2, NCAP_R+1, NCAP_R+2,\
       NCAP_R+1, NCAP_R+3, NCAP_R+2, NCAP_R+0, NCAP_R+2,\
       NCAP_R+4, NCAP_R+2, NCAP_R+6, NCAP_R+4, NCAP_R+5,\
       NCAP_R+4, NCAP_R+2\
       }
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230811172508887.png)

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230811172528951.png)

## Diffdata 感应量校正

### 全屏 Diffdata 感应量矫正

通过 touch工具抓取最大感应量差值，使用9mm铜棒进行上中下感应量确认，要求接充电器均值800左右，不接充电器均值在400以上

- 充电：近端约900感应量，中间约800感应量，远端约600感应量，不用全屏调整

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/%E5%85%85%E7%94%B5.png)

- 悬空：近端约700感应量，中间约600感应量，远端约400感应量，不用全屏调整

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/%E6%82%AC%E7%A9%BA.png)

### 远近端 Diffdata 感应量矫正

使用 touch 工具抓取全屏 Manual 最大值，将 Manual 值填入 Diff 调整系数生成工具生成 LEFT_ADJ_PARAM、RIGHT_ADJ_PARAM 数据，修改固件对应参数，编译烧录固件后查看最大感应量差值

<mark>要注意是否需要翻转数据，参考 X/Y SWAP </mark>

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/Diffdata%20%E6%84%9F%E5%BA%94%E9%87%8F%E7%9F%AB%E6%AD%A3.png)

## 报点阈值

- 针对单指的场景：近端感应量接地是1000，悬空/悬浮800，贴钢化膜/泡棉30cm测试约550，取550/3（经验值）=180

- 多指阈值依次递减，充电阈值按经验约为1.5倍~2倍单指阈值

  <mark>此处阈值设置仅为初版，实际阈值还是要经过测试不断调整</mark>

  ```c
  #define TCH_DOWN_TH_1PT                 180 //150
  #define TCH_DOWN_TH_2PT                 160 //125
  #define TCH_DOWN_TH_3PT                 140 //125
  #define TCH_DOWN_TH_4PT                 130 //125
  #define TCH_DOWN_TH_5PT                 120 //125
  #define TCH_CHARGER_TH                  280 //150
  ```

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816095557114.png)

## SNR

- Normal模式下SNR计算方式采用主流计算方式如下图，Monitor和Gesture模式SNR计算方式采用 Peak-Peak

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822091303088.png)

## 功耗

## 产测

## 参数临界值排查

## Normal相关参数相关性

## 其他

## 遇到的问题

## 参考

