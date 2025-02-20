# TP RC Loading仿真

## 手机IC

手机IC直接将仿真需求转给AE即可。

## 平板IC

- 平板IC需要FAE进行初步判断。
	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202502201425661.jpg)

- 计算方式参考如下：
	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202502201426832.jpg)

- PS：

	1. C7和C8是同一组电容的不同表述方式，算Ctotal的时候，C7或C8算进去一次就可以了

	2. 不给R4和m，就默认Rtotal=R1+R1_Fanout。这个参数目的是计算从ic出来到最远端sensor的电阻的影响

	3. C8'可以不算

		![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202502201427676.jpg)

	4. 部分PASS则需看客户需求是否需要仿真，默认时间一周，可以加急