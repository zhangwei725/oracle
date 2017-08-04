# 一、修改(更新)

## 1、作用

​	对表中已有的数据行进行操作

## 2、语法格式

```
UPDATE 表名称 

SET 更新字段1=更新值1,更新字段2=更新值2,… 

[WHERE 更新条件(s)];
```

## 3、说明

1. 编写 UPDATE 语句的时候，没有编写任何的更新条件，则表示更新表之中的全部数据，进行所有数据更新的时候，都会返回数据的更新行数，如果是增加，会显示影响的行数，同样对于删除也 会出现影响的行数。
2. 在以后编写程序的时候就可以通过是否有更新行数来判断某一个更新操作是否成功

## 4、示例代码

1. 把 empno=111 的员工 comm 改成 100

   ```mysql
   UPDATE emp set comm=100 WEHER empno=111;
   ```

2. 为所有人长工资，标准是：10 部门长 10%；20 部门长 15%；30 部门长 20%其他部门长 18%（要求用 DECODE 函数）

   ```mysql
   UPDATE emp SET sal=decode(deptno,'10',sal(1+0.1), '20',sal(1+0.15), '30',sal(1+0.2),sal(1+0.18));
   ```

3. 根据工作年限长工资，标准是：为公司工作了几个月就长几个百分点

   ```mysql
   update emp set sal= round(sal * (1+(sysdate - hiredate)/365/12/100),2);
   ```

## 5、使用子查询插入

1. 使雇员SCOTT的岗位、工资、补助与雇员SMITH的完全相同

   ```mysql
   UPDATE emp SET (job,sal,comm)=(SELECT job,sal,comm FROM emp WHERE ename='SMITH') WHERE ename='SCOTT';
   ```

# 二、删除操作

## 1、说明

```
没有删除条件删除所有数据
```

## 2、语法格式

```
DELETE FROM 表名称
[WHERE 删除条件(s)];
```

## 3、示例代码

1. 删除 empno=111 的数据

   ```
   delete from emp where empno=111;
   ```

## 4、注意:

```
在开发中我们都不会使用delete删除数据,通常是假删除数据,通过设置字段来判断
```