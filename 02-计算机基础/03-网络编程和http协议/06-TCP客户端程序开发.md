# TCP 客户端程序开发

**学习目标**

* 能够写出 TCP 客户端应用程序发送和接收消息

---

### 1. 开发 TCP 客户端程序开发步骤回顾

1. 创建客户端套接字对象
2. 和服务端套接字建立连接
3. 发送数据
4. 接收数据
5. 关闭客户端套接字

### 2. socket 类的介绍

导入 socket 模块  
**import socket**

创建客户端 socket 对象  
**socket.socket\(AddressFamily, Type\)  
**

**参数说明:**

* AddressFamily 表示IP地址类型, 分为IPv4和IPv6
* Type 表示传输协议类型

**方法说明:**

* connect\(\(host, port\)\) 表示和服务端套接字建立连接, host是服务器ip地址，port是应用程序的端口号
* send\(data\) 表示发送数据，data是二进制数据
* recv\(buffersize\) 表示接收数据, buffersize是每次接收数据的长度

### 3. TCP 客户端程序开发示例代码

```py
import socket


if __name__ == '__main__':
    # 创建tcp客户端套接字
    # 1. AF_INET：表示ipv4
    # 2. SOCK_STREAM: tcp传输协议
    tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 和服务端应用程序建立连接
    tcp_client_socket.connect(("192.168.131.62", 8080))
    # 代码执行到此，说明连接建立成功
    # 准备发送的数据
    send_data = "你好服务端，我是客户端小黑!".encode("gbk")
    # 发送数据
    tcp_client_socket.send(send_data)
    # 接收数据, 这次接收的数据最大字节数是1024
    recv_data = tcp_client_socket.recv(1024)
    # 返回的直接是服务端程序发送的二进制数据
    print(recv_data)
    # 对数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收服务端的数据为:", recv_content)
    # 关闭套接字
    tcp_client_socket.close()
```

**执行结果:**

```py
b'hello'
接收服务端的数据为: hello
```

**说明**

1. str.encode\(编码格式\) 表示把字符串编码成为二进制
2. data.decode\(编码格式\) 表示把二进制解码成为字符串

**网络调试助手充当服务端程序:**

![网络调试助手](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z96pxou5j20p00mmq5v.jpg)

### 4. 小结

1. 导入socket模块
2. 创建TCP套接字‘socket’
   * 参数1: ‘AF\_INET’, 表示IPv4地址类型
   * 参数2: ‘SOCK\_STREAM’, 表示TCP传输协议类型
3. 发送数据‘send’
   * 参数1: 要发送的二进制数据， 注意: 字符串需要使用encode\(\)方法进行编码
4. 接收数据‘recv’
   * 参数1: 表示每次接收数据的大小，单位是字节
5. 关闭套接字‘socket’表示通信完成



