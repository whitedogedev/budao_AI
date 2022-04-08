# 外键SQL语句的编写

**学习目标**

* 能够写出删除外键约束的SQL语句

---

### 1. 外键约束作用

外键约束:对外键字段的值进行更新和插入时会和引用表中字段的数据进行验证，数据如果不合法则更新和插入会失败，保证数据的有效性

### 2. 对于已经存在的字段添加外键约束

```sql
-- 为cls_id字段添加外键约束
alter table students add foreign key(cls_id) references classes(id);
```


### 3. 在创建数据表时设置外键约束

```sql
-- 创建学校表
create table school(
    id int not null primary key auto_increment, 
    name varchar(10)
);

-- 创建老师表
create table teacher(
    id int not null primary key auto_increment, 
    name varchar(10), 
    s_id int not null, 
    foreign key(s_id) references school(id)
);
```

### 4. 删除外键约束

```sql
-- 需要先获取外键约束名称,该名称系统会自动生成,可以通过查看表创建语句来获取名称
show create table teacher;

-- 获取名称之后就可以根据名称来删除外键约束
alter table teacher drop foreign key 外键名;
```

### 5. 小结

* 添加外键约束: alter table 从表 add foreign key(外键字段) references 主表(主键字段);
* 删除外键约束: alter table 表名 drop foreign key 外键名;






