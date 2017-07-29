# 入门语句

## 一、Sql\*plus命令

### 1.1、连接数据库

#### 1.1.1、使用标准方式加密账户登录Oracle

1. 语法格式

   ```
   输入用户名:
   输入口令:
   请注意密码的格式，如果登录角色是sysdba，密码是你的密码[空格]as[空格]sysdba；如果登录的角色是一般角色，只需要输入密码即可
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/59985035.jpg)

##### 1.1.2、使用明码方式用账户登录Oracle

1. 语法格式

   ```
   sqlplus 用户/密码
   请注意密码的格式，如果登录角色是sysdba，密码是你的密码[空格]as[空格]sysdba；如果登录的角色是一般角色，只需要输入密码即可
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/8627996.jpg)

##### 1.1.3、使用默认方式用sys账户登录Oracle

1. 语法格式

   ```
   sqlplus / as sysdba;
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/12716907.jpg)

##### 1.1.4、SQL命令下断开用户连接

1. 语法

   ```
   disconn; 断开连接
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/39558749.jpg)

##### 1.1.5、SQL命令下用户登陆

1. 语法

   ```
   conn 用户名/密码  as  系统管理员
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/84059724.jpg)

1.1.1.5、远程连接数据库

1. 语法

   ```
   sqlplus 用户名/密码@ip:端口/数据库名 as 系统管理员
   ```

2. 示例代码

   ![](http://opzv089nq.bkt.clouddn.com/17-7-29/99171094.jpg)

## 二、其他常用命令

1. SQL&gt; set linesize 1000 --设置屏幕显示行宽，默认100

2. SQL&gt; set pagesize 0; //输出每页行数，缺省为24,为了避免分页，可设定为

3. SQL&gt; @ d:\sql\test.sql --执行sql文件

4. SQL&gt;edit --对当前的输入进行编辑

5. SQL&gt;edit d:\sql\test.sql --编辑sql文件,如果不存在会自动创建

6. SQL&gt;/ --重新运行上一次运行的

7. SQL&gt;host cls --清屏

8. SQL&gt; desc table\_name — 显示一个表的结构

9. SQL&gt; set autocommit ON --设置是否自动提交，默认为OFF

10. SQL&gt;showuser --显示当前用户

11. SQL&gt;shutdown normal--关闭数据库服务，等待所有用户离开后再关闭

12. SQL&gt;shutdown immediate--关闭数据库服务，立刻关闭

13. SQL&gt;shutdown startup—重启数据库服务



