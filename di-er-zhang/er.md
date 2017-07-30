## 一、 前提准备工作

​ 为了方便初学者,教程以Oracle下示例方案下的Scott用户下的表为例

​ scott 用户一共有4张数据表，以后讲解 DML 语法都将用到这四张表。所以大家先记住这4张数据表的表结构。

1. 查询用户下的所有数据表

   ```
   SELECT * 
   FROM tab;
   ```

2. 查询一个数据表的结构

   ```
   DESC 表名;
   ```

## 二、Scott 案例下的表

### 2.1、部门信息表：dept

| 列名 | 类型 | 描述 |
| :--- | :--- | :--- |
| DEPTNO | NUMBER\(2\) | 部门编号；最多由2位数字组成 |
| DNAME | VARCHAR2\(14\) | 部门名称；最多由14个字节组成 |
| LOC | VARCHAR2\(13\) | 部门所在的位置地址；最多由13个字节组成 |

### 2.2、员工\(雇员\)信息表：emp\(employees\)

| 列名 | 类型 | 描述 |
| :--- | :--- | :--- |
| EMPNO | NUMBER\(4\) | 员工编号;由4位数字组成 |
| ENAME | VARCHAR2\(10\) | 员工姓名 |
| JOB | VARCHAR2\(9\) | 员工职位 |
| MGR | NUMBER\(4\) | 员工领导的编号 |
| HIREDATE | DATE | 员工雇佣日期；DATA 包含了日期和时间 |
| SAL | NUMBER\(7,2\) | 基本工资；如：99999.99 |
| COMM | NUMBER\(7,2\) | 佣金\(提层\)，一般只有销售才有佣金的概念。 |
| DEPTNO | NUMBER\(2\) | 员工所在的部门编号。和 dept表的 DEPTNO对应 |

### 2.3、工资等级表：salgrade（Salary Grade）

| 列名 | 类型 | 描述 |
| :--- | :--- | :--- |
| GRADE | NUMBER | 工资等级编号 |
| LOSAL | NUMBER | 该等级的最低工资 |
| HISAL | NUMBER | 该等级的最高工资 |

### 2.4、工资表：bonus

| 列名 | 类型 | 描述 |
| :--- | :--- | :--- |
| ENAME | VARCHAR2\(10\) | 员工名字 |
| JOB | VARCHAR2\(9\) | 职位 |
| SAL | NUMBER | 基本工资 |
| COMM | NUMBER | 佣金 |

## 三、查询\(控制列\)

### 3.1、简单查询

#### 3.1.1、语法格式

```
SELECT [DISTINCT] * | 列名 [AS] [别名] ,列名 [AS] [别名] , --2、 控制要显示的数据列
FROM 表名称 [别名] ;                                       --1、 确定查询的数据来源
```

#### 3.1.2、参数说明

```
1、SELECT 关键字 查询数据
2、FROM  关键字 后面跟数据来源
3、 DISTINCT --表示去掉重复行数据 distinct关键字只能跟在select关键字之后
4、* --查询选择所有的列，count（*）意味着计算所有的行，表示通配符时，表示0个或任意多个字符
5、AS --关键字不能用于指定表的别名,可以用来指定列名的别名,指定列的别名的时候可以省略
```

#### 3.1.3、示例代码

1. 查询雇员\(emp\)表的所有数据，即所有行和所有列的数据

   ```
   SELECT * 
   FROM emp
   ```

2. 查询每个雇员\(emp\)的编号\(empno\)、姓名\(ename\)、职位\(job\)、基本工资\(sal\)

   ```
   SELECT empno,ename,job,sal 
   FROM emp;
   ```

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-19/53973878-file_1489905470416_17a70.png)

3. 使用别名方式查询\(开发中禁止使用中文别名,看到用中文别名的直接拖出打死\)

   ```
    SELECT empno 编号,ename 姓名,job 职位,sal 薪资 
    FROM emp;
   ```

