---
tags:
  - Floyd
data: 2025-03-11
---
# B3647 [【模板】Floyd](https://www.luogu.com.cn/problem/B3647)

## 题目描述

给出一张由 $n$ 个点 $m$ 条边组成的无向图。

求出所有点对 $(i,j)$ 之间的最短路径。

## 输入格式

第一行为两个整数 $n,m$，分别代表点的个数和边的条数。

接下来 $m$ 行，每行三个整数 $u,v,w$，代表 $u,v$ 之间存在一条边权为 $w$ 的边。

## 输出格式

输出 $n$ 行每行 $n$ 个整数。

第 $i$ 行的第 $j$ 个整数代表从 $i$ 到 $j$ 的最短路径。

## 输入输出样例 #1

### 输入 #1

```
4 4
1 2 1
2 3 1
3 4 1
4 1 1
```

### 输出 #1

```
0 1 2 1
1 0 1 2
2 1 0 1
1 2 1 0
```

## 说明/提示

对于 $100\%$ 的数据，$n \le 100$，$m \le 4500$，任意一条边的权值 $w$ 是正整数且 $1 \leqslant w \leqslant 1000$。

**数据中可能存在重边。**

## 题解
[[Floyd-Warshall 算法]]
> 注意 **重边** 问题：`g[u][v] = g[v][u] = min(g[u][v], w)`
> 注意 **数据溢出** 问题：`if (g[i][k] != INT_MAX && g[k][j] != INT_MAX)`

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
typedef pair<double, double> pdd;
typedef pair<string, string> pss;

#define x first
#define y second

const ll mod = 1e9 + 7; // 取模

int main()
{
    ios::sync_with_stdio(false), cin.tie(0);
    int n, m;
    cin >> n >> m;
    vector<vector<int>> g(n + 1, vector<int>(n + 1, INT_MAX));
    for (int i = 1; i <= m; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        g[u][v] = g[v][u] = min(g[u][v], w);
    }
    for (int i = 1; i <= n; ++i)
    {
        g[i][i] = 0;
    }
    for (int k = 1; k <= n; ++k)
    {
        for (int i = 1; i <= n; ++i)
        {
            for (int j = 1; j <= n; ++j)
            {
                if (g[i][k] != INT_MAX && g[k][j] != INT_MAX)

                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                g[j][i] = g[i][j];
            }
        }
    }
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= n; ++j)
        {
            cout << g[i][j] << " ";
        }
        cout << "\n";
    }

    return 0;
}

```