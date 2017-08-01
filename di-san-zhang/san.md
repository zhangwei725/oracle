# 五、多表查询-连接的类型

## 5.1、概述

### 5.1.1、在Oracle8i之前的表连接类型

1. 等值连接\(内连接\)\(Equijoin\),
2. 非等值连接\(Non-Equijoin\),
3. 外连接\(Outer join\):左外连接\(+\),右外连接\(+\)
4. 自连接\(Self join\)

### 5.1.2、SQL1999

​    Oracle9之后新引入的连接形式\(支持SQL99规范\),SQL 语法标准实际上一直在进行更新，从 1999 年之后对于数据表的关联查询给了一个标准的操作语法\(因为“\(+\)”符号只有 Oracle 可以使，那么其它的数据库不支持此类符号，只能够使用 SQL:1999 语法完成\)

1. 交叉联接\(笛卡尔积\)
2. 自然连接\(Natural join\)
3. 外连接\(Outer Join\) 

## 5.2、等值连接\(内连接\)

### 5.2.1、说明

​    在连接的时候使用运算符“=”号的叫等值连接

### 5.2.2、语法格式

1. 语法1

   ```
   SELECT *
   FORM 表1,表2
   WHERE 表1.列名=表2.列名;
   ```

2. 语法2

   ```
   SELECT *
   FORM 表1 [INNER] JION 表2 
           ON 表1.列名=表2.列名;
   备注:内连接中的INNER JION 和 JION 是等价的！
   但是建议为了程序的可读性尽量不要省略INNER!
   ```

### 5.2.3、示例代码

1. 查询员工信息以及对应的员工所在的部门信息

   ```
   SELECT * 
   FROM EMP,DEPT
   WHERE EMP.DEPTNO = DEPT.DEPTNO;
   ```

## 5.3、非等值连接

### 5.3.1、说明

​    在连接的时候不使用运算符“=”号的叫非等值连接,常用的比较符号一般为&gt;,&lt;,...,BETWEEN.. AND..

### 5.3.2、示例代码

1. 查询员工的编号,姓名,工资,以及工资所对应的级别

   ```
   SELECT  e.empno, e.ename, e.sal, s.grade 
   FROM emp e, salgrade s 
   WHERE e.sal BETWEEN s.losal AND s.hisal
   ```

2. 查询员工的编号,姓名,工资,工资级别,所在部门的名称

   ```
   SELECT e.empno,e.ename,e.sal,s,grade,d.name 
   FROM emp e,dept d,salgrade s
   WHERE e.deptno = d.deptno AND e.sal BETWEEN losal AND hisa
   ```

## 5.4、 外链接-左外链接

### 5.4.1、说明

​    两个表在连接过程中除返回满足连接条件的行为外,还返回左表中不满足条件的行为，这种连接称为左外连接

1. USING 关键字

   1、如果在使用using关键字时，而且select的结果列表项中包含了using关键字所指明的那个关键字，那么请不要在select的结果列表项中对该关键字指明它属于哪个表

   2、仅能使用一个列名

   3、natural join关键字和using关键字不能同时出现

2. ON\(去笛卡尔积条件\)

### 5.4.2 、语法格式

1. 语法1\(支持oracle\)

   ```
   SELECT * 
   FROM 表1,表2
   WHERE 表1.列名 = 表2.列名(+)
   ```

2. 语法2\(sql标准\)

   ```
   SELECT * 
   FROM 表1 LEFT OUTER JION 表2
   ON(表1.列名= 表2.列名)|
   USING(列名(同名同类型))
   ```

### 5.4.3、示例代码

1. 查询员工信息以及所对应的部门信息,显示所有的部门

   ```
   无法显示没有部门的员工信息
   SELECT * 
   FROM EMP,DEPT 
   WHERE EMP.DEPTNO = DEPT.DEPTNO(+);
   ```

   ```
   SELECT * 
   FROM emp LEFT JOIN dept using(deptno);
   ```

## 5.5、 外链接-右外链接

### 5.5.1、说明

​    两个表在连接过程中除返回满足连接条件的行为外,还返回右表中不满足条件的行为,这种连接称为右外连接.

### 5.5.2、语法格式

1. 语法1\(oracle\)

   ```
   SELECT * 
   FORM 表1,表2
   WHERE 表1.列名(+)= 表2.列名
   ```

