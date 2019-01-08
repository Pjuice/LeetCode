## <center>[LeetCode 4 Median of Two Sorted Arrays]</center>

### 题目：

给定两个不同时为空的数组，找到这两个数组的中位数。时间复杂度应为O(log (m+n)).

### 示例：

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

### 分析：

一开始看到这道题目的时候觉得可以直接merge两个数组然后直接找到中位数，但是想了下如果这样做的话时间复杂度就为O(m+n)，而不是题目要求的O(log (m+n)).所以应该找到一个时间复杂度更小的方法。

实际上，求得两个数组中的中位数，类似于在两个数组中找到第k个小的数。利用寻找第K个元素的辅助函数。
二分搜索的思想，每次尽量去掉数组中的一部分元素(一半左右)；
一次取K个元素出来，nums1中取 K/2个(不够就全都取出)， nums2中取 K - K/2(或nums1.size()),
判断取出的两个数组元素中的末位谁大谁小；
如果nums1[p1] < nums2[p2]，说明nums1取少了，nums2取多了，第K个元素应该在nums1的后半部分或nums2的前半部分；
如果nums1[p1] > nums2[p2], 说明nums2取少了，nums1取多了，第K个元素应该在nums2的后半部分或nums1的前半部分；
递归求解即可 。边界条件是nums1或nums2为空或K为1；

```
题目思路不是很难想到，但是处理细节有很多容易错的地方。
1.首先应确定Kth是第K个元素，对应下标应该为K-1
2.数组下标，取1/2位置时对应的哪个位置等等要注意，可以采用走样例的方式保证不出错。
3.采用了两种实现方法，一种是拷贝vector，要传参的时候注意左闭右开；
　　　　　　　　　　  另一种是不拷贝vector，多传两个起始位置start，注意每次取元素操作时不能忘记start
  第二个效率稍高，但编码中可能出错的地方也多一点。
```

```c++
class Solution {
private:
    double findKth(vector<int>& nums1, vector<int>& nums2, int K) { //第K个，对应下标K-1
        if (nums1.size() > nums2.size()) {
            return findKth(nums2,nums1,K);
        }
        if (nums1.size() == 0) {
            return nums2[K-1];
        }
        if (nums2.size() == 0) {
            return nums1[K-1];
        }
        if (K == 1) {
            return min(nums1[0], nums2[0]);
        }
        int s = nums1.size();
        int p1 = min( K / 2, s);
        int p2 = K - p1;
        if (nums1[p1 - 1] < nums2[p2 - 1]) { //说明nums1取少了，kth在nums1后半段或nums2前半段
            vector<int> n1(nums1.begin() + p1, nums1.end());
            vector<int> n2(nums2.begin(), nums2.begin() + p2);
            return findKth(n1, n2, K - p1);
        }
        else {
            vector<int> n3(nums1.begin(), nums1.begin() + p1);
            vector<int> n4(nums2.begin() + p2, nums2.end());
            return findKth(n3, n4, K - p2);
        }
     }
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int s1 = nums1.size(), s2 = nums2.size();
        int mid = (s1 + s2) / 2;
        if ( (s1 + s2) % 2 == 0 ) {
            return (findKth(nums1, nums2, mid) + findKth(nums1, nums2, mid + 1)) / 2.0; 
        } 
        else {
            return findKth(nums1, nums2, mid + 1);
        }
    }
};
```



```c



int min(int a, int b)
{
    return a <= b? a : b;
}
int getkth(int* s, int m, int* l, int n, int k)
{
        //确保m < n 
        if (m > n) 
            return getkth(l, n, s, m, k);
        if (m == 0)
            return l[k - 1];
        if (k == 1)
            return min(s[0], l[0]);
	    //递归过程
        int i = min(m, k / 2), j = min(n, k / 2);
        if (s[i - 1] > l[j - 1])
            return getkth(s, m, l + j, n - j, k - j);
        else
            return getkth(s + i, m - i, l, n, k - i);
        return 0;
}


double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) 
{
        //总长度的一半
        int l = (nums1Size + nums2Size + 1) / 2;
        int r = (nums1Size + nums2Size + 2) / 2;
        return (getkth(nums1, nums1Size ,nums2, nums2Size, l) + getkth(nums1, nums1Size ,nums2, nums2Size, r)) / 2.0;
}




```

### 注意：

注意边界情况下的选择，如k=0或1的情况。注意到时间复杂度为log级别，就应该想到使用二分思想解决。