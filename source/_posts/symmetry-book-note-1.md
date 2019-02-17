---
title: Symmetry Book 笔记(1)：类型，自然数，恒等类型，积类型
tags: 
    - Type Theory
category: notes
date: 2019-02-17 16:42:50
---
前两天看到一个[Unimath 奇书一本：Symmetry Book](https://zhuanlan.zhihu.com/p/56755893)。看介绍大概是从类型论开始，讲一些数学上的东西。好吧，其实我并没弄清楚这本书到底是讲什么的，但是看群里许多人都很懂，我想学一点类型论，就翻开了这本书。意外的是，前面的内容还算比较友好。接下来我可能会写一系列下面这种简单的笔记，但是看到我看不下去的地方可能就会咕了。总而言之就是自娱自乐的东西了。

## 什么是类型?

在编程语言中类型是非常常见的东西，我们都知道`True`是`Bool`, `"abc"`是String这些事情。在 Unimath 里，类型被用来给所有数学对象分类。每个数学对象都是某些(唯一)类型中的元素。

我们有一些常见的记号

* $a:X$，$a$的类型是$X$
* $f:X\rightarrow Y$：$f$是一个从类型$X$到$Y$的函数
* 如果$f:X\rightarrow Y$, $g: Y\rightarrow Z$ , 那么$g\circ f:=(a\mapsto g(f(a))) : X\rightarrow Z$ 函数复合，可以读作"$g$ after $f$"
  * 函数复合有结合性：如果$f:X\rightarrow Y​$, $g: Y\rightarrow Z​$, $h: Z\rightarrow W​$ , 那么$(h \circ g)\circ f​$和$h \circ (g\circ f)​$是相同的
* 单位函数 $\mathtt{id}_X:= (a\mapsto a): X\rightarrow X​$
  * 如果$f:X\rightarrow Y​$,那么$f\circ \mathtt{id}_X​$,$\mathtt{id}_Y \circ f​$和$f​$ 是相同的

## 自然数的类型

在类型论中，我们使用下面的皮亚诺规则来构造自然数

* P1: 有一个类型叫做 $\mathbb{N}$，它中的元素被叫做自然数
* P2: $\mathbb{N}​$ 中有一个元素叫做 $0​$
* P3: 如果 $m$ 是一个自然数，那它的后继(successor) $S(m)​$也是自然数
* P4: 给定一族类型 $X(m)​$ ，它们依赖于参数$m:\mathbb{N}​$。只要我们提供$X(0)​$的一个元素$a​$，以及对于每个$m​$，提供一个函数$g_m:X(m)\rightarrow X(S(m))​$，我们就可以为每个$m​$定义出$f(m):X(m)​$。我们可以把结果$f​$看作是通过$f(0):=a​$和$f(S(m)):=g_m(f(m))​$归纳定义而得到的。
  * P4给了我们通过“递归定义函数”或是“进行数学归纳”的能力

然后我们通过常用的方法书写自然数：
$$
\begin{align}
1&:=S(0)\\
2&:=S(1)\\
t+1&:=S(t)
\end{align}
$$
我们也可以定义一些常见的函数，如阶乘函数$fact:\mathbb{N}\rightarrow \mathbb{N}$。

* 首先我们需要对所有的$m$，将$X(m)$定义为$\mathbb{N}$
* 然后我们定义$fact(0):=1$ 和
* 对于所有$m$，$fact(m+1):=(m+1)\cdot fact(m)$ 

这是非常平常的定义。

在这一部分的最后，我们定义一种特殊形式的递归，迭代(iteration)：

如果$X$是一个类型，$e:X\rightarrow X$ ，那么我们可以通过归纳的方式定义$e$的$n$次迭代，记作$e^n$：

* $e^0:=\mathtt{id}_X$
* $e^{n+1}:=e\circ e^n$

这里，我们同样用到了P4，我们让所有的$X(m)$的类型均为$X\rightarrow X$，P4中的$a$我们选取$\mathtt{id}_X$，P4中的$g_m$我们选取$(X\rightarrow X)\rightarrow (X\rightarrow X)$类型的$d\mapsto e\circ d$。于是我们的得到了$f:\mathbb{N}\rightarrow (X\rightarrow X)$。我们只要定义$e^n:=f(n)$就得到了上面的定义。

## 恒等类型

恒等类型(Identity type) 或者相等类型(Equality type)是用来衡量相等性的类型。我们通过下面的规则来定义恒等类型：

* E1: 对于任何类型 $X$ 以及该类型的元素 $a$ 和 $b$ ，存在一个类型 $a=b$
* E2: 对于任何类型 $X$ 以及任何该类型的元素 $a$, 类型$a=a$ 有一个元素叫 $refl_a$ 。$refl$是自反性(reflexivity)的意思，即任何元素都和它自身相等
* E3: 对于任何类型 $X$ 以及该类型的任何元素 $a$， 给定一族类型 $P(b,e)$，这些类型依赖于$X$ 类型的参数$b$和 $a=b$类型的参数$e$。只要我们提供一个$P(a, refl_a)$的元素$p$，我们就可以为这族类型中的每个类型定义出元素 $f(b,e):P(b,e)$ 。我们可以把结果$f$看作被单一地定义为$f(a,refl_a):=p$。
  * 这一条规则说的是在相等性上进行归纳。假设我们要证明某个东西（或者是构造某个东西），我们有$a$和其他东西$b$相等的条件（这是$e$给我们的），那么对$a$成立的自然就对$b$成立了。我们于是只需要考虑最特殊的情况就是$a$等于自身的情况，这时我们没有额外条件，只需要提供一个$refl_a$。

我们可以把$a=b$类型的一个元素$i$称作$a$和$b$的一个等化(identification，不确定正式译名)。如果我们知道至多只有一个这样的$i$，那么$i$就是$a$与$b$相等的证明。所以$refl_{S(S(0))}$是$fact(2)=2$的一个证明（这是平凡的）。

下面，我们可以用这里的东西做一些简单的证明。

* **等价性的对称性质** 首先我们假定$a,b:X$，如果我们有一个$a=b$类型的元素$p$，我们希望从中得到一个$b=a$类型的元素。回到我们的E3规则，对于所有的$X$类型元素$b$这里我们要构造的$P(b,e)$是$b=a$，$e$的类型则是$a=b$。通过E3规则我们可以知道，我们只需要找到特殊情形$P(a,refl_a)$，也就是$a=a$类型的一个值$p$，我们知道$refl_a$刚好满足这个要求。于是我们就完成了等价性的交换性质的证明。
* **等价性的传递性质** 首先我们假定$a,b,c:X$，如果我们有$a=b$类型的元素$p$，一个$b=c$类型的元素$q$，我们希望从中产生出一个$a=c$类型的元素。同样我们在这里仍然需要使用E3规则。我们要构造的E3中的$P(c,e)$是$a=c$，$e$的类型是$b=c$。通过E3规则，我们只需要找到特殊情形$P(b,refl_b)$，也就是类型$a=b$的一个元素。而我们的$p$的类型刚好是$a=b$，所以我们令$f(b,refl_b)=p$即完成了证明。

现在，我们可以更正式地描述我们的对称性质和传递性质的证明结果：

* 对任何类型$X$，任何$a,b:X$，$symm_{a,b}:(a=b)\rightarrow (b=a)$ 的定义为$symm_{a,a}(refl_a):=refl_a$
* 对任何类型$X$，任何$a,b:X$，$trans_{a,b,c}:(a=b)\rightarrow ((b=c)\rightarrow (a=c))$的定义为$trans_{a,b,c}(p):=refl_b\mapsto p$

恒等类型可以用来**替换**：如果$X$是一个类型，$T(x)$是一族依赖于参数$x:X$的类型。假设$x,y:X$，$e:x=y$，那么就有一个函数$transport_{T,e}:T(x)\rightarrow T(y)$。定义如下：

* $transport_{T,refl_x}(t):=t$

这个函数可以被称为“类型族$T$上沿着路径$e$的传输函数(transport function)”，简称传输函数(transport function，同样也不确定正式译名)。

当类型$T(x)$中可能有多个元素的时候，这些元素可以看作是为$x$提供了额外的结构(structure)。这时，我们称传输函数$T(x)\rightarrow T(y)$是从$x$到$y$的结构的传输(transport of structure)。

* 例如，$T(x):=(x=x)$，那么$x=x\rightarrow y=y$类型的$transport_e$将$x$的对称性传输给了$y$。

反过来，如果类型$T(x)$至多只有一个元素，这个元素可以看作是证明了$x$的某个性质。这时，传输函数$T(x)\rightarrow T(y)$就可以说明$y$和$x$有着同样的性质。

最后，我们说明**函数保持相等性**。对于所有类型$X,Y$，函数$f:X\rightarrow Y$，元素$x,y:X$。我们可以归纳地（通过E3规则）定义函数$ap_f:x=y\rightarrow f(x)=f(y)$ 为$ap_f(refl_x):=refl_{f(x)}$（这里的定义和上面类似，略去具体细节）。

更一般地，如果$Z(x)$是一个类型，$x:X$，$f(x):Z(x)$是一个依赖函数，那么我们可以为所有的$e:x=y$定义依赖函数$apd_f:(transport_{Z,e}(f(x)))=f(y)$为$apd_f(refl_x):=refl_{f(x)}$。

## 积类型

如果$X$是类型，$Y(x)$是一族由$x:X$索引的类型，那么就有有积类型(product type)$\prod_{x:X}Y(x)$，它的元素是函数$f(a):Y(a)$。普通的任意$X\rightarrow Y$类型的函数我们可以看作是$\prod_{x:X}Y$类型中的元素。

**积类型中元素的相等性**。如果$\prod_{x:X}Y(x)$中的两个元素$f,g$是相等的，那么对于对于所有的$x:X$，我们都能得到$f(x)=g(x)$ 。为了说明这一点，我们需要定义一个$f=g\rightarrow \prod_{x:X}f(x)=g(x)$类型的函数$prodEq$。

* 这里我们需要用到之前定义的$transport_{T,e}$，其中$T(g)$为$f(x)=g(x)$。于是$transport_{T,e}$通过$transport_{T,refl_f}(t):=t$定义出。它的完整类型为$(f(x)=f(x))\rightarrow(f(x)=g(x))$
* 于是$prodEq(e):=a\mapsto transport_{T,e}(refl_{f(a)})​$
* 好吧这里有点虚感觉哪里写的不对

反过来也有一个基本的原则叫做“函数的外延性”(function extensionality)，即如果对于所有的$x:X$，$f(x)=g(x)$都成立，我们就可以得出$f=g$的结论。
