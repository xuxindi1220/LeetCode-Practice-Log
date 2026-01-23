# 743-Network Delay Time-M

## 题目描述
![743-q-1.png](../../imgs/Advanced%20Graphs/743-q-1.png)
![743-q-2.png](../../imgs/Advanced%20Graphs/743-q-2.png)

题意：
- 给定一个数组times，一个整数n，一个整数k
  - times[i] = (ui, vi, ti)
    - ui, vi都是节点，值为1到n
    - ti是从ui 到 vi所需的时间
  - n: n个节点
  - k: 从值为k的节点出发
- 从值为k的节点出发，遍历所有节点，求最小的ti总和; 若无法遍历所有节点，返回-1

解法：
- dfs
- Floyd Warshall Algorithm
- Bellman Ford Algorithm
- Shortest Path Faster Algorithm
- Dijkstra's Algorithm

## 1. Dijkstra's Algorithm(minHeap)
```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        edges = collections.defaultdict(list)
        for u, v, w in times:
            edges[u].append((v, w))

        minHeap = [(0, k)] # 因为要最后要输出最小的，所以用minHeap，并且注意权重和要放在前面
        visit = set()
        t = 0
        while minHeap:
            w1, n1 = heapq.heappop(minHeap) # 注意是heappop，而非pop
            if n1 in visit:
                continue
            visit.add(n1) # 这里需要add，下面for里不需要，因为for里不是真的访问，只是将候选项放入，要pop出最小的才是。
            t = w1

            for n2, w2 in edges[n1]:
                if n2 not in visit:
                    heapq.heappush(minHeap, (w1 + w2, n2))
        return t if len(visit) == n else -1
```

- TC: O(ElogV)
- SC: O(V+E)
  - edges: O(E), visit: O(V)
- V = num of vertices, E = num of edges = len(times)


- minHeap 里的 tuple 是按“字典序（lexicographical order）”排序的，也就是先排tuple[0]，再排tuple[1]...

