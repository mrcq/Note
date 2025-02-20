# LPWG时序配置

## 准备

- BOE提供的LPWG时序图
- CGOUT对应表
- 对应IC COB
- AE提供的对应IC LPWG CGOUT配置表格

## 过程

- 确认IC内部cgout pin所对应的panel cgout
	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131548073.png)

- 确认相关panel cgout pin是如何扫描（vsp、vsn、gnd）
	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131550298.png)

- 确认相关panel cgout pin扫描幅值（low、high、gnd）
	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131553722.png)

- BOE没有allgateon，根据LPWG时序图确认对应cgout pin 扫描方式填到interval，将对应幅值填到GTPS，没涉及的pin填HIZ即可

	![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202501131545258.png)

- 将生成的LPWG参数填入code

- 在COB上确认LPWG波形是否正确

## 注意

1. TP上位机从图形界面下进入手势抓取波形
2. 每次重新抓取波形需要重新上电，否则扫描准位不对