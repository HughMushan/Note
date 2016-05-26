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
**那什么是traits编程技法**
通过一个'特性萃取机'将一个类的一些特性拿出来，然后通过函数重载，在编译期就能选择正确的版本，从而提升效率。Traits编程技法大量运用于STL中，利用‘内嵌类型’的编程技巧与编译器的template参数推导功能，增强C++未能提供的关于类型认证方面的能力。

'特性萃取机'
```c++
	struct input_iterator_tag{};
	struct output_iterator_tag{};
	struct forward_iterator_tag :public input_iterator_tag {};
	struct bidirectional_iterator_tag :public forward_iterator_tag {};
	struct random_access_iterator_tag :public bidirectional_iterator_tag {};

	template <class T, class Distance> struct input_iterator
	{
		typedef input_iterator_tag	iterator_category;
		typedef T					value_type;
		typedef Distance			difference_type;
		typedef T*					pointer;
		typedef T&					reference;
	};
	struct output_iterator
	{
		typedef output_iterator_tag iterator_category;
		typedef void                value_type;
		typedef void                difference_type;
		typedef void                pointer;
		typedef void                reference;
	};
	template <class T, class Distance> struct forward_iterator
	{
		typedef forward_iterator_tag	iterator_category;
		typedef T						value_type;
		typedef Distance				difference_type;
		typedef T*						pointer;
		typedef T&						reference;
	};
	template <class T, class Distance> struct bidirectional_iterator
	{
		typedef bidirectional_iterator_tag	iterator_category;
		typedef T							value_type;
		typedef Distance					difference_type;
		typedef T*							pointer;
		typedef T&							reference;
	};
	template <class T, class Distance> struct random_access_iterator
	{
		typedef random_access_iterator_tag	iterator_category;
		typedef T							value_type;
		typedef Distance					difference_type;
		typedef T*							pointer;
		typedef T&							reference;
	};

	template<class Category, class T, class Distance = ptrdiff_t,
	class Pointer = T*, class Reference = T&>
	struct iterator
	{
		typedef Category	iterator_category;
		typedef T			value_type;
		typedef Distance	difference_type;
		typedef Pointer		pointer;
		typedef Reference	reference;
	};

	template<class Iterator>
	struct iterator_traits
	{
		typedef typename Iterator::iterator_category	iterator_category;
		typedef typename Iterator::value_type			value_type;
		typedef typename Iterator::difference_type		difference_type;
		typedef typename Iterator::pointer				pointer;
		typedef typename Iterator::reference 			reference;
	};
	template<class T>
	struct iterator_traits<T*>
	{
		typedef random_access_iterator_tag 	iterator_category;
		typedef T 							value_type;
		typedef ptrdiff_t 					difference_type;
		typedef T*							pointer;
		typedef T& 							reference;
	};
	template<class T>
	struct iterator_traits<const T*>
	{
		typedef random_access_iterator_tag 	iterator_category;
		typedef T 							value_type;
		typedef ptrdiff_t 					difference_type;
		typedef const T*					pointer;
		typedef const T& 					reference;
	};

	template<class Iterator>
	inline typename iterator_traits<Iterator>::iterator_category
		iterator_category(const Iterator& It){
			typedef typename iterator_traits<Iterator>::iterator_category category;
		return category();
	}
	template<class Iterator>
	inline typename iterator_traits<Iterator>::value_type*
		value_type(const Iterator& It){
		return static_cast<typename iterator_traits<Iterator>::value_type*>(0);
	}
	template<class Iterator>
	inline typename iterator_traits<Iterator>::difference_type*
		difference_type(const Iterator& It){
		return static_cast<typename iterator_traits<Iterator>::difference_type*>(0);
	}
```

```c++
    
    namespace {
      template<class InputIterator, class Distance>
      void _advance(InputIterator& it, Distance n, input_iterator_tag){
        assert(n >= 0);
        while (n--){
          ++it;
        }
      }
      template<class BidirectionIterator, class Distance>
      void _advance(BidirectionIterator& it, Distance n, bidirectional_iterator_tag){
        if (n < 0){
          while (n++){
            --it;
          }
        }else{
          while (n--){
            ++it;
          }
        }
      }
      template<class RandomIterator, class Distance>
      void _advance(RandomIterator& it, Distance n, random_access_iterator_tag){
        if (n < 0){
          it -= (-n);
        }else{
          it += n;
        }
      }
  }
  
  template <class InputIterator, class Distance> 
  void advance(InputIterator& it, Distance n){
    typedef iterator_traits<InputIterator>::iterator_category iterator_category;
      _advance(it, n, iterator_category());
  }
```

