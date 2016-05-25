#STL源码剖析
##空间配置器
---
> 空间配置器总是隐藏在容器的背后，负责内存、磁盘以及其它辅助存储介质的配置。

  一般而言，C++内存配置操作和释放是通过`new`和`delete`。其中`new`算式内含两阶段操作:(1)调用::operator new 配置内存;(2)调用对应类的构造函数构造对象内容。相应的`delete`操作也内含两阶段操作:(1)调用析构函数将对象析构;(2)调用::operator delete释放内存。
  
  **构造和析构基本工具：construct()和destroy()**。如果是用户没有自定义析构函数，则调用trivial destructor，否则调用no-trivial destructor()。因为要把[first, last)范围所有对象析构掉，万一这个范围很大，而每个对象的析构函数都是trivial destructor，一次次调用这些无关痛痒的析构函数，对效率是一种伤害。对于trivial destructor在析构阶段什么都不做就结束。
  
  ![2-1](imgs/stl-2-1.png)
  
  
  **空间的配置与释放**。对象构造前进行空间配置，对象析构后进行空间释放。主要处理以下一些情况：

 * 向system heap要求空间
 * 考虑多线程状态
 * 考虑内存不足情况
 * 考虑内存碎片问题
 
 
 为了处理内存碎片问题，可以视情况不同而采用不同的策略：当配置区块超过128bytes时，视之足够大，调用第一级配置器，直接分配空间；当配置区块小于128bytes时，为了降低额外负担，采用复杂的memory pool整理方式：每次配置一大块内存，并维护对应自由链表。下次若再有相同大小的内存需求，就直接从free-lists中拔出。
 
 C++ new handler机制是指可以要求系统在内存配置需求无法被满足时，调用一个指定的函数。C++ new handler机制来处理内存不足情况
 
 ##new、operator new、placement new的区别
 (1) `new`，也就是`new operator`,主要执行了以下操作
 
   * 调用`operator new`分配内存
   * 调用构造函数生成类对象
   * 返回相应指针

(2) `operator new`申请堆内存空间，并进行内存对齐,有三种重载形式

  * `void * operator new(std::size_t size) throw (std::bad_alloc);`分配size字节的存储空间，进行内存对齐,成功返回非空的指针指向首地址,失败则抛出异常.可以被用户更换、重载,但一般在类中重载.
  * `void *operator new(std::size_t size, const std::nothrow_t &nothrow) throw();`分配失败时不抛出异常.可以重载.
  * `void *opeator new(std:size_t size, void *ptr) throw();`这个也就是我们所说的`placement new`, 它不分配内存,调用构造函数在ptr所指的地址构造对象,这个函数不可重载.

(3) `placement new`是`operator new`的一种重载，允许在已经分配好的内存中构造一个对象.

##memory pool整理方式
当要分配的内存小于128bytes时，为了降低额外负担，采用memory pool整理方式,主要由以下几个函数实现:

* allocate(), 分配内存空间. 首先要判断区块大小,如果大于128bytes就直接调用一级配置器,直接从内存中分配. 如果小于128bytes就检查对应的`free_list`. 如果它有可用的区块，就直接从`free_list`中取出

* refill(), 调用allocate()发现`free_list`中没有可用区块时需要调用refill(),准备为`free_list`重新填充空间.

* chunk_alloc(),　从内存池中取空间给`free_list`使用. 通过`end_free - start_free`判断内存池中的水量,如果水量充足,就直接调用20个区块给`free_list`，如果水量不足以提供20个区块,但是可以提供一个以上的区块,就拨出着不足20个区块的空间出去. 如果连一块区块空间都提供不了,需要利用malloc()从堆空间中配置需求量两倍的内存.
