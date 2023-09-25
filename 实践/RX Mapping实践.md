# RX Mapping实践

[TOC]

## INFO

- 硬件设备：ICNL9916 麦博韦尔-TCL-civic德智欣_INX6.56,V60hz

- Git Branch：origin/master_ICNL9916

  ICNL99116 SampleCode V2.2 Beta2, lib commit id 97f8481b. see readme.txt for details

  a7862ae2011d4d76590aaedf0b9754011fe54f72

- Cneg归一化工具：ICNL9916_CNEG_Auto_Setting_V1.2_20230712.xlsx

- RX Mapping 图、Sensor 图

- Diffdata补偿工具：异常位置传感器校正_20220510_V1.4.xlsx

## 常规屏 RX Mapping 调整流程

- 常规屏左屏或右屏每列的 RX 排布采用相同的顺序号

  ![image-20230814200412284](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230814200412284.png)

- 将RX Mapping数据填入 ICNL9916_CNEG_Auto_Setting_V1.2_20230712.xlsx，将生成的 HW_CH_MAP_LEFT、HW_CH_MAP_RIGHT 拷贝到代码中

  ![image-20230814200529664](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230814200529664.png)

  ```c
  #define HW_CH_MAP_LEFT {\
          0,  1,  2,  3,\
          4,  5,  6,  7,\
          8,  9,  10, 11,\
          12, 13, 14, 15,\
          16, 17, 18, 19,\
          20, 21, 22, 23,\
          24, 25, 26, 27,\
          28, 29, 30, 31,\
      }
  
  #define HW_CH_MAP_RIGHT {\
          31, 30, 29, 28,\
          27, 26, 25, 24,\
          23, 22, 21, 20,\
          19, 18, 17, 16,\
          15, 14, 13, 12,\
          11, 10, 9,  8,\
          7,  6,  5,  4,\
          3,  2,  1,  0,\
      }
  ```
  
## Diffdata 补偿设置

在 Common.c DiffValAdjust()函数新增补偿代码，新增代码包含三个部分，挖空补偿、顶部补偿和底部补偿

```c
#elif (PANEL_TYPE == PANEL_INX_6_56) // 对应PANEL
    {
// ===========================================================================
// 1. 挖孔补偿，参考其他PANEL，部分参数和固件同时沟通修改（例如 pDif[2][1] * 9）
// ===========================================================================
        if (pDif[1][2] > g_TpTh.u16PosPeakTh * 6 / 4
            || pDif[2][1] > g_TpTh.u16PosPeakTh * 6 / 4)
        {
            if ((pDif[2][2] + pDif[2][1]) != 0)
            {
            pDif[1][1] = (S32)pDif[1][2] * pDif[2][1] * 9
                         / (3 * (pDif[2][2] + pDif[2][1]));
        }
        }

        if (pDif[1][17] > g_TpTh.u16PosPeakTh * 6 / 4
            || pDif[2][18] > g_TpTh.u16PosPeakTh * 6 / 4)
        {
            if ((pDif[2][17] + pDif[2][18]) != 0)
            {
            pDif[1][18] = (S32)pDif[1][17] * pDif[2][18] * 9
                          / (3 * (pDif[2][17] + pDif[2][18]));
        }
        }
        if (pDif[1][1] <= 0)
        {
            pDif[1][1] = 0;//(pDif[1][8] + pDif[1][10]) >> 1;
            //pDif[2][9] = (pDif[2][8] + pDif[2][10]) >> 1;
        }

        if (pDif[1][18] <= 0)
        {
            pDif[1][18] = 0;//(pDif[1][9] + pDif[1][11]) >> 1;
            //pDif[2][10] = (pDif[2][9] + pDif[2][11]) >> 1;
        }
// Manual Diff 值等于 Real Diff 值
        g_s16BufDif_Manual[1][1] = pDif[1][1];
        g_s16BufDif_Manual[1][18] = pDif[1][18];
// ===========================================================================
// 2. 根据RX Mapping图第一行传感器缺失位置，选择异常位置传感器校正表格对应sheet，
//	根据Sensor图填入响应数据，配合机台测试不断调整参数
// ===========================================================================
        //        pDif[1][1] = (pDif[1][2] * 924) >> 10;
        pDif[1][2] = (pDif[1][2] * 452 + pDif[1][3] * 585) >> 10;
        pDif[1][3] = (pDif[1][3] * 438 + pDif[1][4] * 623) >> 10;
        pDif[1][4] = (pDif[1][4] * 400 + pDif[1][5] * 662) >> 10;
        pDif[1][5] = (pDif[1][5] * 361 + pDif[1][6] * 697) >> 10;
        pDif[1][6] = (pDif[1][6] * 326 + pDif[1][7] * 732) >> 10;
        pDif[1][7] = (pDif[1][7] * 291 + pDif[1][8] * 760) >> 10;
        pDif[1][8] = (pDif[1][8] * 263 + pDif[1][9] * 778) >> 10;

        //          pDif[1][18] = (pDif[1][17] * 724) >> 10;
        pDif[1][17] = (pDif[1][17] * 452 + pDif[1][16] * 585) >> 10;
        pDif[1][16] = (pDif[1][16] * 438 + pDif[1][15] * 623) >> 10;
        pDif[1][15] = (pDif[1][15] * 400 + pDif[1][14] * 662) >> 10;
        pDif[1][14] = (pDif[1][14] * 361 + pDif[1][13] * 697) >> 10;
        pDif[1][13] = (pDif[1][13] * 326 + pDif[1][12] * 732) >> 10;
        pDif[1][12] = (pDif[1][12] * 291 + pDif[1][11] * 760) >> 10;
        pDif[1][11] = (pDif[1][11] * 263 + pDif[1][10] * 778) >> 10;
// ===========================================================================
// 3. 根据Sensor图确认第一列和和最后一列底部偏离，选择异常位置传感器校正表格对应sheet，
//	根据Sensor图填入响应数据，配合机台测试不断调整参数
// ===========================================================================

        pDif[27][1] = (pDif[27][1] * 959 + pDif[28][1] * 60) >> 10;
        pDif[28][1] = (pDif[28][1] * 964 + pDif[29][1] * 55) >> 10;
        pDif[29][1] = (pDif[29][1] * 969 + pDif[30][1] * 47) >> 10;
        pDif[30][1] = (pDif[30][1] * 977 + pDif[31][1] * 3) >> 10;
        pDif[31][1] = (pDif[31][1] * 1021 + pDif[32][1] * 256) >> 10;
        pDif[32][1] = (pDif[32][1] * 886) >> 10;

        pDif[27][18] = (pDif[27][18] * 959 + pDif[28][18] * 60) >> 10;
        pDif[28][18] = (pDif[28][18] * 964 + pDif[29][18] * 55) >> 10;
        pDif[29][18] = (pDif[29][18] * 969 + pDif[30][18] * 47) >> 10;
        pDif[30][18] = (pDif[30][18] * 977 + pDif[31][18] * 3) >> 10;
        pDif[31][18] = (pDif[31][18] * 1021 + pDif[32][18] * 256) >> 10;
        pDif[32][18] = (pDif[32][18] * 886) >> 10;
    }
```

![image-20230815111353211](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230815111353211.png)

![image-20230815110707537](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230815110707537.png)

![image-20230815111441369](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230815111441369.png)

![image-20230815111146517](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20230815111146517.png)
