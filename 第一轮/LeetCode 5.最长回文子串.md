## 暴力匹配

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        size = len(s)
        if size < 2:
            return s
        
        max_len = 1
        res = s[0]

        for i in range(size - 1):
            for j in range(i + 1, size):
                if j - i + 1 > max_len and self.__valid(s, i, j):
                    max_len = j - i + 1
                    res = s[i:j + 1]
        return res


    
    def __valid(self, s, left, right):
        while(left < right):
            if s[left] != s[right]:
                return False 
            left += 1
            right -= 1
        return True
```

## 动态规划

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        size = len(s)
        if size < 2:
            return s

        dp = [[False for _ in range(size)] for _ in range(size)]


        max_len = 1
        start = 0

        for i in range(size):
            dp[i][i] = True
        
        for j in range(1, size):
            for i in range(0, j):
                if s[i] == s[j]:
                    if j - i < 3:
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i + 1][j - 1]
                else:
                    dp[i][j] = False
                
                if dp[i][j]:
                    cur_len = j - i + 1
                    if cur_len > max_len:
                        max_len = cur_len
                        start = i
                              
        return s[start:start + max_len]
```

## 中心扩散法

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # 长度小于2直接返回s本身
        size = len(s)
        if size < 2:
            return s
        
        # 初始化最长长度为1
        max_len = 1
        res = s[0]

        for i in range(size):
            # 以i或者i与i+1中的间隙为中心开始扩散
            palindrome_odd, odd_len = self.__center_spread(s, size, i, i)
            palindrome_even, even_len = self.__center_spread(s, size, i, i + 1)

            # 当前找到的最长回文子串，就是奇数或者偶数中最长的那个
            cur_max_sub = palindrome_odd if odd_len >= even_len else palindrome_even
            if len(cur_max_sub) > max_len:
                max_len = len(cur_max_sub)
                res = cur_max_sub

        return res

    # 定义中心扩散函数
    def __center_spread(self, s, size, left, right):
        """
        left = right的时候，此时回文中心是一个字符，回文串的长度是奇数
        right = left + 1的时候，此时回文中心是一个空隙，回文串的长度是偶数
        """
        i = left
        j = right 

        # 注意这里i和j的值，最后一次i-1，j+1，所以计算长度时候为j-i-1
        while i >= 0 and j < size and s[i] == s[j]:
            i -= 1
            j += 1
        return s[i + 1:j], j - i - 1
```

