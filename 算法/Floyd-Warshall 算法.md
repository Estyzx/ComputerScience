---
tags:
  - Floyd
  - 学习笔记
data: 2025-03-11
---
# Floyd-Warshall 算法 #Floyd

通过一个中间节点，找寻最短路径

```python
def floyd(n,edges):
    dp = [[float('inf')]*n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 0
    for(u,v,w) in edges:
        dp[u][v] =  min(dp[u][v],w)
        dp [v][u] = min(dp[u][v],w)
    
    for k in range(1,n):
        for i in range(1,n):
            for j in range(1,n):
                dp[i][j] = min(dp[i][j],dp[i][k]+dp[j][k])
    return dp

edges = [[1,2,1],[2,3,1],[3,4,1],[4,1,1]]
n = 5
dp = floyd(n,edges)

for i in range(1,n):
    for j in range(1,n):
        print(f'{dp[i][j]}',end=" ")
    print()
```