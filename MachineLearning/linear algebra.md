##基本数学知识
> 本篇主要介绍线性代数的一些基本知识

##矩阵的导数
---
1. 设f(x)是一个取标量值的函数,有d个变量$$x_i (i = 1,2,...,d)$$ 。函数$$f(\cdot)$$关于自变量x的梯度定义为
    $$
  \triangledown f(x)= grad f(x)=\frac{\partial f(x)}{\partial x} = 
  \begin{pmatrix}
  \frac{\partial f(x)}{\partial x_1} \\ \frac{\partial f(x)}{\partial x_2} \\.\\.\\.\\\frac{\partial f(x)}{\partial x_d}
  \end{pmatrix}
  $$

2.如果**f**是一个值为n维向量的向量函数，其导数可以用雅克比矩阵表示
   $$
     J(x) = \frac {\partial f(x)}{\partial x}=\begin{pmatrix}\frac {\partial f_1(x)}{\partial x_1}&...&\frac {\partial f_1(x)}{\partial x_d}\\ .&.&.\\.&.&.\\.&.&.\\ \frac {\partial f_n(x)}{\partial x_1}&...&\frac {\partial f_n(x)}{\partial x_d} \end{pmatrix}
   $$
3.如果矩阵**M**的每一个元素都是关于某一标量参数$$\theta$$的函数，则**M**对参数$$\theta$$的导数为
  $$
      \frac{\partial M}{\partial \theta}=\begin{pmatrix}\frac{\partial m_{11}}{\partial \theta}&...&\frac{\partial m_{1d}}{\partial \theta}\\
      .&...&.\\ 
 \frac{\partial m_{n1}}{\partial \theta}&...&\frac{\partial m_{nd}}{\partial \theta} \end{pmatrix}
  $$
4.对逆矩阵$$M^{-1}$$求导数公式
  $$
  \frac {\partial M^{-1}}{\partial \theta} = -M^{-1}\frac {\partial M}{\partial \theta}M^{-1}
  $$
5.矩阵的逆运算，只要一个$$ d \times d$$矩阵的行列式的值不为零，那么其对应的逆矩阵$$M^{-1}$$就存在，并满足
  $$
      MM^{-1} = I
  $$
  两个方阵乘积的逆服从$$[MN]^{-1}=N^{-1}M^{-1}$$
  
 6.本征向量和本征值。已知一个$$d\times d$$的矩阵M，一类非常重要的线性方程组的形式为
   $$
         Mx = \lambda x
   $$
   其中$$\lambda$$为标量。上式也可以重新写成
   $$
     (M-\lambda I)x = 0
   $$
   这个方程的解向量x和对应的标量系数$$\lambda$$分别称作矩阵M的本征向量和对应的本征值。我们可以这样理解本征向量和本征值，矩阵M乘以x表示，对向量x进行一次线性转换（旋转或拉伸），而该转换的效果为常数c乘以向量x（即只进行拉伸）。我们通常求本征值和本征向量即为求出该矩阵能使哪些向量（本征向量）只发生拉伸，使其发生拉伸的程度如何（本征值大小）。这样做的意义在于，看清一个矩阵在那些方面能产生最大的效果（power），并根据所产生的每个本征向量（一般研究本征值最大的那几个）进行分类讨论与研究。
   
一种求解本征向量和本征值的方法是求解特征方程
$$
  |M-\lambda I|= \lambda ^d + a_1 \lambda ^{d-1}+...+a_{d-1}\lambda + a_d = 0
$$特征方程的各个根就是本征值。然后对每一个根，通过求解线性方程组得到对应的本征向量。本征值有一个特别重要的性质就是：全部本征值之和为矩阵的迹，全部本征值的乘积为矩阵的行列式的值

##拉格朗日乘数法
---
我们要求在某些约束条件下，使得标量函数f(x)取到极值的自变量$$x_0$$的值。根据约束条件的不同，可以分为以下几种情况：一个等式约束方程的情况，多个等式约束方程的情况，和多个不等式约束方程的情况(KTT条件)。先研究最简单的只有一个等式约束条件下的求极值的解法。

如果约束条件可以表示为`g(x) = 0` 的形式，那么我们可以用如下的方法求得f(x)的极值。首先，我们定义拉格朗日函数：
$$
  L(x, \lambda) = f(x) + \lambda g(x)
$$其中$$\lambda$$称为拉格朗日待定乘数，也叫拉格朗日乘子。对拉格朗日函数对x求偏导，并令其值为零：
$$
  \frac{\partial L(x, \lambda)}{\partial x}=\frac{\partial f(x)}{\partial x}+\lambda \frac{\partial g(x)}{\partial x} = 0
$$这样就把约束条件下的最优化问题转化为无约束条件的方程求解问题。求解该方程能够得到$$\lambda$$和对应的极值点$$x_0$$。
