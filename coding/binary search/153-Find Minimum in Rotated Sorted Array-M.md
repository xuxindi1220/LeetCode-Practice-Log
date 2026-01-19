# 153-Find Minimum in Rotated Sorted Array-M

## 题目描述
![153-q-1.png](../../imgs/binary%20search/153-q-1.png)
![153-q-2.png](../../imgs/binary%20search/153-q-2.png)

题意：
- 递增序列或分为两段的递增序列，返回其中的最小值


解法：
- binary search

## 1. Binary Search
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        res = nums[0]
        l, r = 0, len(nums) - 1

        while l <= r:
            if nums[l] < nums[r]:
                res = min(res, nums[l])
                break

            m = (l + r) // 2
            res = min(res, nums[m])
            if nums[m] >= nums[l]:
                l = m + 1
            else:
                r = m - 1
        return res
```

- TC: O(log n)
- SC: O(1)

## 2. Binary Search (Lower Bound)
```python
class Solution:
    def findMin(self, nums: List[int]) -> int: 
        l, r = 0, len(nums) - 1
        while l < r:
            m = l + (r - l) // 2
            if nums[m] < nums[r]:
                r = m
            else:
                l = m + 1
        return nums[l]
```
- TC: O(log n)
- SC: O(1)

分析：
- lower bound = 第一个满足条件的位置
- while 里为什么不含等号？
  - lower bound 的搜索区间是闭区间 [l, r]，但当 l == r 时，答案已经确定了，无需进入循环
- 为什么用nums[r]作为判断依据？
  - 本质是找右半段的起点
- 为什么r 和 l的更新一个需要加1，另一个不需要？
  - if分支：最小值可能为m，不能排除
  - else分支：说明m 在左半段，所以m可以排除
- 为什么最终答案是nums[l]？
  - 最终l == r，都一样