# 手写一个 getType 函数，获取详细的数据类型

```typescript
const getType = (value: any) => {
  return Object.prototype.toString.call(value);
};
```
