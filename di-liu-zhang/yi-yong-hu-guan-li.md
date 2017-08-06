# Oracle用户管理

## 1、系统内置重要用户

### 1.1、内置用户

1. sys用数据库的超级用户必须使用sysdba身份登录
2. system 是数据库内置的一个普通管理员

### 1.2、sys和system用户的区别

1. system用户只能用normal身份登陆em。

2. sys用户具有“SYSDBA”或者“SYSOPER”权限，登陆系统也只能用这两个身份不能用normal

   | SYSDBA | SYSOPER |
   | --- | --- |
   | Startup\(启动数据库\) | startup |
   | Shutdown\(关闭数据库\) | shutdown |
   | alter database open/mount/backup | alter database open/mount/backup |
   | 改变字符集 | none |
   | create database\(创建数据库\) | None不能创建数据库 |
   | drop database\(删除数据库\) | none |
   | create spfile | create spfile |
   | alter database archivelog\(归档日志\) | alter database archivelog |
   | alter database recover\(恢复数据库\) | 只能完全恢复，不能执行不完全恢复 |
   | 拥有restricted session\(会话限制\)权限 | 拥有restricted session权限 |
   | 可以让用户作为sys用户连接 | 可以进行一些基本的操作，但不能查看用户数据 |
   | 登录之后用户是sys | 登录之后用户是public |

## 2、权限

### 1、说明

​    权限允许用户访问属于其它用户的对象或执行程序，ORACLE系统提供三种权限：Object 对象级、System 系统级、Role 角色级

### 2、权限分类

系统权限：系统规定用户使用数据库的权限。\(主要针对用户\)

实体权限：某种权限用户对其它用户的表或视图的存取权限。\(主要针DML\)

#### 2.1、系统权限\(用户\)

##### 2.1.1、说明

​    系统权限以让用户执行特定的命令集

##### 2.1.2、系统内置角色

1. DBA\(数据库管理员角色\): 拥有全部特权，是系统最高权限，包括无限制的空间限额和给其他用户授予各种权限的能力，
2. RESOURCE\(资源角色\):正式的数据库用户可以授予resource role。resource提供给用户另外的权限以创建他们自己的表、序列、过程\(procedure\)、触发器\(trigger\)、索引\(index\)和簇\(cluster\)
3. CONNECT\(连接角色\):临时用户，特别是那些不需要建表的用户，通常只赋予他们connectrole。connect是使用oracle的简单权限，这种权限只有在对其他用户的表有访问权时，包括select、insert、update和delete等，才会变得有意义。拥有connect role的用户还能够创建表、视图、序列\(sequence\)、簇\(cluster\)、同义词\(synonym \)。
4. 系统权限只能由DBA用户授出：sys, system\(最开始只能是这两个用户\)

##### 2.1.3、常用的操作

1. 创建新用户
2. 删除用户
3. 删除表
4. 备份表

#### 2.2、对象权限

##### 2.2.1、说明

​    可以让用户能够对各个对象进行某些操作

##### 2.2.2、常用的操作

1. 修改\(alter\)
2. 删除\(delete\)
3. 执行\(execute\)
4. 索引\(index\)
5. 插入\(insert\)
6. 关联\(references\)
7. 查询\(select\) 
8. 更新\(update\)

## 3、角色

### 3.1、 定义

​    角色\(role\)就是一组权限的集合

### 3.2、创建角色

1. 语法格式

   ```
   CREATE ROLE 角色名
   ```

2. 示例代码

   ```
   CREATE ROLE testRole;
   ```

### 3.3、删除角色

1. 语法格式

   ```
   DROP ROLE 角色名;
   ```

2. 示例代码

   ```
   DROP ROLE testRole;
   ```

### 3.4、授权角色

1. 只有生效才能授权给用户

   ```
   SET ROLE testRole;
   ```

2. 语法格式

   ```
   GRANT 角色1 , 角色2, 角色3... 
   TO 用户名1 [,用户名2]...;
   ```

3. 示例代码

   ```
   GRANT  dba ,resource,connect
   TO testRole
   ```

## 4、创建用户

1. 语法格式

   ```
   CREATE USER  用户名 
   IDENTIFIED BY 密码;
   ```

2. 示例代码

   ```
   CREATE USER zhangwei  
   IDENTIFIED BY 123456
   ```

## 5、修改用户

1. 语法格式

   ```
    ALTER USER 用户名
    IDENTIFIED  BY 口令
   ```

2. 示例代码

   ```
   ALTER USER  zhangwei
   IDENTIFIED  BY admin
   ```

## 6、删除用户

1. 语法格式

   ```
   DROP  USER 用户名 CASCADE
   --如果删除的用户有其他对象则使用级联删除
   ```

2. 示例代码

   ```
   DROP USER zhangwei 
   CASCADE;
   ```

## 7、用户授权

1. 语法格式

   ```
   GRANT 权限 , 权限, 权限... 
   TO 用户名1 [,用户名2]...;
   ```

2. 示例代码

   ```
   GRANT  dba ,resource,connect
   TO zhangwei
   ```

## 8、其他命令

```
查询用户拥有哪里权限：
  select * from dba_role_privs;
  select * from dba_sys_privs;
  select * from role_sys_privs;
查自己拥有哪些系统权限
  select * from session_privs;
删除用户
  drop user 用户名 cascade;  //加上cascade则将用户连同其创建的东西全部删除
系统权限传递：增加WITH ADMIN OPTION选项，则得到的权限可以传递。
  grant connect, resorce to testuser with admin option;  //可以传递所获权限。
系统权限回收：系统权限只能由DBA用户回收
  Revoke connect, resource from testuser;

查询用户拥有哪里权限：
  select * from dba_role_privs;
  select * from dba_sys_privs;
  select * from role_sys_privs;
查自己拥有哪些系统权限
    select * from session_privs;
```



