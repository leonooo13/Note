# 表单欺骗攻击

**1.表单**

表单在网页中主要负责数据采集功能。

**一个表单有三个基本组成部分：**

1）表单标签：＜form＞＜/form＞，这里面包含了处理表单数据所用 CGI 程序的 URL 以及数据提交到服务器的方法。用于申明表单，定义采集数据的范围。

语法：＜formaction="URL" method="GET/POST" enctype="..." target="..."＞. . .＜form＞

action="URL" 用来指定处理提交表单的格式，它可以是一个URL地址(提交给程式)或一个电子邮件地址。

method="GET/POST"指明提交表单的HTTP方法，可能的值为：POST 或 GET。

enctype="..." 指明用来把表单提交给服务器时(当method值为"POST")的互联网媒体形式。

target="..."指定提交的结果文档显示的位置。

2）表单域：包含了文本框、密码框、隐藏域、多行文本框、复选框、单选框、下拉选择框和文件上传框等，用于采集用户的输入或选择的数据。

3）表单按钮：控制表单的运作，包括提交按钮、复位按钮和一般按钮，用于将数据传送到服务器上的CGI脚本或者取消输入，还可以用表单按钮来控制其他定义了处理脚本的处理工作。

**2.表单欺骗攻击**

制造一个欺骗表单几乎与假造一个URL一样简单。毕竟，表单的提交只是浏览器发出的一个HTTP请求而已。请求的部分格式取决于表单，某些请求中的数据来自于用户。

大多数表单用一个相对URL地址来指定action属性，如：＜form action="process.php" method="POST"＞。当表单提交时，浏览器会请求action中指定的URL，同时它使用当前的URL地址来定位相对URL。例如，如果之前的表单是对`http://hongya.com/hongya/form.php`请求的回应所产生的，则在用户提交表单后会请求URL地址`http://hongya.com/hongya/process.php`。

知道了这一点，很容易就能想到你可以指定一个绝对地址，这样表单就可以放在任何地方了：`<form action="http://hongya.com/hongya/process.php" method="POST">`。

这个表单可以放在任何地方，并且使用这个表单产生的提交与原始表单产生的提交是相同的。意识到这一点，攻击者可以通过查看页面源文件并保存在他的服务器上，同时将action更改为绝对URL地址。通过使用这些手段，攻击者可以任意更改表单，如取消最大字段长度限制，取消本地验证代码，更改隐藏字段的值，或者出于更加灵活的目的而改写元素类型。这些更改帮助攻击者向服务器提交任何数据，同时由于这个过程非常简便易行，攻击者无需是一个专家即可做到。

欺骗表单攻击是不能防止的，尽管这看起来有点奇怪，但事实上如此。不过这你不需要担心，一旦你正确地过滤了输入，用户就必须要遵守你的规则，这与他们如何提交无关。

# URL攻击

**1.URL**

统一资源定位符（Uniform Resource Locator，URL）是对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

在因特网的历史上，统一资源定位符（URL）的发明是一个非常基础的步骤。统一资源定位符的语法是一般的，可扩展的，它使用ASCII代码的一部分来表示互联网的地址。一般统一资源定位符的开始标志着一个计算机网络所使用的网络协议。

统一资源定位符是统一资源标志符的一个下种。统一资源标志符确定一个资源，而统一资源定位符不但确定一个资源，而且还表示出它在哪里。

**URL 的基本结构：**

基本URL包含模式（或称协议）、服务器名称（或IP地址）、路径和文件名，如“协议://授权/路径?查询”。

完整的、带有授权部分的普通统一资源标志符语法，如：“协议://用户名:密码@子域名.域名.顶级域名:端口号/目录/文件名.文件后缀?参数=值#标志”

**URL 的两种基本类型：**

1）绝对URL

绝对URL（absolute URL）显示文件的完整路径，这意味着绝对URL 本身所在的位置与被引用的实际文件的位置无关。

2）相对URL

相对URL（relative URL）以包含URL本身的文件夹的位置为参考点，描述目标文件夹的位置。如果目标文件与当前页面（也就是包含URL的页面）在同一个目录，那么这个文件的相对URL仅仅是文件名和扩展名，如果目标文件在当前目录的子目录中，那么它的相对URL是子目录名，后面是斜杠，然后是目标文件的文件名和扩展名。

如果要引用文件层次结构中更高层目录中的文件，那么使用两个句点和一条斜杠。可以组合和重复使用两个句点和一条斜杠，从而引用当前文件所在的硬盘上的任何文件，一般来说，对于同一服务器上的文件，应该总是使用相对URL，它们更容易输入，而且在将页面从本地系统转移到服务器上时更方便，只要每个文件的相对位置保持不变，链接就仍然是有效地。

**2.URL攻击**

