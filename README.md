# Alpha 因子说明文档

## 📊 Alpha 因子概览

| Alpha 编号 | 表达式 | 计算步骤 | 市场含义 |
|------------|--------|----------|----------|
| Alpha #1 | `-1 * correlation(rank(delta(log(volume), 1)), rank(((close - open) / open)), 6)` | 1. 计算成交量的对数<br>2. 计算对数成交量的差分<br>3. 计算价格变化率<br>4. 对上述两个序列进行排序<br>5. 计算6日相关系数<br>6. 取负值 | 捕捉成交量变化与价格变化之间的负相关性，反映市场反转信号 |
| Alpha #2 | `-1 * delta(rank((close - open) / open + (high - low) / open + (close - low) / open + (high - close) / open)), 1)` | 1. 计算价格变化率<br>2. 计算日内波动率<br>3. 计算价格区间特征<br>4. 对综合指标进行排序<br>5. 计算一阶差分<br>6. 取负值 | 捕捉价格形态的短期变化，反映市场动量特征 |
| Alpha #3 | `-1 * correlation(open, volume, 10)` | 1. 计算开盘价与成交量的10日相关系数<br>2. 取负值 | 捕捉开盘价与成交量之间的负相关性，反映市场情绪变化 |
| Alpha #4 | `-1 * ts_rank(rank(low), 9)` | 1. 对最低价进行排序<br>2. 计算9日时间序列排序<br>3. 取负值 | 捕捉价格低点的持续性，反映市场支撑强度 |
| Alpha #5 | `(rank((open - (sum(vwap, 10) / 10))) * (-1 * abs(rank((close - vwap)))))` | 1. 计算10日VWAP均值<br>2. 计算开盘价与VWAP均值的差异<br>3. 计算收盘价与VWAP的差异<br>4. 对差异进行排序<br>5. 计算综合指标 | 捕捉价格与VWAP的偏离程度，反映市场定价效率 |
| Alpha #6 | `-1 * correlation(open, volume, 10)` | 1. 计算开盘价与成交量的10日相关系数<br>2. 取负值 | 捕捉开盘价与成交量之间的负相关性，反映市场情绪变化 |
| Alpha #7 | `((adv20 < volume) ? ((-1 * ts_rank(abs(delta(close, 7)), 60)) * sign(delta(close, 7))) : (-1 * 1))` | 1. 比较20日平均成交量与当前成交量<br>2. 计算7日价格变化<br>3. 计算60日时间序列排序<br>4. 根据条件选择信号 | 捕捉高成交量下的价格趋势，反映市场动量特征 |
| Alpha #8 | `-1 * rank(((sum(open, 5) * sum(returns, 5)) - delay((sum(open, 5) * sum(returns, 5)), 10)))` | 1. 计算5日开盘价和收益率之和<br>2. 计算10日延迟值<br>3. 计算差异并排序<br>4. 取负值 | 捕捉价格和收益率的短期变化，反映市场趋势强度 |
| Alpha #9 | `((0 < ts_min(delta(close, 1), 5)) ? delta(close, 1) : ((ts_max(delta(close, 1), 5) < 0) ? delta(close, 1) : (-1 * delta(close, 1))))` | 1. 计算价格变化<br>2. 判断5日最小和最大变化<br>3. 根据条件选择信号 | 捕捉价格趋势的持续性，反映市场动量特征 |
| Alpha #10 | `rank(((0 < ts_min(delta(close, 1), 4)) ? delta(close, 1) : ((ts_max(delta(close, 1), 4) < 0) ? delta(close, 1) : (-1 * delta(close, 1)))))` | 1. 计算价格变化<br>2. 判断4日最小和最大变化<br>3. 根据条件选择信号<br>4. 对结果进行排序 | 捕捉价格趋势的持续性，反映市场动量特征 |

## 🔍 因子特点

- **计算频率**：日频
- **数据要求**：OHLCV数据
- **适用市场**：股票市场
- **信号类型**：多空信号

## 📈 使用建议

1. 建议结合其他技术指标使用
2. 注意控制换手率
3. 考虑市场环境的影响
4. 定期进行因子有效性检验

## ⚠️ 注意事项

- 因子计算可能存在未来数据泄露
- 需要根据实际情况调整参数
- 建议进行充分的回测验证
- 注意控制交易成本

## 📚 参考文献

1. 《量化投资策略与技术》
2. 《因子投资：方法与实践》
3. 《量化交易之路》

