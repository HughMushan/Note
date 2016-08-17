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

```