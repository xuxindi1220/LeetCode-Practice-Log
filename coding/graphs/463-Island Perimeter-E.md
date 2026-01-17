# 463-Island Perimeter-E

Perimeter 周长

## 题目描述
![463-q-1.png](../../imgs/graphs/463-q-1.png)
![463-q-2.png](../../imgs/graphs/463-q-2.png)
![463-q-3.png](../../imgs/graphs/463-q-3.png)

题意：
- 二维数组，元素1代表地，0代表水
- 求所有岛屿的周长之和
- 题目已经简化，假定不会出现有lake的情况。
  - lake: 岛内有水，比如一个3*3的九宫格，最中间是水其他是地。这样算1个岛屿，但是周长计算变复杂了

解法：
- dfs
- bfs
- iteration

## 1. Iteration

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0
        for r in range(m):
            for c in range(n):
                if grid[r][c] == 1:
                    res += 4
                    if r and grid[r - 1][c] == 1:
                        res -= 2
                    if c and grid[r][c - 1] == 1:
                        res -= 2
        return res
```

- TC: O(m * n)
- SC: O(1)
- m is len of row, n is len of col

分析：
- 每一格都是正方形，边长为4
- 所以遇到岛屿，就加4。
- 而若相邻格子也是岛屿，那么需要减去相邻的边2遍
  - 为了方便且不重复计算，从坐标(0,0)出发，且check上一行及左一列这两个方向的相邻格子