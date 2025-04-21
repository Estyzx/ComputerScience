---
tags:
  - SPFA
data: 2025-03-12
---
# P1828 【USACO3.2】 香甜的黄油 Sweet Butter

## 题目描述

Farmer John 发现了做出全威斯康辛州最甜的黄油的方法：糖。

把糖放在一片牧场上，他知道 $N$ 只奶牛会过来舔它，这样就能做出能卖好价钱的超甜黄油。当然，他将付出额外的费用在奶牛上。

Farmer John 很狡猾。像以前的 Pavlov，他知道他可以训练这些奶牛，让它们在听到铃声时去一个特定的牧场。他打算将糖放在那里然后下午发出铃声，以至他可以在晚上挤奶。

Farmer John 知道每只奶牛都在各自喜欢的牧场（一个牧场不一定只有一头牛）。给出各头牛在的牧场和牧场间的路线，找出使所有牛到达的路程和最短的牧场（他将把糖放在那）。

## 输入格式

第一行包含三个整数 $N,P,C$，分别表示奶牛数、牧场数和牧场间道路数。

第二行到第 $N+1$ 行，每行一个整数，其中第 $i$ 行的整数表示第 $i-1$ 头奶牛所在的牧场号。

第 $N+2$ 行到第 $N+C+1$ 行，每行包含三个整数 $A,B,D$，表示牧场号为 $A$ 和 $B$ 的两个牧场之间有一条长度为 $D$ 的双向道路相连。

## 输出格式

输出一行一个整数，表示奶牛必须行走的最小的距离和。

## 输入输出样例 #1

### 输入 #1

```
3 4 5
2
3
4
1 2 1
1 3 5
2 3 7
2 4 3
3 4 5
```

### 输出 #1

```
8
```

## 说明/提示

**数据范围**

对于所有数据，$1 \le N \le 500$，$2 \le P \le 800$，$1 \le A,B \le P$，$1 \le C \le 1450$，$1 \le D \le 255$。

---

**样例解释**

作图如下：

```cpp
          P2  
P1 @--1--@ C1
         |
         | 
       5  7  3
         |   
         |     C3
       C2 @--5--@
          P3    P4
```

把糖放在4号牧场最优。

## 题解

使用 [[SPFA 算法]]

> 注意使用 `long long`

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
#define int ll

const ll mod = 1e9 + 7; // 取模

const int N = 5000;
struct edge
{
    int v, w, nxt;
} e[N];
int head[N];
int cnt = 0;
int n, p, c;
void add_edge(int u, int v, int w)
{
    e[++cnt].v = v;
    e[cnt].w = w;
    e[cnt].nxt = head[u];
    head[u] = cnt;
}
int cow[N] = {0};

int spfa(int s)
{
    int dis[N];
    int vis[N];
    for (int i = 1; i <= p; ++i)
    {
        dis[i] = 1e9;
        vis[i] = false;
    }
    vis[s] = true;
    dis[s] = 0;
    queue<int> q;
    q.push(s);
    while (!q.empty())
    {
        auto cur = q.front();
        q.pop();
        vis[cur] = false;
        for (int i = head[cur]; i > 0; i = e[i].nxt)
        {
            int n_dis = e[i].w + dis[cur];
            if (n_dis < dis[e[i].v])
            {
                dis[e[i].v] = n_dis;
                if (!vis[e[i].v])
                {
                    q.push(e[i].v);
                    vis[e[i].v] = true;
                }
            }
        }
    }
    int ans = 0;
    for (int i = 1; i <= p; ++i)
    {
        ans += dis[i] * cow[i];
    }
    return ans;
}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0);
    cin>>n>>p>>c;
    for (int i = 0; i < n; ++i)
    {
        int x;
        cin >> x;
        cow[x]++;
    }
    for (int i = 0; i < c; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;
        add_edge(u, v, w);
        add_edge(v, u, w);
    }
    int ans = INT_MAX;
    for (int i = 1; i <= p; ++i)
    {
        auto t = spfa(i);
        if (ans > t)
        {
            ans = t;
        }
    }
    cout << ans;

    return 0;
}

```