好奇心是很多攻击者的主要动机，语义URL攻击就是一个很好的例子。此类攻击主要包括对URL进行编辑以期发现一些有趣的事情。例如，如果用户chris点击了你的软件中的一个链接并到达了页面`http://hongya.com/private.php?user=chris`，很自然地他可能会试图改变user的值，看看会发生什么。例如，他可能访问`http://hongya.com/private.php?user=rasmus`来看一下他是否能看到其他人的信息。虽然对GET数据的操纵只是比对POST数据稍为方便，但它的暴露性决定了它更为频繁的受攻击，特别是对于攻击的新手而言。

大多数的漏洞是由于疏漏而产生的，而不是特别复杂的原因引起的。虽然很多有经验的程序员能轻易地意识到上面所述的对URL的信任所带来的危险，但是常常要到别人指出才恍然大悟。

# HTTP攻击

**1.HTTP 协议**

超文本传输协议 HTTP（HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。

HTTP是一个客户端和服务器端请求和应答的标准（TCP）。客户端是终端用户，服务器端是网站。通过使用Web浏览器、网络爬虫或者其它的工具，客户端发起一个到服务器上指定端口（默认端口为80）的HTTP请求。（我们称这个客户端）叫用户代理（user agent）。应答的服务器上存储着（一些）资源，比如HTML文件和图像。（我们称）这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个中间层，比如代理，网关，或者隧道（tunnels）。尽管TCP/IP协议是互联网上最流行的应用，HTTP协议并没有规定必须使用它和（基于）它支持的层。 事实上，HTTP可以在任何其他互联网协议上，或者在其他网络上实现。HTTP只假定（其下层协议提供）可靠的传输，任何能够提供这种保证的协议都可以被其使用。

通常，由HTTP客户端发起一个请求，建立一个到服务器指定端口（默认是80端口）的TCP连接。HTTP服务器则在那个端口监听客户端发送过来的请求。一旦收到请求，服务器（向客户端）发回一个状态行，比如“HTTP/1.1 200 OK”，和（响应的）消息，消息的消息体可能是请求的文件、错误消息、或者其它一些信息。HTTP使用TCP而不是UDP的原因在于（打开）一个网页必须传送很多数据，而TCP协议提供传输控制，按顺序组织数据，和错误纠正。

通过HTTP或者HTTPS协议请求的资源由统一资源标示符（Uniform Resource Identifiers）（或者，更准确一些，URLs）来标识。

**2.HTTP请求**

HTTP请求是指从客户端到服务器端的请求消息。包括：消息首行中，对资源的请求方法、资源的标识符及使用的协议。

**3.HTTP 应答状态**

下面是一个最简单的应答：

  HTTP/1.1 200 OK  

  Content-Type: text/plain  

  Hello World  

第一行是状态行，第二行开始是其他信息。状态行包含HTTP版本、状态代码、与状态代码对应的简短阐明信息。在大多数情况下，除了Content-Type之外的所有应答头都是可选的。但Content-Type是必需的，它描述的是后面文档的MIME类型。虽然大多数应答都包含一个文档，但也有一些不包含，例如对HEAD请求的应答永远不会附带文档。有许多状态代码实际上用来标识一次失败的请求，这些应答也不包含文档（或只包含一个简短的错误信息阐明）。

**HTTP定义了以下8种方法来发送请求：**

1) GET：请求响应，这是最常使用的方法。

2) HEAD：与GET相同的响应，是只要求响应的表头信息。

3) POST：发送数据给服务器处理，数据包含在HTTP信息正文q

4) PUT：上传文件。

5) DELETE：删除文件。

6) TRACE：追踪所收到的请求。

7) OPTIONS：返回服务器所支持的HTTP请求的方法。

8) CONNECT：将HTTP请求的连接转换成透明的TCP/IP通道。

HTTP 状态代码及其定义如下：

代码 指示

2xx 成功。

200 正常；请求已完成。

201 正常；紧接 POST 命令。

202 正常；已接受用于处理，但处理尚未完成。

203 正常；部分信息 — 返回的信息只是一部分。

204 正常；无响应 — 已接收请求，但不存在要回送的信息。

3xx 重定向

301 已移动 — 请求的数据具有新的位置且更改是永久的。

302 已找到 — 请求的数据临时具有不同 URI。

303 请参阅其它 — 可在另一 URI 下找到对请求的响应，且应使用 GET 方法检索此响应。

304 未修改 — 未按预期修改文档。

305 使用代理 — 必须通过位置字段中提供的代理来访问请求的资源。

306 未使用 — 不再使用；保留此代码以便将来使用。

4xx 客户机中出现的错误

400 错误请求 — 请求中有语法问题，或不能满足请求。

401 未授权 — 未授权客户机访问数据。

402 需要付款 — 表示计费系统已有效。

403 禁止 — 即使有授权也不需要访问。

404 找不到 — 服务器找不到给定的资源；文档不存在。

407 代理认证请求 — 客户机首先必须使用代理认证自身。

415 介质类型不受支持 — 服务器拒绝服务请求，因为不支持请求实体的格式。

5xx 服务器中出现的错误

