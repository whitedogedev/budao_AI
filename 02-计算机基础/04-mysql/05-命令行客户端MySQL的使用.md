# 命令行客户端MySQL的使用

**学习目标**

* 能够知道使用命令行连接数据库命令
* 能够写出增、删、改、查的SQL语句

---

### 1. 登录和登出数据库

**登录数据库:**

输入下面命令:

```sql
mysql -uroot -p
```

**说明:**

* -u 后面是登录的用户名
* -p 后面是登录密码, 如果不填写, 回车之后会提示输入密码

**登录数据库效果图:**

![登录数据库](https://tva1.sinaimg.cn/large/e6c9d24ely1h109947hfpj214e0e4juu.jpg)

**登录成功后, 输入如下命令查看效果：**

```sql
# 显示当前时间
select now();
```

**登出\(退出\)数据库:**

```sql
quit 或 exit 或 ctrl + d
```

### 2. 数据库操作的SQL语句

1. 查看所有数据库

   ```sql
   show databases;
   ```

2. 创建数据库

   ```sql
   create database 数据库名 charset=utf8;
   例：
   create database python charset=utf8;
   ```

3. 使用数据库

   ```sql
   use 数据库名;
   ```

4. 查看当前使用的数据库

   ```sql
   select database();
   ```

5. 删除数据库-慎重

   ```sql
   drop database 数据库名;
   例：
   drop database python;
   ```

### 3. 表结构操作的SQL语句

1. 查看当前数据库中所有表

   ```sql
   show tables;
   ```

2. 创建表

   ```sql
   create table students(
    id int unsigned primary key auto_increment not null,
    name varchar(20) not null,
    age tinyint unsigned default 0,
    height decimal(5,2),
    gender enum('男','女','人妖','保密')
   );
   ```

   **说明:**

   ```sql
   create table 表名(
   字段名称 数据类型  可选的约束条件,
   column1 datatype contrai,
   ...
   );
   ```

3. 修改表-添加字段

   ```sql
   alter table 表名 add 列名 类型 约束;
   例：
   alter table students add birthday datetime;
   ```

4. 修改表-修改字段类型

   ```sql
   alter table 表名 modify 列名 类型 约束;
   例：
   alter table students modify birthday date not null;
   ```
   **说明:**
   
   * modify: 只能修改字段类型或者约束，不能修改字段名

5. 修改表-修改字段名和字段类型

   ```sql
   alter table 表名 change 原名 新名 类型及约束;
   例：
   alter table students change birthday birth datetime not null;
   ```
   **说明:**
   
   * change: 既能对字段重命名又能修改字段类型还能修改约束

6. 修改表-删除字段

   ```sql
   alter table 表名 drop 列名;
   例：
   alter table students drop birthday;
   ```

7. 查看创表SQL语句

   ```sql
   show create table 表名;
   例：
   show create table students;
   ```

8. 查看创库SQL语句

   ```sql
   show create database 数据库名;
   例：
   show create database mytest;
   ```

9. 删除表

   ```sql
   drop table 表名;
   例：
   drop table students;
   ```

### 4. 表数据操作的SQL语句

1. 查询数据

   ```sql
   -- 1. 查询所有列
   select * from 表名;
   例：
   select * from students;
   -- 2. 查询指定列
   select 列1,列2,... from 表名;
   例：
   select id,name from students;
   ```

2. 添加数据

   ```sql
   -- 1. 全列插入：值的顺序与表结构字段的顺序完全一一对应
   insert into 表名 values (...)
   例:
   insert into students values(0, 'xx', default, default, '男');
   -- 2. 部分列插入：值的顺序与给出的列顺序对应
   insert into 表名 (列1,...) values(值1,...)
   例:
   insert into students(name, age) values('王二小', 15);
   -- 3. 全列多行插入
   insert into 表名 values(...),(...)...;
   例:
   insert into students values(0, '张飞', 55, 1.75, '男'),(0, '关羽', 58, 1.85, '男');
   -- 4. 部分列多行插入
   insert into 表名(列1,...) values(值1,...),(值1,...)...;
   例：
   insert into students(name, height) values('刘备', 1.75),('曹操', 1.6);
   ```

   **说明:**

   * 主键列是自动增长，但是在全列插入时需要占位，通常使用空值\(0或者null或者default\)
   * 在全列插入时，如果字段列有默认值可以使用 default 来占位，插入后的数据就是之前设置的默认值

3. 修改数据

   ```sql
   update 表名 set 列1=值1,列2=值2... where 条件
   例：
   update students set age = 18, gender = '女' where id = 6;
   ```

4. 删除数据

   ```sql
   delete from 表名 where 条件
   例：
   delete from students where id=5;
   ```

   问题:

   上面的操作称之为物理删除，一旦删除就不容易恢复，我们可以使用逻辑删除的方式来解决这个问题。

   ```sql
   -- 添加删除表示字段，0表示未删除 1表示删除
   alter table students add isdelete bit default 0;
   -- 逻辑删除数据
   update students set isdelete = 1 where id = 8;
   ```

   **说明:**

   * 逻辑删除，本质就是修改操作

### 5. 小结

* 登录数据库: mysql -uroot -p
* 退出数据库: quit 或者 exit 或者 ctr + d
* 创建数据库: create database 数据库名 charset=utf8;
* 使用数据库: use 数据库名;
* 删除数据库: drop database 数据库名;
* 创建表: create table 表名(字段名 字段类型 约束, ...);
* 修改表-添加字段: alter table 表名 add 字段名 字段类型 约束
* 修改表-修改字段类型: alter table 表名 modify 字段名 字段类型 约束
* 修改表-修改字段名和字段类型: alter table 表名 change 原字段名 新字段名 字段类型 约束
* 修改表-删除字段: alter table 表名 drop 字段名;
* 删除表: drop table 表名;
* 查询数据: select * from 表名; 或者 select 列1,列2,... from 表名;
* 插入数据: insert into 表名 values (...) 或者 insert into 表名 (列1,...) values(值1,...)
* 修改数据: update 表名 set 列1=值1,列2=值2... where 条件
* 删除数据: delete from 表名 where 条件











