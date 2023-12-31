###### Collaborative Global-Local Networks for Memory-Efficient Segmentation of Ultra-High Resolution Images ([CVPR 2019](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fextreme-assistant%2Fcvpr2020%2Fblob%2Fmaster%2Fcvpr_2019_githublinks.csv))





**Abstract**

超高分辨率图像常用的分割方法包括下采样和将图像裁剪成小块进行单独处理，但其相应的会造成局部细节的损失或全局上下文信息的丢失。**Collaborative Global-Local Networks (GLNet)** 以高内存效率的方式有效的保留局部和全局信息。GLNet 包括一个**全局分支 (global branch)** 和一个**局部分支 (local branch)**，其中全局分支以下采样得到的低分辨率图像作为输入，而局部分支以裁剪得到的小块图像作为输入。GLNet 融合两个分支的特征，从放大的小块图像中获取高分辨率的细节特征，从下采样的图像中获取全局上下文信息。





背景：下采样或补丁裁剪，将导致高分辨率细节或**空间上下文信**息的丢失

实验结果显示仅利用下采样全局图像信息的模型会损失一些局部细节特征，产生不准确的分割边界；而 DeepGlobe 数据集中 agriculture 和 barren 类的区域通常比较相似，仅利用裁剪得到的局部图像的模型由于缺少全局上下文信息，会难以区分agriculture 和 barren 类别。



SSIM







1.1. Contributions

- 研究了**超高分辨率图像的分割问题**，其目标是在保证分割精度的同时，提高内存使用效率，有针对性的提出了一个有着高内存使用效率的分割模型 GLNet，对于高达30M像素的超高分辨率图像，GLNet 的训练只需要1块1080Ti GPU，而推理仅需要2GB的显存；
- GLNet 可以有效的聚合全局上下文信息和高分辨率的局部细节特征，从而得到高质量的分割结果；
- 为了解决前景和背景不平衡的问题，进一步提出了从粗到细的 GLNet 变体，通过全局分支来进行粗略的分割，得到前景区域之后再利用局部分支进行精细的分割。



###### 网络架构

![image-20231211161826361](image/GLNet%E5%85%A8%E5%B1%80%E5%B1%80%E9%83%A8%E5%8D%8F%E5%8A%A9%E7%BD%91%E7%BB%9C/image-20231211161826361.png)

全局和局部分支有着相同的骨干网络，两个分支之间会进行**深度特征共享 (Deep Feature Map Sharing)**，通过**聚合两个分支的输出**可以得到最后的分割结果 (aggregation laye）。为了约束两个分支并进行稳定的训练，针对局部分支的训练采用了**弱耦合正则化**方法。

###### **深度特征共享 (Deep Feature Map Sharing)**

<img src="image/GLNet%E5%85%A8%E5%B1%80%E5%B1%80%E9%83%A8%E5%8D%8F%E5%8A%A9%E7%BD%91%E7%BB%9C/image-20231211162336018.png" alt="image-20231211162336018" style="zoom:50%;" />

从全局分支到局部分支的特征图共享：在主干网络上的同一层，对全局分支上的特征图按照局部分支特征图所在的位置进行裁剪，长采样，按通道与局部特征进行拼接作为局部分支的输入。

从局部分支到全局分支的特征图共享：在主干网络上的同一层，对局部分支得到的特征图按照patch在全局分支大小进行下采样，并按相应顺序进行拼接得到与全局特征大小一样的特征图，再按通道与全局特征进行拼接作为全局特征的输入。



###### **Branch Aggregation with Regularization:**

聚合层（aggregation laer)是一个3x3的卷积层，用两个分支最后一层的串联作为输入。

其中包含3个损失：Sagg、Sloc、Sglb，分别代表分割结果的损失、局部损失、全局损失

在输入到聚合层之前，由于局部分支容易过拟合某些局部细节，从而影响全局分支的学习，即局部分支学习得比全局分支更快。所以在两个分支的最后一层之间加了一个弱耦合的正则化。是两个分支输出特征的欧几里得范数惩罚



**Coarse-to-Fine GLNet**

类别不平衡的问题：在 ISIC 数据集中，超过99%的图像背景像素比前景像素多，超过60%的图像中前景像素仅占到了不到20%。针对该问题，提出了一个 Two-Stage 的细化策略，即 Coarse-to-Fine GLNet。首先利用全局分支来实现对整体图像的粗分割，然后为分割得到的前景区域创建一个边界框 (在实验中，动态放宽边界框的范围，保证边界框区域内的前-背景像素比约为1)，根据边界框裁剪相应区域的图像作为局部分支的输入。



存在的问题

从局部到全局分支的深度特征共享，要将全部的块拼接后才能与全局特征串联。那么这是怎么训练的？

以我目前的理解是：两个分支，对应两个输入，一次forward生成一个特征图，那么在串联的时候，会出现一个已经训练好，然后等另一个完成的情况？？。

训练方式很有意思。（分三次训练，具体没有弄懂

**<span style='color:red'>代码</span>：**
