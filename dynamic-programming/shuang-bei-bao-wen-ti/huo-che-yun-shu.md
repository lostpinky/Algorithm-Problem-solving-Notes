# 火车运输

[蓝桥杯——火车运输](https://www.lanqiao.cn/problems/17117/learning/?isWithAnswer=true)



```python
import itertools
n ,a, b = map(int, input().split())
w = list(map(int, input().split()))
dp = [[0]* (b+1) for _ in range(a+1)]
ans = 0
for i in range(n):
    for j in range(a, -1, -1):
        for k in range(b, -1, -1):
            if j >= w[i]:
                dp[j][k] = max(dp[j][k], dp[j - w[i]][k] + w[i])
            if k >= w[i]:
                dp[j][k] = max(dp[j][k], dp[j][k - w[i]] + w[i])
            # ans = max(dp[j][k], ans)
print(max(itertools.chain.from_iterable(dp)))
```
