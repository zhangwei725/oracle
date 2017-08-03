

## 三、子查询

### 1.1、定义

1. 指嵌入在其他sql语句中的select语句
2. 通俗的来讲指的是在一个查询之中嵌入若干个小的查询
3. 子查询可以出现在查询语句的(9i中除ORDER  BY语句)任意位置上，但是实际开发运用中，子查询出现在WHERE和FROM子句之中较多。

### 1.2 、核心作用

​	子查询的出现主要是为了解决多表查询之中的性能问题。

### 1.3 、分类

1. 嵌套查询(标准子查询)

   ```
   指子查询可以脱离主查询独立执行
   ```

2. 关联查询(相关子查询)

   ```
   关联子查询就是指子查询与主查询之间有条件关联,关联子查询会引用外部查询中的一列或多列。这种子查询之所以被称为关联子查询，是因为子查询的确与外部查询有关。当问题的答案需要依赖于外部查询中包含的每一行中的值时，通常就需要使用关联子查询。
   ```

### 1.4 、完整的语法格式

```
SELECT [DISTINCT] *|分组字段1 [别名] [,分组字段2 [别名] ,…] | 统计函数 ,(

      SELECT [DISTINCT] *|分组字段1 [别名] [,分组字段2 [别名] ,…] | 统计函数

      FROM 表名称 [别名], [表名称 [别名] ,…]

      [WHERE 条件(s)]

      [GROUP BY 分组字段1 [,分组字段2 ,…]]

      [HAVING 分组后的过滤条件（可以使用统计函数）]

      [ORDER BY 排序字段 ASC | DESC [,排序字段 ASC | DESC]])

FROM 表名称 [别名], [表名称 [别名] ,…] ,(

      SELECT [DISTINCT] *|分组字段1 [别名] [,分组字段2 [别名] ,…] | 统计函数

      FROM 表名称 [别名], [表名称 [别名] ,…]

      [WHERE 条件(s)]

      [GROUP BY 分组字段1 [,分组字段2 ,…]]

      [HAVING 分组后的过滤条件（可以使用统计函数）]

      [ORDER BY 排序字段 ASC | DESC [,排序字段 ASC | DESC]])

[WHERE 条件(s) (

      SELECT [DISTINCT] *|分组字段1 [别名] [,分组字段2 [别名] ,…] | 统计函数

      FROM 表名称 [别名], [表名称 [别名] ,…]

      [WHERE 条件(s)]

      [GROUP BY 分组字段1 [,分组字段2 ,…]]

      [HAVING 分组后的过滤条件（可以使用统计函数）]

      [ORDER BY 排序字段 ASC | DESC [,排序字段 ASC | DESC]])]

[GROUP BY 分组字段1 [,分组字段2 ,…]]

[HAVING 分组后的过滤条件（可以使用统计函数）]

[ORDER BY 排序字段 ASC | DESC [,排序字段 ASC | DESC]];
```

### 1.5、使用原则

1. 在查询中根据返回的结果有以下几种情况

   1、单行子查询（子查询返回的结果是单行单列）

   2、多行子查询（子查询返回多行单列）

   3、多列子查询（子查询返回多列，可以是单行、多行或者不返回任何结果）如果返回结果是单行多列，则可以使用单行比较运算符

2. 子查询可以出现在操作符的左边或者右边

3. 嵌套查询先执行，然后将结果传递给主查询。

### 1.6、在WHERE中使用子查询

#### 1.6.1、说明

​	WHERE 的主要功能是控制数据行，那么在 WHERE 子句之中，子查询的返回结果一般情况是

1. 单行单列
2. 多行单列
3. 单行多列数据 

#### 1.6.2、子查询返回单行单列

​	如果返回单行单行单列可以使用单行操作符:  >、>=、 <、 <= 、<>、=

1、查询出工资比SCOTT还要高的全部雇员信息

1. 查询SCOTT的工资是多少：

   ```mysql
   SELECT sal 
   FROM emp 
   WHERE ename='SCOTT';
   ```

