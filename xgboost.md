# Gradient boosting\(GB\)

GB的思想是迭代生成多个弱模型，然后将每个弱模型的预测结果相加，后面的模型$$F_{m+1}(x)$$基于前一个模型$$F_m(x)$$的效果生成，关系如下：


$$
F_{m+1}(x)=F_{m}(x)+h(x)，1 \leq m \leq M
$$


怎么生成$$h(x)$$ ?  
1. 如果目标函数是回归问题的均方误差，很容易想到最理想的$$h(x)$$是拟合 $$y-F_m(x)$$，就是常说的基于残差的学习。  
2. 一般情况下，是基于Loss Function在**函数空间**的负梯度方向，对于回归问题$$\frac{1}{2}(y-F(x))^2$$，残差与负梯度方向相同，因此基于负梯度的学习也被称为"伪残差"。

![](/assets/import.png)

GB算法的步骤：  
1. 初始化模型为常数值：


$$
F_{0}(x)=\underset{\gamma}{\arg \min } \sum_{i=1}^{n} L\left(y_{i}, \gamma\right)
$$
2. 迭代生成M个基学习器  

a. 计算伪残差


$$
   r_{i m}=-\left[\frac{\partial L\left(y_{i}, F\left(x_{i}\right)\right)}{\partial F\left(x_{i}\right)}\right]_{F(x)=F_{m-1}(x)} \text { for } i=1, \ldots, n
$$


b. 基于样本$$\left\{\left(x_{i}, r_{i m}\right)\right\}_{i=1}^{n}$$，生成基学习器$$h_m(x)$$  
   c. 计算最优的$$\gamma_m$$


$$
   \gamma_{m}=\underset{\gamma}{\arg \min } \sum_{i=1}^{n} L\left(y_{i}, F_{m-1}\left(x_{i}\right)+\gamma h_{m}\left(x_{i}\right)\right)
$$


d. 更新模型


$$
   F_{m}(x)=F_{m-1}(x)+\gamma_{m} h_{m}(x)
$$


# Gradient boosting Decision Tree\(GBDT\)

GB算法中最典型的基学习器是决策树，尤其是CART，

参考GBM论文[Greedy Function Approximation: A Gradient Boosting Machine](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf)

