---
tags:
  - 贪心
data: 2025-02-17
---
# P1803 凌乱的yyy / 线段覆盖

## 题目背景

快 noip 了，yyy 很紧张！

## 题目描述

现在各大 oj 上有 $n$ 个比赛，每个比赛的开始、结束的时间点是知道的。

yyy 认为，参加越多的比赛，noip 就能考的越好（假的）。

所以，他想知道他最多能参加几个比赛。

由于 yyy 是蒟蒻，如果要参加一个比赛必须善始善终，而且不能同时参加 $2$ 个及以上的比赛。

## 输入格式

第一行是一个整数 $n$，接下来 $n$ 行每行是 $2$ 个整数 $a_{i},b_{i}\ (a_{i}<b_{i})$，表示比赛开始、结束的时间。

## 输出格式

一个整数最多参加的比赛数目。

## 输入输出样例 #1

### 输入 #1

```
3
0 2
2 4
1 3
```

### 输出 #1

```
2
```

## 说明/提示

- 对于 $20\%$ 的数据，$n \le 10$；
- 对于 $50\%$ 的数据，$n \le 10^3$；
- 对于 $70\%$ 的数据，$n \le 10^{5}$；
- 对于 $100\%$ 的数据，$1\le n \le 10^{6}$，$0 \le a_{i} < b_{i} \le 10^6$。

## 解题

按照结束时间进行排序

维护`end`，遍历数组，找出`c.start >= end`，计数

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

struct cpet {
    int start, end;
};

signed main() {
    int n;
    cin >> n;
    vector<cpet> v(n);
    
    for (int i = 0; i < n; ++i) {
        cin >> v[i].start >> v[i].end;
    }

    sort(v.begin(), v.end(), [](const cpet &a, const cpet &b) {
        return a.end < b.end;
    });

    int ans = 0, end = 0;
    for (const auto &c : v) {
        if (c.start >= end) {
            ans++;
            end = c.end;
        }
    }

    cout << ans << "\n";
    return 0;
}
```