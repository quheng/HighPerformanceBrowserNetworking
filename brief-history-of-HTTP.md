# HTTP 简史
超文本传输​​协议（HTTP）是互联网上最普遍并广泛采用的应用协议之一：它是客户端和服务器之间通信的常见语言，用于实现现代 Web。从简单的一个单一的关键字和文档路径开始，它已经不仅适用于浏览器，几乎应用在每个由互联网连接起来的软件和硬件。

本章将简要介绍 HTTP 协议的演进过程。全面讨论所有 HTTP 的语义超出了本书的范围，但是了解 HTTP 的关键设计变更以及背后的动机将为我们讨论 HTTP 的性能提供必要的背景知识，特别是在即将到来的 HTT /2 中带来的改进。

## 一行的 HTTP 0.9 协议
Tim Berners-Lee 设计的原始 HTTP 提案很简单用于帮助采用他的另一个新想法：万维网。这个策略似乎有效：注意他是一个有抱负的协议设计者。

1991 年，Berners-Lee 简述了新协议的动机，列出了几个高级设计目标：文件传输功能，能够请求对超文本存档的索引搜索，格式协商以及能够让客户端跳转到另一个服务器。为了用行动证明理论，他建立了一个实现了所提议功能一小部分的简单原型。

1. 客户端的请求是单个 ASCII 字符串。
2. 客户端请求由回车（CRLF）终止。
4. 服务器响应是 ASCII 字符流。
5. 服务器响应是超文本标记语言（HTML）。
6. 连接在文档传输完成后终止。

然而，即使这样描述也比真实协议更复杂。实现这些规则极度简单并且对  Telnet 协议友好。
> $> telnet google.com 80
> Connected to 74.125.xxx.xxx
> 
> GET /about/
> 
> (hypertext response)
> (connection closed)

该请求只有一行：**GET** 方法和请求文档的路径。响应是单个超文本文档 - 不包含标题或任何其他元数据，只是 HTML。它真的不能更简单。此外，由于先前的交互是预期协议的子集，所以它非正式地获取了 HTTP0.9 标签。正如他们所说，其余的都是历史。

从 1991 年这些简陋的规则开始，HTTP 在未来几年内展现了它的生机并迅速发展。让我们快速回顾一下 HTTP0.9 的特点：

1. 客户端-服务器，请求-响应协议。
2. ASCII 协议，运行在 TCP/IP 链路上。
3. 设计用来传输超文本文件（HTML）。
4. 每个请求后关闭服务器和客户端之间的连接。

>*note*
>
流行的 Web 服务器，如 Apache 和 Nginx，仍然支持 HTTP0.9 协议，部分是因为它很简单！如果您好奇的话，打开一个 Telnet 对话并尝试通过 HTTP0.9 访问google.com 或您自己喜欢的站点，并检查此早期协议的行为和限制。

# HTTP/1.0：快速增长和信息 RFC
在 1991 年到 1995 年之间，HTML规范以及被称为“网络浏览器”的新型软件共同快速发展。以消费者为导向的公共互联网基础设施迅速增长。

>**完美风暴：20 世纪 90 年代初互联网的爆发**
>基于 Tim Berner-Lee 最初的浏览器原型，国家超级计算应用中心（NCSA）的一个团队决定实现自己的版本。第一个主流浏览器就这样诞生了：NCSA Mosaic。NCSA 团队中的一名程序员 Marc Andreessen 与 Jim Clark 合作，于 1994 年 10 月成立 Mosaic Communications 公司，后来更名为 Netscape，并于1994 年 12 月发布了 Netscape Navigator 1.0。至此，万维网已经不仅仅只有学术意义。
>
>事实上，第一次世界范围的万维网会议在瑞士日内瓦在同年举行，这导致了旨在帮助指导 HTML 发展的万维网联盟(W3C)的创立。类似地，IETF 中同时建立了一个致力于改进 HTTP 协议的 HTTP 工作组(HTTP-WG)。这两个小组对网络的演进起到了重要的作用。
>
>最后，为了创造一个完美风暴，CompuServe, AOL 和 Prodigy 开始在 1994-1995 年期间提供拨号上网服务。在这波迅速接受的浪潮中，网景公司在 1995 年 8 月 9 日成功的上市创造了历史，标志着互联网的繁荣到来了，每个人都想要分一份羹!

新兴 Web 的期望功能列表及其在公共 Web 上的使用方式迅速暴露了 HTTP 0.9 的许多基本限制：我们需要一个不仅可以服务于超文本文档，还可以提供更多关于请求的元数据响应，启用内容协商等等的协议。反过来，网络开发人员的新兴社区通过生成大量实验性的 HTTP 服务器和客户端作为响应：实施，部署并查看其他人是否采纳。

从快速实验期开始，出现了一套最佳实践和常见模式，1996 年 5 月，HTTP 工作组（HTTP-WG）发布了 RFC 1945，该文件记录了许多 HTTP/1.0 的“常见用法”。请注意，这只是一个信息 RFC：正如我们知道的 HTTP/1.0 不是正式的规范或 Internet 标准！

