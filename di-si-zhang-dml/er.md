# 数据库表约束

## 1、定义

​    约束是强加在表上的规则或条件。确保数据库满足业务规则。保证数据的完整性。

​    当对表进行DML或DDL操作时，如果此操作会造成表中的数据违反约束条件或规则的话，系统就会拒绝执行这个操作。约束可以是列一级别的也可以是表级别的。定义约束时没有给出约束的名字，ORACE系统将为该约束自动生成一个名字，其格式为SYS\_Cn，其中n为自然数\(强烈建议各位在创建表或增加约束时，给约束定义名称

## 2、作用

1. 实现一些业务规则，防止无效的垃圾数据进入数据库，维护数据库的完整性\(完整性指正确性与一致性\)。
2. 从而使数据库的开发和维护都更加容易

## 3、分类

1. 行级约束

   ```
   对一个数据列建立的约束，称为列级约束。列级约束既可以在列定义时声明，也可以在列定义后声明
   ```

2. 表级约束

   ```
   对多个数据列建立的约束，称为表级约束。表级约束只能在列定义后声明
   ```

3. 注意

   ```
   1、NOT NULL约束和DEFAULT约束不存在表级约束。其余的约束都存在列级约束和表级约束。
   2、实际开发过程中，列级约束使用的较多，表级约束很少使用
   ```

   ​

## 4、约束类型

### 1、说明

1. 唯一性
2. 主键约束
3. 外键约束
4. 检查约束
5. 空值约束
6. 默认值约束

### 2、主键约束\(PRIMARY KEY Constraint\)

#### 2.1、定义

​    唯一的标识表中的每一行，不能重复，不能为空。 创建主键或唯一约束后，ORACLE会自动创建一个与约束同名的索引（UNIQUENES为UNIQUE唯一索引）

#### 2.2、作用

​    实现表中记录的一个唯一性,一个表中一般只有一个主键\(建议\)

#### 2.3、主键分类

##### 1、单一主键

##### 2、复合主键\(不建议使用\)

​        表的主键含有一个以上的字段组成

##### 3、联合主键\(多对多的情况下\)

​        多个主键联合形成一个主键组合

#### 2.4、语法格式

1. 格式1

   ```mysql
   CREATE TABLE 表名
   (
         列名 数据类型  [CONSTRAINT 主键名称(PK_表名)] PRIMARY KEY,
       ...,
   );
   ```

2. 格式2

   ```mysql
    CREATE TABLE 表名
    (    
           列名 数据类型,
        列名 数据类型,
        列名 数据类型,
        ...
        CONSTRAINT [主键名称(PK_表名)] PRIMARY KEY (列名, 列名,...,列名n)
    );
   ```

#### 2.5、 示例代码

1. 在创建数据的时候在行的后面添加

   ```mysql
   CREATE  TABLE TB_USER(
     UID NUMBER(10) CONSTRAINT "PK_TB_USER" PRIMARY KEY,
     USERNAME VARCHAR2(64),
     PASSWORD VARCHAR2(32),
     EMAIL   VARCHAR2(32),
     PHONE   CHAR(11)
   );
   ```

2. 放在建表语句的后面添加

   ```mysql
   CREATE  TABLE TB_USER(
       UID NUMBER(10),
       USERNAME VARCHAR2(64),
       PASSWORD VARCHAR2(32),
       EMAIL   VARCHAR2(32),
       PHONE   CHAR(11),
       CONSTRAINT "PK_TBUSER" PRIMARY KEY (UID) 
   );
   ```

#### 2.6、添加方式

1. 建表时添加，
2. 建表后添加 
3. 当建立复合主键的时候必须使用表级添加 

#### 2.7、主键约束的限制

1. 一个表或视图一般只有一个主键
2. 你不能指定一列或组合列既是主键又是唯一键
3. 开发中强烈建议使用NUMBER或VARCHAR2类型
4. 主键大小不能超过一个数据块大小
5. 主键组合键不能超过32列。
6. 创建一个继承层次结构中的子视图时，你不能指定一个主键。主键可以唯一指定的顶层（根）视图

#### 2.8、主键自动增长

1. 建表

2. 建立序列

   ```mysql
   CREATE SEQUENCE dept_sequence
   increment by 1
   start with 1
   nomaxvalue
   nocycle
   nocache;
   ```

3. 建立触发器（目的是在表插入数据的时候启用序列生成的值）

### 2、外键约束\(FOREIGN KEY Constraint\)

#### 2.1 、定义

​    用来维护从表（Child Table）和主表（Parent Table）之间的引用完整性. 外键约束是个有争议性的约束，它一方面能够维护数据库的数据一致性，数据的完整性。防止错误的垃圾数据入库； 另外一方面它会增加表插入、更新等SQL性能的额外开销，不少系统里面通过业务逻辑控制来取消外键约束

#### 2.2 、语法格式

1. 格式1

   ```mysql
   CREATE TABLE 表名
   (
       列名 数据类型 REFERENCES 父表(参照列名) [ON DELETE CASCADE|SET NULL],
   )
   ```

2. 格式2

   ```mysql
   CREATE TABLE 表名
   (
       列名 数据类型 ,
       CONSTRAINT 约束名称(FK_TABLE_NAME) FOREIGN KEY REFERENCES 父表(参照列名) ON DELETE SET NULL
   )
   ```

3. 说明

   1、ON DELETE SET NULL子句：当主表中的一行数据被删除时，Oracle系统会自动地将所有从表中依赖于它的数据记录的外键改成空值；

   2、ON DELETE CASCADE：当主表中的一行数据被删除时，Oracle系统会自动地将所有从表中依赖于它的数据记录删除

#### 2.3 、示例代码

1. 在主键字段后添加

   ```mysql
   CREATE  TABLE TB_ADDRESS(
       ADDRESS NUMBER(10),
       USERNAME VARCHAR2(64),
       UID NUMBER(12)  CONSTRAINT FK_TBUSER_UID REFERENCES TB_USER(UID) ON DELETE CASCADE
   );
   ```

2. 建表后添加

   ```mysql
   CREATE  TABLE TB_ADDRESS(
       ADDRESS NUMBER(10),
       USERNAME VARCHAR2(64),
       UID NUMBER(12),
       CONSTRAINT "FK_TBUSER_UID" FOREIGN KEY(UID) REFERENCES TB_USER(UID) ON DELETE SET NULL
   );
   ```

#### 2.4 、特点

​    参照主键中存在的值并且可以插入重复的记录，而且可以插入重复的空值。

#### 2.5、注意事项

1. 外键字段不能为LOB, LONG, LONG RAW, VARRAY, NESTED TABLE, BFILE, REF, TIMESTAMP WITH TIME ZONE, or user-defined type类型，主键可以包含数据类型为TIMESTAMP WITH LOCAL TIME ZONE的字段
2. 引用唯一或主键约束，必须是父表中已经定义的
3. 外键的组合列不能超过32列
4. 字表和父表必须在同一个数据库。分布式数据库中，外键不能跨节点，但触发器可以你不能在CREATE TABLE语句中包含AS子查询子句定义一个外键约束。相反，你必须创建一个没有约束的表，然后添加ALTER TABLE语句
5. 参照列必须是主键或者是unique列

#### 2.6、对DML与DDL的影响

1. INSERT：只有操作是在子表或从表这一端时才会产生违反引用完整性约束的问题，父表则不然。

2. DELETE：只有操作是在父表或主表这一端时才会产生违反引用完整性约束的问题，子表则不然。

3. UPDATE：子表父表直接操作都会违反引用完整性约束。两种解决方法：

   1、先更新子表的引用列为空，再更新父表的主键的列的值，然后把子表的引用列更新成新的父表的值；

   2、使用ON DELETE SET NULL，先更新父表，然后将子表外键为空的记录更新为新的值。

4. DDL语句：DROP TABLE与TRUNCATE TABLE，操作父表，违反引用完整性约束，子表则不然

### 3、唯一约束\(Unique Counstraint\)

#### 3.1、定义

​    唯一性约束指的是表中一个或者多个字段联合起来能够唯一标识一条记录的约束,  
联合自动中可以包含空值且不能联合字段不能超过32个

#### 3.2、作用

​    因为表中只有一个主键，但是其他的列需要唯一要求

#### 3.3、特点

​    唯一 ，可以插入空值，可以插入重复的空值

#### 3.4、语法格式

1. 格式1

   ```mysql
   CREATE TABLE 表名
   (
       列名 数据类型,
       列名 数据类型,
       ...
       CONSTRAINT [唯一约束名(UK_表名_列名)] UNIQUE (列名, 列名,...)
   );
   ```

#### 3.5、示例代码

1. 单列添加唯一性约束

   ```mysql
   CREATE TABLE TB_UNIQUE
   (
     U_ID NUMBER(10),
     USERNAME  VARCHAR2(32),
     PHONE VARCHAR2(11)
     CONSTRAINT UK_TBUNIQUE_UID  UNIQUE (USERNAME)
   );
   ```

2. 多列添加唯一约束

   ```mysql
   CREATE TABLE TB_UNIQUE
   (
       U_ID NUMBER(10),
       USERNAME  VARCHAR2(32),
       PHONE VARCHAR2(11)
       CONSTRAINT UK_TBUNIQUE_UID  UNIQUE (USERNAME,PHONE)
   );
   ```

#### 3.6、与主键的区别

1. 唯一性约束所在的列允许空值，但是主键约束所在的列不允许空值。
2. 可以把唯一性约束放在一个或者多个列上，这些列或列的组合必须有唯一的。但是，唯一性约束所在的列并不是表的主键列。
3. 唯一性约束强制在指定的列上创建一个唯一性索引。在默认情况下，创建唯一性的非聚簇索引，但是，也可以指定所创建的索引是聚簇索引。
4. 建立主键的目的是让外键来引用.
5. 一个表一般最多只有一个主键，但可以有很多唯一键

### 4、检查约束\(Check Constraint\)

#### 4.1  定义

​    根据用户自己的需求来进行限制

#### 4.2  语法格式

1. 格式1

   ```mysql
   CREATE TABLE TB_CHECK
   (
       列名 数据类型   [CONSTRAINT 检查约束名(CK_表名_列名)] CHECK [NOT FOR REPLICATION],
       列名 数据类型,
   );
   ```

2. 格式2

   ```mysql
   CREATE TABLE TB_CHECK
   (
       列名 数据类型,
       列名 数据类型,
       [CONSTRAINT 检查约束名(CK_表名_列名)] CHECK(列名) [NOT FOR REPLICATION]
   );
   NOT FOR REPLICATION:指定检查约束在把从其它表中复制的数据插入到表中时不发生作用
   ```

#### 4.3  示例代码

1. 在行级别添加

   ```mysql
   CREATE TABLE TB_CHECK
   (
     USERNAME VARCHAR2(32),
     PHONE  VARCHAR2(11) CHECK(LENGTH(PHONE) =11)
   );
   ```

2. 约束放在表后

   ```mysql
   CREATE TABLE TB_CHECK1
   (
       USERNAME VARCHAR2(32),
       PHONE  VARCHAR2(11),
       CONSTRAINT CK_TBCHECK_PHONE CHECK(LENGTH(PHONE)=11) NOT FOR REPLICATION
   );
   ```

### 5、空值约束\(Not Null Constraint\)

#### 5.1、说明

1. 一种是限制空值Not Null 
2. NOT NULL只能在列级定义

#### 5.2、作用：

​    使指定的列必须插入值

#### 5.3、语法格式

1. 格式

   ```
   CREATE TABLE 表名
   (
       列名 数据类型 null/not null,
       ...
   );
   ```

#### 5.4、示例代码

1. 不允许为null

   ```
   CREATE TABLE TB_NULL
   (
       USER_ID NUMBER(10) PRIMARY KEY,
       USERNAME VARCHAR2(64) NOT NULL
   );
   ```

### 6、默认约束\(Default Counstraint\)

#### 6.1、定义

1. 当不插入值的时候给默认效果 
2. 用的使modify 来进行修改 

#### 6.2、语法格式

1. 格式

   ```
   DEFAULT 默认值
   ```

#### 6.3、示例代码

1. 给列添加默认值

   ```
   CREATE TABLE TB_DEFAULT(
       USER_ID  NUMBER(10) PRIMARY KEY,
       USERNAME  VARCHAR2(32) NOT NULL,
       PHONE    VARCHAR2(11) CHECK(LENGTH(PHONE) =11),
       SEX   NUMBER(2)  DEFALUT 0,
       CREATE_TIME  DATE DEFALUT  SYSDATE
   )
   ```

### 7 、其他

#### 7.1、约束名的命名规则

1. 主键约束: PK\_表名
2. 唯一约束: UK _ 表名  _ 列名
3. 外键约束: FK_ 表名_ 列名
4. 条件约束 :CK_ 表名_列名

#### 7.2、注意事项

1. NOT NULL约束和DEFAULT约束只能被创建为列级约束
2. 其他４种则既可以被创建为列级约束，也可以被创建为表级约束
3. 当一个约束涉及到多列时，只能被创建成表级约束
4. 可以为其他4种约束起名，而不能给NOT NULL和DEFAULT约束起名

#### 7.3、约束管理

1. 删除约束

   ```mysql
   ALTER TABLE 表名  DROP CONSTRAINT 列名; 

   ALTER TABLE 表名  DROP PRIMARY KEY

   ALTER TABLE 表名  DROP PRIMARY KEY CASCADE;
   ```

2. 启用约束

   ```mysql
   ALTER TABLE 表名 ENABLE CONSTRAINT 列名; 

   ALTER TABLE 表名 ENABLE PRIMARY KEY;
   ```

3. 禁用约束



