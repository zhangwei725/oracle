

# 创建表

## 1、创建表空间

### 1.1、概述

​	Oracle数据库中所有的数据都存在于表空间内。表空间是一个逻辑的结构，每个表空间都是由叫做数据文件的结构组成，每个表空间必须包含一个或者多个数据文件。每一个数据文件仅属于一个表空间,表空间是一个容器，类似于仓库的货架。用来分类存放数据库对象

### 1.2、语法格式

```
CREATE TABLESPACE 表空间名字 
DATAFILE '文件路径' 
SIZE 大小 
[AUTOEXTEND ON] 
[NEXT 大小]
[MAXSIZE 大小];
```

### 1.3、示例代码

1. 创建临时表空间

   ```
   CREATE temporary tablespace zhangwei_temp
   tempfile 'D:\oracle\zhangwei\oradata\orcl\zhangwei_temp.dbf'
   SIZE 50m
   autoextend ON
   next 50m maxsize 20480m
   extent management local;
   ```

2. 创建表空间

   ```
   create tablespace zhangwei_data 
   logging 
   datafile 'D:\oracle\zhangwei\oradata\orcl\zhangwei_data.dbf'
   size 50m 
   autoextend on 
   next 50m maxsize 20480m 
   extent management local; 
   ```

3. 创建用户并指定表空间

   ```
   create user zhangwei identified by 123 
   default tablespace zhangwei_data 
   temporary tablespace zhangwei_temp; 
   ```

4. 给用户授予权限

   ```
   grant connect,resource,dba to zhangwei; 
   ```

## 2、创建表

2.1、语法格式

1. 格式1(常用)

   ```
   CREATE TABLE [用户名.]表名
   (
   列名  数据类型 [约束],
   列名  数据类型 [约束],...
   ) 
   TABLESPACE 表空间名;
   ```

2. 完整格式

   ```
   CREATE  TABLE  [schema.]table  
   (column  datatype [, column  datatype] … )  
   [TABLESPACE  tablespace]  
   [PCTFREE  integer]  
   [PCTUSED  integer]  
   [INITRANS  integer]  
   [MAXTRANS  integer]  
   [STORAGE  storage-clause]  
   [LOGGING | NOLOGGING]  
   [CACHE | NOCACHE] ];  
   ```

   说明

   1、Schema：表的所有者
   2、Table：表名
   3、Column：字段名
   4、Datatype：字段的数据类型
   5、Tablespace：表所在的表空间
   6、Pctfree：为了行长度增长而在每个块中保留的空间的量（以占整个空间减去块头部后所剩余空间的百分比形式表示），当剩余空间不足pctfree时，不再向该块中增加新行。
   7、Pctused：在块剩余空间不足pctfree后，块已使用空间百分比必须小于pctused后，才能向该块中增加新行。
   8、INITRANS：在块中预先分配的事务项数，缺省值为1
   9、MAXTRANS：限定可以分配给每个块的最大事务项数，缺省值为255
   10、STORAGE：标识决定如何将区分配给表的存储子句
   11、LOGGING：指定表的创建将记录到重做日志文件中。它还指定所有针对该表的后续操作都将被记录下来。这是缺省设置。
   12、NOLOGGING：指定表的创建将不被记录到重做日志文件中。
   13、CACHE：指定即使在执行全表扫描时，为该表检索的块也将放置在缓冲区高速缓存的LRU列表最近使用的一端。
   14、NOCACHE：指定在执行全表扫描时，为该表检索的块将放置在缓冲区高速缓存的LRU列表最近未使用的一端。
   15、STORAGE子句：
   ​	INITIAL：初始区的大小
   ​	NEXT：下一个区的大小
   ​	PCTINCREASE：以后每个区空间增长的百分比
   ​	MINEXTENTS：段中初始区的数量
   ​	MAXEXTENTS：最大能扩展的区数

## 3、示例代码

1.  需求

   | 序号   | 字段名       | 数据类型     | 长度   | 为空(是/否) | 唯一(是否) | 默认值  | 说明    |
   | ---- | --------- | -------- | ---- | ------- | ------ | ---- | ----- |
   | 1    | aid       | NUMBER   | 10   | N       | Y      |      | 用户ID  |
   | 2    | name      | varchar2 | 25   | N       | Y      |      | 用户登陆名 |
   | 3    | password  | varchar2 | 50   | N       | N      |      | 密码    |
   | 4    | email     | varchar2 | 20   | Y       | N      |      | 邮箱    |
   | 5    | phone     | char     | 11   | N       | N      |      | 手机    |
   | 6    | balance   | NUMBER   | 6,2  | Y       | N      | 0    | 账户余额  |
   | 7    | real_name | Varchar2 | 25   | Y       | N      |      | 真实姓名  |

2.  建表语句

   ```
   CREATE TABLE T_ACCOUNT(
    AID NUMBER(10) PRIMARY KEY, --用户ID
    NAME  VARCHAR2(25) UNIQUE, --登陆用户名
    PASSWORD VARCHAR2(32),-- 登陆密码
    EMAIL  VARCHAR2(64), --邮箱
    phone  CHAR(11), --手机号
    isDelete  NUMBER(1), -- 是否删除
    CONSTRAINT UK_ACCOUNT_NAME UNIQUE(NAME)
   )
   ```

3. 使用子查询创建表(了解)

   ```
   CREATE TABLE 表名称 AS 子查询

   CREATE TABLE t_emp AS SELECT * FROM emp;
   ```



