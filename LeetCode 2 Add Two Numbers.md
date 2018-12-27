## <center>LeetCode 2 Add Two Numbers</center>

### 题目：

给定两个非空的链表，反向存储着每一位的大小，将着两个链表相加后的结果输出到另一个链表中。

### 示例：

```c++
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

### 分析：

new一个result链表后，可以用一个temp指针指向头结点的位置，然后对temp指针进行操作，这样可以方便保存结果链表的头结点的位置，需要牺牲头结点。

### 代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution 
{
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
        ListNode* result = new ListNode(0);
        ListNode* temp = result;
        int sum = 0;
        while(l1||l2)
        {
            if(l1)
            {
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2)
            {
                sum += l2->val;
                l2 = l2->next;
            }
            temp->next = new ListNode(sum%10);
            sum /= 10;
            temp = temp->next;
        }
        if(sum)
        {
            temp->next = new ListNode(sum);
        }
        return result->next;
    }
};
```

### 注意：

开始时候并没有考虑到需要牺牲头结点。再之后的分析中，发现如果temp开始的时候指向result的头结点而不是头结点的下一个结点，那么在while循环中，就需要先对temp->val赋值，然后再new一个新的node，但是如果最后sum为0，也就是并没有进位，这样会导致最后多出一个尾结点不容易删除，所以就索性牺牲头结点，最后返回next。

## 