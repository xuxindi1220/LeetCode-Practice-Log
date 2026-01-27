# 1046-Last Stone Weight-E

## 题目描述

![1046-q-1.png](../../imgs/heap/1046-q-1.png)
![1046-q-2.png](../../imgs/heap/1046-q-2.png)

题意：
- 给定一个正整数数组stones
- 对数组元素进行smash操作，返回最后剩下的值，若无值剩下，那么返回0. 
    - smash操作: 选数组里的**最大的两个元素**，只留下两值之差。也就是说如果两者相等，那么将这两个元素都pop出数组；不相等，pop出去后加入两值之差
- 与1049-Last Stone Weight II-M的区别：这里每一步都取最大的两个元素，最终返回是确定值，而不需要在过程中求最小


解法：
- Sorting
- Binary Search
- Heap
- Bucket Sort

## 1. Heap
```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        stone_heap = [-s for s in stones] # 用负数 + 最小堆，堆顶为最大值的相反数
        heapq.heapify(stone_heap)

        while len(stone_heap) > 1:
            first = heapq.heappop(stone_heap)
            second = heapq.heappop(stone_heap)
            if first < second:
                heapq.heappush(stone_heap, first - second)
        
        return 0 if len(stone_heap) < 1 else abs(stone_heap[0])
```

- TC: O(nlogn)
- SC: O(n)

## 2. Bucket Sort
```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:

        maxStone = max(stones)
        bucket = [0] * (maxStone + 1) # bucket是计数器
        for stone in stones:
            bucket[stone] += 1
        
        # first：当前最大的石头重量
        # second：第二大的石头重量（或上一次用到的）
        first = second = maxStone # 因为是选最重的两个，所以初始化为maxStone
        
        while first > 0:
            if bucket[first] % 2 == 0: # 当数量为偶数时，全部抵消；为0时，继续往下找
                first -= 1
                continue

            j = min(first - 1, second) # 当前可用的“第二大石头”，所以要找比 first 小的最大 j，不能等于first，因为bucket[first]是奇数
            while j > 0 and bucket[j] == 0: # 找到当前 < first 的最大 j，使得 bucket[j] > 0
                j -= 1

            if j == 0: # 没有第二块石头了，没有第二块石头了
                return first
            second = j
            bucket[first] -= 1
            bucket[second] -= 1
            bucket[first - second] += 1
            first = max(first - second, second) # 下一轮的最大石头：新生成的石头；还剩下的 second（或更小的）
        return first
```

- TC: O(n + m)
- SC: O(m)
- n = len(stones), m = max(stones)


