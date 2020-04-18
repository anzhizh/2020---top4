# 2020-qingdao-Corporate_Credit-top4
2020-识别失信企业大赛-top4
2020识别失信企业大赛（链接：http://sdac.qingdao.gov.cn/static_page/cmpList.html）
主要基于竞赛圈开源方案进行处理，改进不多，感谢开源。复赛a榜top4，b榜top4，在赛题过拟合情况明显的同时，防过拟操作效果明显。
方案特点：
1. 经EDA发现部分离群点，将这部分离群点改值为None，避免学习到异常信息造成过拟合
2. 经EDA发现训练集和测试集部分特征分布不一致，将训练集中该部分数据的权重设置为0.01，才lightgbm构建数据集时引入该权重，降低该部分数据的影响程度，避免过拟合。
3. 发现stacking提升效果明显，利用lgb、xgb、catboost预测结果作为特征，效果明显。为提升得分可以进一步进行多模型stacking，但为了方案效率放弃堆叠模型。
4. 针对税务特征，借助w2v进行embedding，获取映射向量作为特征，但提升不明显。

后续方向：
1. 重要性较高的特征为税务特征，同时其中存在大量0（缺失值为nan）,该现象较为特殊，可以进一步挖掘。
2. 模型训练过程使用logloss，使用auc可能会降低过拟合程度。（使用auc训练早停代数较低）
3. 开源方案中使用分类特征的woe和iv作为特征，方案得分较高。提取woe和iv过程涉及标签泄露，会造成过拟合。考虑自行训练获取训练集和测试集的标签，从而保证提取过程综合考虑训练集和测试集标签信息，降低过拟。线下提升明显但线上不好，可以进一步研究。
4. 尝试借助原始特征构建唯一公司id，分析数据集中是否存在重复数据，从而实现去重后学习。可进一步尝试构建唯一id。
5. 特征构造较为简单，可进一步进行特征工程。赛题数据较为特殊，大部分特征（比例特征，统计特征，五折转化率、词频统计）都是无效的。
6. 原始数据存在大量缺失和为0情况，同时数值存在脱敏。尝试基于相关数据的进行反脱敏和规则构建，但是个人能力有限和对数据的敏感程度不足，未实现任何相关规则的发掘。
