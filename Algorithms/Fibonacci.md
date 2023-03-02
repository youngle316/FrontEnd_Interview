### 题目

[斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/)

[爬楼梯](https://leetcode.cn/problems/climbing-stairs/) 也是同理

### 解题思路

这道题使用动态规划，动态规划问题可以拆解为五步

1. 确定`dp`数组 `dp[i]` 以及下标的含义
2. 确定递推公式（`dp[i]` 是怎么推断出来的）
3. `dp`数组如何初始化
4. 确定遍历顺序
5. 举例推导`dp`数组

本题中

1. `dp`数组表示斐波那契数列，索引表示斐波那契数列第`i`个数
2. 根据斐波那契数列的规律，`dp[i] = dp[i - 1] + dp[i - 2]`
3. 题目中已经给出`dp[0] = 0`，`dp[1] = 1`
4. 从索引 2 开始遍历，直到 n
5. 举例查看`dp`数组是否满足预期

```typescript
function fib(n: number): number {
  const dp = [0, 1];
  for (let i = 2; i <= n; i++) {
    dp[i] = (dp[i - 1] + dp[i - 2]) % 1000000007;
  }
  return dp[n];
}
```

### 复杂度

时间复杂度: $O(n)$

空间复杂度: $O(n)$
