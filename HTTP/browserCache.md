# 谈谈浏览器的缓存

## 缓存的作用

HTTP 缓存可以说是 HTTP 性能优化中简单高效的一种优化方式了，缓存是一种保存资源副本并在下次请求时直接使用该副本的技术，当 web 缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载。

一个优秀的缓存策略可以缩短网页请求资源的距离，减少延迟，节省网络流量，并且由于缓存文件可以重复利用，降低网络负荷，提高客户端响应。

## 缓存的类型

### 强缓存

![强缓存](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/SCR-20230407-kgd.png)

1. **cache-control**

**cache-control** 是 HTTP1.1 新增的响应头，有以下的几个值可选

- **max-age**：如 3000，单位为秒，表示可缓存的时长
- **s-maxage**：和 max-age 是一样的，不过它只针对代理服务器缓存而言
- **public**：指示响应可被任何缓存区缓存；
- **private**：只能针对个人用户，而不能被代理服务器缓存；
- **no-cache**：强制客户端直接向服务器发送请求,也就是说每次请求都必须向服务器发送。服务器接收到请求，然后判断资源是否变更，是则返回新内容，否则返回 304，未变更。这个很容易让人产生误解，使人误 以为是响应不被缓存。实际上 Cache-Control: no-cache 是会被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。
- **no-store**：禁止一切缓存

1. **expires**

expires 是 HTTP1.0 时使用的，表示过期的时间。但是服务器时间和客户端时间一般是不一样的，所以在 HTTP1.1 的时候新增了 cache-control。

1. cache-control 的优先级要高于 expires

### 协商缓存

![协商缓存](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/202304071444472.png)

1. **last-modified 与 if-modified-since**

last-modified 表示该文件上一次被修改的时间

1. **etag 与 if-none-match**

etag 表示文件的唯一标识，格式如 etag: "5f2976a4-17d"

1. 两者的区别

etag 的出现时为了解决 last-modified 所存在的一些问题

- 当周期性的更改文件的时间，但是并没有更改文件的内容时
- last-modifed 只能精确到秒，如果一个文件在 1 秒内更改了多次，那么无法更新到最新的数据，而 etag 的精确度更高
- 某些服务器不能精确的得到文件的最后修改时间

1. last-modified 与 etag 是可以一起使用的，服务器会优先验证 etag，一致的情况下，才会继续比对 last-modified，最后才决定是否返回 304
