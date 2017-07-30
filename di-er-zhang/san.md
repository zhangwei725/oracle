# 三、限定查询（控制行）

## 3.1、概要

​ 在之前的简单查询中，是将所有的记录进行显示，但是现在可以对显示的记录进行过滤的操作，而这就属于限定查询的工作了，限定查询就是在之前语法的基础上增加了一个WHERE子句，用于指定限定条件

在使用 WHERE 子句操作的时候可以进行条件的判断，而对于条件的判断主要有以下几类操作

1. 关系运算

   ```
   >、<、>=、<=、<>、!=
   ```

2. 范围判断

   ```
   BETWEEN AND
   ```

3. 判断是否为空

   ```
   IS NULL
   IS NOT NULL
   ```

4. 指定范围的判断

   ```
    IN 
    NOT IN
   ```

5. 模糊查询

   ```
   LIKE
   NOT LIKE
   ```

## 3.2、语法格式

```
SELECT [DISTINCT] * | 列名 [别名] ,列名 [别名] , --3、 控制要显示的数据列
FROM 表名称 [别名] 
 --1、 确定数据来源
[WHERE 条件(s)] ; 
-- 2、 根据判断条件选择参与的数据行
```

## 3.3、限定操作符

### 3.3.1、关系运算\(&gt;、&lt;、&gt;=、&lt;=、&lt;&gt;、!=\)

#### 3.3.1.1、等于\(=\)、不等于\(!=或者&lt;&gt;\)

1. 查询出所有职位是办事员的雇员信息

   ```
   1.错误的写法
     SELECT * 
     FROM emp 
     WHERE job='clerk'(所有的数据都是区分大小写的)
   2.正确的写法
     SELECT * 
     FROM emp 
     WHERE job='CLERK'
   ```

2. 查出员工工资不等于1500的员工信息

   ```
   SELECT *
   FROM  EMP
   WHERE sal !=1500;
   ```

3. 查询所有职位不是销售人员的信息

   ```
   SELECT * 
   FROM emp 
   WHERE job<>'SALESMAN' ;
   ```

#### 3.3.1.2、小于&lt;、小于等于 &lt;=

1. 查询出基本工资不大于1500

   ```
   SELECT * 
   FROM emp 
   WHERE sal <= 1500;
   ```

2. ​

#### 3.3.1.3、大于**&gt;**、 大于等于**&gt;=**

1. 找出奖金高于工资的员工

   ```
   SELECT * 
   FROM  empWHERE comm>sal;
   ```

2. 找出工资大于或等于3000的员工

   ```
   SELECT ename ,sal 
   FROM emp
   WHERE sal>=3000;
   ```

### 3.3.2、逻辑运算\(AND OR\)

1. 说明

   在使用where 语句的时候可能会存在编写多个条件的情况,那这个时候就必须使用逻辑运算符了

2. 语法格式

   ```
   AND：条件 AND 条件 AND 条件； 所有条件都要同时满足
   OR：条件 OR 条件 OR 条件；所有条件只要有一个满足即可
   ```

3. 示例代码

   1、查询工资在1500~3000之间的全部雇员信息

   ```
   SELECT * 
   FROM emp
   WHERE sal
   >
   =1500 AND sal
   <
   =3000;
   ```

   2、查询出职位是办事员\(job='CLERK'\)，或者是销售人员\(job='SALESMAN'\)的全部信息

   ```
   SELECT * 
   FROM emp
   WHERE job='CLERK' OR job='SALESMAN';
   ```

​

​

