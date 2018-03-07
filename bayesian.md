#Bayesian

##贝叶斯定理
$$
    \mathcal{p(w|D) = \frac{p(D|w)p(w)}{p(D)}}
$$
其中，$$\mathcal{D}=\{t_1,...,t_N\}$$ 是观测数据，$$\mathcal{p(w|D)}$$ 是后验概率。右侧$$\mathcal{p(D|w)}$$由观测数据$$\mathcal{D}$$来估计，可以被看成参数向量$$\mathcal{w}$$的函数，被称为似然函数（likelihood funcition）。用自然语言描述贝叶斯定理：
$$
    posterior \propto likelihood \times prior
$$

贝叶斯公式的分母是归一化常数，公式两边对$$\mathcal{w}$$求积分，有
$$
    \mathcal{p(D) = \int p(D|w)p(w)\ dw}
$$

##Notations

- Training Data: $$ \mathcal{D = \{(x_1, y_1),...,(x_n, y_n)\}} $$
- Model parameter: $$ \mathcal{\theta} $$
- New Data: $$ x^* $$

##MLE (Maximum Likelihood Estimation)
- Learning: Finding $$\mathcal{\theta^*}$$ such that maximize $$ \mathcal{p(D|\theta)} $$

- Prediction: $$ \mathcal{p(\hat y | x^*, \theta^*)} $$







##MAP




##Bayesian
 
 
 
 
 
 
 
 
 
 
 
 
 
   
