# Black-Litterman-Model

### 简介
1. 使用 **Python** 复现Black-Litterman模型。  
2. 使用四种不同观点类型来评估Black-Litterman模型。
3. 自编 **back-test** 函数对2015年美股数据进行回测。
4. 针对四种不同观点类型，画出BL模型下的资产组合累计收益率与等市值权重策略下的资产组合累计收益率的折线图对比。
5. 数据：2009/10/2-2019/10/18 过去十年间10只美股价格（包含3大股指）、市值数据。
6. 股票：AAPL.O、MSFT.O、AMZN.O、JPM.N、V.N、JNJ.N、WMT.N、BAC.N、BRK_B.N、XOM.N
7. 股指：SPX.GI、DJI.GI、IXIC.GI

*数据来源: Wind*

### 数据格式
* **Price_Data.xlsx**

| Date       | SPX.GI    | DJI.GI     | IXIC.GI   | … | BAC.N  | BRK_B.N | XOM.N  |
| ---------- | --------- | ---------- | --------- | ---------- | ------ | ------- | ------ |
| 2009/10/02 | 1,025.21  | 9,487.67   | 2,048.11  | … | 14.75  | 65.29   | 48.49  |
| 2009/10/09 | 1,071.49  | 9,864.94   | 2,139.28  | … | 15.80  | 65.74   | 50.45  |
| 2009/10/16 | 1,087.68  | 9,995.91   | 2,156.80  | … | 15.59  | 66.12   | 53.25  |
| 2009/10/23 | 1,079.60  | 9,972.18   | 2,154.47  | … | 14.65  | 66.36   | 53.58  |
| 2009/10/30 | 1,036.19  | 9,712.73   | 2,045.11  | … | 13.17  | 65.66   | 52.20  |
| 2009/11/06 | 1,069.30  | 10,023.42  | 2,112.44  | … | 13.59  | 68.50   | 52.86  |
| 2009/11/13 | 1,093.48  | 10,270.47  | 2,167.88  | … | 14.43  | 68.22   | 53.09  |
| …        | …       | …        | …       | … | …    | …     | …    |
| 2019/10/11 | 2,970.27  | 26,816.59  | 8,057.04  | … | 28.91  | 208.08  | 68.98  |
| 2019/10/18 | 2,986.20  | 26,770.20  | 8,089.54  | … | 30.35  | 208.76  | 67.61  |

525 * 13 DataFrame


* **Market_Value.xlsx**

| Date       | AAPL.O            | MSFT.O            | … | XOM.N            | TOTAL             |
| ---------- | ----------------- | ----------------- | ---------------- | ---------------- | ----------------- |
| 2009/09/30 | 166039636095.00   | 229182530573.00   | … | 329725261574.00  | 724947428242.00   |
| 2009/10/09 | 170626217896.00   | 227667716024.00   | … | 332897083067.00  | 731191016987.00   |
| 2009/10/16 | 168458341342.00   | 236132856151.00   | … | 351399375110.00  | 755990572603.00   |
| 2009/10/23 | 182692869627.00   | 248792981012.00   | … | 353561980673.00  | 785047831312.00   |
| 2009/10/30 | 169777892161.00   | 246218035812.00   | … | 344430979405.00  | 760426907378.00   |
| 2009/11/06 | 175037854443.00   | 253232541701.00   | … | 344557856462.00  | 772828252606.00   |
| 2009/11/13 | 184143713805.00   | 263088366430.00   | … | 344035655247.00  | 791267735482.00   |
| …        | …               | …               | … | …              | …               |
| 2019/11/08 | 1136676052300.00  | 1096411943419.00  | … | 294484998062.00  | 2527572993781.00  |
| 2019/11/09 | 1136676052300.00  | 1096411943419.00  | … | 294484998062.00  | 2527572993781.00  |

525 * 10 DataFrame

### 观点
1. **Market value as view**:    
   投资者无观点，使用当前市值权重（即均衡状态下的权重）作为作为投资组合的权重。 