## Alpha因子明细表（1-10）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#1 | rank(Ts_ArgMax(SignedPower(((returns < 0) ? stddev(returns, 20) : close), 2.), 5)) -0.5 | 1. 当收益率为负时，使用20日收益率标准差（波动率）作为信号<br>2. 当收益率为正时，使用收盘价作为信号<br>3. 对信号取平方，计算过去5天最大值位置<br>4. 对最大值位置横截面排名并减0.5 | 捕捉价格波动和趋势交互，波动大用波动率，趋势明显用价格 |
| Alpha#2 | -1 * correlation(rank(delta(log(volume), 2)), rank(((close - open) / open)), 6) | 1. 计算成交量对数的2日变化并横截面排名<br>2. 计算价格变化率并横截面排名<br>3. 计算6日滚动相关系数<br>4. 取负值 | 捕捉成交量变化与价格变化的负相关，预示市场反转 |
| Alpha#3 | -1 * correlation(rank(open), rank(volume), 10) | 1. 对开盘价、成交量横截面排名<br>2. 计算10日滚动相关系数<br>3. 取负值 | 捕捉开盘价与成交量负相关，反映市场情绪变化 |
| Alpha#4 | -1 * Ts_Rank(rank(low), 9) | 1. 对最低价横截面排名<br>2. 9日时间序列排名<br>3. 取负值 | 捕捉价格支撑位强度 |
| Alpha#5 | (rank((open - (sum(vwap, 10) / 10))) * (-1 * abs(rank((close - vwap))))) | 1. 计算开盘价与10日VWAP均值差并排名<br>2. 计算收盘价与VWAP差绝对值并排名<br>3. 两者相乘取负值 | 结合开盘价与VWAP、收盘价与VWAP，捕捉价格偏离均值 |
| Alpha#6 | -1 * correlation(open, volume, 10) | 1. 计算开盘价与成交量10日滚动相关系数<br>2. 取负值 | 捕捉开盘价与成交量负相关，反映开盘交易活跃度异常 |
| Alpha#7 | ((adv20 < volume) ? ((-1 * ts_rank(abs(delta(close, 7)), 60)) * sign(delta(close, 7))) : (-1* 1)) | 1. 比较20日均量与当前成交量<br>2. 若放量，计算收盘价7日变化绝对值60日排名，乘以方向<br>3. 若缩量，因子为-1 | 结合成交量和价格动量，放量时捕捉趋势，缩量时固定负信号 |
| Alpha#8 | -1 * rank(((sum(open, 5) * sum(returns, 5)) - delay((sum(open, 5) * sum(returns, 5)),10))) | 1. 计算开盘价5日和与收益率5日和的乘积<br>2. 计算10日滞后值<br>3. 差值横截面排名取负值 | 捕捉价格趋势加速或减速，短期动量强于长期时因子大 |
| Alpha#9 | ((0 < ts_min(delta(close, 1), 5)) ? delta(close, 1) : ((ts_max(delta(close, 1), 5) < 0) ? delta(close, 1) : (-1 * delta(close, 1)))) | 1. 计算收盘价日变化<br>2. 计算5日最小/最大变化<br>3. 条件判断决定信号方向 | 趋势上升/下降时保持信号，不明时反转，捕捉动量持续性 |
| Alpha#10 | rank(max(delta(close, 1), 5) * -1) | 1. 计算收盘价日变化<br>2. 5日最大值取负<br>3. 横截面排名 | 捕捉价格下跌动量，预示下跌趋势持续 |

## Alpha因子明细表（11-20）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#11 | (rank(ts_max((vwap - close), 3)) + rank(ts_min((vwap - close), 3))) * rank(delta(volume, 3)) | 1. 计算VWAP与收盘价差值，取3日最大/最小值并排名<br>2. 计算成交量3日变化并排名<br>3. 两者相加后相乘 | 结合VWAP与收盘价偏离和成交量变化，预示趋势转变 |
| Alpha#12 | sign(delta(volume, 1)) * (-1 * delta(close, 1)) | 1. 计算成交量日变化方向<br>2. 计算收盘价日变化取负<br>3. 两者相乘 | 捕捉成交量与价格背离，预示趋势转变 |
| Alpha#13 | (-1 * rank(covariance(rank(close), rank(volume), 5))) | 1. 收盘价、成交量横截面排名<br>2. 计算5日滚动协方差<br>3. 协方差排名取负 | 捕捉价格与成交量负相关，预示超买超卖 |
| Alpha#14 | ((-1 * rank(delta(returns, 3))) * correlation(open, volume, 10)) | 1. 计算收益率3日变化排名取负<br>2. 计算开盘价与成交量10日相关系数<br>3. 两者相乘 | 结合收益率动量和开盘价-成交量相关性，预示转折点 |
| Alpha#15 | (-1 * sum(rank(correlation(rank(high), rank(volume), 3)), 3)) | 1. 最高价、成交量横截面排名<br>2. 3日相关系数并排名<br>3. 3日总和取负 | 捕捉最高价与成交量负相关，预示超买 |
| Alpha#16 | (-1 * rank(covariance(rank(high), rank(volume), 5))) | 1. 最高价、成交量横截面排名<br>2. 5日滚动协方差<br>3. 协方差排名取负 | 捕捉最高价与成交量负相关，预示超买 |
| Alpha#17 | (((rank(high) - min(rank(high), 15)) < rank(correlation(vwap, delay(close, 5), 5))) ? -1 : 1) | 1. 最高价排名与15日最小值差<br>2. VWAP与5日滞后收盘价5日相关系数排名<br>3. 条件判断 | 结合价格动量和相关性，动量减弱且相关性高时为负 |
| Alpha#18 | (close - open) / ((high - low) + 0.001) | 1. 计算收盘价与开盘价差<br>2. 计算最高-最低价差<br>3. 相除 | 衡量价格波动效率，反映趋势强度 |
| Alpha#19 | ((close < delay(close, 1)) ? -1 * delta(close, 1) : ((close = delay(close, 1)) ? 0 : delta(close, 1))) | 1. 比较收盘价与昨日收盘价<br>2. 条件决定信号方向 | 捕捉价格动量，上涨为正，下跌为负 |
| Alpha#20 | (-1 * rank(((close - low) - (high - close)) / (high - low) * volume)) | 1. 计算收盘-最低与最高-收盘差<br>2. 除以价格区间乘成交量<br>3. 横截面排名取负 | 结合价格位置和成交量，反映盘整状态 |

