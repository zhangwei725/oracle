# 插入操作

## 1、作用

1. 为表插入单行数据
2. 也可以通过子查询将一张表的多行数据插入到另外一张表里面
3. 同时为多张表插入数据

## 2、基础插入数据

### 2.1、语法格式

```
INSERT INTO 表或视图 [(列名,列名,列名,...)] 
VALUE (值,值,值,...)
```

### 2.2、示例代码

1. dept 表中插入 dept 表中 deptno=100 的数据\(了解\)

   ```
   insert into dept select * from dept where deptno=100;
   ```

2. 往 emp 表中插入 empno,ename,sal 数据

   ```
   (111,'1',1000)(222,'2',2000)
   INSERT INTO emp(empno,ename,sal) values(111,'1',1000);
   insert INTO emp(empno,ename,sal) values(222,'2',2000);
   ```

   ​

### 2.3、说明

1. 添加数字：直接编写数字111，

   ```
   INSERT INTO emp(empno,ename,sal) values(111,'1',1000);
   ```

2. 添加字符串：字符串应该使用单引号''声明

   ```
   INSERT INTO emp(ename) values('苍井空');
   ```

3. 添加日期

   第一种：可以按照已有的字符串的格式编写字符串，例如’17-12月-80’；  
   第二种：利用TO\_DATE\(\)函数将字符串变为DATE型数据；  
   第三种：如果设置的时间为当前系统时间，则使用SYSDATE；

## 3、批量插入\(10g或以上版本\)

### 3.1、语法格式

1. 往一个表中插入多条记录

   ```
   INSERT ALL 
   INTO 表名 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表名 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表名 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   SELECT * FROM dual;
   ```

2. 插入多个值到多个表中

   ```
   INSERT ALL 
   INTO 表1 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表1 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表1 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表2 (列名1, 列名2, 列名3)  VALUES ('VALUE1', 'VALUE2', 'VALUE3') 
   INTO 表2 (列名1, 列名2, 列名3) VALUES ('VALUE1', ''VALUE2', 'VALUE3') 
   SELECT * FROM dual;
   ```

### 3.2、示例代码

1. 往emp表中批量插入数据

   ```
   INSERT ALL 
   INTO emp (EMPNO, ENAME, JOB,SAL)  VALUES (1234, 'test1','IT', 2850.12)
   INTO emp (EMPNO, ENAME, JOB,SAL)  VALUES (1233, 'test2','IT',  9200.12)
   INTO emp (EMPNO, ENAME, JOB,SAL)  VALUES (1231, 'test3', 'IT', 9521.12)
   SELECT * FROM DUAL;
   ```

2. 往emp,dept表中批量插入数据

   ```
   INSERT ALL 
   INTO emp (EMPNO, ENAME, JOB,SAL,DEPTNO)  VALUES (1234, 'test1','IT', 2850.12,50)
   INTO emp (EMPNO, ENAME, JOB,SAL,DEPTNO)  VALUES (1233, 'test2','IT',  9200.12,50)
   INTO emp (EMPNO, ENAME, JOB,SAL,DEPTNO)  VALUES (1231, 'test3', 'IT', 9521.12,50)
   INTO DEPT (DEPTNO,DNAME,LOC) VALUES(50,'IT','shanghai')
   SELECT * FROM DUAL;
   ```

## 4、注意事项

1. INSERT时既可以指定列，也可以不指定列表

   1、如果不指定列表,则values子句必须为table中的每个列提供数据,且数据顺序与列顺序相同

   2、如果指定列表,提供的数据的顺序需与相应列对应

2. 数字列可直接写入，字符列或日期列插入数据时必须使用单引号引住

3. 插入数据必须满足约束规则，主键列和NOT NULL列必须提供数据值

4. 插入的数据必须与列的个数及顺序保持一致



