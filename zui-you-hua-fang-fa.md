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

#牛顿法（Newton's method）
迭代公式如下：
$$
x_{k+1} = x_{k} - \frac{f^\prime(x_k)}{f^{\prime\prime}(x_k)}，k=0，1
$$
$$
\mathbf{x}_{k+1}=\mathbf{x}_{k} - \frac{\nabla f(\mathbf{x}_{k})}{\nabla^{2} f(\mathbf{x}_{k})}=\mathbf{x}_{k}-H_{k}^{-1} \cdot \mathbf{g}_{k}
$$
其中，$$g=\nabla f$$为梯度向量，$$H=\nabla^{2}f$$为海森矩阵(Hessian matrix)，定义为
$$
g=\nabla f=\left[ \begin{array}{c}{\frac{\partial f}{\partial x_{1}}} \\ {\frac{\partial f}{\partial x_{2}}} \\ {\vdots} \\ {\frac{\partial f}{\partial x_{N}}}\end{array}\right]
$$
$$
H=\nabla^{2} f=\left[ \begin{array}{cccc}{\frac{\partial^{2} f}{\partial x_{1}^{2}}} & {\frac{\partial^{2} f}{\partial x_{1} \partial x_{2}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{1} \partial x_{N}}} \\ {\frac{\partial^{2} f}{\partial x_{2} \partial x_{1}}} & {\frac{\partial^{2} f}{\partial x_{2}^{2}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{2} \partial x_{N}}} \\ {\frac{\partial^{2} f}{\partial x_{N} \partial x_{1}}} & {\frac{\partial^{2} f}{\partial x_{N} \partial x_{2}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{N}^{2}}}\end{array}\right]_{N \times N}
$$

###数学推导
####二阶泰勒展开式
在现有极小点估计值的附近，对f(x)做二阶泰勒展开，进而找到极小点的下一个估计值。
设$$x_k$$为当前的极小点估计值，则
$$
\varphi(x)=f\left(x_{k}\right)+f^{\prime}\left(x_{k}\right)\left(x-x_{k}\right)+\frac{1}{2} f^{\prime \prime}\left(x_{k}\right)\left(x-x_{k}\right)^{2}
$$
$$\varphi(x)$$为$$f(x)$$在$$x_k$$附近的二阶泰勒展开式近似函数，由于求的是极值，则
$$
\varphi^{\prime}(x)=0
$$
即
$$
f^{\prime}\left(x_{k}\right)+f^{\prime \prime}\left(x_{k}\right)\left(x-x_{k}\right)=0
$$
从而求得
$$
x=x_{k}-\frac{f^{\prime}\left(x_{k}\right)}{f^{\prime \prime}\left(x_{k}\right)}
$$
若给定初始值$$x_0$$，则得到迭代公式
$$
x_{k+1}=x_{k}-\frac{f^{\prime}\left(x_{k}\right)}{f^{\prime \prime}\left(x_{k}\right)}, k=0,1,...
$$

###阻尼牛顿法
原始牛顿法由于迭代公式中没有步长因子，对于非二次型函数，有时会使函数值上升，即$$f\left(\mathbf{x}_{k+1}\right)>f\left(\mathbf{x}_{k}\right)$$，这表明原始牛顿法不能保证函数值的稳定下降，严重的情况下甚至造成迭代点列$$\{\mathbf{x}_k\}$$发散而导致计算失败。
**阻尼牛顿法**相比原始牛顿法，在每次迭代中引入最优的步长因子$$\lambda_{k}$$，即从点$$\mathbf{x_k}$$出发，沿着牛顿方向$$\mathbf{d}_k$$做一维搜索，获得最优步长：
$$
\lambda_{k}=\arg \min _{\lambda \in \mathbb{R}} f\left(\mathbf{x}_{k}+\lambda \mathbf{d}_{k}\right)
$$
其中，$$\mathbf{d}_{k}=-H_{k}^{-1} \cdot \mathrm{g}_{k}$$

**优点**：
1. 相比梯度下降法，不仅利用了一阶信息，还利用了二阶信息，具有二阶收敛速度。
**缺点**：
1. 对目标函数有较严格的要求，函数必须有连续的一阶、二阶偏导数，海森矩阵必须正定。
2. 计算相当复杂，需要计算梯度、二阶偏导数矩阵及逆矩阵。计算/存储量很大，且均以参数维度 N 的平方比增加($$O(N^2)$$)。

#拟牛顿法(Quasi-Newton Methods)
参考[牛顿法与拟牛顿法](https://blog.csdn.net/itplus/article/details/21896619)
拟牛顿法的**基本思想**是：
1. 不用二阶偏导数而构造出可以近似海森矩阵(或海森矩阵的逆)的正定对称阵
2. 在“拟牛顿”的条件下优化目标函数
常见的拟牛顿法有**DFP**，**BFGS**，**L-BFGS**

##拟牛顿条件
设经过$$k+1$$次迭代后得到$$\mathbf{x}_{k+1}$$，将目标函数$$f(\mathbf{x})$$在$$\mathbf{x}_{k+1}$$附近做二阶泰勒展开：

$$
f(\mathrm{x}) \approx f\left(\mathrm{x}_{k+1}\right)+\nabla f\left(\mathrm{x}_{k+1}\right) \cdot\left(\mathrm{x}-\mathrm{x}_{k+1}\right)+\frac{1}{2} \cdot\left(\mathrm{x}-\mathrm{x}_{k+1}\right)^{T} \cdot \nabla^{2} f\left(\mathrm{x}_{k+1}\right) \cdot\left(\mathrm{x}-\mathrm{x}_{k+1}\right)
$$
两边同时作用一个梯度算子$$\nabla$$，可得
$$
\nabla f(\mathrm{x}) \approx \nabla f\left(\mathrm{x}_{k+1}\right)+H_{k+1} \cdot\left(\mathrm{x}-\mathrm{x}_{k+1}\right)
$$

取$$\mathrm{x} = \mathrm{x}_k$$，可得
$$
\mathbf{g}_{k+1}-\mathbf{g}_{k} \approx H_{k+1} \cdot\left(\mathbf{x}_{k+1}-\mathbf{x}_{k}\right)
$$
若引入记号：
$$
\mathbf{s}_{k}=\mathbf{x}_{k+1}-\mathbf{x}_{k}, \quad \mathbf{y}_{k}=\mathbf{g}_{k+1}-\mathbf{g}_{k}
$$
则可写成：
$$
\mathbf{y}_{k} \approx H_{k+1} \cdot \mathbf{s}_{k}，或者 \  \mathbf{s}_{k} \approx H_{k+1}^{-1} \cdot \mathbf{y}_{k}
$$

这就是所谓的**拟牛顿条件**，它对迭代过程中的海森矩阵$$H_{k+1}$$作约束，因此，对$$H_{k+1}$$作近似的$$B_{k+1}$$，以及对$$H_{k+1}^{-1}$$做近似的$$D_{k+1}$$可以将：
$$
\mathbf{y}_{k} = B_{k+1} \cdot \mathbf{s}_{k}，或者\ \mathbf{s}_{k}=D_{k+1} \cdot \mathbf{y}_{k}
$$

##DFP算法
该算法核心是：












