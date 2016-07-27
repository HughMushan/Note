#实现Singleton模式
> Singleton模式是所有设计模式当中用的比较多，而且容易实现的模式。也是常见的面试题。

###版本一

```c++
class Singleton 
{
public:
    static Singleton& get_instance() {
        static Singleton instance;
        return instance;
    }

private：
    Singleton(){}
    Singleton(const Singleton&){}
    Singleton &operator=(const Singleton &){}    
}
```

###考虑重用

```c++
template<class T>
class Singleton
{
public: 
    static T& Singleton() {
        static T instance;
        return instance;
    }

protected:
    Singleton(){}

private:
    Singleton(const Singleton&){}
    Singleton &operator=(const Singleton &){}    
}
```
