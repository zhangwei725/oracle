# 分页查询

## 1、为什么需要分页？

​	当数据库的数据过的时候，客服端无法一次性显示所有数据,例如我们数据库表里有十万条数据,如果一下加载,查询的速度慢,用户体验差,而且用户也不可能一次性读完这个十万条数据	

## 2、分页技术分类

1. 物理分页(推荐)

   ```
   在数据库执行查询时（实现分页查询），查询需要的数据依赖数据库SQL语句，属于后台分页
   ```

2. 逻辑分页

   ```
   先查询所有数据到内存，再从内存截取需要数据采用程序内部逻辑，属于前台分页
   ```

## 3、ORACLE实现

### 2.1、说明

​	ORACLE分页采用ROWNUM

### 2.2 、语法格式

1. 格式1(推荐)

   ```
   SELECT * FROM   
   (  
   SELECT temp.*, ROWNUM RN   
   FROM (SELECT * FROM 表名) temp  
   WHERE ROWNUM <=end (page*pagesize)  
   )  
   WHERE RN >=start (page-1*pagesize+1)  
   ```

2. 格式2

   ```
   SELECT * FROM   
   (  
   SELECT temp.*, ROWNUM RN   
   FROM (SELECT * FROM TABLE_NAME) temp   
   )  
   WHERE RN BETWEEN start (page-1*pagesize+1) AND end (page*pagesize)  ​
   ```

### 2.3、效率对比

​	CBO优化模式下，Oracle可以将外层的查询条件推到内层查询中，以提高内层查询的执行效率。对于第2个查询语句，第二层的查询条件WHERE ROWNUM <= 40就可以被Oracle推入到内层查询中，这样Oracle查询的结果一旦超过了ROWNUM限制条件，就终止查询将结果返回了。
而第1个查询语句，由于查询条件BETWEEN 21 AND 40是存在于查询的第三层，而Oracle无法将第三层的查询条件推到最内层（即使推到最内层也没有意义，因为最内层查询不知道RN代表什么）。因此，对于第1个查询语句，Oracle最内层返回给中间层的是所有满足条件的数据，而中间层返回给最外层的也是所有数据。数据的过滤在最外层完成，显然这个效率要比第一个查询低得多.



### 2.4、ROWNUM介绍

#### 1、ROWNUM原理

1. 执行查询操作
2. 将第一行的row num置为1
3. 将得到的行的rownum与条件相比较，如果不匹配，则抛弃行，如果匹配，则返回行
4. oracle获取下一行，然后将rownum增1
5. 返回第3步

#### 2、示例代码

1. 使用rownum<查询数据

   ```
   select rownum,emp.* from emp where rownum < 5;  
   ```

   说明:对于查询返回的每一行，使用rownum伪列返回一个数字，表示oracle从表中选择行或将加入行的顺序。选择的第一行rownum为1，第二行为2，以此类推。

2. 使用rownum >来查询一下数据

   ```
   select rownum,emp.* from emp where rownum > 5; 
   ```

   说明: 发现该查询无返回任何数据,原因是第一行读取被分配为1，rownum>1使得条件为假，接着读取第二行现在变为第一行，并还分配为1,rownum使得条件依然是假，所有行随后均未能满足该条件，所以没有行被返回

3. rownum大于一个正整数来查询，必须使用子查询来实现  

   ```
   SELECT * FROM   
   (  
   SELECT temp.*, ROWNUM RN   
   FROM (SELECT * FROM emp) temp  
   WHERE ROWNUM <=15
   )  
   WHERE RN >=11
   ```

4. 使用 **BETWEEN 最小数 AND 最大数** 实现

   ```
   SELECT * FROM   
   (  
   SELECT temp.*, ROWNUM RN   
   FROM (SELECT * FROM emp) temp   
   )  
   WHERE RN BETWEEN 10 AND 15
   ```

#### 3、使用注意事项

1. rownum不能以任何基表的名称作为前缀。 　　
2. 子查询中的rownum必须要有别名，否则还是不会查出记录来，这是因为rownum不是某个表的列，如果不起别名的话，无法知道rownum是子查询的列还是主查询的列。 　　
3. 查询rownum在某区间的数据，rownum对小于某值的查询条件为true，rownum对于大于某值的查询条件直接认为是false的，但是可以间接的让它转为认为是true的。那就必须使用子查询。

## 4、Mysql实现

### 3.1、说明

​	Mysql分页采用 **LIMIT** 关键字

### 3.2、语法格式

1. 格式

   ```
   SELECT * FROM table  LIMIT [offset,] rows | rows OFFSET offset
   ```

2. 说明

   1、LIMIT 子句可以被用于强制 SELECT 语句返回指定的记录数。

   2、LIMIT 接受一个或两个数字参数。参数必须是一个整数常量。

   3、如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。

   4、初始记录行的偏移量是 0(而不是 1)

### 3.3、示例代码

1. 查询6-10条数据

   ```
    SELECT * FROM table LIMIT 5,5; 
   --检索记录行 6-10条数据
   ```

2. 查询从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1

   ```
   SELECT * FROM emp LIMIT 5,-1; // 检索记录行 6-最后一条.
   ```

3. 查询前 5 条记录