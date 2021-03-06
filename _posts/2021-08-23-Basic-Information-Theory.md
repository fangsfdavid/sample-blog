---
title: "信息论基础"
description: 信息论基本概念
comments: true
hide: false
toc: true
layout: post
hide: false
categories: [information theory, machine learning]
image: images/fastpages_posts/codespaces/codespaces.png
author: "<a href='https://github.com/fangsfdavid'>林胜联府</a>"
permalink: /basic-information-theory
---

总结一些基本概念:

- 自信息
- 信息熵
- 联合熵
- 条件熵
- 互信息
- 条件互信息
- 交叉熵

## 自信息

自信息是对某一事件发生时所带来的信息量做了一个量化. 信息是一个比较抽象的概念, 一条信息所包含的信息量和它的不确定性有直接的联系, 而自信息就是把信息的度量等价于对不确定性减少程度的度量.

设$X$是满足概率分布$p(x)$的离散随机变量，其所有可能取值集合构成集合$\mathcal{X}$，
则事件$\{X=x\}$的自信息的定义如下：

$$
I(p(x)) = -\log(p(x)).
$$

其中$p(x)$表示$P(X=x)$.

直接观察可知，自信息的定义与如下信息的三个特点是相一致的：

- 消息x发生的概率$p(x)$越大，信息量$I(x)$越小，反之信息量$I(x)$越大
- 当概率$p(x)=1$时，信息量$I(x)=0$
- 多个独立的消息所包含的信息量应等于各个消息所含信息量之和，即

$$
p(x,y)=p(x)p(y)\Longleftrightarrow I(x,y)=I(x)+I(y)
$$

## 信息熵

信息熵是考虑随机变量X的所有可能取值，对它们按概率分布加权平均：

$$
      H(X) = -\sum\limits_{x\in\mathcal{X}} p(x)\log{p(x)}.
$$

也就是对所有可能发生事件所带来的信息量做一个期望。信息熵具有如下特点：

- 如果一个随机变量的取值越少，其信息熵越小
- 如果一个随机变量越接近于均匀分布，其信息熵越大

信息熵只和随机变量的分布有关系，和随机变量的具体取值并无关系，包括后面定义的条件熵和交叉熵等等。

信息熵具有如下性质：

1. $H(X)\geq 0$
2. $H_b(X)=(\log_ba)H_a(X)$

即信息熵都是正值，且从第二条性质来看，计算信息熵时的基底选择影响不是很大。

## 联合熵

将一维随机变量分布推广到多维随机变量分布，就得到了联合熵：

$$
  H(X,Y) = -\sum\limits_{x\in\mathcal{X}}\sum\limits_{y\in\mathcal{Y}}
  p(x,y)\log{p(x,y)}.
$$

类似地可以定义$n$个随机变量的联合熵：$H(X_1,X_2,\ldots,X_n)$.

## 条件熵

条件熵表示在已知随机变量X的条件下随机变量Y的不确定性,其定义如下：

$$
   H(Y|X) = \sum\limits_{x\in\mathcal{X}} p(x) H(Y|X=x)
$$

也就是以随机变量$X$为条件，随机变量$Y$的条件概率分布的熵关于$X$的数学期望。

直接推导，可知信息熵、联合熵和条件熵具有如下关系

$$
   H(X,Y)= H(X) + H(Y|X)
$$

更一般地，

$$
  H(X_1,X_2,\ldots,X_n) = \sum\limits_{i=1}^n H(X_i|X_{i-1},\ldots,X_1)
$$

上式称为信息熵的链式法则。


**注：** 根据上面的推导可知：

$$ 
 H(Y|X)=H(X,Y)-H(X)
$$

假设系统中只有两个变量$X,Y$，其中$X$为输入变量，$Y$为输出变量，
那么联合熵$H(X,Y)$ 表示整个系统的不确定性，信息熵$H(X)$ 表示输入变量$X$的不确定性，条件熵就刻画了知道输入变量后输出变量的不确定性，机器学习经典的最大熵原理就是在给定一些输入条件后，最大化输出变量的条件熵。

**注：** 根据后面互信息的性质以及信息熵、互信息和条件熵之间的关系:

$$0\leq I(X;Y)=H(X)-H(X|Y)$$

可知

$$H(X|Y)\leq H(X)$$

这也就是说，在知道另一个随机变量$Y$的信息后，可以降低随机变量$X$的不确定性，这一点很关键。

## 相对熵（KL散度）

相对熵也叫KL散度，可以用来衡量两个概率分布之间的差异。设X是离散随机变量，其所有取值构成集合$\mathcal{X}$，$p(x)$和$q(x)$ 是离散随机变量$X$的两个概率分布，则p对q的相对熵是：

$$
  D_{KL}(p||q)=\sum\limits_{x\in\mathcal{X}}p(x)\log{\frac{p(x)}{q(x)}}
$$

可以证明

$$
  D_{KL}(p||q)\geq 0,
$$

等号成立当且仅当$p(x)=q(x), \forall x\in\mathcal{X}$。我们可以使用相对熵来做为两个分布之间距离的某种近似刻画。需要注意的是相对熵并不是两个概率分布之间的距离，因为它并不满足对称性，即

