# 2.Linux权限相关操作

# 一、文件权限命令

**学习目标**

* 能够使用chmod命令完成文件权限的修改

---

### 1. chmod命令的介绍

| 命令  | 说明         |
| :---- | :----------- |
| chmod | 修改文件权限 |

chmod修改文件权限有两种方式:

* 字母法
* 数字法

### 2. chmod 字母法的使用

**角色说明:**

| 角色 | 说明                     |
| :--- | :----------------------- |
| u    | user, 表示该文件的所有者 |
| g    | group, 表示用户组        |
| o    | other, 表示其他用户      |
| a    | all, 表示所有用户        |

**权限设置说明:**

| 操作符 | 说明     |
| :----- | :------- |
| +      | 增加权限 |
| -      | 撤销权限 |
| =      | 设置权限 |

**权限说明:**

| 权限 | 说明       |
| :--- | :--------- |
| r    | 可读       |
| w    | 可写       |
| x    | 可执行     |
| -    | 无任何权限 |

**chmod命令字母法效果图:**

![chmod命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixjgborgj20d50bejtl.jpg)

**chmod命令同时设置多个角色的效果图:**

![chmod命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixjgs0xuj20x10c1jw3.jpg)

### 3. chmod 数字法的使用

数字法就是“rwx” 这些权限也可以用**数字来代替**

**权限说明:**

| 权限 | 说明                  |
| :--- | :-------------------- |
| r    | 可读，权限值是4       |
| w    | 可写，权限值是2       |
| x    | 可执行，权限值是1     |
| -    | 无任何权限，权限值是0 |

**chmod命令数字法效果图:**

![chmod命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixjg29vyj20dg09mmyz.jpg)

### 4. 小结

* 利用 **chmod** 命令可以控制文件的操作权限。
* 字母法格式: chmod 不同角色设置的权限 文件 
* 数字法格式: chmod 不同角色的权限值 文件名



# 二、获取管理员权限的相关命令

** 学习目标 **

* 能够知道切换到管理员用户的命令

---

### 1. sudo命令的使用

| 命令    | 说明                                                       |
| :------ | :--------------------------------------------------------- |
| sudo -s | 切换到root用户，获取管理员权限                             |
| sudo    | 某个命令的执行需要获取管理员权限可以在执行命令前面加上sudo |

**sudo -s效果图:**

![sudo命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwpplztlj20k004cq3a.jpg)

**sudo 命令效果图:**

![sudo命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwpt3cv4j20lo05qwfm.jpg)

**说明:**

* 如果只是某次操作需要使用管理员权限建议使用 sudo , 也就是说临时使用管理器权限。
* 如果大量操作都需要使用管理员权限 sudo –s, 但是操作需谨慎。

### 2. whoami命令的使用

| 命令   | 说明         |
| :----- | :----------- |
| whoami | 查看当前用户 |

**whoami 命令效果图:**

![whoami命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwprs4nij20i108m0tx.jpg)

### 3. exit命令的使用

| 命令 | 说明         |
| :--- | :----------- |
| exit | 退出登录用户 |

**exit 命令的效果图:**

![exit命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwpqwsyej20e40bn402.jpg)

**说明:**

* 如果是切换后的登陆用户，退出则返回上一个登陆账号。
* 如果是终端界面，退出当前终端。

### 4. who命令的使用

| 命令 | 说明               |
| :--- | :----------------- |
| who  | 查看所有的登录用户 |

**who 命令的效果图:**

![who命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwpsgff7j20rn05wgmx.jpg)

### 5. passwd命令的使用

| 命令   | 说明                                             |
| :----- | :----------------------------------------------- |
| passwd | 修改用户密码，不指定用户默认修改当前登录用户密码 |

**passwd 命令的效果图:**

![passwd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwpq0yehj20f30bg0v2.jpg)

### 6. which命令的使用

| 命令  | 说明         |
| :---- | :----------- |
| which | 查看命令位置 |

**which 命令的效果图:**

![which命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0iwprbu8ij20gq08qwfw.jpg)

### 7. 关机和重启命令的使用

| 命令            | 说明     |
| :-------------- | :------- |
| shutdown –h now | 立刻关机 |
| reboot          | 重启     |

### 8. 小结

* sudo 是临时获取管理员权限
* sudo -s 是切换到管理员用户，一直使用管理员权限，需要谨慎操作。
* exit 是退出当前用户
* passwd 默认修改当前用户密码



# 三、用户相关操作

** 学习目标 **

* 能够知道创建用户的命令

---

### 1. 创建用户

| 命令    | 说明           |
| :------ | :------------- |
| useradd | 创建(添加)用户 |

