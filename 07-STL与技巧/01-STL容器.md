# STL 容器

## Priority_queue

### unordered_map 的 m[i] 自动创建（超级重要）

如果我找 `unordered_map<int,bool> m` 里面某个值，直接找 `m[i]`，如果 i 本来就不存在，他会自己创建一个吗？

> **超级重要！一定会创建！而且会自动设为 false！**

### map vs unordered_map 选型

**什么时候必须用 map？**
1. 需要按 key 有序遍历
2. 需要找前一个 / 后一个键（lower_bound / upper_bound）
3. 担心哈希冲突导致最坏 O(n)
4. 内存非常紧张

**什么时候必须用 unordered_map？**
1. 数据量大（1e5 以上）
2. 追求极致速度
3. 只做增删查，不需要有序

### 小顶堆

```cpp
// 小顶堆写法
priority_queue<int, vector<int>, greater<int>> q;
```

### map 的默认排序规则

`std::map` 默认是「从小到大」升序排序，排序的依据是**键（key）**，和值（value）无关。

```cpp
// 用 greater<int> 实现降序
map<int, int, greater<int>> mp;
```

## lower_bound / upper_bound

### 容器 → 用哪个 lower_bound？

| 容器类型 | 用哪个 lower_bound？ | 效率 |
|---|---|---|
| vector / 数组 / string（有序） | `lower_bound(v.begin(), v.end(), x)` | 最优 |
| set / map / multiset | `s.lower_bound(x)` | 最优 |

### 三种查找需求

| 需求 | 对应成员函数 | 逻辑 |
|---|---|---|
| 找 ≥ k 的最小数 | `s.lower_bound(k)` | 第一个不小于 k 的位置 |
| 找 > k 的最小数 | `s.upper_bound(k)` | 第一个大于 k 的位置 |
| 找 ≤ k 的最大数 | `s.upper_bound(k)` 配合 `--it` | 第一个大于 k 的位置往前退一位 |

## emplace / emplace_back（直接构造）

```cpp
q.emplace(a[L], L);

// emplace_back 比 push_back 更高效，直接构造 pair
all_points.emplace_back(x, i);
```

## struct node（带构造函数）

```cpp
struct node {
    long long data;
    int id;
    node(long long d, int i) : data(d), id(i) {}
};
```

## reverse 函数

- `reverse` 是 C++ STL 中的通用算法，作用是反转容器中指定区间内的元素顺序，头文件是 `<algorithm>`
- 适用容器：数组、string、vector、deque 等支持随机访问的容器（set/map 不适用，因为它们是有序的，反转无意义）
- 时间复杂度：O(n)（n 是反转区间的元素个数）
- 核心特点：**直接修改原容器，无返回值**

## 其他

- `pair`（记录）
- `tuple<int, int, int>`（记录）
- 左闭右开区间（记录）

## Map 函数：vector 去重复制

```cpp
//Map函数
Map<int,int> cnt;//定义
for (int i = 0; i < n; i++) { // 自动统计 x 出现了多少次
cin >> x;
cnt[x]++; 
}
for (auto p : cnt) { // 输出每种数的数量
cout << "数字: " << p.first << " 次数: " << p.second << endl;
}
cnt.clear(); // 一键清空所有元素
cnt.size();// 统计：map 里有多少个不同的键（元素数量）
cnt.erase(5);// 删除 键 5
//按value从小到大排序
#include <bits/stdc++.h>
using namespace std;
bool cmp(const pair<int, int>& a, const pair<int, int>& b) {
    // 按 value 从小到大
    return a.second < b.second;
}
int main() {
    map<int, int> mp;
    mp[1] = 5;
    mp[2] = 2;
    mp[3] = 8;
    // 转到 vector
    vector<pair<int, int>> v(mp.begin(), mp.end());
    // 按 value 排序
    sort(v.begin(), v.end(), cmp);
    // 输出
    for (auto& p : v) {
        cout << p.first << " " << p.second << endl;
    }
}
```

## multiset 用法

```cpp
multiset<long long> S;//从小到大有序
S.insert(a[j] - a[i - 1]);
auto p = S.find(a[j] - a[i - 1]); // 找到要删除的子数组和的迭代器
erase(p); // 只删除这一个元素，不删同值其他元素
auto p = S.lower_bound(k); // 找到S中第一个≥k的元素
```

## pair 头文件与定义

```cpp
#include <utility>   // pair的头文件
```

## 辗转相除法求 gcd

```cpp
// 求两个数的最大公因数（辗转相除法） 
long long gcd(long long x, long long y) { 
while (y != 0) { 
long long temp = y; 
y = x % y; 
x = temp;
 } 
return x;
 } 
// 求两个数的最小公倍数 
long long lcm(long long x, long long y) { 
return x / gcd(x, y) * y; // 先除后乘避免溢出
 }
int gcd(int a,int b) { return b?gcd(b,a%b):a; }
```

## Priority_queue 自定义堆

```cpp
//自定义顶根堆顺序
struct Node { int val; };
// 比较规则
struct Cmp {
    bool operator()(const Node& a, const Node& b) {
        // return a.val < b.val; → 大顶堆
        return a.val > b.val;   → 小顶堆
    }
};
priority_queue<Node, vector<Node>, Cmp> pq;
```

## stack 单调栈容器

```cpp
//stack单调栈
stack<int> st;//定义单调栈
```

## vector 动态数组

```cpp
//vector动态数组
vector<int> L(n, -1);//数组长度是 n，里面所有元素一开始都被设为 -1
// 创建一个 l1+1 行，l2+1 列的二维vector，初始值全为0
vector<vector<int>> dp(l1 + 1, vector<int>(l2 + 1, 0));
// 复制二维数组
vector<vector<bool>> dst = src;
//查找
vector<int> v={1,1,2,2,3};
sort(v.begin(),v.end()); // 有序即可
binary_search(v.begin(),v.end(),2); // 正常返回true
/*==std::binary_search 强制要求查找区间必须是【连续有序（升序）】，排序是硬性前提，必须做；去重不是强制要求，但强烈建议做（性能优化）。
==静态集合（离线建好，只查不改）：vector + sort + unique + binary_search，常数远小于 set，不会超时。
==动态在线插入 + 查询，无法离线排序：再考虑 set /unordered_set。
==set 红黑树常数太大，n 开到2×105就容易 TLE；
```

## 双端队列 deque

```cpp
//双端队列 deque
// deque 支持 队首弹出、队尾插入、队首插入，都是 O(1)
deque<int> q;//定义
q.push_back(x);//队尾插入
q.push_front(x);//队首插入
q.emplace_back(x);// 和 push_back 一样，效率更高
q.emplace_front(x);// 和 push_front 一样
q.front();//取队首元素
q.back();//取队尾元素
q.pop_front();//删除队首元素
q.pop_back();// 删除队尾元素
q.empty();//判断是否为空
q.size();//存储元素个数
q.clear();// 清空整个队列（多组测试用例必用！
for (auto x : q) cout << x << " ";//输出所有元素
q.insert(q.begin()+pos, x);// 在指定位置插入
q.erase(q.begin()+pos);// 删除指定位置元素q.erase(q.begin() + 下标);
```

## 队列 queue

```cpp
q = queue<int>();
//这行代码的作用是：清空并重置队列 q，让队列恢复成空的、全新的状态。
```
