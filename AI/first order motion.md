# First Order Motion

## 关于对论文拆分解读

1. Ttitle 标题 + Abstract 摘要 + Introduction (该文章中 Introduction 无重要相关信息) + Conclusion 结尾 + Fig 图+ Table 表（图和表结合 2 Method 来看）

2. Introduction ～ 剩下部分 +  Related work 相关工作 + Method 方法 + Experiments 实验 / Resulted 结果 / Analyze 分析

   

 ## 关键词解释

* 一阶运动

  方程式子图解（形如对某个点展开，这种就是一阶，因为它后面 d / dx 没有更高阶的项，微分式子为一阶；x + x 方，x 方为二阶）

* 一阶运动模型

  使用一组自学习的关键点和局部仿射转化来建模复杂运动，因此称方法为一阶运动模型

* 先验信息

  指获得样本的试验之前，获得的经验和历史资料

* 遮挡感知生成器

  采用一个自动估计的遮挡掩码来指示在源图像中不可见的物体部分，并且应从上下文中中推断出来

* **图像扭转 Iamge warping**

  对一个图像进行扭转 AX，获得新图 x ‘ = AX 的时候，rotation 和 translation 相加的 images 对图像做的变化。主要还是为了生成新的图和原图角度啊，或者说在一些形状上有些变化
  
  

## Abstract摘要 

摘要说图像动画的目的是为了生成一段视频序列，让这个视频序列中的物体能够动起来

那它是靠什么动呢？它是依靠驱动视频中的运动

> 视图说明

S：😑 😊 😁 

 假如原图中人脸是中性的人脸，无任何表情😑。如果驱动的视频中的人脸从没有表情到一个很小的想要笑的表情，到一个很大笑的表情。那它最终的任务是希望在一个原人脸来产生一个慢慢变大笑的表情



> 总结

通过静止图像中设置对象的动画来生成视频，更准确说：图像动画是指通过将源图像中提取的**外观**与从**动态视频**中提取的运动模式相结合，来自动合成视频。因此，图像动画在我们很多的视频创作、电影制作、摄影和电子商务领域都能有着广泛的应用



> 补充说明

视频中展示了一阶运动模型的工作，它从驱动视频 Driving Video 中提取目标人物的目标运动。这个演示就是由这项工作产生的，它使用的方法不需要任何标签 labels 。关于图像中对象有关的注释或知识，方法支持任何各种对象，一旦经过训练，能很好是用于同一类别任意对象（即：一阶运动模可以提取到与对象有关的注释、知识等信息，方法支持任何对象。获取到的有关训练信息，能应用到有关同一类别的任意对象）

对于视频中的面部，first order motion model 能成功提取并重新定位面部表情、头部姿势、甚至于视频人物中的眼球（即对象有关的注释、知识等信息）。为了成功创建动画视频，模型从源图像或我们想要的驱动视频中制作动画图像所需的运动，来用于多个对象类别的使用，这就是使用 Pryor 工作方法生成重定向的结果，通过使用特定对象的先验信息解决问题



>  这个工作如何实现

在关于这个 Model 的论文介绍者说，这个工作有一个最大的好处：不需要任何标注 any annotation 也不需要任何的先验信息 prior information。在工作中先去解耦 decouple 外部的外貌 和运动信息，通过一种自监督的方式 a self-supervised formulation 

同时，这自监督方式是没有任何标注的，那没有任何标志自监督如何提取运动信息呢？作者说它是获得一个表征，这个表征是由关键点 learned keypoints 和局部仿射变换 local affine transformation 。在这个过程中使用到一个生成器网络，生成器网络能对目标运动的中产生的遮挡显示建模，并将从源图像中提取的外貌特征和我们刚刚提到的运动特征相结合



## Conclusions 结论 

1. **数学表示的创新**：在这项任务中，为了实现图像动画，作者引入了一种新的数学表示方法，用于描述两个图像帧之间的运动场。这个数学表示是通过一阶泰勒展开来推导的。在当时，这是第一次真正出现的一阶泰勒展开方法，这也是这篇论文的一个重要创新点。一阶泰勒展开是一种近似计算方法，用于描述图像之间的运动关系，为后续的图像动画任务提供了重要的数学工具

