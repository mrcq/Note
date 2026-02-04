# DDIC封装

随着高刷新率屏幕（如144Hz、240Hz）普及，屏占比成为显示技术竞争的核心指标。屏幕封装工艺通过优化驱动芯片（DDIC）与基板的集成方式，显著影响屏幕边框宽度。目前主流封装技术包括 **COG**、**COF** 和 **COP**，三者技术路径与适用场景差异显著。

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260203100651031.png)

## COG（Chip on Glass）
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260203100654322.png)
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260203100650131.png)

COG（Chip On Glass）：将显示驱动IC直接绑定在LCD面板的玻璃基板上，同时FPC排线也连接在玻璃端，是最传统、技术门槛较低、成本较低且良率高、易量产的封装方式，长期被广泛采用。其优点是结构直接、模块体积可控，但由于玻璃基板不可弯折，且IC位于屏幕有效显示区下方并与排线一起占用空间，面板下侧必须预留下巴区域，边框较难进一步收窄。

## COF（Chip on Film，Chip on Flex）

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/20260203112202696.png)

![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202502141514294.png)

COF（Chip On Film）：将驱动IC安装在柔性FPC（柔性PCB）上，通过热压方式把IC的金凸块（Gold Bump）与软性基板内引脚（Inner Lead）进行绑定，再利用FPC可弯折特性把器件翻折到屏幕下方，本质上可视为COG的升级形态。由于IC不再占用玻璃显示区版面，在相同面板尺寸下可释放布局空间、缩小下边框（通常可减少约1.5mm量级），同时具备结构更轻薄、集成度更高的优势，相比COG在窄边框设计上更有利。

> IC 贴在 **一条独立的FPC** 上

## COP（Chip On Plastic）
![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/202502141525134.png)

COP（Chip On Plastic）：将DDIC直接固定在柔性塑料基板上，并与柔性OLED面板一体配合使用，利用柔性OLED可弯曲特性，把IC与排线整体弯折至屏幕背面，从而实现三者中边框压缩能力最强的封装方案。该方式必须与柔性OLED组合使用（不适用于刚性面板），虽然可显著减少边框，但整体成本较高、工艺难度大且良品率相对偏低。

> IC 贴在 **面板自己的柔性塑料基材上**

## 参考

- [一文看透AMOLED/LCD的DDI、COG\COF\COP封装和FPCB 显示面板用图像向我们传达各种视觉信息。而通过更好的色彩和分辨率，以此来证实显示质量得到稳步的发展。最近推出的三星Gala... - 雪球](https://xueqiu.com/7073292940/106895698)
- [三种屏幕封装工艺介绍：COG、 COF、 COP : 模切网](http://www.moqie.com/articleview_18248.aspx)
