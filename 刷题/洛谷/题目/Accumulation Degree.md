---
tags:
  - 树形DP
  - 动态规划
data: 2025-02-11
---
# [P10974 Accumulation Degree](https://www.luogu.com.cn/problem/P10974)

## 题目背景

树木是自然景观的重要组成部分，因为它们可以防止土壤侵蚀，并在其树叶中及其下方提供特定的气候庇护生态系统。研究表明，树木在生产氧气和减少大气中的二氧化碳方面起着重要作用，还可以调节地面温度。它们在园林设计和农业中也是重要的元素，既因为它们的美学吸引力，也因为它们的果园作物（如苹果）。木材也是常见的建筑材料。

## 题目描述

树在许多世界神话中也扮演着亲密的角色。许多学者对树的一些特殊属性感兴趣，例如树的中心、树的计数、树的着色等。树的累积度 $A(x)$ 就是其中的一种属性。

我们这么定义 $A(x)$：

- 树的每一条边都有一个正容量。
- 树中度为 $1$ 的节点被称为终端节点。
- 每条边的流量不能超过其容量。
- $A(x)$ 是节点 $x$ 可以流向其他终端节点的最大流量。

树的累积度是指其节点中最大累积度的值。你的任务是找到给定树的累积度。

## 输入格式

输入的第一行是一个整数 $T$，表示测试用例的数量。每个测试用例的第一行是一个正整数 $n$。接下来的 $n - 1$ 行中的每一行包含三个整数 $x,y,z$，用空格分隔，表示节点 $x$ 和节点 $y$ 之间有一条边，并且这条边的容量为 $z$。节点编号从 $1$ 到 $n$。所有元素都是不超过 $200000$ 的非负整数。可以假设测试数据都是树。

## 输出格式

对于每个测试用例，在单独的一行输出结果。

## 输入输出样例 #1

### 输入 #1

```
1
5
1 2 11
1 4 13
3 4 5
4 5 10
```

### 输出 #1

```
26
```

## 说明/提示

原题中没有提到的数据范围：$T \le 4$，$\sum n \le 2\times 10 ^ 5$。.

## 思路
> 画图
### DFS函数
```cpp
ll dfs(ll u, ll p, vector<vector<pll>> &vp, vector<ll> &dp) {  
    for (auto &[v, w] : vp[u]) {  
        if (v != p) {  
            ll child = dfs(v, u, vp, dp);  
            dp[u] += min(w, child);  
        }  
    }  
    return dp[u] ? dp[u] : LONG_LONG_MAX;  
}  
```
通过遍历当前节点的下一个节点，寻找子节点的流量

### DFS1函数
```cpp
void dfs1(ll u, ll p, vector<vector<pll>> &vp, vector<ll> &dp, ll sum) {  
    ans = max(ans, sum);  
    for (auto &[v, w] : vp[u]) {  
        if (v == p) continue;  
        ll child_sum = min(w, dp[v]);  
        ll new_sum = dp[v] + min(sum - child_sum, w);  
        dfs1(v, u, vp, dp, new_sum);  
    }  
}  
```
通过遍历子节点进行换根
```cpp
ll child_sum = min(w, dp[v]);  
ll new_sum = dp[v] + min(sum - child_sum, w);  
dfs1(v, u, vp, dp, new_sum);  
```
`child_sum`：比较下一节点的流量与当前节点到下一节点容量
`sum - child_sum`：剩余节点的流量

### AC代码（第一道  提高+/省选−）
```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
typedef pair<ll, ll> pll;  
  
ll ans = 0;  
  
ll dfs(ll u, ll p, vector<vector<pll>> &vp, vector<ll> &dp) {  
    for (auto &[v, w] : vp[u]) {  
        if (v != p) {  
            ll child = dfs(v, u, vp, dp);  
            dp[u] += min(w, child);  
        }  
    }  
    return dp[u] ? dp[u] : LONG_LONG_MAX;  
}  
  
void dfs1(ll u, ll p, vector<vector<pll>> &vp, vector<ll> &dp, ll sum) {  
    ans = max(ans, sum);  
    for (auto &[v, w] : vp[u]) {  
        if (v == p) continue;  
        ll child_sum = min(w, dp[v]);  
        ll new_sum = dp[v] + min(sum - child_sum, w);  
        dfs1(v, u, vp, dp, new_sum);  
    }  
}  
  
void solve() {  
    ll n;  
    cin >> n;  
    vector<vector<pll>> vp(n + 1);  
    for (ll i = 1; i < n; ++i) {  
        ll x, y, z;  
        cin >> x >> y >> z;  
        vp[x].push_back({y, z});  
        vp[y].push_back({x, z});  
    }  
    vector<ll> dp(n + 1, 0);  
    dfs(1, -1, vp, dp);  
    ans = dp[1];  
    dfs1(1, -1, vp, dp, ans);  
    cout << ans << endl;  
}  
  
int main() {  
    ios::sync_with_stdio(false), cin.tie(nullptr);  
    ll _ = 1;  
    cin >> _;  
    while (_--) solve();  
    return 0;  
}
```