2. 过滤数据

   ```mysql
   SELECT * 
   FROM emp
   WHERE sal>(SELECT sal FROM emp WHERE ename='SCOTT');
   ```

2、查询和SMITH职位相同的所有员工的员工姓名和职位

1. 查询出SMITH的职位

   ```mysql
   SELECT job
   FROM emp
   WHERE ename='SMITH'
   ```

2. 过滤数据显示员工的姓名和职位

   ```mysql
   SELECT ename,job
   FROM emp
   WHERE job=(SELECT job
   		  FROM emp
   		  WHERE ename='SMITH')
   ```

3、查询出高于公司平均工资的全部员工信息

1. 公司的平均工资应该使用AVG()函数求出。

   ```mysql
   SELECT AVG(sal) 
   FROM emp;
   ```

2. 过滤数据显示全部员工的信息

   ```mysql
   SELECT *
   FROM emp
   WHERE sal>(SELECT AVG(sal) FROM emp);
   ```

4、查询员工姓名ADAMS相同职位的员工信息 并且薪资大于员工姓名CLARK的薪资的员工信息

1. 查询员工姓名ADAMS的职位

   ```mysql
   SELECT job 
   FROM emp
   WHERE ename='ADAMS'
   ```

2. 查询员工姓名CLARK的薪资

   ```mysql
   SELECT sal  
   FROM emp
   WHERE ename='CLARK'
   ```

3. 过滤数据

   ```mysql
   SELECT *
   FROM  emp
   WHERE job=(SELECT job 
   		  FROM emp
   		  WHERE ename='ADAMS')
   	 AND sal>(SELECT sal  
   			 FROM emp
   			 WHERE ename='CLARK')	  
   ```

#### 1.6.3、子查询返回单列多行

如果返回的多行单列必须使用多行比较操作符

| 操作符    | 说明             |
| ------ | -------------- |
| IN     | 等于列表中的任何一个     |
| ALL    | 和子查询返回的所有值比较   |
| exists | 返回结果集为真        |
| ANY    | 与子查询返回的任意一个值比较 |

##### 1.6.3.1、IN操作符

1. 查询各个职位中工资最高的员工信息(子查询是分组查询)

   ```
   SELECT ename, job, sal 
   FROM emp 
   WHERE sal in (SELECT max(sal) 
   			FROM emp 
   			GROUP BY job)
   ```

2. 查询员工工资跟管理部相同的员工工资

   ```
   SELECT * 
   FROM emp 
   WHERE sal IN (SELECT sal 
   			 FROM emp 
   			 WHERE job='MANAGER');
   ```

##### 1.6.3.2、ANY

1、使用方式

1. **<any为小于最大的**
2. **>any为大于最小的**
3. **=ANY：功能与IN操作符是完全一样的**

2、示例代码

1. ​
2. ​

##### 1.6.3.3、ALL

1、使用方式

1. **<ALL：比子查询最小值还要小**
2. **>ALL：比子查询返回的最大值还要大**
3. **=ANY:跟IN操作符一样**
4. ALL(如果有一个内容是null，则不会查询出任何的结果)

2 示例代码

1. 查询工资高于所有部门的平均工资的员工(>ALL)

   ```
   select * 
   from emp 
   where sal>all(select avg(sal) 
   			 from emp group by deptno)		 
   ```

   ​

2. 查询工资小于所有部门的平均工资的员工不包括(<ALL)

   ```
   select * 
   from emp 
   where sal<all(select avg(sal) 
   			 from emp group by deptno) 
   ```

##### 1.6.3.4、EXISTS 操作符

1、EXISTS 操作符检查在子查询中是否存在满足条件的行

1. 如果在子查询中存在满足条件的行:不在子查询中继续查找,条件返回 TRUE


1. 如果在子查询中不存在满足条件的行:条件返回 FALSE,继续在子查询中查找

2、示例代码

