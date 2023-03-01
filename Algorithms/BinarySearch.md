### 题目

[二分查找](https://leetcode.cn/problems/binary-search/)

### 解题思路

> 使用左闭右闭区间

```typescript
function search(nums: number[], target: number): number {
  let left = 0;
  let right = nums.length - 1;
  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);
    if (target > nums[mid]) {
      left = mid + 1;
    } else if (target < nums[mid]) {
      right = mid - 1;
    } else {
      return mid;
    }
  }
  return -1;
}
```

### 复杂度

时间复杂度：$O(log n)$

空间复杂度：$O(log n)$
