### 7.1、Oracle开发工具

1. SQL\*Plus\(前期学习用,开发中不用\)

   是Oracle自带的管理工具，在上一节中命令行便是调用了该工具。当然，这个工具也可以在开始菜单中找到，直接打开。请注意登录的时候，无法用sys登录

2. Oracle SQL Developer\(推荐\)

   [官方网址](http://www.oracle.com/technology/software/products/sql/index.html)

   ```
   说明：Oracle公司出的管理软件，算是官方管理软件吧！只支持Oracle数据库
   ```

3. Navicat Premium \(推荐\)

   [官方网址](http://www.navicat.com)

   说明：个人现在的最爱，软件小巧，速度快捷，是一个集成数据库管理软件，可以管理MySQL、Oracle、PostgreSQL三种数据库。

### 7.2、数据库

1. 简介

   数据库是数据集合。Oracle是一种数据库管理系统，是一种关系型的数据库管理系统。

   通常情况了我们称的“数据库”，并不仅指物理的数据集合，他包含物理数据、数据库管理系统。也即物理数据、内存、操作系统进程的组合体。

2. 全局数据库名

   就是一个数据库的标识，在安装时就要想好，以后一般不修改，修改起来也麻烦，因为数据库一旦安装，数据库名就写进了控制文件，数据库表，很多地方都会用到这个数据库名。

3. 启动数据库：

   也叫全局数据库，是数据库系统的入口，它会内置一些高级权限的用户如SYS，SYSTEM等。我们用这些高级权限账号登陆就可以在数据库实例中创建表空间，用户，表了。

4. 查询当前数据库名：

   ```
   select name from v$database;
   ```

### 6.3、数据库实例

1. 概要

   官方描述实例是访问Oracle数据库所需的一部分计算机内存和辅助处理后台进程，是由进程和这些进程所使用的内存\(SGA\)所构成一个集合.其实就是用来访问和使用数据库的一块进程。它只存在于内存中。就像Java中new出来的实例对象一样。

   我们访问Oracle都是访问一个实例，但这个实例如果关联了数据库文件，就是可以访问的，如果没有，就会得到实例不可用的错误。

   实例名指的是用于响应某个数据库操作的数据库管理系统的名称。她同时也叫SID。实例名是由参数instance\_name决定的

2. 查询当前数据库实例名:就是数据库安装的时候设置的名字默认是orcl

   ```
   select instance_name from v$instance;
   jdbc:oracle:thin:@localhost:1521:orcl（orcl就为数据库实例名）
   ```

   注:一个数据库可以有多个实例，在作数据库服务集群的时候可以用到

### 6.4、表空间\(TableSpace\)

#### 6.4.1、概要

​ 表空间是最大的存储结构，它对应一个或多个数据文件，表空间的大小是它所对应的数据文件大小的总和，Oracle推荐将不同数据文件放进不同的表空间，一方面可以提高数据访问性能，另一个方面便于数据管理，备份，恢复操作,一个数据库实例可以有N个表空间，一个表空间下可以有N张表,用户可以自己去创建,不指定系统默认创建,

#### 6.4.2、主要作用

1. 控制数据库数据磁盘分配:表空间可以拥有多个数据文件，然而数据文件可以存储在不同的磁盘中，从而达到了控制数据磁盘分配的目的

2. 限制用户在表空间中可以使用的磁盘空间大小

#### 6.4.3、常见表空间分类

| 名称 | 作用 |
| :--- | :--- |
| System系统表空间 | 存放关于表空间的名称，控制文件，数据文件等管理信息 |
| Temp临时表空间 | 存放临时表和临时数据 |
| Users用户表空间 | 存放对应的用户的表 |
| Experience示例表空间 | 用于存放我们安装Oracle时候创建的示例用户模式的数据信息 |

#### 6.4.4、创建表空间语法

1. 语法

   ```
   Create TableSpace 表空间名称  
   DataFile 
     表空间数据文件路径  
   Size 
     表空间初始大小  
   Autoextend on
   ```

2. 示例代码

   ```
   create tablespace db_test  
   datafile 'D:\oracle\zhangwei\product\11.2.0\dbhome_1\userdata\db_test.dbf'  
   size 50m  
   autoextend on;
   ```

3. 查看已经创建好的表空：

   ```
   select default_tablespace, temporary_tablespace, d.username  
   from dba_users d
   ```

### 6.5、Oracle用户

#### 6.5.1、概要

​ 详细内容查看附件Oralce用户管理

​ Oracle数据库建好后，要想在数据库里建表，必须先为数据库建立用户，并为用户指定表空间,

#### 6.5.2、创建用户

1. 语法

   ```
   CREATE USER 
     用户名  
   IDENTIFIED BY 
     密码  
   DEFAULT TABLESPACE 
    表空间(默认USERS)  
   TEMPORARY TABLESPACE 临时表空间(默认TEMP)
   ```

2. 示例代码：

   ```
   CREATE USER test  
   IDENTIFIED BY 123  
   DEFAULT TABLESPACE db_test  
   TEMPORARY TABLESPACE temp;
   ```

3. 有了用户，要想使用用户账号管理自己的表空间，还得给它分权限：

   ```
   GRANT CONNECT TO test;  
   GRANT RESOURCE TO test;  
   GRANT dba TO test;--dba为最高级权限，可以创建数据库，表等。
   ```

4. 查看数据库用户：

   ```
   select  * from dba_users;
   ```

### 6.6、表

#### 6.6.1 、定义

​ 表示关系型数据库中基本的数据结构，它是行的集合。表中的每一行包含一个或者多个列，而表中的一行数据可以理解为数据库中的一个记录

#### 6.6.2、示例图

![](http://opzv089nq.bkt.clouddn.com/17-7-29/83631486.jpg)

有了数据库，表空间和用户，就可以用自定义的用户在自己的表空间创建表了。有了表，我们可以开发了。

