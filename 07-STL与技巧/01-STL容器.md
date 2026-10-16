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
