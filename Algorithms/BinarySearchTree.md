### 题目

[二叉搜索树的第 K 大节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

### 解题思路

> 根据二叉搜索树的特性，中序遍历后会是一个递增的顺序

```typescript
function kthLargest(root: TreeNode | null, k: number): number {
  const result = [];
  const stack = [];
  let cur = root;
  while (cur || stack.length) {
    if (cur) {
      stack.push(cur);
      cur = cur.left;
    } else {
      const node = stack.pop();
      result.push(node.val);
      cur = node.right;
    }
  }
  return result[result.length - k];
}
```

### 复杂度

时间复杂度: $O(n)$

空间复杂度: $O(n)$
