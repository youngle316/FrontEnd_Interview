# 手写一个 JS 函数，实现数组扁平化 Array Flatten

```typescript
const flat = (arr: any[]) => {
  return arr.reduce((pre, cur) => {
    return pre.concat(Array.isArray(cur) ? flat(cur) : cur);
  }, []);
};
```
