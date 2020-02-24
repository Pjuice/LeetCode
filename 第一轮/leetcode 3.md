1.滑动窗口的概念。什么是滑动窗口？

其实就是一个队列,比如例题中的 abcabcbb，进入这个队列（窗口）为 abc 满足题目要求，当再进入 a，队列变成了 abca，这时候不满足要求。所以，我们要移动这个队列！

如何移动？

我们只要把队列的左边的元素移出就行了，直到满足题目要求！

一直维持这样的队列，找出队列出现最长的长度时候，求出解！

时间复杂度：O(n)

注意代码中的while，要把所有重复元素左边的元素全部移除，不能用if操作。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s : return 0
        lens = len(s)
        left = 0
        temp_len = 0
        max_len = 0
        record = set()
        for i in range(lens):
            temp_len += 1
            while s[i] in record:
                record.remove(s[left])
                left += 1
                temp_len -= 1
            if temp_len > max_len:
                max_len = temp_len
            record.add(s[i])
        return max_len

```

