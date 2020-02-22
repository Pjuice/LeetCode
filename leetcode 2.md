1.遍历两个链表，利用l1链表来存储结果。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        a, b, p, carry = l1, l2, None, 0
        while a or b:
            val = (a.val if a else 0) + (b.val if b else 0) + carry
            carry, val = int(val/10) if val >=10 else 0, int(val%10)
            p, p.val = a if a else b, val
            a, b = a.next if a else None, b.next if b else None
            p.next = a if a else b
        if carry:
            p.next = ListNode(carry)
        return l1 

```

每次，P（哨兵节点）优先指向l1链表，即改变l1中的val，当l1长度不够时，l1的尾结点的next就是b中的结点，或者说一个新的结点（当最后的carry不为0时）。

2.递归实现。递归需要结束条件和递归期间的计算，在这里递归的结束条件就是a和b都为None，递归期间的计算就是a.val与b.val与carry相加赋值给a。

这里需要注意第二个条件，因为进位标志需要通告下一层递归函数，所以需要有一个单独的变量作为记录。
函数内部的进位标志判断，val计算的方式和迭代版本是类似的。
调用下一层递归的时候，传递的参数是a.next和b.next。
这里还需要注意一个细节，如果a，b两个链表不一样长，意味递归到一定的层次时，某个链表会出现null，这时需要做一个补0的操作，创建一个新的节点赋给节点为空的链表。这也是为什么递归函数的终止条件是a和b都==null的原因。

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        def add(a, b, carry):
            if not (a or b):
                return ListNode(1) if carry else None
            a = a if a else ListNode(0)
            b = b if b else ListNode(0)
            val = a.val + b.val + carry
            carry = 1 if val >=10 else 0
            a.val = int(val%10)
            a.next = add(a.next, b.next, carry)
            return a 
        return add(l1, l2, 0)
```

