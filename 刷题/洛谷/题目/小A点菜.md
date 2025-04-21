---
tags:
  - 动态规划
data: 2025-02-06
---
# [小A点菜](https://www.luogu.com.cn/problem/P1164)

## 题目背景

uim 神犇拿到了 uoi 的 ra（镭牌）后，立刻拉着基友小 A 到了一家……餐馆，很低端的那种。

uim 指着墙上的价目表（太低级了没有菜单），说：“随便点”。

## 题目描述

不过 uim 由于买了一些书，口袋里只剩 $M$ 元 $(M \le 10000)$。

餐馆虽低端，但是菜品种类不少，有 $N$ 种 $(N \le 100)$，第 $i$ 种卖 $a_i$ 元 $(a_i \le 1000)$。由于是很低端的餐馆，所以每种菜只有一份。

小 A 奉行“不把钱吃光不罢休”的原则，所以他点单一定刚好把 uim 身上所有钱花完。他想知道有多少种点菜方法。

由于小 A 肚子太饿，所以最多只能等待 $1$ 秒。

## 输入格式

第一行是两个数字，表示 $N$ 和 $M$。

第二行起 $N$ 个正数 $a_i$（可以有相同的数字，每个数字均在 $1000$ 以内）。

## 输出格式

一个正整数，表示点菜方案数，保证答案的范围在 int 之内。

## 样例 #1

### 样例输入 #1

```
4 4
1 1 2 2
```

### 样例输出 #1

```
3
```

## 提示

2020.8.29，增添一组 hack 数据 by @yummy
## 思路
**01背包问题**

核心代码
```cpp
for(int i = 1;i<=n;++i){  
    for(int j = m;j>=a[i];--j){  
        dp[j] +=dp[j-a[i]];  
    }  
}
```

**当前方案数** `+=` **当前钱数减去菜价所剩钱数 的方案数**

```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
typedef pair<ll, ll> pll;  
  
int main() {  
    ios::sync_with_stdio(false), cin.tie(nullptr);  
    int n, m;  
    cin >> n >> m;  
    vector<int> a(n + 1);  
    for (int i = 1; i <= n; ++i) {  
        cin >> a[i];  
    }  
    vector<int> dp(n + 1);  
    dp[0] = 1;  
    for (int i = 1; i <= n; ++i) {  
        for (int j = m; j >= a[i]; --j) {  
            dp[j] += dp[j - a[i]];  
        }  
    }  
    cout << dp[m];  
    return 0;  
}
```