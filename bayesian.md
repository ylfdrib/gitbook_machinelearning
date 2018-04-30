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

##MLE vs MAP vs Bayesian

- Training Data: $$ \mathcal{D = \{(x_1, y_1),...,(x_n, y_n)\}} $$
- Model parameter: $$ \mathcal{\theta} $$
- New Data: $$ x^* $$

###1. MLE (Maximum Likelihood Estimation)
- Learning: Finding $$\mathcal{\theta^*}$$ such that maximize $$ \mathcal{p(D|\theta)} $$,  

    即$$\theta^*_{MLE}=argmax_{\theta}\mathcal{p(D|\theta)}$$ ，一般的求解方法是令 $$\mathcal{\frac{\partial p(D|\theta)}{\partial \theta} = 0}$$.
- Prediction: $$ \mathcal{p(\hat y | x^*, \theta^*)} $$
- 缺点
    - 缺少先验
    - 容易过拟合

###2. MAP (Maximum A Posteri estimation)
- Learning: Finding $$\mathcal{\theta^*}$$ such that maximize $$ \mathcal{p(\theta | D)} $$,  
    
    即$$\theta^*_{MAP} = \mathcal{argmax_{\theta}\ p(\theta|D)=argmax_{\theta}\ p(D|\theta)p(\theta) = argmax_{\theta}\ log\ p(D|\theta) + log\ p(\theta) }$$
                
                
- Prediction: $$ \mathcal{p(\hat y | x^*, \theta^*)} $$

###3. Bayesian estimation
- Learning: Computing the posterior $$ \mathcal{p(\theta | D)} $$
- Prediction: $$\mathcal{p(\hat y | x^*, D) = \int_\theta p(\hat y | x^*, \theta)p(\theta|D)d\theta}$$

###4. 区别
- MLE,MAP的目标是找到"特定"的最优解 $$ \theta^* $$，而Bayesian考虑了所有的参数，即参数 $$\theta$$ 的整个分布。

###线性回归
输出是输入变量的线性组合，即
$$
    y(x,w)=w_0 + w_1x_1 + ... + w_Dx_D
$$

 

 ###参考 
 1. 李文哲分享 [链接](http://www.chinahadoop.cn/course/586/learn#lesson/11556)
 
 
 
 
 
 
   