话虽如此，HTTP/1.0 的示例看起来应该很熟悉：
```
$> telnet website.org 80

Connected to xxx.xxx.xxx.xxx

GET /rfc/rfc1945.txt HTTP/1.0 注1
User-Agent: CERN-LineMode/2.15 libwww/2.17b3
Accept: */*

HTTP/1.0 200 OK 注2
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 01 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 1 May 1996 12:45:26 GMT
Server: Apache 0.84

(plain-text response)
(connection closed)
```

注1：请求行与 HTTP 版本号，后跟请求头

注2：响应状态，后跟响应头

上述交换不是 HTTP/1.0 详尽的功能列表，但它说明了一些关键的协议改变：

1. 请求可能包含多个按行分割的头字段。
2. 响应对象以响应状态行为前缀。
3. 响应对象有自己的按行分割的头字段。
4. 响应对象不限于超文本。
5. 每个请求后，服务器和客户端之间的连接关闭。

请求和响应头都用 ASCII 编码保存，但响应对象本身可以是任何类型：HTML 文件，纯文本文件，图像或任何其他内容类型。因此，HTTP 的“超文本传输​​”在引入后不久就部分成为了一个误解。在现实中，HTTP 已经迅速发展成为了一个超媒体传输，但是原来的名字就被卡住了。

除了媒体类型协商外，RFC 还记录了许多其他常用功能：内容编码，字符集支持，多部分类型，授权，缓存，代理行为，日期格式等。