2. **关键点信息**：论文中提到，关键点信息在图像动画任务中扮演了重要的角色。关键点可以被视为图像中的显著点，用于跟踪物体的位置和姿势随时间的变化。这些关键点信息有助于构建运动场表示，并在动画过程中实现对象的平滑运动

3. **局部仿射变换**：另一个重要的概念是局部仿射变换。这些变换用于描述物体的形状和姿势的变化，特别是在动态场景中。局部仿射变换提供了更详细的运动信息，有助于更准确地重建动态场景

4. **数学表示的应用**：这种新的数学表示方法被成功地应用于图像动画任务中，为创建流畅的图像动画提供了基础。通过将关键点信息和局部仿射变换与一阶泰勒展开相结合，模型能够生成连续的图像帧，呈现出自然的动态效果

   

## Introduction 介绍

它的介绍中讲到几个关键的点，还是我们刚刚介绍的几个东西，它在这里，这篇文章说有四个比较重要的点：

1. to use a set of self-learned keypoints together with local affifine transformations to model complex motions 用了**自监督**的方式，能够学习到**关键点**的信息和***局部仿射的变换**
2. an occlusion-aware generator 建模了一个遮挡，采用一个自动估计模型的遮挡掩模来指示目标部分，在源图像中是不可见的，需要从上下文推断
3. extend the equivariance loss commonly used for keypoints detector training  扩展了一次性损失（扩展关键点检测器训练中的等方差损失），把一次性损失扩展到了局部仿射变换上，在论文中说取得了很好的成绩
4. a new high resolution dataset, *Thai-Chi-HD* 提出了一个新的高分辨率数据集

## Related work 相关工作

1. **带有VAE的循环神经网络（ Recurrent Neural Network with a VAE ）**：这个方法结合了循环神经网络（ RNN ）和变分自编码器（ VAE ）的技术。VAE 用于学习潜在表示，而 RNN 用于捕获图像序列中的时间依赖关系。这个方法的应用领域可能包括图像生成和动画

2. **MoCoGAN** ：MoCoGAN 是一种用于生成连续图像帧序列的方法。它可能是一种生成对抗网络（ GAN ）的变体，用于图像动画任务。GAN 是一种生成模型，用于生成逼真的图像

3. **图像扭转（Image Warping）**：图像扭转是一种重要的技术，用于处理图像的几何变换。在图像动画中，它可以用于实现对象的形状变换和运动效果，是模拟运动场景的关键工具之一

4. **X2Face** ：X2Face 是一个在图像动画领域的重要工作。它提出了一种密集的运动场模型，用于捕获人脸表情的细微变化。这个方法可能对表情合成和动画效果有重要影响

5. **Monkey-Net** ：Monkey-Net 是另一个在图像动画领域的方法，它提出了一种稀疏的自监督关键点轨迹模型。这个模型可能用于追踪对象的运动和姿势

   

## Method

> 关于步骤和公式的解释

