### 题目

[反转链表](https://leetcode.cn/problems/reverse-linked-list/)

### 解题思路

> 使用双指针，可以使用画图的方式会更加的直观

```typescript
function reverseList(head: ListNode | null): ListNode | null {
  let pre = null;
  let cur = head;
  let temp = null;
  while (cur) {
    temp = cur.next;
    cur.next = pre;
    pre = cur;
    cur = temp;
  }
  return pre;
}
```

### 复杂度

时间复杂度：$O(n)$

空间复杂度：$O(1)$
