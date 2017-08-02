## 二、分组统计查询

### 1.1、概要

​    Group By语句从英文的字面意义上理解就是“根据\(by\)一定的规则进行分组\(Group\)”。它的作用是通过一定的规则将一个数据集划分成若干个小的区域，然后针对若干个小区域进行数据处理。 如果在查询的过程中需要按某一列的值进行分组,以统计该组内数据的信息时,就要使用group by子句。不管select是否使用了where子句都可以使用group by子句group by子句一定要与分组函数结合使用,否则没有意义。

### 1.2、语法格式

```
SELECT [DISTINCT] * | 列名称 [别名] , 列名称 [别名] ,... | 统计函数  4、确定查询列

FROM 数据表 [别名] , 数据表 [别名] ,...                             1、数据来源

[WHERE 条件(s)]                                                 2、过滤数据行

[GROUP BY 分组字段, 分组字段, ...]                                  3、执行分组操作

[ORDER BY 字段 [ASC | DESC] , 字段 [ASC | DESC] ,...]               5、数据排序
```

### 1.3、示例代码

1. 查询每个部门的人数

   ```
   SELECT deptno ,COUNT(*)
   FROM emp
   GROUP BY deptno;
   ```

2. 显示每个部门员工的平均工资

   ```
   SELECT deptno ,AVG(sal) 平均工资 
   FROM emp
   GROUP BY deptno;
   ```

3. 显示各个部门员工的工资+奖金

   ```
   SELECT deptno,SUM(sal + NVL(comm,0))
   FROM emp
   GROUP BY deptno;
   ```

4. 按照部门编号分组，求出每个部门的人数，平均工资\(要求截取2位\)\(配合单行函数使用\)

   ```
   SELECT deptno, COUNT(empno), ROUND(AVG(sal),2) 
   FROM emp
   GROUP BY deptno;
   ```

5. 按照职位分组，求出每个职位的最高和最低工资\(单字段分组\)

   ```
   SELECT job, MAX(sal), MIN(sal)
   FROM emp
   GROUP BY job;
   ```

6. 查询每个部门的每种岗位的平均工资和最低工资

   ```
   SELECT AVG(sal), MIN(sal)
   FROM emp
   GROUP BY deptno;
   ```

7. 先统计出各个职位\(job\)的平均工资\(AVG\),再统计平均工资最高的工资\(分组函数嵌套\)

   ```
   SELECT MAX(AVG(sal))
   FROM emp
   GROUP BY job
   注意:分组函数允许嵌套，但是嵌套之后的分组函数的查询之中不能再出现任何的其他字段
   ```

8. 查询每个岗位的总工资但不包括'SALESMAN'岗位\(配合Where使用\)

   ```
   SELECT 
   FROM emp
   WHERE name !='SALESMAN'
   ```

9. 按部门、不同的职位，统计员工的工资总额 \(多字段统计\)

   ```
   SELECT deptno, job, sum(sal)
   FROM emp   
   GROUP BY deptno, job;
   ```

10. 查询各个部门中相同职位的员工人数并且按部门编号排序\(多字段统计排序\)

    ```
    SELECT DEPTNO, JOB,COUNT(*) 
    FROM  emp
    GROUP BY deptno,job  
    ORDER BY deptno;
    ```

11. 查询出每个部门的名称、部门的人数、平均工资\(多表单字段查询\)

    ```
    1、确定表
        dept表
        emp表
    2、确定关联字段
        deptno
    3. 查询
      SELECT dname,count(e.empno),AVG(sal)
      FROM emp e,dept d
      WHERE  e.deptno(+) = d.deptno
      group by dname;
    ```

### 1.4、注意事项

1. GROUP BY后不可以接别名

   ```
   SELECT  deptno dn ,AVG(sal) FROM emp  GROUP BY dn;
   ```

2. GROUP BY 后不能接数字

   ```
   SELECT  deptno dn ,AVG(sal)
   FROM emp  
   GROUP BY 1;
   ```

