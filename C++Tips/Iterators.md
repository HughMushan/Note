#STL源码剖析
##迭代器与traits编程技法
> 迭代器是一种抽象的设计概念，提供一种方法，可以遍历某个容器所含有的所有元素，但是又不用暴露该容器的内部表述方式。在STL中，将数据容器和算法分开，批次独立设计，最后通过迭代器将它们结合在一起。

### 如何获得迭代器所指对象的类型？
利用function template的参数推导机制
```c++
  template<class I, class T>
  void func_impl(I iter, T t)
  {
    T temp; //这里获取了迭代器所指对象类型
  }
  
  template<class I>
  inline void func(I iter)
  {
    func_impl(iter, *iter); //通过function template的参数推导机制 
  }
    
```

###Traits编程技法
Q1: 利用function template的参数推导机制可以获取迭代器所指对象的类别，但是不能获取函数的返回值类型，如何解决这个问题？

S1:　通过生命内嵌类型解决

```c++

```