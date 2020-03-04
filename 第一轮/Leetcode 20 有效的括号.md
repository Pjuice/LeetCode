Leetcode 20 有效的括号

感觉这道题主要是考察栈的用法，较简单。



```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) == 0:
            return True
        else:
            stack = []
            for c in s:
                if c == '(':
                    stack.append(')')
                elif c == '{':
                    stack.append('}')
                elif c == '[':
                    stack.append(']')
                elif (len(stack)==0  or c != stack.pop()):
                    return False
            if len(stack)==0:
                return True
            return False


```

这里难受到我的是自己对Python的list的不熟悉，一开始甚至以为list有push这个操作...那么为什么list有pop操作缺没有push操作呢？主要是因为在认识到push的作用之前就有了append这个操作，而且功能基本一致，所以就没必要再去有一个list的push方法了。

另外，还可以使用栈和字典的方法一起来完成，这里也是参考了大神的讲解。https://leetcode-cn.com/problems/valid-parentheses/solution/valid-parentheses-fu-zhu-zhan-fa-by-jin407891080/



```python
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {'{': '}',  '[': ']', '(': ')', '?': '?'}
        stack = ['?']
        for c in s:
            if c in dic: stack.append(c)
            elif dic[stack.pop()] != c: return False 
        return len(stack) == 1
```

