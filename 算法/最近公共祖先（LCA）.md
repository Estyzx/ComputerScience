---
tags:
  - LCA
  - 学习笔记
data: 2025-03-11
---
# 最近公共祖先（LCA）

最近公共祖先（Lowest Common Ancestor，LCA）是指在有根树中，两个节点的所有公共祖先中深度最大的一个节点。

## 1. 朴素算法

1. 预处理
	使用 `dfs` 函数，预先计算每个节点的 **深度** 和 **父节点**
2. 查询
	1. 先将两个节点调整到相同深度
	2. 同时向上回溯，直到相等
```python
def dfs(u, parent, depth, graph, depths, parents):
    parents[u] = parent
    depths[u] = depth
    for v in graph[u]:
        if v != parent:
            dfs(v, u, depth + 1, graph, depths, parents)


def lca(u, v, depths, parents):
    if depths[u] < depths[v]:
        u, v = v, u
    while depths[u] != depths[v]:
        u = parents[u]
    if u == v:
        return u
    while u != v:
        u = parents[u]
        v = parents[v]
    return u


if __name__ == "__main__":
    # 树的邻接表表示
    tree = {1: [2, 3], 2: [1, 4, 5], 3: [1, 6], 4: [2], 5: [2], 6: [3]}

    # 初始化深度和父节点信息
    depth_info = {}
    parent_info = {}

    # 从根节点 1 开始 DFS 遍历
    dfs(1, -1, 0, tree, depth_info, parent_info)

    # 查询 LCA
    u, v = 4, 6
    ancestor = lca(u, v, depth_info, parent_info)
    print(f"LCA of {u} and {v} is {ancestor}")

```

## 2. 倍增算法

#### 关键公式

假设我们已经预处理了每个节点的 $2^j$ 级祖先，记为 `ancestor[u][j]`，那么：

- `ancestor[u][0]` 表示 u 的直接父节点（20 级祖先）。
    
- `ancestor[u][j]` 表示 u 的 $2^j$ 级祖先。
    
- 递推公式：
    
    `ancestor[u][j]=ancestor[ancestor[u][j−1]][j−1]`
    
    这个公式表示：节点 u 的 $2^j$ 级祖先可以通过其 $2^j$−1 级祖先的 $2^j$−1 级祖先得到。

```python
import math
def dfs(u, parent, depth, graph, depths, ancestors):
    depths[u] = depth
    ancestors[u][0] = parent

    for v in graph[u]:
        if v != parent:
            dfs(v,u,depth+1,graph,depths,ancestors)

def  binary_lifting(graph,root):
    n = len(graph)
    max_log = 20
    depths = [0]*n
    ancestors = [[-1]*max_log for _ in range(n)]

    dfs(root ,-1,0,graph,depths,ancestors)

    for j in range(1,max_log):
        for i in range (n):
            if ancestors[i][j-1] != -1:
                ancestors[i][j] = ancestors[ancestors[i][j-1]][j-1]
    return depths,ancestors

def lca(u,v,depths,ancestors):
    if depths[u]<depths[v]:
        u,v = v,u
    
    for j in range(len(ancestors[0])-1,-1,-1):
        if depths[u] - (1<<j)>=depths[v]:
            u = ancestors[u][j]

    if u == v:
        return u
    
    for j in range(len(ancestors[0])-1,-1,-1):
        if ancestors[u][j]!= ancestors[v][j]:
            u = ancestors[u][j]
            v = ancestors[v][j]
    
    return ancestors[u][0] 

if __name__ == "__main__":
    tree = {
        0: [1, 2],
        1: [0, 3, 4],
        2: [0, 5],
        3: [1],
        4: [1],
        5: [2]
    }

    root = 0
    depths, ancestors = binary_lifting(tree, root)

    u, v = 3, 4
    ancestor = lca(u, v, depths, ancestors)
    print(f"LCA of {u} and {v} is {ancestor}")

```