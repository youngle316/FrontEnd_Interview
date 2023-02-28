### 题目

[有效的括号](https://leetcode.cn/problems/valid-parentheses/)

### 解题思路

1. 使用对象将括号匹配
2. 遍历字符串，如果是左括号则将对应的右括号压入栈中
3. 如果是右括号，则将栈顶的字符串取出，如果不相等则返回 `false`
4. 遍历结束后，判断栈的长度是否为 0

```typescript
function isValid(s: string): boolean {
  if (s.length % 2 === 1) {
    return false;
  }

  const map = {
    "(": ")",
    "[": "]",
    "{": "}",
  };
  const stack = [];
  for (let i = 0; i < s.length; i++) {
    if (s[i] === "(" || s[i] === "[" || s[i] === "{") {
      stack.push(map[s[i]]);
    } else {
      const popString = stack.pop();
      if (s[i] !== popString) {
        return false;
      }
    }
  }
  return stack.length === 0;
}
```

### 复杂度

时间复杂度：$O(n)$

空间复杂度：$O(n)$
