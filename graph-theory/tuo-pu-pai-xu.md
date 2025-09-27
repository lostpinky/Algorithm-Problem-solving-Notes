# 拓扑排序

```python
# deg[i]表示节点i的入度

# 找出入度为0的节点初始化双端队列进行bfs
dq = deque(i for i,d in enumerate(deg) if d == 0)

# 拓扑排序
while dq:
    # 弹出队列左端点
    cur = dq.popleft()
    # 找出当前节点连接的下一层节点，并将其入度减去1
    nxt = path[cur]
    deg[nxt] -= 1
    如果这些节点入度为0，加入双端队列
    if deg[nxt] == 0:
        dq.append(nxt)
```
