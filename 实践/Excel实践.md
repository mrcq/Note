# Excel实践

## 场景一：整理Rawdata变化曲线

> 手势下前几帧Rawdata一直波动，需要统计变化曲线观察Rawdata波动趋势

1. 收集原始数据

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117165846144.png)

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117165909673.png)

2. 处理原始数据

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117170159805.png)

   - `INDIRECT`：这是一个Excel函数，用于将一个以文本形式表示的单元格引用转换为实际的单元格引用。它接受一个字符串参数作为输入，该字符串指定要间接引用的单元格地址。
   - `"E"`：这是一个字符串，用于表示列标签为"E"。我们将在公式中使用该字符串来构建单元格引用。
   - `ROW()`：这是一个Excel函数，用于返回包含当前公式的单元格所在的行号。
   - `ROW($A$2)`：这是一个固定的单元格引用，用于表示A2单元格的行号。我们将在公式中使用这个固定的引用来计算相对于A2单元格的行偏移量。
   - `(ROW()-ROW($A$2))`：这个表达式计算出当前公式所在的行相对于A2单元格的行偏移量。例如，如果当前公式位于A2，那么这个值就是0；如果当前公式位于A3，那么这个值就是1，以此类推。
   - `*35 + 3`：这个表达式将上述行偏移量乘以35，并加上3，得到了根据模式需要的行号。在您的例子中，每隔35行出现一次目标单元格，因此我们需要乘以35来跳过其他行。
   - `INDIRECT("E" & (ROW()-ROW($A$2))*35 + 3)`：这个表达式将上述计算得到的行号与列标签"E"连接起来，然后作为参数传递给INDIRECT函数。最终的结果是根据模式生成的单元格引用。

   - 将A2向下拖动，直至A31(取30帧)，即可依次填充下面的数据

   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117171406211.png)

3. 绘制曲线

   选中全部数据：
   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117171737126.png)

   选择合适和图表：
   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117171839319.png)
   
   修改名字：
   ![](https://cdn.jsdelivr.net/gh/mrcq/Image@main/image-20231117171947080.png)