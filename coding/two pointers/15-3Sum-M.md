# 15-3Sum-M

## 题目描述
![15-q-1.png](../../imgs/two%20pointers/15-q-1.png)
![15-q-2.png](../../imgs/two%20pointers/15-q-2.png)


题意：
- 给一个整数数组，返回满足三数之和为0的三元组，返回二维数组
- 三元组内部元素可以重复，但是三元组不能重复

- 解法：
- Tow pointers

## 1. Brute Force
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = set()
        nums.sort()
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    if nums[i] + nums[j] + nums[k] == 0:
                        tmp = [nums[i], nums[j], nums[k]]
                        res.add(tuple(tmp))
        return [list(i) for i in res]
```

- TC: O(n^3)
- SC: O(m)
- m is the number of triplets(output list) and n is the length of the given array.

## 2. Hash Map
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        count = defaultdict(int)
        for num in nums:
            count[num] += 1

        res = []
        for i in range(len(nums)):
            count[nums[i]] -= 1
            if i and nums[i] == nums[i - 1]:
                continue

            for j in range(i + 1, len(nums)):
                count[nums[j]] -= 1
                if j - 1 > i and nums[j] == nums[j - 1]:
                    continue
                target = -(nums[i] + nums[j])
                if count[target] > 0:
                    res.append([nums[i], nums[j], target])

            for j in range(i + 1, len(nums)):
                count[nums[j]] += 1
        return res
```

- TC: O(n^2)
- SC: O(n)

## 3. Two Pointers
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        for i, a in enumerate(nums):
            if a > 0: # 剪枝
                break

            if i > 0 and a == nums[i - 1]: # 去重
                continue

            l, r = i + 1, len(nums) - 1
            while l < r:
                threeSum = a + nums[l] + nums[r]
                if threeSum > 0:
                    r -= 1
                elif threeSum < 0:
                    l += 1
                else:
                    res.append([a, nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while nums[l] == nums[l - 1] and l < r: # 去重
                        l += 1

        return res
```

- TC: O(n^2)
- SC: O(m)
- m is the number of triplets(output list) and n is the length of the given array.

分析：
- 本质就是固定一个数，转为求两数之和