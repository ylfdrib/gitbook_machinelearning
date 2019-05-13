#Huber Loss function
Huber Loss是为了增强平方误差损失函数(squared loss function)对噪声(或者叫离群点，outliers)的鲁棒性提出的。

定义：
$$
L_{\delta}(a)=\left\{\begin{array}{ll}{\frac{1}{2} a^{2},} & {\text { for }|a| \leq \delta} \\ {\delta \cdot\left(|a|-\frac{1}{2} \delta\right),} & {\text { otherwise }}\end{array}\right.
$$
注：当$$|a|\leq\delta$$时，loss随$$a$$平方增长；否则，线性增长

通常$$a$$表示residuals，也即$$(y-\hat{y})$$或者$$(y-f(x))$$，当$$a=y-f(x)$$时，上述形式可拓展为：
$$
L_{\delta}(y, f(x))=\left\{\begin{array}{ll}{\frac{1}{2}(y-f(x))^{2},} & {\text { for }|y-f(x)| \leq \delta} \\ {\delta \cdot\left(|y-f(x)|-\frac{1}{2} \delta\right),} & {\text { otherwise }}\end{array}\right.
$$
