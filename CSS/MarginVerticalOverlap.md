# margin 纵向重叠问题

```html
<style>
  p {
    font-size: 16px;
    line-height: 1;
    margin-top: 10px;
    margin-bottom: 15px;
  }
</style>

<p>AAA</p>
<p></p>
<p></p>
<p></p>
<p>BBB</p>
```

### 问：AAA 和 BBB 之间的距离是多少

答：由于有 `margin` 塌陷问题，所以距离是 15px

原因：

- 相邻元素的 `margin-top` 和 `margin-bottom` 会重叠
- 空白内容的 `p` 标签也会重叠