**useradd命令选项:**

| 选项 | 说明                                                       |
| :--- | :--------------------------------------------------------- |
| -m   | 自动创建用户主目录,主目录的名字就是用户名                  |
| -g   | 指定用户所属的用户组，默认不指定会自动创建一个同名的用户组 |

**创建用户效果图:**

![useradd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixa9bncgj211m06e0tp.jpg)

**查看所有用户信息的文件效果图:**

![useradd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixaan5yoj20x2076q46.jpg)
![useradd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixacuy6jj214c0mqk2k.jpg)

**说明:**

* **useradd** 命令的使用需要使用管理员权限，前面需要加上 **sudo**
* 创建用户如果不指定用户组，默认会自动创建一个同名的用户组
* 查看用户是否创建成功，可以查看**/etc/passwd**这个文件
* 查看用户组是否创建成功，可以查看**/etc/group**这个文件

**passwd文件中的每项信息说明,以root:x:0:0:root:/root:/bin/bash为例:**

* 第一个：用户名
* 第二个：密码占位符
* 第三个：uid, 用户id
* 第四个：gid, 用户所在组id
* 第五个：用户描述, 可选，
* 第六个：用户的主目录所在位置
* 第七个：用户所用 shell 的类型，一般由bash或者sh，默认不设置是sh类型

**group文件中的每项信息说明, 以laowang:x:1001:为例:**

* 第一个：用户组名
* 第二个：用户组密码占位符，一般Linux系统的用户组都没有密码的
* 第三个：组id

** id命令查看用户信息: **

| 命令 | 说明         |
| :--- | :----------- |
| id   | 查看用户信息 |

** id命令效果图: ** 
![id命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixadhyepj20uy03owf5.jpg)

** 每项信息说明: **

uid=1001(laowang) gid=1001(laowang) 组=1001(laowang)

* 第一个: uid 表示用户id
* 第二个: gid 表示用户组id
* 第三个: 组 表示用户所在的用户组

### 2. 设置密码

给其它用户设置密码，需要使用:** sudo passwd 用户名**

** 设置密码效果图: **
![useradd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixaa6zdlj20v606e0u2.jpg)


### 3. 切换用户

| 命令 | 说明     |
| :--- | :------- |
| su   | 切换用户 |

**语法格式: su - 用户名**

** 切换用户效果图: **

![su命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixab394wj20wa05gt9e.jpg)

** 在laowang用户使用sudo -s效果图: **

### 4. 删除用户

| 命令    | 说明     |
| :------ | :------- |
| userdel | 删除用户 |

** userdel命令选项: **

| 选项      | 说明                                               |
| :-------- | :------------------------------------------------- |
| -r 用户名 | 删除用户主目录，必须要设置，否则用户主目录不会删除 |

** 删除用户效果图: **

![userdel命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixabm2gdj20xy040jsc.jpg)

** id查看用户信息效果图: **

![userdel命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixac5cuvj20yc038gm7.jpg)

** 查看group文件信息效果图: **

![userdel命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixa8k82mj211k0mojv3.jpg)

** 说明: **

* 删除用户，默认同名的用户组也会被删除


### 5. 小结

* 创建用户命令格式: **sudo useradd -m 用户名**, 默认会创建一个同名的用户组。
* 查看用户信息使用 **id** 命令或者 **/etc/passwd文件**
* 给用户设置密码使用 **sudo passwd 用户名**
* 切换用户使用 **su - 用户名**
* 删除用户使用 **sudo userdel -r 用户名**，默认会删除同名的用户组。 



# 四、用户组相关操作

** 学习目标 **

* 能够知道创建用户组的命令

---


### 1. 创建用户组

| 命令     | 说明             |
| :------- | :--------------- |
| groupadd | 创建(添加)用户组 |

**创建用户组效果图:**

![groupadd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixgsrr2lj20o4056dgn.jpg)

### 2. 创建用户并指定用户组

**创建用户并指定用户组效果图:**

![groupadd命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixgrz4cvj20wy05kgmy.jpg)

### 3. 删除用户组

| 命令     | 说明       |
| :------- | :--------- |
| groupdel | 删除用户组 |

**删除用户组效果图:**

![groupdel命令](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ixgsck2jj20zk09qad6.jpg)

**说明:**

* 如果用户组下面有用户先删除用户在删除用户组

### 4. 小结

* 创建用户组使用: **sudo groupadd 用户组名**
* 创建用户并指定用户组使用: **sudo useradd -m -g 用户组 用户名**
* 删除用户组使用: **sudo groupdel 用户组名**
* 查看用户组信息使用 **/etc/group文件**



















