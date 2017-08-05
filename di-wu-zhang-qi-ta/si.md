# 一、常规导入导出

## 1、导出dmp文件

**说明： 导出dmp文件一般用于数据库备份，或者数据迁移**

## 2、导出数据的三种方式

1. 全库导出（这种方式一般不用）
2. 按用户导出
3. 按表导出

## 3、exp常用命令解释

### 1、全库导出

exp test/123456@ORCL full=y file=d:\expdata.dmp log=explog.txt

### 2、按用户导出

exp test/123456@ORCL owner=test file=d:\expdata.dmp log=explog.txt

### 3、按表导出

exp test/123456@ORCL tables=test.test file=d:\expdata.dmp log=explog.txt

## 4、导入数据

### 1、先查询数据库表空间

```
SELECT TBS 表空间名,    
SUM(TOTALM) 总共大小M,   
SUM(USEDM) 已使用空间M,   
SUM(REMAINEDM) 剩余空间M,   
SUM(USEDM) / SUM(TOTALM) 使用率  
FROM (SELECT B.FILE_ID ID,       
B.TABLESPACE_NAME TBS,            
B.FILE_NAME NAME,              
B.BYTES / 1024 / 1024 TOTALM,    
(B.BYTES - SUM(A.BYTES)) / 1024 / 1024 USEDM, 
SUM(A.BYTES) / 1024 / 1024 REMAINEDM     
FROM DBA_FREE_SPACE A, DBA_DATA_FILES B      
WHERE A.FILE_ID = B.FILE_ID        
GROUP BY B.TABLESPACE_NAME, B.FILE_NAME, B.FILE_ID, B.BYTES    
ORDER BY B.TABLESPACE_NAME) GROUP BY TBS;
```

### 2、导入前先创建用户并授权

#### 2.1、创建用户

```
create user test identified by test default tablespace TEST_SPACE
```

其中 TEST\_SPACE代表指定的表空间，导入的数据到指定的表空间中。

#### 2.2、授权用户操作

```
grant connect,resource,dba to test
```

#### 2.3、执行导入操作

```
imp test/123456@TS fromuser=test_2 touser=test_2  file=d:\expdata.dmp;
```

# 2、采用数据泵快速导入导出

### 1、数据泵导入条件

1. 数据泵必须是oracle 10.1以上版本才支持
2. 数据泵必须登录到数据库服务器，在数据库服务器上执行相关操作

### 2、数据泵导入

1. 创建逻辑目录，该命令不会在操作系统创建真正的目录，最好以system等管理员创建。

   ```
   create directory dpdata1 as 'd:\test\dump';
   ```

2. 查看管理理员目录（同时查看操作系统是否存在，因为Oracle并不关心该目录是否存在，如果不存在，则出错）

   ```
   select * from dba_directories;
   ```

3. 给scott用户赋予在指定目录的操作权限，最好以system等管理员赋予。

   ```
   grant read,write on directory dpdata1 to scott;
   ```

### 3、导出数据

1. 按用户导

   ```
   expdp scott/tiger@orcl schemas=scott dumpfile=expdp.dmp DIRECTORY=dpdata1;
   ```

2. 并行进程parallel

   ```
   expdp scott/tiger@orcl directory=dpdata1 dumpfile=scott3.dmp parallel=40 job_name=scott3
   ```

3. 按表名导

   ```
   expdp scott/tiger@orcl TABLES=emp,dept dumpfile=expdp.dmp DIRECTORY=dpdata1;
   ```

4. 按查询条件导

   ```
   expdp scott/tiger@orcl directory=dpdata1 dumpfile=expdp.dmp Tables=emp query='WHERE deptno=20';
   ```

5. 按表空间导

   ```
   expdp system/manager DIRECTORY=dpdata1 DUMPFILE=tablespace.dmp TABLESPACES=temp,example;
   ```

6. 导整个数据库

   ```
   expdp system/manager DIRECTORY=dpdata1 DUMPFILE=full.dmp FULL=y;
   ```

### 4、数据泵导出

1. 导到指定用户下

   ```
   impdp scott/tiger DIRECTORY=dpdata1 DUMPFILE=expdp.dmp SCHEMAS=scott;
   ```

2. 改变表的owner

   ```
   impdp system/manager DIRECTORY=dpdata1 DUMPFILE=expdp.dmp TABLES=scott.dept REMAP_SCHEMA=scott:system;
   ```

3. 导入表空间

   ```
   impdp system/manager DIRECTORY=dpdata1 DUMPFILE=tablespace.dmp TABLESPACES=example;
   ```

4. 导入数据库

   ```
   impdb system/manager DIRECTORY=dump_dir DUMPFILE=full.dmp FULL=y;
   ```

5. 追加数据

   ```
   impdp system/manager DIRECTORY=dpdata1 DUMPFILE=expdp.dmp SCHEMAS=system TABLE_EXISTS_ACTION
   ```