1. 查询是管理员的员工编号,姓名,职位,部门编号信息(关联查询 EXISTS关键字)

   ```
   SELECT empno, ename, job, deptno
   FROM emp e
   WHERE EXISTS (SELECT *
   			 FROM emp
   			 WHERE  mgr =  e.empno);
   ```

#### 1.6.4、子查询返回单行多列

​	返回结果在内存中构成一个单行多列的数据表，返回单行多列的子查询在实际应用中与返回单行单列的数据类似，只是查询条件可以扩展成多个，用括号把查询条件括起来

​	语法格式: where (列名,列名) in (子查询)

1、查询sal和comm和empno为7698相同的人员的信息

1. 先查询sal和comm和empno为7698的信息

   ```mysql
   select * 
   from emp 
   where (sal,comm)=(SELECT sal,comm 
   				FROM emp 
   				WHERE empno=7698);
   ```

2. 过滤数据

   ```mysql
   select * 
   from emp 
   where (sal,comm)=(SELECT sal,comm 
   				FROM emp 
   				WHERE empno=7698);
   ```

#### 1.6.5、多行多列(了解)

1. 查询部门中员工每个职位,工资最高的员工的姓名 ,职位还有工资

   ```mysql
   select ename, job, sal 
   from emp 
   where (sal,job) in (select max(sal), job 
   				  from emp 
   				  group by job);
   ```

### 1.7、FROM中使用子查询

#### 1.7.1、说明

​	如果在 FROM 子句里面出现的子查询，其返回的结果一定是多行多列数据。

#### 1.7.2、示例代码

1. **范例：**查询出每个部门的编号、名称、位置、部门人数、平均工资

   1、方式一

   ```mysql
   SELECT d.deptno,d.dname,d.loc,COUNT(e.empno),AVG(e.sal)
   FROM emp e,dept d
   WHERE e.deptno(+)=d.deptno
   GROUP BY d.deptno,d.dname,d.loc;
   ```

   2、方式二:通过子查询完成，所有的统计查询只能在GROUP BY中出现，所以在子查询之中负责统计数据，而在外部的查询之中，负责将统计数据和dept表数据相统一。

   ```mysql
   SELECT d.deptno,d.dname,d.loc,temp.count,temp.avg
   FROM dept d,(SELECT deptno dno,COUNT(empno) count,AVG(sal) avg
   			FROM emp
   			GROUP BY deptno) temp
   WHERE d.deptno=temp.dno(+);
   ```

   3、说明

   方式一的数据量，

   ​	积的数量：emp 的 14 条 * dept 的 4 条 = 56 条数据；

   方式二的数据量

   - 子查询中统计的记录是14条记录，最终统计的显示结果是3条记录；
   - dept表之中一共有4条记录；
   - 如果现在产生笛卡尔积的话只有12条记录，再加上子查询中雇员的14条记录，一共才26条记录；

   通过如上的分析，可以发现，使用子查询的确要比使用多表查询更加节省性能，所以在开发之中子查询出现是最多的，而且在给出一个不成文的规定：大部分情况下，如果最终的查询结果之中需要出现SELECT子句，但是又不能直接使用统计函数的时候，就在子查询中统计信息，即：有复杂统计的地方大部分都需要子查询。

2. 查询比本部门平均工资高的员工的姓名,部门编号,工资及平均工资

   ```mysql
   SELECT e.ename, e.sal, e.deptno, temp.salavg
   FROM emp e, (SELECT department_id, AVG(sal) salavg
   			FROM employees
   			GROUP BY department_id) temp
   			WHERE e.deptno = temp.deptno
   AND e.sal > temp.salavg		
   ```

3. 查询出部门名称，部门的员工数，部门的平均工资，部门的最低收入雇员的姓名

   ```mysql
   SELECT d.dname,temp.cou,temp.avg,e.ename
   FROM (SELECT deptno,count(1) cou,round(avg(sal)) avg,min(sal) min
   	 			FROM emp	
   	 			GROUP BY deptno
   	 ) temp,
    	dept d,
   	emp e
   WHERE d.deptno=temp.deptno AND  e.sal=temp.min;
   ```