![](http://ojx4zwltq.bkt.clouddn.com/17-3-19/72398330-file_1489905740070_14235.png)

1. 去掉重复数据

   ```
   SELECT job 
   FROM emp;
   ```

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-19/21758026-file_1489905837327_acfe.png)

   ```
   SELECT DISTINCT job 
   FROM emp;
   ```

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-19/93556269-file_1489905984399_ac86.png)

### 3.2、简单查询\(+、-、×、÷\)

#### 3.2.1、说明

1. 四则运算可以对数字类型,日期类型,数字类型的字符串进行运算

2. 运算符的优先级跟数学的加，减，乘，除一样

#### 3.2.2 、示例代码

1. 查询每个雇员的编号、姓名、基本年薪

   ```
   SELECT empno,ename,sal*12 
   FROM emp ;
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/23441362.jpg)

2. 查询每个雇员的编号、姓名、职位、年薪，而且每位雇员，每个月有 300 元的饭食补助、180元的交通补助，年底的时候可以领到 14个月的基本工资

   ```
   SELECT empno 编号,ename 姓名 ,job 职位,(sal*14 + (300+180)*12) 年薪 
   FROM emp
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/66782512.jpg)

总结:以上是SQL标准的基本操作但在Oracle里还有一些扩充的操作

### 3.3、连接符 \|\|\(Oralce\)

#### 3.3.1、说明

1. 对数据进行连接

2. 开发基本不用

#### 3.3.2、示例代码

1. 查询 empno 员工编码,ename 员工姓名,sal 员工工资,要求显示在一起

   ```
   SELECT empno || ename || sal 
   FROM emp
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/8206349.jpg)

2. 很显示这样显示没有任何意义\(当然连接本省就没有多大意义\),如果想阅读性稍微好一点可以使用常量

### 3.4、常量使用

#### 3.4.1、 说明

1. 如果常量是数字类型直接编写

2. 如果常量是其他类型则需要使用**单引号**'

#### 3.4.2、示例代码

1. 使用数字常量

   ```
   SELECT  1 
   FROM emp;
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/95262306.jpg)

2. 使用其他类型常量

   ```
   SELECT '员工编号:' 
   FROM emp;
   ```

   ![](file:///Users/zhangwei/Desktop/字符串常量.png?lastModify=1501394015)

3. 结合连接符,查询 empno 员工编码,ename 员工姓名,sal 员工工资,要求显示在一起,每行前面加上常量格式如下

   ```
   SELECT '员工编号: ' || empno 编号 ,
    '员工姓名: ' || ename 姓名, 
    '员工工资: ' || sal 工资,
    '部门: '|| deptno 部门  
   FROM emp;
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/60083369.jpg)

### 3.5、其他

#### 3.5.1 单引号

1. 表示字符串常量

   ```
   select "sysdate" from dual;
   ```

2. 字符串中的双引号仅仅被当作一个普通字符进行处理。此时，双引号不需要成对出现

3. ​动态SQL 在一对单引号包含的语句中，必须有一对相邻的单引号表示一个单引号 两个相邻的单引号的作用，第一个是用来表示转义字符，后面一个表示真正的单引号 单引号里要用单引号应该是两个连续的单引号,而不是双引号

### 3.5.2 双引号

1. 表示其内部的字符串严格区分大小写\(**Oracle将严格区分大小写**，否则Oracl都默认大写\)

2. 用于特殊字符或关键字

   ```
   查询当前系统的时间
   select 'sysdate' from dual;
   ```

3. 不受标识符规则限制

4. 会被当成一个列来处理,当出现在to\_char的格式字符串中时，双引号有特殊的作用，就是将非法的格式符包装起来, 避免出现ORA-01821: date format not recognized错误， to\_char在处理格式字符串时，会忽略双引号

   ```
    select to_char(sysdate, 'hh24"小时"mi"分"ss"秒"') AS RESULT from dual
   ```

   ![](http://opzv089nq.bkt.clouddn.com/17-7-30/80442625.jpg)



