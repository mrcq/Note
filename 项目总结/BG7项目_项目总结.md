# BG7项目_项目总结.md

## 驱动节点

- 驱动查看固件属性

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230914103128445.png)

- 驱动查看驱动属性

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230914103015983.png)

- 示例：esd_protection

  **驱动通过tcs通信询问固件是否配置：**
  
  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230914104845770.png)
  
  **驱动查看自身宏是否开启：**
  
  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230914104725791.png)
  
## 边缘抑制

### 虎口抑制区错误

> 测试边缘抑制时，发现横屏状态下虎口抑制区域不对，抑制区域同竖屏状态下没有变

- edge_restain

  上层通过修改/sys/chipone-tddi/misc下的edge_restain节点来通知固件当前屏幕状态

  该节点可写可读，读时打印当前值，写时校验写入值，若正确则发送tcs指令通知固件
  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231109094430025.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231109094456841.png)

- 不同状态下虎口抑制区

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/8973a828925d8c41aeaa6ed2f5bfa4b.png)

- 问题原因分析

  该项目原因是在屏幕旋转时，上层没有修改该节点（会打印log），所以导致抑制区域不对

### 虎口抑制区消点（正常）

横屏状态下分为左右两个半屏 当任意半屏只有虎口处一个点时，可以正常报点，但任意半屏除虎口以外有报点时，会消点虎口报点，因为此时固件认为虎口处点是无效点，右半屏的点不会消除左半屏虎口处的点

## Charger

> 在分析多指消点问题时，埋点发现充电状态位没有被清掉，阈值变高导致消点

- charger_state

  通过wifi连接adb，查看/sys/chipone-tddi/misc下的charger_state在插拔充电器时是否正常

- manual diff打印charger状态

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120165012594.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120165047961.png)

- kernel log

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_98dd63f6-fd9d-46a3-bcfd-c7d5744fde1g.jpg)

  发现亮屏下插拔数据线，charger_state不改变，查看驱动log发现消息被忽略了

- 查看配置

  到/sys/chipone-tddi/charger-det路径下查看充电器配置，发现name和kernel log中的不一致![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231109170238891.png)

- 临时修改配置

  ```shell
  adb shell "echo mtk_charger_type POWER_SUPPLY_PROP_ONLINE 2000 > /sys/chipone-tddi/charger-det/param"&& adb shell "echo 1 > /sys/chipone-tddi/charger-det/enable" && adb shell "echo 1 > /sys/chipone-tddi/charger-det/running"&& adb shell "cat /sys/chipone-tddi/charger-det/enable"&& adb shell "cat /sys/chipone-tddi/charger-det/param"&& adb shell "cat /sys/chipone-tddi/charger-det/running"&& adb shell "cat /sys/chipone-tddi/charger-det/state"&& adb shell "cat /sys/chipone-tddi/charger-det/type"
  ```
  
  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231109171917346.png)
  
  测试插拔数据线后充电器状态位正常变更，于是可以让客户修改设备树
  
- 正式修改配置

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231109172032503.png)

- 参考文档

  [ICNL9911C _触控类_充电器检测调试方法.doc - 飞书云文档 (feishu.cn)](https://chiponeic.feishu.cn/wiki/wikcnxF6z42ru9o2DWYnnXWQJTf)

## 补UP机制

> 客户测试发现存在如不触碰，屏幕卡点不消的情况，内部沟通其他项目有过该问题，原因是是丢UP了，开启补UP机制可解决

- CFG_CTS_FORCE_UP

  驱动开启该宏后使能补UP机制

- 代码流程

  报点流程中检测坐标事件，按下、移动、停留都会使contact变为正值
  
  ```c
  	switch (msgs[i].event) {
          case CTS_DEVICE_TOUCH_EVENT_DOWN:
          case CTS_DEVICE_TOUCH_EVENT_MOVE:
          case CTS_DEVICE_TOUCH_EVENT_STAY:
              contact++;
              input_report_abs(input_dev, ABS_MT_PRESSURE, msgs[i].pressure);
              input_report_abs(input_dev, ABS_MT_TOUCH_MAJOR, msgs[i].pressure);
              input_report_key(input_dev, BTN_TOUCH, 1);
              input_report_abs(input_dev, ABS_MT_POSITION_X, x);
              input_report_abs(input_dev, ABS_MT_POSITION_Y, y);
              input_mt_sync(input_dev);
              break;
  
          case CTS_DEVICE_TOUCH_EVENT_UP:
              break;
          default:
              cts_warn("Process touch msg with unknwon event %u id %u",
                   msgs[i].event, msgs[i].id);
              break;
          }
  ```
  
  如果contact为正且不存在touch_event_timeout_work延迟工作则会创建，已存在则会刷新，UP事件则会注销touch_event_timeout_work延迟工作
  
  ```c
  #ifdef CFG_CTS_FORCE_UP
      if (contact) {
          if (delayed_work_pending(&pdata->touch_event_timeout_work)) {
              mod_delayed_work(cts_data->workqueue,
                      &pdata->touch_event_timeout_work, msecs_to_jiffies(100));
          } else {
              queue_delayed_work(cts_data->workqueue,
                      &pdata->touch_event_timeout_work, msecs_to_jiffies(100));
          }
      } else {
          cancel_delayed_work_sync(&pdata->touch_event_timeout_work);
      }
  #endif
  ```
  
  触发延迟队列时会释放所有的当前报点
  
  ```c
  static void cts_plat_touch_event_timeout_work(struct work_struct *work)
  {
      struct cts_platform_data *pdata = container_of(work,
              struct cts_platform_data, touch_event_timeout_work.work);
  
      cts_warn("Touch event timeout work");
  
      cts_plat_release_all_touch(pdata);
  }
  ```

## 固件升级

> 固件升级脚本常因重启后data挂载路径的失效而升级失败，可通过df指令查看当前data挂载节点，然后修改固件升级脚本

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231115101439634.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231115101721013.png)

