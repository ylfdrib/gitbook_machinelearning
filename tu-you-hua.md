#凸优化
##凸集
集合 _C_ 被称为**凸集**，如果 _C_ 中任意两点间的线段仍然在 _C_ 中，
即$$\  \forall \mathcal{x_1,x_2} \in C, 0 \leqslant \theta \leqslant 1\ $$，有
$$
    \theta x_1 + (1 - \theta)x_2\in C
$$

##凸锥
###锥
集合 _C_ 被称为**锥**，
即$$\ \forall x \in C, \theta \geqslant 0 $$，有
$$
    \theta x \in C
$$

###凸锥
集合 _C_ 被称为**凸锥**，如果集合 _C_ 是锥，并且是凸的，
即$$\ \forall x_1,x_2 \in C, \theta_1,\theta_2 \geqslant 0$$, 有
$$
    \theta_1x_1 + \theta_2x_2 \in C
$$

##凸函数
函数$$f:R^n \to R$$ 被称为**凸函数**，
即 如果dom $$f$$ 是凸集，且 $$\forall x_1,x_2 \in dom f, \forall 0 \leqslant \theta \leqslant 1$$，有
$$
f(\theta x_1 + (1-\theta) x_2) \leqslant \theta f(x_1) + (1-\theta)f(x_2)
$$




        
    