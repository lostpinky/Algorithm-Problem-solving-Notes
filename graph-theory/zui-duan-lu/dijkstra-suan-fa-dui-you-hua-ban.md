# Dijkstra算法（堆优化版）

Dijkstra算法用于查找图中从一个起始节点到所有其他节点的最短路径。该算法的工作原理如下：

* 初始化距离数组，将起始节点的距离设置为0，将所有其他节点的距离设置为无穷大。
* 选择当前距离数组中距离最小的节点，将其标记为已访问。
* 更新与该节点相邻的节点的距离，如果通过当前节点到达相邻节点的距离小于已知距离，则更新距离数组。
* 重复上述步骤，直到所有节点都被访问。

Dijkstra算法的优点是对于没有负权边的图非常有效，但不能处理负权边。

```python
import math
import heapq


def dijkstra(graph, start, end):
    distance = {node: math.inf for node in graph}
    parent = {}
    distance[start] = 0
    pq = [(0, start)]
    while pq:
        dist, cur_node = heapq.heappop(pq)

        # 在不使用堆优化的dijkstra算法中，已确认最短路径的值会被标记，每次只在未标记节点中寻找，因此不会有节点被重复加入最优路径集合；
        # 但在堆优化的dijkstra算法中，每次更新临近节点距离时，distance值被直接修改，而(dist, node)元组则直接加入队列,因此队列中可能存在node相同的多条记录
        # 若优先队列中记录的距离若大于距离字典中的值，说明该节点与distance值相等的最小值已经被计算过，因此直接跳过
        if dist > distance[cur_node]:
            continue

        if cur_node == end:
            paths = [cur_node]
            while cur_node != start:
                last = parent[cur_node]
                paths.append(last)
                cur_node = last
            paths.reverse()
            return distance[end], paths

        for neighbor, weight in graph[cur_node].items():
            new_distance = weight + distance[cur_node]
            if new_distance < distance[neighbor]:
                distance[neighbor] = new_distance
                parent[neighbor] = cur_node
                heapq.heappush(pq, (new_distance, neighbor))

if __name__ == '__main__':
    # 无向图的邻接表表示
    graph = {
        'A': {'B': 2, 'C': 4},
        'B': {'A': 2, 'C': 1, 'D': 7},
        'C': {'A': 4, 'B': 1, 'D': 3},
        'D': {'B': 7, 'C': 3}
    }

    start_node = 'A'
    end_node = 'D'
    dist, path = dijkstra(graph, start_node, end_node)
    print(f"Shortest distance from {start_node} to {end_node}: {dist}")
    print(f"Shortest path: {' -> '.join(path)}")
```