- 参考文档

  [‌⁣‍‬⁣⁣⁡﻿⁤‍⁡⁢‍⁢⁡⁣‬‌﻿⁡⁡⁤‌‌‍⁤⁤⁤﻿‍‍‍‬⁣⁤⁢‬‍⁣‌‬TP驱动概述 - 飞书云文档 (feishu.cn)](https://chiponeic.feishu.cn/docx/CoxudaQveoKKU3xMXCzcwyv3n0d)

## 仿真数据

- 判断当前扫描状态（eg：normal、mnt、gstr）

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117142456728.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_34c456d9-2c7e-4147-8b74-9700784eac2g.jpg)

- 报点

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117145347276.png)

  当前帧数、报点id、x、y、压力值（diff值模拟）、报点事件

  **报点事件**

  ```c
  #define CTS_DEVICE_TOUCH_EVENT_NONE         (0)
  #define CTS_DEVICE_TOUCH_EVENT_DOWN         (1)
  #define CTS_DEVICE_TOUCH_EVENT_MOVE         (2)
  #define CTS_DEVICE_TOUCH_EVENT_STAY         (3)
  #define CTS_DEVICE_TOUCH_EVENT_UP           (4)
  ```

- 手势下上报手势码（猜测）

  手势下无事件上报时，AB buffer会 0、1切换，Frame index 会一直+1

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117150558428.png)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117150740479.png)

  猜测当发生手势码上报时，AB buffer会不再切换而是同上一帧，并且Frame index不再+1

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117150903440.png)

  分析手势码问题时可以配合kernel log对应分析，0x50一般是双击唤醒

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117151151232.png)

## 模组产测

- 配置文件

  - 配置文件约定了当前ic类型、通讯方式以及产测配置

  - 如果ic和配置文件不匹配，则无法进入编程模式烧录固件

- 测试参数

  设置参数会修改当前配置文件的测试项和测试项参数

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117152552274.png)

- 后台参数

  后台参数配置产测的流程细节

  - 手势是否跳帧

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117152940444.png)

  - 是否noise项从rawdata改为realdiff

    ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117153208162.png)

- 产测结果

  产测结果文件名为配置文件名+时间戳

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117153245148.png)

## 整机产测

整机产测有两种方式

1. 配置设备树，使用/proc/cts_selftest [参考](https://chiponeic.feishu.cn/docx/BlrydarDfoDCmoxKHxPcL8v5nIc)
2. 配置bin，使用/proc/cts_factory_test [参考](https://chiponeic.feishu.cn/docx/KNcxdk6XqoZYHgxNhJvc6D16nse)

## 手势

### rate

```c
#define GSTR_REPORT_RATE_LP             40   //Hz
#define GSTR_REPORT_RATE_FS             60   //Hz
```

### 手势码

- 固件识别手势事件，通过上报手势码告诉驱动手势信息，常见的手势码如双击唤醒 0x50
- 分析手势码问题建议从kernel log开始分析，查看是否有收到手势码，手势码是否正确
- 分析完kernel log后如未解决问题，则分析固件仿真数据，查看是否有上报手势码，是否成功识别手势码

### debug（通过指令使能手势数据）

```c
#define GSTR_DATA_DBG_EN                OFF  //OFF:gstr slpin
```

固件中该宏在正式版本默认是关闭的，作用是灭屏下仍然可以读取rawdata、diff等数据

如未开启该宏，但在不改变固件的基础上需要抓取灭屏下的手势数据，可以通过发送tcs指令的方式

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117155403964.png)

以9916 IC为例 <font color="Red">每次 </font>亮屏时执行 ./write_tcs 3 43 01指令，即可读取接下来灭屏时的手势数据，再次亮屏时失效

如需限制灭屏下的手势模式（LP/FS）则需再次在灭屏下执行对应的两条指令，如（FS）：

```shell
./write_tcs 2 4 05
./write_tcs 3 32 01
```

### 双击亮屏时间长

> 客户反馈灭屏双击唤醒时间长

- 灭屏双击唤醒流程

  手指双击，固件识别手势码，固件上报手势码，驱动收到手势码，背光亮

- 该问题可能原因分为三部分：固件识别慢、固件驱动通信异常、平台响应背光慢

- 平台响应背光慢

  正常情况约70ms

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_11197c52-491a-4faa-bf02-a47e0c40c24g.jpg)

  异常时约600ms

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_e4390893-32c8-416b-83e4-6786d041e17g.jpg)

  ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_18559cd0-3aba-47e0-ad64-86e7901ea5fg.jpg)

