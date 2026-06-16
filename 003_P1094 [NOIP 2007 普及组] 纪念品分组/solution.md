# P1094 [NOIP 2007 普及组] 纪念品分组

## 题目背景

NOIP2007 普及组 T2

## 题目描述

元旦快到了，校学生会让乐乐负责新年晚会的纪念品发放工作。为使得参加晚会的同学所获得的纪念品价值相对均衡，他要把购来的纪念品根据价格进行分组，但每组最多只能包括两件纪念品，并且每组纪念品的价格之和不能超过一个给定的整数。为了保证在尽量短的时间内发完所有纪念品，乐乐希望分组的数目最少。

你的任务是写一个程序，找出所有分组方案中分组数最少的一种，输出最少的分组数目。

## 输入格式

共 $n+2$ 行：

第一行包括一个整数 $w$，为每组纪念品价格之和的上限。

第二行为一个整数 $n$，表示购来的纪念品的总件数 $G$。

第 $3\sim n+2$ 行每行包含一个正整数 $P_i$ 表示所对应纪念品的价格。

## 输出格式

一个整数，即最少的分组数目。

## 输入输出样例 #1

### 输入 #1

```
100 
9 
90 
20 
20 
30 
50 
60 
70 
80 
90

```

### 输出 #1

```
6

```

## 说明/提示

$50\%$ 的数据满足：$1\le n\le15$。

$100\%$ 的数据满足：$1\le n\le3\times10^4$，$80\le w\le200$，$5 \le  P_i  \le  w$。

```c++
#include <bits/stdc++.h>
using namespace std;
int main() {
    std::ios::sync_with_stdio(false);
    cout.tie(nullptr);
    cin.tie(nullptr);
    int mx, cnt;
    cin >> mx;
    cin >> cnt;
    vector<int> a;
    while (cnt--) {
        int x;
        cin >> x;
        a.emplace_back(x);
    }
    sort(a.begin(), a.end());
    int left = 0, right = a.size() - 1, ret = 0;
    while (left < right) {
        int sum =a[left] + a[right];
        if (sum > mx) {
            --right;
            ++ret;
        }
        else {
            ++left;
            --right;
            ++ret;
        }
    }
    if (right == left) ++ret;
    cout << ret;
    return 0;
}
```

具体建议：

------

### 1. 使用更直观的变量名

原代码中 `mx`、`cnt`、`ret` 含义不够明确，建议改为：

- `limit`（上限）
- `n`（物品数量）
- `ans`（最少分组数）

这样代码自解释性更强。

------

### 2. 预分配 `vector` 内存

在读入价格前，可以调用 `a.reserve(n)`，避免动态扩容带来的多次内存分配，提升效率。

------

### 3. 处理 `n == 0` 的边界情况

虽然题目大概率不会出现 `n = 0`，但严谨的代码应能正确处理。当前代码在 `n=0` 时能输出 `0`，没有错误，但可以在开始显式判断并提前返回。

------

### 4. 简化最后单个物品的处理逻辑

当前循环结束后用 `if(left == right) ++ret` 来处理剩余一个物品。可以改为 `while(left <= right)` 的循环结构，使逻辑更统一：

cpp

```
while (left <= right) {
    if (left == right) {        // 只剩一个物品
        ++ans;
        break;
    }
    if (a[left] + a[right] <= limit) {
        ++left;
    }
    --right;
    ++ans;
}
```



这样就不需要循环结束后的额外判断。

------

### 5. 添加必要的注释

虽然是竞赛代码，但简要说明贪心策略（“最小和最大配对，不行则最大单独一组”）有助于他人（或未来的你）理解。

------

### 改进后的代码示例

cpp

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int limit, n;
    cin >> limit >> n;

    vector<int> prices;
    prices.reserve(n);          // 预分配空间

    for (int i = 0; i < n; ++i) {
        int x;
        cin >> x;
        prices.push_back(x);
    }

    sort(prices.begin(), prices.end());

    int left = 0, right = n - 1, ans = 0;

    while (left <= right) {
        if (left == right) {    // 仅剩一个物品
            ++ans;
            break;
        }
        if (prices[left] + prices[right] <= limit) {
            ++left;             // 配对成功，左指针右移
        }
        // 无论是否配对，右指针都左移（未配对时该物品单独成组）
        --right;
        ++ans;
    }

    cout << ans << '\n';
    return 0;
}
```



------

### 你的原代码仍然正确，以上建议只是锦上添

- **优点**：逻辑清晰、时空复杂度均为 O(n log n)，符合题目要求。
- **可以保留**：你使用的 `while (left < right)` + 最后处理单个物品的写法也很常见，没问题。

继续保持这种“先排序再双指针”的贪心思维，这对许多区间/配对问题都很有用！