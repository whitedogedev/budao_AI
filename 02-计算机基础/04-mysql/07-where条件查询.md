# where条件查询

**学习目标**

* 能够写出模糊查询的SQL语句

---

### 1. where条件查询的介绍

使用where条件查询可以对表中的数据进行筛选，条件成立的记录会出现在结果集中。

**where语句支持的运算符:**  
1. 比较运算符  
2. 逻辑运算符  
3. 模糊查询  
4. 范围查询  
5. 空判断

**where条件查询语法格式如下:**

```sql
select * from 表名 where 条件;
例：
select * from students where id = 1;
```

### 2. 比较运算符查询

1. 等于: =
2. 大于: &gt;
3. 大于等于: &gt;=
4. 小于: &lt;
5. 小于等于: &lt;=
6. 不等于: != 或 &lt;&gt;

**例1：查询编号大于3的学生:**

```sql
select * from students where id > 3;
```

**例2：查询编号不大于4的学生:**

```sql
select * from students where id <= 4;
```

**例3：查询姓名不是“黄蓉”的学生:**

```sql
select * from students where name != '黄蓉';
```

**例4：查询没被删除的学生:**

```sql
select * from students where is_delete=0;
```

### 3. 逻辑运算符查询

1. and
2. or
3. not

**例1：查询编号大于3的女同学:**

```sql
select * from students where id > 3 and gender=0;
```

**例2：查询编号小于4或没被删除的学生:**

```sql
select * from students where id < 4 or is_delete=0;
```

**例3：查询年龄不在10岁到15岁之间的学生:**

```sql
select * from students where not (age >= 10 and age <= 15);
```

**说明:**

* 多个条件判断想要作为一个整体，可以结合‘\(\)’。

### 4. 模糊查询

1. like是模糊查询关键字
2. %表示任意多个任意字符
3. \_表示一个任意字符

**例1：查询姓黄的学生:**

```sql
select * from students where name like '黄%';
```

**例2：查询姓黄并且“名”是一个字的学生:**

```sql
select * from students where name like '黄_';
```

**例3：查询姓黄或叫靖的学生:**

```sql
select * from students where name like '黄%' or name like '%靖';
```

### 5. 范围查询

1. between .. and .. 表示在一个连续的范围内查询
2. in 表示在一个非连续的范围内查询

**例1：查询编号为3至8的学生:**

```sql
select * from students where id between 3 and 8;
```

**例2：查询编号不是3至8的男生:**

```
select * from students where (not id between 3 and 8) and gender='男';
```

### 6. 空判断查询

1. 判断为空使用: is null
2. 判断非空使用: is not null

**例1：查询没有填写身高的学生:**

```sql
select * from students where height is null;
```

**注意:**

1. 不能使用 where height = null 判断为空
2. 不能使用 where height != null 判断非空
3. null 不等于 '' 空字符串

### 7. 小结

* 常见的比较运算符有 &gt;,&lt;,&gt;=,&lt;=,!=
* 逻辑运算符and表示多个条件同时成立则为真，or表示多个条件有一个成立则为真，not表示对条件取反 
* like和%结合使用表示任意多个任意字符，like和\_结合使用表示一个任意字符 
* between-and限制连续性范围 in限制非连续性范围 
* 判断为空使用: is null
* 判断非空使用: is not null