## Alpha因子明细表（21-30）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#21 | ((((sum(close, 8) / 8) + stddev(close, 8)) < (sum(close, 2) / 2)) ? (-1 * 1) : (((sum(close, 2) / 2) < ((sum(close, 8) / 8) - stddev(close, 8))) ? 1 : (((1 < (volume / mean(volume,20))) || ((volume / mean(volume,20)) == 1)) ? 1 : (-1 * 1)))) | 1. 计算收盘价8日均值和标准差<br>2. 计算收盘价2日均值<br>3. 计算成交量与20日均值比值<br>4. 多条件判断决定信号 | 结合价格趋势和成交量，趋势明显且放量为正，否则为负 |
| Alpha#22 | (-1 * (delta(correlation(high, volume, 5), 5) * rank(stddev(close, 20)))) | 1. 计算最高价与成交量5日相关系数<br>2. 计算相关系数5日变化<br>3. 计算收盘价20日标准差并排名<br>4. 两者相乘取负 | 结合波动性和价格-成交量相关性，预示转折点 |
| Alpha#23 | (((sum(high, 20) / 20) < high) ? (-1 * delta(high, 2)) : 0) | 1. 计算最高价20日均值<br>2. 比较当前最高价与均值<br>3. 若高于均值，取2日变化负值，否则为0 | 捕捉价格突破后的反转，预示回归均值 |
| Alpha#24 | ((((delta((sum(close, 100) / 100), 100) / delay(close, 100)) < 0.05) || ((delta((sum(close, 100) / 100), 100) / delay(close, 100)) == 0.05)) ? (-1 * (close - delay(close, 1))) : (-1 * delta(close, 3)))) | 1. 计算收盘价100日均值及其100日变化率<br>2. 判断变化率是否小于等于5%<br>3. 若是，取日变化负值，否则取3日变化负值 | 结合长期趋势和短期动量，趋势不明显用短动量，趋势明显用中动量 |
| Alpha#25 | rank(((((-1 * returns) * adv20) * vwap) * (high - close))) | 1. 计算负收益率<br>2. 乘以20日均量、VWAP、最高-收盘价差<br>3. 横截面排名 | 结合收益率、成交量和价格区间，预示超卖 |
| Alpha#26 | ((((sum(close, 7) / 7) - open) * ((high - low) / (sum(close, 7) / 7))) * rank(correlation(vwap, delay(close, 5), 230))) | 1. 计算收盘价7日均值与开盘价差<br>2. 计算价格区间与均值比值<br>3. VWAP与5日滞后收盘价230日相关系数排名<br>4. 三者相乘 | 结合价格趋势、区间和相关性，预示趋势持续 |
| Alpha#27 | ((0.5 < rank((mean(correlation(rank(volume), rank(vwap), 6), 2) / rank(mean(correlation(rank(vwap), rank(volume), 2), 3))))) ? (-1 * 1) : 1) | 1. 计算成交量排名与VWAP排名6日相关系数2日均值<br>2. VWAP排名与成交量排名2日相关系数3日均值<br>3. 两者排名相除并判断<br>4. 比值大于0.5为-1，否则为1 | 捕捉成交量与VWAP相关性变化，相关性大反向信号 |
| Alpha#28 | scale(((correlation(adv20, low, 5) + ((high + low) / 2)) - close)) | 1. 计算20日均量与最低价5日相关系数<br>2. 计算最高与最低均值<br>3. 两者相加减去收盘价<br>4. 结果缩放 | 结合成交量-价格相关性和价格位置，反映盘整 |
| Alpha#29 | (min(product(rank(rank(scale(log(sum(ts_min(rank(rank(-1 * rank(delta((close - 1), 5)))), 2), 1))))), 1), 5) + ts_rank(delay((-1 * returns), 6), 5)) | 1. 计算收盘价-1的5日变化，取负两次排名<br>2. 2日最小值，对数缩放两次排名<br>3. 5日最小值<br>4. 计算负收益率6日滞后5日排名<br>5. 两者相加 | 结合价格动量和收益率动量，动量强时因子大 |
| Alpha#30 | (((1.0 - rank(((sign((close - delay(close, 1))) + sign((delay(close, 1) - delay(close, 2)))) + sign((delay(close, 2) - delay(close, 3)))))) * sum(volume, 5)) / sum(volume, 20)) | 1. 计算收盘价连续三日变化方向<br>2. 方向序列排名<br>3. 计算5日、20日成交量总和<br>4. 排名乘5日成交量再除以20日成交量 | 结合价格趋势和成交量比率，趋势明显且短期量大时因子大 |

