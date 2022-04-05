# HTTP响应报文

**学习目标**

* 能够知道HTTP响应报文的结构

---

### 1. HTTP响应报文分析

**HTTP 响应报文效果图:**

![HTTP 响应报文](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9jwtu0zj20ns0qxwi5.jpg)  

**响应报文说明:**

```http
--- 响应行/状态行 ---
HTTP/1.1 200 OK # HTTP协议版本 状态码 状态描述
--- 响应头 ---
Server: Tengine # 服务器名称
Content-Type: text/html; charset=UTF-8 # 内容类型
Transfer-Encoding: chunked # 发送给客户端内容不确定内容长度，发送结束的标记是0\r\n, Content-Length表示服务端确定发送给客户端的内容大小，但是二者只能用其一。
Connection: keep-alive # 和客户端保持长连接
Date: Fri, 23 Nov 2018 02:01:05 GMT # 服务端的响应时间
--- 空行 ---
--- 响应体 ---
<!DOCTYPE html><html lang=“en”> …</html> # 响应给客户端的数据
```

**原始响应报文说明:**

```http
HTTP/1.1 200 OK\r\n
Server: Tengine\r\n
Content-Type: text/html; charset=UTF-8\r\n
Transfer-Encoding: chunked\r\n
Connection: keep-alive\r\n
Date: Fri, 23 Nov 2018 02:01:05 GMT\r\n
\r\n(响应头信息后面还有一个单独的’\r\n’不能省略)
<!DOCTYPE html><html lang=“en”> …</html>
```

**说明:**

每项数据之间使用:**\r\n**

### 2. HTTP 状态码介绍

HTTP 状态码是**用于表示web服务器响应状态的3位数字代码**。

| 状态码 | 说明 |
| :--- | :--- |
| 200 | 请求成功 |
| 307 | 重定向 |
| 400 | 错误的请求，请求地址或者参数有误 |
| 404 | 请求资源在服务器不存在 |
| 500 | 服务器内部源代码出现错误 |

### 3. 小结

* 一个HTTP响应报文是由**响应行、响应头、空行和响应体**4个部分组成。
    ![HTTP 响应报文](./imgs/响应报文.png) 
* 响应行是由三部分组成：**HTTP协议版本 状态码 状态描述**，最常见的状态码是200



