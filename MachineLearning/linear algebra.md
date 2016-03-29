##线性代数基础
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