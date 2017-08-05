## 创建索引

### 1、定义

​    索引这个数据结构用于加速对数据库中特定行的访问。它与特定的表相关联，包含来自这个表的一列或者多个列的数据，索引主要进行提高数据的查询速度。 当进行DML时，会更新索引。因此索引越多，则DML越慢，其需要维护索引。 因此在创建索引及DML需要权衡

### 2、分类

#### 2.1、按照数据存储方式，

1. B-树索引
2. 反向索引
3. 位图索引

#### 2.2、按照索引列的个数，

1. 分为单列索引
2. 复合索引

#### 2.3、按照索引列值的唯一性

1. 唯一索引
2. 非唯一索引

此外还有函数索引、全局索引、分区索引等。

### 3、语法格式    

```
CREATE [ UNIQUE|BITMAP] INDEX 索引名 
ON 表名  (列名 ASC|DESC, 列名2 ASC|DESC,…)
[REVERSE];
```

### 4、 示例代码

1. 普通索引：

   ```
   create index idx_dpt_dname on dept(dname);
   ```

2. 复合索引：

   ```
   create index idx_dept_dname_deptno on dept(dname, deptno);
   ```

3. 唯一索引

   ```
   create unique index idx_emp_ename on emp(ename);
   ```

4. 反向键索引

   ```
   create index idx_emp_rev_no on emp(empno) reverse;
   ```

5. 位图索引

   ```
   create bitmap index idx_dept_name on dept(dname);
   --如果用户查询的列的基数非常的小， 即只有的几个固定值，如部门的名称等等。要为这些基数值比较小的列建索引，就需要建立位图索引
   ```

### 4、什么情况使用索引

1. 索引应该经常建在Where 子句经常用到的列上。如果某个大表经常使用某个字段进行查询，并且检索行数小于总表行数的5%。则应该考虑。
2. 对于两表连接的字段，应该建立索引。如果经常在某表的一个字段进行Order By 则也经过进行索引。
3. 不应该在小表上建设索引。

### 5、索引失效的情况

1. Not Null/Null 如果某列建立索引,当进行Select \* from emp where depto is not null/is null。 则会是索引失效。
2. 索引列上不要使用函数,SELECT Col FROM tbl WHERE substr\(name ,1 ,3 \) = 'ABC' 或者SELECT Col FROM tbl WHERE name LIKE '%ABC%' 而SELECT Col FROM tbl WHERE name LIKE 'ABC%' 会使用索引。

3. 索引列上不能进行计算SELECT Col FROM tbl WHERE col / 10 &gt; 10 则会使索引失效，应该改成SELECT Col FROM tbl WHERE col &gt; 10 \* 10

4. 索引列上不要使用NOT （ != 、 &lt;&gt; ）如:SELECT Col FROM tbl WHERE col ! = 10 应该 改成：SELECT Col FROM tbl WHERE col &gt; 10 OR col &lt; 10 。

### 6、索引缺点

1. 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。 
2. 索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。 
3. 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。

### 7、总结

1. 能用唯一索引，一定用唯一索引 
2. 能加非空，就加非空约束 
3. 联合索引的顺序不同，影响索引的选择，尽量将值少的放在前面 