#XGBoost
XGB官方文档[XGBoost Documentation](https://xgboost.readthedocs.io/en/latest/index.html)
##[Boosted Tree](https://xgboost.readthedocs.io/en/latest/tutorials/model.html)
线性模型的函数形式是输入特征的线性组合：
$$
\hat{y}_{i}=\sum_{j} \theta_{j} x_{i j}
$$
经典的目标函数形式：
$$
\operatorname{obj}(\theta)=L(\theta)+\Omega(\theta) \\
objective function = training loss + regularization \\
目标函数 = 训练集loss + 正则项
$$
线性模型用的MSE(Mean squared error) loss，
$$
L(\theta)=\sum_{i}\left(y_{i}-\hat{y}_{i}\right)^{2}
$$
逻辑回归模型用的 交叉熵 loss，
$$
L(\theta) =\sum_{i}\left[y_{i} \ln \left(1+e^{-\hat{y}_{i}}\right)+\left(1-y_{i}\right) \ln \left(1+e^{\hat{y}_{i}}\right)\right]，label=[0,1] \\
L(\theta) =\sum_{i}\ln\left(1+e^{-2y_{i}\hat{y}_{i}}\right), label=[-1,1]
$$

###Decision Tree Ensembles
GBDT 和 RandomForst 都属于Tree Ensembles一类模型，区别在于训练过程。
GBDT的每棵子树都是回归模型，叶子节点的score对应着f(x)，而不是label(0,1)或者是概率p。
$$
\hat{y}_{i}=\sum_{k=1}^{K} f_{k}\left(x_{i}\right), f_{k} \in \mathcal{F}
$$
其中$$K$$是树的数量，$$\mathcal{F}$$是一个回归树集合，所有可能的CARTs。
样本的最终分数，是所有树上的分数求和，每棵树的分数取的是样本最终落到的那个叶子节点分。最终的分类概率与分数的关系是：
$$
p = sigmoid(\hat{y}_{i}) = 1/(1+e^{-\hat{y}_{i}})
$$
目标函数可以写成：
$$
\operatorname{obj}(\theta)=\sum_{i}^{n} l\left(y_{i}, \hat{y}_{i}\right)+\sum_{k=1}^{K} \Omega\left(f_{k}\right)
$$
###Tree Boosting
Tree Boosting 相比RandomForst的训练过程区别在于，additive training：一次训练一棵树，步步逼近最优解，目标函数
$$
\mathrm{obj}=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t)}\right)+\sum_{i=1}^{t} \Omega\left(f_{i}\right)
$$
其中，第t次 prediction value为 $$\hat{y}_{i}^{(t)}$$：
$$
\hat{y}_{i}^{(t)}=\sum_{k=1}^{t} f_{k}\left(x_{i}\right)=\hat{y}_{i}^{(t-1)}+f_{t}\left(x_{i}\right)
$$
so，迭代的目标函数为
$$
\begin{aligned} \mathrm{obj}^{(t)} &=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t)}\right)+\sum_{i=1}^{t} \Omega\left(f_{i}\right) \\ 
&=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t-1)}+f_{t}\left(x_{i}\right)\right)+\Omega\left(f_{t}\right)+\text { constant } \end{aligned}
$$
如果取MSE-loss，则
$$
\begin{aligned} \mathrm{obj}^{(t)} &=\sum_{i=1}^{n}\left(y_{i}-\left(\hat{y}_{i}^{(t-1)}+f_{t}\left(x_{i}\right)\right)\right)^{2}+\sum_{i=1}^{t} \Omega\left(f_{i}\right) \\ &=\sum_{i=1}^{n}\left[2\left(\hat{y}_{i}^{(t-1)}-y_{i}\right) f_{t}\left(x_{i}\right)+f_{t}\left(x_{i}\right)^{2}\right]+\Omega\left(f_{t}\right)+\mathrm{constant} \end{aligned}
$$
其中，在第t次训练中，$$f_t(x_i)$$为变量，$$y_i， \hat{y}_{i}^{(t-1)}$$为已知，合并为constant
为了与其他类型的loss统一，从广义上定义一个一般的目标函数：
($$l(y_i,\hat{y}_{i}^{(t)}$$ 在$$l(y_i,\hat{y}_{i}^{(t-1)}$$处的泰勒二阶展开，$$\Delta x =f_t(x_i)$$)
$$
\mathrm{obj}^{(t)}=\sum_{i=1}^{n}\left[l\left(y_{i}, \hat{y}_{i}^{(t-1)}\right)+g_{i} f_{t}\left(x_{i}\right)+\frac{1}{2} h_{i} f_{t}^{2}\left(x_{i}\right)\right]+\Omega\left(f_{t}\right)+\mathrm{constant}
$$
其中，$$g_i,h_i$$分别是一阶导和二阶导：
$$
\begin{aligned} g_{i} &=\partial_{\hat{y}_{i}^{(t-1)}}\left(y_{i}, \hat{y}_{i}^{(t-1)}\right) \\ h_{i} &=\partial_{\hat{y}_{i}^{(t-1}}^{2} l\left(y_{i}, \hat{y}_{i}^{(t-1)}\right) \end{aligned}
$$
去掉constants，第t次训练中，第7棵树的目标函数就是：
$$
obj^{(t)} = \sum_{i=1}^{n}\left[g_{i} f_{t}\left(x_{i}\right)+\frac{1}{2} h_{i} f_{t}^{2}\left(x_{i}\right)\right]+\Omega\left(f_{t}\right)
$$

###模型复杂度
正则项用来描述树的复杂度，定义树函数：
$$
f_{t}(x)=w_{q(x)}, w \in R^{T}, q : R^{d} \rightarrow\{1,2, \cdots, T\}
$$
其中，$$w$$是叶子上分数向量，$$T$$维，$$T$$表示当前树的叶子节点数, $$q$$表示一个映射，当前样本$$x$$映射到哪个叶子节点上，XGBoost的正则项表示为
$$
\Omega(f)=\gamma T+\frac{1}{2} \lambda \sum_{j=1}^{T} w_{j}^{2}
$$

###The Structure Score
第t棵数的目标函数：
$$
\begin{aligned} \mathrm{obj}^{(t)} & \approx \sum_{i=1}^{n}\left[g_{i} w_{q\left(x_{i}\right)}+\frac{1}{2} h_{i} w_{q\left(x_{i}\right)}^{2}\right]+\gamma T+\frac{1}{2} \lambda \sum_{j=1}^{T} w_{j}^{2} \\ &=\sum_{j=1}^{T}\left[\left(\sum_{i \in I_{j}} g_{i}\right) w_{j}+\frac{1}{2}\left(\sum_{i \in I_{j}} h_{i}+\lambda\right) w_{j}^{2}\right]+\gamma T \end{aligned}
$$
其中，$$I_{j}=\left\{i | q\left(x_{i}\right)=j\right\}$$表示第j个叶子上的所有样本。
定义：$$G_{j}=\sum_{i \in I_{j}} g_{i}，H_{j}=\sum_{i \in I_{j}} h_{i}$$，则
$$
\mathrm{obj}^{(t)}=\sum_{j=1}^{T}\left[G_{j} w_{j}+\frac{1}{2}\left(H_{j}+\lambda\right) w_{j}^{2}\right]+\gamma T
$$
$$w_j$$是相互独立的，公式$$G_{j} w_{j}+\frac{1}{2}\left(H_{j}+\lambda\right) w_{j}^{2}$$是二次形式，所以在$$q(x)$$已知情况下，可以直接求导得到最优解：
$$
\begin{aligned} w_{j}^{*} &=-\frac{G_{j}}{H_{j}+\lambda} \\ \mathrm{obj}^{*} &=-\frac{1}{2} \sum_{j=1}^{T} \frac{G_{j}^{2}}{H_{j}+\lambda}+\gamma T \end{aligned}
$$


###Learn the tree structure
构建树时，决定是否对叶子节点分裂的衡量指标：
$$
\operatorname{Gain}=\frac{1}{2}\left[\frac{G_{L}^{2}}{H_{L}+\lambda}+\frac{G_{R}^{2}}{H_{R}+\lambda}-\frac{\left(G_{L}+G_{R}\right)^{2}}{H_{L}+H_{R}+\lambda}\right]-\gamma
$$
分别表示
a. 左叶子节点得分 
b. 右叶子节点得分 
c. 原始叶子节点得分 
d. 新增叶子的正则惩罚项








