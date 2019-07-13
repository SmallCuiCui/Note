### GET和POST的区别

#### HTTP请求的八种方式

1.OPTIONS 返回服务器所支持的请求方法

2.GET 向服务器获取指定资源

3.HEAD 与GET一致，只不过响应体不返回，只返回响应头

4.POST 向服务器提交数据，数据放在请求体里

5.PUT 与POST相似，只是具有幂等特性，一般用于更新

6.DELETE 删除服务器指定资源

7.TRACE 回显服务器端收到的请求，测试的时候会用到这个

8.CONNECT 预留，暂无使用

#### GET和POST的区别

1.GET在浏览器回退时是无害的，而POST会再次提交请求。

2.GET产生的URL地址可以被Bookmark，而POST不可以。

3.GET请求会被浏览器主动cache，而POST不会，除非手动设置。

4.GET请求只能进行url编码，而POST支持多种编码方式。

5.GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

6.GET请求在URL中传送的参数是有长度限制的，而POST没有。

7.对参数的数据类型，GET只接受ASCII字符，而POST没有限制。

8.GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

**9.GET产生一个TCP数据包；POST产生两个TCP数据包。**

对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；

而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

