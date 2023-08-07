<!-- 
time: 2023/8/7
category: 技术
tag:
    - HTTP
    - 前端
-->

# 5种HTTP传递数据的方式

对于前端来说，后端主要是提供 http 接口来传输数据，而这种数据传输的方式主要有 5 种：

+ URL 传参
+ query 传参
+ form-urlencoded 传参
+ form-data 传参
+ json 传参

## URL 传参

直接将参数写在 URL 中，例如：

```ts
http://localhost:3000/api/user/1
```

这里的 1 就是路径中的参数（url param），服务端框架或者单页应用的路由都支持从 url 中取出参数。

## query 传参

query 传参是将参数写在 url 的 query 中，例如：

```ts
http://localhost:3000/api/user?name=jack&age=18
```

这里的 name 和 age 就是 query 参数，如果有多个参数，可以用 & 连接。其中非英文的字符和一些特殊字符要经过编码，可以使用 encodeURIComponent 的 api 来编码：

```ts
const query = '?name=' + encodeURIComponent('张三') + '&age=18'; // ?name=%E5%BC%A0%E4%B8%89&age=18
```

或者使用封装了一层的 query-string 库来处理。

```ts
const queryString = require('query-string');
const query = queryString.stringify({
    name: '张三',
    age: 18
});
// name=%E5%BC%A0%E4%B8%89&age=18
```

## form-urlencoded传参

直接使用form表单形式提交数据就是form-urlencoded传参，与query传参的区别是，form-urlencoded传参是将参数写在请求体中，且将content-type设置为application/x-www-form-urlencoded，而query传参是将参数写在url中。

```ts
POST /api/user HTTP/1.1
Host: localhost:3000
Content-Type: application/x-www-form-urlencoded

name=jack&age=18
```

因此，如果使用form-urlencoded传参，依旧要使用encodeURIComponent来编码非英文字符和特殊字符。所以文件上传这种二进制数据就不适合使用form-urlencoded传参。

## form-data传参

form-data传参是将参数写在请求体中，不再使用&来连接，而是使用分隔符 boundary （------一串数字）来分隔参数，且将content-type设置为multipart/form-data。因为不是url编码，所以不需要对参数进行编码。

这种方式适合传输文件，因为文件是二进制数据，不适合使用url编码。

```ts
POST /api/user HTTP/1.1
Host: localhost:3000
Content-Type: multipart/form-data; boundary=----332423525235

----332423525235
Content-Disposition: form-data; name="name"

jack
----332423525235

Content-Disposition: form-data; name="age"

18
----332423525235--
```

## json传参

form-urlencoded传参需要对参数进行编码，而form-data传参需要使用分隔符来分隔参数，这两种方式都不太方便，所以json传参就出现了，json传参是将参数写在请求体中，且将content-type设置为application/json。

```ts
POST /api/user HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{
    "name": "jack",
    "age": 18
}
```




