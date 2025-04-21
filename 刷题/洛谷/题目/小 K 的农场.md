---
tags:
  - 差分
  - SPFA
data: 2025-03-14
---
# P1993 小 K 的农场

## 题目描述

小 K 在 MC 里面建立很多很多的农场，总共 $n$ 个，以至于他自己都忘记了每个农场中种植作物的具体数量了，他只记得一些含糊的信息（共 $m$ 个），以下列三种形式描述：  
- 农场 $a$ 比农场 $b$ 至少多种植了 $c$ 个单位的作物；
- 农场 $a$ 比农场 $b$ 至多多种植了 $c$ 个单位的作物；
- 农场 $a$ 与农场 $b$ 种植的作物数一样多。  

但是，由于小 K 的记忆有些偏差，所以他想要知道存不存在一种情况，使得农场的种植作物数量与他记忆中的所有信息吻合。

## 输入格式

第一行包括两个整数 $n$ 和 $m$，分别表示农场数目和小 K 记忆中的信息数目。  

接下来 $m$ 行：  
- 如果每行的第一个数是 $1$，接下来有三个整数 $a,b,c$，表示农场 $a$ 比农场 $b$ 至少多种植了 $c$ 个单位的作物；  
- 如果每行的第一个数是 $2$，接下来有三个整数 $a,b,c$，表示农场 $a$ 比农场 $b$ 至多多种植了 $c$ 个单位的作物;  
- 如果每行的第一个数是 $3$，接下来有两个整数 $a,b$，表示农场 $a$ 种植的的数量和 $b$ 一样多。

## 输出格式

如果存在某种情况与小 K 的记忆吻合，输出 `Yes`，否则输出 `No`。

## 输入输出样例 #1

### 输入 #1

```
3 3
3 1 2
1 1 3 1
2 2 3 2
```

### 输出 #1

```
Yes
```

## 说明/提示

对于 $100\%$ 的数据，保证 $1 \le n,m,a,b,c \le 5 \times 10^3$。

## [[SPFA 算法]]

[[差分约束]]
> 注意加入队列的是什么节点
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

const ll N = 100000;

struct edge {
    ll v, w, nxt;
} e[N];

ll head[N];
ll cnt = 0;

void add_edge(int u, int v, int w) {
    e[++cnt].v = v;
    e[cnt].nxt = head[u];
    e[cnt].w = w;
    head[u] = cnt;
}

ll n, m;


int main() {
    ios::sync_with_stdio(false), cin.tie(0);
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int opt;
        cin >> opt;
        switch (opt) {
            case 1: {
                int a, b, c;
                cin >> a >> b >> c;
                add_edge(a, b, -c);
                break;
            }
            case 2: {
                int a, b, c;
                cin >> a >> b >> c;
                add_edge(b, a, c);
                break;
            }
            case 3: {
                int a, b;
                cin >> a >> b;
                add_edge(b, a, 0);
                add_edge(a, b, 0);
                break;
            }
            default: break;
        }
    }
    ll dis[n + 1];
    bool vis[n + 1] = {0};
    ll cnt[n + 1] = {0};
    for (int i = 0; i < n + 1; ++i) {
        dis[i] = INT_MAX;
    }
    for (int i = 1; i <= n; ++i) {
        add_edge(0, i, 0);
    }
    dis[0] = 0;
    vis[0] = 1;
    queue<int> q;
    q.push(0);
    cnt[0]++;
    while (!q.empty()) {
        auto x = q.front();
        q.pop();
        vis[x] = 0;
        for (ll i = head[x]; i > 0; i = e[i].nxt) {
            auto v = e[i].v;
            auto w = e[i].w;
            if (w + dis[x] < dis[v]) {
                dis[v] = w + dis[x];
                if (!vis[v]) {
                    cnt[v]++;
                    if (cnt[v] > n) {
                        cout << "No";
                        return 0;
                    }
                    vis[v] = 1;
                    q.push(v);
                }
            }
        }
    }
    cout << "Yes";
    return 0;
}

```