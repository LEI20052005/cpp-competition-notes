# 字符串哈希与 substr

## 字符串哈希

- 字符串哈希（记录索引）

## substr 截取函数

在 C++ 的 string 里，`s.substr(0, ins)` 是字符串截取函数，**核心作用**：从字符串 s 的第 0 个位置（开头）开始，截取长度为 ins 的子字符串。

**极简解析**：

1. **参数含义**
   - 第一个参数 0：起始下标（字符串下标从 0 开始）
   - 第二个参数 ins：要截取的字符个数
2. **示例**
   - 若 s = "12345678", ins = 3
   - `s.substr(0,3)` → 截取开头 3 个字符 → "123"

## 参考：字符串哈希模板（补充）

```cpp
typedef unsigned long long ull;
const ull BASE = 131;

ull h[MAXN], p[MAXN];
void init_hash(const string &s) {
    p[0] = 1;
    for (int i = 1; i <= (int)s.size(); i++) {
        h[i] = h[i-1] * BASE + s[i-1];  // 前缀哈希
        p[i] = p[i-1] * BASE;
    }
}
ull get_hash(int l, int r) {            // [l, r] 子串哈希（1-based）
    return h[r] - h[l-1] * p[r-l+1];
}
```
