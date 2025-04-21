---
tags:
  - dijkstra
data: 2025-03-11
---
# dijkstra 算法

Dijkstra算法的核心思想是贪心思想，它通过逐步扩展已知最短路径集合来找到从起点到其他所有顶点的最短路径。

> 注意：在 C++ 中，`priority_queue` 是最大堆

```python
import heapq


def dijkstra(g, start):
    n = len(g)
    dis = [float("inf")] * n
    dis[start] = 0
    pq = [(0, start)]

    while pq:
        cdis, u = heapq.heappop(pq)

        if cdis > dis[u]:
            continue
        for v, w in g[u]:
            ndis = cdis + w
            if ndis < dis[v]:
                dis[v] = ndis
                heapq.heappush(pq, (ndis, v))
    return dis


# 示例图（邻接表表示）
graph = [[(1, 4), (2, 1)], [(2, 2), (3, 5)], [(1, 2), (3, 1), (4, 3)], [(4, 3)], []]

start = 0  # 起点
result = dijkstra(graph, start)
print("最短路径距离：", result)

```
