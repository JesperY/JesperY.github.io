---
title: '什么是似然'
date: 2024-09-22T17:01:16+08:00
draft: false
ShowToc: true
TocOpen: true
typora-root-url: ..\..\..\static
---



我们对概率（Probability）都很了解，描述一个事件发生的可能性。例如投掷一枚硬币，其正面向上的概率为 0.5，连续两次都是正面向上的概率为 0.25。

那么似然（Likelihood）是什么呢？似然其实也是描述了一种可能性，只是其描述的内容和概率有一点区别，接下来我们用更规范的方式来讨论一下什么是似然。

## 一、投硬币中的概率与似然

假设有一枚质地均匀的硬币，我们要做抛硬币的实验，令正面向上为事件 A ，反面向上为事件 B，自然我们有如下结论：
$$
P(A)=0.5\\
P(B)=0.5\\
P(A+B)=1
$$
我们不会深入讨论概率的定义等内容。那么我们是如何得出以上结论的呢？

你可能会说这是显然的，也有人会说可以通过实验验证，无限次投掷硬币，其频率逼近概率。

不管怎么样，没有人会质疑以上结论，因为我们都知道理想下质地均匀的硬币符合上面三个公式，我们姑且称这三个公式构成了一个 **模型**，这个模型描述了掷硬币这个实验。

假如硬币 **不均匀** 呢？此时 $P(A)=?$，没有人知道。

因此我们假设一个参数 $\mu$，令 
$$
P(A) = \mu\\
P(B) = 1-\mu\\
P(A+B) = 1
$$
我们又得到了一个模型，用来描述一个投掷不均匀硬币的实验。但是其中有一个未知的参数 $\mu$，我们无法确定。但是我们可以依靠蒙特卡洛方法来近似这个参数。只要我们的样本无限多，那么样本参数就会无限逼近总体参数。

但很显然我们无法进行无限次实验，因此我们无法得到绝对正确的参数。

但我们仍然要做，即使不是百分百准确，我们也需要一个样本参数。于是我们做了10次实验，观察发现其中7次硬币向上，3次硬币向下。

经过分析我们认为 $\mu=0.7$ 是一个 **可能性很高** 的取值。

**可能性**，在刚刚的结论中我用到了这个词。前面描述质地均匀的硬币在投掷时正面向上的可能性为 0.5，现在我描述样本参数取值为 0.7 的可能性，但是并没有一个具体的值来描述这个可能性多高。

这里我们已经窥探到似然的影子了，通过刚才的实验分析，我们成功的给模型的参数取值赋予了一个可能性，但是这个可能性并不是一个绝对值，而是一个相对值，因为 $\mu=0.7$ 这个结论的可能性比较高是相对于其他的结论，例如相比 $\mu=0.5$，$\mu=0.7$ 正确的可能性更高。

这就是似然的一个特点，**不同于概率，似然的可能性是相对于其他选择而言的，因此其并没有固定值，所有可能取值的似然之和也并不一定为 1。**

同时我们也在一定程度上了解了似然的定义，即似然是在已知实验结果的情况下，描述模型参数取值的可能性。

我们已经知道，当实验的次数趋于无限次时，样本参数会逼近总体参数，因此参数取值的可能性和样本数量是有关的，样本越多，取到正确参数的可能性越大，但我们无法进行无限次实验，因此在已有的样本结果中，找到最可能的参数，就是我们的目标。

## 二、似然函数

假设我们有一些实验，例如我们掷硬币掷了 10 次，其中 7 次正面向上，3 次反面向上。现在我们想要知道这个掷硬币模型的参数，很显然我们使用全部样本结果才能得到目前可能性最高的参数估计。

现在我们假设模型参数 $P(A)=\mu, P(B)=1-\mu$ 。掷硬币是一个独立同分布的实验（我们不深入讨论独立同分布），那么对于已有的实验结果，我们可以给出如下公式：
$$
P(X|\mu)=\prod\limits_{n=1}^{10}B(x_n|10，\mu)
$$
其中 $X$ 表示 $AAAAAAABBB$ 十次实验的样本，$B(x_n|10,\mu)$ 表示每个样本 $x_n$ 服从一个参数为 $10,\mu$ 的二项分布。

那么在已有样本结果下，根据乘法法则，我们可以自然的得出上述公式。这个公式被称为似然函数，因为它是一个关于参数 $\mu$ 的函数，表示了在 $B(x_n|\mu)$ 分布下进行十次实验，得到目前已有的实验结果的概率。因为实验结果是已知的，而参数是未知的，因此我们要找到一个令该函数最大的参数，这就是极大似然。

为了方便处理，我们两边取对数：
$$
lnP=\sum_{n=1}^{10}B(x_n|10,\mu)=\sum_{n=1}^{10}(
\begin{array}{c}
n\\
10
\end{array})\mu^k(1-\mu)^{10-n}
$$
对数函数总是单调的，因此求解上述对数似然的极大值就可以求得在目前已知样本下最可能的参数的取值，这也就是**极大似然估计**。

以上讨论的都是离散模型，连续模型的推导过程同理。