## Alpha因子明细表（31-40）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#31 | (rank(rank(rank(decay_linear((-1 * rank(rank(delta(close, 10)))), 10)))) + rank((-1 * delta(close, 3)))) + sign(scale(correlation(adv20, low, 12))) | 1. 计算收盘价10日变化，两次排名，10日线性衰减，三次排名<br>2. 计算收盘价3日变化取负并排名<br>3. 计算20日均量与最低价12日相关系数缩放取符号<br>4. 三者相加 | 结合价格动量、变化和成交量-价格相关性，动量强时因子大 |
| Alpha#32 | (scale(((sum(close, 7) / 7) - open)) * ((high - low) / (sum(close, 7) / 7))) | 1. 计算收盘价7日均值与开盘价差并缩放<br>2. 计算价格区间与均值比值<br>3. 两者相乘 | 结合价格趋势和区间，趋势明显且区间大时因子大 |
| Alpha#33 | rank((-1 * ((1 - (open / close))^1))) | 1. 计算开盘价与收盘价比值，1减去比值取负<br>2. 横截面排名 | 捕捉日内价格走势，开盘高于收盘时因子大 |
| Alpha#34 | mean(abs(close - open), 12) + (close - open) + correlation(close, open, 12) | 1. 计算收盘-开盘差绝对值12日均值<br>2. 计算收盘-开盘差<br>3. 计算收盘与开盘12日相关系数<br>4. 三者相加 | 结合波动性、方向和相关性，三者强时因子大 |
| Alpha#35 | (ts_rank(volume / mean(volume,20), 20) * ts_rank((-1 * delta(close, 7)), 8)) | 1. 计算成交量与20日均值比值20日排名<br>2. 计算收盘价7日变化取负8日排名<br>3. 两者相乘 | 结合成交量相对强度和价格动量，动量强时因子大 |
| Alpha#36 | (((2.21 * rank(correlation((close - open), delay(volume, 1), 15))) + (0.7 * rank((open - close)))) + (0.73 * rank(ts_rank(delay((-1 * returns), 6), 5)))) + rank(abs(correlation(vwap, adv20, 6))) | 1. 计算收盘-开盘与滞后成交量15日相关系数排名乘2.21<br>2. 计算开盘-收盘排名乘0.7<br>3. 计算负收益率6日滞后5日排名乘0.73<br>4. 计算VWAP与20日均量6日相关系数绝对值排名<br>5. 四者相加 | 结合多指标，趋势明显时因子大 |
| Alpha#37 | (rank(correlation(delay((open - close), 1), close, 200)) + rank((open - close))) | 1. 计算开盘-收盘差1日滞后与收盘价200日相关系数排名<br>2. 计算开盘-收盘差排名<br>3. 两者相加 | 结合历史相关性和当前价格方向，趋势强时因子大 |
| Alpha#38 | ((-1 * rank(ts_rank(close, 10))) * rank((close / open))) | 1. 计算收盘价10日时间序列排名并横截面排名取负<br>2. 计算收盘/开盘比值排名<br>3. 两者相乘 | 结合价格动量和方向，动量弱且方向向下时因子大 |
| Alpha#39 | ((-1 * rank((delta(close, 7) * (1 - rank(decay_linear((volume / adv20), 9)))))) * (1 + rank(sum(returns, 250)))) | 1. 计算成交量与20日均值比值9日线性衰减排名<br>2. 计算收盘价7日变化乘(1-排名)<br>3. 横截面排名取负<br>4. 计算收益率250日总和排名加1<br>5. 两者相乘 | 结合价格动量、成交量衰减和长期收益率，预示转折点 |
| Alpha#40 | ((-1 * rank(stddev(high, 10))) * correlation(close, volume, 10)) | 1. 计算最高价10日标准差排名取负<br>2. 计算收盘价与成交量10日相关系数<br>3. 两者相乘 | 结合波动性和价格-成交量相关性，预示转折点 |

