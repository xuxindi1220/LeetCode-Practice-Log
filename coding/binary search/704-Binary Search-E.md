# 704-Binary Search-E

## 题目描述
![704-q.png](../../imgs/binary%20search/704-q.png)

题意：
- 给一个整数数组，元素递增且不重复，和一个 target
- 要求在O(logn)时间内在数组中找出target，返回下标，若不存在返回-1

解法：
- Binary Search

## 1. Binary Search(Lower Bound)
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)

        while l < r:
            m = l + ((r - l) // 2)
            if nums[m] >= target:
                r = m
            elif nums[m] < target:
                l = m + 1
        return l if (l < len(nums) and nums[l] == target) else -1
```

- TC: O(logn)
- SC: O(1)

分析：
- 最后return里要判断下标是否越界的原因是r初始化为len(nums)
  - 当target大于所有元素时，最终l == len(nums)，不加边界判断，会使得代码在nums[l] == target这一句报错
- 如果初始化为len(nums) - 1就不需要判断了
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l < r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            elif nums[m] > target:
                r = m - 1
            else:
                l = m + 1
        return l if nums[l] == target else -1
```