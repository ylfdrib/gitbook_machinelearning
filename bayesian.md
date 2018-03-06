#Bayesian

##贝叶斯定理
$$
    \mathcal{p(w|D)=\frac{p(D|w)p(w)}{p(D)}}
$$
其中，$$\mathcal{D}=\{t_1,...,t_N\}$$ 是观测数据，$$\mathcal{p(w|D)}$$ 是后验概率。右侧$$\mathcal{p(D|w)}$$由观测数据$$\mathcal{D}$$来估计，可以被看成参数向量$$\mathcal{w}$$的函数，被称为似然函数（likelihood funcition）。用自然语言描述贝叶斯定理：
$$
    posterior \propto likelihood \times prior
$$