>*note*
>
>网络上几乎每个服务器今天都可以并仍然会使用 HTTP/1.0。除此之外，你应该知道的更多！每个请求都建立新的 TCP 连接对 HTTP/1.0 会造成重大的性能损失;请参阅[三次握手](https://hpbn.co/building-blocks-of-tcp/#three-way-handshake)与[缓慢启动](https://hpbn.co/building-blocks-of-tcp/#slow-start)。

# HTTP/1.1: 互联网标准
让 HTTP 称为官方 IETF 互联网标准的工作与 HTTP/1.0 的文档工作并行进行，这大约用了四年时间：1995 年至 1999 年。事实上，1997 年 1 月，大约是 HTTP/1.0 发布之后六个月，正式发布的RFC 2068 定义了第一个官方 HTTP/1.1 标准。两年半后，1999 年 6 月，标准纳入了一些改进和更新，并作为 RFC 2616 发布。

HTTP/1.1 标准解决了早期版本中的许多模糊不清的地方，并引入了许多关键性能优化：keepalive 连接，分块编码传输，字节范围请求，附加缓存机制，传输编码和请求流水线。

通过这些功能，我们现在可以查看任何现代 HTTP 浏览器和客户端执行的典型 HTTP/1.1 会话：

```
$> telnet website.org 80
Connected to xxx.xxx.xxx.xxx

GET /index.html HTTP/1.1 注1
Host: website.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4)... (snip)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip,deflate,sdch
Accept-Language: en-US,en;q=0.8
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
Cookie: __qca=P0-800083390... (snip)

HTTP/1.1 200 OK 注2
Server: nginx/1.0.11
Connection: keep-alive
Content-Type: text/html; charset=utf-8
Via: HTTP/1.1 GWA
Date: Wed, 25 Jul 2012 20:23:35 GMT
Expires: Wed, 25 Jul 2012 20:23:35 GMT
Cache-Control: max-age=0, no-cache
Transfer-Encoding: chunked

100 注3
<!doctype html>
(snip)

100
(snip)

0 注4

GET /favicon.ico HTTP/1.1 注5
Host: www.website.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4)... (snip)
Accept: */*
Referer: http://website.org/
Connection: close 注6
Accept-Encoding: gzip,deflate,sdch
Accept-Language: en-US,en;q=0.8
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
Cookie: __qca=P0-800083390... (snip)

HTTP/1.1 200 OK 注7
Server: nginx/1.0.11
Content-Type: image/x-icon
Content-Length: 3638
Connection: close
Last-Modified: Thu, 19 Jul 2012 17:51:44 GMT
Cache-Control: max-age=315360000
Accept-Ranges: bytes
Via: HTTP/1.1 GWA
Date: Sat, 21 Jul 2012 21:35:22 GMT
Expires: Thu, 31 Dec 2037 23:55:55 GMT
Etag: W/PSA-GAu26oXbDi

(icon data)
(connection closed)
```

注1：请求 HTML 文件，其中包含编码，字符集和 Cookie 元数据

注2：分块响应原始 HTML 请求

注3：块中的八位字节数以 ASCII 十六进制数表示（256字节）

注4：分块流响应结束

注5：请求在同一个 TCP 连接上的图标文件

注6：通知服务器连接不会被重用

注7：图标响应，后跟连接关闭

呃，那里有很多事情！第一个也是最明显的区别是，我们有两个对象请求，一个用于 HTML 页面，另一个用于图像，两者通过单个连接传递。这是连接 keepalive 的行为，这允许指向相同的主机的多个请求重用现有的 TCP 连接以提供更快的用户体验;请参阅[优化 TCP](https://hpbn.co/building-blocks-of-tcp/#optimizing-for-tcp)。

要终止持久连接，请注意第二个客户端请求通过显式的 **Connection** 头向服务器发送明确的 **close** 令牌。类似地，一旦响应结果传输完毕，服务器可以通知客户端尝试关闭当前 TCP 连接。从技术上讲，任何一方都可以在任何时候终止 TCP 连接而无需这种信号，但客户端和服务器都应尽可能的提供它，以便在双方实现更好的连接重用策略。

>*note*
>
>默认情况下，HTTP/1.1 改变了 HTTP 协议的语义用于 keepalive。这就是说，除非另有说明（通过 **Connection: close** 头），服务器应默认保持连接打开。
>
>但是，同样的功能也被反映到 HTTP / 1.0 是通过 **Connection：Keep-Alive** 头启用。因此，如果您使用 HTTP/1.1，在技术上您不需要 **Connection：Keep-Alive** 头，但是许多客户端选择提供它。

此外，HTTP/1.1 协议添加了内容，编码，字符集，甚至语言协商，传输编码，缓存指令，客户端 cookie，以及可以在每个请求上协商的十几个其他功能。

我们不会讲述每个 HTTP/1.1 功能的语义。可以有一本专门的书来讲这个主题并且已经完成了许多出色的书籍。相反，前面的例子很好地说明了 HTTP 的快速演进，以及每个客户端-服务器交换的错综复杂。那里有很多的事情！

>*note*
>
>David Gourley 和 Brian Totty 编著的 O'Reilly 图书 *HTTP: The Definitive Guide* 是有关 HTTP 协议的所有内部工作的一个很好的参考。

# HTTP/2：提升传输性能
RFC 2616 发布以来，已经成为了互联网前所未有的增长的基础：数十亿种各种类别和大小的设备，从台式电脑到我们口袋里的小型网络设备，每天都会使用 HTTP 传递新闻，视频，以及我们生活中依赖的数百万其他网络应用。

从最开始一个简单的用于检索超文本的单行协议快速演变成​​一个通用的超媒体传输协议，从今往后的十年内都可以用来支持任何你可以想象到的使用场景。广泛使用 HTTP 协议的客户端与服务器意味着目前许多应用都需要针对 HTTP 特别设计与部署。

需要一个协议来控制你的咖啡壶？RFC 2324 已经包含了超文本咖啡壶控制协议（HTCPCP / 1.0） - 最初是 IETF 的一个愚人节玩笑，而且在我们新的超连接世界中，这样的玩笑越来越多。

>超文本传输​​协议（HTTP）是用于分布式，协作的超媒体信息系统的应用层协议。它是一种通用的，无状态的协议，可以通过扩展请求方法，错误代码和头信息的方式将其用于许多超出超文本传输的任务，如域名服务器和分布式对象管理系统。 HTTP 的一个特征是类型以及数据表示方式的协商，允许系统构建与待传输数据相互独立。
>
>RFC 2616: HTTP/1.1 1999 六月

HTTP 协议的简洁使其最初被采用并快速增长成为了可能。事实上现在很难找到使用HTTP 作为主要的控制和数据协议的嵌入式设备 - 传感器，制动器和咖啡壶。但在自身成功的重压下，随着我们日益将日常互动迁移到网络社交，电子邮件，新闻和视频上，以及越来越多的个人和工作空间，HTTP 也难以应对这些任务。用户和 Web 开发人员现在需要 HTTP/1.1 达到接近实时的响应速度和协议性能，而这些性能要求在不修改协议的情况下难以满足。

为了应对这些新的挑战，HTTP 必须继续发展，因此 HTTPbis 工作组在 2012 年初发布了一个新的 HTTP/2 计划：

>协议中存在新兴的实现经验和兴趣，保留了 HTTP 的语义，去掉了被确定为阻碍性能并滥用底层传输的 HTTP/1.x 的消息框架和语法遗留。
>
>工作组将利用现有的有序双向流的 HTTP 语义开发指定新的表达方式规范。与 HTTP/1.x 一样，主要传输目标是 TCP，但应该可以使用其他传输。
>
>HTTP/2 charter, 2012 年 1 月

HTTP/2 的主要重点是改善传输性能​​，实现更低的延迟和更高的吞吐量。大版本号增加听起来像是很大的一步，虽然它在性能方面确实是很大的一步，但注意到没有高层次协议语义受到影响是很重要的：所有 HTTP 头信息，值和使用场景都是相同的的。

任何现有的网站或应用都可以无需修改的使用 HTTP/2：您可是享受到 HTTP/2 的优点而不需要修改应用标记。 HTTP 服务器将必须使用 HTTP/2，但这是对大多数用户透明的升级。唯一的区别是，如果工作组实现了它们的目标，我们的应用应该具有更低的延迟并且可以更好的利用网络链接！

话虽如此，让我们不要走得太快。在使用新的 HTTP/2 协议功能之前，值得退一步并审查现有的 HTTP/1.1 的部署和性能最佳实践。HTTP/2 工作组正在快速发展新的规范，但即使最终标准已经完成并准备就绪，在可预见的将来我们仍然需要支持较旧的 HTTP/1.1 客户端。

