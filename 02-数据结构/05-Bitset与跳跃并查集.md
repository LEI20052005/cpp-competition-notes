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

## Bitset 模板

```cpp
// 1. 定义：bitset<位数> 变量名
    bitset<MAX> bs;
    bitset<MAX> bs2(0x5);    // 十进制5 → 二进制101，低位0、2位是1
    bitset<MAX> bs3("1011"); // 字符串构造：从右到左低位
    // 2. 单点赋值：第k位（从0开始，最低位是bit0）
    bs.set(3);    // 把第3位设为1
    bs.reset(3);  // 把第3位设为0
    bs.flip(2);   // 第2位取反 0<->1
    bs[5] = 1;    // 下标直接修改第5位
    // 3. 查询
    cout << bs.test(5); // 查询第5位是否为1，返回bool
    cout << bs.count(); // 统计整个bitset中1的总个数
    cout << bs.any();   // 是否存在至少一个1
    cout << bs.none();  // 是否全0
    cout << bs.all();   // 是否全部是1
    // 4. 整体输出二进制
cout << bs << endl;
bitset<20> a("1101"), b("1011");
    auto c1 = a & b; // 按位与：同1才1
    auto c2 = a | b; // 按位或：有1则1
    auto c3 = a ^ b; // 按位异或：不同为1
    auto c4 = ~a;    // 按位取反(全部位翻转)
    auto c5 = a << 2;// 左移2位(低位补0，等价×2²)
    auto c6 = a >> 1;// 右移1位(高位补0，等价÷2)
cout << a.to_ulong(); //输出13
```
