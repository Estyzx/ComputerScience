---
tags:
  - 状压DP
  - 最短路
  - SPFA
data: 2025-03-03
---
# P2622 [关灯问题II](https://www.luogu.com.cn/problem/P2622)

## 题目描述

现有 $n$ 盏灯，以及 $m$ 个按钮。每个按钮可以同时控制这 $n$ 盏灯——按下了第 $i$ 个按钮，对于所有的灯都有一个效果。按下 $i$ 按钮对于第 $j$ 盏灯，是下面 $3$ 中效果之一：如果 $a_{i,j}$ 为 $1$，那么当这盏灯开了的时候，把它关上，否则不管；如果为 $-1$ 的话，如果这盏灯是关的，那么把它打开，否则也不管；如果是 $0$，无论这灯是否开，都不管。

现在这些灯都是开的，给出所有开关对所有灯的控制效果，求问最少要按几下按钮才能全部关掉。

## 输入格式

前两行两个数，$n, m$。

接下来 $m$ 行，每行 $n$ 个数 $,a_{i, j}$ 表示第 $i$ 个开关对第 $j$ 个灯的效果。

## 输出格式

一个整数，表示最少按按钮次数。如果没有任何办法使其全部关闭，输出 $-1$。

## 输入输出样例 #1

### 输入 #1

```
3
2
1 0 1
-1 1 0
```

### 输出 #1

```
2
```

## 说明/提示

### 数据范围及约定

- 对于 $20\%$ 数据，输出无解可以得分。
- 对于 $20\%$ 数据，$n \le 5$。
- 对于 $20\%$ 数据，$m \le 20$。

上面的数据点可能会重叠。

对于 $100\%$ 数据 $n \le 10,m \le 100$。

## SPFA 算法[[SPFA 算法]]

与 [[【蓝桥杯 2019 省 A】 糖果]] 相似

> 通过队列将每一步存入，如果队列为空则返回不可达到


```CPP
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
typedef pair<double, double> pdd;
typedef pair<string, string> pss;

#define x first
#define y second
int n, m;
int dis[1 << 20];

struct node
{
    int s, step;
};
int spfa(vector<vector<int>> &v)
{
    queue<node> q;
    q.push({(1 << n) - 1, 0});
    bool vis[1 << n];
    memset(vis, false, sizeof vis);
    vis[(1 << n) - 1] = true;
    while (!q.empty())
    {
        auto [s, step] = q.front();
        q.pop();
        if (s == 0)
        {
            return step;
        }
        for (int i = 1; i <= m; ++i)
        {
            int s1 = s;
            for (int j = 1; j <= n; ++j)
            {
                if (v[i][j] == 1 && (s1 & (1 << (j - 1))))
                {
                    s1 ^= (1 << (j - 1));
                }
                else if (v[i][j] == -1 && !(s1 & (1 << (j - 1))))
                {
                    s1 |= (1 << (j - 1));
                }
            }
            if (!vis[s1])
            {
                vis[s1] = true;
                q.push({s1, step + 1});
            }
        }
    }
    return -1;
}

int main()
{
    ios::sync_with_stdio(false), cin.tie(0);

    cin >> n >> m;
    vector<vector<int>> v(m + 1, vector<int>(n + 1));
    for (int i = 1; i <= m; ++i)
    {
        for (int j = 1; j <= n; ++j)
        {
            cin >> v[i][j];
        }
    }
    cout << spfa(v);
    return 0;
}

```