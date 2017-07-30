# 四、函数查询

## 4.1、概要

​    函数是一种有零个或多个参数并且有一个返回值的程序,函数主要分为两大类单行函数,多行函数\(聚合函数\)

## 4.2、单行函数

### 4.2.1、定义

​    单行函数: 对每一个函数应用在表的记录中时，只能输入一行结果，返回一个结果。

### 4.2.2、分类

1. 字符函数
2. 数值函数
3. 日期函数
4. 转换函数
5. 通用函数

### 4.2.3、字符函数

#### 4.2.3.1、UPPER

1. 说明

   ```
   将输入的字符串变为大写返回；
   ```

2. 作用

   ```
   在一般的使用之中，用户输入数据的时候去关心数据本身存放的是大写还小写
   ```

3. 语法

   ```
   upper（列 | 字符串）
   ```

4. 示例代码

   1、将abcd改变成大写

   ```
   SELECT UPPER('abcd') FROM dual
   ```

   2、查询姓名为simth的员工信息

   ```
   SELECT * 
   FROM  emp 
   WHERE ename = UPPER('&str');
   ```

#### 4.2.3.2、LOWER

1. 说明

   ```
   将输入的字符串变为小写返回
   ```

2. 语法格式

   ```
   LOWER(字符串 | 列)
   ```

3. 示例代码

   1、将员工姓名转化成小写显示

   ```
   SELECT lower(ename)
   FROM  emp
   ```

#### 4.2.3.3、REPLACE

1. 说明

   ```
   字符串进行替换
   ```

2. 语法格式

   ```
   REPLACE(字符串 | 列)
   ```

3. 示例代码

   使用字母'\*'替换掉姓名中的所有字母's'

   ```
   SELECT REPLACE(ename,'S','*') FROM emp
   ```

#### 4.2.3.4、LENGTH

1. 说明

   ```
   求出字符串的长度
   ```

2. 语法格式

   ```
   LENGTH(字符串 | 列)
   ```

3. 示例代码

   查询出每个雇员姓名的长度

   ```
   SELECT LENGTH(ename) 
   FROM emp;
   ```

#### 4.2.3.5、INITCAP

1. 说明

   ```
   首字母大写
   ```

2. 语法格式

   ```
   INITCAP(字符串 | 列)
   ```

3. 示例代码

   将员工的姓名全部大写字母开头

   ```
   SELECT initcap(ename) 
   FROM emp;
   ```

#### 4.2.3.6、SUBSTR

1. 说明

   ```
   字符串截取,开始点可以是正也可以是负,如果是负表示从后面开始截取 ,如果长度不写,默认截取到末尾
   ```

2. 语法格式

   ```
   SUBSTR(字符串 | 列，开始点, 长度)
   ```

3. 示例代码

   1、从开始点一直截取到结尾

   ```
   SELECT SUBSTR('abcdefg',2) from  dual; --bcdefg
   ```

   2、从开始点截取到结束点，截取部分内容

   ```
   SELECT SUBSTR('abcdefg',2,4) from  dual;--bcde
   ```

   3、要求截取每个雇员姓名的后2个字母

   ```
   SELECT ename,SUBSTR(ename,LENGTH(ename)-1) FROM emp;
   等价于
   SELECT ename,SUBSTR(ename,-2) FROM emp;
   ```

### 4.2.4、数值函数

#### 4.2.4.1、ROUND

1. 说明

   ```
   四舍五入的操作 默认保留0位
   ```

2. 语法格式

   ```
   ROUND(数字 | 列 [,保留小数的位数])
   ```

3. 示例代码

   ```
   SELECT  ROUND(100.12),
   ROUND(100.12 ,1) ,
   ROUND(-100.56), 
   ROUND(-100.56,1), 
   ROUND(-100.567123,3)
   FROM DUAL;

   100           100.1           -101           -100.6             -100.567
   ```

#### 4.2.4.2、TRUNC

1. 说明

   ```
   舍弃指定位置的内容
   ```

2. 语法格式

   ```
   TRUNC(数字 | 列 [,保留小数的位数])
   ```

3. 示例代码

   ```
   SELECT TRUNC(903.53567),TRUNC(-903.53567), TRUNC(903.53567,2), TRUNC(-90353567,-1) 
   FROM dual;

   903              -903             903.53           -90353560
   ```

#### 4.2.4.3、MOD

1. 说明

   ```
   取余数
   ```

2. 语法

   ```
   MOD(数字 1，数字2)
   ```

3. 示例代码

   ```
   SELECT MOD(10,3) FROM dual
   ```

### 4.2.5、日期函数

#### 4.2.5.1、MONTHS\_BETWEEN

1. 说明

   ```
   返回两个日期类型数据之间间隔的自然月数
   ```

2. 语法格式

   ```
   MONTHS_BETWEEN（日期1，日期2）
   ```

3. 示例代码

   ```
   SELECT MONTHS_BETWEEN('1-6月 -87','1-5月 -87') FROM dual;
   1
   ```

#### 4.2.5.2、ADD\_MONTHS

1. 说明

   ```
   在某一个日期 d 上，加上指定的月数 n，返回计算后的新日期
   ```

2. 语法格式

   ```
   ADD_MONTHS(d,n)
   ```

3. 示例代码

   ```
   SELECT SYSDATE ,ADD_MONTHS(SYSDATE ,1) FROM DUAL;
   ```

#### 4.2.5.3、LAST\_DAY

