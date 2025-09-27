# 神奇数

[神奇数——蓝桥1024双周赛](https://www.lanqiao.cn/problems/5891/learning/?contest\_id=145)

1. f(i, pre\_mod, mod, is\_limit)表示i位及此后数位的合法方案数，mod表示最后一位数字，pre\_mod表示前缀和对mod的模， is\_limit表示当前是否受到n的限制。
2. 需要对最后一位数字枚举，时间复杂度 = 状态数 \* 状态转移次数 = （200 \* 10 \* 9 \* 2）\* 10 ≈ 4 \* 10^5（数据范围为10 - 10^200）

&#x20;      若不枚举，直接使用前缀和作为状态，或前缀和mod 9！作为状态，时间复杂度超过（200 \* 362880 \* 2）\* 10 ≈ 1.45 \* 10^9，无法通过

3. 由于题目给出的是一段区间，需要求解两次，所以运行完第一次必须调用f.cache\_clear()清空缓存。

```python
import os
import sys
from functools import lru_cache
l, r = input(),input()
s = ''
M = 998244353
mod = 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1

@lru_cache()
def f(i, pre_mod, mod, is_limit):
    up = int(s[i]) if is_limit else 9
    res = 0
    if i == len(s) - 1:
        if pre_mod  == 0 and mod <= up:
            res += 1
    else:
        for d in range(0, up + 1):
            res += f(i + 1, (pre_mod + d) % mod, mod, is_limit and d == up)
    return res

s =  l
res_l = sum([f(0, 0, i, True) for i in range(1,10)])
# 此处必须清除记忆化缓存，否则第二次调用会直接返回第一次的结果
f.cache_clear()

s = r
res_r = sum([f(0, 0, i, True) for i in range(1,10)])

is_l = 0
if l[-1] != '0':
    for c in l[:-1]:
        is_l = is_l + int(c)
    is_l = 1 if (is_l % int(l[-1])) == 0 else 0

# print(res_l, res_r, is_l)
print((res_r - res_l + int(is_l)) % M)
```

