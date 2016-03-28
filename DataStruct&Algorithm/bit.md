#绝妙位操作
---
> 本篇主要介绍一些位操作相关的题。与位操作相关的题大多非常灵活，表面上跟位操作没有关系，但是用位操作解决确实非常巧妙以及高效率，主要是涉及按位与/或/异或等特性

先来看一下各种位运算的使用
1. `&`运算，通常用于二进制取位操作，例如一个数`&`1的结果就是获取二进制最末位，可以判断一个整数的奇偶。
2. `|`运算，通常用于二进制特定位上的无条件赋值，例如一个数`|`1的结果就是将二进制最末位强行变成1。
3. `^`运算，通常用于对二进制特定一位进行取反操作，而且它的逆运算是它本身，可以对数据简单加密。也可以实现无变量的swap。

```c++
      void Swap(int &a, int &b){
           a = a ^ b;
           b = a ^ b;
           a = a ^ b;
      }
```

4. `~`运算，可以把内存中的0和1全部取反，但是要格外小心，注意整数类型有无符号。
5. `a << b`运算，将a的二进制左移b位，相当于a乘以2的b次方，通常认为`a << 1`比`a * 2`快，因为前者是更底层一些的操作。
6. `a >> b`运算，将a的二进制右移b位，去掉末尾b位，相当于a除以2的b次方取整。用这个运算代替除法运算可以是程序效率大大提高。

下面来看一些具体的题：

##数1
---
- lintcode: [(365) Count 1 in Binary](http://www.lintcode.com/en/problem/count-1-in-binary/)

```
Count how many 1 in binary representation of a 32-bit integer.

Example
Given 32, return 1
Given 5, return 2
Given 1023, return 9

Challenge
If the integer is n bits with m 1 bits. Can you do it in O(m) time?
```
利用运算`a & (a-1)`可以将a的最低位1置0的性质，可以很巧妙的使用位操作来实现。

``` c++
class Solution {
public:
    /**
     * @param num: an integer
     * @return: an integer, the number of ones in num
     */
    int countOnes(int num) {
        int count = 0;
        while(num) {
            num &= (num-1);
            count ++;
        }
        return count;
    }
};
```

##找单数
---

- lintcode: [(82) Single Number](http://www.lintcode.com/en/problem/single-number/)

```
Given 2*n + 1 numbers, every numbers occurs twice except one, find it.

Example
Given [1,2,2,1,3,4,3], return 4

Challenge
One-pass, constant extra space
```
可以使用异或运算的特性，根据`x ^ x = 0`和`x ^ 0 = x`可将给定数组的所有数依次异或，最后保留的即为结果。

```c++
class Solution {
public:
	/**
	 * @param A: Array of integers.
	 * return: The single number.
	 */
    int singleNumber(vector<int> &A) {
        if(A.empty()) return -1;
        int result = 0;
        for(auto num : A) {
          result = result ^ num;
        }
        return result;
    }
};
```
来看这个问题的升级版，如果每个数不是重复2个而是三个怎么办？
- leetcode: [Single Number II | LeetCode OJ](https://leetcode.com/problems/single-number-ii/)
- lintcode: [(83) Single Number II](http://www.lintcode.com/en/problem/single-number-ii/)

```
Given `3*n + 1` numbers, every numbers occurs triple times except one, find it.

Example
Given `[1,1,2,3,3,3,2,2,4,1]` return `4`

Challenge
One-pass, constant extra space.
```
上题 Single Number 用到了二进制中异或的运算特性，这题给出的元素数目为`3*n + 1`，因此我们很自然地想到如果有种运算能满足「三三运算」为0该有多好！对于三个相同的数来说，其相加的和必然是3的倍数，仅仅使用这一个特性还不足以将单数找出来，我们再来挖掘隐含的信息。以3为例，若使用不进位加法，三个3相加的结果为：

```
0011
0011
0011
----
0033
```
注意到其中的奥义了么？三个相同的数相加，不仅其和能被3整除，其二进制位上的每一位也能被3整除！因此我们只需要一个和`int`类型相同大小的数组记录每一位累加的结果即可。时间复杂度约为 $$O((3n+1)\cdot sizeof(int) \cdot 8)$$ 

```c++
class Solution {
public:
	/**
	 * @param A : An integer array
	 * @return : An integer
	 */
    int singleNumberII(vector<int> &A) {
        if(A.empty()) return -1;
        int result = 0;
        for(int i = 0; i < 8 * sizeof(int); i ++) {
            int bit_i_sum = 0;
            for(auto num : A) {
                bit_i_sum += ((num >> i)&1)
            }
            result += ((bit_i_sum%3) << i)
        }
        return result;
    }
};
```
这个问题还有一个升级版，就是这个落单的数有两个怎么办？

- lintcode: [(84) Single Number III](http://www.lintcode.com/en/problem/single-number-iii/)

```
Given 2*n + 2 numbers, every numbers occurs twice except two, find them.

Example
Given [1,2,2,3,4,4,5,3] return 1 and 5

Challenge
O(n) time, O(1) extra space.
```
不妨设最后两个只出现一次的数分别为 `x1, x2`. 那么遍历数组时根据两两异或的方法可得最后的结果为 `x1 ^ x2`, 如果我们要分别求得 `x1` 和 `x2`, 我们可以根据 `x1 ^ x2 ^ x1 = x2` 求得 `x2`, 同理可得 `x_1`. 那么问题来了，如何得到`x1`和`x2`呢？看起来似乎是个死循环。大多数人一般也就能想到这一步(比如我...)。

这道题的巧妙之处在于利用`x1 ^ x2`的结果对原数组进行了分组，进而将`x1`和`x2`分开了。具体方法则是利用了`x1 ^ x2`不为0的特性，如果`x1 ^ x2`不为0，那么`x1 ^ x2`的结果必然存在某一二进制位不为0（即为1），我们不妨将最低位的1提取出来，由于在这一二进制位上`x1`和`x2`必然相异，即`x1`, `x2`中相应位一个为0，另一个为1，所以我们可以利用这个最低位的1将`x1`和`x2`分开。又由于除了`x1`和`x2`之外其他数都是成对出现，故与最低位的1异或时一定会抵消，十分之精妙！

```c++
    class Solution {
    public:
      vector<int> singleNumberIII(vector<int> &A) {
            vector<int> result;
            if(A.empty()) return result;
            //获得 x1^x2
            int x1_xor_x2 = 0;
            for(auto num : A) {
                x1_xor_x2 ^= num;
            }
            //获得最低位为1的位
            int last_1_bit = x1_xor_x2 -(x1_xor_x2&(x1_xor_x2-1));
            int x1 = 0, x2 = 0;
            for(auto num : A) {
                if((last_1_bit & num) == 0) {
                    x1 ^= num;
                } 
                else {
                    x2 ^= num;
                }
            }
            result.push_back(x1);
            result.push_back(x2);
            return result;
      }
    };
```

