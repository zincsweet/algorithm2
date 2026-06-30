# P1014 [NOIP 1999 普及组] Cantor 表

## 题目描述

现代数学的著名证明之一是 Georg Cantor 证明了有理数是可枚举的。他是用下面这一张表来证明这一命题的：

![](https://cdn.luogu.com.cn/upload/image_hosting/jdjdaf73.png)

我们以 Z 字形给上表的每一项编号。第一项是 $1/1$，然后是 $1/2$，$2/1$，$3/1$，$2/2$，……。

## 输入格式

输入一个整数 $N$（$1 \le N \le 10^7$）。

## 输出格式

输出表中的第 $N$ 项。

## 输入输出样例 #1

### 输入 #1

```
7

```

### 输出 #1

```
1/4
```

## 说明/提示

对于全部测试数据，$1 \le N \le 10^7$。

- 2024-11-18 0:30 数据中加入了样例，放在不计分的子任务 2 中。

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
LL n = 1, sum, N;

int main() {
    cin >> N;
    while ((1 + n) * n / 2 < N) ++n;
    sum = n * (n - 1) / 2;
    LL diff = N - sum;
    if (n % 2 == 0) {
        // 从右往左
        cout << diff << '/' << n - diff + 1;
    }
    else {
        // 从左往右
        cout << n - diff + 1 << '/' << diff;
    }
    return 0;
}
```

