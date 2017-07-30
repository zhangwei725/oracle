# 三、限定查询（控制行）

## 3.1、概要

​	当数据返回查询结果之后，所有的数据默认情况下是按照雇员编号排序的，当然，现在也可以使用”ORDER BY”子句指定所需要的排序的操作列排序的方式主要有两种

1. 排序

   ```
   升序（ASC）：默认，不写也是升序；
   ```

2. 降序排序

   ```
   降序（DESC）：用户需要指定，由大到小排序；
   ```

## 3.2、语法格式

```
SELECT [DISTINCT] * | 列名 [别名] ,列名 [别名] , --3、控制要显示的数据列
FROM 表名称 [别名]             		           --1、确定数据来源
[WHERE 条件(s)] ;        					     -- 2、根据判断条件选择参与的数据行
[ORDER BY 字段 [ASC|DESC] [,字段 [ASC|DESC],…]]; 4 、排序
```

## 3.3 示例代码

1. 说明

   ORDER BY 子句永远是最后执行,所以一般写在所有的SQL语句的最后

2. 示例代码

   1、查询所有的雇员的信息，要求按照工资排序

   ```
   SELECT * 
   FROM emp
   ORDER BY sal;
   等价于
   SELECT *
   FROM emp 
   ORDER BY sal ASC;
   ```

   2、按工资降序对雇员信息进行排序 

   ```
   SELECT * 
   FROM emp
   ORDER BY sal DESC
   ```

   3、查询出所有的雇员信息，按照工资由高到低排序，如果工资相同，则按照雇佣日期由早到晚排序,此时肯定需要两个字段排序：工资（DESC），雇佣日期（ASC）

   ```
   SELECT * 
   FROM emp 
   ORDER BY sal DESC, hiredate ASC
   ```

   4、查询出所有的雇员信息,按照年薪升序排序

   ```
   SELECT empno,ename,(sal*12) income 
   FROM emp 
   ORDER BY income ;
   ```

   5、根据deptno降序排列

   ```
   SELECT * 
   FROM emp 
   ORDER BY deptno DESC;
   ```

   6、根据empno升序排列

   ```
   SELECT * 
   FROM emp 
   ORDER BY empno;
   ```

   7、查询部门为30是所有员工的年薪按降序排序(使用列编号排序)

   ```
   SELECT ename,sal*12 
   FROM emp 
   WHERE deptno=20 
   ORDER BY 2 DESC

   ```

   ​