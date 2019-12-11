# 简介
Louvain算法是一种基于图数据的社区发现算法。原论文[《Fast unfolding of communities in large networks》](https://arxiv.org/abs/0803.0476)。
# 模块度
模块度（Modularity） 是社区发现类算法的评估指标，用来评估发现的社区质量好坏。

**定义如下**：
$$Q=\frac{1}{2 m} \sum_{i, j} \left[A_{i j}-\frac{k_{i} k_{j}}{2 m}\right] \delta\left(c_{i}, c_{j}\right)，Q\in(-1, 1)$$
$$\text{其中，} \delta(u, v)=\left\{\begin{array}{ll}{1} & {\text { when } \mathbf{u}==\mathbf{v}} \\ {0} & {\text { else }}\end{array}\right.$$

$$A_{i j}$$：表示顶点$$i$$和顶点$$j$$之间边的权重
$$k_i$$：顶点$$i$$的所有邻接边的权重之和 $$k_i = {\sum_j}{A_{i j}}$$
$$c_i$$：表示顶点$$i$$所属的社区
$$m$$：整个图G中所有边权重之和 $$m=\frac{1}{2}\sum_{ij}A_{ij}$$（每条边计算了一次）

公式中只有$$i, j$$属于同一个社区才有意义，**公式变形**：

$$\begin{aligned} Q &=\frac{1}{2 m} \sum_{i, j}\left[A_{i, j}-\frac{k_{i} k_{j}}{2 m}\right] \delta\left(c_{i}, c_{j}\right) \\ &= \frac{1}{2 m}\left[\sum_{i, j} A_{i, j}-\frac{\sum_{i} k_{i} \sum_{j} k_{j}}{2 m}\right] \delta\left(c_{i}, c_{j}\right) \\ &=\frac{1}{2 m} \sum_{c}\left[\Sigma_{i n}-\frac{ {\Sigma_{tot} }^{2}}{2 m}\right] \\ &= \sum_{c}\left[\frac{\Sigma_{in}}{2m}  - \left(\frac{\Sigma_{tot}}{2m}\right)^2\right] \end{aligned}$$

$$\Sigma_{in}$$：表示社区$$c$$内所有顶点的社区内的边权重之和（每条边计算了两次）
$$\Sigma_{tot}$$：表示社区$$c$$内所有顶点的邻接边权重之和（包括社区内的邻接边和社区外的邻接边）
$$\Sigma_{i}k_i$$：$$\Sigma_i k_i = \Sigma_j k_j = \Sigma_{tot}$$

# 优化目标

Louvain算法是以**模块度**为直接优化目标，在实际的实现中，只需要优化模块度增量，即一个独立的顶点$$i$$加入一个社区$$c$$带来的模块度增量$$\Delta Q$$，公式为：

$$\begin{aligned} \Delta Q&=Q_{after} - Q_{before}\\&=\left[\frac{\sum_{i n}+k_{i, i n}}{2 m}-\left(\frac{\sum_{t o t}+k_{i}}{2 m}\right)^{2}\right]-\left[\frac{\sum_{i n}}{2 m}-\left(\frac{\sum_{t o t}}{2 m}\right)^{2}-\left(\frac{k_{i}}{2 m}\right)^{2}\right] \\&=\frac{k_{i, i n}}{2 m}-\frac{\sum_{t o t} k_{i}}{2 m^{2}} \end{aligned}$$

$$k_{i,in}$$：表示顶点$$i$$与社区$$c$$的连接边权重（每条边计算了两次）
$$k_i$$：表示顶点$$i$$的所有邻接边的权重之和 $$k_i = {\sum_j}{A_{i j}}$$
$$\Sigma_{tot}$$：表示要社区$$c$$内所有顶点的邻接边权重之和（包括社区内的邻接边和社区外的邻接边）

实际计算的时候，可以把常量$$\frac{1}{2m}$$提出来，只计算 $$k_{i,in} - \frac{\sum_{tot}k_i}{m}$$

# 算法基本流程

Louvain算法包括两个阶段，整个流程是两个阶段的不断迭代。

**阶段一**：社区划分，不断遍历图中所有节点，依次判断每个节点给它的邻居社区带来的模块度增量，选增量最大的社区（$$\Delta Q > 0$$）加入，直至一次遍历中没有社区变化。（为了使得每个节点是独立顶点，需要先把当前节点从所在社区删除，计算模块度减量加入到模块度增量的计算中）

**阶段二**：社区折叠，对第一阶段的社区进行折叠，将每个社区合并成超级节点重新构图，两个超级节点的边权是之前社区的所有邻接边权之和，超级节点增加自连边，权重是之前社区内所有边权之和的两倍$$\Sigma_{in}$$。

整个Louvain算法就是不断迭代两个阶段，直至算法稳定(图的模块度不再变化)，或者达到最大迭代次数。

[Louvain源码实现参考](https://github.com/taynaud/python-louvain)


