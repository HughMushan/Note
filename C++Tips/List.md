#STL中的list实现

###list的节点

```c++
template<class T>
struct __list_node
{
  typedef void* void_pointer;
  void_pointer prev;
  void_pointer next;
  T val;
};
```
是一个双向链表，其迭代器的实现是一个Bidirectional Iterators。list有一个重要的特征：其插入和接合操作都不会造成原来的迭代器失效，甚至list的元素删除操作都只是“指向被删除元素”的那个迭代器失效，其他迭代器不受影响。

###list的数据结构
STL中，list不只是一个双向链表，而且是一个环状双向链表。只需要一个__list_node指针便可以完整表现整个链表。这个__list_node指针是指向链表尾端的