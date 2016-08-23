#String字符串
---
> 本篇主要总结与字符串相关的算法问题

字符串操作是数据结构与算法中最基本的内容，主要是操作`char *`或者是`string`类型，主要会考到字符串的遍历、匹配、反转等相关的操作，常见的问题有以下几种：

##简单的字符串遍历
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
##从后往前遍历更简单
---
- leetcode: [Length of Last Word | LeetCode OJ](https://leetcode.com/problems/length-of-last-word/)
- lintcode: [(422) Length of Last Word](http://www.lintcode.com/en/problem/length-of-last-word/)

```
Given a string s consists of upper/lower-case alphabets and empty space characters ' ',
return the length of last word in the string.

If the last word does not exist, return 0.

Have you met this question in a real interview? Yes
Example
Given s = "Hello World", return 5.

Note
A word is defined as a character sequence consists of non-space characters only.
```
有些字符串的处理顺序处理的情况比较多，可以考虑从后往前逆向操作，减少情况的讨论。此外字符串的尾部往往有足够空间，可以直接修改而不用担心覆盖字符串前面的数据。
``` c++
    int lengthOfLastWord(string s) {
        if (s.size() == 0) return 0;
       
        int count = 0;
        for (int i=s.size()-1; i>=0; i--)
            if (s[i] == ' ') {
                if (count) break;
            } else count++;

        return count;
    }
```
类似的题还有：

lintcode: [(212) Space Replacement](http://www.lintcode.com/en/problem/space-replacement/)

##万能的hash
---
- lintcode: [(55) Compare Strings](http://www.lintcode.com/en/problem/compare-strings/)
 
```
Compare two strings A and B, determine whether A contains all of the characters in B.

The characters in string A and B are all Upper Case letters.

Example
For A = "ABCD", B = "ABC", return true.

For A = "ABCD" B = "AABC", return false.
```
这类问题跟匹配子串的问题不同，它跟字符串的顺序是没有关系。题目中另外给的条件则是A和B都是全大写单词，理解题意后容易想到的方案就是先遍历 A 和 B 统计各字符出现的频次，然后比较频次大小即可。嗯，祭出万能的哈希表。
```c++
class Solution {
public:
    /**
     * @param A: A string includes Upper Case letters
     * @param B: A string includes Upper Case letter
     * @return:  if string A contains all of the characters in B return true
     *           else return false
     */
    bool compareStrings(string A, string B) {
        if (A.size() < B.size()) {
            return false;
        }

        const int alphabetNum = 26;
        int letterCount[alphabetNum] = {0};
        for (int i = 0; i != A.size(); ++i) {
            ++letterCount[A[i] - 'A'];
        }
        for (int i = 0; i != B.size(); ++i) {
            --letterCount[B[i] - 'A'];
            if (letterCount[B[i] - 'A'] < 0) {
                return false;
            }
        }

        return true;
    }
};
```
##字符串匹配问题
- leetcode: [Implement strStr() | LeetCode OJ](https://leetcode.com/problems/implement-strstr/)
- lintcode: [lintcode - (13) strstr](http://www.lintcode.com/en/problem/strstr/)

```
For a given source string and a target string, you should output the **first**
index(from 0) of target string in source string.
If target does not exist in source, just return `-1`.
If source = `"source"` and target = `"target"`, return `-1`.
If source = `"abcdabcdefg"` and target = `"bcd"`, return `1`
O(n2) is acceptable. Can you implement an O(n) algorithm? (hint: _KMP_)
```
字符串匹配问题是字符串相关问题中比较难的了，可能会问到KMP算法。首先最简单的方法就是暴力遍历，两个for循环可以解决，时间复杂度为O(n^2)。可以注意到，暴力方法每次都是重新匹配，没有使用遍历过程中已经匹配过的信息。KMP算法就是利用不匹配字符的前面那一段字符的最长前后缀来尽可能地跳过最大的距离。难点在于如何理解next数组，以及利用模式串自身的信息，构建一个next数组。KMP是通过统计模式串的前缀与后缀共有元素的长度来构造next数组的。如图:

![kmp](img/kmp-next.png)

* "A"的前缀和后缀都为空集，共有元素的长度为0；
* "AB"的前缀为[A]，后缀为[B]，共有元素的长度为0；
* "ABC"的前缀为[A, AB]，后缀为[BC, C]，共有元素的长度0；
* "ABCD"的前缀为[A, AB, ABC]，后缀为[BCD, CD, D]，共有元素的长度为0；
* "ABCDA"的前缀为[A, AB, ABC, ABCD]，后缀为[BCDA, CDA, DA, A]，共有元素为"A"，长度为1；
* "ABCDAB"的前缀为[A, AB, ABC, ABCD, ABCDA]，后缀为[BCDAB, CDAB, DAB, AB, B]，共有元素为"AB"，长度为2；
* "ABCDABD"的前缀为[A, AB, ABC, ABCD, ABCDA, ABCDAB]，后缀为[BCDABD, CDABD, DABD, ABD, BD, D]，共有元素的长度为0
  
当模式串最后一个D与源串发生不匹配，此时已匹配数为6，查看next[6-1]为2，重新将已匹配数设为2，比较模式串的第三个字符是否与源串匹配。如此类推。详细KMP算法讲解可以参考[这里](http://www.cnblogs.com/c-cloud/p/3224788.html)。

代码实现如下：
```c++
class Solution {
public:
    /**
     * Returns a index to the first occurrence of target in source,
     * or -1  if target is not part of source.
     * @param source string to be scanned.
     * @param target string containing the sequence of characters to match.
     */
    int strStr(const char *source, const char *target) {
        if(source == NULL || target == NULL)
        {
            return -1;
        }

        return kmp(source, target);
    }
    
    int normalMethod(const char *source, const char *target) {
        if(source == NULL || target == NULL)
        {
            return -1;
        }
        const int sourceLen = strlen(source);
        const int targetLen = strlen(target);
        for(int i = 0; i < sourceLen-targetLen+1; i ++)
        {
            int j = 0;
            for(; j < targetLen; j ++)
            {
                if(*(source+i+j) != *(target+j))
                    break;
            }

            if( j == targetLen)
            {
                return i;
            }

        }
        return -1;
    }
    
    
    int kmp(const char *source, const char *target) {
        const int srcLen = strlen(source);
        const int targetLen = strlen(target);
        
        if(targetLen == 0) return 0;
        if(srcLen == 0) return -1;
        
        int *next = new int[targetLen];
        makeNext(target, next);
        
        for(int i = 0, k = 0; i < srcLen; i ++) {
            while(k > 0 && target[k] != source[i]) {
                k = next[k - 1];
            }
            if(source[i] == target[k]) {
                k ++;
            }
            
            if(k == targetLen) return i-targetLen+1;
            
        }
        
        return -1;
        
    }
    
    void makeNext(const char *target, int next[]) {
        const int targetLen = strlen(target);
        next[0] = 0;
        
        for(int i = 1, k = 0; i < targetLen; i++) {
            while(k > 0 && target[k] != target[i]) {
                k = next[k - 1];
            }
            
            if(target[i] == target[k]) {
                
                k ++;
            }
            
            next[i] = k;
        }
        
    }
};

```
除了暴力遍历和KMP算法外还有许多字符串匹配的算法，比如Boyer Moore、Robin　Karp、Sunday、Bitap等方法。可以参考[这里](http://blog.csdn.net/airfer/article/details/8951802/)和[这里](http://wiki.jikexueyuan.com/project/kmp-algorithm/introduction.html)。