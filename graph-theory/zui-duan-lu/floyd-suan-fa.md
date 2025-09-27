# Floyd算法

1. 记忆化搜索

```python
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        w = [[inf] * n for _ in range(n)]
        for u, v, wi in edges:
            w[u][v] = w[v][u] = wi
        
        @cache
        # 表示中间节点小于等于t时u到v的最短路
        def dfs(t, u, v):
            if t == -1:
                return w[u][v]
            return min(dfs(t - 1, u, t) + dfs(t - 1, t, v), dfs(t - 1, u, v))
        
        min_cnt = inf
        ans = -1

        for i in range(n):
            cnt = 0
            for j in range(n):
                if j == i:
                    continue
                if dfs(n - 1, i, j) <= distanceThreshold:
                    cnt += 1
            if cnt <= min_cnt:
                min_cnt = cnt
                ans = i
        return ans
```

2. 递推

```python
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        w = [[inf] * n for _ in range(n)]
        for u, v, wi in edges:
            w[u][v] = w[v][u] = wi
        
        f = w
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    # 此处可以直接覆盖，因为f[i][k]表示i到k的最短路，中间节点必然不包含k，所以f(k, i, k) = f(k - 1, i, k), f[k][j]同理
                    f[i][j] = min(f[i][k] + f[k][j], f[i][j])

        min_cnt = inf
        ans = -1
        for i in range(n):
            cnt = 0
            for j in range(n):
                if j != i and f[i][j] <= distanceThreshold:
                    cnt += 1
            if cnt <= min_cnt:
                min_cnt = cnt
                ans = i
        return ans
```
