##基本数学知识
> 本篇主要介绍线性代数的一些基本知识

##矩阵的导数
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