## Alpha因子明细表（41-50）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#41 | (((high * low)^0.5) - vwap) | 1. 计算最高价与最低价的几何平均值<br>2. 从几何平均值中减去VWAP | 捕捉价格区间与成交加权均价的差异，区间大且高于VWAP时因子大 |
| Alpha#42 | (rank((vwap - close)) / rank((vwap + close))) | 1. 计算VWAP与收盘价差值并排名<br>2. 计算VWAP与收盘价和并排名<br>3. 两者相除 | 捕捉价格相对VWAP的偏离，价格低于VWAP时因子大 |
| Alpha#43 | (ts_rank((volume / adv20), 20) * ts_rank((-1 * delta(close, 7)), 8)) | 1. 计算成交量与20日均值比值20日排名<br>2. 计算收盘价7日变化取负8日排名<br>3. 两者相乘 | 结合成交量相对强度和价格动量，动量强时因子大 |
| Alpha#44 | (-1 * correlation(high, rank(volume), 5)) | 1. 对成交量排名<br>2. 计算最高价与成交量排名5日相关系数<br>3. 取负值 | 捕捉价格高位与成交量负相关，预示超买 |
| Alpha#45 | (-1 * ((rank((sum(delay(close, 5), 20) / 20)) * correlation(close, volume, 2)) * rank(correlation(sum(close, 5), sum(close, 20), 2)))) | 1. 计算收盘价5日滞后值20日总和/20并排名<br>2. 计算收盘价与成交量2日相关系数<br>3. 计算收盘价5日总和与20日总和2日相关系数并排名<br>4. 三者相乘取负 | 结合价格趋势、价格-成交量相关性和动量，预示转折点 |
| Alpha#46 | ((0.25 < (((delay(close, 20) - delay(close, 10)) / 10) - ((delay(close, 10) - close) / 10))) ? (-1 * 1) : (((((delay(close, 20) - delay(close, 10)) / 10) - ((delay(close, 10) - close) / 10)) < 0) ? 1 : ((-1 * 1) * (close - delay(close, 1))))) | 1. 计算收盘价20日、10日滞后值和当前值<br>2. 计算两个时间段平均变化率<br>3. 条件判断：大于0.25为-1，小于0为1，否则取日变化负值 | 捕捉价格趋势加速度，趋势加速或减速时给出信号 |
| Alpha#47 | ((((rank((1 / close)) * volume) / mean(volume,20)) * ((high * rank((high - close))) / (mean(high, 5) / mean(close, 5)))) | 1. 计算收盘价倒数排名乘成交量/20日均量<br>2. 计算最高价与(最高-收盘)排名乘积/5日均值比<br>3. 两者相乘 | 结合价格、成交量和区间，价格低、量大、区间大时因子大 |
| Alpha#48 | (indneutralize(((correlation(delta(close, 1), delta(delay(close, 1), 1), 3) * ((close - open) / open)) * volume), adv20) / sum(((close - open) / open) * volume, 20)) | 1. 计算收盘价日变化与1日滞后变化3日相关系数<br>2. 计算收盘-开盘变化率乘成交量<br>3. 行业中性化处理<br>4. 计算变化率乘成交量20日总和<br>5. 两者相除 | 结合价格动量、变化率和成交量，动量强且量大时因子大 |
| Alpha#49 | (((((delay(close, 20) - delay(close, 10)) / 10) - ((delay(close, 10) - close) / 10)) < (-1 * 0.1)) ? 1 : ((-1 * 1) * (close - delay(close, 1)))) | 1. 计算收盘价20日、10日滞后值和当前值<br>2. 计算两个时间段平均变化率<br>3. 条件判断：小于-0.1为1，否则取日变化负值 | 捕捉价格趋势加速度，趋势显著减速时正向信号 |
| Alpha#50 | ((rank((1 / close)) * volume) / mean(volume,20)) | 1. 计算收盘价倒数排名乘成交量<br>2. 除以20日均量 | 结合价格和成交量，价格低且量大时因子大 |

