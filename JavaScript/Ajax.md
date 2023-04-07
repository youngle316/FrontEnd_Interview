# JS-WEB-API-Ajax

### 问：什么是浏览器的同源策略

答：浏览器的同源策略是一种安全机制，它要求浏览器只能在同一域名、协议和端口下加载资源。也就是说，一个网页只能访问与其本身在同一域名、协议和端口的网页的数据，而不能访问其他域名、协议或端口的资源。这种限制可以防止恶意网站获取用户的敏感信息，或者通过跨域请求来攻击其他网站。

### 问：解决跨域的方式

答：

1. 利用 script 标签不受同源政策限制的特性，通过动态创建 script 标签来实现跨域通信。

```javascript
<script>
function handleResponse(response) {
  console.log(response);
}

const script = document.createElement('script');
script.src = 'https://domain-a.com/data?callback=handleResponse';
document.body.appendChild(script);
</script>
```

2. CORS：服务端设置响应头信息来允许特定域名或所有域名的请求访问。
   ```shell
   Access-Control-Allow-Origin: https://www.example.com
   Access-Control-Allow-Methods: GET, POST, PUT
   Access-Control-Allow-Headers: Content-Type
   ```
3. 代理：前端将请求发给自己的服务器，再由服务器转发请求并返回结果，从而实现跨域。
4. WebSocket：使用 WebSocket 协议进行通信，不受同源政策限制。
