# PointNet++
This repo is implementation for PointNet and PointNet++ in pytorch.

### 三维点云技术概述

#### 3D representation

- Point Cloud 

- Mesh
- Volumetric
- Projected View（RGB-D）

2.5D RGB-D Depth Map

3D Point Clouds

#### 点云应用

- Robot Rerception

- Augmented Reality（AR）

- VR
- Shape Design
- FaceID

#### 点云处理任务

- Oject classification 物体分类
- Parts segmentation 部件分割
- Object detection 目标检测

- Semantic segmentation (场景语义分割)

#### 数据集

![image-20210320095044652](README/image-20210320095044652.png)

![image-20210320095112980](README/image-20210320095112980.png)

#### Challenges

- Irregular.近密远疏
- Unstructured
- Unordered
  - Invariance to permutations 置换不变性

#### Structured grid based learning

- Voxel based
- multiview based

- Point-Based Methods
  - PointNet 开山之作 MLPs --> Max pooling
  - PointNet++   Sampling --> Grouping
- Convolution-based Methods
  - 3D neighboring points
  - 3D continuous convolution
  - 3D discrete convolution
- Graph-based Methods
  - Input points --> Graph Construnction --> Feature Learning & Pooling --> Output Points

#### A taxonomy of deep learning methods for 3D point clouds

![image-20210320095238433](README/image-20210320095238433.png)

#### Chronological overview

![image-20210320095326616](README/image-20210320095326616.png)

![image-20210320095337746](README/image-20210320095337746.png)

![image-20210320095342478](README/image-20210320095342478.png)

### PointNet 点云处理原理

#### Previous Works

Point clud is converted to other representations before it's fed to ad deep neural network

| Conversion           | Deep Net        | Algorithm      | Shortcoming                            |
| -------------------- | --------------- | -------------- | -------------------------------------- |
| Voxelization         | 3D CNN          | VoxNet         | 栅格量化以后分辨率会降低，存在信息丢失 |
| Projection/Rendering | 2D CNN          | Multi-view CNN | Rendering个数增加计算量也随之增加      |
| Feature etraction    | Fully Connected |                |                                        |

#### PointNet

feature learning directly

- End-to-end learning for scattered, unordered point data
- Unified framework for various tasks

##### Challenges

- Unorder point set as input

  Model needs to be invariant to N! permutations

- Invariance under geometric transformations

  Point cloud rotations should not alter classification results

PointNet(vanilla)



#### Universal Set Function Approximator 

PointNet的网络结构能够拟合任意的连续集合函数

作者证明Max pooling的引入不会降低拟合其他函数的能力（通过将特征映射到高维）

#### Input Alignment by Transformer Network

Idea: Data dependent transformation for automatic alignment

![image-20210320104116589](README/image-20210320104116589.png)

#### Embedding Space Alignment

![image-20210320103935018](README/image-20210320103935018.png)

#### PointNet Classification Network

![image-20210320111456215](README/image-20210320111456215.png)

#### Extension to PointNet Segmentation Network

![image-20210320111633133](README/image-20210320111633133.png)

#### Model Size and Speed -- Light-weight & Fast

![image-20210320111717650](README/image-20210320111717650.png)

![image-20210320111725725](README/image-20210320111725725.png)

### PointNet++ 点云处理原理

PointNet没有local context

#### Basic idea

Recursively apply pointnet at local regions

- Hierarchical feature learning

- Translation invariant
- Permutation invariant

##### Hierarchical feature learning

![image-20210320134539377](README/image-20210320134539377.png)

![image-20210320134711517](README/image-20210320134711517.png)

Set Abstraction：sampling + grouping + pointnet

**PointNet++ PointNet++ 总体思路 :** 首先通过将点集划分为重叠的局部区域。 与CNN 相似，提取 局部特征以捕获来自小邻域的精细几何结构。 这些局部特征将进一步分组为较大的单 元，并进行处理以生成更高级别的特征。 重复此过程，直到获得整个点集的特征为止。

- Sampling
  - Uniform sampling
  - Farthest sampling
- Grouping
  - K nearest neighbors
  - Ball query（within range)

- Apply PointNet to each group

![image-20210320135533130](README/image-20210320135533130.png)

#### 对于非均匀点云的处理方法

- Multi-scale grouping（MSG）
- Multi-resolution grouping(MRG)

#### Network Architectures

- SSG:single scale grouping
- FP（feature propagation）

#### Results: Non-Euclidean Space

![image-20210320140404622](README/image-20210320140404622.png)

### 卷积感受野

在卷积神经网络中，感受野（Receptive Field）的定义是卷积神经网络每一层输出的特征图（feature map）上的像素点在输入图片上映射的区域大小。再通俗点的解释是，特征图上的一个点对应输入图上的区域，如图1所示。

![img](PointNet++%E7%82%B9%E4%BA%91%E5%A4%84%E7%90%86%E7%B2%BE%E8%AE%B2(PyTorch)/20190718164953811.png)