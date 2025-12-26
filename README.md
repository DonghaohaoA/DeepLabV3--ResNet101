本项目基于 DeepLabV3+ 语义分割框架，以 ResNet-101 作为主干网络（backbone），用于完成 科学图像拷贝–移动伪造区域的像素级检测与定位。
该实现针对 Recod.ai/LUC Scientific Image Forgery Detection 竞赛任务，旨在评估多尺度上下文建模方法在科学图像伪造检测场景下的适用性与局限性。

模型通过 ASPP（Atrous Spatial Pyramid Pooling）模块 捕获多尺度上下文信息，并在解码阶段恢复空间分辨率，从而输出与输入图像同尺寸的伪造概率图。