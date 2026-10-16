# Bitset 与跳跃并查集

## Bitset

在 C++ 竞赛 / 做题环境里：`bitset` 最大一般能开到 **1e8 ~ 2e8**。

## 跳跃并查集

- 跳跃并查集：右小优先，`find` 找空闲；用完 `pos` 跳 `pos+1`，永远小数指向大。

## 参考：bitset 用法（补充）

```cpp
#include <bitset>
bitset<1000010> bs;      // 1e6 位
bs[i] = 1;               // 置位
bs.count();              // 1 的个数
bs &= (bs << 1);         // 位运算批量操作
```
