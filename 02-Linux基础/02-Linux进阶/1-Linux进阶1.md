# Linux进阶1

# 一、重定向命令

**学习目标**

* 能够使用重定向命令将终端显示内容重定向到文件

---

### 1. 重定向命令的介绍

重定向也称为输出重定向，把在终端执行命令的结果保存到目标文件。

### 2. 重定向命令的使用

| 命令 | 说明                                                       |
| :--- | :--------------------------------------------------------- |
| >    | 如果文件存在会覆盖原有文件内容，相当于文件操作中的‘w’模式  |
| >>   | 如果文件存在会追加写入文件末尾，相当于文件操作中的‘a’ 模式 |

**重定向命令效果图:**

![重定向命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw017tq3j20p0045aaj.jpg)

![重定向命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw00s81hj20p007ygmv.jpg)

**说明:**

只要在终端能显示信息的命令都可以使用重定向，比如: tree

### 3. 小结

* 终端内容保存到文件使用重定向有两种方式: **>** 和 **>>**
* **>** 表示每次只写入最新的数据，原有数据不保留。
* **>>** 表示每次在原有数据的基础上进行追加，原有数据会保留。





# 二、查看文件内容命令

**学习目标**

* 能够说出查看大文件分屏显示使用的命令

---

### 1. 查看文件内容命令的使用

| 命令 | 说明             |
| :--- | :--------------- |
| cat  | 查看小型文件     |
| more | 分屏查看大型文件 |

**cat命令的效果图**

![cat命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ivzjvgioj20fo0780to.jpg)

**说明:**

* cat命令结合重定向可以完成多个文件的合并
* gedit 文件编辑命令，可以查看和编辑文件

**more命令的效果图**

当查看内容信息过长无法在一屏上显示时，可以使用 more 命令在终端分屏显示文件内容。

![more命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ivziuzo6j20fq036weq.jpg)

**操作键说明:**

| 操作键 | 说明           |
| :----- | :------------- |
| 空格   | 显示下一屏信息 |
| 回车   | 显示下一行信息 |
| b      | 显示上一屏信息 |
| f      | 显示下一屏信息 |
| q      | 退出           |

### 2. 管道(|)命令的使用

管道(|)：一个命令的输出可以通过管道做为另一个命令的输入，可以理解成是一个容器，存放在终端显示的内容。

**管道命令的效果图:**

![管道命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ivzjdryyj20fq07bwg7.jpg)

**说明:**

管道(|)一般结合 more 命令使用，主要是分配查看终端显示内容。

### 3. 小结

* 查看小文件使用 **cat** 命令
* 分屏查看大型文件使用 **more** 命令，
* 查看终端显示内容并分屏展示，使用 **管道(|)** 结合 **more** 命令。





# 三、链接命令

**学习目标**

* 能够说出软链接的创建方式
* 能够说出硬链接的创建方式

---

### 1. 链接命令的介绍

链接命令是创建链接文件，链接文件分为:

* 软链接
* 硬链接

| 命令  | 说明       |
| :---- | :--------- |
| ln -s | 创建软链接 |
| ln    | 创建硬链接 |

### 2. 软链接

类似于**Windows下的快捷方式**，当一个源文件的目录层级比较深，我们想要方便使用它可以给源文件创建一个软链接。

**软链接效果图:**

![软链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw154w22j20p007440n.jpg)  
![软链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw129p33j20p00f144g.jpg)  
![软链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw16yfd5j20p00e2jwt.jpg)  
![软链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw15h3fuj20p009g77f.jpg)  
![软链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw147j85j20p00enjwg.jpg)

**注意点:**

* **如果软链接和源文件不在同一个目录，源文件要使用绝对路径，不能使用相对路径。**
* **删除源文件则软链接失效**
* **可以给目录创建软链接**

### 3. 软链接小结

* 软链接的作用是方便文件的快速访问，比如:给一个复杂路径下的文件创建一个软链接，以后就可以通过软链接完成快速访问操作。
* 创建软链接命令格式: ln -s 源文件路径\(使用绝对路径\) 软链接

### 4. 硬链接

类似于**源文件的一个别名**，也就是说这两个名字指向的是同一个文件数据。

![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw0zzl7rj20p006k3z5.jpg)

**硬链接效果图:**

![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw10alftj20p007w0v5.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw160cyfj20p005mmyg.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw11v0qnj20p0052abe.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw10tuvzj20p00cwq7g.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw119mqwj20p00jztg8.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw137bl8j20p00av0wl.jpg)  
![硬链接](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw12rzgxj20p006ydi8.jpg)

**注意点:**

* **创建硬链接使用相对路径和绝对路径都可以**
* **删除源文件，硬链接还可以访问到数据。**
* **创建硬链接，硬链接数会加1，删除源文件或者硬链接，硬链接数会减1。**
* **创建软链接，硬链接数不会加1**
* **不能给目录创建硬链接**

**硬链接数:**

**硬链接数就是文件数据被文件名使用的次数, 好比引用计数**

### 5. 硬链接小结

* 硬链接的作用是可以给重要文件创建硬链接，能够防止文件数据被误删。
* 删除源文件，软链接失效，但是硬链接依然可以使用。
* 创建硬链接命令格式: ln 源文件路径 硬链接



# 四、文本搜索命令

**学习目标**

* 能够知道文本搜索使用的命令

---

### 1. grep命令的使用

| 命令 | 说明     |
| :--- | :------- |
| grep | 文本搜索 |

**grep命令效果图:**

![grep命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5tmccij20dw038weo.jpg)  


### 2. grep命令选项的使用

