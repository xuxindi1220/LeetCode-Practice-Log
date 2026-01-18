# 344-Reverse String-E

## 题目描述
![344-Reverse String-E.png](../../imgs/two%20pointers/344-Reverse%20String-E.png)

题意：
- 反转字符串

解法：
- two pointers

## 1. two pointers

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        l, r = 0, len(s) - 1
        while l < r: # 这里用 <= 也可以
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
```

- TC: O(n)
- SC: O(1)