## Alpha因子明细表（51-60）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#51 | ((((delay(close, 20) - delay(close, 10)) / 10) - ((delay(close, 10) - close) / 10)) < (-1 * 0.05)) ? 1 : ((-1 * 1) * (close - delay(close, 1)))) | 1. 计算收盘价20日、10日滞后值和当前值<br>2. 计算两个时间段平均变化率<br>3. 条件判断：小于-0.05为1，否则取日变化负值 | 捕捉价格趋势加速度，趋势显著减速时正向信号 |
| Alpha#52 | (((-1 * ts_min(low, 5)) + delay(ts_min(low, 5), 5)) * rank(((sum(returns, 240) - sum(returns, 20)) / 220))) * ts_rank(volume, 5) | 1. 计算最低价5日最小值及5日滞后值<br>2. 差值<br>3. 计算收益率240日和、20日和，差值/220并排名<br>4. 成交量5日时间序列排名<br>5. 三者相乘 | 结合价格支撑、长期收益率和成交量，反映上涨趋势 |
| Alpha#53 | (-1 * delta((((close - low) - (high - close)) / (close - low)), 9)) | 1. 计算收盘-最低、最高-收盘差值<br>2. 两者差/收盘-最低<br>3. 9日变化取负 | 捕捉价格区间内位置变化，位置下降时因子大 |
| Alpha#54 | ((-1 * ((low - close) * (open^5))) / ((low - high) * (close^5))) | 1. 计算最低-收盘、开盘5次方乘积<br>2. 计算最低-最高、收盘5次方乘积<br>3. 两者相除取负 | 捕捉区间位置与价格水平关系，低位高价时因子大 |
| Alpha#55 | (-1 * correlation(rank(((close - ts_min(low, 12)) / (ts_max(high, 12) - ts_min(low, 12)))), rank(volume), 6)) | 1. 计算最低12日最小、最高12日最大<br>2. 收盘-最小/最大-最小，排名<br>3. 成交量排名<br>4. 两者6日相关系数取负 | 捕捉区间位置与成交量负相关，预示超买 |
| Alpha#56 | (0 - (1 * (rank((sum(returns, 10) / sum(sum(returns, 2), 3))) * rank((returns * cap))))) | 1. 计算收益率10日和、2日和、3日和<br>2. 10日和/3日和排名<br>3. 收益率*市值排名<br>4. 两者相乘取负 | 结合收益率动量和市值，动量弱市值大时因子大 |
| Alpha#57 | ((close - vwap) / decay_linear(rank(ts_argmax(close, 30)), 2)) | 1. 计算收盘-VWAP差<br>2. 收盘30日最大值位置排名<br>3. 2日线性衰减<br>4. 差值/衰减结果 | 捕捉价格偏离VWAP与高点关系，低于VWAP且远离高点时因子大 |
| Alpha#58 | (-1 * ts_rank(decay_linear(correlation(IndNeutralize(vwap, IndClass.sector), IndNeutralize(volume, IndClass.sector), 3.92795), 7.89291), 5.50322)) | 1. VWAP、成交量行业中性化<br>2. 3.93日相关系数<br>3. 7.89日线性衰减<br>4. 5.5日时间序列排名取负 | 捕捉行业中性化后价格-成交量相关性，负相关时因子大 |
| Alpha#59 | (-1 * ts_rank(decay_linear(correlation(IndNeutralize(((vwap * 0.728317) + (vwap * (1 - 0.728317))), IndClass.industry), IndNeutralize(volume, IndClass.industry), 4.25137), 16.2289), 8.19648)) | 1. VWAP加权组合行业中性化<br>2. 成交量行业中性化<br>3. 4.25日相关系数<br>4. 16.2日线性衰减<br>5. 8.19日时间序列排名取负 | 捕捉行业中性化后价格-成交量相关性，负相关时因子大 |
| Alpha#60 | ((0 - (1 * ((2 * scale(rank(((((close - low) - (high - close)) / (high - low)) * volume)))) - scale(rank(ts_argmax(close, 10)))))) * -1) | 1. 计算收盘-最低、最高-收盘差值/区间*成交量，排名缩放*2<br>2. 收盘10日最大值位置排名缩放<br>3. 两者相减取负 | 结合区间位置、成交量和高点，低位量大远离高点时因子大 |

## Alpha因子明细表（61-70）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#61 | (rank(decay_linear(delta(vwap, 3), 7)) + rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 7))) * -1) | 1. 计算VWAP的3日差分，7日线性衰减并排名<br>2. 计算加权价格（收盘、最低、开盘加权平均）3日差分等，7日线性衰减并排名<br>3. 两者相加取负 | 捕捉价格与成交量关系，反映趋势延续或超买 |
| Alpha#62 | ((rank(correlation(vwap, sum(adv20, 22), 10)) < rank(((rank(open) + rank(open)) < (rank(((high + low) / 2)) + rank(high))))) * -1) | 1. 计算VWAP与22日均量10日相关系数排名<br>2. 计算开盘价、最高价、最低价排名及组合<br>3. 比较两个排名大小，乘以-1 | 捕捉价格与成交量关系，负相关时因子大 |
| Alpha#63 | ((rank(decay_linear(correlation(sum(open, 5), sum(adv20, 5), 5), 2)) < rank(decay_linear(((((high + low) / 2) * 0.2) + (vwap * (1 - 0.2))), 5))) * -1) | 1. 计算开盘价5日和、20日均量5日和，5日相关系数2日线性衰减并排名<br>2. 计算高低均值*0.2+VWAP*0.8，5日线性衰减并排名<br>3. 比较两个排名大小，乘以-1 | 捕捉价格与成交量关系，负相关时因子大 |
| Alpha#64 | (rank(correlation(sum(((open * 0.178404) + (low * (1 - 0.178404))), 12), sum(adv20, 12), 12)) < rank(correlation(rank(((high + low) / 2)), rank(volume), 6))) | 1. 计算开盘价与最低价加权平均12日和、20日均量12日和，12日相关系数排名<br>2. 计算高低均值排名、成交量排名，6日相关系数排名<br>3. 比较两个排名大小 | 捕捉价格与成交量关系，负相关时因子大 |
| Alpha#65 | (rank(correlation(sum(((open * 0.178404) + (low * (1 - 0.178404))), 12), sum(adv20, 12), 12)) < rank(correlation(rank(((high + low) / 2)), rank(volume), 6))) | 1. 同Alpha#64 | 同Alpha#64 |
| Alpha#66 | ((rank(decay_linear(delta(vwap, 3), 7)) + rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 7))) * -1) | 1. 计算VWAP的3日差分，7日线性衰减并排名<br>2. 计算加权价格（收盘、最低、开盘加权平均）3日差分等，7日线性衰减并排名<br>3. 两者相加取负 | 捕捉价格与成交量关系，反映趋势延续或超买 |
| Alpha#67 | ((rank((high - ts_min(high, 5))) * rank(correlation(adv20, volume, 5))) * -1) | 1. 计算最高价5日最小值，差值排名<br>2. 20日均量与成交量5日相关系数排名<br>3. 两者相乘取负 | 捕捉高位价格与成交量负相关，反映超买 |
| Alpha#68 | ((ts_rank(correlation(rank(high), rank(adv20), 5), 5) * rank(delta(((close * 0.518371) + (low * (1 - 0.518371))), 5))) * -1) | 1. 最高价排名、20日均量排名，5日相关系数5日排名<br>2. 收盘与最低加权平均5日差分排名<br>3. 两者相乘取负 | 捕捉高位价格与趋势向下，反映超买 |
| Alpha#69 | ((rank(ts_max(delta(close, 3), 5)) * rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 5))) * -1) | 1. 计算收盘3日差分5日最大值排名<br>2. 计算加权价格3日差分等，5日线性衰减并排名<br>3. 两者相乘取负 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#70 | ((rank(decay_linear(correlation(((low * 0.967285) + (low * (1 - 0.967285))), adv20, 6), 5)) + rank(decay_linear(delta(vwap, 3), 5))) * -1) | 1. 计算最低价加权平均与20日均量6日相关系数5日线性衰减并排名<br>2. VWAP3日差分5日线性衰减并排名<br>3. 两者相加取负 | 捕捉价格与成交量正相关，反映趋势延续 |

