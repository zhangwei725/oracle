# 一、Oracle数据类型

## 1、字符类型

### 1.1、 CHAR

#### 1、说明

1. 指定长度的字符串类型 最大值为2,000字节
2. 一般用户需求明确,长度固定的

#### 2、语法格式

```
CHAR(长度)
```

#### 3、示例

```
CHAR(10)
解释:表示存储大小10个字节的固定大小,不足用空字符串代替
```

### 1.2、VARCHAR2\(重点\)

#### 1、说明

1. 指定长度的字符串类型 最大值为2,000字节
2. 开发中常用

#### 2、语法格式

```
CHAR(字节长度)
```

#### 3、示例

```
示例:  varchar2(10) 
可能包含0-10个字节信息
```

### 1.3、其他

#### 1、NCHAR

```
根据字符集而定的固定长度的字符串
```

#### 2、NVARCHAR2

```
UNICODE格式数据的变长字符串。 NVARCHAR2最多可以存储4,000字节的信息
```

### 1.4、注意

#### 1、每个汉字占多少字节，要看具体的编码方式

1. UTF-8（1-3字节）、
2. GB2312（2字节）
3. GBK（2字节）
4. GB18030（1、2、4字节）
5. 数据库的NLS\_CHARACTERSET 为AL32UTF8，即一个汉字占用三个字节
6. NLS\_CHARACTERSET为ZHS16GBK，则一个字符占用两个字节。

## 2、数字类型

### 2.1、NUMBER\(重点\)

#### 1、说明

1. 数字类型,存取的范围-10^130~10^126\(不包含\)
2. 开发常用,重点掌握

#### 2、语法格式

```
NUMBER(P,S)
```

1. 说明

   ```
   (可选)p precision的简写即精度(总长度),表示有效数字的位数,最长为38位,默认是38,可以用字符*表示38
   (可选)s scale的简写表示小数点位数且四舍五入,默认值取决于p，如果没有指定p，那么s是最大范围，如果指定了p，那么s=0
   ```

2. P ,S取值情况

   ```
   最高整数位数＝p-s 
   s正数，小数点右边指定位置开始四舍五入 
   s负数，小数点左边指定位置开始四舍五入 
   s是0或者未指定，四舍五入到最近整数 
   当p小于s时候，表示数字是绝对值小于1的数字，且从小数点右边开始的前s-p位必须是0，保留s位小数。
   p>0，对s分2种情况
       1. s>0 
       精确到小数点右边s位，并四舍五入。然后检验有效数位是否<=p；如果s>p，小数点右边至少有s-p个0填充。 
       2. s<0 
       精确到小数点左边s位，并四舍五入。然后检验有效数位是否<=p+|s|
   ```

#### 3、示例代码

| **值**       | **数据类型**         | **存储值**   |
| ----------- | ---------------- | --------- |
| 123.2564    | NUMBER           | 123.2564  |
| 1234.9876   | NUMBER\(6,2\)    | 1234.99   |
| 12345.12345 | NUMBER\(6,2\)    | Error     |
| 1234.9876   | NUMBER\(6\)      | 1235      |
| 12345.345   | NUMBER\(5,-2\)   | 12300     |
| 1234567     | NUMBER\(5,-2\)   | 1234600   |
| 12345678    | NUMBER\(5,-2\)   | Error     |
| 123456789   | NUMBER\(5,-4\)   | 123460000 |
| 1234567890  | NUMBER\(5,-4\)   | Error     |
| 12345.58    | NUMBER\(\*,  1\) | 12345.6   |
| 0.1         | NUMBER\(4,5\)    | Error     |
| 0.01234567  | NUMBER\(4,5\)    | 0.01235   |
| 0.09999     | NUMBER\(4,5\)    | 0.09999   |
| 0.099996    | NUMBER\(4,5\)    | Error     |

### 2.2、INTEGER类型\(了解\)

​    INTEGER是NUMBER的子类型，它等同于NUMBER（38,0），用来存储整数。若插入、更新的数值有小数，则会被四舍五入。

### 2.3、FLOAT类型\(了解\)

​    FLOAT类型也是NUMBER的子类型,Float\(n\),数 n 指示位的精度，可以存储的值的数目。N 值的范围可以从 1 到 126。若要从二进制转换为十进制的精度，请将 n 乘以 0.30103。要从十进制转换为二进制的精度，请用 3.32193 乘小数精度。126 位二进制精度的最大值是大约相当于 38 位小数精度

## 3、日期类型\(重点\)

### 3.1、DATE

#### 1、说明

​    指定一个七个字节日期/时间的数据类型 包含年月日时分秒

#### 2、格式

```
DD-MM-YY(HH-MI-SS)
```

#### 3、示例代码

```
2017-05-03 00:00:00
```

### 3.2、TIMESTAMP\(重点\)

#### 1、说明

​    指定一个7个字节或者12个字节的日期/时间的数据类型,他与date类型不同的是包含了带小数秒,小数秒最多为9位,默认6位

#### 2、示例代码

```
2017-05-03 23:12:43:000000
```

## 4、 二进制及大文本数据\(了解\)

### 4.1 CLOB 数据类型

```
它存储单字节和多字节字符数据。支持固定宽度和可变宽度的字符集。
CLOB对象可以存储最多 \(4 gigabytes-1\) \* \(database block size\) 大小的字符
```

### 4.2 NCLOB 数据类型

```
它存储UNICODE类型的数据，支持固定宽度和可变宽度的字符集，
NCLOB对象可以存储最多\(4 gigabytes-1\) \* \(database block size\)大小的文本数据。
```

### 4.3 BLOB 数据类型

```
它存储非结构化的二进制数据大对象，它可以被认为是没有字符集语义的比特流，一般是图像、声音、视频等文件。
BLOB对象最多存储\(4 gigabytes-1\) \* \(database block size\)的二进制数据。
```

### 4.4 BFILE 数据类型

```
二进制文件，存储在数据库外的系统文件，只读的，数据库会将该文件当二进制文件处理
```