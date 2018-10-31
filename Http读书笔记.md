### Http头

1. Http1.0没有长链接， Http1.1默认长链接，是为了避免不断的建立链接，断开链接的消耗，并且可以不得到确认继续请求，提交响应速度。

2. Http分为请求报文和相应报文

   1. 请求报文
      1. 报文首部
         1. 请求行
         2. 请求首部字段
         3. 通用首部字段
         4. 实体首部字段
         5. 其他
         6. 空行
         7. 报文主体
   2. 响应报文
      1. 报文首部
         1. 状态行
         2. 响应首部字段
         3. 通用首部字段
         4. 实体首部字段
         5. 其他
      2. 空行
      3. 报文主体

   ![1540880370025](/home/justinniu/MarkDown/notebook/1540880370025.png)

3. Http通过编码格式的方式来提升传输速率，并且分块发送

4. Http使用Rang请求部分内容

5. Http使用Accept Accept-Charset Accept-Encoding Accept-Language Content-Language 来进行内容协商，内容协商是指国际化

### [HTTP状态码](https://www.cnblogs.com/cjwxf/p/6186287.html)

1. 2xx
   1. 200 OK 正常处理
   2. 204 请求处理成功，但没有资源可以返回 返回的响应报文中不含实体的主体部分
   3. 206 服务器成功响应了范围请求 响应报文中包含由Content-Range 指定范围的实体内容
2. 3xx
   1. 301 永久重定向 该状态码表示请求的资源已被分配了新的URI，自动转到新位置，按Location首部字段提示的URI重新保存
   2. 302 Found 临时重定向 该状态码表示请求的资源被分配了新的URI，本次希望用户以新的URI访问，但可以继续按当前URI访问
   3. 303 See Other 该状态码表示由于请求对应的资源存在另一个URI，应使用GET方法定向获取请求的资源
   4. 304 Not Modififed 客户端发送附带条件的请求，服务器端允许请求返回资源，但是条件未满足，不返回任何主体
   5. 307 Temporary Redirect 临时重定向 ，服务器目前从不同的位置响应网页请求，但请求者应继续使用原有的链接进行访问，类似302
3. 4xx
   1. 400 Bad Request 请求报文中存在语法错误
   2. 401 Unauthorized 未经授权，当浏览器初次收到401响应，会弹出认证用的对话框
   3. 403 Forbidden 服务器拒绝
   4. 404 Not Found 服务器上没找到请求的资源
4. 5xx
   1. 500 Internal Server Error 内部服务器出错
   2. 503 Service Unavailable 服务器超负荷或者正在停机维护

### WEB服务器

1. 代理
   1. 缓存代理 将资源的副本保存在代理服务器上
   2. 透明代理 不对报文做任何加工处理
2. 网关
   1. 网关能把HTTP请求转换成非HTTP请求
   2. 网关可以提高通信的安全性
3. 隧道 保证客户端和服务器进行安全的通信
4. 页面缓存 分为客户端缓存和代理服务器缓存

### HTTP首部

1. 通用首部字段

   | 首部字段名          | 说明                               |
   | ------------------- | :--------------------------------- |
   | Cache-Control       | 控制缓存的行为                     |
   | Connection          | 逐跳首部、连接的管理               |
   | Date                | 创建报文的日期时间                 |
   | Paragma             | 报文指令                           |
   | Trailer             | 报文末端的首部一览                 |
   | Transfer-Encoding   | 指定报文主题的传输编码方式         |
   | Upgrade             | 升级为其他协议                     |
   | Via                 | 代理服务器相关信息                 |
   | Warning             | 错误通知                           |
   | Accept              | 用户代理可以处理的媒体类型         |
   | Accept-Charset      | 优先的字符集                       |
   | Accept-Encoding     | 优先的内容编码                     |
   | Accept-Language     | 有限的语言                         |
   | Authorization       | Web认证信息                        |
   | Exepect             | 期待服务器的特定行为               |
   | From                | 用户的电子邮件                     |
   | Host                | 请求资源所在的服务器               |
   | If-Match            | 比较实体标记(ETag)                 |
   | If-Modified-Since   | 比较资源的更新时间                 |
   | If-None-Match       | 比较实体标记                       |
   | If-range            | 资源未更新时发送实体Byte的范围请求 |
   | If-Unmodified-Since | 比较资源的更新时间                 |
   | Max-Forwards        | 最大传输跳数                       |
   | Proxy-Authorization | 代理服务器要求客户端的认证信息     |
   | Range               | 实体的字节范围请求                 |
   | Referer             | 对请求中的URI的原始获取方          |
   | TE                  | 传输编码的优先级                   |
   | User-Agent          | HTTP 客户端程序的信息              |

