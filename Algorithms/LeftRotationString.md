### 题目

[剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

### 解题思路

1. 先反转前 k 个字符串
2. 再反转剩下的字符串
3. 最后反转整个字符串

```typescript
function reverseLeftWords(s: string, n: number): string {
  const reverse = (str: string[], left: number, right: number) => {
    while (left < right) {
      let temp = str[left];
      str[left] = str[right];
      str[right] = temp;
      left++;
      right--;
    }
  };
  const sArr = s.split("");
  reverse(sArr, 0, n - 1);
  reverse(sArr, n, s.length - 1);
  reverse(sArr, 0, s.length - 1);
  return sArr.join("");
}
```

### 复杂度

时间复杂度：$O(n)$

空间复杂度：$O(n)$ 由于字符串不可变，需要转换为数组
