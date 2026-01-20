# 269-Alien Dictionary-H

## 题目描述
![269-q-1.png](../../imgs/Advanced%20Graphs/269-q-1.png)
![269-q-2.png](../../imgs/Advanced%20Graphs/269-q-2.png)

lexicographically: 字典顺序

题意：
- input是由 外星语言的单词 组成的列表 words，这些单词已经是按外星语言字典序排序好的
- output是要根据这些单词的顺序，推断出外星语言中所有字符的先后顺序
- 若顺序不存在，返回""：比如有环、后词是前词的前缀（前词 = 后词 + xx字符）
- 若顺序不唯一，返回任意一个合法的顺序：发生在图不是一个连通图的情况下，有多个独立的小图（子图 / 连通分量）。比如a->b, c->d那么返回abcd, acbd等都是可以的。只要保证各图内部顺序就行


- 解法：
- Topological Sort (Kahn's Algorithm/BFS)
- dfs

## 1.Topological Sort (Kahn's Algorithm/BFS)
```python
class Solution:
    def foreignDictionary(self, words: List[str]) -> str:
        adj = {c: set() for w in words for c in w} # adj 是邻接表，adj[c] 表示字母 c 指向的后续字母集合
        indegree = {c: 0 for c in adj} # 当前字母的入度（有多少前驱字母必须在它前面）。注意key是adj里的字符

        for i in range((len(words) - 1)):
            w1, w2 = words[i], words[i+1]
            minLen = min(len(w1), len(w2))
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return "" # invalid：后词是前词的前缀
            for j in range(minLen):
                if w1[j] != w2[j]: # 找到第一个不相同的字符，w1[j]是w2[j]的前驱
                    if w2[j] not in adj[w1[j]]: # 且之前没加入过w1[j]邻接表的
                        adj[w1[j]].add(w2[j])
                        indegree[w2[j]] += 1
                    break # 注意这里有break，因为一对word只能管一组大小关系
        
        # 把所有入度为 0 的字符加入队列（即没有前置依赖的字母）
        q = deque([c for c in indegree if indegree[c] == 0])
        res = []

        while q:
            # 弹出入度为 0 的节点， 表示它的字母顺序确定了，可以加入res了
            char = q.popleft()
            res.append(char)
            for nei in adj[char]:
                indegree[nei] -= 1
                if indegree[nei] == 0: # 入度为0才可以加入
                    q.append(nei)
        
        # len(indegree)是图中所有出现过的字符的总数
        # len(res)是最终拓扑排序结果中被加入的字符数。也就是在拓扑排序过程中，入度变为 0 的节点数量。
        # a → b, b → a情况下，len(res) = 0，len(indegree) = 2
        if len(res) != len(indegree): # invalid：有环
            return ""
        
        return "".join(res)
        
```

时空复杂度：
- TC: O(N+V+E)
- SC: O(V+E)
- V = unique characters数, E = edges数 and N = sum of lengths of all the strings.
- TC: 
  1. adj,indegree的初始化和更新需要遍历所有字符串中的字符，所以O(N)
  2. bfs里每个节点入队出队一次，每条边最多遍历一次，所以O(V+E)
- SC:
  - 邻接表 adj： 存储 V 个节点和最多 E 条边 → O(V+E)
  - 入度表 indegree： O(V)
  - 队列 q + 结果 res： 最多存放 V 个节点 → O(V)

分析：
- 这是一道 图论 + 拓扑排序（Topological Sort） 问题： 
  1. 构建有向图：
     - 节点：外星语言的每个字符。
     - 边：从单词顺序中推出“哪个字符应在另一个字符之前”。
     - 用邻接表 和 入度表示
  2. 填充邻接表 和 入度
     - 排除invalid情况1: 后词是前词的前缀
     - 只考虑第一个不同的字符，因为后面的字符不影响（并且判断之前没加入过其邻接表的，再加入）
  3. 执行拓扑排序：
     - 把所有入度为0的字符加入队列，弹出入度为0的节点，一直这样循环，直至队列为空
     - 若图中存在环（矛盾），则返回 ""。
     - 否则返回拓扑排序结果（即字符顺序）。





## 2. dfs
```python
class Solution:
    def foreignDictionary(self, words: List[str]) -> str:
        adj = {c: set() for w in words for c in w} 

        for i in range((len(words) - 1)):
            w1, w2 = words[i], words[i+1]
            minLen = min(len(w1), len(w2))
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return "" 
            for j in range(minLen):
                if w1[j] != w2[j]:
                    if w2[j] not in adj[w1[j]]:
                        adj[w1[j]].add(w2[j])
                    break
        
        visited = {}
        res = []

        def dfs(char):
            if char in visited:
                return visited[char]
            
            visited[char] = True

            for nei in adj[char]:
                if dfs(nei):
                    return True
            
            visited[char] = False
            res.append(char)
        
        for char in adj:
            if dfs(char):
                return "" # 有环
        
        res.reverse()
        return "".join(res)
        
        
```

时空复杂度：
- TC: O(N+V+E)
- SC: O(V+E)
- V = unique characters数, E = edges数 and N = sum of lengths of all the strings.