### 1.8、在 HAVING 子句之中使用子查询

#### 1.8.1、说明

​	如果使用了 HAVING 子句一定意味着进行了分组，而且进行了统计查询。在 HAVING 之中出现的子查询只能够返回 单行单列的数据

#### 1.8.2、示例代码

1. 查询出高于公司平均工资的部门编号、平均工资 

   1、计算出公司的平均工资

   ```mysql
   SELECT AVG(sal) 
   FROM emp ;
   ```

   2、要在分组之后进行过滤

   ```mysql
   SELECT deptno,AVG(sal) 
   FROM emp 
   GROUP BY deptno 
   HAVING AVG(sal)>(SELECT AVG(sal) 
   				FROM emp);
   ```

2. 查询出平均工资最低的职位信息、人数、平均工资 

   1、找到平均工资最低的职位数据

   ```
   SELECT MIN(AVG(sal)) 
   FROM emp 
   GROUP BY job ;
   ```

   2、统计函数嵌套之后无法再出现分组字段，所以现在可以将以上单行单列的结果，出现在 HAVING 子句之中

   ```
   SELECT job,AVG(sal) 
   FROM emp 
   GROUP BY job 
   HAVING AVG(sal)=(SELECT MIN(AVG(sal)) FROM emp GROUP BY job)				
   ```

3. 查询出部门名称，部门的员工数，部门的平均工资，部门的最低收入雇员的姓名

   ```
   SELECT d.dname,temp.cou,temp.avg,e.ename
   FROM (SELECT deptno,count(1) cou,round(avg(sal)) avg,min(sal) min
   	 			from scott.emp	
   	 			GROUP  BY deptno)
    	temp,
    	dept d,
   	emp e
   WHERE d.deptno=temp.deptno AND  e.sal=temp.min;
   ```

### 1.9、综合练习

1. 查询部门名称，部门的员工数，部门的平均工资，部门的最低收入员工的姓名和最高收入员工的姓名

   1、查询出部门名称，部门员工数，部门平均工资，部门最低收入和最高收入

   ```
   SELECT count(*) 员工人数, 
   	   avg(sal),部门平均工资
   	   min(sal)最低工资, 
   	   max(sal) 最高工资
   FROM emp 
   GROUP BY deptno;
   ```

   2、查询最低收入者的姓名

   ```
    SELECT e.ename 
      FROM emp e, (SELECT deptno, 
                          count(*), 
                          avg(sal), 
                          min(sal) min_sal, 
                          max(sal) max_sal 
              	FROM emp 
            		GROUP BY deptno) t 
      WHERE e.sal = t.min_sal;
   ```

    3、查询最高收入者的姓名

   ```
      SELECT e.ename 
      FROM emp e, (SELECT deptno, 
                          count(*), 
                          avg(sal), 
                          min(sal) min_sal, 
                          max(sal) max_sal 
              	FROM emp 
            		GROUP BY deptno) t 
      WHERE e.sal = t.min_sal;
   ```

    4、跟第一次查询出来的部门最高收入和最低收入同时关联两张emp 表，分别获取最高收入者和最高收入者的姓名

   ```
   SELECT DEPT.DNAME 部门名称,
             TEMP.C  人数,
             TEMP.A  平均工资,
             TEMP.M  最低工资,
             TEMP.MS  最高工资,
             E.ENAME  最低工资人,
             DD.ENAME 最高工资人
      FROM (SELECT DEPTNO, 
          		COUNT(*) C, 
          		AVG(SAL) A, 
          		MIN(SAL) M, 
          		MAX(SAL) MS
             FROM EMP
             GROUP BY DEPTNO) TEMP,
             DEPT,
             EMP E,
             EMP DD
      WHERE TEMP.DEPTNO = DEPT.DEPTNO
             AND E.SAL = TEMP.M
             AND E.DEPTNO = TEMP.DEPTNO
             AND DD.SAL = TEMP.MS
             AND DD.DEPTNO = TEMP.DEPTNO
   ```