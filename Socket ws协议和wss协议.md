## WebSocket WS协议和WSS协议

Websocket协议是为了解决web即时应用中服务器与客户端浏览器全双工通信的问题而设计的，是完全意义上的Web应用端的双向通信技术，可以取代之前使用半双工HTTP协议而模拟全双工通信，同时克服了带宽和访问速度等的诸多问题。协议定义为ws和wss协议，分别为普通请求和基于SSL的安全传输，占用端口与http协议系统，ws为80端口，wss为443端口，这样可以支持HTTP代理。


WSS协议：Web Services Security
保证消息的完整性和保密性的基础上实现了更高级别的Web服务应用程序

### Websocket URI

定义的两个协议框架ws和wss与http类似，而且各自部分的要求也是在HTTP协议中使用的一样，各自的URI如下：
ws-URI = "ws:" "//" host [ ":" port ] path [ "?" query ]
wss-URI = "wss:" "//" host [ ":" port ] path [ "?" query ]
其中port是可选项，query前接“?”。


### 握手（Opening & Closing Handshake）打开连接

当建立一个Websocket连接时，为了保持基于HTTP协议的服务器软件和中间件进行兼容工作，客户端打开一个连接时使用与HTTP连接的同一个端口到服务器进行连接，这样被设计为一个升级的HTTP请求。

### 1 发送握手请求:
时的连接状态是CONNECTING，客户端需要提供host、port、resource-name和一个是否是安全连接的标记，也就是一个WebSocket URI。
客户端发送的一个到服务器端握手请求如下：

  GET /chat HTTP/1.1
  Host: server.example.com
  Upgrade: websocket
  Connection: Upgrade
  Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
  Origin: http://example.com
  Sec-WebSocket-Protocol: chat, superchat
  Sec-WebSocket-Version: 13

这个升级的HTTP请求头中的字段顺序是可以随便的。与普通HTTP请求相比多了一些字段。
- Sec-WebSocket-Protocol：字段表示客户端可以接受的子协议类型，也就是在Websocket协议上的应用层协议类型。上面可以看到客户端支持chat和superchat两个应用层协议，当服务器接受到这个字段后要从中选出一个协议返回给客户端。
- Upgrade：告诉服务器这个HTTP连接是升级的Websocket连接。
- Connection：告知服务器当前请求连接是升级的。
- Origin：该字段是用来防止客户端浏览器使用脚本进行未授权的跨源攻击，这个字段在WebSocket协议中非常重要。服务器要根据这个字段判断是否接受客户端的Socket连接。可以返回一个HTTP错误状态码来拒绝连接。
- Sec-WebSocket-Key：为了表示服务器同意和客户端进行Socket连接，服务器端需要使用客户端发送的这个Key进行校验，然后返回一个校验过的字符串给客户端，客户端验证通过后才能正式建立Socket连接。服务器验证方法是：首先进行 Key + 全局唯一标示符（GUID）“258EAFA5-E914-47DA-95CA-C5AB0DC85B11”连接起来，然后将连接起来的字符串使用SHA-1哈希加密，再进行base64加密，将得到的字符串返回给客户端作为握手依据。其中GUID是一个对于不识别WebSocket的网络端点不可能使用的字符串。

相关资料连接：
http://www.cnblogs.com/oshyn/p/3574497.html