| 命令选项 | 说明                       |
| :------- | :------------------------- |
| -i       | 忽略大小写                 |
| -n       | 显示匹配行号               |
| -v       | 显示不包含匹配文本的所有行 |

**-i命令选项效果图:**

![grep命令选项](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5uz8b3j20dw04kdgb.jpg)  

**-n命令选项效果图:**

![grep命令选项](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5ve2auj20dw04kgm3.jpg)  

**-v命令选项效果图:**

![grep命令选项](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5u15xyj20dw06f74z.jpg) 

### 3. grep命令结合正则表达式的使用

| 正则表达式 | 说明                   |
| :--------- | :--------------------- |
| ^          | 以指定字符串开头       |
| $          | 以指定字符串结尾       |
| .          | 匹配一个非换行符的字符 |

**正则表达式‘^’的效果图:**

![grep正则](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5unv9zj20dw05bq3h.jpg) 

**正则表达式‘$’的效果图:**

![grep正则](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5sqt2lj20dw069jry.jpg) 

**正则表达式‘.’的效果图:**

![grep正则](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iw5t3qo6j20dw04e74m.jpg) 

### 4. 扩展

* grep 命令还可以文本搜索管道中的内容，比如: **ls / | grep ‘lib’** 
* 在使用  **grep**  命令的时候还可以省略搜索内容的引号，比如: **ls / | grep lib**， **grep hello 1.txt **

### 5. 小结

* grep 命令是完成文本搜索操作的
* 文本搜索的命令格式: grep 选项 文本搜索内容



# 五、查找文件命令

**学习目标**

* 能够说出查找文件使用的命令

---

### 1. find命令及选项的使用

| 命令 | 说明                             |
| :--- | :------------------------------- |
| find | 在指定目录下查找文件\(包括目录\) |

** find命令选项: **

| 选项  | 说明                           |
| :---- | :----------------------------- |
| -name | 根据文件名\(包括目录名\)字查找 |

** find命令及选项的效果图: **

![find命令及选项](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwejalrjj20dw037mxl.jpg)

### 2. find命令结合通配符的使用

**通配符:**

是一种特殊语句，主要有星号\(\*\)和问号\(?\)，用来模糊搜索文件

| 通配符 | 说明                  |
| :----- | :-------------------- |
| \*     | 代表0个或多个任意字符 |
| ?      | 代表任意一个字符      |

**\*通配符的效果图:**

![通配符](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwekmk5oj20dw05o0tc.jpg)

**?通配符的效果图:**

![通配符](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iweizgbgj20dw03ndgc.jpg)

**扩展:**

通配符不仅能结合 **find** 命令使用，还可以结合其它命令使用, 比如: **ls、mv、cp** 等，这里需要注意只有 **find** 命令使用通配符需要加上引号。

**扩展效果图:**

![find扩展](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwejr09rj20dw0bz0u8.jpg)

### 3. 小结

* find命令是完成查找文件操作的
* 查找文件的命令格式: find 指定查找目录 -name "文件名"

# 六、压缩和解压缩命令

**学习目标**

* 能够使用tar命令完成文件的压缩和解压缩

---

### 1. 压缩格式的介绍

Linux默认支持的压缩格式:

* .gz
* .bz2
* .zip

**说明:**

* .gz和.bz2的压缩包需要使用tar命令来压缩和解压缩
* .zip的压缩包需要使用zip命令来压缩，使用unzip命令来解压缩

**压缩目的:**

* 节省磁盘空间

### 2. tar命令及选项的使用

| 命令 | 说明             |
| :--- | :--------------- |
| tar  | 压缩和解压缩命令 |

**tar命令选项:**

| 选项 | 说明                               |
| :--- | :--------------------------------- |
| -c   | 创建打包文件                       |
| -v   | 显示打包或者解包的详细信息         |
| -f   | 指定文件名称, 必须放到所有选项后面 |
| -z   | 压缩或解压缩(.gz)                  |
| -j   | 压缩或解压缩(.bz2)                 |
| -x   | 解包                               |
| -C   | 解压缩到指定目录                   |

**压缩成.gz的效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwigsf26j20dw0bkgo6.jpg)  

**压缩成.bz2的效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwiimitbj20dw0cs0vl.jpg) 

**解压缩.gz的效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwifflp3j20dw04ft93.jpg) 

**解压缩.gz到指定目录效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwifox2fj20dw0awt9k.jpg) 

**解压缩.bz2的效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwij4ql3j20dw04u74r.jpg) 

**解压缩.bz2到指定目录效果图:**

![tar命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwig6j9hj20dw0arjs8.jpg) 

### 3. zip和unzip命令及选项的使用

| 命令  | 说明               |
| :---- | :----------------- |
| zip   | 压缩成.zip格式文件 |
| unzip | 解压缩.zip格式文件 |

**unzip命令选项:**

| 选项 | 说明             |
| :--- | :--------------- |
| -d   | 解压缩到指定目录 |

**压缩成.zip的效果图:**

![zip命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwii0s03j20dw0csn07.jpg)  

**解压缩.gz的效果图:**

![unzip命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwih41y2j20dw049mxl.jpg) 

**解压缩.zip到指定目录效果图:**

![unzip命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwijmiakj20dw0a8zl4.jpg) 


**说明:**

* 压缩文件尽量使用.gz格式，因为占用空间较少
* 使用zip命令压缩的文件占用空间比较多, 但是比较通用，操作更加简单。

### 4. 小结

* .gz和.bz2的压缩文件使用tar命令来完成压缩和解压缩
* .zip的压缩文件使用zip和unzip命令来完成压缩和解压缩































