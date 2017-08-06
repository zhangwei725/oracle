# 序列

## 一 、概要

​	是oacle提供的用于产生一系列唯一数字的数据库对象。自动提供唯一的数值。主要用于提供主键值,将序列值装入内存可以提高访问效率,像SQL Server、MySQL、DB2提供了这样一张自动增长的机制(Oracle 12C 之后提供),

## 二、语法格式

1. 格式

   ```
   CREATE SEQUENCE sequence_name

   [START WITH num]

   [INCREMENT BY increment]

   [MAXVALUE num|NOMAXVALUE]

   [MINVALUE num|NOMINVALUE]

   [CYCLE|NOCYCLE]

   [CACHE num|NOCACHE]
   ```

2. 语法解释

   1、START WITH：从某一个整数开始，升序默认值是1，降序默认值是-1。

   2、INCREMENT BY：增长数。如果是正数则升序生成，如果是负数则降序生成。升序默认值是1，降序默认值是-1。

   3、 MAXVALUE：指最大值。

   4、 NOMAXVALUE：这是最大值的默认选项，升序的最大值是：1027，降序默认值是-1。

   5、 MINVALUE：指最小值。

   6、NOMINVALUE：这是默认值选项，升序默认值是1，降序默认值是-1026。

   7、CYCLE：表示如果升序达到最大值后，从最小值重新开始;如果是降序序列，达到最小值后，从最大值重新开始。

   8、 NOCYCLE：表示不重新开始，序列升序达到最大值、降序达到最小值后就报错。默认NOCYCLE。

   9、CACHE：使用CACHE选项时，该序列会根据序列规则预生成一组序列号。保留在内存中，当使用下一个序列号时，可以更快的响应。当内存中的序列号用完时，系统再生成一组新的序列号，并保存在缓存中，这样可以提高生成序列号的效率。Oracle默认会生产20个序列号。

   10、 NOCACHE：不预先在内存中生成序列号。

## 三、示例代码

1. 创建一个从1开始，默认最大值，每次增长1的序列，要求NOCYCLE，缓存中有10个预先分配好的序列号

   ```mysql
   CREATE SEQUENCE SEQ_EMP_EMPNO
   MINVALUE 1 --最小值1
   START WITH 1 --开始计数
   NOMAXVALUE --没有最大值
   INCREMENT BY 1 --自增1
   NOCYCLE --不循环
   NOCACHE --不缓存
   ```

2. 序列使用

   ```mysql
   --使用序列
   SELECT  SEQ_EMP_EMPNO.NEXTVAL FROM DUAL
   --查看当前序列
   SELECT SEQ_EMP_EMPNO.CURRVAL FROM DUAL
   --注意要显示用NEXTVAL才能使用CURRVAL
   ```

## 四、修改序列

### 4.1、注意事项：

1. 不能修改序列的初始值。
2. 最小值不能大于当前值。
3. 最大值不能小于当前值。

### 4.2、示例代码

1. 修改序列

   ```
   ALTER SEQUENCE SEQ_EMP_EMPNO
   MAXVALUE 10000
   MINVALUE -300
   ```

2. 删除序列

   ```
   DROP SEQUENCE SEQ_EMP_EMPNO
   ```