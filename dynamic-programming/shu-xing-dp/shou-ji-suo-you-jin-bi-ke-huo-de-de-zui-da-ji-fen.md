# 收集所有金币可获得的最大积分

[100108. 收集所有金币可获得的最大积分](https://leetcode.cn/problems/maximum-points-after-collecting-coins-from-all-nodes/)\

记忆化搜索：dfs(i, pre, fa)表示以 i 为根节点的子树，上层节点使用pre次第二种操作，fa为父节点，该情况下的最大积分。

时间复杂度 = 状态数 \* 转移次数

易知，状态数 = n \* pre取值区间，转移次数总数 = 边数\* 2 = （n - 1） \* 2， 则平均每个节点的转移次数为O(1)

因此，当不限制pre取值时，时间复杂度为O（n²），对pre进行剪枝，删去计算结果为0的情况后，时间复杂度O（n \* log(m)）(m为coins最大值)

```python
class Solution:
    def maximumPoints(self, edges: List[List[int]], coins: List[int], k: int) -> int:
        n = len(edges) + 1
        path = [[] for _ in range(n)]
        for u, v in edges:
            path[u].append(v)
            path[v].append(u)
        
        @cache
        def f(c,time):
            if time == 0:
                return c
            return floor(f(c,time - 1) / 2)
        @cache
        def dfs(i,pre,fa):
            coin = f(coins[i], pre)
            ans1 = coin - k
            ans2 = floor(coin / 2)
            for u in path[i]:
                if u != fa:
                    ans1 += dfs(u, pre, i)
                    # 此处必须进行剪枝，否则会超时
                    if pre < 13:
                        ans2 += dfs(u, pre + 1, i)
            return max(ans1, ans2)
        return dfs(0, 0, -1)
```
