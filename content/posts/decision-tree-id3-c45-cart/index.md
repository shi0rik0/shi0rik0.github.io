---
title: "3种决策树：ID3、C4.5和CART"
date: 2024-07-07T21:13:55+08:00
draft: false
---

假设有以下数据集，记录着一些人的种族、收入、是否有子女以及他们是否有购买保险：

| Race | Income | Child | Insurance |
| ---- | ------ | ----- | --------- |
| B    | H      | N     | Y         |
| W    | H      | Y     | Y         |
| W    | L      | Y     | Y         |
| W    | L      | Y     | Y         |
| B    | L      | N     | N         |
| B    | L      | N     | N         |
| B    | L      | N     | N         |
| W    | L      | N     | N         |

我们希望构造一棵决策树，来根据种族、收入、是否有子女这三个属性来预测一个人是否有购买保险。构造决策树的关键问题在于决定分叉处要根据哪个属性来进行分类。常见的选择方法有3种：ID3、C4.5和CART。

在下文中，将属性集$\\{\text{Race}, \text{Income}, \text{Child}\\}$记作$D$。将要预测的目标Insurance记作$T$。

## ID3

ID3选择属性的方法是：
$$
\argmax_{X\in D} H(T) - H(T|X) = \argmin_{X\in D} H(T|X)
$$

## C4.5

C4.5选择属性的方法是：
$$
\argmax_{X\in D} \frac{H(T) - H(T|X)}{H(X)}
$$

## CART

CART选择属性的方法是：
$$
\argmax_{X \in D} G(T) - G(T|X) = \argmin_{X \in D} G(T|X)
$$

其中$G(X)$是Gini指数，其定义为：
$$
G(X) = 1- \sum_{x} p^2(x)
$$

例如$G(T) = 1-(\frac{1}{2})^2-(\frac{1}{2})^2=\frac{1}{2}$。

条件Gini指数的定义和条件信息熵类似：
$$
G(X|Y)=\sum_y p(y)G(X|Y=y)
$$