2. 响应首部字段

   | 首部字段名         | 说明                         |
   | ------------------ | ---------------------------- |
   | Accept-Ranges      | 是否接受范围请求             |
   | Age                | 推算资源创建经过的时间       |
   | ETag               | 资源的匹配信息               |
   | Location           | 另客户端重定向至指定URI      |
   | Proxy-Authenticate | 代理服务器对客户端的认证信息 |
   | Petry-After        | 对再次发起请求的时机要求     |
   | Server             | HTTP服务器的安装信息         |
   | Vary               | 代理服务器缓存的管理信息     |
   | WWW-Authenticate   | 服务器对客户端的认证信息     |

3. 实体首部字段

   | 首部字段名       | 说明                   |
   | ---------------- | ---------------------- |
   | Allow            | 资源可支持的HTTP方式   |
   | Content-Encoding | 实体主体使用的编码方式 |
   | Content-Language | 实体主体的自然语言     |
   | Content-Length   | 实体主体的大小(字节)   |
   | Content-Location | 代替对应资源的URI      |
   | Content-MD5      | 实体主体的报文摘要     |
   | Content-Range    | 实体主体的位置范围     |
   | Content-Type     | 实体主体的媒体类型     |
   | Expires          | 实体主体过期的日期时间 |
   | Last-Modified    | 资源最后修改日期时间   |

4. 字段详解

   1. Accept 可以通知服务器，客户端可以接受的媒体类型 可以设置多种，可以设置权重 -q 0到1
   2. Etag 服务器通过Etag来确定需要访问的资源

### HTTPS

1. HTTP的缺点
   1. 通信使用明文，内容可能会被窃听
   2. 不验证通信方的身份，可能遭遇伪装攻击
   3. 无法验证报文的完整性，可能会被篡改

2. HTTPS

   1. 利用SSL协议，将HTTP内容封装起来，交给TCP进行传输

   2. HTTPS过程

      1. 客户端发送Client Hello 报文开始SSL通信

      2. 当服务器可以进行SSL通信时，会返回Server Hello报文

      3. 服务器发送Certificate报文，包含公开密钥

      4. 最后发送Server Hello Done 报文 SSL握手结束

      5. 客户端一Client Key Exchange 报文回应，使用公钥加密

      6. 客户端继续发送Change Cipher Spec 报文，提醒服务器，之后按公钥加密

      7. 客户端发送Finished报文，协商链接是否建立成功

      8. 服务器发送Change Cipher Spec报文

      9. 服务器同样发送Finished报文

      10. 服务器和客户端Finished报文交换完毕后，SSL链接建立完成

      11. 之后采用共享密钥加密发送内容

          ![1540949386567](/home/justinniu/MarkDown/notebook/1540949386567.png)

### Session和Cookie

1. 服务器根据用户登陆信息为其生成Session，并把包含这个Session id的Cookie 发送给客户端
2. 客户端以后在访问就会带着Cookie发送
3. Cookie添加HttpOnly避免XSS攻击

### HTTP攻击

1. 主动攻击 SQL注入和OS命令注入

2. 跨站脚本攻击和跨站点请求伪造，也就是利用钓鱼网站进行攻击






