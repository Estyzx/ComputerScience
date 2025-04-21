---
tags:
  - 动态规划
data: 2025-02-05
---
# [【IOI 1994】 数字三角形 Number Triangles](https://www.luogu.com.cn/problem/P1216)

## 题目描述

观察下面的数字金字塔。


写一个程序来查找从最高点到底部任意处结束的路径，使路径经过数字的和最大。每一步可以走到左下方的点也可以到达右下方的点。

![](https://cdn.luogu.com.cn/upload/image_hosting/95pzs0ne.png)

在上面的样例中，从 $7 \to 3 \to 8 \to 7 \to 5$ 的路径产生了最大权值。

## 输入格式

第一个行一个正整数 $r$ ,表示行的数目。

后面每行为这个数字金字塔特定行包含的整数。

## 输出格式

单独的一行,包含那个可能得到的最大的和。

## 样例 #1

### 样例输入 #1

```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

### 样例输出 #1

```
30
```

## 提示

【数据范围】  
对于 $100\%$ 的数据，$1\le r \le 1000$，所有输入在 $[0,100]$ 范围内。

题目翻译来自NOCOW。

USACO Training Section 1.5

IOI1994 Day1T1

## 思路

构建`dp`数组`vector dp (n+1,vector(n+1,0))` ，使用方程求出路径和`dp [i][j] = v[i][j] + max(dp[i-1][j-1],dp[i-1][j])`

```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
typedef pair<ll, ll> pll;  
  
int main() {  
    ios::sync_with_stdio(false), cin.tie(nullptr);  
    int n;  
    cin >> n;  
    vector v(n + 1, vector(n + 1, 0));  
    vector dp(n + 1, vector(n + 1, 0));  
    for (int i = 1; i <= n; ++i) {  
        for (int j = 1; j <= i; ++j) {  
            cin >> v[i][j];  
            dp[i][j] = v[i][j] + max(dp[i - 1][j - 1], dp[i - 1][j]);  
        }  
    }  
    cout << *ranges::max_element(dp[n]);  
    return 0;  
}
```
