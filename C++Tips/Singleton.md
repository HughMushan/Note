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
                atexit(Destroy);
            }
        }
        return *m_pInstance;    
    }


protected:
    Singleton(){}

private:
    void Destroy() {
        if(m_pInstance != NULL) {
            delete m_pInstance;
            m_pInstance = NULL;
        }
    }
    static T *m_pInstance;
    Singleton(const Singleton&){}
    Singleton &operator=(const Singleton &){}    

}

template<class T>
T* Singleton:m_pInstance = NULL;

```
局部静态变量在多线程下会出现问题，因为编译器为了满足局部静态变量只被初始化一次，会通过一个全局的标志位记录该静态变量是否已经被初始化。其伪代码可以写成如下
```c++
bool flag = false;
if(!flag) {
    flag = true;
    staticvar = initstatic();
}
```
在多线程环境下，对这个flag的判断会出现问题，有可能会出现多个线程同时进入if分支里面。

最近看到资料，有人指出由于乱序执行的影响，DCL也是靠不住的。可以直接使用pthread_once_t来保证lazy_initialization的线程安全
```c++
template<class T>
class Singleton
{
public:
    static T& get_instance()
    {
        pthread_once(&ponce_, &Singleton::init);
        return *m_pInstance;
    }

private:
    Singleton();
    ~Singleton();
    Singleton(const Singleton&){}
    Singleton &operator=(const Singleton &){}
    static void init(){m_pInstance = new T();}

private:
    static pthread_once_t ponce_;
    static T *m_pInstance;
};

template<class T>
pthread_once_t Singleton<T>::ponce_ = PTHREAD_ONCE_INIT;

template<class T>
T* Singleton<T>::m_pInstance = NULL;



}
```