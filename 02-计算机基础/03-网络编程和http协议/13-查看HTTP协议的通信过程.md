# 查看HTTP协议的通信过程

**学习目标**

* 能够使用谷歌浏览器的开发者工具查看HTTP协议的通信过程

---

### 1. 谷歌浏览器开发者工具的使用

首先需要安装Google Chrome浏览器，然后Windows和Linux平台按**F12**调出开发者工具, mac OS选择 视图 -> 开发者 -> 开发者工具或者直接使用 **alt+command+i** 这个快捷键，还有一个多平台通用的操作就是在**网页右击选择检查**。

**开发者工具的效果图:**

![标签说明](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9fbsulzj20cs09o0sw.jpg)

**开发者工具的标签选项说明:**

* 元素（Elements）：用于查看或修改HTML标签
* 控制台（Console）：执行js代码
* 源代码（Sources）：查看静态资源文件，断点调试JS代码
* 网络（Network）：查看http协议的通信过程

**开发者工具使用效果图:**

![使用说明](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9faw6z5j20cs0eiq45.jpg)

**开发者工具的使用说明:**

1. 点击Network标签选项
2. 在浏览器的地址栏输入百度的网址，就能看到请求百度首页的http的通信过程
3. **这里的每项记录都是请求+响应的一次过程**


### 2. 查看HTTP协议的通信过程

**查看http请求信息效果图:**

![查看http请求信息](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9fakgwlj20cs0ehaaw.jpg)
![查看http请求信息](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9fcrt9bj20cs0eimyw.jpg)

**查看http响应信息效果图:**

![查看http请求信息](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9fbcs6cj20cs0ehab6.jpg)
![查看http请求信息](https://tva1.sinaimg.cn/large/e6c9d24ely1h0z9fccl9ej20cs0egwga.jpg)

### 3. 小结

* 谷歌浏览器的开发者工具是查看http协议的通信过程利器，通过Network标签选项可以查看每一次的请求和响应的通信过程，调出开发者工具的通用方法是在网页右击选择检查。
* 开发者工具的Headers选项总共有三部分组成:
 1. General: 主要信息
 2. Response Headers: 响应头
 3. Request Headers: 请求头
* Response选项是查看响应体信息的





