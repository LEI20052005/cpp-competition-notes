# Kruskal 重构树

## 核心结论

1. **Kruskal 重构树 = 最小生成树变形**，把边变成新节点，权值就是原边权
2. **两点 LCA 的权值 = 两点路径上最大边权的最小值**（最常用功能）

## 适用问题

- 求图中两点之间，所有可行路径里，**最大边权的最小值**
- 跳跃并查集：右小优先，find 找空闲；用完 pos 跳 pos+1，永远小数指向大

## Kruskal 重构树模板

```cpp
#include <bits/stdc++.h>
using namespace std;

// ===================== 常量定义（全部纯数字，无任何二进制运算） =====================
const int MAXN = 200010;    // 原图最大节点数量
const int MAXM = 400010;    // 原图最大边数量
const int LOG = 20;         // 倍增跳步层数（2^20 足够覆盖所有节点）

// ===================== 并查集数组 =====================
// Kruskal 重构树总结点数量 = 2*MAXN，所以开到 400020
int fa[400020];

// 并查集查找函数 + 路径压缩（加速查找）
int find(int x)
{
    // 如果 x 的父节点是自己，说明是根节点，直接返回
    // 否则递归查找根，并把 x 直接连到根上（路径压缩）
    return fa[x] == x ? x : fa[x] = find(fa[x]);
}

// ===================== 边的结构体：存储原图的一条边 =====================
struct Edge
{
    int u, v, w;  // u = 起点, v = 终点, w = 边权

    // 重载小于号：让 sort 按边权从小到大排序
    bool operator<(const Edge& other) const
    {
        return w < other.w;
    }
} edge[MAXM];  // 存储所有边

// ===================== Kruskal 重构树相关数组 =====================
vector<int> tree[400020];  // 重构树的邻接表
int val[400020];           // 重构树【新节点】的权值 = 原图边权
int node_cnt;              // 重构树当前用到的节点编号

// ===================== 倍增 LCA 相关数组 =====================
int up[LOG][400020];  // up[j][u] 表示 u 向上跳 2^j 步到达的节点
int dep[400020];      // dep[u] 表示节点 u 在重构树中的深度

// ===================== DFS：预处理每个节点的深度和倍增祖先 =====================
void dfs(int u, int father)
{
    up[0][u] = father;         // u 向上跳 1 步(2^0) 就是父节点
    dep[u] = dep[father] + 1;  // 当前节点深度 = 父节点深度 + 1

    // 预处理倍增表：2^j 步 = 跳两次 2^(j-1) 步
    for (int j = 1; j < LOG; j++)
    {
        up[j][u] = up[j-1][ up[j-1][u] ];
    }

    // 遍历 u 的所有邻接点
    for (int v : tree[u])
    {
        if (v != father)  // 不往回走（不访问父节点）
        {
            dfs(v, u);    // 递归处理子节点
        }
    }
}

// ===================== 求两个节点的最近公共祖先 LCA =====================
int lca(int x, int y)
{
    // 确保 x 的深度更大，方便统一往上跳
    if (dep[x] < dep[y]) swap(x, y);

    // 第一步：把 x 跳到和 y 同一深度
    for (int j = LOG - 1; j >= 0; j--)
    {
        if (dep[ up[j][x] ] >= dep[y])
            x = up[j][x];
    }

    // 如果跳到了同一个点，直接返回
    if (x == y) return x;

    // 第二步：x 和 y 一起往上跳，直到它们的父节点相同
    for (int j = LOG - 1; j >= 0; j--)
    {
        if (up[j][x] != up[j][y])
        {
            x = up[j][x];
            y = up[j][y];
        }
    }

    // 最后再跳一步，就是 LCA
    return up[0][x];
}

// ===================== 核心函数：构建 Kruskal 重构树 =====================
void build_kruskal(int n, int m)
{
    node_cnt = n;  // 初始节点编号：原图 1~n，新节点从 n+1 开始

    // 并查集初始化：每个节点的父节点 = 自己
    for (int i = 1; i <= 400020; i++)
        fa[i] = i;

    // 把所有边按权值从小到大排序（Kruskal 最小生成树）
    sort(edge + 1, edge + m + 1);

    // 遍历每一条边，尝试合并连通块
    for (int i = 1; i <= m; i++)
    {
        int u = edge[i].u;
        int v = edge[i].v;
        int w = edge[i].w;

        int fu = find(u);  // u 所在集合的根
        int fv = find(v);  // v 所在集合的根

        // 如果 u 和 v 不在同一个集合
        if (fu != fv)
        {
            node_cnt++;          // 创建一个【新节点】
            val[node_cnt] = w;   // 新节点权值 = 当前边的权值

            // 重构树加边：新节点 作为 fu 和 fv 的父节点
            tree[node_cnt].push_back(fu);
            tree[node_cnt].push_back(fv);

            // 并查集更新：把两个根连到新节点上
            fa[fu] = node_cnt;
            fa[fv] = node_cnt;
        }
    }

    // 以最后一个新建节点为根，DFS 预处理 LCA
    dfs(node_cnt, 0);
}

// ===================== 主函数 =====================
int main()
{
    int n, m;          // n = 点数, m = 边数
    cin >> n >> m;

    // 读入 m 条边
    for (int i = 1; i <= m; i++)
    {
        cin >> edge[i].u >> edge[i].v >> edge[i].w;
    }

    // 构建 Kruskal 重构树
    build_kruskal(n, m);

    int s, t;          // 查询 s 到 t 的路径最大边权最小值
    cin >> s >> t;

    // LCA 的权值 = 答案
    cout << val[lca(s, t)] << endl;

    return 0;
}
```