3. GROUP BY 后可以接select后没有的列名

   ```
   SELECT  deptno dn ,AVG(sal) 
   FROM emp  
   GROUP BY job;
   ```

4. 如果一个SELECT中使用了分组函数，任何不在分组函数中的列\(表达式\)必须要在GROUP BY中

   ```
   SELECT  job ,deptno dn ,AVG(sal) 
   FROM emp  
   GROUP BY job;
   ```

5. group by之前可以使用where过滤数据,因为where是在分组之前起作用的，

### 1.5、使用HAVING过滤分组

#### 1.5.1 说明

1. 首先对数据行进行分组。 
2. 把所得到的分组应用到分组函数中。
3. 最后显示满足having条件的记录 

#### 1.5.2 语法格式

```
SELECT [DISTINCT] * | 列名称 [别名] , 列名称 [别名] ,... | 统计函数  4、确定查询列

FROM 数据表 [别名] , 数据表 [别名] ,...                             1、数据来源

[WHERE 条件(s)]                                                 2、过滤数据行

[GROUP BY 分组字段, 分组字段, ...]    [HIAVING 过滤分组]                3、执行分组操作

[ORDER BY 字段 [ASC | DESC] , 字段 [ASC | DESC] ,...]               5、数据排序
```

#### 1.5.2 示例代码

1. 查询部门的员工人数大于五部门编号

   ```
   SELECT deptno,COUNT(*)
   FROM emp
   GROUP BY deptno 
   HAVING COUNT(*)> 5;
   ```

2. 查询部门工资总和大于10000的部门编号

   ```
   SELECT deptno, SUM(sal) 
   FROM emp
   GROUP BY deptno 
   HAVING SUM(sal)>10000;
   ```

3. 查询平均工资低于2000的部门号和它的平均工资

   ```
   SELECT deptno,AVG(sal) a
   FROM emp
   GROUP BY deptno 
   HAVING avg(sal)>2000;
   ```

4. 查询每个岗位的总工资并且不包括职位是'SALESMAN'岗位而且工资和大于5000

   ```
   SELECT SUM(sal)
   FROM emp
   WHERE job!='SALESMAN'
   GROUP BY job  HAVING SUM(sal)>5000
   ```

### 1.6、综合示例

1. 查询非销售人员工作名称以及从事同一工作雇员的月工资的总和，并且要满足从事同一工作的雇员的月工资合计大于$5000，输出结果按月工资的合计升序排列

   1、查询出所有的非销售人员的信息

   ```sql
   SELECT * FROM emp WHERE job!=SALESMAN';
   ```

   2、按照职位进行分组，并且使用SUM函数统计

   ```mysql
   SELECT job,SUM(sal)
   FROM emp
   WHERE job<>'SALESMAN'
   GROUP BY job;
   ```

   3、月工资的合计是通过统计函数查询的，所以现在这个对分组后的过滤要使用HAVING子句完成

   ```mysql
   SELECT job,SUM(sal)
   FROM emp
   WHERE job!='SALESMAN'
   GROUP BY job
   HAVING SUM(sal)>5000;
   ```

   3、按照升序排列

   ```mysql
   SELECT job,SUM(sal) sum
   FROM emp
   WHERE job!='SALESMAN'
   GROUP BY job
   HAVING SUM(sal)>5000
   ORDER BY sum ASC;
   ```

2. 显示部门编号不是30的，的部门详细信息（部门编号、部门名称、部门人数、部门月薪资总和），并要求 部门月工资总和大于8000，输出结果按部门月薪资的总和降序排列。

   ```
   SELECT d.deptno,d.dname,COUNT(*) 人数,NVL(SUM(e.sal),0) 月总收入
   FROM dept d,emp e
   WHERE d.deptno=e.deptno(+) AND d.deptno!=30
   GROUP BY d.deptno,d.dname
   HAVING SUM(e.sal) >8000
   ORDER BY SUM(e.sal) DESC;
   ```