1. **问题背景**：该方法的目标是将一个源图像（ Source ）转化为一个驱动图像（ Driving Frame ）。这两个图像通过一个 encoder-decoder 网络传递，生成关键点（ Keypoints ）和仿射变换（ Affine Transformations ）的信息
2. **一阶泰勒展开 First Order Taylor Expansion **：这个方法的核心是一阶泰勒展开。它是一个数学技术，用于近似变换的效果。在这篇论文中，泰勒展开被用于捕获图像之间的运动关系
3. **关键点和仿射变换的生成（ Keypoints and Affine Transformations Generation ）**：源图像和驱动图像经过encoder-decoder网络后，生成关键点和仿射变换的信息。这些信息将在后续步骤中用于构建运动场
4. **中间帧的生成（ Intermediate Frame Generation ）**：为了得到从源图像到驱动图像的完整变换，方法引入了中间帧。源图像和驱动图像都与中间帧参考。这个步骤帮助建立运动场
5. **稀疏运动场的优化（ Sparse Motion Field Optimization ）**：方法得到了稀疏运动场，但需要进一步优化，以获得更丰富的运动信息。这个过程涉及计算关键点之间的差异和变换，以创建更密集的运动场
6. **稠密运动场的生成（ Dense Motion Field Generation ）**：通过将稀疏运动场传递到另一个网络，称为 " Dense Motion " 网络，生成稠密的运动场。这个运动场描述了图像中的对象运动
7. **图像动画生成（ Image Animation Generation ）**：稠密运动场和源图像一起，通过反向扭曲（ backward warping ）生成最终的动画效果。这涉及到对图像的几何变换和重建
8. **遮挡建模（Occlusion Modeling）**：方法还显式地建模了遮挡效应，即某些区域被其他物体或背景遮挡。这个过程帮助模型识别哪些区域没有被遮挡，哪些区域被遮挡
9. **自监督学习（Self-Supervised Learning）**：该方法是自监督学习的一种形式，因为它使用来自同一视频的帧来进行训练。模型在训练过程中学习生成关键点和仿射变换，以重建驱动图像的输入帧
10. **最终输出建模（Final Output Modeling）**：最后，模型使用生成的关键点、仿射变换和稠密运动场，以及源图像的编码特征来生成最终的图像动画输出

在这些步骤中，一阶泰勒展开是方法的核心数学技术，用于捕获图像之间的运动关系。这个方法的主要目标是实现流畅和逼真的图像动画效果，通过多个步骤的组合来实现

## Losses

在使用 First Order Motion 模型时，作者使用了多个损失函数来训练模型，以确保生成的图像具有高质量和一致性。以下是对损失函数的一些补充信息：

1. **重建损失（ Reconstruction Loss ）**：类似于许多 GAN 任务，作者在计算损失时引入了重建损失，以确保生成的图像与原始图像的身份信息保持一致。这有助于生成的图像在外观和运动方面保持一致性

2. **一致性损失（ Consistency Loss ）**：作者扩展了一致性损失，将其应用于仿射变换，以确保生成的图像在变换过程中保持一致性。具体公式详见论文中的公式13

3. **运动估计损失（ Motion Estimation Loss ）**：在生成最终图像的过程中，模型的任务是估计驾驶图像（ Dt ）和源图像（ S ）之间的差异。为了处理这个复杂的任务，作者采用了一种迭代的方法。首先，他们估计自身驾驶图像中第一帧和第T帧之间的差异，然后使用这个信息来估计 St 和 S1 之间的差异，从而实现相对的运动变换

这些损失函数的使用有助于确保生成的图像在外观和运动上保持一致性，提高了模型的性能和生成图像的质量。这个方法的复杂性要求采用多层次的损失函数来处理不同方面的一致性

## 数据集实验

然后最后它在四个数据集上做了实验

* VoxCeleb  人脸数据集

  The VoxCeleb dataset  is a face dataset of 22496 videos

* UvA - Nemo 人脸

  The UvA-Nemo dataset  is a facial analysis dataset that consists of 1240 videos

* BAIR 机械手臂

  The BAIR robot pushing dataset contains videos collected by a Sawyer robotic arm pushing

* Tai-Chi-HD 太极拳数据集



## 关于训练和报错

> 关于训练

