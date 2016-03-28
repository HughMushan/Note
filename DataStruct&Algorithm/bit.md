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
```