500 内部错误 — 因为意外情况，服务器不能完成请求。

501 未执行 — 服务器不支持请求的工具。

502 错误网关 — 服务器接收到来自上游服务器的无效响应。

503 无法获得服务 — 由于临时过载或维护，服务器无法处理请求。

**4.HTTP请求欺骗攻击**

HTTP请求欺骗攻击（Soiifed HTTP Requests）是黑客将原本的HTTP请求经过修改后，再执行黑客修改后的HTTP请求来欺骗网站。

HTTP请求欺骗比欺骗表单更高级和复杂。这给了攻击者完全的控制权与灵活性，它进一步证明了不能盲目信任用户提交的任何数据。

为了讲解这是如何进行的，请看如下表单代码：

  ＜form action="process.php" method="POST"＞  

  ＜p＞Please select a color:  

  ＜select name="color"＞  

  ＜option value="red"＞Red＜/option＞  

  ＜option value="green"＞Green＜/option＞  

  ＜option value="blue"＞Blue＜/option＞  

  ＜/select＞＜br /＞  

  ＜input type="submit" value="Select" /＞＜/p＞  

  ＜/form＞  

如果用户选择了Red并点击了Select按钮后，浏览器会发出如下的HTTP请求：

  POST /process.php HTTP/1.1  

  Host: [example.org](http://example.org)  

  User-Agent: Mozilla/5.0 (X11; U; Linux i686)  

  Referer: `http://example.org/form.php`  

  Content-Type: application/x-www-form-urlencoded  

  Content-Length: 9  

  color=red  

看到大多数浏览器会包含一个来源的URL值，你可能会试图使用$_SERVER['HTTP_REFERER']变量去防止欺骗。确实，这可以用于对付利用标准浏览器发起的攻击，但攻击者是不会被这个小麻烦给挡住的。通过编辑HTTP请求的原始信息，攻击者可以完全控制HTTP头部的值，GET和POST的数据，以及所有在HTTP请求的内容。

攻击者如何更改原始的HTTP请求？过程非常简单。通过在大多数系统平台上都提供的Telnet实用程序，你就可以通过连接网站服务器的侦听端口（典型的端口为80）来与Web服务器直接通信。

下面就是使用这个技巧的例子：

  $ telnet [example.org](http://example.org) 80  

  Trying 192.0.34.166...  

  Connected to [example.org](http://example.org) (192.0.34.166).  

  Escape character is '^]'.  

  GET / HTTP/1.1  

  Host: [example.org](http://example.org)  

  HTTP/1.1 200 OK  

  Date: Sat, 21 May 2005 12:34:56 GMT  

  Server: Apache/1.3.31 (Unix)  

  Accept-Ranges: bytes  

  Content-Length: 410  

  Connection: close  

  Content-Type: text/html  

  ＜html＞  

  ＜head＞  

  ＜title＞Example Web Page＜/title＞  

  ＜/head＞  

  ＜body＞  

  ＜p＞You have reached this web page by typing "[example.com](http://example.com)","[example.net](http://example.net)", or 　　"[example.org](http://example.org)" into your web browser.＜/p＞  

  ＜p＞These domain names are reserved for use in documentation and are notavailable for registration. See＜a href=`"http://www.rfc-editor.org/rfc/rfc2606.txt"`＞RFC 2606＜/a＞, Section3.＜/p＞  

  ＜/body＞  

  ＜/html＞  

  Connection closed by foreign host.  

  $  

上例中所显示的请求是符合HTTP/1.1规范的最简单的请求，这是因为Host信息是头部信息中所必须有的。一旦你输入了表示请求结束的连续两个换行符，整个HTML的回应即显示在屏幕上。

Telnet实用程序不是与Web服务器直接通信的唯一方法，但它常常是最方便的。可是如果你用php编码同样的请求，你可以就可以实现自动操作了。

前面的请求可以用下面的PHP代码实现：

  ＜?php  

    $http_response = '';  

    $fp = fsockopen('[example.org](http://example.org)', 80);  

    fputs($fp, "GET / HTTP/1.1\r\n");  

    fputs($fp, "Host: [example.org](http://example.org)\r\n\r\n");  

      while (!feof($fp))  

      {  

        $http_response .= fgets($fp, 128);  

      }  

      fclose($fp);  

      echo nl2br(htmlentities($http_response, ENT_QUOTES, 'UTF-8'));  

    ?＞  

当然，还有很多方法去达到上面的目的，但其要点是HTTP是一个广为人知的标准协议，稍有经验的攻击者都会对它非常熟悉，并且对常见的安全漏洞的攻击方法也很熟悉。

相对于欺骗表单，欺骗HTTP请求的做法并不多，对它不应该过多关注。讲述这些技巧的原因是为了更好的演示一个攻击者在向你的应用输入恶意信息时是如何地方便。这再次强调了过滤输入的重要性和HTTP请求提供的任何信息都是不可信的这个事实。