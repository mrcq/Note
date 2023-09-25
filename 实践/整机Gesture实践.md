# 整机Gesture实践

[TOC]

## INFO

- 硬件设备：ICNL9916 禾苗E6553

- Git Branch：ZTE_A73_Sprocomm_E6553_Easyquick_BOE6.56_ICNL9916

- Cneg归一化工具：ICNL9916_CNEG_Auto_Setting_V1.2_20230712.xlsx

- touch工具：touch_viewer_9916、chipone-touch-v1.2.5.apk

## 参数配置

<mark>以下参数在调屏前根据设备信息或客户需求进行确认和配置</mark>

<mark>大部分案子手势只要求双击唤醒功能，有一小部分案子客户会有相对应的其他字母手势需求，需要根据需求将相对应的功能开启，也需要驱动部分做对应的配合，手势码，手势ID设置</mark>

### Panel_06.h

1. 手势功能使能

   ```c
   #define GSTR_WAKE_UP_EN                 ON
   ```

2. 模拟参数 <mark>以下为模拟参数，通常无需修改。如要修改请询问研发</mark>

   ```c
   // LP、FS 反馈电容调整
   #define CFB_ADJ_GSTR_LP                 0x10
   #define CFB_ADJ_GSTR_FS                 0x10
   // VSTIM 幅度调整
   #define GSTR_VSTIM_LEVEL_LP             0x20
   ```

3. Gster 模式下的 Shift Number 设置 Touch 扫描建立时间 单位 us

   <mark>建立时间一般设为 20~150 us</mark>

   ```c
   #define GSTR_SETUP_TIME                 60
   ```


4. 报点率 <mark>该值越大，检测灵敏度越高，但功耗会越大</mark>  <mark>报点率低可能会丢点，视情况修改</mark>

   ```c
   #define GSTR_REPORT_RATE_LP             20   //Hz
   #define GSTR_REPORT_RATE_FS             40   //Hz
   ```

## TPS

Gesture下显示不工作，一般不涉及 TPS Break

## 设置目标值(LP)

> Gesture LP和Gesture FS扫描方式：
>
> Gesture FS手势扫描，同Normal状态扫描。Gesture LP手势扫描，同亮屏状态的Monitor扫描。
>
> Gesture LP和FS如何切换：
>
> 息屏显示sleep，TP进入Gesture模式——>TP 更新Gesture下的Base——>TP进入Gesture LP直至difVal大于阈值（如开启GSTR_LP_TO_FS_EN则经过GSTR_LP_TO_FS_FRM_CNT帧后切换到FS模式下一帧更新Base）——>进入Gesture FS，计算手势码——>手势码匹配成功则上报主机并退出手势模式，匹配失败则退回到Gesture LP模式

1. 关闭干扰功能
    关闭自动补偿

  ```c
  #define CNEG_GSTR_EN_LP                 OFF
  ```

2. 清空补偿电容初始化数据

   <mark>补偿电容初始化数据：防止由于屏内寄生电容导致原始数据饱和</mark>

   ```c
   #define DF_NEG_CAP_G                   0
   
   #define CNEG_GSTR_LP                       {\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G },\
           {DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G , DF_NEG_CAP_G }}
   
   ```

3. 设置相关参数 <mark>默认值或常见值，视情况调整</mark>

   ```c
   // Gesture LP 模式下的 Shift Number
   #define GSTR_CYCLE_NUM_LP               4
   // Gesture LP 模式 Cycle num
   #define GSTR_SHIFT_NUM_LP               0x0B
   ```

