# 树的直径

[串门——蓝桥算法1024双周赛](https://www.lanqiao.cn/problems/5890/learning/?contest\_id=145)

**树的等价定义：**

* G是树（连通无回路）
* G中任何两个顶点之间有唯一的路径
* G中无回路，且m=n-1（m为边数，n为点数）
* G是连通的，且m=n-1
* G是连通的，且任意一条边都是桥
* G没有回路，且在G中任意两个顶点间新填一条边，新图中存在唯一的含新边的一个圈

**解题思路：**

假设串门起点和终点分别为m和n，由树的定义2知，m到n有唯一路径，则最短串门路径长度 =  m到n的路径长度 + 其他边权重和 \* 2  = 所有边权重和 \* 2 - m 到 n 的路径长度。当m到n的路径长度最大时，串门路径长度最小。

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>绿色边经过1次，紫色边经过两次</p></figcaption></figure>

于是问题转化为求树中的最长路径，也即树的直径。

**树的直径：**

**定义：** 树中最远的两个节点之间的距离被称为树的直径,连接这两个点的路径被称为树的最长链。

**两种解法：**

1. 两次dfs或bfs

从任意节点出发进行dfs或bfs，由反证法易得，最远节点一定是直径的两端点之一，再从该端点出发进行dfs或bfs，即得到树的直径

**dfs方法**

```python
import os
import sys
sys.setrecursionlimit(10**5 + 1)

n = int(input())
path = [[] for _ in range(n + 1)]
ans = 0

for i in range(1, n):
    v, u, w = map(int, input().split())
    ans += w * 2
    path[v].append((u, w))
    path[u].append((v, w))
state = [0,0]
vis = [False] * (n + 1)

def dfs(node,length):
    if length > state[1]:
        state[1] = length
        state[0] = node
    vis[node] = True
    for u, w in path[node]:
        if not vis[u]:
            dfs(u,length + w)
dfs(1, 0)
vis = [False] * (n + 1)
dfs(state[0], 0)
print(ans - state[1])
```

**bfs方法**

```python
import os
import sys
from collections import deque

n = int(input())
path = [[] for _ in range(n + 1)]
ans = 0
for i in range(1, n):
    v, u, w = map(int, input().split())
    ans += w * 2
    path[v].append((u, w))
    path[u].append((v, w))

def bfs(node):
    dis = [-1] * (n + 1)
    dis[node] = 0
    dq = deque([node])
    while dq:
        cur = dq.popleft()
        for u, w in path[cur]:
            if dis[u] == -1:
                dis[u] = dis[cur] + w
                dq.append(u)
    long_path = max(dis)
    long_node = dis.index(long_path)
    return long_node, long_path
long_node, _ = bfs(1)
_, lp = bfs(long_node)
print(ans - lp)
```

2. 树形dp

```python
import os
import sys
sys.setrecursionlimit(10**5 + 1)

n = int(input())
path = [[] for _ in range(n + 1)]
ans = 0
for i in range(1, n):
    v, u, w = map(int, input().split())
    ans += w * 2
    path[v].append((u, w))
    path[u].append((v, w))

dp = [0] * (n + 1) #  dp[x]表示从结点x出发，往以x作为根的子树走，能够到达的最远距离
d = 0 # 树的直径
# 以任意u为根dfs
def tree_dp(u, fa):
    global d
    for to, w in path[u]:
        if to != fa:
            tree_dp(to, u)
            # 树的直径一定是某个子树根节点（或自身根节点）出发的最长距离和次最长距离之和
            d = max(d, dp[u] + dp[to] + w) 
            dp[u] = max(dp[u], dp[to] + w)
            
tree_dp(1, -1)
print(ans - d)
```

