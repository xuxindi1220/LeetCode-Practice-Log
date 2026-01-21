# 1631-Path with Minimum Effort-M

## 题目描述
![1631-q-1.png](../../imgs/Advanced%20Graphs/1631-q-1.png)
![1631-q-2.png](../../imgs/Advanced%20Graphs/1631-q-2.png)

题意：
- m * n 的格子，元素为数字，代表height
- 人要从最左上走到最右下，求Minimum Effort
  - 人能走四个方向，上下左右
  - A route：从最左上走到最右下路上的height的list
  - A route's effort：max(一条路径上相邻两个值的差的绝对值)
    - 比如路径为[1,1,2,4,4]，effort为2


解法：
- Dijkstra's Algorithm
- dfs
- Shortest Path Faster Algorithm(本质bfs)


## 1. Shortest Path Faster Algorithm(本质bfs)

```python
class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        ROWS, COLS = len(heights), len(heights[0])
        dist = [float('inf')] * (ROWS * COLS) # dist[i] → 表示 从起点到格子 i 的最小“努力值”；初始化为无穷大（因为后面要比小）
        dist[0] = 0
        in_queue = [False] * (ROWS * COLS) # 记录格子 i 是否在队列中，用于 SPFA 优化

        def index(r, c): # 辅助函数：将二维坐标 (r, c) 转换为一维索引，方便存储在 dist 数组里
            return r * COLS + c # 注意这里是COLS，非ROWS；想想第二行的第一列，一维结果是1 * COLS + c

        queue = deque([0])
        in_queue[0] = True
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

        while queue:
            u = queue.popleft()
            in_queue[u] = False # 改为False是因为弹出了
            r, c = divmod(u, COLS) # 注意这里是COLS，非ROWS

            for dr, dc in directions:
                newR, newC = r + dr, c + dc
                if 0 <= newR < ROWS and 0 <= newC < COLS: # 在邻居不越界的前提下
                    v = index(newR, newC)
                    weight = abs(heights[r][c] - heights[newR][newC])
                    new_dist = max(dist[u], weight)
                    if new_dist < dist[v]:
                        dist[v] = new_dist
                        if not in_queue[v]: # 入队
                            queue.append(v)
                            in_queue[v] = True

        return dist[ROWS * COLS - 1]
```

### 时空复杂度：
- TC: 
  - O(m * n) in average case. 
    - average case：绝大多数格子只会被更新一次或几次
  - O(m^2 * n^2) in worst case.
    - worst case：每次更新都导致邻居入队，邻居的邻居再入队...
      - 每个格子可能被入队O(m * n)次，所以总共O((m * n) * (m * n))
- SC: O(m * n)
  - dist 和 in_queue
- m is len of row, n is len of col

### 分析：
- 这道题本质上是 “最短路径问题”，不过路径权值不是累加，而是取路径中最大边权
- SPFA(Shortest Path Faster Algorithm) / BFS 通过队列逐步更新每个格子的最小努力值
- dist[u] → 表示 从起点到格子 i 的最小“努力值”，也就是最小的最大高度差
- BFS 保证每次尝试更新邻居，只要能降低最大高度差就更新

### 代码分析：
- r, c = divmod(u, COLS)
  - divmod(a, b)得到的是tuple：第一个数是a // b；第二个数是a % b
  - 而放入队列的u：u = r * COLS + c
- u, v相当于就是两个格子（的下标的一维形式）

1. 反复入队出队，难道不会重复计算吗？
   - 重复入队是必须的，因为路径的最小努力值可能被更优的路径不断降低
     - SPFA 的核心是 放松（relax）算法：
       - 每次从队列取出一个格子 u，看能否通过它更新邻居 v 的 dist[v]
       - 如果能更新 → 把 v 放入队列，继续尝试更新它的邻居
2. 为什么“new_dist = max(dist[u], weight)”？
    - 已知：dist[u]是(0,0)到u的effort；weight是u到v的effort
    - 那么起点到v的effort = 起点到u的effort 与u到v的effort的最大值
      - 起点到u的effort也是起点到之前点与其他weight比较的结果
      - 所以max(dist[u], weight)就是要求的最大高度差
    为什么是这俩取最大，最大的最小effort，不理解
3. 而dist[i]其实是最小的最大高度差，所以需要比较new_dist与dist[v]，取较小值
   - new_dist更小的情况下，再将v入队，往四个方向传播计算