2. 语法2\(SQL标准\)

   ```
   SELECT * 
   FROM 表1 RIGHT OUTER JION 表2 
       ON(表1.列名=表2.列名)| USING(列名(同名同类型))
   ```

### 5.5.3、示例代码

1. 显示员工信息以及所对应的部门信息,显示没有员工的部门信息

   ```
   SELECT * 
   FROM EMP,DEPT 
   WHERE EMP.DEPTNO(+) = DEPT.DEPTNO;
   ```

   ```
   SELECT * 
   FROM emp e RIGHT OUTER  JION dept.d USING(deptno)
   ```

### 5.5.4、关于使用（+）的一些注意事项：

1. （+）操作符只能出现在where子句中，并且不能与outer join语法同时使用。
2. （+）操作符执行外连接时，如果在where子句中包含有多个条件，则必须在所有条件中都包含（+）操作符
3. （+）操作符只适用于列，而不能用在表达式上。
4. （+）操作符不能与or和in操作符一起使用。
5. （+）操作符只能用于实现左外连接和右外连接，而不能用于实现完全外连接。

## 5.6、 全外连接\(Full Outer Join\)

### 5.6.1、说明

Oracle9开始新增功能,两个表在连接过程中除返回满足连接条件的行为外，还返回两个表中不满足条件的所有行为，这种连接称为满外连接.

### 5.6.1 、语法格式

1. 语法\(SQL标准\)

   ```
   SELECT *
   FROM 表1  FULL OUTER JION 
       ON(表1.列名= 表2.列名)
   ```

### 5.6.2 、示例代码

1. 查询员工编号,员工姓名,职位,部门编号,部门名称.要求显示没有部门的员工信息同事显示没有员工的部门

   ```
   SELECT e.empno,e.ename,e.job,d.deptno,d.dname
   FROM emp e FULL OUTER JOIN dept d
   ON e.deptno=d.deptno;
   ```

## 5.7、自然连接\(NATURAL JOIN\)

### 5.7.1、说明

​    自然连接是在两张表中寻找那些数据类型和列名都相同的字段，然后自动地将他们连接起来，并返回所有符合条件按的结果

### 5.7.2、使用条件

1. **如果两个表中的同名列的所有数据类型不同,则出错**
2. **不允许在参照列上使用表名或者别名作为前缀**
3. 如果做自然连接的两个表的有多个字段都满足有相同名称个类型，那么他们会被作为自然连接的条件
4. 查询出来的结果会合并同名列
5. **由于oracle中可以进行这种非常简单的NATURAL JION，我们在设计表时，应该尽量在不同表中具有相同含义的字段使用相同的名字和数据类型。以方便以后使用 NATURAL JION**

### 5.7.3、语法格式

1. 语法

   ```
   SELECT * |列名 [别名] ,列名[别名],...
   FROM 表1 NATURAL JION 表2;
   ```

### 5.7.4、示例代码

1. 查询员工信息以及对应的员工所在的部门信息

   ```
   SELECT * 
   FROM emp NATURAL JION dept;
   ```

## 5.8、自连接\(SELF JOIN\)

### 5.8.1、说明

​    使用自连接可以将自身表的一个镜像当作另一个表来对待

### 5.8.2、示例代码

1. 显示雇员的编号,名称,以及该雇员的经理名称

   ```
   SELECT e.ENAME,e.MGR,e.EMPNO,m.ENAME
   FROM EMP e,EMP m
   WHERE e.MGR = m.EMPNO
   ```

## 5.9、交叉连接\(Self join\)

### 5.9.1、说明

​    交叉联接返回左表中的所有行，左表中的每一行与右表中的所有行组合。交叉联接也称作笛卡尔积

## 5.其他

SQL标准的版本

1. 1986年，ANSI X3.135-1986，ISO/IEC 9075:1986，SQL-86 
2. 1989年，ANSI X3.135-1989，ISO/IEC 9075:1989，SQL-89 
3. 1992年，ANSI X3.135-1992，ISO/IEC 9075:1992，SQL-92（SQL2） 
4. 1999年，ISO/IEC 9075:1999，SQL:1999（SQL3） 
5. 2003年，ISO/IEC 9075:2003，SQL:2003
6. 2008年，ISO/IEC 9075:2008，SQL:2008
7. 2011年，ISO/IEC 9075:2011，SQL:2011



