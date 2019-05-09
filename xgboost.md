#Gradient boosting(GB)
GB的思想是迭代生成多个弱模型，然后将每个弱模型的预测结果相加，后面的模型$$F_{m+1}(x)$$基于前一个模型$$F_m(x)$$的效果生成，关系如下：
$$
F_{m+1}(x)=F_{m}(x)+h(x)，1 \leq m \leq M
$$
怎么生成$$h(x)$$ ?
1. 如果目标函数是回归问题的均方误差，很容易想到最理想的$$h(x)$$是拟合 $$y-F_m(x)$$，就是常说的基于残差的学习。
2. 一般情况下，是基于Loss Function在**函数空间**的负梯度方向，对于回归问题$$\frac{1}{2}(y-F(x))^2$$，残差与负梯度方向相同，因此基于负梯度的学习也被称为"伪残差"。

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

#Gradient boosting Decision Tree(GBDT)
GB算法中最典型的基学习器是决策树，尤其是CART，


参考GBM论文[Greedy Function Approximation: A Gradient Boosting Machine](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf)


    
    
    


