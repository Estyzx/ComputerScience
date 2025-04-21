---
tags:
  - Floyd
data: 2025-03-13
---
# P2910 【USACO08OPEN】 Clear And Present Danger S

## 题目描述

农夫约翰正驾驶一条小艇在牛勒比海上航行．

海上有 $N(1\leq N\leq 100)$ 个岛屿，用 $1$ 到 $N$ 编号．约翰从 $1$ 号小岛出发，最后到达 $N$ 号小岛．

一张藏宝图上说，如果他的路程上经过的小岛依次出现了  $A_1,A_2,\dots ,A_M(2\leq M\leq 10000)$ 这样的序列（不一定相邻），那他最终就能找到古老的宝藏． 但是，由于牛勒比海有海盗出没．约翰知道任意两个岛屿之间的航线上海盗出没的概率，他用一个危险指数 $D_{i,j}(0\leq D_{i,j}\leq 100000)$ 来描述．他希望他的寻宝活动经过的航线危险指数之和最小．那么，在找到宝藏的前提下，这个最小的危险指数是多少呢？

## 输入格式

第一行：两个用空格隔开的正整数 $N$ 和 $M$。

第二到第 $M+1$ 行：第 $i+1$ 行用一个整数 $A_i$ 表示 FJ 必须经过的第 $i$ 个岛屿

第 $M+2$ 到第 $N+M+1$ 行：第 $i+M+1$ 行包含 $N$ 个用空格隔开的非负整数分别表示 $i$ 号小岛到第 $1\dots N$ 号小岛的航线各自的危险指数。保证第 $i$ 个数是 $0$。

## 输出格式

第一行：FJ 在找到宝藏的前提下经过的航线的危险指数之和的最小值。

说明
这组数据中有三个岛屿，藏宝图要求 FJ 按顺序经过四个岛屿：$1$ 号岛屿、$2$ 号岛屿、回到 $1$ 号岛屿、最后到 $3$ 号岛屿。每条航线的危险指数也给出了：航路$(1,2),(2,3),(3,1)$ 和它们的反向路径的危险指数分别是 $5,2,1$。

FJ 可以通过依次经过 $1,3,2,3,1,3$ 号岛屿以 $7$ 的最小总危险指数获得宝藏。这条道路满足了奶牛地图的要求 $(1,2,1,3)$。我们避开了 $1$ 号和 $2$ 号岛屿之间的航线，因为它的危险指数太大了。

注意：测试数据中 $a$ 到 $b$ 的危险指数不一定等于 $b$ 到 $a$ 的危险指数！

Translated by @LJC00125

## 输入输出样例 #1

### 输入 #1

```
3 4 
1 
2 
1 
3 
0 5 1 
5 0 2 
1 2 0
```

### 输出 #1

```
7
```

## Floyd 算法 [[Floyd-Warshall 算法]]

使用 `Floyd`，计算 $a_i$ 到 $a_j$ 的最短距离，再按照题目给定顺序进行计算

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

const ll N = 505;

struct edge
{
    ll v, w, nxt;
} e[N];
ll head[N];
ll cnt = 0;

ll n, m;
ll dis[N][N];

void add_edge(ll u, ll v, ll w)
{
    e[++cnt].v = v;
    e[cnt].w = w;
    e[cnt].nxt = head[u];
    head[u] = cnt;
}

void FF()
{
    for (ll k = 1; k <= n; ++k)
    {
        for (ll i = 1; i <= n; ++i)
        {
            for (ll j = 1; j <= n; ++j)
            {
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
            }
        }
    }
}

int main()
{
    ios::sync_with_stdio(false), cin.tie(0);
    cin >> n >> m;
    vector<int> v(m);
    for (ll i = 0; i < m; ++i)
    {
        cin >> v[i];
    }

    for (ll i = 1; i <= n; ++i)
    {
        for (ll j = 1; j <= n; ++j)
        {
            cin >> dis[i][j];
        }
    }
    ll ans = 0;
    FF();
    for (ll i = 1; i < m; ++i)
    {
        ll x = v[i - 1], y = v[i];
        ans += dis[x][y];
    }
    cout << ans;

    return 0;
}

```