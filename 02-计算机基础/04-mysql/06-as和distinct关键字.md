# as和distinct关键字

**学习目标**

* 能够知道去除重复数据行的关键字

---

### 1. as关键字

在使用SQL语句显示结果的时候，往往在屏幕显示的字段名并不具备良好的可读性，此时可以使用 as 给字段起一个别名。

   1. 使用 as 给字段起别名

   ```sql
   select id as 序号, name as 名字, gender as 性别 from students;
   ```

   2. 可以通过 as 给表起别名

   ```sql
   -- 如果是单表查询 可以省略表名
   select id, name, gender from students;

   -- 表名.字段名
   select students.id,students.name,students.gender from students;

   -- 可以通过 as 给表起别名 
   select s.id,s.name,s.gender from students as s;
   ```
   **说明:**  
    * 在这里给表起别名看起来并没有什么意义,然而并不是这样的，我们在后期学习 自连接 的时候，必须要对表起别名。
    
### 2. distinct关键字

distinct可以去除重复数据行。

```sql
select distinct 列1,... from 表名;

例： 查询班级中学生的性别
select name, gender from students;

-- 看到了很多重复数据 想要对其中重复数据行进行去重操作可以使用 distinct
select distinct name, gender from students;
```

### 3. 小结

* as 关键字可以给表中字段 或者 表名起别名
* distinct 关键字可以去除重复数据行。

