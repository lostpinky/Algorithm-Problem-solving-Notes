# 面试题 17.06. 2出现的次数

[面试题 17.06. 2出现的次数](https://leetcode.cn/problems/number-of-2s-in-range-lcci/)\


```python
class Solution:
    def numberOf2sInRange(self, n: int) -> int:
        s = str(n)
        @cache
        def f(i, count1, is_limit):
            if i == len(s):
                return count1 
            res = 0
            up = int(s[i]) if is_limit else 9
            for d in range(0, up + 1):
                res += f(i + 1, count1 + int(d == 2), is_limit and d == up)
            return res
        return f(0, 0, True)
```
