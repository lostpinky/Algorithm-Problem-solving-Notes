# 通关——蓝桥1024双周赛

[通关——蓝桥1024双周赛](https://www.lanqiao.cn/problems/5889/learning/?contest\_id=145)

用优先队列维护一个可以访问到的节点列表，每次弹出经验限制最小的节点，判断是否可以通过，如果通过则将该节点的所有子节点加入优先队列，否则结束循环

```python
import os
import sys
from queue import PriorityQueue

n, p = map(int, input().split())
path = [[] for _ in range(n + 1)]
sk = [() for _ in range(n + 1)]


for i in range(1, n+1):
    f, s, k = map(int, input().split())
    path[f].append(i)
    sk[i] = (s, k)
    
ans = 0
pq = PriorityQueue()
pq.put((sk[1][1], (sk[1][0], 1)))
while pq:
    cur = pq.get()
    idx = cur[1][1]
    if cur[0] <= p:
        ans  += 1
        p += cur[1][0]
        for nxt in path[idx]:
            pq.put((sk[nxt][1], (sk[nxt][0], nxt)))
    else:
        break
print(ans)
```
