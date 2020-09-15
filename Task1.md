# Task1 	赛题理解



本次比赛是Datawhale与天池联合发起的0基础入门系列赛事第四场 —— 零基础入门金融风控之贷款违约预测
挑战赛。  

赛题以金融风控中的个人信贷为背景，要求选手根据贷款申请人的数据信息预测其是否有违约的可能，以此判
断是否通过此项贷款，这是一个典型的分类问题。  



## 数据

数据来自某信贷平台的贷款记录，总数据量超过120w，包含47列变量信息，其中15列为匿名变量。

### 数据集划分 

训练集80万条，测试集A 20万条 ，测试集B 20万条 

### 数据注意事项

47列变量中，为了保护隐私，有一些变量进行了脱敏操作

### 变量命名

#### train.csv

\1. **id** 为贷款清单分配的唯一信用证标识
\2. **loanAmnt** 贷款金额
\3. **term** 贷款期限（year）
\4. **interestRate** 贷款利率
\5. **installment** 分期付款金额
\6. **grade** 贷款等级
\7. **subGrade** 贷款等级之子级
\8. **employmentTitle**就业职称
\9. **employmentLength** 就业年限（年）
\10. **homeOwnership** 借款人在登记时提供的房屋所有权状况
\11. **annualIncome** 年收入
\12. **verificationStatus** 验证状态
\13. **issueDate** 贷款发放的月份
\14. **purpose** 借款人在贷款申请时的贷款用途类别
\15. **postCode** 借款人在贷款申请中提供的邮政编码的前3位数字
\16. **regionCode** 地区编码
\17. **dti** 债务收入比
\18. **delinquency_2years** 借款人过去2年信用档案中逾期30天以上的违约事件数
\19. **ficoRangeLow** 借款人在贷款发放时的fico所属的下限范围
\20. **ficoRangeHigh** 借款人在贷款发放时的fico所属的上限范围
\21. **openAcc** 借款人信用档案中未结信用额度的数量
\22. **pubRec** 贬损公共记录的数量
\23. **pubRecBankruptcies** 公开记录清除的数量
\24. **revolBal** 信贷周转余额合计
\25. **revolUtil** 循环额度利用率，或借款人使用的相对于所有可用循环信贷的信贷金额
\26. **totalAcc** 借款人信用档案中当前的信用额度总数
\27. **initialListStatus** 贷款的初始列表状态
\28. **applicationType** 表明贷款是个人申请还是与两个共同借款人的联合申请
\29. **earliesCreditLine** 借款人最早报告的信用额度开立的月份
\30. **title** 借款人提供的贷款名称
\31. **policyCode** 公开可用的策略代码=1新产品不公开可用的策略代码=2
\32. **n**系列匿名特征 匿名特征n0-n14，为一些贷款人行为计数特征的处理  

## 评价指标

### 混淆矩阵

-->表示预测

TP：真-->真

FN：真-->假

FP：假-->真

TN：假-->假

### 具体指标

准确率（Accuracy）  	精确率（Precision）  	召回率（Recall）  	F1score	P-R曲线（Precision-Recall Curve）  ROC曲线（Receiver Operating Characteristic）  	AUC曲线(Area Under Curve)  

## 代码

### 数据读取

显示数据集大小

```
import pandas as pd
train = pd.read_csv('train.csv')
testA = pd.read_csv('testA.csv')
print('Train data shape:',train.shape)
print('TestA data shape:',testA.shape)
```

```
Train data shape: (800000, 47)
TestA data shape: (200000, 48)
```

计算混淆矩阵，y_true是标签，y_pred是预测值

```
from sklearn.metrics import confusion_matrix
y_pred = [0, 1, 0, 1]
y_true = [0, 1, 1, 0]
confusion_matrix(y_true, y_pred)
```

计算准确率，y_true、y_pred同上

```
from sklearn.metrics import accuracy_score
metrics.precision_score(y_true, y_pred)
```

计算精确率、召回率、F1-score

```
metrics.precision_score(y_true, y_pred)
metrics.recall_score(y_true, y_pred)
metrics.f1_score(y_true, y_pred)
```

计算P-R曲线，纵轴precision，横轴recall

```
from sklearn.metrics import precision_recall_curve
precision, recall, thresholds = precision_recall_curve(y_true, y_pred)
```

计算ROC曲线，横轴FPR，纵轴TPR

```
from sklearn.metrics import roc_curve
FPR,TPR,thresholds=roc_curve(y_true, y_pred)
```

计算AUC曲线

```
from sklearn.metrics import roc_auc_score
roc_auc_score(y_true, y_scores)
```

