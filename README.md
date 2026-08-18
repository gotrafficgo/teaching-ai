# AI 课堂示例：交通仿真与卷积神经网络

这个仓库整理了两个可独立运行的 AI 教学 section，分别从“规则/模型驱动的仿真”和“数据驱动的深度学习”展示 AI 方法。

| Section | 核心内容 | 入口 |
| --- | --- | --- |
| [Car-following simulation](car-following-simulation/README.md) | IDM 跟驰模型、随机交通流、局部限速瓶颈、时空图 | `car-following-simulation/main.py` |
| [CNN](cnn/README.md) | MNIST、卷积、ReLU、池化、全连接、Softmax、训练与预测 | `cnn/train.py`、`cnn/predict_step_by_step.py` |

## 建议课堂顺序

1. 先运行 car-following simulation，区分“车辆行为模型”和“仿真环境”。
2. 对比四种交通流实验，观察微观规则如何产生宏观拥堵波。
3. 再进入 CNN，用手写数字识别解释神经网络如何从数据中学习参数。
4. 最后比较两类方法：IDM 的规则由人明确写出，CNN 的卷积核与分类权重由训练得到。

每个 section 都有独立的环境安装与运行说明，建议分别创建虚拟环境。
