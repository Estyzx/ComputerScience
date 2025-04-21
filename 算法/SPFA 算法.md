---
tags:
  - SPFA
  - 学习笔记
data: 2025-03-12
---
# SPFA 算法

SPFA通过维护一个先进先出的队列来动态更新每个节点的最短路径。

```python
from collections import deque,defaultdict

class SPFA:
    def __init__(self,n):
        self.n = n
        self.graph = defaultdict(list)

    def add_edge(self,u,v,w):
        self.graph[u].append((v,w))
    
    def spfa(self,start):
        dis = [float('inf')] * (self.n + 1)
        vis = [False] * (self.n + 1)
        counts = [0] * (self.n + 1)
        dis[start] = 0
        queue = deque([start])
        vis[start] = True

        while queue:
            cur = queue.popleft()
            vis[cur] = False

            for v,w in self.graph[cur]:
                n = dis[cur] + w
                if n<dis[v]:
                    dis[v] = n
                    if not vis[v]:
                        queue.append(v)
                        vis[v] = True
                        counts[v] +=1
                        if counts[v]>self.n:
                            return None,"图中存在负权环"
        return dis,None
    

if __name__ == "__main__":
    n = 5  # 节点数量
    spfa_solver = SPFA(n)

    # 添加边
    spfa_solver.add_edge(1, 2, 3)
    spfa_solver.add_edge(1, 3, 2)
    spfa_solver.add_edge(2, 3, -1)
    spfa_solver.add_edge(3, 4, 1)
    spfa_solver.add_edge(4, 2, 2)

    start_node = 1  # 起点
    distance, error = spfa_solver.spfa(start_node)

    if error:
        print(error)
    else:
        print(f"从节点{start_node}到各节点的最短距离为：")
        for i in range(1, n):
            print(f"节点{i}: {distance[i]}")


```