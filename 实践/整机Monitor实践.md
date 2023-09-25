# 整机Monitor实践

[TOC]

## INFO

- 硬件设备：ICNL9916 禾苗E6553
- Git Branch：ZTE_A73_Sprocomm_E6553_Easyquick_BOE6.56_ICNL9916
- Cneg归一化工具：ICNL9916_CNEG_Auto_Setting_V1.2_20230712.xlsx
- touch工具：touch_viewer_9916、chipone-touch-v1.2.5.apk
- AutoCheckTool：AutoCheckTool_V3.7.2

## 参数配置

<mark>以下参数在调屏前根据设备信息或客户需求进行确认和配置</mark>

### Panel_06.h

- Monitor mode 功能使能

  ```c
  #define MNT_MODE_EN                     ON //OFF
  ```
  
- Monitor mode base trace 默认开启

  ```c
  #define BASE_TRACE_MNT_EN               ON
  ```
  
- 报点率 

  <mark>例如默认报点率是 60HZ，若是本参数这是为 2 则报点率降为 30，1则不降频</mark>
  
  ```c
  #define REPORT_RATE_DOWN_RATIO          1 //2
  ```
  
- 无手指触摸时进入MNT的Count

  <mark>Count数为这个这个值*8，用来计算进入MNT的时间（和客户确认）</mark>

  ```c
  #define ENTER_MNT_CNT                   25 //100
  ```

- 模拟参数 <mark>以下为模拟参数，通常无需修改。如要修改请询问研发</mark>

  ```c
  // 反馈电容调整
  #define CFB_ADJ_MNT                     0x3F
  // VSTIM 幅度调整
  #define VSTIM0_LEVEL_MNT                0x1F
  ```

- Monitor 模式调试功能

  <mark>启用该功能后，可以通过命令输出 monitor 模式下的数据（一般整机会用到，模组关掉）</mark>
  
  <mark>目前通过tcs指令强行切换进mnt，该参数意义不大</mark>
  
  ```c
  #define MNT_DATA_DBG_EN                 OFF
  ```
  
## TPS

1. 确保MNT下不会TPS Break，留好余量

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/DSTSS12.bmp)

   > **降TPS：**
   >
   > MNT_SETUP_TIME 减小
   >
   > CYCLE_NUM_MNT 减小

## 设置目标值

1. 关闭干扰功能

   关闭 Monitor 自动补偿

   ```c
   #define CNEG_EN_MNT                     OFF //ON
   ```

2. 清空补偿电容初始化数据

   <mark>补偿电容初始化数据：防止由于屏内寄生电容导致原始数据饱和</mark>

   ```c
   #define DF_NEG_CAP_M                    0
   
     #define   CNEG_MNT                       {\
       {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M },\
    {DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M, DF_NEG_CAP_M } }
   ```

3. 设置相关参数 <mark>默认值或常见值，视情况调整</mark>

   ```c
   // Monitor 模式下的 Shift Number
   #define MNT_SHIFT_NUM                   0xB
   // Monitor 模式 Cycle num
   #define CYCLE_NUM_MNT                   3
   ```
   
4. 编译烧录固件，下发指令进入mnt，读取 Rawdata 获取到饱和值4088，设置 RAW_DEST_VALUE_MNT 为饱和值一半2000

   ```shell
    /data/write_tcs 2 4 04
   ```

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816212250443.png)

   ```c
   #define RAW_DEST_VALUE_MNT              2000
   ```

## Cneg归一化

1. 选择两个合适的DF_NEG_CAP_M参数，尽量使rawdata接近目标值，并且没有饱和值，例如40和42

   ```c
   #define DF_NEG_CAP_M                    42 //0
   ```

   分别将两次DF_NEG_CAP_M参数编译烧录，下发指令进入mnt，获取rawdata并填入Cneg归一化工具

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816215900446.png)

