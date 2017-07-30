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

        3、查询出职位是办事员\(job='CLERK'\)，或者是销售人员\(job='SALESMAN'\)的全部信息，并且要求这些雇员的工资大于1200

          

### 3.3.3、范围判断\(BETWEEN AND\)

1. 说明

   表示的是一个范围的判断过程

2. 语法格式

   ```
   BETWEEN 最小值 AND 最大值”，
   ```

3. 示例代码

   1、查出工资在1000~2000之间的员工

   ```
   SELECT * 
   FROM emp
   WHERE sal BETWEEN  1000 AND 2000;
   ```

   2、查出工资在1985年1月20至1987年5月20之间入职的员工

   ```
   SELECT * 
   FROM emp
   WHERE hiredate BETWEEN  '20-1月 -85' AND '20-5月 -87';
   ​
   SELECT *
   FROM emp
   WHERE hiredate 
   BETWEEN date '1985-01-01' 
   AND date '1985-05-20';
   ```

### 3.3.4、判断是否为空\(IS\[NOT\]NULL\)

1. 查询出所有领取奖金的雇员信息

   ```
   SELECT * 
   FROM emp
   WHERE comm IS NOT NULL;
   ​
   SELECT * 
   FROM emp
   WHERE NOT comm IS NULL;
   ```

2. 查询出所有不领取奖金的雇员

   ```
   SELECT * 
   FROM emp
   WHERE comm IS NULL
   ```

### 3.3.5、指定范围的判断\(\[NOT\]IN\)

1. 说明

   IN操作符表示的是指定一个查询的范围

2. 语法格式

   ```
   字段 IN (数值,数值,…)
   ```

3. 示例代码

   1、查询出雇员编号是7369、7566、7799的雇员信息

   ```
   SELECT * 
   FROM emp
   WHERE empno=7369 OR empno=7566 OR empno=7799
   ```

   ```
   SELECT * 
   FROM emp
   WHERE empno IN (7369,7566,7799);
   ```

   2、查询出姓名为 SMITH,ALLEN,KING的雇员信息

   ```
   SELECT * 
   FROM emp 
   WHERE ename in ('SMITH','ALLEN', 'KING');
   ```

   3、查询出雇员编号不是7369、7566、7799的雇员信息

   ```
   SELECT * 
   FROM emp
   WHERE not empno IN (7369,7566,7799);
   ```

4. 注意事项

   ```
   关于NOT IN的问题
   如果现在使用了IN操作符，查询的范围之中存在了null，不影响查询；
   SELECT * FROM emp WHERE empno IN(7369,7566,null);
   如果现在使用的是NOT IN操作符，如果查询范围之中有了null则表示的就是查询全部数据。
   SELECT * FROM emp WHERE empno NOT IN(7369,7566,null);
   ​
   empno in (7369,7566,null)可以等价于empno=7369 or empno=7566 or empno=null，
   empno not in (7369,7566,null)可以等价于not(7566=7369 or 7566=7566 or empno=null)
   或empno!=7566 and empno!=7566 and empno=null。
   为什么都是or拼接，in可以而not in不可以呢，可以把not in理解为后面的and表达式就知道了，因为empno=null为null，
   也就相当于false，导致整个表达式为false，无论传何值都为false，自然无法返回数据
   ```

### 3.3.6、模糊查询

1. 说明

   LIKE子句的功能是提供了模糊查找的操作,程序里出现的搜索操作，都属于LIKE子句的实现,但是要想使用LIKE子句则必须认识两个匹配符号

   ```
   匹配单个字符：_ 
    --匹配任意一个字符
   匹配任意多个字符：% --匹配 0 个、1 个或多个任意字符
   ```

2. 语法

   ```
   字段 LIKE 关键字
   ```

3. 示例代码

   1、查询出雇员第二个字母为“L”的雇员信息

   ```
   select * 
   from emp 
   WHERE ename like '_L%';
   ```

   2、查询出雇员姓名以字母“S”开头的雇员信息

   ```
   SELECT * 
   FROM emp 
   WHERE ename like 'S%'




   ```

   3、查询出雇员姓名包含字母“S”的雇员信息

   ```
   SELECT * 
   FROM emp 
   WHERE ename like '%S%'




   ```

   4、查询入职年份为81年的雇员信息

   ```
   SELECT * 
   FROM emp 
   WHERE hiredate like '%81'




   ```

   5、查询工资值中包含数字5的雇员信息

   ```
   SELECT * 
   FROM emp 
   WHERE sal like '%5%'




   ```

  
   ​

​

