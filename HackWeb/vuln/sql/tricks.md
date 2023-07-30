SQL注入小技巧
================================

宽字节注入
--------------------------------
一般程序员用gbk编码做开发的时候，会用 ``set names 'gbk'`` 来设定，这句话等同于

::

    set
    character_set_connection = 'gbk',
    character_set_result = 'gbk',
    character_set_client = 'gbk';

漏洞发生的原因是执行了 ``set character_set_client = 'gbk';`` 之后，mysql就会认为客户端传过来的数据是gbk编码的，从而使用gbk去解码，而mysql_real_escape是在解码前执行的。但是直接用 ``set names 'gbk'`` 的话real_escape是不知道设置的数据的编码的，就会加 ``%5c`` 。此时server拿到数据解码  就认为提交的字符+%5c是gbk的一个字符，这样就产生漏洞了。

解决的办法有三种，第一种是把client的charset设置为binary，就不会做一次解码的操作。第二种是是 ``mysql_set_charset('gbk')`` ，这里就会把编码的信息保存在和数据库的连接里面，就不会出现这个问题了。
第三种就是用pdo。

还有一些其他的编码技巧，比如latin会弃掉无效的unicode，那么admin%32在代码里面不等于admin，在数据库比较会等于admin。

二次注入
--------------------------------
二次SQL注入（Second-Order SQL Injection）是一种特殊类型的SQL注入攻击。与一般的SQL注入攻击类似，攻击者会通过输入恶意的SQL语句来执行非法操作。而二次SQL注入则是指攻击者在应用程序中注入恶意的数据，然后等待应用程序将这些数据存储在数据库中。当应用程序再次从数据库中读取这些数据时，恶意数据就会被读取出来，并执行恶意操作。

例如，一个web应用程序可能会将用户输入的内容存储在数据库中，然后在后续的页面中将这些内容显示出来。如果攻击者在用户输入中注入了恶意SQL语句，那么这些语句会被存储在数据库中。当应用程序从数据库中读取这些内容并在后续的逻辑中使用时，恶意SQL语句就有可能被执行，从而导致攻击成功。

与一般的SQL注入攻击相比，二次SQL注入攻击更加难以防范和检测，因为攻击者并不直接向应用程序发送恶意SQL语句，而是将其存储在数据库中等待应用程序读取。因此，防止二次SQL注入攻击需要采取一些特殊的防御措施，例如对输入数据进行更加严格的过滤和转义处理，使用预编译等。