## 五、多表查询

### 5.1、定义

​    针对多张表的查询,显示多个表的数据

### 5.2、语法格式

```
SELECT [DISTINCT] * | 列名称 [别名] , 列名称 [别名] ,...
FROM 数据表 [别名] , 数据表 [别名] ,...
[WHERE 条件(s)]
[ORDER BY 字段 [ASC | DESC] , 字段 [ASC | DESC] ,...]
```

### 5.3、笛卡尔集

#### 5.3.1、定义

​    笛卡尔乘积是指在数学中，两个集合X和_Y_的笛卡尓积（Cartesian product），又称直积

假设集合A={a, b}，集合B={0, 1, 2}，则两个集合的笛卡尔积为{\(a, 0\), \(a, 1\), \(a, 2\), \(b, 0\), \(b, 1\), \(b, 2\)}

#### 5.3.2、案例分析

1. 同时查询emp和dept表

   ```
   SELECT * FROM emp;
   SELECT * FROM dept;
   SELECT * FROM emp,dept;
   ```

2. 分析

   发现emp14条，dept表4条。但返回了56条数据56=14\*4

#### 5.3.3、如何避免笛卡尔积

​    唯一的方案就是在WHERE字句中加入有效的关联字段,

1. 示例代码

   ```
   SELECT * 
   FROM emp e,dept d
   WHERE  e.deptno=d.deptno;
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-31/46451349.jpg)

2. 说明

   1、从查询的结果看,我们显示的数据正常了,但仅仅是显示上去除了笛卡尔,而真正笛卡尔积现在依然存在，因为数据库的操作机制就属于逐行的进行数据的判断，那么如果按照这个思路理解的话，现在假设两张表的数据量都很大的话，那么使用这种多表查询的性能,无法从根本上解决这个问题  
   2、在进行多表查询的时候一定会存在有关联列，在开发中一般都使用表的主键作为关联表的字段,名字基本也一样.这样方便阅读和理解

   3、如果表之中存在有同名的列，那么一定要使用“表名称.字段名”,或者使用“别名.字段名“\(推荐使用别名,开发中常用\)

3. 接下来，我们切换用户到sh。

   ```sql
   CONN sh/sh;
   ```

   ​    ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/90857408-file_1491997538727_1045a.png)

4. 查询 sh 用户下的数据表。

   ```sql
   SELECT * FROM tab;
   ```

   ​    ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/56346382-file_1491997787529_5148.png)

5. 主要观察 COSTS 数据表 与 SALES 数据表。发现他们都有一个共同的字段  prod\_id;

   ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/81037805-file_1491998059406_152b8.png)

6. 查看 COSTS 数据表的数据条数。（观察程序查询的执行速度）

   ```sql
   SELECT COUNT(*) FROM costs;
   ```

   ​    ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/33348847-file_1491998176825_ff6e.png)

7. 查看 SALES  数据表的数据条数。（观察程序查询的执行速度）

   ```sql
   SELECT COUNT(*) FROM sales;
   ```

   ​    ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/62580692-file_1491998249041_7569.png)

8. 查看  COSTS 与 SALES  多表查询的结果的数据条数。（观察程序查询的执行速度）

   ```sql
   SELECT COUNT(*) FROM costs,sales WHERE costs.prod_id=sales.prod_id;
   ```

   如果查询过程没有 笛卡尔积问题，查询速度理论上应该和单独查询 COSTS或SALES差不了多少。但是我们会发现，整个查询过程耗时特别长，大概20秒\(根据系统的处理能力可能有所差距\)。查询结果条数为 1165337550。

   ​    ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/36547811-file_1491998635049_18068.png)

   但实际操作的数据量为：costs数据表的 82112 × sales数据表的 918843 = 75448036416。有750多亿条数据。

   > 在开发中，假设一个用户请求查询处理750多亿条数据。那么如果用户量比较大的情况。如果同时有几百个用户在请求。那么计算机肯定要完蛋。

   **总结：**所以一定要记住一个原则，多表查询的性能一定是很差的\(因为笛卡尔积问题\)，所以在开发中应该尽可能的避免。_\*_多表查询更多使用使用在数据比较少的数据表中。

   ​

#### 5.3.4、练习题

1. 查询每个雇员的编号，姓名，职位，工资，部门名称，部门位置。

   **分析问题：**

   * 确定使用到的数据表
     * emp表\(编号，姓名，职位，工资\)
     * dept表\(部门名称，部门位置\)
   * 确定已知的关联字段
     * emp表与dept表的 deptno。 emp.deptno = dept.deptno；

   **解决问题：**

   1. 第一步：查询出每一位雇员的编号，姓名，职位，工资

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal
      FROM emp e;
      ```

   2. 第二步：为查询中引入部门表，同时需要增加一个消除笛卡尔积的条件

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal,d.dname,d.loc
      FROM emp e,dept d
      WHERE e.deptno=d.deptno;
      ```

   ![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/65349840-file_1492008989937_129b.png)

2. 查询每个雇员的编号，姓名，职位，工资，工资等级。

   **分析问题：**

   * 确定使用到的数据表
     * emp表\(编号，姓名，职位，工资\)
     * salgrade表\(工资等级\)
   * 确定已知的关联字段
     * emp 表 sal 与salgrade表的 lasal,hisal。 emp.sal BETWEEN  salgrade.losal AND salgrade.hisal；

   **解决问题：**

   1. 第一步：查询出每一位雇员的编号，姓名，职位，工资

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal
      FROM emp e;
      ```

   2. 第二步：为查询中引入工资等级表，同时需要增加一个消除笛卡尔积的条件

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal,s.grade
      FROM emp e,salgrade s
      WHERE e.sal BETWEEN  s.losal AND s.hisal;
      ```

      ​![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/72600430-file_1492009517039_9088.png)

3. 查询每个雇员的编号，姓名，职位，工资，工资等级，部门名称。

   **分析问题：**

   * 确定使用到的数据表
     * emp表\(编号，姓名，职位，工资\)
     * salgrade表\(工资等级\)
     * dept表\(部门名称\)
   * 确定已知的关联字段
     * emp表与dept表的 deptno。 emp.deptno = dept.deptno；
     * emp 表 sal 与salgrade表的 lasal,hisal。 emp.sal BETWEEN  salgrade.losal AND salgrade.hisal；

   **解决问题：**

   1. 第一步：查询出每个雇员的编号、姓名、基本工资、职位、工资

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal
      FROM emp e;
      ```

   2. 第二步：引入工资等级表，同时增加一个消除笛卡尔积的条件

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal,s.grade
      FROM emp e,salgrade s
      WHERE e.sal BETWEEN  s.losal AND s.hisal;
      ```

   3. 第三步：引入部门表，继续增加消除笛卡尔积的条件

      ```sql
      SELECT e.empno,e.ename,e.job,e.sal,s.grade,d.dname
      FROM emp e,salgrade s,dept d
      WHERE e.deptno=d.deptno AND e.sal BETWEEN  s.losal AND s.hisal;
      ```

      ​![](http://ojx4zwltq.bkt.clouddn.com/17-4-12/59190297-file_1492009756970_67e7.png)



