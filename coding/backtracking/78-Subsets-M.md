# 78-Subsets-M

## 题目描述
![78-q.png](../../imgs/backtracking/78-q.png)

## 题意分析

题意:
- 给数组求子集

限制：
- 数组数字都是唯一的
- 返回的子集元素顺序不重要

解法：
- Backtracking


## 1. Backtracking
分析：
- 求集合的子集：n个元素，根据每一位选或不选，各有2种选择，总共就有2^n种结果
- **递归三问：**
  - 当前操作是什么？
  - 子问题是什么？
  - 下一个子问题是什么？
- **回溯**
  - 边界条件：下标到达末尾
  - 当前操作：选 或 不选
  - 子问题：下标 + 1

```python
class Solution:
    def subsets(self, nums: list[int]) -> list[list[int]]:
        res = []
        subset = []

        def dfs(i):
            if i >= len(nums): # >= 是防御式写法；本题中 == 可行
                res.append(subset.copy()) # 注意要浅拷贝
                return
            # 选
            subset.append(nums[i])
            dfs(i + 1)
            subset.pop()
            # 不选
            dfs(i + 1)

        dfs(0)
        return res
```

- TC: O(n * 2^n)
  - 递归 O(2^n) 次
  - 加入答案时复制 subset (执行 subset.copy())需要 O(n) 的时间
- SC: O(n) 递归栈的空间
  - 返回值的空间不计入
 
1. 要浅拷贝的原因
- subset 是一个全局共享列表，DFS 会不断 append/pop 改变它
- 如果直接 res.append(subset)，所有子集都会最终指向同一个 subset 对象，结果会出错
  - 最终res的元素都是subset的最终状态
    - 因为不断pop，所以subset的最终状态是[]
    - 所以最终res为[[], [], [], ...]
- .copy() 会生成一个 新的列表对象，保存当前状态
- 因为子集里的元素都是 不可变类型（int），所以浅拷贝就足够了，不需要深拷贝

2. 若将选(3行代码)和不选(1行代码)调换位置，不会影响本题通过性。只是res里子集元素顺序会不一样。