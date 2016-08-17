#STL中的精妙算法

###next_permutation、prev_permutation
> 计算排列组合关系的算法。考虑三个字符所组成的序列{a, b, c}。这个序列有六个可能的排列组合：abc, acb, bac, bca, cab, cba。这些排列组合是根据less-than操作符做字典顺序的排列。

```c++
template<class RandomAccessIterator>
bool next_permutation(RandomAccessIterator first, RandomAccessIterator last)
{
    if(first == last) return false;
    RandomAccessItrerator i = first;
    ++i;
    if(i == last) return false;
    i = last;
    --i;
    while(true) 
    {
        RandomAccessIterator ii = i;
        --i;
        if(*i < *ii)
        {
            RandomAccessIterator j = last;
            while(!(*--j > *i));
            iter_swap(i, j);
            reverse(ii, last);
            return true;
        }

        if(i == first)
        {
            reverse(i, last);
            return false;    
        }
    }
}
```
prev_permutation跟next_permutation类似，只是关键的比较不一样而已。

###heap系列的算法

```c++
namespace HughAlg
{
    //假设之前的结构都是已经符合heap的要求
template<class RandomAccessIterator, class Distance, class T>
    void __push_heap(RandomAccessIterator first, Distance hole, Distance top, T value)
    {
        Distance parent = (hole - 1)/2;
        while(hole > top && value > *(first+parent))
        {
            *(first+hole) = *(first+parent);
            hole = parent;
            parent = (hole-1)/2;
        }
        *(first+hole) = value;
    }
    
    //调整容器，使得以hole为根的堆保持堆的格式
template<class RandomAccessIterator, class Distance, class T>
    void adjust_heap(RandomAccessIterator first, Distance hole, Distance len, T value)
    {
        Distance top = hole;
        Distance child = 2 * hole + 2;
        while(child < len) {
            if(*(first+child-1) > *(first+child))
            child --;
            *(first+hole) = *(first+child);
            hole = child;
            child = 2 * hole + 2;
        }
        //别忘了考虑没有右节点的情况
        if(child == len)
        {
            *(first+hole) = *(first+child-1);
            hole = child-1;
        }
        HughAlg::__push_heap(first, hole, top, value);
    }
    
    //从中间开始调整
template<class RandomAccessIterator>
    void make_heap(RandomAccessIterator first, RandomAccessIterator last)
    {
        typedef typename RandomAccessIterator::difference_type Distance;
        Distance hole = (last - first)/2;
        while(true)
        {
            HughAlg::adjust_heap(first, hole, last-first, *(first+hole));
            if(hole==0) return;
            hole --;
        }
    }
    
    //将第一个元素放到最后，并重新调整[first, last-1)区间的heap格式
template<class RandomAccessIterator>
    void pop_heap(RandomAccessIterator first, RandomAccessIterator last)
    {
        typedef typename RandomAccessIterator::value_type T;
        T value = *(last-1);
        *(last-1) = *first;
        HughAlg::adjust_heap(first, first-first, last-first-1, value);
    }
    
    //不停的pop就行了, O(nlgn)
template<class RandomAccessIterator>
    void sort_heap(RandomAccessIterator first, RandomAccessIterator last)
    {
        while(last-first>1)
        HughAlg::pop_heap(first, last--);
    }

    //排出前n个，先将前n个变成heap的格式，遍历后面len-n个元素，如果比最大值堆的第一个小，那么交换位置，重新调整heap
template<class RandomAccessIterator>
    void partial_sort(RandomAccessIterator first, RandomAccessIterator middle, RandomAccessIterator last)
    {
        typedef typename RandomAccessIterator::value_type T;
        HughAlg::make_heap(first, middle);
        for(RandomAccessIterator it = middle; it < last; it ++)
        {
            if(*it < *first){
                T value = *it;
                *it = *first;
                HughAlg::adjust_heap(first, first-first, middle-first, value);
            }
        }
        HughAlg::sort_heap(first, middle);
    }
}

```

###list的排序
> 有点不大好理解，主要用merge sort的思想

```c++
template<class T>
void list::sort(){
    //判断是否为空或者只有一个元素
    if(node->next == node || link_type(node->next)->next == node)
        return;
    //用于merge的中间变量
    list<T> carry;
    list<T> counter[64];
    int fill = 0;
    
    while(empty()) {
        //将list的一个元素放进carry中，这里list的元素数量会少一
        carry.splice(carry.begin(), *this, begin());
        
    }


```