4. 编译烧录固件，下发指令进入Gesture，读取 Rawdata 获取到饱和值4088，设置 RAW_DEST_VALUE_MNT 为饱和值一半2000

   ```shell
   # 亮屏执行：
   ./write_tcs 3 43 01
   # 灭屏执行：
   ./write_tcs 2 4 05
   ./write_tcs 3 32 00
   cat rawdata
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817183903714.png)

   ```c
   #define RAW_DST_VALUE_GSTR_LP           2000
   ```

## Cneg归一化（LP）

1. 在选取 DF_NEG_CAP_G 时发现 Rawdata 比较不均匀，存在极大极小点。尝试修改 GSTR_VSTIM_LEVEL_LP、GSTR_CYCLE_NUM_LP 影响不大，后通过修改 CFB_ADJ_GSTR_LP 改善

   - 修改前

     ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822143805168.png)

   - 修改后

     ```c
     //只针对低功耗模式，值越小触摸量越大，同时噪声也会变大。
     #define CFB_ADJ_GSTR_LP                 0x3F //0x10
     ```

     ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822144705736.png)

2. 增加 CFB_ADJ_GSTR_LP 会导致感应量降低，可通过增加 GSTR_VSTIM_LEVEL_LP、GSTR_CYCLE_NUM_LP_FS 等方式增加感应量

   ```c
   #define GSTR_VSTIM_LEVEL_LP             0x25 //0x20
   ```

3. 选择两个合适的DF_NEG_CAP_G 参数，尽量使Rawdata 接近目标值，并且没有饱和值，例如42和45

   ```c
   #define DF_NEG_CAP_G                   45
   ```

4. 将两次的LP rawdata填入Cneg归一化工具，将生成的CNEG_GSTR_LP、CNEG_GSTR_STEP_LP填入固件，烧录固件查看效果

   <mark>确认 Rawdata 在目标值附近 ±1 CNEG_STEP 并且 Cneg 居中（40,90）</mark>

   ![](C:/Users/zwzhao/AppData/Roaming/Typora/typora-user-images/image-20230823120442271.png)

   ![](C:/Users/zwzhao/AppData/Roaming/Typora/typora-user-images/image-20230823120542462.png)

   

## Diffdata 感应量校正（LP）

1. 查看LP模式下的感应量，模组不低于500，整机不低于400

   ```shell
   ./touch_viewer_9916 -Dm
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817195627965.png)
   
   > 调试说明：
   > 如果发现 GestureLP 状态下 Diff 比较小，达不到标准，可以通过以下手段来调试。以下
   > 手段尝试的优先级按照论述顺序依次降低。如果前面的手段已经满足则不必继续后续的调
   > 试。
   > 增大 CycleNumber(GSTR_CYCLE_NUM_LP_FS)
   > 增大 Vistim Level(GSTR_VSTIM_LEVEL_LP)
   > 减小 ShiftNumber(GSTR_SHIFT_NUM_LP_FS)
   > 减小 CFB_ADJ_GSTR_LP
   > 调节 Reference Positive Cneg Value

## Cneg归一化（FS）



## LP SNR确认

1. 远端/近端Diffdata / 无触20帧时Diffdata最大值 要大于 10

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817211013073.png)
   
## FS 设置目标值

1. 调整 GSTR_SHIFT_NUM_FS，NEG_CAP_GSTR_FS设置为0，下指令查看FS下的rawdata，选择一个合适的饱和值

   ```c
   #define GSTR_SHIFT_NUM_FS               0x0D
   #define NEG_CAP_GSTR_FS                 0
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817212036471.png)

2. 设置目标值为饱和值一半

   ```c
   #define RAW_DST_VALUE_GSTR_FS           1800
   ```

## FS Cneg确认

1. 

   





## 扫描频率调整

## 扫描LP、FS实测图

## allgateon的参数设定

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_c46f433e-6566-4518-959b-7cd47ceb6bbg.jpg)

> 开启手势的机器息屏后，RX、Source、GOA会拉到GND，GOA会拉到VGL（但升压电路已经不工作，所以其电压接近VSN）。
> TP开始扫描后，因为全驱动，RX、Source和GOA开始打方波，但GOA因升压电路已经不工作的原因只有部分电荷涌入，但对于一些放电较差的机器，还是需要在每次tp扫描一段时间后，打开所有gate（VSP），Source拉到GND去释放残留电荷，从而保护玻璃。

1. 和客户确认是否开启 allgateon，如果开启和 显示FAE沟通allgateon相关参数，例如：GSTR_GOUT_ALLGATEON_REG，之后确认开启allgateon对触控是否有影响

2. 该项目没有开启allgateon

   ```c
   #define GSTR_ALLGATEON_EN   OFF
   #define GSTR_GOUT_ALLGATEON_REG {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}
   #define GSTR_ALLGATE_WIDTH  (1U) // 1 means 10us
   ```

## 遇到的问题

1. 手势下没有显示，GSTR_SETUP_TIME的作用是什么？什么情况下需要修改？

2. 在调试 LP Cneg时遇到一个问题，CFB_ADJ_GSTR_LP默认值0x10的情况下Rawdata存在跟多极大极小点，修改GSTR_CYCLE_NUM_LP_FS、GSTR_VSTIM_LEVEL_LP帮助不大

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822143805168.png)

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822143741123.png)

   为什么修改CFB_ADJ_GSTR_LP后可以将极大值变小，极小值变大。CFB_ADJ_GSTR_LP的作用不是随着值增加减小触摸量，减小噪声吗？

   ```c
   #define CFB_ADJ_GSTR_LP                 0x3F //0x10
   ```

3. 修改CFB_ADJ_GSTR_LP可以优化噪声，是不是CFB_ADJ_GSTR_LP的值在允许（DIffdata）的范围内要尽量大？

4. CFB_ADJ_GSTR_LP为什么调试文档上有两个范围且不同

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822143441191.png)

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230822144231021.png)

## 参考

- [ICNL9916 ICNL9916C ICNL9922C调试指南 20230718](https://chiponeic.feishu.cn/file/DfNWbIopeohZ13xqvl3cOnifnKh)
- [ICNL9911C、ICNL9916 、ICNL9922C 整机mnt、手势查看指令](https://chiponeic.feishu.cn/sheets/shtcnXK8jVya9TTigd6eLTVlzdh?sheet=ad3735)



