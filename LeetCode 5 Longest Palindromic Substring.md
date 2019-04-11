<center>[LeetCode 5 Longest Palindromic Substring]</center>

### 题目：

给定一个字符串s，找到s中的最长回文子串，字符串的最大长度为1000.

### 示例：

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

### 分析：

看到该题目时，应该想到回文子串就是左右一致的子串，那么一种可行的方案就是以每一个字符为中心，然后向左向右走对比该字符左边和右边字符的一致性，其中，回文子串可以分为两类，即奇数形式的回文，例如"aba",或者是偶数形式的回文，例如"abba"，对于奇数形式的，我们就从遍历到的位置为中心，向两边进行扩散，对于偶数情况，我们就把当前位置和下一个位置当作偶数行回文的最中间两个字符，然后向两边进行搜索。

第一种方法就是按照以上思想来的，注意每次应该同时考虑出现奇数回文和偶数回文。

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.size() < 2) return s;
        int n = s.size(), maxLen = 0, start = 0;
        for (int i = 0; i < n - 1; ++i) {
            searchPalindrome(s, i, i, start, maxLen);
            searchPalindrome(s, i, i + 1, start, maxLen);
        }
        return s.substr(start, maxLen);
    }
    void searchPalindrome(string s, int left, int right, int& start, int& maxLen) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left; ++right;
        }
        if (maxLen < right - left - 1) {
            start = left + 1;
            maxLen = right - left - 1;
        }
    }
};
```

此外，也可以使用动态规划思想解决该问题，代码比较简洁易懂。

我们维护一个二维数组 dp，其中 dp[i][j] 表示字符串区间 [i, j] 是否为回文串，当 i = j 时，只有一个字符，肯定是回文串，如果 i = j + 1，说明是相邻字符，此时需要判断 s[i] 是否等于 s[j]，如果i和j不相邻，即 i - j >= 2 时，除了判断 s[i] 和 s[j] 相等之外，dp[j+1] [i-1]就是回文串，通过以上分析，可以写出递推式如下：

dp[i, j] = 1                                               if i == j

​           = s[i] == s[j]                                if j = i + 1

​           = s[i] == s[j] && dp[i + 1] [j - 1]    if j > i + 1      

代码如下：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.empty()) return "";
        int dp[s.size()][s.size()] = {0}, left = 0, right = 0, len = 0;
        for (int i = 0; i < s.size(); ++i) {
            dp[i][i] = 1;
            for (int j = 0; j < i; ++j) {
                dp[j][i] = (s[i] == s[j] && (i - j < 2 || dp[j + 1][i - 1]));
                if (dp[j][i] && len < i - j + 1) {
                    len = i - j + 1;
                    left = j;
                    right = i;
                }
            }
        }
        return s.substr(left, right - left + 1);
    }
};
```

