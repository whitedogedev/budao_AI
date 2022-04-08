# PyMySQL的使用

**学习目标**

* 能够使用PyMySQL完成数据库的增删改查

---

### 1. 思考

如何实现将100000条数据插入到MySQL数据库?

**答案:**

如果使用之前学习的MySQL客户端来完成这个操作，那么这个工作量无疑是巨大的，我们可以通过使用程序代码的方式去连接MySQL数据库，然后对MySQL数据库进行增删改查的方式，实现10000条数据的插入，像这样使用代码的方式操作数据库就称为数据库编程。

### 2. Python程序操作MySQL数据库

**安装pymysql第三方包:**

```
sudo pip3 install pymysql
```

**说明:**

* 安装命令使用 sudo pip3 install 第三方包名
* 卸载命令使用 sudo pip3 uninstall 第三方包
* **pip3 list** 查看使用pip命令安装的第三方包列表

**pymysql的使用:**

1. 导入 pymysql 包

   ```py
    import pymysql
   ```

2. 创建连接对象

   调用pymysql模块中的connect\(\)函数来创建连接对象,代码如下:

   ```py
    conn=connect(参数列表)

    * 参数host：连接的mysql主机，如果本机是'localhost'
    * 参数port：连接的mysql主机的端口，默认是3306
    * 参数user：连接的用户名
    * 参数password：连接的密码
    * 参数database：数据库的名称
    * 参数charset：通信采用的编码方式，推荐使用utf8
   ```

   **连接对象操作说明:**

   * 关闭连接 conn.close\(\)
   * 提交数据 conn.commit\(\)
   * 撤销数据 conn.rollback\(\)

3. 获取游标对象

   获取游标对象的目标就是要执行sql语句，完成对数据库的增、删、改、查操作。代码如下:

   ```py
    # 调用连接对象的cursor()方法获取游标对象   
    cur =conn.cursor()
   ```

   **游标操作说明:**

   * 使用游标执行SQL语句: execute\(operation \[parameters \]\) 执行SQL语句，返回受影响的行数，主要用于执行insert、update、delete、select等语句
   * 获取查询结果集中的一条数据:cur.fetchone\(\)返回一个元组, 如 \(1,'张三'\)
   * 获取查询结果集中的所有数据: cur.fetchall\(\)返回一个元组,如\(\(1,'张三'\),\(2,'李四'\)\)
   * 关闭游标: cur.close\(\),表示和数据库操作完成

4. pymysql完成数据的查询操作

   ```python
   import pymysql

   # 创建连接对象
   conn = pymysql.connect(host='localhost', port=3306, user='root', password='mysql',database='python', charset='utf8')

   # 获取游标对象
   cursor = conn.cursor()

   # 查询 SQL 语句
   sql = "select * from students;"
   # 执行 SQL 语句 返回值就是 SQL 语句在执行过程中影响的行数
   row_count = cursor.execute(sql)
   print("SQL 语句执行影响的行数%d" % row_count)

   # 取出结果集中一行数据,　例如:(1, '张三')
   # print(cursor.fetchone())

   # 取出结果集中的所有数据, 例如:((1, '张三'), (2, '李四'), (3, '王五'))
   for line in cursor.fetchall():
       print(line)

   # 关闭游标
   cursor.close()

   # 关闭连接
   conn.close()
   ```

5. pymysql完成对数据的增删改

   ```python
   import pymysql

   # 创建连接对象
   conn = pymysql.connect(host='localhost', port=3306, user='root', password='mysql',database='python', charset='utf8')

   # 获取游标对象
   cursor = conn.cursor()

   try:
       # 添加 SQL 语句
       # sql = "insert into students(name) values('刘璐'), ('王美丽');"
       # 删除 SQ L语句
       # sql = "delete from students where id = 5;"
       # 修改 SQL 语句
       sql = "update students set name = '王铁蛋' where id = 6;"
       # 执行 SQL 语句
       row_count = cursor.execute(sql)
       print("SQL 语句执行影响的行数%d" % row_count)
       # 提交数据到数据库
       conn.commit()
   except Exception as e:
       # 回滚数据， 即撤销刚刚的SQL语句操作
       conn.rollback()

   # 关闭游标
   cursor.close()

   # 关闭连接
   conn.close()
   ```

   **说明:**

   * conn.commit\(\) 表示将修改操作提交到数据库
   * conn.rollback\(\) 表示回滚数据

6. 防止SQL注入

   什么是SQL注入?

   用户提交带有恶意的数据与SQL语句进行字符串方式的拼接，从而影响了SQL语句的语义，最终产生数据泄露的现象。

   如何防止SQL注入?

   SQL语句参数化

   * SQL语言中的参数使用%s来占位，此处不是python中的字符串格式化操作
   * 将SQL语句中%s占位所需要的参数存在一个列表中，把参数列表传递给execute方法中第二个参数

   **防止SQL注入的示例代码:**

   ```python
   from pymysql import connect

   def main():

       find_name = input("请输入物品名称：")

       # 创建Connection连接
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='jing_dong',charset='utf8')
       # 获得Cursor对象
       cs1 = conn.cursor()

       # 非安全的方式
       # 输入 ' or 1 = 1 or '   (单引号也要输入)
       # sql = "select * from goods where name='%s'" % find_name
       # print("""sql===>%s<====""" % sql)
       # # 执行select语句，并返回受影响的行数：查询所有数据
       # count = cs1.execute(sql)

       # 安全的方式
       # 构造参数列表
       params = [find_name]
       # 执行select语句，并返回受影响的行数：查询所有数据
       count = cs1.execute("select * from goods where name=%s", params)
       # 注意：
       # 如果要是有多个参数，需要进行参数化
       # 那么params = [数值1, 数值2....]，此时sql语句中有多个%s即可
       # %s 不需要带引号

       # 打印受影响的行数
       print(count)
       # 获取查询的结果
       # result = cs1.fetchone()
       result = cs1.fetchall()
       # 打印查询的结果
       print(result)
       # 关闭Cursor对象
       cs1.close()
       # 关闭Connection对象
       conn.close()

   if __name__ == '__main__':
       main()
   ```

   **说明:**

   * execute方法中的 %s 占位不需要带引号 

### 3. 小结

1. 导包

   ```py
    import pymysql
   ```

2. 创建连接对象

   ```py
    pymysql.connect(参数列表)
   ```

3. 获取游标对象

   ```py
    cursor =conn.cursor()
   ```

4. 执行SQL语句

   ```py
    row_count = cursor.execute(sql)
   ```

5. 获取查询结果集

   ```py
    result = cursor.fetchall()
   ```

6. 将修改操作提交到数据库

   ```py
    conn.commit()
   ```

7. 回滚数据

   ```py
    conn.rollback()
   ```

8. 关闭游标

   ```py
    cursor.close()
   ```

9. 关闭连接

   ```py
    conn.close()
   ```