如果对 [first-order-model](https://github.com/AliaksandrSiarohin/first-order-model) 感兴趣的话，可以上 GitHub 上搜索开源项目，拉到自己本地中进行图像动画模型生成和训练，在这里使用的是 Colab 演示

> 关于报错

因为现在使用电脑是 mac，macOS 并不支持 NVIDIA（英伟达） 的 CUDA GPU 加速，因为大多数 Mac 电脑使用的是 AMD 或者集成显卡（Intel Iris Graphics），而不是 NVIDIA 的 GPU。CUDA 是一种用于 NVIDIA GPU 的并行计算平台，用于加速深度学习和科学计算等任务。因此，在大多数情况下，Mac 上无法使用 CUDA。所以在模型的使用过程中会出现下述报错：

在尝试运行一个使用了 CUDA 的脚本，但您的 Torch 并没有编译启用 CUDA 支持的报错信息

> 运行代码相关解释

解释一下关于 [first-order-model](https://github.com/AliaksandrSiarohin/first-order-model) 的模型使用中的演示代码

可以看到我新建看 driving_video、source_image、form_checkpoints 三个文件夹，分别用于存放对应的驱动视频、源图像和对应的预训练检查点

因此在实际运行中，我们要对动画演示代码进行二次修改，比如 github 上给出的训练代码为：

```bash
python demo.py  --config config/dataset_name.yaml --driving_video path/to/driving --source_image path/to/source --checkpoint path/to/checkpoint --relative --adapt_scale
```

随着创建好后，我们要根据实际存放文件目录重新书写运行，选择正确配置文件：

```bash
python demo.py --config config/vox-256.yaml --driving_video driving_video/crop.mp4 --source_image source_image/source.jpeg --checkpoint form_checkpoint/vox-adv-cpk.pth.tar --relative --adapt_scale
```

> 关于对应参数解释

* **vox-256.yaml** ：VoxCeleb 数据集

  Tips：（指路数据集实验）比如实现太极动作迁移就用太极视频数据集进行训练，想要达到表情迁移的效果就使用人脸视频数据集 voxceleb 进行训练

*  **driving_video/crop.mp4**：对齐裁剪驱动视频

*  **source_image/source.jpeg**：源图像中源文件中

* **checkpoint form**：检查点文件，因为我使用的是 Vox 事件，所以文件类型键为 vox-adv-cpk.pth.tar 

> 面部对齐

在使用模型的过程中，可以在 Github 上面看到关于 Animation Demo 动画演示中有一段话：

*The driving videos and source images should be cropped before it can be used in our method.... face-alligment library is needed*

作者提到了使用该模型时需要对驾驶视频和源图像进行裁剪，并建议使用面部对齐库来处理它们（用于在图像处理中进行面部对齐，**[face-alignment](https://github.com/1adrianb/face-alignment)** 这个库可以帮助在驾驶视频和源图像上进行必要的预处理，以确保模型的准确性）来进行视频的面部对齐。这个过程对于构建 2D 和 3D 图像动画非常重要

> 关于 MyHeritage

[MyHeritage](https://www.myheritage.com/) 为一个家谱和家庭历史研究平台，平台提供了一系列工具和资源，帮助用户探索他们的家庭树、家庭历史和遗传信息。特别是，该平台中允许用户将老照片转化为动画，这正是基于类似于 First Order Motion 的技术实现的。这个功能为用户提供了一种有趣的方式来探索他们的家庭历史

# 运行环境

First Order Motion 是一个用于图像和视频动画的深度学习模型，它基于神经网络技术。要在你的计算机上运行 First Order Motion，你需要以下运行环境：

1. **Python**：First Order Motion 是基于 Python 编写的，所以你需要安装 Python。建议使用 Python 3.6或更高版本

2. **深度学习框架**：First Order Motion使用深度学习框架来训练和运行模型。通常情况下，它使用了PyTorch或TensorFlow。你需要安装其中一个框架，根据模型的具体实现来选择

3. **GPU**：虽然 First Order Motion 可以在 CPU 上运行，但要实现实时或高性能的动画生成，推荐使用支持 CUDA 的 NVIDIA GPU。这将显著加速模型的训练和推理过程

4. **依赖库**：你需要安装一些 Python 库，例如 NumPy、OpenCV、dlib 等，以处理图像和视频数据，以及在模型训练和推理过程中进行必要的计算

5. **模型和权重**：你需要下载或训练 First Order Motion 模型的权重。通常，这些权重会由模型的作者提供，你需要将它们加载到你的环境中

6. **训练数据**：如果你想要使用自己的数据来训练模型，你需要具有相应的训练数据集

   