### BG7手势双击无法唤醒（打印Baseline）

1. 打开debug，查看realdiff发现有抬起导致无法识别双击

   ```c
   #define GSTR_DATA_DBG_EN                ON  //OFF:gstr slpin
   ```

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120153536067.png)

2. 查看base发现base和第一帧rawdata有较大偏差，且因diff抬起达到了阈值，所以误认为有触摸无法更新base

   修改TCSCmd.c，注释realdiff代码段，将realdiff的if语句替换到basedata处，这时realdiff将打印basediff

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v3_0250_bb92b443-eaa9-455c-afa3-2f2e8bf2665g.jpg)

   此处为manual替换为basedata

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120154015412.png)

   可以看出base和rawdata有明显差值

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120154238580.png)

3. 修改方案为**进手势后空扫30帧后做updatabase机制**

相关报告：[Transsion_BG7_BOE_ICNL9916_手势唤醒失败问题分析报告](https://chiponeic.feishu.cn/file/LpH7bCadOoljOxxuZ0NcaiZ1nz8)

### 手势边缘通道清空

手势边缘通道会被清空，所以手势下手持触碰不要靠近边缘

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v2_93117fab-b65c-4bb7-97c6-a269a0de38dg.jpg)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231120153536067.png)

## EQ

- 什么是EQ

  EQ简单讲就是上升沿（或者下降沿）不直接到达，中间加个台阶

- BG7 EQ配置

  BG7现在开的是CLK上升沿（2023/11/17），有些玻璃会开有些不开

- 开启EQ的好处

  开了会略微省电

- EQ在何处配置

  显示初始化设置

## 打印噪声

NOISE_DECT_DEBUG

```c
#if NOISE_DECT_DEBUG
NoiseDectDebug();
#endif

#if NOISE_DECT_DEBUG
extern U16 u16NoiDifVal[3];

U32 u32Max1 = 0;
U32 u32Max2 = 0;
U32 u32Max3 = 0;
void NoiseDectDebug(void)
{
    S16 (*s16DiffData)[PHY_NUM_COL + 2U]  = (void *)GetCurDataAddr(DATA_TYPE_MAN_DIFF);
    S16 (*s16NoiData)[2] = (void *)GetCurDataAddr(DATA_TYPE_NOISE_RAW);

    #if 1
    s16DiffData[1][1] = g_ClassPara.u16ScanFreqRw;
    //if (u32Max1 < g_u32MaxNoiseDiff[0])
    {
        u32Max1 = g_u32MaxNoiseDiff[0] ;
    }
    //if (u32Max2 < g_u32MaxNoiseDiff[1])
    {
        u32Max2 = g_u32MaxNoiseDiff[1] ;
    }
    //if (u32Max3 < g_u32MaxNoiseDiff[2])
    {
        u32Max3 = g_u32MaxNoiseDiff[2] ;
    }
    s16DiffData[1][2] = (S16)u32Max1;
    s16DiffData[1][3] = (S16)u32Max2;
    s16DiffData[1][4] = (S16)u32Max3;

    if (g_FrmInfo.u8CurNum == 0)
    {
        u32Max1 = 0;
        u32Max2 = 0;
        u32Max3 = 0;
    }
    #endif

    #if 0
    int i = 0, j = 0;
    for (j = 0; j < 32; j++)
    {
        s16DiffData[32 - j][18] = s16NoiData[j][0];
        s16DiffData[32 - j][17] = s16NoiData[j][1];
    }


    for (j = 0; j < 32; j++)
    {
        s16DiffData[32 - j][16] = s16NoiData[j + 32][0];
        s16DiffData[32 - j][15] = s16NoiData[j + 32][1];
    }

    for (j = 0; j < 32; j++)
    {
        s16DiffData[32 - j][14] = s16NoiData[j + 64][0];
        s16DiffData[32 - j][13] = s16NoiData[j + 64][1];
    }
    #endif
}
#endif
```

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v3_025c_d7baed47-7caa-4c70-ab34-2639a407c04g.jpg)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/img_v3_025c_ed025d0d-fc44-4f49-ab4f-b0bac3f5c59g.jpg)

## TODO

1. 学习如何固件（库外）如何埋点，打印充电状态位
1. 什么是温补？什么场景下需要温补？如何配置温补？
1. 如何理解滑动阈值，和UP阈值的关系是？
1. 手势边缘通道为什么要清空
1. 打印噪声从代码上看是打印到 s16DiffData 为什么实际上是manual diff，而且打印的数值很高，如何理解（拔掉数据线变化不大）



  