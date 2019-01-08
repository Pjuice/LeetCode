## <center>[LeetCode 3  Longest Substring Without Repeating Characters]</center>

### 题目：

给定一个字符串，找出不带重复字符的最长子串。

### 示例：

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

### 分析：

这道题目可以使用滑动窗口的思想解决。使用一个map存储char到int的映射，来判断目前分析的字符是否在之前出现过。需要定义一个begin和end值，用以记录目前分析的不含重复字符的子串的起始位置，而map中记录的是每个字符在该给定字符串中最后出现的位置。首先对map进行初始化，第一个字符肯定没有出现过，所以初始化key值为0，然后使用end记录当前分析的字符的位置，进行一次遍历。每次更新指针位置后，就判断该字符是否在map中出现过，如果没有出现过，就直接将map中该字符的key值设置为当前的指针位置，然后继续分析下一个字符。如果目前分析的字符已经在map中出现过，也就是说出现了重复的字符，那么就要计算目前前面分析过的没有出现重复字符的字符串的长度，并且改变目前的begin值。当出现过重复字符后，计算gap值和目前已经有的ans值比较，如果gap值更大，则替换。然后begin值和目前重复字符的map中的key值+1相比较，如果后者较大，就说明目前重复的字符的上一次出现位置是位于begin之后的，所以将begin替换为出现过重复字符的位置的下一个位置，即保证了现在从begin到end这一段没有重复字符，然后更新map[s[end]]值为目前的指针位置。

分析完整个字符串后，要在最后进行比较目前分析结束后的最后一段不含重复字符的字符串的长度和之前计算的最长的长度，因为更新长度发生在每一次出现重复字符之后，所有最后一段不会自动计算长度，需要在循环之外进行额外计算比较。

```c++



class Solution 
{
public:
    int lengthOfLongestSubstring(string s) 
    {
        if(s.length() <= 1)
        {
            return s.length();
        }
        
        int begin = 0;
        int end = 0;
        map <char, int> mapping;
        int ans = 0;
        mapping[s[0]] = 0;
        while (end < s.length() - 1)
        {
            end ++;
            if (mapping.find(s[end]) != mapping.end() )
            {
                int gap = end - begin;
                if (gap > ans)
                {
                    ans = gap;
                }
                begin = mapping[s[end]] + 1 > begin ? mapping[s[end]] + 1 : begin;
                mapping[s[end]] = end;
            }
            else
            {
                mapping[s[end]] = end;
            }
        }
        
        int gap = end - begin + 1;
        if (gap > ans)
        {
            ans = gap;
        }
        return ans;
    }
};
```

### 注意：

注意子串和子序列的区别。