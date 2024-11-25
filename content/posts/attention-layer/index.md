---
title: "Attention层的简单讲解"
date: 2024-10-02T14:57:00+08:00
draft: false
---

## 输入与输出的维度

Attention 层是用来处理序列输入的，所以它接受的输入是一个二维的$n \times N_i$的矩阵，而输出则是一个$n \times N_o$的矩阵。$n$是输入序列的 token 个数，$N_i$则是 embedding 向量的长度，$N_o$则是输出的状态向量的长度。理论上 Attention 层对于$n$没有限制，而$N_i$和$N_o$则是模型的超参数。

## Attention 层的超参数

除了$N_i$和$N_o$以外，Attention 层还有其他超参数。

一个 Attention 层包含 3 个全连接层，分别叫做 Query、Key 和 Value，用符号$Q$、$K$和$V$表示。这 3 个全连接层的输入维度都是$N_i$，而输出维度都是超参数，分别为$D_q$、$D_k$和$D_v$。稍后会看到，Attention 层要求$D_q=D_k$以及$D_v=N_o$。

综上所述，Attention 层的超参数有$N_i$、$N_o$和$D_q$。

## 推理过程

我们将输入的向量依次记作$x_1$、$x_2$……$x_n$，同理输出的向量记作$y_1$、$y_2$……$y_n$。

推理的时候，首先要将输入向量都经过全连接层一次，得到$Q(x_1)$、$K(x_1)$、$V(x_1)$等等。最终的输出是由$V$的结果加权求和得到的：

$$
y_i = \sum_j \alpha_{ij} V(x_j), \forall i
$$

因为$\alpha_{ij}$是权重，所以自然要满足：

$$
\sum_j \alpha_{ij} = 1,\forall i
$$

$\alpha_{ij}$是根据$Q$和$K$的结果得到的：

$$
\alpha^\prime_{ij} = \frac{Q(x_i) \cdot K(x_j)}{\sqrt{D_q}}, \forall i, j
$$

这里我用的符号是$\alpha^\prime$而非$\alpha$，因为$\alpha^\prime$不满足权重和为 1 的要求，因此不是最终结果。要让和为 1，使用 softmax 层处理一下即可。

（计算$\alpha$要除以$\sqrt{D_q}$是为了归一化。）
