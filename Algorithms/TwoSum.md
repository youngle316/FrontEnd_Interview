### 题目

[两数之和](https://leetcode.cn/problems/two-sum/)

### 解题思路

1. 使用哈希表将元素存起来
2. 遍历数组，查找当前元素与目标值的差值是否在哈希表中
3. 存在就返回索引

```typescript
function twoSum(nums: number[], target: number): number[] {
  let map = new Map();
  for (let i = 0; i < nums.length; i++) {
    map.set(nums[i], i);
  }

  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (map.has(need) && i !== map.get(need)) {
      return [i, map.get(need)];
    }
  }
}
```

### 复杂度

时间复杂度：$O(n)$

空间复杂度：$O(n)$
