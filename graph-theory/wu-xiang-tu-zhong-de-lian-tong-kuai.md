# 无向图中的连通块

[2316. 统计无向图中无法互相到达点对数](https://leetcode.cn/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/)



1. &#x20;遍历edges建图
2. dfs搜索连通块中所有的点
3. 将每个连通块中点的个数两两相乘

在计算答案时如果每次遍历一遍前面所有连通块，则累加部分的时间复杂度为n²（n为连通块个数），超时

```python
class Solution:
    def countPairs(self, n: int, edges: List[List[int]]) -> int:
        paths = [[] for _ in range(n)]

        for i, j in edges:
            paths[i].append(j)
            paths[j].append(i)

        groups = []
        ans = 0
        tmp = []
        def dfs(node):
            tmp.append(node)
            for t in paths[node]:
                if t not in tmp:
                    dfs(t)

        for i in range(n):
            if i not in tmp:
                cur = len(tmp)
                dfs(i)
                cur = len(tmp) - cur
                for g in groups:
                    ans += cur * g
                groups.append(cur)
        return ans
```

维护一个tot存储之前的连通块节点数之和，累加部分的时间复杂度优化为n，代码可以通过

```python
class Solution:
    def countPairs(self, n: int, edges: List[List[int]]) -> int:
        paths = [[] for _ in range(n)]
        for i, j in edges:
            paths[i].append(j)
            paths[j].append(i)
        ans = 0
        tmp = set()
        def dfs(node):
            tmp.add(node)
            for t in paths[node]:
                if t not in tmp:
                    dfs(t)
        tot = 0
        for i in range(n):
            if i not in tmp:
                cur = len(tmp)
                dfs(i)
                cur = len(tmp) - cur
                ans += tot * cur
                tot += cur
        return ans
```

可继续优化，用一个数组vis记录节点状态（访问或未访问），dfs直接返回连通块大小

```python
class Solution:
    def countPairs(self, n: int, edges: List[List[int]]) -> int:
        paths = [[] for _ in range(n)]
        for i, j in edges:
            paths[i].append(j)
            paths[j].append(i)
        ans = 0
        vis = [False] * n
        # 计算node节点连通的所有未访问节点个数（包括node节点）
        def dfs(node):
            cur = 1
            vis[node] = True
            for t in paths[node]:
                if not vis[t]:
                    cur += dfs(t)
            return cur
        tot = 0
        for i in range(n):
            if not vis[i]:
                cur = dfs(i)
                ans += tot * cur
                tot += cur
        return ans
```
