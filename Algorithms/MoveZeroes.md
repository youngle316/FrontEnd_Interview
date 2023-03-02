### 题目

[移动零](https://leetcode.cn/problems/move-zeroes/)

### 解题思路

题目要求在原地对数组进行操作，可以使用快慢指针，快指针指向下一个元素，慢指针一直指向数组第一个 0

```typescript
/**
 Do not return anything, modify nums in-place instead.
 */
function moveZeroes(nums: number[]): void {
  let left = 0;
  let right = 0;
  while (right < nums.length) {
    if (nums[right] !== 0) {
      let temp = nums[left];
      nums[left] = nums[right];
      nums[right] = temp;
      left++;
    }
    right++;
  }
}
```

### 复杂度

时间复杂度: $O(n)$

空间复杂度: $O(1)$
