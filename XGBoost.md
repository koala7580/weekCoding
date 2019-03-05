# 一、CART树

决策树面临的问题：

- 过拟合：决策树算法增长数的每一个分支的深度，敲好能对训练样例比较完美的分类。实际应用中，当数据有噪声或训练样例的数量太少以至于不能产生目标函数的有代表性的采样时，该决策会遇到困难。
- 决策树算法更适合处理离散数值的属性。在应用连续属性值时，在一个树节点可以将属性Ai的值划分为几个区间，然后信息增益的计算就可以采用和离散处理一样的方法。C4.5中采用的是二元分割，需要找出一个合适的分割阈值。

目标变量是离散的 —— 分类树

目标变量是连续的 —— 回归树

**CART与ID3的不同**

- 二元划分：二叉树不易产生数据碎片，精确度往往高于多叉树

- CART中选择白能量的不纯性度量：

  分类目标：Gini指标、Towing、order Towing

  连续目标：最小平方残差、最小绝对残差

- 剪枝：用预剪枝或后剪枝对训练集生长的树进行剪枝

- 树的建立：若目标变量是标称的，且具有两个以上的类别，则CART可能是考虑将目标类别合并成两个超类别（双化）；若目标变量是连续的，则CART算法找出一组基于树的回归方程来预测目标变量。

**CART分类树建立**

![1551752324900](https://github.com/koala7580/weekCoding/blob/master/img/CART.png)



# 二、算法原理

相比于原始的GBDT，XGBoost的目标函数多了正则项，使得学习出来的模型更加不容易过拟合。

## 1. 函数模型

![1551752836458](https://github.com/koala7580/weekCoding/blob/master/img/model.png)

## 2. 损失函数

![1551752912160](https://github.com/koala7580/weekCoding/blob/master/img/loss.png)

## 3. 正则化

**三个角度的理解**

- 通过偏差方差分解来解释

- PCA-learning 泛化界来解释

- Bayes鲜艳解释，把正则当成先验

  L2正则：模型参数服从高斯分布，大部分绝对值很小——防止过拟合；

  L1正则：模型参数服从拉普拉斯分布，大部分取值为0——稀疏.

## 4. 损失函数

将误差函数进行二阶泰勒展开：

![1551768233940](https://github.com/koala7580/weekCoding/blob/master/img/taylor-1.png)

![1551768245598](https://github.com/koala7580/weekCoding/blob/master/img/taylor-2.png)

![1551768261452](https://github.com/koala7580/weekCoding/blob/master/img/taylor-3.png)

![1551768274225](https://github.com/koala7580/weekCoding/blob/master/img/taylor-loss.png)

## 5.分裂结点算法

**分裂前后的增益如何计算：**

- ID3算法采用信息增益
- C4.5算法采用信息增益比
- CART采用Gini稀疏
- XGBoost  ？（如下）

对于一个叶子结点进行分裂，分裂前后的增益定义为： $Gain=\frac{G_L^2}{H_L+\lambda}+\frac{D_R^2}{H_R+\lambda}-\frac{(G_L+G_R)^2}{H_L+H_R+\lambda}-\gamma$

Gain的值越大，分裂后L减小越多。

**1） 精确算法** ：遍历所有特征的所有可能的分割点，计算gain值，选取值最大的（feature，value）去分割

![1551768871140](https://github.com/koala7580/weekCoding/blob/master/img/exact.png)

**2） 近似算法** ：对于每个特征，只考察分位点，减少计算复杂度

- Global：学习每颗树前，提出候选切分点

- Local：每次分列前，重新提出候选切分点

  ![1551768973345](https://github.com/koala7580/weekCoding/blob/master/img/approximate.png)

**3） 稀疏值处理** ：

- 稀疏值：缺失，类别one-hot编码，大量0值

- 特征缺失：XGBoost可以学习出默认的节点分裂方向

  ![1551769113298](https://github.com/koala7580/weekCoding/blob/master/img/sparse.png)

  

# 三、特性

## 1. 优点

- 高度灵活性
- 正则化，可控制模型复杂度
- Shrinkage（缩减），相当于学习率
- 剪枝，XGBoost会一直分裂到指定最大深度，可以保留部分潜在最优
- 缺失值处理：有内置处理缺失值规则
- 并行操作

## 2. 缺点

- 基于level-wise的分裂方式 
- 预排序方法空间消耗比较大，不仅要保存特征值，也要保存特征的排序索引，同时时间消耗也大

# 四、sklearn参数

1）objective [ default=reg:linear ] 定义学习任务及相应的学习目标，可选的目标函数如下：

- “reg:linear” –线性回归。
- “reg:logistic” –逻辑回归。
- “binary:logistic” –二分类的逻辑回归问题，输出为概率。
- “binary:logitraw” –二分类的逻辑回归问题，输出的结果为wTx。
- “count:poisson” –计数问题的poisson回归，输出结果为poisson分布。 在poisson回归中，max_delta_step的缺省值为0.7。(used to safeguard optimization)
- “multi:softmax” –让XGBoost采用softmax目标函数处理多分类问题，同时需要设置参数num_class（类别个数）
- “multi:softprob” –和softmax一样，但是输出的是ndata * nclass的向量，可以将该向量reshape成ndata行nclass列的矩阵。没行数据表示样本所属于每个类别的概率。
- “rank:pairwise” –set XGBoost to do ranking task by minimizing the pairwise loss

（2）’eval_metric’ The choices are listed below，评估指标:

- “rmse”: root mean square error
- “logloss”: negative log-likelihood
- “error”: Binary classification error rate. It is calculated as #(wrong cases)/#(all cases). For the predictions, the evaluation will regard the instances with prediction value larger than 0.5 as positive instances, and the others as negative instances.
- “merror”: Multiclass classification error rate. It is calculated as #(wrong cases)/#(all cases).
- “mlogloss”: Multiclass logloss
- “auc”: Area under the curve for ranking evaluation.
- “ndcg”:Normalized Discounted Cumulative Gain
- “map”:Mean average precision
- “ndcg@n”,”map@n”: n can be assigned as an integer to cut off the top positions in the lists for evaluation.
- “ndcg-“,”map-“,”ndcg@n-“,”map@n-“: In XGBoost, NDCG and MAP will evaluate the score of a list without any positive samples as 1. By adding “-” in the evaluation metric XGBoost will evaluate these score as 0 to be consistent under some conditions.

（3）lambda [default=0] L2 正则的惩罚系数

（4）alpha [default=0] L1 正则的惩罚系数

（5）lambda_bias 在偏置上的L2正则。缺省值为0（在L1上没有偏置项的正则，因为L1时偏置不重要）

（6）eta [default=0.3] 
为了防止过拟合，更新过程中用到的收缩步长。在每次提升计算之后，算法会直接获得新特征的权重。 eta通过缩减特征的权重使提升计算过程更加保守。缺省值为0.3 
取值范围为：[0,1]

（7）max_depth [default=6] 数的最大深度。缺省值为6 ，取值范围为：[1,∞]

（8）min_child_weight [default=1] 
孩子节点中最小的样本权重和。如果一个叶子节点的样本权重和小于min_child_weight则拆分过程结束。在现行回归模型中，这个参数是指建立每个模型所需要的最小样本数。该成熟越大算法越conservative 
取值范围为: [0,∞]

*参考连接*

[XGBoost原理及应用](https://blog.csdn.net/u012102306/article/details/52523045)
