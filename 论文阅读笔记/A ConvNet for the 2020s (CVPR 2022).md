A ConvNet for the 2020s（CVPR2022）

博客+略读

[CVPR2022 | A ConvNet for the 2020s & 如何设计神经网络总结](https://zhuanlan.zhihu.com/p/528057027)

[【论文简述及翻译】A ConvNet for the 2020s(CVPR 2022)](https://blog.csdn.net/qq_43307074/article/details/127247752)



## 值得注意的点

**分层 Transformers** ——**Swin Transformers**
"Swin Transformers" 是一种用于图像处理的深度学习模型，其全称为 "Swin Transformer"。这个模型由微软研究院于2021年提出，并在论文 "Swin Transformer: Hierarchical Vision Transformer using Shifted Windows" 中进行了详细描述。这篇论文由Ze Liu、Yutong Lin、Yi Zhang、Han Hu等人撰写。

Swin Transformer 的设计灵感来自于对 Vision Transformer (ViT) 模型的改进。它引入了一种称为 "shifted windows" 的机制，以替代传统的均匀划分图像的方式，从而更好地捕捉图像中的局部和全局信息。Swin Transformer 通过分层的方式组织注意力机制，使得模型能够有效地处理不同尺度的信息。

此外，Swin Transformer 在计算效率上也进行了优化，通过使用局部移位操作（shift）来减少计算复杂度。这使得模型在处理大尺寸图像时更加高效。

Swin Transformer 在许多计算机视觉任务上取得了很好的性能，例如图像分类、目标检测和语义分割。这个模型的提出进一步推动了对于使用 Transformer 架构处理视觉任务的研究。





**ViT**

最大的挑战是Vit的全局注意力设计，他在输入大小方面举有二次复杂度。对于imageNet分类这是可以接受的，但在处理更高分辨率的输入图像时就会变得非常难以接受。

所以出现了分层Transformers，采用混合方法来弥补这一差距。



**ConvNets失去动力的原因**

ConvNets似乎失去动力的唯一原因是（分层）在许多视觉任务中Transformers超过了它们，而性能差异通常归因于Transformers卓越的缩放行为，其中多头自注意力是关键组成部分。





## 博客总结

**4. 关键词：**ConvNet、Transformers、CNNs、数据集

**5. 探索动机：**在2020年，视觉Transformers，尤其是Swin Transformers等分层Transformers开始取代ConvNets，成为通用视觉主干的首选。人们普遍认为，视觉Transformers比ConvNets更准确、更高效、更易扩展。但是大量的工作其实是将之前运用在CNN网络结构上的思路改进到Transformers结构当中去。而且现在有更多的数据，更好的数据增强，以及更加合理的优化器等但所以vision Transformers之所以能够取得SOTA的效果，会不会是这些其他因素影响了网络。Transformers到底是厉害在哪了？

**6. 工作目标：**如果把这些用在transformer上的技巧用在CNN上之后，进而重新设计ConvNet，卷积能达到的效果的极限是在哪里？是否也能得到相似的结果呢？

**7. 核心思想：**ConvNeXt在ResNet50模型的基础上，仿照Swin Transformers的结构进行改进而得到的纯卷积模型。模型改进可以分为：整体结构改变、层结构改变和细节改变三大部分。

**8. 实现方法：**

（过了一遍，发现里面的很多东西都没有具体的解释，也可能是在这个领域大家都知道所以没有解释。但需要知道的是，这些方法都对CNN的效果是有帮助的，在之后的网络设计中可以参考。）

**Training Techniques**

**Macro Design**

**ResNeXt-ify**

**Inverted Bottleneck**

**Large Kernel Sizes**

**Micro Design**

**9. 实验结果：**ConvNeXt是一个借鉴Transformers网络的CNN模型，从作者的实验看出，每一点精度的提升都是经过大量的实验。虽然原生模型是一个分类模型，但是其可以作为backbone被应用到任何其它模型中。通过一系列实验比对，在相同的FLOPs下，ConvNeXt相比Swin Transformers拥有更快的推理速度以及更高的精度，在ImageNet 22K上ConvNeXt-XL达到了87.8%的准确率。













## 个人总结：

（旨在学习如何设计CNN网络架构）

作者想要将CNN架构和Transformers做对比，研究Transformers到底强在哪里？CNN的极限又在哪里。

所以论文通过仿照Swin Transformers结构，对CNN进行修改，以达到更高更快的效果。









