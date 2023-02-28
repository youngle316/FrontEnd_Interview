## 题目

[剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof)

### 解题思路

1. 两个栈，`stackIn`用来存，`stackOut`用来取
2. `appendTail`: 将值 `push` 到 `stackIn` 中
3. `deleteHead`: 如果 `stackOut` 中为空的话，将 `stackIn` 中的值 `pop` 出再 `push` 到 `stackOut` 中
4. 返回 `stackOut` 栈顶的值或-1

```typescript
class CQueue {
  private stackIn: number[];
  private stackOut: number[];
  constructor() {
    this.stackIn = [];
    this.stackOut = [];
  }

  appendTail(value: number): void {
    this.stackIn.push(value);
  }

  deleteHead(): number {
    if (this.stackOut.length === 0) {
      while (this.stackIn.length) {
        const popNum = this.stackIn.pop();
        this.stackOut.push(popNum);
      }
    }
    return this.stackOut.pop() || -1;
  }
}
```

### 复杂度

时间复杂度：$O(n)$

空间复杂度：$O(n)$