2. 将Cneg归一化工具生成的RAW_DEST_VALUE_MNT和CNEG_MNT填入代码

   ```c
   #define CNEG_STEP_MNT                   90 //100
   
   #define   CNEG_MNT                       { {DF_NEG_CAP_M+7, DF_NEG_CAP_M+4, DF_NEG_CAP_M+0, DF_NEG_CAP_M-4, DF_NEG_CAP_M+1, DF_NEG_CAP_M+5 },\
    {DF_NEG_CAP_M+1, DF_NEG_CAP_M-3, DF_NEG_CAP_M-2, DF_NEG_CAP_M-2, DF_NEG_CAP_M+0, DF_NEG_CAP_M+4 },\
    {DF_NEG_CAP_M+2, DF_NEG_CAP_M-1, DF_NEG_CAP_M+0, DF_NEG_CAP_M-1, DF_NEG_CAP_M+1, DF_NEG_CAP_M+4 },\
    {DF_NEG_CAP_M+1, DF_NEG_CAP_M-3, DF_NEG_CAP_M-2, DF_NEG_CAP_M+1, DF_NEG_CAP_M+2, DF_NEG_CAP_M+6 },\
    {DF_NEG_CAP_M+1, DF_NEG_CAP_M-2, DF_NEG_CAP_M-1, DF_NEG_CAP_M+2, DF_NEG_CAP_M+3, DF_NEG_CAP_M+6 },\
    {DF_NEG_CAP_M+0, DF_NEG_CAP_M-3, DF_NEG_CAP_M-2, DF_NEG_CAP_M-2, DF_NEG_CAP_M-1, DF_NEG_CAP_M+2 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+1, DF_NEG_CAP_M+1, DF_NEG_CAP_M+3, DF_NEG_CAP_M+5, DF_NEG_CAP_M+9 },\
    {DF_NEG_CAP_M+1, DF_NEG_CAP_M-3, DF_NEG_CAP_M-4, DF_NEG_CAP_M-2, DF_NEG_CAP_M+1, DF_NEG_CAP_M+5 },\
    {DF_NEG_CAP_M+2, DF_NEG_CAP_M-2, DF_NEG_CAP_M-3, DF_NEG_CAP_M+2, DF_NEG_CAP_M+5, DF_NEG_CAP_M+8 },\
    {DF_NEG_CAP_M+3, DF_NEG_CAP_M-1, DF_NEG_CAP_M-1, DF_NEG_CAP_M-2, DF_NEG_CAP_M+1, DF_NEG_CAP_M+5 },\
    {DF_NEG_CAP_M+4, DF_NEG_CAP_M+0, DF_NEG_CAP_M+0, DF_NEG_CAP_M-1, DF_NEG_CAP_M+2, DF_NEG_CAP_M+6 },\
    {DF_NEG_CAP_M+10, DF_NEG_CAP_M+7, DF_NEG_CAP_M+6, DF_NEG_CAP_M+1, DF_NEG_CAP_M+4, DF_NEG_CAP_M+8 },\
    {DF_NEG_CAP_M+3, DF_NEG_CAP_M+0, DF_NEG_CAP_M-1, DF_NEG_CAP_M-1, DF_NEG_CAP_M+2, DF_NEG_CAP_M+6 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+1, DF_NEG_CAP_M+1, DF_NEG_CAP_M+4, DF_NEG_CAP_M+7, DF_NEG_CAP_M+11 },\
    {DF_NEG_CAP_M-1, DF_NEG_CAP_M-5, DF_NEG_CAP_M-6, DF_NEG_CAP_M+1, DF_NEG_CAP_M+4, DF_NEG_CAP_M+8 },\
    {DF_NEG_CAP_M+9, DF_NEG_CAP_M+5, DF_NEG_CAP_M+4, DF_NEG_CAP_M+4, DF_NEG_CAP_M+8, DF_NEG_CAP_M+12 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+1, DF_NEG_CAP_M+0, DF_NEG_CAP_M+2, DF_NEG_CAP_M+6, DF_NEG_CAP_M+10 },\
    {DF_NEG_CAP_M+7, DF_NEG_CAP_M+3, DF_NEG_CAP_M+2, DF_NEG_CAP_M-2, DF_NEG_CAP_M+1, DF_NEG_CAP_M+6 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+1, DF_NEG_CAP_M+0, DF_NEG_CAP_M+0, DF_NEG_CAP_M+3, DF_NEG_CAP_M+7 },\
    {DF_NEG_CAP_M+6, DF_NEG_CAP_M+2, DF_NEG_CAP_M+1, DF_NEG_CAP_M-1, DF_NEG_CAP_M+2, DF_NEG_CAP_M+7 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+1, DF_NEG_CAP_M+0, DF_NEG_CAP_M+1, DF_NEG_CAP_M+4, DF_NEG_CAP_M+9 },\
    {DF_NEG_CAP_M+5, DF_NEG_CAP_M+2, DF_NEG_CAP_M+1, DF_NEG_CAP_M-6, DF_NEG_CAP_M-2, DF_NEG_CAP_M+3 },\
    {DF_NEG_CAP_M-1, DF_NEG_CAP_M-5, DF_NEG_CAP_M-7, DF_NEG_CAP_M+3, DF_NEG_CAP_M+6, DF_NEG_CAP_M+11 },\
    {DF_NEG_CAP_M+3, DF_NEG_CAP_M-1, DF_NEG_CAP_M-2, DF_NEG_CAP_M-5, DF_NEG_CAP_M-2, DF_NEG_CAP_M+4 },\
    {DF_NEG_CAP_M-3, DF_NEG_CAP_M-8, DF_NEG_CAP_M-9, DF_NEG_CAP_M+0, DF_NEG_CAP_M+3, DF_NEG_CAP_M+8 },\
    {DF_NEG_CAP_M+3, DF_NEG_CAP_M-1, DF_NEG_CAP_M-2, DF_NEG_CAP_M+2, DF_NEG_CAP_M+5, DF_NEG_CAP_M+10 },\
    {DF_NEG_CAP_M+3, DF_NEG_CAP_M-1, DF_NEG_CAP_M-1, DF_NEG_CAP_M-5, DF_NEG_CAP_M-1, DF_NEG_CAP_M+3 },\
    {DF_NEG_CAP_M+4, DF_NEG_CAP_M+0, DF_NEG_CAP_M+0, DF_NEG_CAP_M+4, DF_NEG_CAP_M+7, DF_NEG_CAP_M+11 },\
    {DF_NEG_CAP_M+8, DF_NEG_CAP_M+4, DF_NEG_CAP_M+4, DF_NEG_CAP_M+4, DF_NEG_CAP_M+7, DF_NEG_CAP_M+11 },\
    {DF_NEG_CAP_M+2, DF_NEG_CAP_M-2, DF_NEG_CAP_M-2, DF_NEG_CAP_M+3, DF_NEG_CAP_M+6, DF_NEG_CAP_M+10 },\
    {DF_NEG_CAP_M+1, DF_NEG_CAP_M-4, DF_NEG_CAP_M-4, DF_NEG_CAP_M+1, DF_NEG_CAP_M+3, DF_NEG_CAP_M+8 },\
    {DF_NEG_CAP_M+8, DF_NEG_CAP_M+4, DF_NEG_CAP_M+5, DF_NEG_CAP_M-1, DF_NEG_CAP_M+1, DF_NEG_CAP_M+5 } }
   ```

