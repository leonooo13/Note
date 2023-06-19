`Base64`是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64[26+26+10+++/]个可打印字符来表示二进制数据的方法

![](https://img-blog.csdn.net/20180313122446386?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjA1NDUzNjc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

63→111111

127-1111111

#### 编码原理：

  某一个Byte值是`10111011`B，对应十进制187不属于ASCII码范围，因此无法被传输。这个时候，Base64编码应用而生了，它利用6bit字符表达了原本的8bit字符。Base64可以把原本ASCII码的控制字符甚至ASCII码之外的字符都转换成可打印的6big字符。

  既然单一字符的位数有限，我们可以增加字符的数量。8和6的最小公倍数是24，这就意味着我们可以用4个Base64字符来表示3个传统的8bit字符。

#### 编码过程：

每3个8位明文数据为一组

![](https://secure2.wostatic.cn/static/nJRv5vFYSp3VJ8F3zsRnzK/image.png?auth_key=1687159882-p5EDg8vZ9VjevbKCzgMP9m-0-dc0664a5e11977ebf3329ba210f68cc8)