1. 说明

   ```
   返回指定日期当月的最后一天
   ```

2. 语法格式

   ```
   LAST_DAY(d)
   ```

3. 示例代码

   ```
   SELECT SYSDATE,last_day(SYSDATE) FROM dual
   ```

### 4.2.6、转换函数

转换函数将值从一种数据类型转换为另外一种数据类型

#### 4.2.6.1、TO\_CHAR

1. 说明

   ```
   把日期和数字转换为制定格式的字符串。Fmt是格式化字符串
   ```

2. 语法格式

   ```
   TO_CHAR(d|n[,fmt])
   ```

3. 示例代码

   对日期的处理

   ```
   SELECT TO_CHAR(SYSDATE,'YYYY"年"MM"月"DD"日" HH24:MI:SS') "date" FROM dual;
   SELECT to_char(sysdate,'yyyy-mm-dd hh24 mi ss')FROM dual;
   SELECT to_char(sysdate,'yyyy-mm-dd hh:mi:ss') FROM dual;
   SELECT to_char(sysdate,'yyyy-mm-dd hh') FROM dual;
   SELECT to_char(sysdate,'yyyy') FROM dual;
   ```

#### 4.2.6.2、TO\_DATE

1. 说明

   ```
   主要功能是将一个字符串变为DATE型数据
   ```

2. 语法格式

   ```
   TO_DATE(X,[,fmt])
   ```

3. 示例代码

   将字符串日期转化为年月日

   ```
   SELECT TO_DATE('1986-07-25 ','yyyy-mm-dd') FROM dual;
   25-7月 -86
   ```

   将字符串日期+时间转化成年与日时分秒

   ```
   SELECT TO_date('2015-12-25,13:25:59' 'YYYY-MM-DD HH24:MI:SS') FROM dual;
   2015-12-25 13:25:59
   ```

   #### 4.2.6.3、TO\_NUMBER\(字符串\)

### 4.2.7、通用函数

#### 4.2.7.1、NVL

1. 说明

   ```
   如果X为空，返回value，否则返回X
   ```

2. 语法格式

   ```
   NVL(X,VALUE)
   ```

3. 示例代码

   对工资是2000元以下的员工，如果没发奖金，每人奖金100元

   ```
   SELECT ename,job,sal,NVL(comm,100) FROM emp WHERE sal<2000;
   ```

#### 4.2.7.2、NVL2

1. 说明

   ```
   如果x非空，返回value1，否则返回value2
   ```

2. 语法格式

   ```
   NVL2(x,value1,value2)
   ```

3. 示例代码

   对EMP表中工资为2000元以下的员工，如果没有奖金，则奖金为200元，如果有奖金，则在原来的奖金基础上加100元

   ```
   SELECT ENAME,JOB,SAL,NVL2(COMM,comm+100,200) "comm"
   FROM EMP WHERE SAL<2000;
   ```

   ​

#### 4.2.7.3、DECODE

1. 说明

   ```
   函数非常类似于程序中的if…else…语句，唯一不同的是DECODE()函数判断的是数值，而不是逻辑条件
   ```

2. 语法格式

   ```
   ECODE(数值 | 列 ，判断值1，显示值1，判断值2，显示值2，判断值3，显示值3，…)
   ```

3. 示例代码

   现在要求显示全部雇员的职位，但是这些职位要求替换为中文显示：

   CLERK：办事员；

   SALESMAN：销售；

   MANAGER：经理；

   ANALYST：分析员；

   PRESIDENT：总裁；

   ```
   SELECT empno,ename,job,DECODE(job,'CLERK','办事员','SALESMAN','销售人员','MANAGER','经理','ANALYST','分析员','PRESIDENT','总裁')
   FROM emp;
   ```

#### 4.2.7.4、CASE

1. 说明

   ```
   类似我们的swich语句根据条件返回相应的结构,Case函数只返回第一个符合条件的值，剩下的Case部分将会被自动忽略
   ```

2. 语法格式一\(简单Case函数\)

   ```
   CASE 条件
   WHEN 表达式1 THEN 结果1

   WHEN 表达式2 THEN 结果2
   ...
   WHEN 表达式N THEN 结果N

   ELSE 默认值 
   END  [别名]
   ```

3. 语法二\(Case搜索函数\)

   ```
   CASE  
   　　WHEN condition1 THEN result1  
   　　WHEN condistion2 THEN result2  
   　　...  
   　　WHEN condistionN THEN resultN  
   　　ELSE default_result  
   END
   ```

4. 示例代码

   1、根据员工的部门编号查询员工所属的部门中文显示

   ```
   SELECT ename ,job ,
   CASE   deptno
   WHEN  10 THEN '财务部'
   WHEN  20 THEN '市场调研部'
   WHEN  30 THEN '销售部'
   WHEN  40 THEN '运营部'
   ELSE '其他部门' 
   end "所属部门"
   FROM emp;
   ```

   2、统计每一个员工的工资等级

   ```
   SELECT  ENAME  ,
       CASE   
       WHEN sal> 700 AND sal <= 1200  THEN '1'   
       WHEN sal > 1201 AND sal <= 1400  THEN '2'   
       WHEN sal > 1401 AND sal <= 2000 THEN '3'  
       WHEN sal > 2001 AND sal <= 3000 THEN '4'  
       WHEN sal > 3001 AND sal <= 9999 THEN '5'  
     ELSE '0'
     END grade -- 别名命名
   FROM EMP
   ```

   ​