3. 开启Monitor 自动补偿，烧录固件观察效果

   ```c
   #define CNEG_EN_MNT                     ON //OFF
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816220155460.png)
   
   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816220227476.png)
   
   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816224108371.png)

   > **Cneg检查**
   >
   > **如整体偏大：**
   >
   > 开启cfb做补偿，”#defineUSE_CFB_F_CNEG_MNTON”；
   >
   > 重调rawdata
   >
   > **如个别节点偏大：**
   >
   > 调整对应节点cneg或step
   >
   > **如个别节点偏小：**
   >
   > 先增大cneg居中->导致rawdata缩小，再调整CFB_ADJ_MNT、VSTIM0_LEVEL_MNT、CYCLE_NUM_MNT，促使rawdata恢复正常水平

## Diffdata 感应量矫正

### 全屏 Diffdata 感应量矫正

确认 Diffdata，模组任何位置触摸量（差值）不小于 500，整机比模组小些（差值）不小于 400

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816223145748.png)

### 远近端 Diffdata 感应量矫正

如果远近端差距比较大，建议开启 DIFF_DIFFERENCE_ADJ 调整下远近端补偿

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817165358988.png)

> **增大Diffdata：** 
>
> MNT_SHIFT_NUM 减小
>
> CFB_ADJ_MNT 减小
>
> VSTIM0_LEVEL_MNT 增大
>
> CYCLE_NUM_MNT 增大

## 报点阈值

## SNR

1. Monitor和Gesture模式SNR计算方式采用 Peak-Peak，Normal模式下SNR计算方式采用主流计算方式（详见 整机Normal实践）

   ![image-20230821205714870](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230821205714870.png)

2. 远端/近端Diffdata / 无触20帧时Diffdata最大值 要大于 10

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230816223656771.png)

> **降noise：**
>
> CNEG_STEP_MNT 减小
>
> MNT_SETUP_TIME 增大
>
> MNT_SHIFT_NUM 增大
>
> CFB_ADJ_MNT 增大
>
> VSTIM0_LEVEL_MNT 增大
>
> CYCLE_NUM_MNT 增大

## 功耗

1. 根据客户需求确认三路供电是否符合功耗要求，测量手法可参考 [TDDI功耗测试手册V0.1](https://chiponeic.feishu.cn/file/RHCCbIhuYo3dl0xZ6K7cuXHWnbg)

   > VSTIM0_LEVEL_MNT 减小
   >
   > CYCLE_NUM_MNT 减小

## 产测

1. 检查是否有异常偏小Rawdata节点，如有尽量从FW上去做cover

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817102024130.png)

## 参数临界值排查（基于Rawdata 2000）

1. CNEG_STEP_MNT 不要设太小（一般60~130）

   ```c
   #define CNEG_STEP_MNT                   90
   ```

2. MNT_SETUP_TIME 不要设太小（不要小于30微秒，太小噪声大）（Monitor测噪声要用重载画面测）

   ```c
   #define MNT_SETUP_TIME                  40
   ```

3. CFB_ADJ_MNT 不要设太小（太小噪声大）

   ```c
   #define CFB_ADJ_MNT                     0x3F
   ```

4. VSTIM0_LEVEL_MNT 不要设太小

5. CYCLE_NUM_MNT 不要设太大（太大TPS Break、功耗偏大）

## Monitor相关参数相关性

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817144138928.png)

## 其他

### FPC接地(背光)排查

导电胶贴不贴对Rawdata和Cneg影响很大

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230817145150090.png)

### Normal/Monitor切换排查

1. Monitor下切Normal效果不好可以开启 BASE_TRACE_MNT_EN（追Base）

   ```c
   #define BASE_TRACE_MNT_EN               ON
   ```

2. Monitor下切Normal效果不好可以降低阈值，防止无法退出Monitor

   ```c
   #define MNT_LP_TH                       80
   ```

## 遇到的问题

1. 整机阶段调试 MNT_DATA_DBG_EN 开关没有发现什么区别，进入MNT后即使MNT_DATA_DBG_EN为OFF也能获得数据
1. 重载画面是纯黑和纯白来回切换吗
1. Monitor报点阈值 MNT_LP_TH 多少比较好

## 参考

- [ICNL9916_触控类_mnt调试总结](https://chiponeic.feishu.cn/minutes/obcnno6213qt1k6ds6tza2z2)
- [ICNL9916 ICNL9916C ICNL9922C调试指南 20230718](https://chiponeic.feishu.cn/file/DfNWbIopeohZ13xqvl3cOnifnKh)
- [ICNL9911C、ICNL9916 、ICNL9922C 整机mnt、手势查看指令](https://chiponeic.feishu.cn/sheets/shtcnXK8jVya9TTigd6eLTVlzdh?sheet=ad3735)
- [TDDI功耗测试手册V0.1](https://chiponeic.feishu.cn/file/RHCCbIhuYo3dl0xZ6K7cuXHWnbg)
