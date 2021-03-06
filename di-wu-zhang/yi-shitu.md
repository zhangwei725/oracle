# 视图

## 一、定义

​    也称虚表，视图是表中数据的逻辑表示。视图本身并不存储任何数据（，而真正的数据是存在于基表中的。视图也和表一样，也带有名称的列和行。简单的说，视图就是一个展示的窗口，它可以从这个表拿点数据，从另一个表拿点数据，进行展示。

## 二、为什么要使用视图

1. 提供各种数据表现形式,

   ```
   可以使用各种不同的方式将基表的数据展现在用户面前, 以便符合用户的使用习惯(使用别名).
   ```

2. 隐藏数据的逻辑复杂性并简化查询语句

   ```
   多表查询语句一般是比较复杂的, 而且用户需要了解表之间的关系, 否则容易写错; 
   如果基于这样的查询语句创建一个视图, 用户就可以直接对这个视图进行"简单查询"而获得结果. 
   这样就隐藏了数据的复杂性并简化了查询语句
   ```

3. 提供一定安全性保证.

   ```
   视图提供了一种可以控制的方式, 即可以让不同的用户看见不同的列, 而不允许访问那些敏感的列, 
   这样就可以保证敏感数据不被用户看见.
   ```

4. 简化用户权限的管理.

   ```
   可以将视图的权限授予用户, 而不必将基表中某些列的权限授予用户, 这样就简化了用户权限的定义
   ```

## 三、分类

### 1、简单视图

1. 简单视图只从单表里获取数据
2. 简单视图不包含函数和数据组
3. 简单视图可以实现DML操作

### 2、复杂视图

1. 复杂视图从多表中获取数据
2. 复杂视图包含是函数和数据组
3. 复杂视图不可以DML操作

## 四、创建视图

1. 语法格式

   ```
   CREATE [OR REPLACE] VIEW 视图名称 
   AS 子查询 [WITH CHECK OPTIONI] [WITH READ ONLY] ;
   ```

2. 示例代码

   ```
   CREATE  OR REPLACE VIEW VO_DEPT_EMP
   AS  (SELECT  e.ename ,
               e.DEPTNO, 
               e.job,
               e.sal,
               e.mgr,
               d.DNAME, 
               d.loc
         FROM  emp e,DEPT d
         WHERE e.DEPTNO = d.DEPTNO(+))
   WITH READ ONLY;
   ```

   注:如果权限不足,切换到超级管理员用户 GRANT CREATE ANY VIEW  TO  scott;



