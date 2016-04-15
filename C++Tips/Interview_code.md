#常见库函数的实现
> 在面试中，面试官会让你写一些基础的C或者C++常见库函数的实现，要写出bug free的代码其实还是有一定的难度的，要注意一些特殊情况。

##atoi实现
这其实是一道非常经典的字符串的问题，也是面试中比较容易碰到的题。虽然容易，但是也很容易出问题，要考虑以下问题：
* 正负符号问题
* 非法输入问题
* 溢出问题
* 前面和后面空格问题


```c++
class Solution {
public:
    int myAtoi(string str) {
        if(str.empty()) return 0;
        int i = 0; 
        int len = str.size();
        //ignore space
        while(i < len && str[i] == ' ') {
            i ++;
        }
        // +/-
        int sign = 1;
        if(str[i]=='-' || str[i]=='+') {
            if(str[i]=='-') {
                sign = -1;
            }
            i++;
        }
        //long -> avoid overflow
        long result = 0;
        while(i < len) {
            //illegal input 
            if(str[i] < '0' || str[i] > '9') {
                break;
            }      
            result = result * 10 + sign * (str[i] - '0');
            //overflow
            if(result > INT_MAX) {
                return INT_MAX;
            }
            else if(result < INT_MIN) {
                return INT_MIN;
            }   
            i++; 
        }
        return (int) result; 
    }
};
```

##memcpy实现
```c++
class Solution {
public:
    void * memcpy(const void *src, void *dest, unsigned int memsize) {
        assert(src != NULL && dest != NULL);
        void *ret = dest;
        unsigned int i = 0;
        if(dest < src || dest > (char *)src + memsize) {
            while(memsize--) {
                *(char*)dest = *(char*)src;
                dest = (char*)dest+1;
                src = (char*)src+1;
            }
        }
        else {
            dest = (char*)dest+memsize-1;
            src = (char*)src+memsize-1;
            while(memsize--) {
                *(char*)dest = *(char*)src;
                dest = (char*)dest-1;
                src = (char*)src-1;
            }
        }

        return ret;
    }
};
```