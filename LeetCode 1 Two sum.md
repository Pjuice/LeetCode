## <center>[LeetCode 1 Two sum]</center>

### 题目：

给定一个整数数列，找出其中和为特定值的那两个数。

你可以假设每个输入都只会有一种答案，同样的元素不能被重用。

### 示例：

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

### 分析：

这道题目可以直接遍历两次数组，时间复杂度为 $O(n^2)$，为了优化时间复杂度，牺牲空间复杂度，使用哈希表。

unordered_map内部实现了一个**哈希表**（也叫散列表，通过把关键码值映射到Hash表中一个位置来访问记录，查找的时间复杂度可达到O(1)，其在海量数据处理中有着
广泛应用）。因此，其元素的排列顺序是无序的。用哈希表先把数组中的数字和对应的下标存储一遍，即数字作为键，下标作为值，存储，当遍历数组的时候用target-
nums[i]，得到差k，然后在map中找是否存在 k，找到即返回k所对应的value,也就是所对应的数组下标。这样时间复杂度就为O(n+l)。

```c++




class Solution
{
    public:
    vector<int> twoSum(vector<int>& nums, int value)
    {
        int sz = nums.size();
        vector<int> vi(2);
        unordered_map<int,int> m;
        for (int i = 0; i < sz; ++i)
        {
            m[nums[i]] = i;
        }
        for (int i = 0; i < sz; ++i)
        {
            int k = value - nums[i];
            if(m.count(k) && m[k] != i)
            {
                vi[0] = i;
                vi[1] = m[k];
                return vi;
            }
        }
        return vi;
    }
};

```

### 注意：

在求得k对应的m中存储的下标的时候要和当前对比，需要二者大小不同。
