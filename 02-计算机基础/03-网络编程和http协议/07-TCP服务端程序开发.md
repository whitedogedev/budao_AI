# TCP服务端程序开发

**学习目标**

* 能够写出TCP服务端应用程序接收和发送消息

---

### 1. 开发 TCP 服务端程序开发步骤回顾

1. 创建服务端端套接字对象
2. 绑定端口号
3. 设置监听
4. 等待接受客户端的连接请求
5. 接收数据
6. 发送数据
7. 关闭套接字

### 2. socket 类的介绍

导入 socket 模块  
**import socket**

创建服务端 socket 对象  
**socket.socket\(AddressFamily, Type\)                  
**

**参数说明:**

* AddressFamily 表示IP地址类型, 分为IPv4和IPv6
* Type 表示传输协议类型

**方法说明:**

* bind\(\(host, port\)\) 表示绑定端口号, host 是 ip 地址，port 是端口号，ip 地址一般不指定，表示本机的任何一个ip地址都可以。
* listen
  \(backlog\) 表示设置监听，backlog参数表示最大等待建立连接的个数。
* accept\(\) 表示等待接受客户端的连接请求
* send\(data\) 表示发送数据，data 是二进制数据
* recv\(buffersize\) 表示接收数据, buffersize 是每次接收数据的长度

### 3. TCP 服务端程序开发示例代码

```py
import socket

if __name__ == '__main__':
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用，让程序退出端口号立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True) 
    # 给程序绑定端口号
    tcp_server_socket.bind(("", 8989))
    # 设置监听
    # 128:最大等待建立连接的个数， 提示： 目前是单任务的服务端，同一时刻只能服务与一个客户端，后续使用多任务能够让服务端同时服务与多个客户端，
    # 不需要让客户端进行等待建立连接
    # listen后的这个套接字只负责接收客户端连接请求，不能收发消息，收发消息使用返回的这个新套接字来完成
    tcp_server_socket.listen(128)
    # 等待客户端建立连接的请求, 只有客户端和服务端建立连接成功代码才会解阻塞，代码才能继续往下执行
    # 1. 专门和客户端通信的套接字： service_client_socket
    # 2. 客户端的ip地址和端口号： ip_port
    service_client_socket, ip_port = tcp_server_socket.accept()
    # 代码执行到此说明连接建立成功
    print("客户端的ip地址和端口号:", ip_port)
    # 接收客户端发送的数据, 这次接收数据的最大字节数是1024
    recv_data = service_client_socket.recv(1024)
    # 获取数据的长度
    recv_data_length = len(recv_data)
    print("接收数据的长度为:", recv_data_length)
    # 对二进制数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收客户端的数据为:", recv_content)
    # 准备发送的数据
    send_data = "ok, 问题正在处理中...".encode("gbk")
    # 发送数据给客户端
    service_client_socket.send(send_data)
    # 关闭服务与客户端的套接字， 终止和客户端通信的服务
    service_client_socket.close()
    # 关闭服务端的套接字, 终止和客户端提供建立连接请求的服务
    tcp_server_socket.close()
```

**执行结果:**

```py
客户端的ip地址和端口号: ('172.16.47.209', 52472)
接收数据的长度为: 5
接收客户端的数据为: hello
```

**说明:**

当客户端和服务端建立连接后，**服务端程序退出后端口号不会立即释放，需要等待大概1-2分钟。**

解决办法有两种:

1. 更换服务端端口号
2. 设置端口号复用\(推荐大家使用\)，也就是说让服务端程序退出后端口号立即释放。

设置端口号复用的代码如下:

```py
# 参数1: 表示当前套接字
# 参数2: 设置端口号复用选项
# 参数3: 设置端口号复用选项对应的值
tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
```

**网络调试助手充当客户端程序:**

![网络调试助手](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z97nmbgoj20xe0u0tcb.jpg)

### 4. 小结

1. 导入socket模块
2. 创建TCP套接字‘socket’
   * 参数1: ‘AF\_INET’, 表示IPv4地址类型
   * 参数2: ‘SOCK\_STREAM’, 表示TCP传输协议类型
3. 绑定端口号‘bind’
   * 参数: 元组, 比如:\(ip地址, 端口号\)
4. 设置监听‘listen’
   * 参数:    最大等待建立连接的个数
5. 等待接受客户端的连接请求‘accept’
6. 发送数据‘send’
   * 参数: 要发送的二进制数据， 注意: 字符串需要使用encode\(\)方法进行编码
7. 接收数据‘recv’
   * 参数: 表示每次接收数据的大小，单位是字节，注意: 解码成字符串使用decode\(\)方法
8. 关闭套接字‘socket’表示通信完成



