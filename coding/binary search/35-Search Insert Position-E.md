# 35-Search Insert Position-E

## 题目描述

![35-q.png](../../imgs/binary%20search/35-q.png)

题意：
- 给定一个整数数组 nums，和一个整数 target。nums 递增且无重复元素
- 若nums中存在target，返回下标；若不存在，返回插入的位置的下标
- 简而言之，就是找到第一个 >= target的下标

解法：
- Binary Search (Lower Bound)

## 1. Binary Search (Lower Bound)
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)
        while l < r:
            m = l + ((r - l) // 2)
            if nums[m] >= target:
                r = m
            elif nums[m] < target:
                l = m + 1
        return l
```

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        n = len(nums)
        l, r = 0, n - 1
        while l <= r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            elif nums[m] > target:
                r = m - 1
            else:
                l = m + 1
        return l
```

- TC: O(nlogn)
- SC: O(1)

## 2. Built-In Binary Search Function
```python
import bisect
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        return bisect.bisect_left(nums, target)
```

bisect.bisect_left