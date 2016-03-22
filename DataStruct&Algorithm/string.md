#String字符串
---
> 本篇主要总结与字符串相关的算法问题

字符串操作是数据结构与算法中最基本的内容，主要是操作`char *`或者是`string`类型，主要会考到字符串的遍历、匹配、反转等相关的操作，常见的问题有以下几种：

##Longest Common Substring
---
- lintcode: [(79) Longest Common Substring](http://www.lintcode.com/en/problem/longest-common-substring/)

```
Given two strings, find the longest common substring.
Return the length of it.

Example
Given A="ABCD", B="CBCE", return 2.
Note
The characters in substring should occur continuously in original string.
This is different with subsequence.
```
如果面试面到这道题，也不用想的太复杂，就是考两个字符串的遍历操作而已，两个for循环可以搞定，不过需要注意一下一些特殊情况的处理，C++实现如下：

```c++
class Solution {
public:
    /**
     * @param A, B: Two string.
     * @return: the length of the longest common substring.
     */
    int longestCommonSubstring(string &A, string &B) {
        if (A.empty() || B.empty()) {
            return 0;
        }

        int lcs = 0, lcs_temp = 0;
        for (int i = 0; i < A.size(); ++i) {
            for (int j = 0; j < B.size(); ++j) {
                lcs_temp = 0;
                while ((i + lcs_temp < A.size()) &&\
                       (j + lcs_temp < B.size()) &&\
                       (A[i + lcs_temp] == B[j + lcs_temp]))
                {
                    ++lcs_temp;
                }

                // update lcs
                if (lcs_temp > lcs) {
                    lcs = lcs_temp;
                }
            }
        }

        return lcs;
    }
};
```