## 33.Search in Rotated Sorted Array-M

## 题目描述

![33-q-1.png](../../imgs/binary%20search/33-q-1.png)
![33-q-2.png](../../imgs/binary%20search/33-q-2.png)

## 题意分析

题意：
- 递增序列或分为两段的递增序列，找target，找到返回下标，没找到返回-1

解法：
1. Brute Force
2. Binary Search
    - One pass
    - Two pass

## 1. Brute Force

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        for i in range(len(nums)):
            if nums[i] == target:
                return i
        return -1
```

- TC: O(n)
- SC: O(1)

## 2. Binary Search (Two Pass)

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l < r:
            m = (l + r) // 2
            if nums[m] > nums[r]:
                l = m + 1
            else:
                r = m

        pivot = l
        l, r = 0, len(nums) - 1

        if target >= nums[pivot] and target <= nums[r]:
            l = pivot
        else:
            r = pivot - 1

        while l <= r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            elif nums[m] < target:
                l = m + 1
            else:
                r = m - 1

        return -1
```
- TC: O(logn)
- SC: O(1)

注意本题求最小点pivot的写法：
1. 因为有nums[r]一定位于 递增区间的尾部，始终 ≥ 最小值；但 nums[l]可能在递增段，也可能在旋转段
所以在if判断时要用nums[r]，而不用nums[l]
2. 当mid <= 右边时，更新r为mid，而非mid-1。不然就会少值了
3. 从r=mid而非r=mid-1的更新就可以看出，这不是闭区间，而是左闭右开区间，所以while的条件不加等号


## 3. Binary Search (One Pass)

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l <= r:
            mid = (l + r) // 2
            if target == nums[mid]:
                return mid

            if nums[l] <= nums[mid]:
                if target > nums[mid] or target < nums[l]:
                    l = mid + 1
                else:
                    r = mid - 1

            else:
                if target < nums[mid] or target > nums[r]:
                    r = mid - 1
                else:
                    l = mid + 1
        return -1
```

- TC: O(logn)
- SC: O(1)

| 判断层 | 条件                 | 有序区间      | target在哪                             | 下一步              |
|-----|--------------------|-----------|--------------------------------------|------------------|
| ①   | nums[l] <= nums[m] | 左半 [l..m] | target > nums[m] or target < nums[l] | 去右半 → l = m + 1  |
| ②   | nums[l] <= nums[m] | 左半 [l..m] | 否则                                   | 	去左半 → r = m - 1 | 
| ③   | nums[l] > nums[m]  | 右半 [m..r] | target < nums[m] or target > nums[r] | 去左半 → r = m - 1  | 
| ④   | nums[l] > nums[m]  | 右半 [m..r] | 否则                                   | 去右半 → l = m + 1  | 




**直观总结一句话:**
- 每一轮循环：
  - 先判断哪半区是递增的；
  - 再看 target 是否在这个递增区间的值域内；
  - 如果在，就往这边缩；
  - 如果不在，就去另一边。