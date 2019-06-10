#lightGBM
lightGBM也是一种GBDT框架，相比XGBoost，有一定的优势
- 训练速度更快，占用内存更小
    - 每次划分计算增益的时候，速度更快
        - XGB用的是pre-sort-based 方法对特征值排序，时间复杂度O(#data)
        - lightGBM用的直方图算法，将特征值分桶，时间复杂度O(#bins)
