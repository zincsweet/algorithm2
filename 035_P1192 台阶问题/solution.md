# P1192 台阶问题

## 题目描述

有 $N$ 级台阶，你一开始在底部，每次可以向上迈 $1\sim K$ 级台阶，问到达第 $N$ 级台阶有多少种不同方式。

## 输入格式

两个正整数 $N,K$。

## 输出格式

一个正整数 $ans\pmod{100003}$，为到达第 $N$ 级台阶的不同方式数。

## 输入输出样例 #1

### 输入 #1

```
5 2
```

### 输出 #1

```
8
```

## 说明/提示

- 对于 $20\%$ 的数据，$1\leq N\leq10$，$1\leq K\leq3$；
- 对于 $40\%$ 的数据，$1\leq N\leq1000$；
- 对于 $100\%$ 的数据，$1\leq N\leq10^5$，$1\leq K\leq100$。

```c++
#include <bits/stdc++.h>
using namespace std;
int n, k, arr[100010];

int main() {
    cin >> n >> k;
    arr[0] = arr[1] = 1;
    for (int i = 2; i <= n; ++i) {
        if (i <= k) {
            arr[i] = arr[i-1] * 2 % 100003;
        }
        else {
            arr[i] = (arr[i-1] * 2 - arr[i-1-k]) % 100003;
        }
    }
    cout << (arr[n]+100003)%100003;
    return 0;
}
```

