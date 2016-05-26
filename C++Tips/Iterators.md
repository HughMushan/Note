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

S1:　通过声明内嵌类型解决

```c++
  template<class T>
  struct MyIter {
    typedef T value_type;
    T *ptr;
    MyIter(T *p = 0):ptr(p) {}
    T& operator*() const{return *ptr;}
    // ...
  }
  
  template <class I>
  typename I::value_type
  func(I iter) { return *iter;}
  
  // ...
  MyIter<int> ite(new int(8));
  cout << func(ite) << endl; //输出８
```

Q2: 万一上述的class I不是class type，而是原生指针，无法定义内嵌类别，但STL必须接受原生指针作为一种迭代器，有没有办法让上述的一般化概念针对特定情况做特殊化处理?

S2: 通过template partial specilization(模板偏特化)解决，针对特定情况做特殊化处理

```c++
  //一般情况
  template <class I>
  struct iterator_traits {
    typedef typename I:value_type value_type;
  };
  
  ／／原生指针
  template <class T>
  struct iterator_traits<T *> {
    typedef T value_type;
  };
```

