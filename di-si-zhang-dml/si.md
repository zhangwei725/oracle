# 修改\(ALTER\)操作\(了解\)

## 一、修改表名

### 1.1、语法格式

1. 格式1

   ```mysql
   ALTER TABLE 旧表名  
   RENAME TO 新表名;
   ```

2. 简写语法

   ```
   RENAME 旧表名

   TO 新表名
   ```

### 1.2、示例代码

1. 示例1

   ```
   ALTER  TABLE  emp
   RENAME TO tb_emp;
   ```

2. 简写示例

   ```
   RENAME emp 
   TO tb_emp
   ```

## 二、添加修改列名

### 2.1、添加列

1. 语法格式

   ```
   ALTER TABLE  表名 
   ADD 列名 数据类型
   ```

2. 示例代码

   ```
   ALTER TABLE emp
   ADD test VARCHAR2(15)
   ```

### 2.2、修改列

1. 语法格式

   ```mysql
   ALTER TABLE  表名 
   RENAME COLUMN 旧列名
   TO 新列名
   ```

2. 示例代码

   ```mysql
   ALTER TABLE emp
   RENAME COLUMN empno 
   TO empid;
   ```

## 三、修改列数据类型

1. 语法格式

   ```mysql
   ALTER TABLE 表名
   MODIFY (列名 数据类型(length));
   ```

2. 示例代码

   ```mysql
   ALTER TABLE EMP 
   MODIFY (EMPNO NUMBER(10));
   ```

## 四、添加约束

### 1、添加约束

#### 1.1、添加主键约束

1. 语法格式

   ```mysql
    ALTER TABLE 表名 
    ADD CONSTRAINT 主键名 
    PRIMARY KEY(主键字段);
   ```

2. 示例代码

   ```mysql
    ALTER TABLE emp 
    ADD CONSTRAINT pk_emp 
    PRIMARY KEY(empno);
   ```

#### 1.2、添加外键约束

1. 语法格式

   ```mysql
   ALTER TABLE 表名 
   ADD CONSTRAINT 约束名 
   FOREIGN KEY (外键字段) 
   REFERENCES 父表(外键字段) 
   [ON DELETE CASCAED|ON DELECT SET NULL];
   ```

2. 示例代码

   ```mysql
   ALTER TABLE emp 
   ADD CONSTRAINT 约束名 
   FOREIGN KEY (deptno) 
   REFERENCES dept(deptno) 
   ON DELETE CASCAED 
   --ON DELECT SET NULL
   ```

#### 1.3、添加唯一约束

1. 语法格式

   ```mysql
   ALTER TABLE 表名
   ADD CONSTRAINT UK_表名_列名
   UNIQUE (列名1, 列名2, ... , 列名n);
   ```

2. 示例代码

   ```mysql
   ALTER TABLE emp
   ADD CONSTRAINT "UK_EMP_ENAME"
   UNIQUE (ename);
   ```

#### 1.4、添加检查约束

1. 语法格式

   ```mysql
   ALTER TABLE 表名
   ADD CONSTRAINT 约束名(CK_表名_列名) 
   CHECK (列名 表达式) [DISABLE];
   ```

2. 示例代码

   ```mysql
   ALTER TABLE emp
   ADD CONSTRAINT "CK_EMP_SAL" 
   CHECK (sal> 0) [DISABLE];
   ```

#### 1.5 、空约束跟默认约束 \(注意\)

1. 空约束语法格式

   ```mysql
   ALTER TABLE 表名
   MODIFY 列名 NULL; 

   ALTER TABLE 表名
   modify 列名 NOT NULL;
   ```

2. 示例代码

   ```mysql
   ALTER TABLE 表名
   MODIFY ename NOT NULL;

   ALTER TABLE 表名
   MODIFY ename  NULL;
   ```

3. 默认约束语法格式

   ```mysql
   ALTER TABLE 表名 
   MODIFY 列名 default 默认值
   ```

4. 示例代码

   ```MYSQL
   ALTER TABLE emp 
   MODIFY sal default 0
   ```

### 2、修改约束名\(不能修改约束\)

#### 2.1、无法修改主键名只能禁用

1. 语法格式

   ```
   alter table
   emp disable constraint 主键约束名
   ```

2. 示例代码

   ```
   alter table
   emp disable constraint pk_emp

   alter table
   emp enable constraint pk_emp
   ```

## 5、其他

1. 删除约束

   ```mysql
   alter table 表名 drop 约束名
   ```

2. 禁用主键约束

   ```
   alter table test disable constraint 主键约束名
   ```

3. 删除表

   ```
   drop table tb_name [cascade constraint];

   说明
   1.删除表中所有数据
   2.所有的索引被删除
   3.使用cascade constraint,级联删除所有的依赖完整性约束
   drop table test cascade constraint;
   ```

4. 截断表：truncate



