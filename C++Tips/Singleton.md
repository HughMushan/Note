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
    static T&  get_instance() {
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
当然上述的实现仍然有一些问题，比如没有考虑线程安全问题

###线程安全

```c++
template<class T>
class Singleton
{
public:
    static T& get_instance() {
        if(m_pInstance == NULL) {
            Lock lock;
            if(m_pInstance == NULL) {
                m_pInstance = new T();
            }
        }
        return *m_pInstance;    
    }


protected:
    Singleton(){}

private:
    static T *m_pInstance;
    Singleton(const Singleton&){}
    Singleton &operator=(const Singleton &){}    



```