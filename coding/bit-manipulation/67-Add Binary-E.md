# 67-Add Binary-E

## 题目描述
![67-q.png](../../imgs/bit-manipulation/67-q.png)

题意：
- 给定两个二进制字符串a和b，返回它们的和（用二进制表示）
- 输入字符串只包含'0'和'1'字符

解法：
- Iteration

## 1. Iteration
```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        res = []
        carry = 0

        i, j = len(a) - 1, len(b) - 1
        while i >= 0 or j >= 0 or carry > 0:
            digitA = int(a[i]) if i >= 0 else 0
            digitB = int(b[j]) if j >= 0 else 0

            total = digitA + digitB + carry
            res.append(total % 2)
            carry = total // 2

            i -= 1
            j -= 1

        res.reverse()
        return ''.join(map(str, res)) # map(str, res):将可迭代对象中的每个元素转换为字符串类型
```

- TC: O(max(m, n))
- SC: O(max(m, n))
- m = len(a), n = len(b)
