1. 项目简介
本项目基于 DeepLabV3+ 语义分割框架，以 ResNet-101 作为主干网络（backbone），用于完成 科学图像拷贝–移动伪造区域的像素级检测与定位。
该实现针对 Recod.ai/LUC Scientific Image Forgery Detection 竞赛任务，旨在评估多尺度上下文建模方法在科学图像伪造检测场景下的适用性与局限性。
模型通过 ASPP（Atrous Spatial Pyramid Pooling）模块 捕获多尺度上下文信息，并在解码阶段恢复空间分辨率，从而输出与输入图像同尺寸的伪造概率图。
2. 方法概述
2.1 网络结构
编码器（Encoder）
采用 ResNet-101 作为特征提取主干
使用空洞卷积（Atrous Convolution）扩大感受野，同时保持较高空间分辨率

ASPP 模块
并行设置多组不同空洞率的卷积分支
融合多尺度上下文信息，增强对不同尺寸伪造区域的感知能力

解码器（Decoder）
对高层语义特征进行上采样
与浅层特征进行融合
恢复精细空间结构并生成像素级预测结果

输出层
通过 1×1 卷积输出单通道概率图
使用 Sigmoid 函数将结果映射至 [0,1]

2.2损失函数
训练阶段主要采用 Dice Loss（或 Dice + BCE 的组合）作为优化目标，以缓解科学图像中伪造区域像素占比小、类别不平衡严重的问题。

3. 代码结构说明
本项目以 Jupyter Notebook 形式组织，主要包含以下几个功能模块：
环境与依赖导入
包括 PyTorch、Torchvision、NumPy、OpenCV 等深度学习与图像处理库
数据集加载与预处理
图像读取与尺寸统一
掩码加载与二值化处理
构建训练与验证数据集
模型构建
调用 torchvision 中的 DeepLabV3+ 接口
替换分类头以适配二分类分割任务
训练流程：前向传播损失计算反向传播与参数更新训练过程日志输出推理与可视化生成伪造概率图阈值分割得到二值掩码预测结果与原图、GT 的可视化对比

4. 环境依赖
推荐使用如下环境配置：
Python ≥ 3.8
PyTorch ≥ 1.12
torchvision ≥ 0.13
numpy
opencv-python
matplotlib
PIL

5. 数据准备
dataset/
├── train_images/
│   ├── authentic/
│   └── forged/
├── train_masks/
├── test_images/
├── supplemental_images/
└── supplemental_masks/

6. 使用说明
打开 deeplabv3-resnet101.ipynb
按顺序执行 Notebook 中的各个 Cell
模型将在训练集上进行训练，并在验证集上评估 Dice 指标