## 一、分组函数

### 1.1、什么叫分组函数

​    聚合函数也叫分组函数，有的也叫集合函数，它的数据源一般来自多组数据，但返回的时候一般是一组数据，聚合函数对一组行中的某个列执行计算并返回单一的值。聚合函数经常与 SELECT 语句的 GROUP BY 子句一同使用,所以有的时候也把其称之为分组函数

### 2.1、分类

| 函数名称 | 返回值\(结果\)类型 | 说明 |
| :--- | :---: | :---: |
| MAX\(DISTINCT\|ALL 列\) | 数值 | 对所有数值求和 |
| COUNT\(\[DISTINCT\|ALL\] 列\)\|COUNT\(\*\); | 数值 | 求非空的记录、数据个数 |
| MAX\(\[DISTINCT\|ALL\] 数值日期列\); | 数值 | 求最大值 |
| MIN\(\[DISTINCT\|ALL\] 数值日期列\); | 数值 | 求最小值 |
| AVG\(\[DISTINCT\|ALL\] 数值列\); | 数值 | 求平均值 |

### 2.2、 SUM\(求总和\)

#### 2.2.1、 说明

1. ALL表示对所有值求和
2. DISTINCT表示只对不同值求和\(相同值只取一次\)

#### 2.2.2、示例代码

1. 计算雇员姓名为 'SMITH'和 'ALLEN' 两个人的基本薪资和

   ```
   SELECT SUM(sal) 
   FROM emp 
   WHERE ename IN('SMITH','ALLEN');
   ```

### 2.3、 COUNT\(统计行数\)

#### 2.3.1、 说明

1. ALL对所有记录，数组做统计
2. DISTINCT只对不同值统计\(相同值只取一次\)

#### 2.3.2、 示例代码

1. 显示emp表中的总条数据

   ```
   SELECT COUNT(*) 
   FROM emp
   ```

2. 统计 emp  职位类型的个数。

   ```
   SELECT COUNT(DISTINCT job) 
   FROM emp;
   ```

3. 统计 emp 职位为 SALESMAN 的雇员个数

   ```
   SELECT COUNT(*) 
   FROM emp 
   WHERE job='SALESMAN';
   ```

4. 统计 emp 中 有佣金的雇员的个数

   ```
   SELECT COUNT(comm) 
   FROM emp;
   ```

### 2.4、 MAX\(求最大值\)

#### 2.4.1、 说明

1. ALL表示对所有的值求最大值 默认值
2. DISTINCT表示对不同的值求最大值,相同的只取一次

#### 2.4.2、 示例代码

1. 查询所有雇员中最高的薪资

   ```
   SELECT　MAX(sal)
   FROM emp;
   ```

2. 显示所有工资不同的员工中工资最高的

   ```
   SELECT MAX(DISTINCT SAL) 
   FROM EMP;
   ```

### 2.5、 AVG\(求平均值\)

#### 2.5.1、 说明

1. ALL表示对所有的值求最大值 默认值
2. DISTINCT表示对不同的值求最大值,相同的只取一次

#### 2.5.2、 示例代码

1. 求所有员工工资的平均值

   ```
   SELECT  (sal) 
   FROM  emp;
   ```

2. 求不重复的员工工资的平均值

   ```
   SELECT  AVG(DISTINCT sal) 
   FROM  emp;
   ```

### 2.6 单行函数和聚合函数的区别

### 2.7、单行函数和聚合函数的区别

1. 单行函数操作时，根据函数的功能同时处理一行数据，返回每一行的处理结果；

2. 聚合函数同时对分组后的一组行进行操作，返回分组后各组的处理结果