$$D_{KL}(p||q)\neq D_{KL}(q||p)$$

除此之外，类似于信息熵，相对熵也有链式法则：

$$
  D_{KL}((p(x,y)||q(x,y)) = D_{KL}(p(x)||q(x)) + D_{KL}(p(y|x)||q(y|x))
$$

## 互信息

下面我们来介绍互信息，它是用来刻画一个随机变量$X$中所包含的关于另一个随机变量$Y$的信息量，其描述了两个随机变量之间的依赖程度。其定义公式如下：

$$
    I(X;Y)=\sum\limits_{x\in\mathcal{X}}\sum\limits_{y\in\mathcal{Y}}
  p(x,y)\log{\frac{p(x,y)}{p(x)p(y)}}
$$

根据相对熵的定义可知，互信息就是某种KL散度：

$$
   I(X;Y)=D_{KL}(p(x,y)||p(x)p(y))
$$

相对熵是用来比较两个概率分布之间的差异的，而由互信息的定义可知，互信息其实就是比较联合概率分布$p(x,y)$ 和边际概率分布之积$p(x)p(y)$ 之间的差异：

$$
  \mathbf{p(x,y)\overset{?}{=}p(x)p(y)}
$$

除此之外，我们可以从条件熵地角度来理解互信息，通过简单的推导可知互信息和相对熵有如下关系：

$$
\begin{aligned}
    I(X;Y) &= H(X) - H(X|Y) \\
	&= H(Y) - H(Y|X) \\
	&= H(X) + H(Y) - H(X,Y)\\
\end{aligned}	
$$

从上式可以看出，互信息刻画了已知一个随机变量的信息后另一个随机变量不确定性的减少程度，也就是对一个随机变量中所包含另一个随机变量的信息做出了一个量化。

根据KL散度的性质可知互信息具有如下性质：

1. $I(X;,Y)\geq0$
2. $I(X;Y) = I(Y;X)$

类似于信息熵，互信息也有链式法则：

$$
  I(X_1,X_2,\ldots,X_n;Y) = \sum\limits_{i=1}^nI(X_i;Y|X_{i-1},X_{i-2},\ldots,X_1)
$$

例：

$$
  I(X_1,X_2,X_3;Y) = I(X_1;Y) + I(X_2;Y|X_1) + I(X_3;Y|X_1,X_2)
$$

互信息刻画两个变量之间的非线性依赖关系，而条件互信息则是在互信息的基础上，考虑背景变量$Z$已知的情况下变量$X$和$Y$之间的关系：

$$
  \mathbf{p(x,y|z)\overset{?}{=}p(x|z)p(y|z)}
$$

具体地，条件互信息的定义如下

$$
  I(X,Y|Z) = \sum\limits_{z\in\mathcal{Z}}p(z)\sum\limits_{y\in\mathcal{Y}}\sum\limits_{x\in\mathcal{X}}p(x,y|z)\log\frac{p(x,y|z)}{p(x|z)p(y|z)}
$$

直接计算可知

$$
\begin{aligned}
I(X;Y|Z) &= H(X,Z) + H(Y,Z) - H(X,Y,Z) - H(Z) \\
&= H(X|Z) - H(X|Y,Z) \\
&= H(Y|Z) - H(Y|X,Z) \\
&= H(X|Z) + H(Y|Z) - H(X,Y|Z)		
\end{aligned}
$$

从上式可知，条件互信息表示在随机变量$Z$已知的情况下，随机变量$Y$的信息对随机变量$X$ 的不确定的减少的量化度量，或者随机变量$X$的信息对随机变量$Y$的不确定性减少的量化度量。除此之外，条件互信息是定义两个随机过程之间转移熵的基础。


## 冗余

冗余、边际冗余和条件冗余其实是互信息和条件互信息的高维推广。互信息主要是用来研究两个变量之间的依赖关系，为研究多个变量之间的关系，
Fraser 和Prichard 等人引入了冗余的概念。冗余的出发点是研究下面这个问题

$$
  \mathbf{P(x_1, x_2, \ldots, x_n) \overset{?}{=} P(x_1)P(x_2)\ldots P(x_n)}
$$

其中$P(x_1, x_2,\ldots, x_n)$ 是n个变量的联合概率分布，$P(x_1),P(x_2), \ldots,P(x_n)$ 表示单个变量的概率分布。针对上述问题，Prichard1995Generalized提出了冗余的定义

$$
  R(x_1;\ldots;x_m)= \sum\limits_{i}H(x_i) - H(x_1;\ldots;x_m)
$$

其中$H(x_i), H(x_1;\ldots;x_m)$分别表示信息熵和联合熵。回顾下熵是用来衡量不确定性的，不确定性的减少意味着信息量的增加，从定义可以看出，冗余表示了系统变量被同时测量
相对于被单独测量所能带来的额外的信息量。由联合熵的定义可知：$H(x_1;\ldots;x_m)\leq\sum\limits_{i}H(x_i)$，于是$R(x_1;\ldots;x_m)\geq0$；且但这m 个变量之间互不相关时，冗余为零。

边际冗余的定义如下

$$
  R_M(x_1,\ldots,x_{m-1};x_m)= R(x_1;\ldots;x_{m-1};x_m)-R(x_1;\ldots;x_{m-1})
$$

边际冗余衡量了变量$x_m$对变量集合中所有其他变量$(x_1,x_2,\ldots,x_{m-1})$之间的依赖性，它可以看做是互信息的推广，直接计算可知

$$
\begin{aligned}
  R_M(x_1,\ldots,x_{m-1};x_m)&= H(x_m) + H(x_1,\ldots,x_{m-1}) - H(x_1,\ldots, x_{m-1}, x_m)\\
  &= H(x_m) - H(x_m|x_1,\ldots,x_{m-1})
\end{aligned}
$$

有了边际冗余，我们就可以来定义条件冗余

$$
  R_C(x_1|x_2,\ldots,x_{m-1};x_m) = R_M(x_1,\ldots,x_{m-1};x_m) - R_M(x_2,\ldots,x_{m-1};x_m)
$$

直接计算可知

$$
\begin{aligned}
  R_C(x_1|x_2,\ldots,x_{m-1};x_m) &= H(x_1,\ldots,x_{m-1}) + H(x_2,\ldots,x_m) - H(x_1,\ldots,x_m)- H(x_2,\ldots,x_{m-1})\\
  &=H(x_m|x_2,\ldots,x_{m-1}) - H(x_m|x_1,\ldots,x_{m-1})\\
  &=H(x_1|x_2,\ldots,x_{m-1}) - H(x_1|x_2,\ldots,x_{m-1},x_m)
\end{aligned}
$$

条件冗余衡量了在已知变量$(x_2,\ldots,x_{m-1})$的条件下，变量$x_m$和$x_1$之间的依赖性，它可以看做是条件互信息的推广。


## 交叉熵

所谓交叉熵，其实就是相对熵的一部分：

$$
  H(p,q)=-\sum\limits_{x\in\mathcal{X}}p(x)\log{q(x)}
$$

从公式来看，交叉熵

$$
  D_{KL}(p||q)=\sum\limits_{x\in\mathcal{X}}p(x)\log{p(x)}+H(p,q)
$$

机器学习中的分类问题常常使用交叉熵作为损失函数，其实最小化交叉熵等价于最小化相对熵，因为这里的$p(x)$表示真实分布，那么相对熵的第一项表示
的就是信息熵的相反数，它是一个常量，而$q(x)$是我们去逼近数据真实分布的一个近似分布。

根据相对熵的性质我们发现一个有意思的东西：

$$
-\sum\limits_{x\in\mathcal{X}}p(x)\log{p(x)}\leq-\sum\limits_{x\in\mathcal{X}}p(x)\log{q(x)}
$$

具体证明可以参考相对熵非负性的证明，主要是借助了凸函数和Jensen不等式。

## JS散度

JS散度衡量了两个概率分布之间的相似度，它是基于KL散度的变体，解决了KL散度非对称的问题。一般地，JS散度是对称的，且取值是在$[0,1]$范围内。具体定义如下：

$$
JS(P_1,P_2) = \frac{1}{2}KL\left(P_1\bigg\|\frac{P_1+P_2}{2}\right) +
\frac{1}{2}KL\left(P_2\bigg\|\frac{P_1+P_2}{2}\right)
$$

KL 散度和JS散度度量的时候有一个问题：如果两个分布$P_1,P_2$距离很远，完全没有重叠的时候，那么KL散度是没有意义的，而JS散度此时是一个常数，
这在机器学习中是比较致命的，因为此时意味着改点处的梯度为0，模型不能好好训练。

## Wasserstein距离

Wasserstein距离也是度量两个分布之间的距离，其定义如下：

$$
W(P_1,P_2) = \inf\limits_{\gamma\sim\Pi(P_1,P_2)}\mathbf{E}_{(x,y)\sim\gamma}\left[\|x-y\|\right]
$$

其中$\Pi(P_1,P_2)$是$P_1$和$P_2$分布组合起来的所有可能的联合分布的集合。对于每一个可能的联合分布$\gamma$，可以从中采样$(x,y)\sim\gamma$得到
一个样本$x$和$y$，并计算出这对样本的距离$\|x-y\|$，所以可以计算该联合分布$\gamma$下，样本对距离的期望值
$\mathbf{E}_{(x,y)\sim\gamma}\left[\|x-y\|\right]$. 在所有可能的联合分布中能够对这个期望值取到的下界就是所谓的Wasserstein距离。

直观上可以把$\mathbf{E}_{(x,y)\sim\gamma}\left[\|x-y\|\right]$理解为在$\gamma$这个路径规划下把土堆$P_1$挪到土堆$P_2$所需要的消耗。
而Wasserstein距离就是在最优路径规划下的最小消耗。所以Wesserstein距离又叫Earth-Mover距离。

Wessertein距离相比KL散度和JS散度的优势在于：即使两个分布的支撑集没有重叠或者重叠非常少，仍然能反映两个分布的远近。
而JS散度在此情况下是常量，KL散度可能无意义。
