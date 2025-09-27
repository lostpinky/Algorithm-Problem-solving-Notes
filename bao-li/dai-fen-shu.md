# 带分数

[带分数-蓝桥杯](https://www.lanqiao.cn/problems/208/learning/?page=1\&first\_category\_id=1\&sort=students\_count\&second\_category\_id=3\&name=%E5%B8%A6%E5%88%86%E6%95%B0)

1. 设带分数为a + b / c ，对1-9进行全排列，枚举两个拆分点O(n²），C++可以AC，但python运行超时，原因未知
2. python需要优化枚举拆分点的算法，for循环枚举第一个拆分点确定a的值，然后计算n-a的最后一位数与c的最后一位数的乘积，确认b的末尾数字，这样可以避免第二层循环。

```python
from itertools import permutations
n = int(input())
nums = '123456789'
ans = 0    
for p in permutations(nums):
    p = ''.join(p)
    for i in range(1,len(str(n)) + 1):
        a = int(p[0:i])
        # 取排列最后一位数
        b_last = (int(p[-1]) * ((n - a) % 10)) % 10
        if b_last == int(p[-1]) or b_last == 0:
            continue
        j = p.index(str(b_last)) + 1
        if j > i and (n - a) * int(p[j:]) == int(p[i:j]):
            ans += 1
print(ans)
```

3. python也可以先枚举整数部分a，确认b/c的值，然后c从1开始枚举，每次c自增1，b等比例增加，判断是否符合1-9的不重复集合，终止条件是数字个数大于9

```python
n = int(input())
target = '123456789'
ans = 0
for x in range(1, n):
    up = n - x
    down = 1
    s = str(up) + str(down) + str(x)
    while len(s) <= 9:
        ss = set(s)
        if len(s) == 9 and len(ss) == 9 and '0' not in ss:
            ans += 1
        up += n - x
        down += 1
        s = str(up) + str(down) + str(x)

print(ans)
```
