#你会写赋值构造函数么？
> 这是一道非常基础和经典的面试题，但是如果不注意很容易出现一些错误

```
如下为类型CMyString的声明，请为该类型添加赋值运算符函数
```

```c++
class CMyString 
{
public:
    CMyString(char *pData = NULL);
    CMyString(const CMyString& str);
    ~CMyString();
private:
    char *m_pData;
}
```

定义赋值构造函数要关注以下几点：

1. 是否把返回值的类型声明为该类型的引用，并在函数结束前返回实例自身的引用(*this)。这样才允许连续赋值。

2. 是否把传入的参数类型声明为常引用。避免又调用一次复制构造。

3. 是否释放自身已有的内存。

4. 是否判断传入的参数和当前的实例是不是同一个实例。 

5. 有没有考虑异常安全情况。

```c++
CMyString& CMyString::operator = (const CMyString& str)
{
    if(this != &str) {
        CMyString temp(str);
        char *pTemp = str.m_pData;
        str.m_pData = m_pData;
        m_pData = pTemp;
    }
    return *this;
}

```