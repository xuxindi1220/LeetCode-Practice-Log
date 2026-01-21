# 77-Combinations-E

## 题目描述
![77-q.png](../../imgs/backtracking/77-q.png)

题意：
- 给2个整数，n 和 k，从n个数（1到n）里选出 k 个数，返回所有答案集合（二维数组）


解法：
- Backtracking
- Bit Manipulation

## 1. Backtracking
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []

        def backtrack(start, comb):
            if len(comb) == k:
                res.append(comb.copy())
                return

            for i in range(start, n + 1): # 因为可选范围是1到n
                comb.append(i)
                backtrack(i + 1, comb)
                comb.pop()

        backtrack(1, []) # 可选范围是1到n，所以从1开始
        return res
```

- TC: O(C(n, k) * k)
  - 从 n 个数里选 k 个，一共有 C(n, k) 种组合
  - C(n, k) = n! / ((n-k)! * k!)
  - .copy() 需要拷贝 k 个元素
- SC: O(C(n, k) * k), output space