## Alpha因子明细表（71-80）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#71 | max(rank(decay_linear(correlation(ts_rank(close, 5), ts_rank(adv20, 5), 5), 3)), rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 3))) | 1. 计算收盘价5日滚动排名和20日均量5日滚动排名，5日相关系数3日线性衰减并排名<br>2. 计算加权价格（收盘、最低、开盘加权平均）3日差分等，3日线性衰减并排名<br>3. 取两者最大值 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#72 | (rank(decay_linear(correlation(((high + low) / 2), adv20, 8), 3)) / rank(decay_linear(correlation(ts_rank(vwap, 4), ts_rank(volume, 18), 6), 2))) | 1. 计算高低均值与20日均量8日相关系数3日线性衰减并排名<br>2. VWAP和成交量分别4日、18日滚动排名，6日相关系数2日线性衰减并排名<br>3. 两者相除 | 捕捉价格与成交量正相关，趋势延续时因子大 |
| Alpha#73 | (max(rank(decay_linear(delta(vwap, 4), 2)), rank(decay_linear(((delta(((open * 0.147155) + (low * (1 - 0.147155))), 3) / ((open * 0.147155) + (low * (1 - 0.147155)))) * -1), 3))) * -1) | 1. 计算VWAP 4日差分2日线性衰减并排名<br>2. 计算开盘与最低加权平均3日差分/自身，乘-1，3日线性衰减并排名<br>3. 取两者最大值乘-1 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#74 | ((rank(correlation(close, sum(adv20, 15), 6), 20) < rank(((open + close) - (vwap + open)))) * -1) | 1. 计算20日均量15日总和，收盘价与总和6日相关系数20日排名<br>2. 计算开盘+收盘与VWAP+开盘的差值排名<br>3. 比较两个排名大小，乘-1 | 捕捉价格与成交量负相关，超买时因子大 |
| Alpha#75 | (rank(decay_linear(delta(vwap, 1), 3)) + rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 3))) * -1) | 1. 计算VWAP 1日差分3日线性衰减并排名<br>2. 计算加权价格3日差分等，3日线性衰减并排名<br>3. 两者相加乘-1 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#76 | (rank(decay_linear(correlation(((high + low) / 2), adv20, 3), 3)) / rank(decay_linear(correlation(rank(vwap), rank(volume), 18), 3))) | 1. 计算高低均值与20日均量3日相关系数3日线性衰减并排名<br>2. VWAP和成交量排名18日相关系数3日线性衰减并排名<br>3. 两者相除 | 捕捉价格与成交量正相关，趋势延续时因子大 |
| Alpha#77 | ((rank(correlation(sum(((high + low) / 2), 19), sum(adv20, 19), 8)) < rank(correlation(rank(low), rank(adv20), 8))) * -1) | 1. 计算高低均值19日总和与20日均量19日总和，8日相关系数排名<br>2. 最低价和20日均量排名8日相关系数排名<br>3. 比较两个排名大小，乘-1 | 捕捉价格与成交量负相关，超买时因子大 |
| Alpha#78 | (0 - (1 * (((1.5 * scale(indneutralize(indneutralize(rank(((((close - low) - (high - close)) / (high - low)) * volume)), IndClass.subindustry), IndClass.subindustry))) < scale(indneutralize((correlation(close, rank(adv20), 5) - ((high + low) / 5)), IndClass.subindustry))) * (volume / adv20)))) | 1. 计算区间位置乘成交量，行业中性化两次，排名*1.5标准化<br>2. 收盘价与成交量排名5日相关系数-高低均值，行业中性化标准化<br>3. 比较两个结果大小，乘成交量/20日均量，再乘-1 | 捕捉区间低位且量大，超卖时因子大 |
| Alpha#79 | ((close - open) / ((high - low) + .001)) | 1. 计算收盘-开盘差<br>2. 计算高-低差<br>3. 两者相除 | 捕捉价格区间内位置，区间高位时因子大 |
| Alpha#80 | (rank(decay_linear(correlation(((high + low) / 2), adv20, 3), 3)) / rank(decay_linear(correlation(rank(vwap), rank(volume), 18), 3))) | 1. 计算高低均值与20日均量3日相关系数3日线性衰减并排名<br>2. VWAP和成交量排名18日相关系数3日线性衰减并排名<br>3. 两者相除 | 捕捉价格与成交量正相关，趋势延续时因子大 |

