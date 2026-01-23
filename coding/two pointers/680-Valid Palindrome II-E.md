# 680-Valid Palindrome II-E

## 题目描述
![680-q-1.png](../../imgs/two%20pointers/680-q-1.png)
![680-q-2.png](../../imgs/two%20pointers/680-q-2.png)

题意：
- 给定一个字符串，判断在最多删除一个字符的情况下，是否为回文串

解法：
- Two Pointers

## 1. Two Pointers
```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        def is_palindrome(l, r):
            while l < r:
                if s[l] != s[r]:
                    return False
                l += 1
                r -= 1
            return True

        l, r = 0, len(s) - 1
        while l < r:
            if s[l] != s[r]:
                return (is_palindrome(l + 1, r) or
                        is_palindrome(l, r - 1))
            l += 1
            r -= 1

        return True
```

- TC: O(n)
- SC: O(1)

分析：
- 在遇到不相同字符时，跳过左或右指针所指元素，判断剩下的子串是否为回文
