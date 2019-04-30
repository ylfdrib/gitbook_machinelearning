#梯度下降法（Gradient Descent）
梯度下降法，属于一阶优化算法，根据每次迭代求解L代入的样本数，可以分为：**全量梯度下降**（一次计算所有样本的损失），**批量梯度下降***(一次计算一个batch样本的损失)和**随机梯度下降**(一次只随机计算一个样本的损失)。
$$
\theta_{n} := \theta_{n-1} - \eta L^{\prime}(\theta_{n-1})
$$
其中，$$\alpha$$是梯度每次逼近的步长，‘-’负号表示负梯度方向，L是损失函数。

###数学推导
####一阶泰勒展开式
主要利用的数学思想是，曲线函数的线性拟合近似。
$$
f(\theta) \approx f\left(\theta_{0}\right)+\left(\theta-\theta_{0}\right) \cdot \nabla f\left(\theta_{0}\right)
$$
其中，$$\theta-\theta_0$$是微小矢量，它的大小用步长$$\eta$$表示，单位向量用$$v$$表示，则
$$
\theta-\theta_0=\eta v
$$
注意$$\theta-\theta_0$$不能太大，不然线性近似就不准确了，一阶泰勒近似也不成立了，替换后：
$$
f(\theta) \approx f\left(\theta_{0}\right)+\eta v \cdot \nabla f\left(\theta_{0}\right)
$$
局部下降的目的是每次更新$$\theta$$后，函数值$$f(\theta)$$变小，也就是：
$$
f(\theta)-f\left(\theta_{0}\right) \approx \eta v \cdot \nabla f\left(\theta_{0}\right) < 0 
$$
$$\eta$$为标量，一般为正值，不等式变成了：
$$
v \cdot \nabla f(\theta_0) < 0
$$
两个向量的乘积：
$$
A \cdot B=\|A\| \cdot\|B\| \cdot \cos (\alpha)
$$
当两个向量反方向时，乘积取得最小值，即单位向量$$v$$和$$\nabla f(\theta_0)$$方向相反，得到
$$
v=-\frac{\nabla f\left(\theta_{0}\right)}{\left\|\nabla f\left(\theta_{0}\right)\right\|}
$$
代入表达式$$\theta-\theta_0=\eta v$$中，得到：
$$
\theta=\theta_{0}-\eta \frac{\nabla f\left(\theta_{0}\right)}{\left\|\nabla f\left(\theta_{0}\right)\right\|}
$$
一般的，因为$$\left\|\nabla f\left(\theta_{0}\right)\right\|$$ 是标量，可以并入因子$$\eta$$中，化简为：
$$
\theta=\theta_{0}-\eta^\prime\nabla f\left(\theta_{0}\right)
$$
其中，$$\eta^\prime = \frac{\eta}{\|\nabla f(\theta_0) \|}$$

#牛顿法






