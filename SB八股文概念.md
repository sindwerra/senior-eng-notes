### HTTP和HTTPS的区别
1. http是明文传输，https是加密传输（加密方法是SSL/TLS）
2. http的默认端口是80，https默认端口是443
3. https需要申请证书才能使用
4. http速度比https快，http只需要tcp的三次握手就可以完成，https还需要加上ssl协议的九个数据包

### HTTP 1.0 vs 1.1 vs 2.0
1. http1.0是无状态（不记录过去请求）无连接（使用完立即断开）的协议，通过cookie和session来弥补无状态无连接的缺陷
2. http1.1支持长连接（通过Connection字段），支持请求管道化（假管道化），支持缓存处理（cache-control字段），支持断点传输，增加Host字段
3. http2.0是真正的并行传输，一个响应会分成多个帧在流中间传输，且多个帧之间可以乱序传输，且传输的是二进制数据而不是http1.x的文本传输，http1.1为了解决队头阻塞通过浏览器开启多个tcp连接绕过了该问题，而http2.0通过一个tcp连接支持一个可以无限拓展数量的双向数据流根本上解决了这个问题

### cookie和session
1. 共同点是他们都是用来解决http协议无状态缺陷的一种workaround
2. cookie是服务器发送给客户端的一小块可以用来辨识客户端身份的数据，存储在客户端，cookie不可跨域
3. session是存储在服务器端的维持状态的数据，cookie中包含sessionID就可以与session机制配合解决无状态问题
4. session比cookie安全（存储位置不同）
5. cookie只支持字符串，session可以存储任意类型数据 
6. cookie过期时间长，session短
7. cookie存储量小，session可存数据量大