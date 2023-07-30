XSS保护
===================================================

HTML过滤
---------------------------------------------------
使用一些白名单或者黑名单来过滤用户输入的HTML，以实现过滤的效果。例如DOMPurify等工具都是用该方式实现了XSS的保护。

X-Frame
---------------------------------------------------
X-Frame-Options 响应头有三个可选的值：

- DENY
    - 页面不能被嵌入到任何iframe或frame中
- SAMEORIGIN
    - 页面只能被本站页面嵌入到iframe或者frame中
- ALLOW-FROM
    - 页面允许frame或frame加载

XSS保护头
---------------------------------------------------
基于 Webkit 内核的浏览器(比如Chrome)在特定版本范围内有一个名为XSS auditor的防护机制，如果浏览器检测到了含有恶意代码的输入被呈现在HTML文档中，那么这段呈现的恶意代码要么被删除，要么被转义，恶意代码不会被正常的渲染出来。

而浏览器是否要拦截这段恶意代码取决于浏览器的XSS防护设置。

要设置浏览器的防护机制，则可使用X-XSS-Protection字段
该字段有三个可选的值

- ``0`` : 表示关闭浏览器的XSS防护机制
- ``1`` : 删除检测到的恶意代码， 如果响应报文中没有看到 X-XSS-Protection 字段，那么浏览器就认为X-XSS-Protection配置为1，这是浏览器的默认设置
- ``1; mode=block`` : 如果检测到恶意代码，在不渲染恶意代码

FireFox没有相关的保护机制，如果需要保护，可使用NoScript等相关插件。