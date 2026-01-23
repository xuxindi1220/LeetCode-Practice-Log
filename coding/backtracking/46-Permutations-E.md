# 46-Permutations-E

## 题目描述
![46-q.png](../../imgs/backtracking/46-q.png)

题意：
- 给定一个整数数组 nums，元素都是unique的，返回全排列（二维数组）

解法：
- Recursion
- Iteration
- Backtracking

## 1. Backtracking
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        pick = [False] * len(nums)

        def dfs(path, pick):
            if len(path) == len(nums):
                res.append(path.copy())
            
            for i in range(len(nums)):
                if not pick[i]:
                    path.append(nums[i])
                    pick[i] = True
                    dfs(path, pick)
                    path.pop()
                    pick[i] = False
        dfs([], pick)
        return res
```

- TC: O(n! * n)
  - 全排列有n!种；copy需要O(n)
- SC: O(n! * n) for the output list