2. **Arbitrary views**:    
   为投资者分配任意观点，这里随机分配了3个观点：
   * 观点1-伯克希尔哈撒韦(BRK_B)比埃克森美孚(XOM)的预期收益高0.01%；
   * 观点2-微软(MSFT)比摩根大通(JPM)的预期收益高0.025%；
   * 观点3-10%摩根(JPM)+90%VISA(V)的投资组合比10%沃尔玛(WMT)+90%美国银行(BAC)的投资组合预期收益高0.01%。
3. **Reasonable views**:  
   分析研报之后给出的合理性观点，可以参考券商的一致性预期，这里的观点是亚马逊(AMZN)比摩根大通(JPM)的预期收益率高1.7%
4. **Near period return as view**:  
   选用最近`VIEW_T`期的历史平均收益率作为预期收益率。

### 回测结果
1. **Market value as views**:

![Market value as views](https://github.com/jrothschild33/Black-Litterman-Model/blob/master/plot/BL%20Return%20Back%20Test_Market%20value%20as%20view_Year%202015.png)

   * 与等市值权重策略（对照组）表现基本相同
   * 使用市值权重策略不能对未来收益率进行很好的预测
2. **Arbitrary views**:

![Arbitrary views](https://github.com/jrothschild33/Black-Litterman-Model/blob/master/plot/BL%20Return%20Back%20Test_Arbitrary%20views_Year%202015.png)

   * 与等市值权重策略（对照组）表现基本相同
   * 如果投资者的观点不够正确，则即便使用了复杂的BL模型，资产组合表现也不会变得更好
3. **Reasonable views**:  

![Reasonable views](https://github.com/jrothschild33/Black-Litterman-Model/blob/master/plot/BL%20Return%20Back%20Test_Reasonable%20views_Year%202015.png)

* 当给出**准确且有效**的观点时,BL模型的表现非常好
   * 当市场趋势向上时，BL模型的表现效果比整个市场更好（例如：2015年中有4次大的上涨）
   * 需要注意的是，即便是合理观点下的BL模型，也无法抵抗短期的下跌（例如：2015年中有3次大的下跌）
4. **Near period return as views**:

![Near period return as views](https://github.com/jrothschild33/Black-Litterman-Model/blob/master/plot/BL%20Return%20Back%20Test_Near%20period%20return%20as%20view_Year%202015.png)

   * 根据最近`VIEW_T`期的历史平均收益率作为期望收益率的观点，对市场变化很敏感
   * 有时与市场的走势相反，但是也能够快速修复
   * 在30-40周内，市场下跌时表现出了抗跌性；在40-50周内，修复速度明显高于市场速度
   * 整体表现不如**Reasonable views**观点下的BL模型

### 主要参数：在structures.py中设定
* 价格数据
  * `PRICE_FILENAME`：股票价格文件路径
  * `PRICE_SHEETNAME`：股票价格工作簿名称
* 市值数据
  * `MV_FILENAME`：股票市值文件路径
  * `MV_SHEETNAME`：股票市值工作簿名称
* 模型参数
  * `TAU`：后验期望收益率协方差矩阵的放缩尺度，取值在0~1之间
* 回测参数
  * `BACK_TEST_T`：回测时间T窗口：默认200，即利用指定索引前T期作为收益率矩阵、协方差矩阵的计算标准
  * `START_INDEX`：开始日期：2015/1/2，索引为273
  * `END_INDEX`：结束日期：2015/12/25，索引为324
  * `INDEX_NUMBER`：股指数据索引：0.标普500，1.道琼斯，2.纳斯达克
* 绘图参数
  * `BACK_TEST_X_LABEL`：'Week'
  * `BACK_TEST_Y_LABEL`：'Accumulated Return(log)'
  * `BACK_TEST_PERIOD_NAME`：'2015'
* 观点参数
  * `VIEW_TYPE`：对观点列表进行索引，取值为0、1、2、3
  * `VIEW_TYPE_NAME`：['Market value as view', "Arbitrary views", "Reasonable views", "Near period return as view"]
  * `VIEW_T` ：当观点为"Near period return as view"时，需要定义近期参数，即取VIEW_T期历史收益率求平均值，作为预期收益率

### 项目结构

![Structure](https://github.com/jrothschild33/Black-Litterman-Model/blob/master/Report/Black-Litterman-Model.png)
