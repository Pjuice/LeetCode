1. 超出时间。纯暴力。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        lens = len(nums)
        for i in range(lens):
            for j in range(lens):
                if (nums[i] + nums[j] == target) and (i != j):
                    return i,j
```

2.用时1466ms，内存29.3m。使用list的index。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        lens = len(nums)
        flag = 0
        for i in range(lens):
            if (target - nums[i]) in nums:
                if (nums.count(target - nums[i]) == 1 and target - nums[i] == nums[i]):
                    continue
                else:
                    flag = 1
                    j = nums.index(target - nums[i] , i+1)
                    break
        if flag == 1:
            return [i,j]
        else:
            return []
```

3.构建临时list，每次index只查找一部分。用时468ms，内存29.3m。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        lens = len(nums)
        flag = 0
        for i in range(1, lens):
            temp = nums[:i]
            if (target  - nums[i]) in temp:
                flag = 1
                j = temp.index(target - nums[i])
                break
        if flag == 1:
            return[i,j]
        
```

4.使用哈希。用时大大缩短，128ms，内存略有增大，30.5m。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap= {}
        for index, num  in enumerate(nums):
            hashmap[num] = index
        for i,num in enumerate(nums):
            j = hashmap.get(target - num)
            if j is not None and i != j:
                return [i,j]
```

5.使用哈希，并且查找时在num1之前的dict查找，只需要一次循环即可。不过时间没有提升。但代码简单了很多。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap= {}
        for i,num in enumerate(nums):
            if hashmap.get(target - num) is not None:
                return [i, hashmap.get(target - num)]
            hashmap[num] = i
```

6.极度精简版。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}
        for i,n in enumerate(nums):
            if target - n in dic:
                return [dic[target - n], i]
            dic[n] = i
```

