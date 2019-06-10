#lightGBM
**lightGBM**也是一种GBDT框架，相比**XGBoost**，有一定的优势
- 训练速度更快，占用内存更小
    - 每次划分计算增益的时候，速度更快
        - XGB用的是pre-sort-based 方法对特征值排序，时间复杂度O(#data)
        - lightGBM用的直方图算法，将特征值分桶，时间复杂度O(#bins)
    - 直方图做差，速度提升一倍
        - 划分子树时，右子树直方图 = 父节点直方图 - 左子树直方图
    - 内存使用的更少
        - 直方图算法将特征值映射成桶编号，存储桶编号(例如 uint8_t)比原始特征值(例如 float)使用更少的内存。
        - 不用存储附加的pre-sorting特征值
- 稀疏优化
    - 构造稀疏特征的直方图，只需要O(2 * #non_zero_data)
- 准确率提升
    - Leaf-wise(Best-first) 方式树生长
        - 大部分决策树都是按level(depth)-wise方式生长（每次取）
