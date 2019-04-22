#熵
entropy, 熵是随机变量不确定性的度量
$$
H(X)=-\sum_{i=1}^{n}p(x_i)logp(x_i)
$$

#条件熵
在x取值一定的情况下随机变量y不确定性的度量
$$
H(Y|X) = \sum_{x \in X} p(x)H(Y|X=x)
$$

#信息增益
information gain, 信息增益就是熵和特征条件熵的差，就是在得知特征X一定的情况下，Y（逾期概率）不确定性的减少程度
$$
IG(Y,X)=H(Y) - H(Y|X)
$$