## Alpha因子明细表（81-88）

| Alpha编号 | 表达式 | 计算步骤 | 市场含义 |
|-----------|--------|----------|----------|
| Alpha#81 | (rank(decay_linear(delta(vwap, 1), 3)) + rank(decay_linear(((((delta(((((close * 0.518371) + (low * (1 - 0.518371))) / 2) * 2), 3) + ((close - low) * 0.518371)) + ((open * 0.518371) + (low * (1 - 0.518371)))) / 2), 3))) * -1) | 1. 计算VWAP 1日差分3日线性衰减并排名<br>2. 计算加权价格3日差分等，3日线性衰减并排名<br>3. 两者相加乘-1 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#82 | (rank(decay_linear(delta(open, 1), 3)) + rank(decay_linear(correlation(rank(open), rank(adv20), 12), 3))) * -1) | 1. 计算开盘价1日差分3日线性衰减并排名<br>2. 开盘价和20日均量排名12日相关系数3日线性衰减并排名<br>3. 两者相加乘-1 | 捕捉价格与成交量负相关，超买时因子大 |
| Alpha#83 | ((rank(delay(((high - low) / (sum(close, 5) / 5)), 2)) * rank(rank(volume))) / (((high - low) / (sum(close, 5) / 5)) / (vwap - close))) | 1. 计算高-低/5日收盘均值，2日延迟并排名<br>2. 成交量两次排名，相乘<br>3. 计算高-低/5日收盘均值/(VWAP-收盘)，两者相除 | 捕捉价格区间与成交量关系，区间大且量大时因子大 |
| Alpha#84 | (rank(power(ts_rank((vwap - ts_max(vwap, 15)), 5), delta(close, 5))) * -1) | 1. 计算VWAP 15日最大值，差值5日排名<br>2. 收盘价5日差分为指数，排名为底数，计算幂并排名<br>3. 乘-1 | 捕捉价格趋势持续性，趋势向上时因子大 |
| Alpha#85 | (rank(correlation(((high * 0.876703) + (close * (1 - 0.876703))), adv20, 9)) / rank(correlation(rank(((high * 0.876703) + (close * (1 - 0.876703)))), rank(volume), 6))) | 1. 计算高收加权平均与20日均量9日相关系数排名<br>2. 高收加权平均排名与成交量排名6日相关系数排名<br>3. 两者相除 | 捕捉价格与成交量正相关，趋势延续时因子大 |
| Alpha#86 | ((ts_rank(correlation(close, sum(adv20, 15), 6), 20) < rank(((open + close) - (vwap + open)))) * -1) | 1. 计算20日均量15日总和，收盘价与总和6日相关系数20日排名<br>2. 计算开盘+收盘与VWAP+开盘的差值排名<br>3. 比较两个排名大小，乘-1 | 捕捉价格与成交量负相关，超买时因子大 |
| Alpha#87 | (max(rank(decay_linear(delta(((close * 0.369701) + (vwap * (1 - 0.369701))), 1), 3)), rank(decay_linear(abs(correlation(rank(adv20), rank(close), 6)), 3))) * -1) | 1. 计算收盘与VWAP加权平均1日差分3日线性衰减并排名<br>2. 20日均量和收盘价排名6日相关系数绝对值3日线性衰减并排名<br>3. 取两者最大值乘-1 | 捕捉价格与成交量负相关，超买时因子大 |
| Alpha#88 | (min(rank(decay_linear(((((high + low) / 2) + high) - (vwap + high)), 3)), rank(decay_linear(correlation(((high + low) / 2), adv20, 12), 3))) * -1) | 1. 计算高低均值+高-VWAP+高，3日线性衰减并排名<br>2. 高低均值与20日均量12日相关系数3日线性衰减并排名<br>3. 取两者最小值乘-1 | 捕捉区间高位且量大，超买时因子大 |

---
*最后更新：2024年3月* 