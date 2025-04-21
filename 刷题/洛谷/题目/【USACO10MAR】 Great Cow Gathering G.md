---
tags:
  - 动态规划
  - 树形DP
data: 2025-02-10
---
# [【USACO10MAR】 Great Cow Gathering G](https://www.luogu.com.cn/problem/P2986)

## 题目描述

Bessie 正在计划一年一度的奶牛大集会，来自全国各地的奶牛将来参加这一次集会。当然，她会选择最方便的地点来举办这次集会。

每个奶牛居住在 $N$ 个农场中的一个，这些农场由 $N-1$ 条道路连接，并且从任意一个农场都能够到达另外一个农场。道路 $i$ 连接农场 $A_i$ 和 $B_i$，长度为 $L_i$。集会可以在 $N$ 个农场中的任意一个举行。另外，每个牛棚中居住着 $C_i$ 只奶牛。

在选择集会的地点的时候，Bessie 希望最大化方便的程度（也就是最小化不方便程度）。比如选择第 $X$ 个农场作为集会地点，它的不方便程度是其它牛棚中每只奶牛去参加集会所走的路程之和（比如，农场 $i$ 到达农场 $X$ 的距离是 $20$，那么总路程就是 $C_i\times 20$）。帮助 Bessie 找出最方便的地点来举行大集会。

## 输入格式

第一行一个整数 $N$ 。

第二到 $N+1$ 行：第 $i+1$ 行有一个整数 $C_i$。

第 $N+2$ 行到 $2N$ 行：第 $i+N+1$ 行为 $3$ 个整数：$A_i,B_i$ 和 $L_i$。

## 输出格式

一行一个整数，表示最小的不方便值。

## 样例 #1

### 样例输入 #1

```
5 
1 
1 
0 
0 
2 
1 3 1 
2 3 2 
3 4 3 
4 5 3
```

### 样例输出 #1

```
15
```

## 提示

$1\leq N\leq 10^5$，$1\leq A_i\leq B_i\leq N$，$0 \leq C_i,L_i \leq 10^3$。

## 思路

使用两次`dfs`
- 第一个`dfs`：查找以`1`为根的不方便值，并记录到`i`节点的奶牛数
- 第二个`dfs`：换根`ans[v] = dis + (cows[1] - 2 * cows[v]) * w;`
	- 以`v`为节点的奶牛不方便值减少`cows[v]*w`
	- 以上一个节点的奶牛不方便值增加`(cows[1]-cows[v])*w`

### AC代码

> 注意数据范围，使用`long long`

```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
typedef pair<ll, ll> pll;  
typedef pair<int, int> pii;  
  
int main() {  
    ios::sync_with_stdio(false), cin.tie(nullptr);  
    ll n;  
    cin >> n;  
    vector<ll> C(n + 1);  
    for (ll i = 1; i <= n; ++i) {  
        cin >> C[i];  
    }  
    vector<vector<pll> > g(n + 1);  
    for (ll i = 1; i < n; ++i) {  
        int a, b, v;  
        cin >> a >> b >> v;  
        g[a].push_back({b, v});  
        g[b].push_back({a, v});  
    }  
    vector<ll> cows(n + 1, 0);  
    ll total_dis = 0;  
    std::function<void(ll, ll, ll)> dfs = [&](ll u, ll p, ll dis) {  
        cows[u] += C[u];  
        for (auto [v, w]: g[u]) {  
            if (v == p) continue;  
            dfs(v, u, dis + w);  
            cows[u] += cows[v];  
        }  
        total_dis += C[u] * dis;  
    };  
    dfs(1, 0, 0);  
    vector<ll> ans(n + 1);  
    ans[1] = total_dis;  
    ll dis_ans = LLONG_MAX;  
    std::function<void(ll, ll, ll)> dfs1 = [&](ll u, ll p, ll dis) {  
        dis_ans = min(dis_ans, dis);  
        for (auto [v, w]: g[u]) {  
            if (v == p) continue;  
            ans[v] = dis + (cows[1] - 2 * cows[v]) * w;  
            dfs1(v, u, ans[v]);  
        }  
    };  
    dfs1(1, 0, total_dis);  
    cout << dis_ans << endl;  
    return 0;  
}
```