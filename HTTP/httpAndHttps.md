# http 与 https 的区别

简单来说 HTTPS = HTTP + SSL

1. https 协议需要到 ca 申请证书，是要收费的。
2. http 是超文本传输协议，是明文传输的；https 是具有安全性的 ssl 加密传输协议
3. 连接方式不同，默认使用的端口也不同，http 是 80；https 是 443
4. http 的连接很简单，是无状态的；https 是有 http + ssl 构成的可进行加密传输，身份认证的网络协议，更为安全
